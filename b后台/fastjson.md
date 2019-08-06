## Fastjson

1. 高版本在自动解析时要显示自动类型适用。(eg: 1.1.26 -> 1.2.58)

   ~~~java
   App app = new App();
   String str = JSON.toJSONString(app, SerializerFeature.WriteClassName);
   // 方式1 适用所有类型
   Object parse = JSON.parse(str, Feature.SupportAutoType); 
   // 方式2 仅适用指定类型
   ParserConfig.getGlobalInstance().addAccept("com.zls.App");
~~~
   
   