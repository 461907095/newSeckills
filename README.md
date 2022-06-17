                                                               __   _ ____
                                               ________  _____/ /__(_) / /
                                              / ___/ _ \/ ___/ //_/ / / / 
                                             (__  )  __/ /__/ ,< / / / /  
                                            /____/\___/\___/_/|_/_/_/_/
<br/>

## 高并发的瓶颈在数据库

##### 测试地址：http://47.93.242.254:8088/login/to_login<br>
###### 登录手机号：13000000000<br>
###### 密码：123456

##### 请先访问：http://47.93.242.254:8088/seckill/reset<br>
##### 如果响应为：
```
{
    code: 0,
    msg: "success",
    data: true
}
```
##### 再进行登录。
<hr>
<br>


### 减少数据库访问
思路：<br>
1. 系统初始化，把商品库存数量加载到Redis<br>
2. 收到请求，Redis预减库存，库存不足，直接返回，否则进入3<br>
3. 请求入队，立即返回排队中<br>
4. 请求出队，生成订单，减少库存<br>
5. 客户端轮询，是否秒杀成功
<hr>
<br>



### 项目框架
1. Spring Boot环境搭建<br>
2. 集成Thymeleaf，Result结果封装<br>
3. 集成Mybatis+Druid<br>
4. 集成Jedis+Redis安装+通用缓存Key封装

### 页面优化技术
1. 页面缓存+URL缓存+对象缓存<br>
2. 页面静态化，前后端分离

### 接口优化
1. Redis预减库存减少数据库访问<br>
2. 内存标记减少Redis访问<br>
3. RabbitMQ队列缓冲，异步下单，增强用户体验

### 安全优化
#### 1.秒杀接口地址隐藏
秒杀开始之前，先去请求接口获取秒杀地址<br>
思路：<br>
  (1) 接口改造，带上PathVariable参数<br>
  (2) 添加生成地址的接口<br>
  (3) 秒杀收到请求，先验证PathVariable<br>

#### 2.数学公式验证码
点击秒杀之后，先输入验证码，分散用户请求<br>
思路：<br>
  (1) 添加生成验证码的接口<br>
  (2) 在获取秒杀路径的时候，验证验证码<br>
  (3) 使用ScriptEngine<br>

#### 3.接口防刷
对接口做限流<br>
思路：
* 用拦截器减少对业务侵入

<br>

##### ps:
##### 代码是跟着慕课网实战课程《Java秒杀系统方案优化 高性能高并发实战》一步一步写的，感觉有很大的收获，推荐小伙伴们也去学习一下。
##### 课程地址：[Java秒杀系统方案优化 高性能高并发实战](https://coding.imooc.com/class/168.html)
