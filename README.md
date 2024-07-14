# leetcodePremiumQuestions
This repo only consists of the premium questions in SQL from Leetcode.
Q1)
/*
Leetcode premium SQL Question Level: Medium.
Day 29 of Solving SQL Problem 87. Q) Flight Occupancy and Waitlist Analysis.
This is similar question to 85 question with twist.
There is a test case in this question where you need to find if any flight have no bookings then the count need to be 0.
--->> Passengers book tickets for flights in advance. If a passenger books a ticket for a flight and there are still empty seats available on the flight, the passenger ticket will be confirmed. However, the passenger will be on a waitlist if the flight is already at full capacity.
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








