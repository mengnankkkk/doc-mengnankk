---
title: Mysql练习 十月篇
categories:
  - sql
  
abbrlink: 2701
date: 2024.10.9
tags: 
   - mysql 

---

# if结构

## 1.[**VQ22** **判断其是否有过购买记录**](https://www.nowcoder.com/practice/5cddad8995974b0c915ab89961428ee0?tpId=341&tqId=10100101&ru=/exam/oj&qru=/ta/vip-sql/question-ranking&sourceUrl=%2Fexam%2Foj)



现有牛客顾客购买信息表customers_info，请查询客户id并新增一列判断该客户是否有过购买记录(`latest_place_order_date` 1为有 0为没有)

![](https://uploadfiles.nowcoder.com/images/20220816/0_1660618848686/F8BAD4BF0D08CAA5EC5E3E28353944E4)

题解：

```
select
    customer_id,
    if (latest_place_order_date, 1, 0) as if_placed_order
from
    customers_info
```

直接使用if语句

注意：

**if语句的使用**

if(condition1,0)

条件判断成功则为1，判断失败则为0

## 2.[**VQ23** **请按城市对客户进行排序,如果城市为空，则按国家排序**](https://www.nowcoder.com/practice/c1b1d5bd008b4797ab2ef53a3afc4aba?tpId=341&tqId=10100109&ru=/exam/oj&qru=/ta/vip-sql/question-ranking&sourceUrl=%2Fexam%2Foj)

这个题目和上一个题目共用一个表，所以我们直接写题解

题解：

```
select customer_id, gender, city, country, age, latest_place_order_date
from customers_info
order by
    case
        when city is null then country
        else city
    end;
```

我们主要是使用了order的分组，然后设置了第一个和第二个的比较条件

然后end就可以了捏

## 3.[**VQ24** **分群并计算群体人数**](https://www.nowcoder.com/practice/24319fd5c03e482c935783107114933d?tpId=341&tqId=10100113&ru=/exam/oj&qru=/ta/vip-sql/question-ranking&sourceUrl=%2Fexam%2Foj)

这个题目仍然和上一个一样，所以我们直接写题解

```
select 
    case
        when age < 20 then '20以下'
        when age < 50 then '20-50'
        when age > 50 then '50以上'
        when age is null then '未填写'
    end
    age_group,
    count(customer_id) as user_count
from customers_info
group by age_group;
```

主要就是用了case+when进行分组，然后其他的照常理来捏

# 某音面试题目

## 4.[**SQL156** **各个视频的平均完播率**](https://www.nowcoder.com/practice/96263162f69a48df9d84a93c71045753?tpId=268&tqId=2285032&ru=/exam/intelligent&qru=/ta/sql-factory-interview/question-ranking&sourceUrl=%2Fexam%2Fintelligent%3Ftype%3Dvip)

用户-视频互动表tb_user_video_log

| id   | uid  | video_id | start_time          | end_time            | if_follow | if_like | if_retweet | comment_id |
| ---- | ---- | -------- | ------------------- | ------------------- | --------- | ------- | ---------- | ---------- |
| 1    | 101  | 2001     | 2021-10-01 10:00:00 | 2021-10-01 10:00:30 | 0         | 1       | 1          | NULL       |
| 2    | 102  | 2001     | 2021-10-01 10:00:00 | 2021-10-01 10:00:24 | 0         | 0       | 1          | NULL       |
| 3    | 103  | 2001     | 2021-10-01 11:00:00 | 2021-10-01 11:00:34 | 0         | 1       | 0          | 1732526    |
| 4    | 101  | 2002     | 2021-09-01 10:00:00 | 2021-9-01 10:00:42  | 1         | 0       | 1          | NULL       |
| 5    | 102  | 2002     | 2021-10-01 11:00:00 | 2021-10-01 10:00:30 | 1         | 0       | 1          | NULL       |

（uid-用户ID, video_id-视频ID, start_time-开始观看时间, end_time-结束观看时间, if_follow-是否关注, if_like-是否点赞, if_retweet-是否转发, comment_id-评论ID）

短视频信息表tb_video_info

| id   | video_id | author | tag  | duration | release_time        |
| ---- | -------- | ------ | ---- | -------- | ------------------- |
| 1    | 2001     | 901    | 影视 | 30       | 2021-01-01 07:00:00 |
| 2    | 2002     | 901    | 美食 | 60       | 2021-01-01 07:00:00 |
| 3    | 2003     | 902    | 旅游 | 90       | 2021-01-01 07:00:00 |

（video_id-视频ID, author-创作者ID, tag-类别标签, duration-视频时长（秒）, release_time-发布时间）

**问题**：计算2021年里有播放记录的每个视频的完播率(结果保留三位小数)，并按完播率降序排序

**注**：视频完播率是指完成播放次数占总播放次数的比例。简单起见，结束观看时间与开始播放时间的差>=视频时长时，视为完成播放。

题解

```
select video_id,ROUND(AVG(IF(TIMESTAMPDIFF(SECOND,start_time,end_time)>=duration,1,0)),3) as avg_comp_play_rate
from tb_user_video_log
join tb_video_info USING(video_id)
where YEAR(start_time)=2021
group by video_id
order by avg_comp_play_rate desc;
```

- 关联用户-视频互动记录和短视频信息表：JOIN tb_video_info USING(video_id)；
- 筛选2021年的记录：WHERE YEAR(start_time)=2021
- 按视频id分组：GROUP BY video_id
- 计算每条播放记录是否完成播放：`IF(TIMESTAMPDIFF(SECOND, start_time, end_time)>=duration, 1, 0)`
- 计算完播率，完成播放为1，未完成播放为0，取平均即为完播率AVG()
- 保留3位小数：ROUND(x, 3)

注意：

！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！！

在 SQL 中，`ROUND()` 和 `AVG()` 这两个函数常用于数值计算：

1. **`AVG()`**: 计算一组数值的平均值。
2. **`ROUND()`**: 对数值进行四舍五入到指定的小数位。

比如

```
SELECT ROUND(AVG(price), 2) AS avg_price
FROM sales;
```

求price的平均值，保留两位小数，作为**avg_price**
