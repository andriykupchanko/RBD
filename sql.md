# RBD
# desktop-tutorial
GitHub Desktop tutorial repository
/*LR 2 ORBD*/
CREATE DATABASE "OREND AUTO"; 
CREATE TABLE card (
  "id_card" int PRIMARY KEY,
  "num_card" int,
  "date_limit" varchar,
  "cvc" int
);

CREATE TABLE inspector (
  "id_inspector" int PRIMARY KEY,
  "name" varchar,
  "sure_name" varchar,
  "last_name" varchar,
  "birsday" date,
  "num_phone" varchar,
  "id_card_recvisit" int,
  FOREIGN KEY (id_card_recvisit) REFERENCES card(id_card) ON DELETE CASCADE
);

CREATE TABLE problem (
  "id_problem" int PRIMARY KEY,
  "type_problems" varchar,
  "price_problems" int
);

CREATE TABLE inspection (
  "id_inspection" int   PRIMARY KEY ,
  "id_insp" int,
  "id_problems" int,
  FOREIGN KEY (id_problems) REFERENCES problem (id_problem),
  FOREIGN KEY (id_insp) REFERENCES inspector (id_inspector) 
);

CREATE TABLE fine_list (
  "id_type_fine" int PRIMARY KEY,
  "type_fine" varchar,
  "price_fine" int
);

CREATE TABLE fine (
  "id_fine" int PRIMARY KEY ,
  "id_type_fine" int,
    FOREIGN KEY (id_type_fine) REFERENCES fine_list (id_type_fine) 
);

CREATE TABLE sale (
  "id_sale" int PRIMARY KEY,
  "type_sale" varchar  (100) NOT NULL,
  "procent_sale" int NOT NULL
);

CREATE TABLE belay (
  "id_belay" int PRIMARY KEY,
  "tapy_belay" varchar (100)  NOT NULL,
  "price_belay_one_years" int NOT NULL,
  "insurance_refund" int NOT NULL
);

CREATE TABLE user_orend (
  "id_user" serial PRIMARY KEY,
  "name" varchar (50)  UNIQUE CHECK(name !=''),
  "sure_name" varchar (50)   NOT NULL,
  "last_name" varchar (50)  NOT NULL,
  "birsday" varchar NOT NULL,
  "num_phone" varchar NOT NULL,
  "num_doc_cars" varchar NOT NULL,
  "id_card_recvisit" int,
  "black_list" bool DEFAULT false,
	FOREIGN KEY (id_card_recvisit) REFERENCES card (id_card) ON DELETE CASCADE
);
CREATE TABLE price_one_day 
(
  "id_price_one_day" int PRIMARY KEY,
  "price_one_day" int
);

CREATE TABLE car (
  "id_car" int PRIMARY KEY,
  "brand" varchar,
  "model" varchar,
  "drive" int,
  "race" int,
  "fuels" int,
  "year" date,
  "id_price_one_day" int,
  "num_belay" int ,
  "num_inspection" int,
  FOREIGN KEY (num_belay) REFERENCES belay (id_belay),
  FOREIGN KEY (num_inspection) REFERENCES inspection (id_inspection) ,
  FOREIGN KEY (id_price_one_day) REFERENCES price_one_day (id_price_one_day) 
);



CREATE TABLE price
(
  "id_price" int PRIMARY KEY ,
  "price_one_day" int,
  "count_day" int,
  "sale" int,
  "fine" int,
  FOREIGN KEY (sale) REFERENCES sale (id_sale),
  FOREIGN KEY (fine) REFERENCES fine (id_fine),
  FOREIGN KEY (price_one_day) REFERENCES price_one_day (id_price_one_day)
);

CREATE TABLE order_or (
  "id_order" int  PRIMARY KEY ,
  "id_user" int,
  "id_car" int,
  "id_price" int,
  "data_order" date,
  "time_order" time,
  FOREIGN KEY (id_user) REFERENCES user_orend (id_user),
  FOREIGN KEY (id_car) REFERENCES car (id_car),
  FOREIGN KEY (id_price) REFERENCES price (id_price) 
 );
/*LR 3 ORBD*/
/* tasks 1 */
ALTER TABLE inspector DROP CONSTRAINT fine_id_type_fine_fkey;
/* tasks 2 */
alter table fine_list drop column "type_fine";
alter table fine_list alter column "price_fine" type float;
/*tasks 3 */
alter table sale rename column "procent_sale" to "prsale";
ALTER TABLE Department ADD CONSTRAINT prsale UNIQUE (procent_sale, prsale);
/* tasks 4 */
ALTER TABLE "Order" DROP CONSTRAINT "Order_id_user_fkey";
alter table "Order" add constraint "User" foreign key (id_user) references user_orend (id_user); 

/*LR 4 ORBD*/
/* tasks 1 */
INSERT INTO card VALUES (2,1,'1', 1);
INSERT INTO card VALUES (3,2,'2', 2);
INSERT INTO card VALUES (4,3,'3', 3);
INSERT INTO card VALUES (5,4,'4', 4);
/* tasks 2 */
copy card (num_card,date_limit,cvc) 
From 'C:\Desktop\card.csv'
Delimiter ',' CSV Header;
select*from card;
/*tasks 3 */
UPDATE card SET cvc = 4 WHERE cvc= 45;
/* tasks 4 */
delete from card where cvc=1;
delete from card where id_card=2;
/*LR 5 ORBD*/
create table card1 as select * from card where id_card<= 5;
create table card2 as select * from card where id_card>=4;
/* tasks 1 */
select * from card1 union select * from card2;
select * from card1 union all select * from card2;
/* tasks 2 */
select * from card1 intersect select * from card2;
/*tasks 3 */
SELECT * FROM card1 EXCEPT SELECT * FROM card2;
/* tasks 4 */
select * from card1,card2;
