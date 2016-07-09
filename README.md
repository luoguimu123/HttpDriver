# HttpDriver

本库是开发宗旨是用注解来方便实现对RESTful风格的Web接口，并自动将返回的接口数据封装成JOPO。
本库中主要的注解有以下几个

@Get

@Post

@Delete

@Put

@Param

@PathVariable

@HttpDriver

其中get,post,put,delete对应RESTful风格的接口的四种不同的接口访问方式，其都是针对方法级别的注解。其中都有String path()属性，代表的是
访问接口路径中的url中path部分。

@Param是针对参数级别的注解，其中有String value()字段，其表示的是所需要传入参数的key。

@PathVariable是参数级别的注解，其中String value()会等同于SpringMVC中的@PathVariable注解。其会主动替换掉该参数所在的方法中被上述4个访问
方法对应的path中{xxx}的部分

@HttpDriver是一个对类级别的注解，表示的是拥有@Get,@Post,@Delete,@Put这四个方法的类，表示这个类是用于访问Web RESTful的接口。


参考代码例子：


