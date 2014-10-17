---
layout: post
title: "rails lib 文件"
date: 2012-08-17 21:42
comments: true
categories: [work, lib, rails]
---

今天在rails项目中的`rails_dir/lib`文件夹新建了个文件，当类库使用。

如：my_jobs.rb，内容如下:

    module Jobs
      def self.hello
        "hello"
      end
    end

但是，当我用

    rails c

并不能正常加载，不是说rails项目的lib中的文件会自动加载吗？问了旁边的齐洋，哦，要模块名与文件名要同名(符合rails规则)，才能自动加载。

唉，之前都是习惯同名，现在不同名却能发现这一问题。多coding，才能发现问题啊。
