---
title: 关于控制器Controller
date: 2016-07-15 09:53:19
tags: symfony2
---
{% qnimg 120.png title:测试  alt:测试 'class:class1 class2' extend:?imageView2/2/w/600 %}
关于控制器Controller

Bundle在创建时默认创建了一个DefaultController，对应的文件为src/Blogger/BlogBundle/Controller/DefaultController.php。

路由配置中控制器的设置格式为：bundle:controller:template。例如上面的例子中设置为：BloggerBlogBundle:Default:index，即表示对应的控制器处理方法为src/Blogger/BlogBundle/Controller/DefaultController.php中的indexAction，使用的模板为src/Blogger/BlogBundle/Resources/views/Default/index.html.twig。

这个算是Symfony框架中的约定。在Symfony框架中有存在一些约定，如：

文件名应该与类名一致（这个应该是从JAVA抄来的）；
文件分类放在不同目录，如Controller目录只存放控制器，Entity目录存放Model实体等；
还有就是文件的命名。控制器文件的命名格式为：<name>Controller.php，模板文件的命名为：<name>.html.twig，测试用命文件命名以Test.php结尾，等等；
控制器中对应的处理方法的命名格式为：<name>Action，对应的模板文件为：<name>.html.twig。
对于这些约定如果之前接触过Rails的话应该不难理解。