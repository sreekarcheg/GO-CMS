#Course Management Portal

Designed and built efficient back-end for a course-management system as part of CS3010: Database Theory, IIT 2016.

##ER diagram
(https://github.com/sreekarcheg/GO-CMS/blob/master/ER-diag.png)

##Tables

###Accounts
Stores details of all users  

`AccountID` int primary key auto increment  
`FirstName` varchar(32) not null  
`LastName` varchar(32) not null  
`Email` varchar(64) not null  
`RollNo` varchar(32)  
`PrivilegeLevel` int not null < 0 - student, 1 - teacher, 2 - dean  
`ActivationKey` char(64) < for account confirmation through email, null indicates already confirmed

###Courses
Stores details of courses  
  
`CourseName` varchar(32) not null  
`CourseCode` varchar(16) not null  
`CourseCreatorID` int not null foreign key references AccountID  
`CourseStartDate` date not null  
`CourseEndDate` date not null  
`MaxGraceDays` int not null  

###Roles
Stores the role of an account for a course. An account may have different roles for different courses.  

`AccountID` int not null foreign key references AccountID  
`CourseID` int not null foreign key references CourseID  
`Role` int not null < 0 - student, 1 - ta, 2 - teacher  

###Assignments and Announcements  
Stores details of assignments and announcements. Combined since they have similar structure, and an announcement is basically an assignment without any submissions required.  
`AssignmentID` int primary key auto increment  
`CourseID` int not null foreign key references CourseID  
`CreationTime` datetime not null  
`CreatorID` int not null foreign key references AccountID  
`DueTime` datetime < NULL in case of announcement  
`MaxSubmissionTime` datetime < NULL in case of announcement or no restriction  
`TitleString` text not null  
`SubmissionType` varchar(4) not null < ‘file’ or ‘text’ or ‘none’ in case of announcement  
`MaximumMarks` int not null = 0 in case of announcement  

###Grades  
Stores grades of students for each assignment. Also stores no. of grace days used.  

`SubmittorID` int not null foreign key references AccountID  
`AssignmentID` int not null foreign key references AssignmentID  
`GivenGrade` int < NULL indicates not graded  
`GraceDaysUsed` int not null  

###Files
Stores details of all files, including submissions as well as question files  

`FileID` int primary key auto increment  
`Hash` char(64) not null < for quick storage and retrieval of file from filesystem  
`FileName` varchar(64) not null  
`Submission Files`  
`Stores` details of student submissions for assignments  
`FileID` int not null foreign key references FileID  
`SubmittorID` int not null foreign key references AccountsID  
`AssignmentID` int not null foreign key references AssignmentID  
`SubmissionTime` datetime not null  

###Question Files
Stores details of files from the instructor that are part of an assignment specification  

`FileID` int not null foreign key references FileID  
`AssignmentID` int not null foreign key references AssignmentID  
`Comments`  
`Stores` user comments  
`CommentString` text not null  
`CommentorID` not null foreign key references AccountID  
`AssignmentID` not null foreign key references AssignmentID  
`CommentTime` datetime not null  
`Submission` Strings  
`Stores` answer strings  
`SubmissionString` text not null  
`SubmittorID` int not null foreign key references AccountID  
`AssignmentID` int not null foreign key references AssignmentID  
`Submission Time` datetime not null  

###Grace Days  
`Stores` no. of grace days remaining for each student for each course.  
`CourseID` int foreign key not null references CourseID  
`StudentID` int not null foreign key references AccountID   
`GraceDaysRemaining` int not null  

##Supported Operations    

###Account Operations  

* Creation of Account
* Email verification of Account
* Deletion of Account
* Elevating student accounts to dean or instructor level
* Creation of Course
* Deletion of Course

###Course Operations
* Commenting on an Assignment or Announcement
* Editing previous comments
* Deleting comments
* Adding other Instructors, TAs, and Students
* Removing other Instructors, TAs, and Students
* Seeing student grades (weighted average of marks in all assignments, out of 100)
* Adding Announcements or Assignments
* Modifying Announcements or Assignments
* Removing Announcements or Assignments
* Accessing Student Submissions
* Grading Submissions
* Modifying Grades
* Adding a submission
* Modifying a submission (upto last submit date, if not graded)
* Removing a submission (upto last submit date, if not graded)

###Common
* Creation of Account
* Email verification of Account
* Deletion of Account

###Dean Only
* Elevating student accounts to dean or instructor level

###Instructor Only
* Creation of Course
* Deletion of Course

##Course Operations

###Common
* Commenting on an Assignment or Announcement

###Instructor Only
* Adding other Instructors, TAs, and Students
* Removing other Instructors, TAs, and Students
* Seeing student grades (weighted average of marks in all assignments, out of 100)

###Instructor or TA
* Adding Announcements or Assignments
* Modifying Announcements or Assignments
* Removing Announcements or Assignments
* Accessing Student Submissions
* Grading Submissions
* Modifying Grades

###Student Only
* Adding a submission
* Modifying a submission (upto last submit date, if not graded)
* Removing a submission (upto last submit date, if not graded)
* Adding Announcements

##Contributors
* SREE RAM SREEKAR cs13b1008@iith.ac.in
* Akshita Mittel cs13b1040@iith.ac.in
* Keyur Joshi
* Arjun V Anand
