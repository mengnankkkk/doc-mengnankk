---
title: Mysql练习 寒假篇
categories:
  - sql
  
abbrlink: 2701
date: 2025.1.10
tags: 
   - mysql 

---

# 基础语法回顾

## 1.[**SQL110** **插入记录（一）**](https://www.nowcoder.com/practice/5d2a42bfaa134479afb9fffd9eee970c?tpId=240&tqId=2221797&ru=/exam/oj&qru=/ta/sql-advanced/question-ranking&sourceUrl=%2Fexam%2Foj%3FquestionJobId%3D10%26subTabName%3Donline_coding_page)

牛客后台会记录每个用户的试卷作答记录到exam_record表，现在有两个用户的作答记录详情如下：

- 用户1001在2021年9月1日晚上10点11分12秒开始作答试卷9001，并在50分钟后提交，得了90分；
- 用户1002在2021年9月4日上午7点1分2秒开始作答试卷9002，并在10分钟后退出了平台。

试卷作答记录表exam_record中，表已建好，其结构如下，请用一条语句将这两条记录插入表中。

| Filed       | Type       | Null | Key  | Extra          | Default | Comment  |
| ----------- | ---------- | ---- | ---- | -------------- | ------- | -------- |
| id          | int(11)    | NO   | PRI  | auto_increment | (NULL)  | 自增ID   |
| uid         | int(11)    | NO   |      |                | (NULL)  | 用户ID   |
| exam_id     | int(11)    | NO   |      |                | (NULL)  | 试卷ID   |
| start_time  | datetime   | NO   |      |                | (NULL)  | 开始时间 |
| submit_time | datetime   | YES  |      |                | (NULL)  | 提交时间 |
| score       | tinyint(4) | YES  |      |                | (NULL)  | 得分     |

该题最后会通过执行SELECT uid, exam_id, start_time, submit_time, score FROM exam_record;来对比结果

题解：

```
insert into exam_record(uid,exam_id,start_time,submit_time,score) 
VALUES (1001,9001,'2021-09-01 22:11:12','2021-09-01 23:01:12' ,90),
(1002,9002,'2021-09-04 07:01:02',null,NULL);

```

按照顺序插入即可，注意一一对应

2.

牛客的运营同学想要查看大家在SQL类别中高难度试卷的得分情况。

请你帮她从exam_record数据表中计算所有用户完成SQL类别高难度试卷得分的截断平均值（去掉一个最大值和一个最小值后的平均值）。

示例数据：examination_info（exam_id试卷ID, tag试卷类别, difficulty试卷难度, duration考试时长, release_time发布时间）

| id   | exam_id | tag  | difficulty | duration | release_time        |
| ---- | ------- | ---- | ---------- | -------- | ------------------- |
| 1    | 9001    | SQL  | hard       | 60       | 2020-01-01 10:00:00 |
| 2    | 9002    | 算法 | medium     | 80       | 2020-08-02 10:00:00 |

```
示例数据：exam_record（uid用户ID, exam_id试卷ID, start_time开始作答时间, submit_time交卷时间, score得分）iduidexam_idstart_timesubmit_time
score1100190012020-01-02 09:01:012020-01-02 09:21:01
8021001
9001
2021-05-02 10:01:012021-05-02 10:30:01
81310019001
2021-06-02 19:01:01
2021-06-02 19:31:01
84
410019002
2021-09-05 19:01:01
2021-09-05 19:40:0189
51001
90012021-09-02 12:01:01
(NULL)
(NULL)
61001
9002
2021-09-01 12:01:01
(NULL)
(NULL)
710029002
2021-02-02 19:01:01
2021-02-02 19:30:01
87810029001
2021-05-05 18:01:01
2021-05-05 18:59:02909
10039001
2021-09-07 12:01:01
2021-09-07 10:31:01
501010049001
2021-09-06 10:01:01
(NULL)
(NULL)
```

根据输入你的查询结果如下：



题解：

```
SELECT tag, difficulty,
    ROUND((SUM(score) - MAX(score) - MIN(score)) / (COUNT(score) - 2), 1) AS clip_avg_score
FROM exam_record
JOIN examination_info USING(exam_id)
WHERE tag = 'SQL' AND difficulty = 'hard'
GROUP BY tag, difficulty;

```

根据sql查询的分别出现的列，然后来根据这些列来写

主要是

```
ROUND((SUM(score) - MAX(score) - MIN(score)) / (COUNT(score) - 2), 1) AS clip_avg_score
```

这个保留一位小数，计算平均数

然后使用联合查询

将examination_info和exam_record连接起来，其主键是exam_id

然后where条件

最后可以来个按组排序

