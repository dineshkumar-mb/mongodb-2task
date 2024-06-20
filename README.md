MongoDB Task

Design database for Zen class programme
users
codekata
attendance
topics
tasks
company_drives
mentors

Find all the topics and tasks which are thought in the month of October
Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020
Find all the company drives and students who are appeared for the placement.
Find the number of problems solved by the user in codekata
Find all the mentors with who has the mentee's count more than 15
Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020

```Answers```

Find all the topics and tasks which are thought in the month of October

db.topics.find({
... "date":{
... $gte: ISODate("2020-10-01"),
... $lt:ISODate("2020-10-31")
... }
... })

db.tasks.find({
... "date":{
... $gte: ISODate("2020-10-01"),
... $lt: ISODate("2020-10-31")
... }
... })

Find all the company drives which appeared between 15 oct-2020 and 31-oct-2020

db.company_drives.find({
... "interview_date": {
... $gte: ISODate("2020-10-15"),
... $lt: ISODate("2020-10-31")
... }
... })

Find all the company drives and students who are appeared for the placement.

db.company_drives.find({},{
... "_id":0,
... "company_name":1,
... "student_id_list":1
... })

Find the number of problems solved by the user in codekata

db.users.aggregate({
... $project: {
... _id: 0,
... user_name: 1,
... no_of_prob_solved: {
... $size: "$codekata_solved"
... }
... }
... })

Find all the mentors with who has the mentee's count more than 15

db.mentors.find({
... $where: "this.student_id_list.length > 15"
... },
... {
... _id: 0,
... mentor_name: 1
... })

Find the number of users who are absent and task is not submitted  between 15 oct-2020 and 31-oct-2020

db.attendance.aggregate({
... $match: { 
... "date": {
... $gte: ISODate("2020-10-15"),
... $lt: ISODate("2020-10-31")
... } 
... }
... },{
... $project: {
... _id: 0,
... date: 1,
... no_of_absent: {$size: "$absent"}
... }
... })

db.tasks.aggregate({
... $match: {
... "date": {
... $gte: ISODate("2020-10-15"),
... $lt: ISODate("2020-10-31")
... }
... }
... },{
... $project: {
... _id: 0,
... task_name:1,
... date: 1,
... no_of_not_submitted: {$size: "$not_submitted"}
... }
... })
