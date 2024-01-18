# param-verify-spring-boot-starter


## 简介
基于springmvc的参数校验开源组件，使用注解和在实现类实现对参数的校验

## 源码地址

[gitee](https://gitee.com/Xiong-qi/param-verify-spring-boot.git) 点击前往添砖加瓦

## 提供bug反馈或建议


提交问题反馈请说明正在使用的JDK版本呢、paramVerify版本和相关依赖库版本。

## 贡献代码的步骤

   在Gitee上fork项目到自己的repo

   把fork过去的项目也就是你的项目clone到你的本地

   修改代码（记得一定要修改v1-dev分支）

   commit后push到自己的库（v1-dev分支）

   登录Gitee在你首页可以看到一个 pull request 按钮，点击它，填写一些说明信息，然后提交即可。 

   等待维护者合并

- [gitee issues](https://gitee.com/Xiong-qi/param-verify-spring-boot/issues)

## 系统要求
- JDK 1.8 and above
- Maven 3.0 and above
- Spring Boot 2.0 and above

## 安装

在项目的pom.xml的dependencies中加入以下内容:

```xml

<dependency>
    <groupId>com.gitee.xiong-qi</groupId>
    <artifactId>param-verify-spring-boot-starter</artifactId>
    <version>最新版本</version>
</dependency>
```
当前版本1.0.2

版本请参考maven仓库


## 使用说明

可参考test工程中的
com.xq.test.controller.TestController
com.xq.test.controller.TestI18nController

1. 在controller的参数上加上注解@ParamValidate

```java
public class TestController {
    
   @GetMapping("/testName")
   @ParamValidate(TestValidate.class)
   public CommonBean test(String name) {
      ...
   }
}
```

2. 实现类实现接口AbstractFromValidatorImpl,并且注入到spring中
```java

@Component
public class TestValidate extends AbstractFromValidatorImpl {
    /**
     * 验证器
     */
    protected void validate() {
        validateRequired("name","必填1111");
    }
}

```
3. 配置统一返回对象

   本例中，统一返回对象的code为code1，message为message1，按照自己系统的进行调整

```yaml
## 无国际化
#参数校验配置 同时也是支持驼峰 paramVerify
param-verify:
   # 是否启用参数校验 true 启用 false 不启用
   enable: true
   # 缺少参数的code码
   missingParameter: 0
   # 错误参数的code码
   errorParameter: 1
   # 统一响应的class
   responseClass: com.xq.test.response.CommonBean
   # code对应的字段 默认值  code
   code: code
   # 消息内容提示对应的字段 默认值  message
   message: message

## 国际化
#参数校验配置 同时也是支持驼峰 paramVerify
param-verify:
   # 是否启用参数校验 true 启用 false 不启用
   enable: true
   # 缺少参数的code码
   missingParameter: 0
   # 错误参数的code码
   errorParameter: 1
   # 统一响应的class
   responseClass: com.xq.test.response.I18nCommonBean
   # code对应的字段 默认值  code
   code: code
   # 消息内容提示对应的字段 默认值  message
   message: message
   # 系统内部报错对应的字段(用户无需关注的内容) 无默认值
   errorMsg: errorMsg
   # 国际化对应的代码 无默认值
   errorCode: I18nCode
   # 国际化参数对应的字段 无默认值
   errorParam: I18nParam
```

4. 返回结果为String时,确保不是返回springmvc的视图，则参数校验组件生效

   如何确保生效，在controller中加入@RestController，或者在方法上加入@ResponseBody

```java
@RestController
public class TestController {

    @GetMapping("/testName")
    @ParamValidate(TestValidate.class)
    public String test(String name) {
        System.out.println(name);

        CommonBean commonBean = new CommonBean();
        commonBean.setCode("操作成功");
        commonBean.setMessage("11111");
        return "1";

    }
    
   @GetMapping("/testName2")
   @ParamValidate(TestValidate.class)
   public CommonBean<Person> testName2(String name) {
      System.out.println(name);
      CommonBean commonBean = new CommonBean();

      Person person = new Person();
      person.setName("1111");
      commonBean.setData(person);
      return commonBean;

   }
    
    
}

```


## 常见问题
1. 引入了依赖不想使用如何处理
    
    可在配置文件加入以下配置可禁用改组件功能
```properties
# 是否启用参数校验 true 启用 false 不启用
paramVerify.enable=false 
```
   yml文件同理

```yaml
#参数校验配置
paramVerify:
# 是否启用参数校验 true 启用 false 不启用
  enable: true
```


## 更新记录

[更新记录](https://gitee.com/Xiong-qi/param-verify-spring-boot/blob/v1.0-master/CHANGELOG.md)
