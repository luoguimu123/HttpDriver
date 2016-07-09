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

其中get,post,put,delete对应RESTful风格的接口的四种不同的接口访问方式，其都是针对方法级别的注解。其中都有String path()属性和Class returnClass属性，其中path代表的是访问接口路径中的url中path部分，returnClass代表的是返回类型，需要注意的是诸如List<Person>这样的返回
值类型，也应该设置为Person.class

@Param是针对参数级别的注解，其中有String value()字段，其表示的是所需要传入参数的key。

@PathVariable是参数级别的注解，其中String value()会等同于SpringMVC中的@PathVariable注解。其会主动替换掉该参数所在的方法中被上述4个访问
方法对应的path中{xxx}的部分

@HttpDriver是一个对类级别的注解，表示的是拥有@Get,@Post,@Delete,@Put这四个方法的类，表示这个类是用于访问Web RESTful的接口。


参考代码例子：

传输的实体类为：
package com.example.lgm;

/**
 * Created by lgm on 16/7/8.
 */
public class Person {

    private String sayhello;

    private String test;

    private String haha;

    public Person() {
    }

    public Person(String sayhello, String test, String haha) {
        this.sayhello = sayhello;
        this.test = test;
        this.haha = haha;
    }

    public String getSayhello() {
        return sayhello;
    }

    public void setSayhello(String sayhello) {
        this.sayhello = sayhello;
    }

    public String getTest() {
        return test;
    }

    public void setTest(String test) {
        this.test = test;
    }

    public String getHaha() {
        return haha;
    }

    public void setHaha(String haha) {
        this.haha = haha;
    }
}

访问的服务接口为：
package com.example.lgm;

import com.example.lgm.annotations.*;

import java.util.List;

/**
 * Created by lgm on 16/7/8.
 */
@HttpDriver(baseurl = "http://127.0.0.1:8080")
public interface Service {

    @Post(path = "/{path}", returnClass = Person.class)
    public Person call(@PathVariable(value = "path") String path, @Param(value = "pp") String pp);

    @Get(path = "/{path}", returnClass = Person.class)
    public List<Person> callList(@PathVariable(value = "path") String path);

}


Client调用类为：

package com.example.lgm;

import com.example.lgm.factory.HttpDriverFactory;

import java.util.List;

/**
 * Created by lgm on 16/7/8.
 */
public class Client {

    public static void main(String[] args) throws IllegalAccessException, InstantiationException, NoSuchMethodException {

        Service service = (Service) new HttpDriverFactory().get(Service.class);
        System.out.println(service.call("haha", "ppp").getHaha()+" ");
        List<Person> persons = service.callList("list");
        for(Person person : persons){
            System.out.println(person.getHaha());
        }


    }

}


