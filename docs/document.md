# 开发规范
本规范基于主流开发规范，并结合自身开发团队开发风格，形成的一份相对轻量的Java开发规范。规范并不是标准的约束，每个使用者应根据公司自身的实际情况调整，以便更加贴合实际，而不是为了规范而规范。

## 一、技术栈规约

|应用场景|技术栈|版本|备注|
|--|-----|-----|-----|
|Java|SDK|JDK|8|
|IDE|IntelliJ IDEA||
|构建工具|Apache Maven||
|核心框架|Spring、Spring Boot、Spring Cloud、Spring Cloud Alibaba| |
|注册中心+配置中心|Alibaba Nacos| | |
|服务网关|Spring Cloud Gateway|||
|分布式事务|Alibaba Seata||
|高可用流量管理框架（断路器）|Alibaba Sentinel||
|服务调用|Spring Cloud Openfeign|||
|应用服务器|| |
|持久框架||
|支付框架|||
|文件上传|||
|数据库|MySQL||
|MySQL数据库连接池|||
|缓存|Apache Redis（spring-data-redis）| |
|分布式锁|Redisson| |使用公共组件
|消息队列|RabbitMQ（spring-rabbit）、kafka||使用公共组件
|搜索引擎|Elastic Search||
|负载均衡、静态服务器|Apache Nginx||
|安全框架（登录、权限、认证）|Spring Security + OAuthor2 + JWT Token||
|定时任务|Spring Scheduler / powerJob||
|日志处理|Logback| |
|JSON处理|Alibaba Fastjson| |
|模板引擎（PC前端）|Vue||
|移动端（h5、小程序）| ||
|UI组件|Alibaba Ant Design|

## 二、编程规约
### (一) 命名风格
1. 包名统一使用小写，点分隔符之间有且仅有一个自然语义的英语单词。包名统一使用单数形式，但是类名如果有复数含义，类名可以使用复数形式。命名区分度要高，减少与现有模块的命名冲突。包名、groupId 统一使用前缀com.lshq ，若不是请确认。
   
    __正例__：package com.lshq.his.core.util / public class XmlUtils

2. 代码中的命名均不能以下划线或美元符号开始，也不能以下划线或美元符号结束，名称不能使用JAVA中的关键字。

   __反例__：_name / __name / $name / name_ / name$ / name__ / __double__

3. 命名使用英文词组合，严禁使用中文拼音或拼音首字母组合命名（专有名词例外）。
4. 类名使用 UpperCamelCase 风格，但以下情形例外：DO / BO / DTO / VO / AO / PO / UID 等。

   __正例__：UserService / OrderDTO / MemberCenterVO

5. 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格。

   __正例__：goodsName / editStoreAddress() / memberId

6. 常量命名全大写，单词间下划线分隔。

   __正例__：JD_MYSQL_READ_USERNAME / UNKNOWN_ERROR_MSG

7. 基类/抽象类使用Base/Abstract等前缀标识；异常类命名使用 Exception 结尾；测试类命名以 Test 结尾。

   __正例__：AbstractIdGeneratorFactory / BaseController

8. 如果使用了设计模式，需在命名上有所体现，便于快速理解设计理念。

   __正例__：
    ``` javascript
    public class OrderFactory;
    public class LoginProxy;
    public class ResourceObserver;
    public final class DataSourceBuilder;
    ```
9. Service层若不涉及多类实现无需定义接口和接口实现，只定义单个业务类即可，命名按 XxxService 方式。

   __正例：__
    ```javascript
    @Component
    public class GoodsCategoryService { }
    ```
10. 枚举类名带上 Enum 后缀，枚举成员名称需要全大写，单词间用下划线隔开。

    __正例：__
    ```javascript
    public enum ErrorCodeEnum {
        TOKEN_ERR(5, "token is error"),
        SYS_ERR(6, "sys error");
        // ………………
    }
    ```
11. 命名避免不规范的缩写（固有写法除外），且尽可能的使用完整的单词组合来表达，做到见名知义。
    
    __反例__：OrderFactory 缩写为 OrderFac

12. 如果一个变量用于描述多个数据时，尽量使用单词的复数形式进行书写。另外如果表述的是一个Map结构，则应使用”Map”作为变量名后缀。
    
    **正例：**
    ```javascript
    Collection<Order> orders;
    int[] values;
    Map<String,User> userMap;
    ```
13. 所有接口方法不需要用 public 修饰符来修饰。
    __反例：__
    ```javascript
        public interface IdGenerator {
            public Long nextId();
        }
    ```
14. 【参考】各层命名规约：
- Controller 层方法命名规约
  1. 获取多个对象的方法用 list ，获取其他多个对象用 xxList 。
  2.  插入的方法用 insert 做前缀。
  3.  删除的方法用 delete 做前缀。
  4. 修改的方法用 update 做前缀。
  
- Service 层方法命名规约
  1. 获取单个对象的方法用 get 做前缀。
  2. 获取多个对象的方法用 get做前缀，list 做后缀，如：getGoodsList 。 如果需要扩展方法，需按照方法功能、返回值、需要的参数进行命名，如get ArticleList By CateId 根据分类id获取文章列表。
  3. 获取统计值的方法用 get做前缀，count 做后缀 如：getGoodsCount 。
  4. 插入的方法用 save/insert 做前缀。
  5. 删除的方法用 delete 做前缀。
  6. 修改的方法用 update 做前缀。
  
- 领域模型命名规约
  1. 数据对象：xxx，xxx 即为数据库表名。
  2. 数据传输对象：xxxDTO，xxx 为业务领域相关的名称。
  3. 展示对象：xxxVO，xxx 一般为网页名称。
  4. POJO 是 DO/DTO/BO/VO 的统称，禁止命名成 xxxPOJO。
  5. Example 对象：xxxExample，xxx 即为数据库表名。

### (二) 常量定义
1. 代码在 long 或者 Long 赋值时，数值后使用大写字母 L，不能是小写字母 l，小写容易跟数字混淆，造成误解。

    __反例：__ Long a = 2l; 写的是数字的 21，还是 Long 型的 2？

   __正例：__ Long a = 2L;

2. 常用字符串统一定义在常量类里，如: “utf-8”, “yyyy-MM-dd”。

3. 常量类需要按功能进行分类，各司其职，分开维护。不能一个常量类维护所有常量。

   __正例：__ PayConst / ResponseConst / OrderPaymentConst

###  (三)注释规约
1. 所有的类都必须添加创建者和创建日期。

    说明：在设置模板时，注意 IDEA 的@author 为`${USER}`，而日期的设置统一为 yyyy/MM/dd 的格式。

2. 类、类属性、类方法的注释必须使用 Javadoc 规范，使用/**内容*/格式，不得使用// xxx 方式。
   
   __正例：__
    ```
   /**
     * 根据条件获取商品表（SKU），指定特定规格列表
     *
     * @param example 查询条件信息
     * @param pager 分页信息
     * @return
     */
     public List<Product> getProductList(ProductExample example, PagerInfo pager) {}
    ```
3. 方法内部单行注释，在被注释语句上方另起一行，使用//注释。方法内部多行注释使用/* */注释，注意与代码对齐。

    **正例：**
    ```javascript
    //参数校验
    this.publishCheck(paramDTO);    
    ```

4. 所有的枚举类型字段必须要有注释，说明每个数据项的用途。

    **正例：**
    ```javascript
    public enum ErrorCode {
    /**
    * token is wrong
    */
    TOKEN_ERR(5, "token is error"),
    /**
    * server internal error
    */
    SYS_ERR(6, "sys error");
    ```

### (四) 逻辑规约
1. 关注代码警告，分析告警内容，通过优化代码写法等方式做到消除警告，提升软件交付客户的代码阅读体验。

2. 关于基本数据类型与包装数据类型的使用标准如下：

   - 所有的 POJO 类属性必须使用包装数据类型。
  
   - RPC 方法的返回值和参数必须使用包装数据类型。
  
   - 【参考】 所有的局部变量使用基本数据类型。

3. 定义数据对象 Domain 类时，属性类型要与数据库字段类型相匹配，避免溢出问题及隐式转换导致索引失效等问题。

    **正例：**数据库类型为 bigint 则类的属性类型必须为 Long 类型。

4. 避免通过一个类的实例化对象来访问此类的静态变量或静态方法，直接使用类名即可。

5. Object 的 equals 方法容易抛空指针异常，常量或确定有值的对象放在前面进行调用。

    **正例：** "test".equals(object);
    
    **反例：** object.equals("test");

6. 如果是方法重写覆盖，方法上必须加@Override注解。

7. 需要序列化的Bean类统一实现Serializable接口并用IDE生成serialVersionUID。

   **正例：**
    ```javascript
        public class GoodsCategory implements Serializable {
        private static final long serialVersionUID = -2559915237044765353L;
        …… }
    ```
8. 避免大类和长方法，类和方法要遵循职责单一原则，若过大则需考虑拆分重构，单个方法尽可能不超过80行，最多120是上限。

9. 集合初始化时，指定集合初始值大小。无法确定容量大小的可以给默认值。

    **正例：**
    ```javascript
    HashMap<String, User> userMap = new HashMap<>(16); // 16是HashMap默认值。
    
    List<Integer> ids = new ArrayList<>(10)；
    ```

10. 使用判断逻辑时多用封装，比如判断集合内部的元素是否为空，使用 isEmpty()方法，而不是 size()==0 的方式。再比如判断订单是否需要支付，使用封装的needPay() 而不是 order.getTotalAmount() > BigDecimal.ZERO。逻辑变了只需调整内部一个地方，反之需要调整所有依赖。

    **正例：**
    ```javascript
    public class Order {
    
    public Boolean needPay() {
    
    return this.totalAmount > BigDecimal.ZERO;
    
    }}
    ```
11. 如果方法返回值是集合类型，无任何元素时不能直接返null，需要构造空集合返回，避免上层调用层层判空处理。
    
    **正例：**
    ```javascript
    public List<MaterialCategory> listMaterialCategory() {
        List<MaterialCategory>categories = this.getMaterialCategory();
        if (CollectionUtils.isEmpty(categories)) {
            return Collections.emptyList();
        }
        return categories;
    }
    ```
12. 做好逻辑性校验，处理任何业务逻辑时要校验该操作是否符合该场景前置条件， 比如：修改订单要判断是否符合修改的条件；订单已经申请退款了，则不应该通过发货接口还可以进行发货；支付的时候订单金额已经变化了，则不应该再支付成功，回调成功。

13. 提炼公共组件，能提炼到公共地方的，一定要提炼，可以减少代码冗余，实现复用。

14. 尽量使用标准组件、统一化， 如果有bug，那么也只是修改一个地方即可。

### (五) 前后端规约
1. 后端返回数据格式统一为json格式，如果是新增要返回id（依据需求，非必须）。

    **正例：**
    ```json
    {
      "code":0,
      "data":{},
      "msg":"成功",
      "timestamp":1665477849519
    }
    ```

2. 给前端数据列表相关的接口返回，如果为空，则返回空数组[]或空集合{}，不能是null。

3. HTTP 请求通过 URL 传递参数注意参数长度，不能超过 2048 字节。通过 body 传递内容时，必须控制长度，超出最大长度后，后端解析会出错。(nginx 默认限制是 1MB，tomcat 默认限制为 2MB)

4. 在分页查询中，若后端接收到的页码参数小于 1，则返回首页数据；若接收到的页码参数大于总页码，则返回最后一页。

5. 后端接口返回列表时不能用disabled，会跟前端react里面的是否选中冲突。

6. 若要展示状态值的描述，在状态值字段后加Value，如state状态值为20，30 则状态值描述为 stateValue 对应的内容为 审核通过，审核失败。

7. 状态值的定义要考虑扩展性，如code 状态值定为 10 11 12 13 14 现需要在中间再加状态则无法满足。若状态值为 100 200 300 400 则足够兼容未来状态扩展需求。

8. 接口code 状态值字典

0 正常，

255 失败

259 没有权限

260 需要登录

……未完待续

msg: //操作成功的提示，操作失败的提示（失败的具体原因），没有权限的提示，服务器发生错误

### (六) 测试规约
1. 复杂模块，尽量编写单元测试，这样在模块调整后方便快速测试。

2. 接口交付给前端需保证可用，交付前要自测，不允许接口不通等各种问题影响集成。

## 三、安全规约
### (一) 数据安全
1. 后端所有敏感数据脱敏处理，如用户手机号、身份证号、银行卡号码、密码等，禁止明文传输、明文存储。

2. 前端展示的数据不能包含未脱敏的敏感数据。

3. 做好用户权限控制校验，防止随意访问、修改他人数据，比如a用户看到了b用户的订单。

4. 修改用户信息，尤其是密码的时候，需要校验用户和密码。

5. 重要数据使用强加密算法加密，传输使用https（SSL）。

6. 接口不能返回多余数据到前端，控制VO对象字段，只返需要部分。

### (二) Web安全
1. 对前端接收到的参数做合法性校验，杜绝非法参数。比如pageSize = 1000000, 不能直接丢给数据库执行。

2. 对输入内容统一进行XSS过滤，防止XSS攻击。

3. 禁止手工拼接SQL参数，应使用参数化方式创建SQL语句，避免SQL注入。

4. 及时更新依赖组件版本，高版本一般会解决历史安全问题。关注开源组件漏洞，发现后要及时修复。

5. 特定业务场景或模块需做防重、防刷机制。比如短信、下单、支付等模块，验证码短信不能无限制发送；用户很短时间内（1秒）下了多笔相同的订单是不合理的；又如一笔订单短期被重复支付了多次也是不应该的。

## 四、数据库规约
### (一) 库表规约
1. 表名、字段名必须使用小写字母或数字，禁止出现数字开头，禁止两个下划线中间只出现数字。

    **正例：** member_address，level3_name
    
    **反例：** MemberAddress，level_3_name

2. 表名、字段名禁止使用数据库关键字，如：name，time ，datetime， password 等。

3. 表名称不应该取得太长（一般不超过三个英文单词），表名不使用复数名词。

    **正例：** good_picture
    
    **反例：** good_pictures

4. 创建表时字段必须填写描述信息。

5. 在命名表的列时，不要重复表的名称 例如，在名employe的表中避免使用名为employee_lastname的字段。

6. 字段命名使用完整名称，禁止缩写，字段名不要包含数据类型。

7. 明细表的名称为：主表的名称+detail；扩展表的名称为：主表的名称+extend。

8. 特殊业务场景不能将主键id直接暴露使用，比如不能将订单表自增id字段与前端交互，防止通过id值推测单量。可以另建id或者其他名称表示，比如订单使用order_sn字段。

9. 商品表、货品表、订单表、订单货品表、商家表、商家账号表等此类的业务主键（非自增id）采用bigint、非自增；其余表采用int，自增。

10. 涉及金额字段使用 decimal(10,2) 类型。

11. 新的数据库 表 字段字符集统一使用utf8mb4，排序规则utf8mb4_general_ci。现有的库新加表或字段字符和排序规则保持和当前表一致，一般为 utf8 utf8_general_ci。如果要区分大小写则utf8对应utf8_bin规则， utf8mb4对应utf8mb4_bin规则。若字段需要支持Emoji表情符号则字符集使用utf8mb4。

    说明：如果将两个表的关联字段的字符集不相同，而且排序规则也不同，那么索引会失效。
    
    如果将两个表的关联字段的字符集保持相同，但是排序规则不同，那么索引会失效。

12. 表必备字段：create_by, create_time, update_by, update_time, remark。

13. 通用字段：
    
    create_by bigint 创建人

    create_time datetime 创建时间

    update_by bigint 修改人

    update_time datetime 修改时间

    remark varchar 备注，冗余字段

### (二) 索引规约
1. 为常用于查询且区分度较高的字段建立索引，提升查询性能。比如为Order表的OrderNumber字段建立唯一索引。

2. 减少多表join关联操作，如需join，字段的数据类型要保持一致，而且最好关联的字段有索引。

3. 在varchar可变长字段上建立索引，最好指定索引长度，只要有较高的区分度即可，无需全字段建立索引。

4. 在建立组合索引时，遵循最左匹配原则，左侧应该用区分度最高的字段。

## 五、Git规约
### (一) 开发分支
1. 开发分支从主干master拉取，命名如dev_xxx。

2. 修复分支从主干master拉取，命名如hotfix_xxx。

3. 如有必要定期清理已完成上线的开发分支。

### (二) 发布测试
1. 测试分支从主干master拉取，一般命名为test。

2. 开发分支发布测试时，需merge到测试分支test，如多个开发分支同时需要发布测试，都遵循统一的规则先将开发分支合并到test，以test分支发布测试，避免测试环境相互覆盖。

### (三) 发布生产
1. 禁止将开发分支直接merge到主干master发布生产。

2. 如要发布生产则先从主干master拉取一个release分支作为上线分支，比如release_tonglian_pay，然后将开发或修复分支merge到release分支，再将release分支进行发布。待release分支验证通过后，再将release分支merge到主干master。

### (四) 提交推送
1. commit操作必须填写提交备注。

