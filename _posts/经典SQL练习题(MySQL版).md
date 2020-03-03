---
title: 经典SQL练习题(MySQL版)
date: 2020-03-03 21:33:20
author: fuf
notebook: blog
evernote-version: 0
source: 原创/转载
thumbnail: 
tags:
    - 默认
blogexcerpt:
---

<!-- more -->

 


# 经典SQL练习题(MySQL版)
> 当SELECT 后 既有 表结构本身的字段，又有需要使用聚合函数（COUNT(),SUM(),MAX(),MIN(),AVG()等）的字段，就要用到group by分组，查询的限定条件里有需要用聚合函数计算的字段时也需要用分组
> select  count(*) from Teacher  where Teacher.Tname  like '李%';
<!-- more -->

## 四张表
1.  学生表

Student(SId,Sname,Sage,Ssex)

--SId 学生编号,Sname 学生姓名,Sage 出生年月,Ssex 学生性别

2. 课程表

Course(CId,Cname,TId)

--CId 课程编号,Cname 课程名称,TId 教师编号

3. 教师表

Teacher(TId,Tname)

--TId 教师编号,Tname 教师姓名

4. 成绩表

SC(SId,CId,score)

--SId 学生编号,CId 课程编号,score 分数

# 在网上看到许多的 SQL 练习题，今天复习复习，练练手。
## 0. 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数

其实就是通过SC表得到2张子表，笛卡儿积的方法合并两张表，然后通过WHERE条件进行筛选


## 0.1 查询同时存在" 01 "课程和" 02 "课程的情况
首先明确得到的信息是从那张表(SC)得到。
```
 select * from (select * from SC where SC.CId = '01') AS A , (SELECT * FROM SC WHERE SC.CId='02') AS B  ON A.SId = B.SId;
```

## 0.2 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )
```
select * from (select * from SC where SC.CId='01') AS A LEFT JOIN (SELECT * FROM SC WHERE SC.CId='02') AS B ON A.SId = B.SId;
```

## 0.3 查询不存在" 01 "课程但存在" 02 "课程的情况

```
select * from (select * from SC where SC.CId='01') AS A LEFT JOIN (SELECT * FROM SC WHERE SC.CId='02') AS B ON A.SId = B.SId WHERE B.SId IS NULL;
```

## 总结
1. SELECT,使用 AS 重命名表。
2. WHERE，AND 多重条件。
3. LEFT JOIN ....ON 的使用。
4. 使用 逗号连接两表，是 inner JOIN。
5. WHERE ,NOT IN 条件。


## 1. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩
```
select stu.SId,any_value(stu.Sname) ,avg(sc.score)
from Student stu  inner join SC sc on stu.SId = sc.SId
group by stu.SId
having avg(sc.score)>60;
```

## 2. 查询在 SC 表存在成绩的学生信息
```
select a.SId,a.Sname,a.Sage,a.Ssex from Student a join (select distinct  SId from SC where score <>0) b on a.SId =b.SId ;
```


##  3. 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )
select DISTINCT Student.* from Student  JOIN SC ON Student.SId=SC.SId;

## 4.1 查有成绩的学生信息
```
select * from Student  where exists(select * from SC where Student.SId=SC.SId);
```

5. 查询「李」姓老师的数量
```
select  count(*) from Teacher  where Teacher.Tname  like '李%';
```
6. 查询学过「张三」老师授课的同学的信息
```
SELECT Student.* FROM Student,SC,Teacher,Course
where Teacher.Tname = '张三'
and Teacher.TId = Course.TId
and Course.CId = SC.CId
and Student.SId = SC.SId;
```


7. 查询没有学全所有课程的同学的信息
我们先查询所有学过完整课程的学生，然后我们要没有学完的学生 就是 not in

```
SELECT Student.* from Student WHERE Student.SId NOT IN(
    SELECT SId FROM SC GROUP BY SId HAVING COUNT(CId) = (SELECT COUNT(*) FROM Course)
    );


```

8. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息


9. 查询和" 01 "号的同学学习的课程 完全相同的其他同学的信息



10. 查询没学过"张三"老师讲授的任一门课程的学生姓名
```
select * from Student where Student.SId not in(SELECT  Student.SId  FROM Student LEFT JOIN SC ON Student.SId=SC.SId WHERE EXISTS(
    SELECT * FROM Teacher,Course WHERE Teacher.Tname='张三'
    and Teacher.TId = Course.TId
    and Course.CId = SC.CId
    ));

```


11. 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩

12. 检索" 01 "课程分数小于 60，按分数降序排列的学生信息

13. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

14. 查询各科成绩最高分、最低分和平均分：

以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率

及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90

要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

15. 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺

15.1 按各科成绩进行排序，并显示排名， Score 重复时合并名次

16. 查询学生的总成绩，并进行排名，总分重复时保留名次空缺

16.1 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺

17. 统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比

18. 查询各科成绩前三名的记录

19. 查询每门课程被选修的学生数

20. 查询出只选修两门课程的学生学号和姓名

21. 查询男生、女生人数

22. 查询名字中含有「风」字的学生信息

23. 查询同名同性学生名单，并统计同名人数

24. 查询 1990 年出生的学生名单

25. 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列

26. 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩

27. 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数

28. 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）

29. 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数

30. 查询不及格的课程

31. 查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名

32. 求每门课程的学生人数

33. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

34. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

35. 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩

36. 查询每门功成绩最好的前两名

37. 统计每门课程的学生选修人数（超过 5 人的课程才统计）。

38. 检索至少选修两门课程的学生学号

39. 查询选修了全部课程的学生信息

40. 查询各学生的年龄，只按年份来算

41. 按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一

42. 查询本周过生日的学生

43. 查询下周过生日的学生

44. 查询本月过生日的学生

45. 查询下月过生日的学生




