---
layout:     post
title:      Database Implementation for Global Insurance Agency
subtitle:   
date:       2017-12-04
author:     Jianan Li
header-img: img/bg-database.jpg
catalog: true
tags:
    - Oracle
    - SQL 
---

## Introduction
Database Implementation for Global Insurance Agency project is to design a database in Oracle for agency to computerize all data relating to insurance policies and to calculate KPI for over 40 agencies and use SQL to test the rationality and integrity of the database and adjust the entity relationship diagram and relational model.

## Entity Relationship Diagram for the Insurance Agency Database
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/1.png)

## Assumptions for Entity Relationship Diagram for the Insurance Agency Database
AgentAutoSoldD, AgentAutoSoldPremiumsD, and AutoCommisionD are descriptive attributes between AGENT and AUTOPOLICY
AgentLifeSoldD, AgentLifeSoldPremiumsD, and LifeCommisionD are descriptive attributes between AGENT and LIFEPOLICY
FirstAppointmentDateD is a descriptive attribute between CLIENT and AGENT
ClientBeneficiaryD is a descriptive attribute between CLIENT and LIFEPOLICY
CarMakeD, CarModelD, CarRegistrationNoD, DriverDOBD, DriverLicenseNoD, DriverNameD, and DriverGenderD are descriptive attribute between AUTOPOLICY and CLIENT

## Data Dictionary for Entity Relationship Diagram for the Insurance Agency Database
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/54.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/55.png)
## Default Physical Data Model for the Insurance Agency Database
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/2.png)
## Revised Physical Data Model for the Insurance Agency Database
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/3.png)
## Normalized Physical Data Model for the Insurance Agency Database
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/4.png)
## Assumptions for Normalized Physical Data Model for the Insurance Agency Database
1.	AgentLifeSoldD can be calculated by querying the number of life policy numbers with a particular AgentID
2.	AgentLifeSoldPremiumsD can be calculated by SUM{LifePolicyPremium} with a particular AgentID
3.	AgentAutoSoldD can be calculated by querying the number of like policy numbers with a particular AgentID
4.	AgentAutoSoldPremiumsD can be calculated by SUM{AutoPolicyPremium} with a particular AgentID
5.	“LifePolicyPremium” and “lifePolicyValidUntil” are dependent on the type of life insurance purchased. 
6.	LifePolicyBalance can be calculated. Query where the invoices for a particular client’s policy number and their payments. Then LifePolicyBalance =  SUM(invoice amount due) – SUM(payment amount)
7.	Invoice due date is dependent on invoice date, and invoice grace date is dependent on invoice due date.
8.	AutoPolicyBalance can be calculated. Query where the invoices for a particular client’s policy number and their payments. Then AutoPolicyBalance =  SUM(invoice amount due) – SUM(payment amount)
9.	Each Driver can be registered to drive more than one car
10.	The Driver Name, DoB, and Gender are not dependent on the entire key, and can be captured from the driver’s license by itself.
11.	InvoiceGraceDate can be calculated from adding days to InvoiceDueDate

## DDL Script for Insurance Agency Database
CREATE TABLE agent (
    agentid          NUMBER(10) NOT NULL,
    agentfirstname   CHAR(15),
    agentlastname    CHAR(15 CHAR)
)
    LOGGING;

ALTER TABLE agent ADD CONSTRAINT agent_pk PRIMARY KEY ( agentid );

CREATE TABLE "AGENT-AUTOPOLICY" (
    autopolicy_autopolicynumber   NUMBER(5) NOT NULL,
    agent_agentid                 NUMBER(10) NOT NULL,
    autocommissiond               NUMBER(6)
)
    LOGGING;

ALTER TABLE "AGENT-AUTOPOLICY" ADD CONSTRAINT "AGENT-LIFEPOLICY_PKv3" PRIMARY KEY ( autopolicy_autopolicynumber );

CREATE TABLE "AGENT-LIFEPOLICY" (
    lifepolicy_lifepolicynumber   NUMBER(5) NOT NULL,
    agent_agentid                 NUMBER(10) NOT NULL,
    lifecommissiond               NUMBER(6)
)
    LOGGING;

ALTER TABLE "AGENT-LIFEPOLICY" ADD CONSTRAINT "AGENT-LIFEPOLICY_PK" PRIMARY KEY ( lifepolicy_lifepolicynumber );

CREATE TABLE autopolicy (
    autopolicynumber               NUMBER(5) NOT NULL,
    autopolicypremium              NUMBER(10),
    autopolicyvaliduntil           DATE,
    autopolicydeductibleamount     NUMBER(10),
    autopolicystatus               CHAR(10 CHAR),
    agent_agentid                  NUMBER(10),
    insurancecompany_companyname   CHAR(20 CHAR) NOT NULL
)
    LOGGING;

ALTER TABLE autopolicy ADD CONSTRAINT autopolicy_pk PRIMARY KEY ( autopolicynumber );

CREATE TABLE carinfo (
    carregistrationnod   NUMBER(10) NOT NULL,
    carmodeld            CHAR(10 CHAR),
    carmaked             CHAR(10 CHAR)
)
    LOGGING;

ALTER TABLE carinfo ADD CONSTRAINT carinfo_pk PRIMARY KEY ( carregistrationnod );

CREATE TABLE claim (
    claimid                       NUMBER(5) NOT NULL,
    claimdate                     DATE,
    claimtype                     CHAR(8 CHAR),
    claimmaker                    CHAR(20 CHAR),
    claimamount                   NUMBER(10),
    claimadjustor                 CHAR(20 CHAR),
    autopolicy_autopolicynumber   NUMBER(5) NOT NULL,
    lifepolicy_lifepolicynumber   NUMBER(5) NOT NULL
)
    LOGGING;

ALTER TABLE claim ADD CONSTRAINT claim_pk PRIMARY KEY ( claimid );

CREATE TABLE client (
    clientid              NUMBER(4) NOT NULL,
    clientfirstname       CHAR(10 CHAR),
    clientmiddlename      CHAR(10 CHAR),
    clientlastname        CHAR(10 CHAR),
    clientstreetaddress   CHAR(20 CHAR),
    clientstateaddress    CHAR(15 CHAR),
    clientusedaddress     CHAR(30 CHAR),
    clientphonenumber     INTEGER,
    clientgender          BLOB,
    clientdateofbirth     DATE
)
    LOGGING;

ALTER TABLE client ADD CONSTRAINT client_pk PRIMARY KEY ( clientid );

CREATE TABLE "CLIENT-AGENT" (
    client_clientid         NUMBER(4) NOT NULL,
    agent_agentid           NUMBER(10) NOT NULL,
    firstappointmentdated   DATE
)
    LOGGING;

ALTER TABLE "CLIENT-AGENT" ADD CONSTRAINT "CLIENT-AGENT_PK" PRIMARY KEY ( client_clientid );

CREATE TABLE "CLIENT-AUTOPOLICY" (
    autopolicy_autopolicynumber   NUMBER(5) NOT NULL,
    client_clientid               NUMBER(4) NOT NULL,
    driverinfo_driverlicensenod   NUMBER(8) NOT NULL,
    carinfo_carregistrationnod    NUMBER(10) NOT NULL
)
    LOGGING;

ALTER TABLE "CLIENT-AUTOPOLICY"
    ADD CONSTRAINT "CLIENT-AUTOPOLICY_PK" PRIMARY KEY ( autopolicy_autopolicynumber,driverinfo_driverlicensenod,carinfo_carregistrationnod )
;

CREATE TABLE driverinfo (
    driverlicensenod   NUMBER(8) NOT NULL,
    drivernamed        CHAR(10 CHAR),
    driverdobd         DATE,
    drivergenderd      BLOB
)
    LOGGING;

ALTER TABLE driverinfo ADD CONSTRAINT driverinfo_pk PRIMARY KEY ( driverlicensenod );

CREATE TABLE insurancecompany (
    companyname                CHAR(20 CHAR) NOT NULL,
    insurancecostreetaddress   CHAR(30 CHAR),
    insurancecostateaddress    CHAR(30 CHAR),
    insurancecophone           CHAR(10 CHAR)
)
    LOGGING;

ALTER TABLE insurancecompany ADD CONSTRAINT insurancecompany_pk PRIMARY KEY ( companyname );

CREATE TABLE invoice (
    invoiceid                     NUMBER(10) NOT NULL,
    invoiceamountdue              NUMBER(10),
    autopolicy_autopolicynumber   NUMBER(5) NOT NULL,
    lifepolicy_lifepolicynumber   NUMBER(5) NOT NULL,
    invoiceduedate_invoicedate    DATE NOT NULL
)
    LOGGING;

ALTER TABLE invoice ADD CONSTRAINT invoice_pk PRIMARY KEY ( invoiceid );

CREATE TABLE invoiceduedate (
    invoicedate      DATE NOT NULL,
    invoicedatedue   DATE NOT NULL
)
    LOGGING;

ALTER TABLE invoiceduedate ADD CONSTRAINT invoiceduedate_pk PRIMARY KEY ( invoicedate );

CREATE TABLE lifecharges (
    lifepolicytype         CHAR(10 CHAR) NOT NULL,
    lifepolicypremium      NUMBER(10),
    lifepolicyvaliduntil   DATE
)
    LOGGING;

ALTER TABLE lifecharges ADD CONSTRAINT lifecharges_pk PRIMARY KEY ( lifepolicytype );

CREATE TABLE lifepolicy (
    lifepolicynumber               NUMBER(5) NOT NULL,
    lifepolicystatus               CHAR(10 CHAR),
    client_clientid                NUMBER(4) NOT NULL,
    agent_agentid                  NUMBER(10) NOT NULL,
    insurancecompany_companyname   CHAR(20 CHAR) NOT NULL,
    lifecharges_lifepolicytype     CHAR(10 CHAR) NOT NULL
)
    LOGGING;

ALTER TABLE lifepolicy ADD CONSTRAINT lifepolicy_pk PRIMARY KEY ( lifepolicynumber );

CREATE TABLE "LIFEPOLICY-CLIENT" (
    client_clientid               NUMBER(4) NOT NULL,
    lifepolicy_lifepolicynumber   NUMBER(5) NOT NULL,
    clientbeneficiaryd            CHAR(15 CHAR)
)
    LOGGING;

ALTER TABLE "LIFEPOLICY-CLIENT" ADD CONSTRAINT "LIFEPOLICY-CLIENT_PK" PRIMARY KEY ( client_clientid );

CREATE TABLE payment (
    paymentid           NUMBER(10) NOT NULL,
    paymentamount       NUMBER(10),
    paymentdate         DATE,
    paymenttype         CHAR(3 CHAR),
    client_clientid     NUMBER(4) NOT NULL,
    invoice_invoiceid   NUMBER(10) NOT NULL
)
    LOGGING;

ALTER TABLE payment ADD CONSTRAINT payment_pk PRIMARY KEY ( paymentid );

ALTER TABLE "AGENT-LIFEPOLICY"
    ADD CONSTRAINT "AGENT-LIFEPOLICY_AGENT_FK" FOREIGN KEY ( agent_agentid )
        REFERENCES agent ( agentid )
    NOT DEFERRABLE;

ALTER TABLE "AGENT-AUTOPOLICY"
    ADD CONSTRAINT "AGENT-LIFEPOLICY_AGENT_FKv2" FOREIGN KEY ( agent_agentid )
        REFERENCES agent ( agentid )
    NOT DEFERRABLE;

ALTER TABLE "AGENT-AUTOPOLICY"
    ADD CONSTRAINT "AGENT-LIFEPOLICY_AUTOPOLICY_FK" FOREIGN KEY ( autopolicy_autopolicynumber )
        REFERENCES autopolicy ( autopolicynumber )
    NOT DEFERRABLE;

ALTER TABLE "AGENT-LIFEPOLICY"
    ADD CONSTRAINT "AGENT-LIFEPOLICY_LIFEPOLICY_FK" FOREIGN KEY ( lifepolicy_lifepolicynumber )
        REFERENCES lifepolicy ( lifepolicynumber )
    NOT DEFERRABLE;

ALTER TABLE autopolicy
    ADD CONSTRAINT autopolicy_agent_fk FOREIGN KEY ( agent_agentid )
        REFERENCES agent ( agentid )
    NOT DEFERRABLE;

ALTER TABLE autopolicy
    ADD CONSTRAINT autopolicy_insuranceco_fk FOREIGN KEY ( insurancecompany_companyname )
        REFERENCES insurancecompany ( companyname )
    NOT DEFERRABLE;

ALTER TABLE claim
    ADD CONSTRAINT claim_autopolicy_fk FOREIGN KEY ( autopolicy_autopolicynumber )
        REFERENCES autopolicy ( autopolicynumber )
    NOT DEFERRABLE;

ALTER TABLE claim
    ADD CONSTRAINT claim_lifepolicy_fk FOREIGN KEY ( lifepolicy_lifepolicynumber )
        REFERENCES lifepolicy ( lifepolicynumber )
    NOT DEFERRABLE;

ALTER TABLE "CLIENT-AGENT"
    ADD CONSTRAINT "CLIENT-AGENT_AGENT_FK" FOREIGN KEY ( agent_agentid )
        REFERENCES agent ( agentid )
    NOT DEFERRABLE;

ALTER TABLE "CLIENT-AGENT"
    ADD CONSTRAINT "CLIENT-AGENT_CLIENT_FK" FOREIGN KEY ( client_clientid )
        REFERENCES client ( clientid )
    NOT DEFERRABLE;

ALTER TABLE "CLIENT-AUTOPOLICY"
    ADD CONSTRAINT "CLIENT-AUTO_DRIVERINFO_FK" FOREIGN KEY ( driverinfo_driverlicensenod )
        REFERENCES driverinfo ( driverlicensenod )
    NOT DEFERRABLE;

ALTER TABLE "CLIENT-AUTOPOLICY"
    ADD CONSTRAINT "CLIENT-AUTOPOLICY_AUTOFK" FOREIGN KEY ( autopolicy_autopolicynumber )
        REFERENCES autopolicy ( autopolicynumber )
    NOT DEFERRABLE;

ALTER TABLE "CLIENT-AUTOPOLICY"
    ADD CONSTRAINT "CLIENT-AUTOPOLICY_CARINFO_FK" FOREIGN KEY ( carinfo_carregistrationnod )
        REFERENCES carinfo ( carregistrationnod )
    NOT DEFERRABLE;

ALTER TABLE "CLIENT-AUTOPOLICY"
    ADD CONSTRAINT "CLIENT-AUTOPOLICY_CLIENT_FK" FOREIGN KEY ( client_clientid )
        REFERENCES client ( clientid )
    NOT DEFERRABLE;

ALTER TABLE invoice
    ADD CONSTRAINT invoice_autopolicy_fk FOREIGN KEY ( autopolicy_autopolicynumber )
        REFERENCES autopolicy ( autopolicynumber )
    NOT DEFERRABLE;

ALTER TABLE invoice
    ADD CONSTRAINT invoice_invoicedue_fk FOREIGN KEY ( invoiceduedate_invoicedate )
        REFERENCES invoiceduedate ( invoicedate )
    NOT DEFERRABLE;

ALTER TABLE invoice
    ADD CONSTRAINT invoice_lifepolicy_fk FOREIGN KEY ( lifepolicy_lifepolicynumber )
        REFERENCES lifepolicy ( lifepolicynumber )
    NOT DEFERRABLE;

ALTER TABLE lifepolicy
    ADD CONSTRAINT lifepolicy_agent_fk FOREIGN KEY ( agent_agentid )
        REFERENCES agent ( agentid )
    NOT DEFERRABLE;

ALTER TABLE lifepolicy
    ADD CONSTRAINT lifepolicy_client_fk FOREIGN KEY ( client_clientid )
        REFERENCES client ( clientid )
    NOT DEFERRABLE;

ALTER TABLE lifepolicy
    ADD CONSTRAINT lifepolicy_insurancecompany_fk FOREIGN KEY ( insurancecompany_companyname )
        REFERENCES insurancecompany ( companyname )
    NOT DEFERRABLE;

ALTER TABLE lifepolicy
    ADD CONSTRAINT lifepolicy_lifecharges_fk FOREIGN KEY ( lifecharges_lifepolicytype )
        REFERENCES lifecharges ( lifepolicytype )
    NOT DEFERRABLE;

ALTER TABLE "LIFEPOLICY-CLIENT"
    ADD CONSTRAINT "LIFEPOLICY-CLIENT_CLIENT_FK" FOREIGN KEY ( client_clientid )
        REFERENCES client ( clientid )
    NOT DEFERRABLE;

ALTER TABLE "LIFEPOLICY-CLIENT"
    ADD CONSTRAINT "LIFEPOLICY-CLIENT_LIFEPOLICYFK" FOREIGN KEY ( lifepolicy_lifepolicynumber )
        REFERENCES lifepolicy ( lifepolicynumber )
    NOT DEFERRABLE;

ALTER TABLE payment
    ADD CONSTRAINT payment_client_fk FOREIGN KEY ( client_clientid )
        REFERENCES client ( clientid )
    NOT DEFERRABLE;

ALTER TABLE payment
    ADD CONSTRAINT payment_invoice_fk FOREIGN KEY ( invoice_invoiceid )
        REFERENCES invoice ( invoiceid )
    NOT DEFERRABLE;

## Table Structures for Insurance Agency 
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/5.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/6.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/7.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/8.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/9.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/10.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/11.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/12.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/13.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/14.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/15.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/16.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/17.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/18.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/19.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/20.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/21.png)
## Insurance Agency Database Tables with Data
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/22.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/23.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/24.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/25.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/26.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/27.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/28.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/29.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/30.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/31.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/32.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/33.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/34.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/35.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/36.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/37.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/38.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/39.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/40.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/41.png)
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/42.png)
## Application
10 SQL Queries using the Insurance Agency Database.
### List the clients’ names who buy life policies and their agent’s name. 
select CLIENTID, CLIENTFIRSTNAME|| ','|| CLIENTMIDDLENAME || ','|| CLIENTLASTNAME as client, LIFEPOLICYNUMBER, LIFEPOLICYSTATUS, AGENTID, AGENTFIRSTNAME|| ','|| AGENTLASTNAME as agent
from CLIENT, LIFEPOLICY, AGENT
where CLIENT.CLIENTID = LIFEPOLICY. CLIENT_CLIENTID
and AGENT.AGENTID = LIFEPOLICY. AGENT_AGENTID
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/43.png)
### List the claim ID, amount and insurance company of auto policy. 
select CLAIMID, CLAIMAMOUNT, AUTOPOLICYPREMIUM, AUTOPOLICYSTATUS, COMPANYNAME, INSURANCECOSTREETADDRESS||','||INSURANCECOSTATEADDRESS as address, INSURANCECOPHONE 
from CLAIM, AUTOPOLICY, INSURANCECOMPANY
where CLAIM. AUTOPOLICY_AUTOPOLICYNUMBER = AUTOPOLICY. AUTOPOLICYNUMBER
and AUTOPOLICY. INSURANCECOMPANY_COMPANYNAME = INSURANCECOMPANY.COMPANYNAME
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/44.png)
### List the client’s info and their payment type for any who bought a life policy.
select CLIENTFIRSTNAME|| ','|| CLIENTMIDDLENAME || ','|| CLIENTLASTNAME as client, INVOICEAMOUNTDUE, LIFEPOLICYNUMBER, PAYMENTTYPE
from CLIENT, INVOICE, LIFEPOLICY, PAYMENT
where PAYMENT.CLIENT_CLIENTID = CLIENT.CLIENTID
and PAYMENT.INVOICE_INVOICEID = INVOICE.INVOICEID
and LIFEPOLICY.CLIENT_CLIENTID = CLIENT.CLIENTID
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/45.png)
### List our client Ferris’s policy type and her insurance company. 
select CLIENTFIRSTNAME|| ','|| CLIENTMIDDLENAME || ','|| CLIENTLASTNAME as client, COMPANYNAME, LIFECHARGES_LIFEPOLICYTYPE
from CLIENT, INSURANCECOMPANY, LIFEPOLICY
where CLIENT. CLIENTID = LIFEPOLICY.CLIENT_CLIENTID
and LIFEPOLICY.INSURANCECOMPANY_COMPANYNAME = INSURANCECOMPANY.COMPANYNAME
and CLIENTFIRSTNAME = 'Ferris'
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/46.png)
### List the total number of clients and agents.
select count(distinct CLIENTID) as number_of_clients, count(distinct AGENTID) as number_of_agents
from CLIENT, AGENT
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/47.png)
### List the clients’ payment.
select CLIENTFIRSTNAME|| ','|| CLIENTMIDDLENAME || ','|| CLIENTLASTNAME as client, PAYMENTAMOUNT, PAYMENTTYPE
from CLIENT, PAYMENT
where CLIENT. CLIENTID = PAYMENT.CLIENT_CLIENTID
order by PAYMENTAMOUNT 
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/48.png)
### List clients’ life policy balance.
select CLIENTFIRSTNAME|| ','|| CLIENTMIDDLENAME || ','|| CLIENTLASTNAME as client, INVOICEAMOUNTDUE, PAYMENTAMOUNT, INVOICEAMOUNTDUE- PAYMENTAMOUNT as LifePolicyBalance
from CLIENT, INVOICE, PAYMENT
where INVOICE.INVOICEID = PAYMENT. INVOICE_INVOICEID
and CLIENT.CLIENTID = PAYMENT. CLIENT_CLIENTID
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/49.png)
### Compare Freddy Hodgon’s different auto policy premium.
select AGENTFIRSTNAME|| ','|| AGENTLASTNAME as agent, AUTOPOLICYPREMIUM
from AGENT, AUTOPOLICY
where AUTOPOLICY. AGENT_AGENTID = AGENT.AGENTID
and AGENTFIRSTNAME = 'Freddy'
and AGENTLASTNAME = 'Hodgon'
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/50.png)
### Compare clients who pay their invoice within one month.
select CLIENT_CLIENTID, PAYMENTDATE, INVOICEDUEDATE_INVOICEDATE as INVOICEDATE, PAYMENTDATE-INVOICEDUEDATE_INVOICEDATE as PaymentPeriod
from PAYMENT, INVOICE
where PAYMENT.INVOICE_INVOICEID = INVOICE.INVOICEID
and PAYMENTDATE-INVOICEDUEDATE_INVOICEDATE < 30
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/51.png)
### List the total client number of every agent.
select AGENTFIRSTNAME, AGENTLASTNAME, count(*)
from AGENT, CLIENT_AGENT
where AGENT.AGENTID = CLIENT_AGENT.AGENT_AGENTID
group by AGENTFIRSTNAME, AGENTLASTNAME
order by count(*)desc
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/52.png)

## Screenshot of Insurance Agency APEX Database Completion
![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/Database/53.png)
