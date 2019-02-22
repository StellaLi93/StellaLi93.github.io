---
layout:     post
title:      Mileage Redeeming Program for Air China
date:       2017-12-06
author:     Jianan Li
header-img: img/post-bg-flight.jpg
catalog: true
tags:
    - Python
    - Database
---

## Introduction
This mileage redeeming program is used Python (pandas and TinyDB) to design a small database to store the mileage redeeming policies for Air China for clients to check what ticket they can redeem based on their mileage.

The whole program can be divivded into three parts: search function, database building, and test.

## Part 1: Database building
Figure 1 is just importing. 

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/2.png)
Figure 1

Figure 2 is that two databases initiate. 

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/3.png)
Figure 2

Figure 3 is the scheme of database.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/4.png)
Figure 3

CA_Table_One_way, CA_Table_Round, SA_Table_One_way, SA_Table_Round stands for one way gift ticket in CA, round trip in CA, one way ticket by Star Alliance (except CA) and round ticket by StarAlliance(except CA), separately. 
New_User() is just a function that adds user to INFO database, use uuid5 as the identifier of a user. INS_Log() is a logger that record each transaction to log database. 
Change_INFO() is to update info database when transaction happened, and total mileage should not be influence when airlines gift user a ticket. 
User_buy_a_ticket() would add mileage and obviously Airline_gift_a_ticket() would substart mileage. 
Search_gift_Mileage() is to get how much mileage should cost when gift a ticket to user, data comes from these four variant: CA_Table_One_way, CA_Table_Round, SA_Table_One_way, SA_Table_Round. 
Output_All_data() is a function that print all data in database via pandas. User_Class_filter() is a filter that classify a user.
Note: CA: Air China; SA: Star Alliance. 

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/1.png)
Figure 4

Figure 4 shows the example how to store the redeeming policy of Air China.

## Part 2: Test
### TEST_ALL.py
Add all class in *.py in ./test and run all test in one command.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/5.png)

### New_User_test.py
Test if New_User() function works fine.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/6.png)

### Gift_ticket_test.py
Test all possibility that airlines can gift to user and check if Total_Mileage would not be changed when the clients redeem their tickets.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/7.png)

### User_class_test.py
Test if User_Class_filter() function works fine.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/8.png)

### Buy_ticket_test.py
Test User_buy_a_ticket() function.

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/9.png)

## Part 3: Search Function
This program is to help the clients who want to redeem a free ticket by using their customer ID. Here is an example:

![](https://raw.githubusercontent.com/StellaLi93/MarkDown-Photos/master/AirChina/10.png)
