# Sql scripts 

### having vs where
- “Where” 是一个约束声明，使用Where来约束来之数据库的数据，Where是在结果返回之前起作用的，且Where中不能使用聚合函数。
- “Having”是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在Having中可以使用聚合函数。


#### list out all classes which have more than or equal to 5 students. [Leetcode596](https://leetcode.com/problems/classes-more-than-5-students/)

| student | class      |
|---------|------------|
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |

```sql
select class from courses group by class having count(distinct courses.student) >= 5
```

#### find all duplicate emails in a table. [LeetCode182](https://leetcode.com/problems/duplicate-emails/).


| Id | Email   |
| ---- | ------| 
| 1  | a@b.com |
| 2  | c@d.com |
| 3  | a@b.com |


```sql
select email from Person group by email having count(*) > 1
```

### join
- left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录 
- right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录
- inner join(等值连接) 只返回两个表中联结字段相等的行

#### combine tables. [LeetCode175](https://leetcode.com/problems/combine-two-tables/).

| Column Name | Type    |
|-------------|---------|
| PersonId    | int     |
| FirstName   | varchar |
| LastName    | varchar |
PersonId is the primary key column for this table.

| Column Name | Type    |
|-------------|---------|
| AddressId   | int     |
| PersonId    | int     |
| City        | varchar |
| State       | varchar |
AddressId is the primary key column for this table.

Provides the following information for each person in the Person table, regardless if there is an address for each of those people:

```sql
select FirstName, LastName, City, State from Person left join  Address on (Person.PersonID = Address.PersonID) 
```

### delete 

#### delete duplicate emails. [LeetCode196](https://leetcode.com/problems/delete-duplicate-emails/).

| Id | Email            |
|----|------------------|
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |


```sql
delete from Person where Id not in (select * from (select min(p1.Id) from Person p1 group by p1.Email) as tmp)

```