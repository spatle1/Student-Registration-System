drop table grades;
drop table enrollments;
drop table classes;
drop table course_credit;
drop table courses;
drop table students;

create table students (sid char(4) primary key check (sid like 'B%'),
firstname varchar2(15) not null, lastname varchar2(15) not null, status varchar2(10) 
check (status in ('freshman', 'sophomore', 'junior', 'senior', 'graduate')), 
gpa number(3,2) check (gpa between 0 and 4.0), email varchar2(20) unique);

create table courses (dept_code varchar2(4) not null, course# number(3) not null
check (course# between 100 and 799), title varchar2(20) not null,
primary key (dept_code, course#));

create table course_credit (course# number(3) not null primary key
check (course# between 100 and 799), credits number(1) check (credits in (3, 4)),
check ((course# < 500 and credits = 4) or (course# >= 500 and credits = 3)));

create table classes (classid char(5) primary key check (classid like 'c%'), 
dept_code varchar2(4) not null, course# number(3) not null, 
sect# number(2), year number(4), semester varchar2(6) 
check (semester in ('Spring', 'Fall', 'Summer')), limit number(3), 
class_size number(3), foreign key (dept_code, course#) references courses
on delete cascade, unique(dept_code, course#, sect#, year, semester),
check (class_size <= limit));

create table enrollments (sid char(4) references students, classid char(5) references classes, 
lgrade char check (lgrade in ('A', 'B', 'C', 'D', 'F', 'I', null)), primary key (sid, classid));

create table grades(lgrade char check (lgrade in ('A', 'B', 'C', 'D', 'F', 'I', null)),
ngrade number(1) check (ngrade in (4, 3, 2, 1, 0, null)),
check ((lgrade = 'A' and ngrade = 4) or (lgrade = 'B' and ngrade = 3) or  
(lgrade = 'C' and ngrade = 2) or (lgrade = 'D' and ngrade = 1) or  
(lgrade = 'F' and ngrade = 0) or  (lgrade = 'I' and ngrade = null) or  
(lgrade = null and ngrade = null)));

insert into students values ('B001', 'Anne', 'Broder', 'junior', 3.6, 'broder@bu.edu');
insert into students values ('B002', 'Terry', 'Buttler', 'senior', 3.7, 'buttler@bu.edu');
insert into students values ('B003', 'Tracy', 'Wang', 'senior', 4.0, 'wang@bu.edu');
insert into students values ('B004', 'Barbara', 'Callan', 'junior', 2.5, 'callan@bu.edu');
insert into students values ('B005', 'Jack', 'Smith', 'graduate', 3.0, 'smith@bu.edu');
insert into students values ('B006', 'Terry', 'Zillman', 'graduate', 4.0, 'zillman@bu.edu');
insert into students values ('B007', 'Becky', 'Lee', 'senior', 4.0, 'lee@bu.edu');
insert into students values ('B008', 'Tom', 'Baker', 'freshman', null, 'baker@bu.edu');

insert into courses values ('CS', 432, 'database systems');
insert into courses values ('Math', 314, 'discrete math');
insert into courses values ('CS', 240, 'data structure');
insert into courses values ('Math', 221, 'calculus I');
insert into courses values ('CS', 532, 'database systems');
insert into courses values ('CS', 552, 'operating systems');
insert into courses values ('BIOL', 425, 'molecular biology');

insert into course_credit values (432, 4);
insert into course_credit values (314, 4);
insert into course_credit values (240, 4);
insert into course_credit values (221, 4);
insert into course_credit values (532, 3);
insert into course_credit values (552, 3);
insert into course_credit values (425, 4);

insert into classes values  ('c0001', 'CS', 432, 1, 2022, 'Spring', 35, 34);
insert into classes values  ('c0002', 'Math', 314, 1, 2022, 'Spring', 25, 24);
insert into classes values  ('c0003', 'Math', 314, 2, 2022, 'Spring', 25, 22);
insert into classes values  ('c0004', 'CS', 432, 1, 2021, 'Spring', 30, 30);
insert into classes values  ('c0005', 'CS', 240, 1, 2021, 'Fall', 40, 39);
insert into classes values  ('c0006', 'CS', 532, 1, 2021, 'Spring', 29, 28);
insert into classes values  ('c0007', 'Math', 221, 1, 2022, 'Spring', 30, 30);

insert into enrollments values  ('B001', 'c0001', 'A');
insert into enrollments values  ('B002', 'c0002', 'B');
insert into enrollments values  ('B003', 'c0004', 'A');
insert into enrollments values  ('B004', 'c0004', 'C');
insert into enrollments values  ('B004', 'c0005', 'B');
insert into enrollments values  ('B005', 'c0006', 'B');
insert into enrollments values  ('B006', 'c0006', 'A');
insert into enrollments values  ('B001', 'c0002', 'C');
insert into enrollments values  ('B003', 'c0005', null);
insert into enrollments values  ('B007', 'c0007', 'A');
insert into enrollments values  ('B001', 'c0004', 'B');
insert into enrollments values  ('B001', 'c0006', 'B');
insert into enrollments values  ('B002', 'c0004', 'A');
insert into enrollments values  ('B001', 'c0005', 'B');

insert into grades values  ('A', 4);
insert into grades values  ('B', 3);
insert into grades values  ('C', 2);
insert into grades values  ('D', 1);
insert into grades values  ('F', 0);
insert into grades values  ('I', null);
