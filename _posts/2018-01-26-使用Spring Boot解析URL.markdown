---
published: true
---
本文介绍Spring Boot中注解对URL的解析方法

### @RequestMapping
   此注解用于将URL某一模式或路径映射到具体的处理方法上面
{% highlight java %}

    //path后面可以有多个模式用逗号分隔，它们都映射到同一个处理函数
    //method定义了http请求的方式，当定义为GET时其他方式不被允许
    @RequestMapping(path={"/","/index"}, method = {RequestMethod.GET})
    @ResponseBody
    public String index() {
        return "Hello,Spring Boot!";
    }
    
{% endhighlight %}

### 处理URL中的参数
   在URL中，我们要先取得URL中每一个/item/的内容，可以现在RequestMapping里面写成/profile/{groupId}/{userId}的形式，花括号里我们先定义一个占位符变量
   在之后的处理函数的参数中使用@PathVariable注解便可以将每一个/item/取出来
   
   另外，在URL后面仍然有许多重要的参数传递，往往以{% highlight java %}?type=xxx&key=xxx{% endhighlight %}这样的形式出现，同理我们使用@RequestParam来处理
{% highlight java %}

    @RequestMapping(value = {"/profile/{groupId}/{userId}"}, method = {RequestMethod.GET})
    @ResponseBody
    public String profile(@PathVariable("groupId")String groupId,
                   @PathVariable("userId")int userId,
                   @RequestParam(value = "type", defaultValue = "1") String type,
                   @RequestParam(value = "key", required = false) String key) {

        return String.format("Gourp id is %s, User id is %d, type is %s, key is %s", groupId, userId, type, key);
    }
    
{% endhighlight %}

   在@RequestParam注解中，若required为false，那么在URL传输中可以忽略key的值; 若required不设置默认为true，那么必须给key赋值; 若设置了defaultValue的值，那么同样可以不给defaultValue的值，参数的值会为默认值; 同时，当RequestParam中的值为int时，若required设置为false时仍然会报错，因为忽略时的值为null，无法将null转化为int类型。