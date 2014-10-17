---
layout: post
title: "Use Carrierwave to crop image with MiniMagick"
date: 2013-07-16 14:18
comments: true
categories: [carrierwave, miniMagick]
---

做Web应用，常常要涉及到文件的上传，图片的上传就是其中之一。在Rails中，上传文件经常用到的是Carrierwave。

### [Carrierwave](https://github.com/carrierwaveuploader/carrierwave)

#### [创建方法](https://github.com/carrierwaveuploader/carrierwave#getting-started)

```
rails g uploader avatar
```

之后会产生，文件`app/uploaders/avatar_uploader.rb`:

```
class AvatarUploader < CarrierWave::Uploader::Base
  storage :file
end
```

#### 使用

1.直接使用：

```
uploader = AvatarUploader.new
uploader.store!(my_file)
uploader.retrieve_from_store!('my_file.png')
```

2.运用于 model 中:

```
class User < ActiveRecord::Base
  mount_uploader :avatar, AvatarUploader
end
```

这样有个好处，可以利用 model 传入各种操作参数，如：

```
def crop
  if model.crop_x.present?
    resize_to_limit(700, 700)

    manipulate! do |img|
      x = model.crop_x
      y = model.crop_y
      w = model.crop_w
      h = model.crop_h

      size = w << 'x' << h
      offset = '+' << x << '+' << y

      img.crop("#{size}#{offset}") # Doesn't return an image...
      img # ...so you'll neet to call it yourself
    end
  end
end
```

可能会遇到这种情况：想传入参数，但是并不要 ActiveRecord、MongoMapper、MongoId 等等 ORM 的中间件，那么可以这样子：

```
class Avatar
  extend CarrierWave::Mount

  attr_accessor :crop_w, :crop_h, :crop_x, :crop_y
  mount_uploader :image, AvatarUploader
end
```

`CarrierWave::Mount`之中包含了`mount_uploader`方法的定义。

### [RMagick](http://carrierwave.rubyforge.org/rdoc/classes/CarrierWave/RMagick.html) vs [MiniMagick](http://carrierwave.rubyforge.org/rdoc/classes/CarrierWave/MiniMagick.html)

> http://stackoverflow.com/questions/8418973/undefined-method-crop-using-carrierwave-with-minimagick-on-rails-3-1-3#answer-9961434

RMagick 和 MiniMagick 是不一样的，MiniMagick 较 RMagick 更为轻量级。RMagick 根据 Ruby 的语法规则定义了许多操作图片的方法，如：

```
rmagick_image.crop(x_offset, y_offset, width, height) # Returns an image object
rmagick_image.crop!(x_offset, y_offset, width, height) # Edits object in place
```

而 MiniMagick 运用了元编程的方法，动态的创建请求的方法（创建的[规则](https://github.com/minimagick/minimagick/blob/c12decb8bf45383a3b1ea602c320ff79f49d2b79/lib/mini_magick.rb#L456)）。方法直接访问 [mogrify](http://www.imagemagick.org/www/mogrify.html)，并不会返回图片本身对象：

```
minimagick_image.crop('100x200') # Translates to `mogrify -crop 100x200 image.ext`
minimagick_image.polaroid('12')  # Executes `mogrify -polaroid 12 image.ext`
```

所以 MiniMagick 中没有 Ruby 语法式的 crop! 方法。

### MiniMagick

#### crop

要裁剪图片，不得不谈到 crop 方法。使用的方法

```
# 'w' means width 宽
# 'h' means height 高
# 'x' means x-offset x轴偏移量
# 'y' means y-offset y轴偏移量
image.crop("wxh+x+y")
```

#### resize

参照 [imagemagick](http://www.imagemagick.org/Usage/resize/#noaspect)

在 MiniMagick 使用中，前面四种方法对应有 

```
resize("64x64!") # 固定大小，64x64
resize("64x64>") # 使大图像变小，小的不变；调整后，宽和高都不能大于64
resize("64x64<") # 使小图像变大，大的不变；调整后，宽和高都不小于64
resize("64x64^") # 使最小的宽或高变为64，另外一个按比例放大或者缩小
```

这四种方法对照链接中的图片，理解会更加容易些。

在　process 中不能直接使用　resize 方法：

```
process resize: "300x300^" # 这会找不到resize方法
```

有一场景：在页面中，将上传的图片以300x300显示来预览，即缩放后的图片最大的边变成300。并且，进行特定格式的截图，如　100x100，代码有：

```
version :thumb do 
  process :lazy_resize_300
  process :cropper
  process :force_resize_100x100!
end

def lazy_resize_300
  manipulate! do |img| 
    img.resize "300x300^" if img[:width].to_i < 300 && img[:height].to_i < 300
    img.resize "300x300>"
    img
  end
end

def force_resize_100x100!
  manipulate! do |img| 
    img.resize "100x100!"
    img
  end
end

def cropper
  manipulate! do |img|      
    x = model.crop_x
    y = model.crop_y
    w = model.crop_w
    h = model.crop_h
    size = w << 'x' << h
    offset = '+' << x << '+' << y
    img.crop("#{size}#{offset}") # Doesn't return an image...
    img # ...so you'll neet to call it yourself
  end 
end
```