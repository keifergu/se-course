# 学校收费管理系统需求文档

---

### 1. 引言
#### 1.1 项目背景
- 人工缴费耗时多，效率低，容易出错
- 互联网支付系统已经成熟，如支付宝、微信支付
- 智能手机普及率十分高，使用手机支付十分便捷
- 学校人数众多，使用网络缴费系统能够有效降低人工成本
#### 1.2 编写目的
- 对学校收费管理系统的进行可行性分析
- 对客户需求进行梳理，明确功能点
- 进行数据建模，明确数据关联性及依赖性
- 进行交互设计，初步拟定人机交互模型
- 进行内部模块接口以及类接口设计
#### 1.3 定义
- Order： 订单系统
- Operator: 操作人员，指财务工作人员
- User: 用户
- Money： 款项系统
- TPPI: 第三方支付接口
### 2. 可行性及需求分析
#### 2.1 可行性分析
- 用户数： 目前我国高校学生人数普遍在 2万 ~ 3万之间，加上教职工人数，总用户数则在 3万左右。由此，该收费管理系统现阶段应设计为最大能够承载 10万的用户，技术难度可控。
- 系统复杂度： 收费管理系统需要处理用户的所有费用项目的收缴，以及对于缴款记录的查询功能，复杂度可控。
- 支付： 第三方支付系统，如支付宝、微信支付，已经十分普及，用户基数大。接入第三方支付技术难度小，可靠性高，易于实现。
#### 2.2 需求分析
- 财务人员管理功能
	1. 登录：财务人员应该使用自己的账号密码登陆财务人员管理中心
	2. 用户信息更改：能够对用户的姓名院校等信息进行更改
	3. 添加款项：添加一项新的应缴款项，应指明款项名称，应收金额等信息
	4. 款项配置：对款项应缴人员进行批量设置
	5. 统计：对当前的缴款情况进行统计
	6. 数据表导出：将统计所得结果进行Excel表格的导出，例如未缴款名单
- 用户功能
	1. 登录：使用密码登录用户中心
	2. 查看应缴款项：系统将用户的应缴款项名称，金额显示在界面上
	3. 付款：调用第三方支付接口，等待第三方返回支付成功或失败的信息
	4. 查看历史还款记录：用户应该能够查看历史的还狂记录，包括还款时间，还款金额，交易状态等信息
- 订单系统
	1. 新建订单：根据用户ID、款项、当前时间、金额等数据生成订单
	2. 查询订单：根据用户ID或其它参数查询订单记录
#### 2.3 操作人员条件
- 能够熟练进行中文输入
- 了解Excel的基本操作
### 3. 功能模型
#### 3.1 用例
![用例](https://raw.githubusercontent.com/keifergu/se-course/master/viso/user-cases.png)

- 登录 (login)
- 查看应缴款项 (getItem)
- 付款 (Pay)
- 第三方接口 (TPPI)
- 生成订单流水号 (Order)
- 查看历史还款记录 (showHistory)
- 导出数据表(export)
#### 3.2 类模型

![类模型](https://raw.githubusercontent.com/keifergu/se-course/master/viso/class.png)

#### 3.3 交互模型
- 用户查看应缴款项

```flow
st=>start: 查询应缴款项
getItem=>condition: 是否有未缴款？
show=>operation: 显示未缴款
e=>end

st->getItem
getItem(yes)->show->e
```

- 用户缴款

```flow
st=>start: 显示应缴款
choose=>inputoutput: 选择支付方式
order=>operation: 生成订单
pay=>subroutine: 使用第三方支付
payr=>condition: 是否支付成功？
re=>operation: 从应缴款中删除相应项
e=>end

st->choose->order->pay->payr
payr(no)->choose
payr(yes)->re->e
```

- 显示用户历史缴款记录

```flow
st=>start: 查看历史缴款记录
op=>operation: 从订单流水中查询
s=>inputoutput： 显示查询结果
e=>end

st->op->s->e
```
- 添加款项

```flow
st=>start: 添加款项
op=>inputoutput: 设置款项名称、金额等
add=>operation: 添加进款项表
e=>end

st->op->add->e
```

- 统计用户缴款记录

```flow
st=>start: 统计缴款记录
io=>inputoutput: 选择查询类型
r=>inputoutput: 显示查询结果
ch=>condition: 是否导出数据表格
ex=>subroutine: 导出表格
e=>end

st->io->r->ch
ch(yes)->ex
ch(no)->e
```
### 4. 数据对象模型
#### 4.1 对象关联图（ERD）
![数据关联图](https://raw.githubusercontent.com/keifergu/se-course/master/viso/data-model.png)

#### 4.2 对象规范说明
1. 财务人员信息
2. 用户信息
3. 款项信息
4. 订单信息
### 5. 运行环境