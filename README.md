
CREATE TABLE BOOK_MASTER(
BK_ID NUMBER PRIMARY KEY,
BK_NNAME VARCHAR2(40) NOT NULL,
BK_AUTHOR VARCHAR2(50) NOT NULL,
BK_PUBLICATIONDATE DATE,
BK_TYPE VARCHAR2(50) NOT NULL,
BK_PRICE NUMBER NOT NULL,
BK_DESC VARCHAR2(50));

CREATE TABLE LIB_MASTER(
LIB_ID NUMBER PRIMARY KEY NOT NULL,
LIB_NAME VARCHAR2(50) NOT NULL,
LIB_MOBILR NUMBER NOT NULL,
LIB_EMAIL VARCHAR2(50),
LIB_USERNAME VARCHAR2(60) NOT NULL,
LIB_PASSWORD VARCHAR2(50) NOT NULL,
LIB_ADDRESS VARCHAR2(40));

CREATE TABLE MEM_MASTER(

MEM_ID NUMBER PRIMARY KEY NOT NULL,
MEM_NAME VARCHAR2(40) NOT NULL,
MEM_MOBILE NUMBER ,
MEM_EMAIL VARCHAR2(50) NOT NULL,
MEM_USER VARCHAR2(50) NOT NULL,
MEM_PASS VARCHAR2(50) NOT NULL,
MEM_ADDRESS VARCHAR2(100));

SQL> ED
Wrote file afiedt.buf

  1  CREATE TABLE BK_ISS_DLS1(
  2  ISS_ID NUMBER PRIMARY KEY NOT NULL,
  3  BK_ID NUMBER NOT NULL ,
  4  MEM_ID NUMBER NOT NULL,
  5  ISS_DATE DATE  NOT NULL,
  6  ISS_EXPIRY DATE NOT NULL,
  7  ISS_DESC VARCHAR2(50),
  8  FOREIGN KEY(BK_ID)REFERENCES BOOK_MASTER(BK_ID),
  9* FOREIGN KEY(MEM_ID)REFERENCES MEM_MASTER(MEM_ID))

1)insert into book_master values(01,'c','bala guru swamy','02-feb-89','reference',3000,'A Book Co
vers all basics of c and data structures of c');

2) insert into book_master values(02,'python','james','09-jan-1878','subject',3000,'A Book Covers all about python’);

3)insert into book_master values(4,'java','james gosling','23-mar-56','theory',3000,'A Book that describe all about java’);

4)insert into book_master values(5,'Data Structures','ram','23-apr-98','theory',5000,'A Book that describe all about ds’);

5) insert into book_master values(6,'Html','raju','21-may-86','reference',2000,'A Book THat Describe all about html’);

6) insert into book_master values(7,'.Net','Ajay','21-jun-86','reference',2000,'A Book THat Describe all about .net’);

7) insert into book_master values(8,'Artificial intiligence','ravi','21-aug-88','theory',3000,'A book that describe all about 
                                                                                                                                                                           Ai’)
8) insert into book_master values(9,'Deep Learning','jhon','21-sep-78','theory',3000,'A Book that describe all about deep learning’);

9) insert into book_master values(10,'Java Script','rames','21-oct-69','programming',2000,'A Book that describe all about
                       Java script’);
 SQL> select * from book_master;



1)	insert into mem_master values(01,'ajay',7013413075,'ajju1677143@gmail.com','ajju1234','ajay143','23-1182/3 vishnukundi nagar 2nd line 522647');

2)	insert into mem_master values(02,'khadar',7013413785,'khadar@gmail.com','khadar1234','kha143','23-1182/3 vishnukundi nagar 2nd line 522647');


3)	insert into mem_master values(03,'konda',7013413785,'konda@gmail.com','konda1234','kondi143','23-1182/3 vishnukundi nagar 2nd line 522647');

4)	insert into mem_master values(04,'vijay',7013413785,'vijay@gmail.com','vijay1234','vijju143','23-
            1182/3 vishnukundi nagar 2nd line 522647');

5)	insert into mem_master values(05,'kumar',7013413785,'kumar@gmail.com','kumr1234','kumar143','23-1182/3 vishnukundi nagar 2nd line 522647');

select * from mem_master;

1)	insert into lib_master values(05,'neelima',7013413785,'neelima@gmail.com','neelima','neelima','23-1182/3 vishnukundi nagar 522647');

2)	insert into lib_master values(102,'parvathi',7013413075,'para@gmail.com','parvathi','parvathi','34-1149/3  ongole');


insert into lib_master values(103,'prasad',7013413075,'prasad@gmail.com','prasad','prasad',' 34-1149/3 ongole')
/
3)	insert into lib_master values(104,'kiran',7013413075,'kiran@gmail.com','kiran','kiran', '34-1149/3 ongole ');


4)	insert into lib_master values(105,'sreenadh',7013413075,'sreenadh@gmail.com','sreenadh','sreenadh','34-1149/3  ongole');



SQL> select * from lib_master;

1)	insert into bk_iss_dls values(2001,8,3, '12-may-23 ','20-may-23','given by neelima to konda at:3.00pm’);

2)	insert into bk_iss_dls values(2002,5,4, '12-may-23 ','20-may-23','given by parvathi to vijay at:12.00 PM’);


3)	Insert into bk_iss_dls values(2003,4,5, '12-may-23 ','20-may-23','given by prasad to kumar at:2.00pm’);

4)	insert into bk_iss_dls values(2004,3,4, '12-may-23 ','25-may-23','given by kiran to vijay at:2.00pm ‘);

5)	insert into bk_iss_dls values(2005,2,3, '12-may-23 ','25-may-23','given by sreenadh to konda at:2.00Pm’);


SQL> select * from bk_iss_dls;

CREATE TABLE LIB_LOG(
WHO VARCHAR2(30),
ACTION VARCHAR2(40),
WHEN DATE);

SQL> DESC LIB_LOG;
create or replace trigger lib_head
  before insert or update or delete on bk_iss_dls
  declare
  u_action varchar2(30);
   begin
   if inserting then
  u_action := 'insert';
   elsif updating then u_action := 'update';
  elsif deleting then u_action := 'delete';
  else
 raise_application_error(-20001,'you should never ever get this error');
 end if;
 insert into lib_log(who,action,when) values(user,u_action,sysdate);
 end;

Procedure-1
  Q)Write a procedure which will return the list of    books drawn in a day, it must show columns as    member_id, member_name, book_id, book_name

CODE:
CREATE OR REPLACE PROCEDURE PROC_1 (CURRDATE DATE)AS
 CURSOR C1 IS select mem_master.mem_id,mem_master.mem_name,book_master.bk_id,book_master.bk_name from ((bk_iss_dls  join book_master on bk_iss_dls.bk_id=book_master.bk_id) join mem_master on bk_iss_dls.mem_id=mem_master.mem_id) where bk_iss_dls.iss_date=TRUNC(CURRDATE);
 V_REC C1%ROWTYPE;
 BEGIN
 OPEN C1;
 LOOP
 FETCH C1 INTO V_REC;
 EXIT WHEN C1%NOTFOUND;
 DBMS_OUTPUT.PUT_LINE(V_REC.MEM_ID||' '||V_REC.MEM_NAME||' '||V_REC.BK_ID||' '||V_REC.BK_NAME);
 END LOOP;
 CLOSE C1;
EXCEPTION
WHEN NO_DATA_FOUND THEN
DBMS_OUTPUT.PUT_LINE('NO DATA FOUND');
WHEN OTHERS THEN
DBMS_OUTPUT.PUT_LINE('NO BOOKS ARE ISSUED AT TODAY');



Procedure -2:

Q)Write a procedure which will return the list of   members whose books return date is expired, it must show columns as  member_id, member_name, book_id, book_name.
CODE:
CREATE OR REPLACE PROCEDURE PROC_2 (EXPRDATE DATE)AS
 CURSOR C1 IS select mem_master.mem_id,mem_master.mem_name,book_master.bk_id,book_master.bk_name from ((bk_iss_dls  join book_master on bk_iss_dls.bk_id=book_master.bk_id) join mem_master on bk_iss_dls.mem_id=mem_master.mem_id) where bk_iss_dls. ISS_EXPR_DTE  =TRUNC(EXPRDATE);
 V_REC C1%ROWTYPE;
 BEGIN
 OPEN C1;
 LOOP
 FETCH C1 INTO V_REC;
 EXIT WHEN C1%NOTFOUND;
 DBMS_OUTPUT.PUT_LINE(V_REC.MEM_ID||' '||V_REC.MEM_NAME||' '||V_REC.BK_ID||' '||V_REC.BK_NAME);
 END LOOP;
 CLOSE C1;
 END;
/


Procedure -3:
Q)Write a procedure which will do transaction processing when a book is issued to a member details must be stores in the respective tables .

CODE: 

CREATE SEQUENCE SEQ_ISSID
  START WITH 2025
 INCREMENT BY 1
  MINVALUE 0
  MAXVALUE 100000;

Sequence created.

SQL> ED
Wrote file afiedt.buf

   CREATE OR REPLACE PROCEDURE PROC_3 (
    BOOKID NUMBER ,MEMID NUMBER,
  ISSDATE DATE, EXPRDATE DATE,
    ISSDESC VARCHAR2) IS
   BEGIN
    INSERT INTO BK_ISS_DLS VALUES(SEQ_ISSID.NEXTVAL,BOOKID,MEMID,ISSDATE,EXPRDATE,ISSDESC);
    END;
 /

PROCEDURE-4:
Q)Write a procedure which will Add transaction of a new  book in the database   

CODE:
CREATE SEQUENCE SEQ_BKID
    START WITH 13
    INCREMENT BY 1
    MINVALUE 0
   MAXVALUE 100000


SQL> ED
Wrote file afiedt.buf

 CREATE OR REPLACE PROCEDURE PROC_4 (
 BKNAME VARCHAR2 , BKAUTHOR VARCHAR2 ,
 BKPUBDATE DATE , BKTYPE VARCHAR2 , BKPRICE NUMBER,
 BKDESC VARCHAR2) IS
 BEGIN
 INSERT INTO BOOK_MASTER VALUES(SEQ_BKID.NEXTVAL,BKNAME,BKAUTHOR,BKPUBDATE,BKTYPE,BKPRICE,BKDESC
     END;
/

Procedure created.

SQL> EXEC PROC_4('SQL','CHARNDRA','23-MAY-96','VOLUME 2',3000,'A BOOK PUBLISHED FOR ALL THE STANDARD
S');


Q)Write a procedure which will Add transaction of a new member in the database

CODE:

 CREATE OR REPLACE PROCEDURE PROC_5(
  MEMNAME VARCHAR2 , MEMMOBIL NUMBER , MEMEMAIL VARCHAR2 ,
  MEMUSER VARCHAR2 , MEMPASS VARCHAR2 , MEMADRS VARCHAR2) IS
  BEGIN
 INSERT INTO MEM_MASTER VALUES(SEQ_MEMID.NEXTVAL,MEMNAME,MEMMOBIL,MEMEMAIL,MEMUSER,MEMPASS,
      END;
 /

SQL> EXEC PROC_5('PUSHPA',6547893214,'PUSHPA@GMAIL.COM','PUSHPA','PUSHPA@123','23-1182/3 ONGOLE 5230
01');
