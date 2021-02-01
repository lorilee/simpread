> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [mp.weixin.qq.com](https://mp.weixin.qq.com/s/oG7NQNXkjBvkuRIhkiIBgw)

![](https://mmbiz.qpic.cn/mmbiz_gif/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO7vgTGP53icxyLKZKC6yAKvs5s479MoYDVbObrnSuxoXbzNIFMmaX83g/640?wx_fmt=gif)

![](https://mmbiz.qpic.cn/mmbiz_jpg/Pn4Sm0RsAujOqLZ9JYvS6eq3aXT4otUaRhaUssbXcmXzLib5YwyXxxmXSCRG12oJ5bBvRliaZsHNfLaGf6gjjiaAA/640?wx_fmt=jpeg)
==============================================================================================================================================

作者 | Brian Kane @SeekWell  

编译 | 伍杏玲  

出品 | AI 科技大本营（ID：rgznai100）

【导语】手写业务 SQL 很繁琐？GPT-3 来帮你！本文作者通过手动输入简单的英文描述秒 Get 到 SQL 了。听说 AI 又来抢开发者饭碗，一起来看看吧：

作为分析师，我花费大量时间编写 SQL 来回复各类业务问题，如简单的客户查询问题 “用户 X 是否有正确的计划”，评估 KPI，增长指标 “上个月有多少用户注册，转换为付费用户的百分比是多少”，预估问题 “12 个月内我们将获得多少收入”。

为了让我的工作更轻松，我尽量使用自动化来完成。尽管公司研发一些工具来帮助解决，但需要写 SQL 代码的话用当前工具很难自动化，GPT-3 来救驾！

GPT-3 是一种文本完成引擎，它接受输入的文本并返回它认为接下来会出现的文本。现在已经有很多人使用它根据指定说明来生成 HTML、CSS 等代码。

由于我的大部分工作是写 SQL，所以我希望通过简单的英语描述语句，GPT-3 将这些语句转为 SQL 代码。接着执行该 SQL，等待页面返回结果。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOc2rXicCsC79VyqrfQywa6E74wdAaexVSMb4RYiacoJ9E71XJcjnkdWwA/640?wx_fmt=jpeg)

  

对我来说，GPT-3 Instruct（仍处于 beta 版）比更通用的 GPT-3 引擎更有效果，GPT-3 Instruct（尤其是 davinci-instruct）可让你给出 GPT-3 响应的特定指令。下面我将详细为大家展示下一些示例，其中 gif 里输入问题由我生成，为绿色，GPT-3 将其转换为 SQL（由 GPT-3 生成，为蓝色），我对 GPT-3 说明（黄色）和 GPT-3 示例（橙色）。

这是我给的第一条指令，一个问题以及 GPT-3 的回复：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPRBHx52RaKMXpToYlLOqMxzRRzSQK4o6RZ8659WVh5S1jfEgdHIL62Q/640?wx_fmt=gif)

说明：给定输入问题，请使用语法正确的 PostgreSQL 进行响应。可以有创新，但 SQL 必须正确

输入：how many users signed up in the past month?

GPT-3：

```
SELECT COUNT(*) FROM users
WHERE signup_time > now() - interval '1 month'
```

我没给过数据库架构，但假设我有一个名为 “users” 的表，有 “ signup_time” 时间戳字段用于用户注册，这个符合逻辑。它知道在 PostgreSQL 中从今天的日期减去一个月的正确方法（now（）- 间隔“ 1 个月”）。太酷了！

为了给 GPT-3 提供更多的上下文，我将有关数据库模式的信息添加到指令中。我的新指令：

说明：给定输入问题，请使用语法正确的 PostgreSQL 进行响应。仅使用 “用户” 和“收费”的表。“用户”表具有以下列：id（整数），signup_dt（时间戳），电子邮件（字符变化）和 plan_type（字符变化）。“费用”表中包含列：金额（bigint），user_id（整数）和 charge_dt（时间戳）。

我问另一个问题，看看 GPT-3 是否可以根据实际数据库来响应：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPUNCjtSKdiakBtUODZCBKq1Pz4msQMWqrc0OLT1jfomWoLGzuDOz0vZw/640?wx_fmt=gif)

输入：how much revenue did we have in the past 7 days?

GPT-3：

```
SELECT SUM(amount) FROM charges WHERE charge_dt > now() - interval '7 days'
```

我向它提供了有关数据库中表和列的更多信息，它正确地推断出 “amount” =“ revenue”，并 SUM（）来统计 7 天收费后的金额，我们看到，它已知道一些基本的 SQL 语法，并推断出我的意思。

  

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqO3ToytjEfeNjJlGPcZqK98xO4MJ2zC1xj07PfS6kAic0LAlMKFBLvWPQ/640?wx_fmt=jpeg)**

### **用示例启动 GPT-3**

我想需要通过一个简单的示例来明确告知 GPT-3 表中的列，示例是提供 GPT-3 上下文并充实一些细节的好方法，这有助于确定哪些列和表可以使用（GPT-3 有时喜欢 “发明” 列）。这是我添加到提示中的示例：

输入：select all available columns from users table.

输出：

```
SELECT id, signup_dt, email, plan_type FROM users
```

输入：select all available columns from charges table.

输出：

```
SELECT amount, user_id, and charge_dt FROM charges
```

 将这些示例提供给 GPT-3，我将再问一个问题：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqP6grqq5wuSibPfCNTKf1CbUJVv9CZ1YsPVYygRp4XpibX9cIwbjeTFXUA/640?wx_fmt=gif)

输入：what was the largest payment amount made in the past 30 days?

GPT-3：

```
SELECT MAX(amount) FROM charges WHERE charge_dt > NOW() - INTERVAL '30 days'
```

它知道使用 MAX（金额）来获得最大的付款并正确设置日期条件，没看到有关如何从当前日期减去天数的示例。我们再试试其他的：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPtklNpjLI8jeDYP24JUYAffjOcwwgfm2tBCheM5kIM0DbENwO7eCyjw/640?wx_fmt=gif)

输入：when did user with email brian@seekwell.io make his last payment?

GPT-3：

```
SELECT MAX(charge_dt) FROM charges WHERE user_id = 1 AND email = 'brian@seekwell.io'
```

不太对，“收费”表中没有电子邮件列，因此该查询将失败。我也从来没有说过电子邮件 =“ brian@seekwell.io”的用户的 user_id = 1 - 不知道从何而来。要正确回答该问题，必须将 “收费” 表与 “用户” 表结合在一起，因为只有 “用户” 有电子邮件列。

由于知道在 users.id = Charges.user_id 上联接这些表是一个大的进步，所以我将给 GPT-3 一个 “联接” 的示例：

输入：when did jane@seekwell.io make her first payment?

输出：

```
SELECT MIN(charge_dt) as last_payment_dt from charges LEFT JOIN
users ON users.id = charges.user_id
WHERE users.email = 'jane@seekwell.io'
```

我再试一次：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPylzmhAyaSKKdWbUnIt1y6rH9LjqQkU5PelKsJ5r8vtNssWyo7wMj7w/640?wx_fmt=gif)

输入：when did user with email brian@seekwell.io make his last payment?

GPT-3：

```
SELECT MAX(charge_dt) as last_payment_dt from charges LEFT JOIN users ON users.id = charges.user_id WHERE users.email = 'brian@seekwell.io'
```

根据示例，它变得太简单了，它正确地更改了 MIN（）→MAX（）并用我要求的电子邮件替换了该电子邮件。我们尝试另一个难度级别，先加更多示例来提供更多背景信息并改善响应。新的例子：

输入：how many users signed up in the past 30 days?

输出：

```
SELECT COUNT(*) FROM users WHERE signup_dt >= now() - interval '30 days'
```

输入：when did user with email brian@seekwell.io sign up?

输出：

```
SELECT signup_dt FROM users WHERE email = 'brian@seekwell.io'
```

输入： how much revenue did we have in the past 7 days?

输出：

```
SELECT SUM(amount) from charges WHERE charge_dt >= now() - interval '7 days'
```

输入：how much revenue did we have from 10-01-20 through 11-15-20?

输出：

```
SELECT SUM(case when charge_dt>= '10-01-20'::date and charge_dt < '11-15-20'::date then amount else 0 end) as revenue
FROM charges
```

输入：how much revenue have we had from users that signed up in the last 6 months?

输出：

```
SELECT SUM(charges.amount) FROM users INNER JOIN
charges ON users.id = charges.user_id
WHERE users.signup_dt>= now() - interval '6 months'
```

输入：when did user with email brian@seekwell.io make his first payment?

输出：

```
SELECT MIN(charge_dt) as last_payment_dt from users INNER JOIN
charges ON users.id = charges.user_id
WHERE users.email = 'brian@seekwell.io'
```

输入：how many new users signed up in each of the last 2 months?

输出：

```
SELECT sum(case when signup_dt>= now() - interval '1 month' then 1 else 0 end) as signups_this_month,
sum(case when signup_dt>= now() - interval '2 months' and signup_dt < now() - interval '1 month'  then 1 else 0 end) as signups_last_month
FROM users
```

这些示例为它提供了一些基本查询，因此它更加熟悉我的数据库以及用例的特定术语和概念（例如，“付费用户” 表示其 plan_type =“ pro”）。我还介绍了汇总两个日期之间的指标（收入）。我们继续加深难度：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPADVEydSqcf4ZzibicmbedTWPrWZebbTnw3nxogM13TIdct5h8YevCHNQ/640?wx_fmt=gif)

输入：how many payments has user with email joe@freshpizza.com made?

GPT-3：

```
SELECT COUNT(*) FROM users INNER JOIN  charges ON users.id = charges.user_id WHERE users.email = 'joe@freshpizza.com'
```

GPT-3 认为这里的 COUNT（）是适当的表达式，知道将 “用户” 与“收费”表结合在一起，并将电子邮件正确设置为我所询问的电子邮件。GPT-3 的另一个问题：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPc43RmKdicn7ob5a9tJnN9GiaREjkV7t2yr826NKAC55M14LLsKHHjzvA/640?wx_fmt=gif)

输入：how much revenue have we received from users with an email ending in 'seekwell.io' in the last 3 months?

GPT-3：

```
SELECT SUM(charges.amount) FROM users INNER JOIN charges ON users.id = charges.user_id WHERE users.email LIKE '%seekwell.io%' AND users.signup_dt>= now() - interval '3 months'
```

太棒了，它知道将 “以 seekwell.io 结尾的电子邮件” 在 SQL 中转换为“ users.email LIKE'％seekwell.io％'”（即使之前从未见过），并对与此匹配的用户收取了所有费用的总和健康）状况。还有一个：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPVyt1pdD5oTyVVvaqPib00JZY6mVDUCC8ZCW82eg4am8hMrjpPHICWaw/640?wx_fmt=gif)

输入：how much revenue did we have between June 1st and August 1st 2020?

GPT-3：

```
SELECT SUM(case when charge_dt >= '06-01-2020'::date and charge_dt < '08-01-2020'::date then amount else 0 end) as revenue FROM charges
```

我已经教过它如何从 10-01-20 到 11-15-20 来获取收入，所以它要容易一些，它确实知道将 6 月 1 日和 8 月 1 日转换为 SQL 中适当的日期格式（'06- 01-2020”和 “08-01-2020”）。如果它只是用“ 6 月 1 日” 和“ 8 月 1 日”替换了我示例中的日期，我它不是在获取日期的概念，只是将模式匹配到最接近的示例。有了这个结果，我可以知道 GPT-3 至少在某种程度上了解了日期的概念。

  

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOPyiaaIVCePZnia426uNibf0PLTPfS7VRaWql6vFWCwb2Cqgjp06UvU39Q/640?wx_fmt=jpeg)**

### **将 GPT-3 “玩坏了”**

刚才我列举的例子都是关于用户、收入和增长指标等基本问题，GPT-3 处理得很好。现在我想知道有多少用户转换为付费用户：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPas2hwccYEMq5GGlPXt4hcqCtH9sCrOzNb685H1KlicSlYUwQp8ZicfWQ/640?wx_fmt=gif)

输入：what percent of users who signed up converted to paid?

GPT-3：

```
SELECT COUNT() as total_users, COUNT() as total_paid_users, 100.0*(COUNT() as total_paid_users) / COUNT() as total_users AS percent_converted FROM users
```

这不是我想要的，绝对不是正确的 SQL。它不理解 “百分比” 的概，它仅是 COUNT（）并乘以 100。我使用 “百分比” 来试验下：

输入：what percent of users have an email ending in 'gmail.com'?

输出：

```
SELECT SUM(CASE WHEN email like '%gmail.com' then 1 else 0 end)/COUNT(*) as percent_gmail
FROM users
```

当我尝试相同的问题时，我得到：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPaEZ2Xgpcd2FAicL6ZYVOmWU17ZtPc2HTbibQKLib7N479Lpj7YEtttgsA/640?wx_fmt=gif)

输入：what percent of users who signed up converted to paid?

GPT-3：

```
SELECT SUM(CASE WHEN plan_type = 'pro' THEN 1 else 0 end)/COUNT(*) as percent_paid FROM users
```

整洁多了，它知道将 plan_type ='pro'的用户数除以用户总数即可得到我要求的百分比。我还可以向部分用户（例如最近 6 个月内注册的用户）询问相同的问题，并获得准确的答复：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPT0OuEssYcPCbaAJNjFjiawKzyqKYVlhxdqSjJ7FvYVjLKjrHXBkKJIw/640?wx_fmt=gif)

输入：what percent of users that signed up in the past 6 months converted to paid?

GPT-3：

```
SELECT SUM(CASE WHEN plan_type='pro' THEN 1 ELSE 0 END)/COUNT(*) AS percent_paid FROM users WHERE users.signup_dt>= now() - interval '6 months'
```

  

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOUuicXVGTwzDI9d46uDtYVV6QiaicNDpwPS4L8YeTopM3y06A6v6pamjoA/640?wx_fmt=jpeg)**

### **调整 GPT-3 的参数**

除了给 GPT-3 提供指导和示例外，还可以调整参数来提高 GPT-3 的有效性。其中关键要素是 “Temperature”，它转化为 GPT-3 在响应中包含多少随机性。如果只需要最高的概率响应（即无随机性），则将 Temperature 设置为 0。

以 “最近两个月的每个月我们有多少收入？” 提示为例，将 Temperature 设置为. 8 的话：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPH4ibzicXUD4QnkBbOzV2AXmibvLwck8ezY5q1Ao8DWTbxQniakAUrqH2xQ/640?wx_fmt=gif)

输入： how much revenue did we have in each of the last 2 months?

Temperature 为. 8 时 GPT-3：

```
SELECT SUM(case when charge_dt>= now() - interval '1 month'  then charges.amount else 0 end) as revenue_this_month, CASE WHEN signup_dt>= now() - interval '2 months' AND signup_dt < now() - interval '1 month' THEN SUM(charges.amount) else 0 end as revenue_last_month FROM charges  LEFT OUTER JOIN users ON users.id = charges.user_id \ WHERE users.signup_dt >= now() - interval '1 month'
```

将 Temperature 设置为较低的. 3，可提供一个有效的查询，并回答以下问题：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPH4ibzicXUD4QnkBbOzV2AXmibvLwck8ezY5q1Ao8DWTbxQniakAUrqH2xQ/640?wx_fmt=gif)

输入：how much revenue did we have in each of the last 2 months?

Temperature 为. 3 时 GPT-3：

```
SELECT SUM(case when charge_dt>= now() - interval '1 month' then amount else 0 end) as revenue_this_month, SUM(case when charge_dt>= now() - interval '2 months' and charge_dt < now() - interval '1 month' then amount else 0 end) as revenue_last_month FROM charges
```

更高的 Temperature 让 GPT-3  变得更具 “创造力”，但也使它过于复杂并给出了几乎没有意义的响应。Temperature 设置为. 3，响应将问题正确转换为 SQL。  

根据我的经验，如果你问的问题遵循的是示例所建立的模式，则较低的 Temperature 往往会表现得更好。但是，如果您要问一些需要更多创造力的问题，则更高的 Temperature 是最佳选择，因为 GPT-3 更有可能尝试未明确教导的事情。

在下面的案例中，我问：“过去 6 个月内注册的用户给我们带来了多少收入？” （从我的示例中删除之后）。在较低的 Temperature 下，GPT-3 尝试在 “charges” 表中创建一个 “ signup_dt” 列，因此不必将 “ users” 表和 “ charges” 表连接在一起。随着 Temperature 的升高，它确实加入了，这是 Temperature 设置为. 8 的响应：

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqPK2iaPHibxqtDiaD4ZWXnPNeqNN9qV4mmtWicKibL06hwpokwa9DgjRXaIzg/640?wx_fmt=gif)

输入：how much revenue have we had from users that signed up in the last 6 months?

Temperature 为. 8 时 GPT-3：

```
SELECT SUM(charges.amount) FROM users INNER JOIN charges ON users.id = charges.user_id WHERE signup_dt >= DATE_SUB(now(), INTERVAL '6 months')
```

可以肯定的是，“DATE_SUB（now（），INTERVAL'6 months'）”不是一个有效的 Postgres 表达式，因此该查询在技术上将失败。但是从结构上讲，它通过加入 “ charges” 和“ users”而处于正确的道路上，因此可以以 “ signup_dt” 为条件。同时，使用 “ charges” 表中实际上不存在的 “ signup_dt” 列，.Temperature 设置为. 2 时响应完全不正确：  

![](https://mmbiz.qpic.cn/mmbiz_gif/VeJKXItpwPXJwpd1S4aACzXjE0AAxeqP9FSWnCmeab5EBvouXSAXJGpwwPGFhDN3q8wXBWXsKO66h4m9JyMHiaw/640?wx_fmt=gif)

输入：how much revenue have we had from users that signed up in the last 6 months?

Temperature 为. 2 时 GPT-3：

```
SELECT SUM(CASE WHEN signup_dt >= now() - interval '6 months' THEN amount ELSE 0 END) AS revenue FROM charges
```

  
  

**![](https://mmbiz.qpic.cn/mmbiz_jpg/BnSNEaficFAYgTGBXg0ZvckYHA2iaBOsqOHoPVFuCP8IMZsIKcMhGRrqicdwJpzvnXBwaI6fC2obKw3eiangqibyE5A/640?wx_fmt=jpeg)**

### **结论**

  

GPT-3 实例会根据简单的英语问题，将其转换为数据库真正有效的 SQL。它不总是完美，对 “增长率” 或“百分比”等复杂的概念仍然需要一些人工帮助，但是它对我的日常工作是有用，对于简单的 SQL，GPT-3 能帮忙减轻我的负担，并且对于不熟悉 SQL 语法的初学者很有用。

这个项目的代码传送门：https://github.com/bkane1/gpt3-instruct-sandbox

原文链接：https://blog.seekwell.io/gpt3

本文为 AI 科技大本营翻译，转载请注明来源出处。

![](https://mmbiz.qpic.cn/mmbiz_jpg/Pn4Sm0RsAuggpZ0lia5ZjmiaqEIGqmY9Ch46r8l3Cv9Lvuqq7ic7xlHVf7zNlwgiaYRMdLib25h7pMEunOCf4IwibDRw/640?wx_fmt=jpeg)

```
更多精彩推荐
```

```
☞科学“干饭”，AI 营养师教你
☞GitHub 标星超 26600，TiDB 社区运营的道与术！
☞区区几行Python代码，一分钟搞定一天工作量
☞读懂深度迁移学习，看这文就够了 
```

```
点分享
点收藏
点点赞
点在看
```