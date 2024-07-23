# leetcodePremiumQuestions
This repo only consists of the premium questions in SQL from Leetcode.
Q1)

/*

Leetcode premium SQL Question Level: Medium.

Day 29 of Solving SQL Problem 87. Q) Flight Occupancy and Waitlist Analysis.

This is similar question to 85 question with twist.

There is a test case in this question where you need to find if any flight have no bookings then the count need to be 0.

--->> Passengers book tickets for flights in advance. If a passenger books a ticket for a flight and there are still empty seats available on the flight, the passenger ticket 

will be confirmed. However, the passenger will be on a waitlist if the flight is already at full capacity.

<<---

*/
The Schema:

Create table if not exists Flights(flight_id int,capacity int);

Create table if not exists Passengers (passenger_id int,flight_id int);

insert into Flights (flight_id, capacity) values ('1', '2');

insert into Flights (flight_id, capacity) values ('2', '2');

insert into Flights (flight_id, capacity) values ('3', '1');

insert into Passengers (passenger_id, flight_id) values ('101', '1');

insert into Passengers (passenger_id, flight_id) values ('102', '1');

insert into Passengers (passenger_id, flight_id) values ('103', '1');

insert into Passengers (passenger_id, flight_id) values ('104', '2');

insert into Passengers (passenger_id, flight_id) values ('105', '2');

insert into Passengers (passenger_id, flight_id) values ('106', '3');

insert into Passengers (passenger_id, flight_id) values ('107', '3');

Solution:

with cte as
(
  select distinct count(p.passenger_id) as total_p_count, f.flight_id,f.capacity  
from Flights f
  join Passengers p
  on f.flight_id = p.flight_id
  group by f.flight_id,f.capacity
)
select (case when capacity<=total_p_count then capacity end) as booked_count,
(total_p_count-capacity) as waitlist_count from cte

Q2)

Leetcode premium SQL Question Level: Medium.
Day 29 of Solving SQL Problem 88. Q) Election Results.

*The election is conducted in a city where everyone can vote for one or more candidates or choose not to vote. Each person has 1 vote so if they vote for multiple candidates, their vote gets equally split across them. For example, if a person votes for 2 candidates, these candidates receive an equivalent of 0.5 votes each.

*Write a solution to find candidate who got the most votes and won the election. Output the name of the candidate or If multiple candidates have an equal number of votes, display the names of all of them.

*Return the result table ordered by candidate in ascending order.

Schema:

Create table if not exists Votes(voter varchar(30), candidate varchar(30));

insert into Votes (voter, candidate) values ('Kathy', 'None');

insert into Votes (voter, candidate) values ('Charles', 'Ryan');

insert into Votes (voter, candidate) values ('Charles', 'Christine');

insert into Votes (voter, candidate) values ('Charles', 'Kathy');

insert into Votes (voter, candidate) values ('Benjamin', 'Christine');

insert into Votes (voter, candidate) values ('Anthony', 'Ryan');

insert into Votes (voter, candidate) values ('Edward', 'Ryan');

insert into Votes (voter, candidate) values ('Terry', 'None');

insert into Votes (voter, candidate) values ('Evelyn', 'Kathy');

insert into Votes (voter, candidate) values ('Arthur', 'Christine');

Solution:

WITH CTE AS(
SELECT voter, ROUND(1.0/(COUNT(*)*1.0),1) AS votecount
FROM votes
GROUP BY voter),

CTE2 AS(
SELECT candidate , SUM(votecount) AS finalResult
FROM votes V
LEFT JOIN CTE C ON
V.voter = C.voter
WHERE candidate is not null
GROUP BY candidate
)
select candidate from CTE2
where finalResult = (select max(finalResult) from CTE2)
order by candidate 

Q3)

Interview SQL Question Level: Easy
Company: Capegemini.
Find the number of Languages in skills column. And return Id, skills, total_count.

Schema:

CREATE TABLE skills (
 id INT,
 skills VARCHAR(255)
);

INSERT INTO skills (id, skills) VALUES
(1, 'Sql, Plsql, Oracle'),
(2, 'Database, Mysql');

Solution:

select id,skills,length(skills)-length(replace(skills,',',''))+1 from skills

Q4)

Interview SQL Question Level: Unique.
***Must Try
Find no of Vacant positions available based on totalPost.

Schema:

create table job_positions
(
 id int,
 title varchar(100),
 group_s varchar(10),
 level_s varchar(10),
 payscale int,
 totalpost int
);
insert into job_positions values (1, 'General manager', 'A', 'l-15', 10000, 1);
insert into job_positions values (2, 'Manager', 'B', 'l-14', 9000, 5);
insert into job_positions values (3, 'Asst. Manager', 'C', 'l-13', 8000, 10);

drop table if exists job_employees;
create table job_employees
(
 id int,
 name varchar(100),
 position_id int
);
insert into job_employees values (1, 'John Smith', 1);
insert into job_employees values (2, 'Jane Doe', 2);
insert into job_employees values (3, 'Michael Brown', 2);
insert into job_employees values (4, 'Emily Johnson', 2);
insert into job_employees values (5, 'William Lee', 3);
insert into job_employees values (6, 'Jessica Clark', 3);
insert into job_employees values (7, 'Christopher Harris', 3);
insert into job_employees values (8, 'Olivia Wilson', 3);
insert into job_employees values (9, 'Daniel Martinez', 3);
insert into job_employees values (10, 'Sophia Miller', 3);

Solution:


Q5) write a query to find out callers whose first and last call was to the same person on a given day
Schema:

create table phonelog(
    callerid int, 
    recipientid int,
    datecalled datetime
);

insert into phonelog(callerid, recipientid, datecalled)
values(1, 2, '2019-01-01 09:00:00.000'),
       (1, 3, '2019-01-01 17:00:00.000'),
       (1, 4, '2019-01-01 23:00:00.000'),
       (2, 5, '2019-07-05 09:00:00.000'),
       (2, 3, '2019-07-05 17:00:00.000'),
       (2, 3, '2019-07-05 17:20:00.000'),
       (2, 5, '2019-07-05 23:00:00.000'),
       (2, 3, '2019-08-01 09:00:00.000'),
       (2, 3, '2019-08-01 17:00:00.000'),
       (2, 5, '2019-08-01 19:30:00.000'),
       (2, 4, '2019-08-02 09:00:00.000'),
       (2, 5, '2019-08-02 10:00:00.000'),
       (2, 5, '2019-08-02 10:45:00.000'),
       (2, 4, '2019-08-02 11:00:00.000');


Solution:
/*
write a query to find out callers whose first and last call was to the same person on a given day
*/
with cte as
(
select callerid,cast(datecalled as date) as called_date,min(datecalled) as first_call,max(datecalled) as last_call
from phonelog
group by callerid,cast(datecalled as date)
)
select c.*,p1.recipientid
from cte c
inner join phonelog p1
on p1.callerid = c.callerid and c.first_call = p1.datecalled
inner join phonelog p2
on p2.callerid = c.callerid and c.last_call = p2.datecalled
where p1.recipientid = p2.recipientid

Q6)




 










