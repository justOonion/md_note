##Jackson

```java

import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonSetter;

public class User{
			//@JsonProperty 功能包括@JsonGetter @JsonSetter
  		// 序列化为json时，password属性会被序列化为 {pwd :""}
      @JsonProperty(value = "pwd")
      private String password;  
      
      //@JsonSetter 只能作用于setter方法上
      // 接受json中有个 “_id"属性的，解析后属性名为 mid
      @JsonSetter("_id")
			private String mid;
}
```
### Jackson的核心模块由三部分组成
-	jackson-core, 核心包，提供基于“流模式”解析的相关API，它包括JsonPaser, JsonGenerator。
	Jackson内部实现正是通过高性能的流模式API来生成和解析json的
-	jackson_annotations，注解包，提供标准的注解功能
-	jackson-databind ,数据绑定包，提供基于“对象绑定”解析的相关API（ObjectMapper）和“ 树模型“解析的相关API(JsonNode);  他们依赖基于“流模式"解析的API



