---
title: Symfony2之创建一个简单的web应用
date: 2016-07-19 16:36:45
categories: symfony2 #文章分类目录，可以为空，注意:后面有个空格
tags: [symfony2,develop,php] #文章标签，可空，多标签请用格式[tag1,tag2,tag3]，注意:后面有个空格
---
Symfony2——创建bundle
 
bundle就像插件或者一个功能齐全的应用，我们在应用层上开发的应用的所有代码，包括：PHP文件、配置文件、图片、css文件、js文件等都会包含在bunde系统中。
    
可以通过两种方法创建bundle，一种是通过命令行创建，一种是通过手动创建相应的文件和文件夹。
<!-- more --> 
一：通过命令行创建，如下：
``` bash
$ php app/console generate:bundle --namespace=Acme/HelloBundle --format=yml
```
执行以上命令 src/Acme/HelloBundle 被创建，指定使用的配置文件格式yml（还可以使用xml和php），同时自动在 app/AppKernel.php 添加一行代码，使得bundle注册到kernel。
``` bash
//app/AppKernel.php
  public function registerBundles()
      {
          $bundles = array(
             ...,
              new Acme\HelloBundle\AcmeHelloBundle(),
          );
          return $bundles;
      }
```

Symfony2——创建一个简单的web应用（配置文件均已yml为例）
 
step 1：创建路由地址
    路由的默认配置文件  app/config/routing.yml ，打开该文件你会看到当你创建bundle的时候，Symfony已经在该文件中添加了 AcmeHelloBundle路由配置文件的入口，该入口会告诉Symfony到哪里加载AcmeHelloBundle的路由配置。
``` bash
    #app/config/routing.yml
    acme_hello:
        resource: "@AcmeHelloBundle/Resources/config/routing.yml"
        type:     annotation
```
打开 Resources/config/routing.yml 定义URL对应的执行的控制器。
``` bash
    #src/Acme/HelloBundle/Resources/config/routing.yml
    hello:
        path: /hello/{name}
        defaults: { _controller:AcmeHelloBundle:hello:index }
```

 路由设置包含了两方面，path对应了相应的URL，defaults指向URL执行的controller。占位符{name}是一个通配符，用来匹配URL中，如：/hello/jc 或者 /hello/jack 的 jc 或者 jack ，同时匹配的值作为参数传入到indexAction方法中。
 
 
step 2：创建controller
 
web应用系统解析相应的URL，交由symfony框架执行相应的controller（AcmeHelloBundle:Hello:index），该controller对应的是Acme\HelloBundle\Controller\Hellotroller类中的indexAction方法。
``` bash 
//src/Acme/HelloBundle/Controller/HelloController.php
   namespace Acme\HelloBundle\Controller;
   class HelloController{
   }
```
controller其实就是一个PHP方法，该方法由我们去创建，symfony能够执行的方法。
    编写indexAction方法，并返回Response对象，最后由symfony框架输出Response对象。（Response类是Symfony框架提供的）

``` bash 
//src/Acme/HelloBundle/Controller/HelloController.php
   namespace Acme\HelloBundle\Controller;
   use Symfony\Component\HttpFoundation\Response;
   
   class HelloController{
    public function indexAction($name)
       {
           // replace this example code with whatever you need
           return new Response('<html><body>Hello'.$name.'!</body></html>');
       }
   }
```

  其中indexAction中$name参数是配置文件中 path 里面的占位符{name}，http://localhost/app_dev.php/hello/Ryan可以看到相应的输出结果。
 
step 3：创建输出模板

``` bash 
```