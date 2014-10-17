---
layout: post
title: "rails pjax in diff partial"
date: 2013-05-04 09:13
comments: true
categories: [rails pjax]
---

How to use pjax refresh pages on rails?

## Useage

> https://github.com/defunkt/jquery-pjax

It's a tool to refresh page through ajax on rails.
You can see README from [jquery-pjax](https://github.com/defunkt/jquery-pjax).

__gem__
v =0.3.4

In `Gemfile`

    gem 'pjax_rails'

In `application.js`

    //= require pjax
  
__Example one__

Add to `application.js`

    $(document).pjax('a', '#pjax-container')
    
Then, when you click a `a` link, your page partion which id is `pjax-container` will be replaced through AJAX.

Mostly, it's used in pagination.

On rails, we can increase data compution from changing action code. `index` action, use kaminari to page: 

    def index
        @posts=Post.all.page(page)
        respond_to do |format|
        format.html {(render :layout => false if request.headers['X-PJAX'])}
      format.json { render :json => @buildings }
    end
    end
    
In `index.haml`

    #pjax-container
        @posts.each ...
        
        = pagination @posts
    
When click `a` tag, it will show a params[:_pjax] which equals `#pjax-container` in console of backend. 
Frontend, `#pjax-container` partion is replaced, and other partions do not.

In this, _warning_:

when i use syntax: [rails/pjax_rails](https://github.com/rails/pjax_rails)

    $('a:not([data-remote]):not([data-behavior]):not([data-skip-pjax])').pjax('[data-pjax-container]');
    
it happens lots of problems, especially use tag id and class attribute. So, dont use this.

Example Two
---

Sometimes, we want to diff link replace diff container.
Likely, 

* nav bar usually update `#main` partion
* login form update login status
* ...

This, in application.js

    $(document).pjax('.pjax-1 a', '#pjax-container-1')
    $(document).pjax('.pjax-2 a', '#pjax-cantainer-2')
    
Then change relative views and actions. Ok.

Example Three:
---
On rails apps, we use `render` paritial function in mostly.
In `show.haml`,

    #pjax-container-1
        #post-info
            ...
        #comments
            = render 'comments', :@comments=>@post.comments
        
in `_comments`,

    @comments.each ...
    = pagination @comments
    
This case, we can see comments pagination in post `show` page. When we click pager and just want to refresh `#comments` partion.

Maybe, we do:

* $(document).pjax('.pagination a', '#pjax-container-1')
* $(document).pjax('.pagination a', '#comments')

First one, it will refresh comments info and post info.
Second, it will refresh only comments info. Maybe, it's better!

However, what is the replaced from server? the `show` action in controller:

    def index
        @post=Post.find(params[:id])
        respond_to do |format|
            format.html {(render :layout => false if request.headers['X-PJAX'])}
            format.json { render :json => @building }
        end
    end
    
Then, the post info and comments info will be return from server to frontend.
In this time, first one should be right!

Can we use second one? And, we must control the return info from server. Then,
change the view `show.haml` code

    /#pjax-container-1
    unless request.headers['X-PJAX']
        #post-info
            ...
    #comments
        = render 'comments', :@comments=>@post.comments

if use pjax, only return comments info! Done!

End
---

Mostly, we also have some function when refresh page, like js functions. 
I will write something for that if i need!
