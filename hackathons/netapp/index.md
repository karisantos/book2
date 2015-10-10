# NetApp

Brian McKean from NetApp presented to the class a data analysis problem he'd
like to get some help on.

# Overview

Listen to his presentation carefully. Write down the answers to the following
questions to show your team's understanding of the basics of the problem.

## What is the problem?
Can less expensive SSDs (which have shorter life spans) be used in NetApp products? 
SSDs wear out based on use and technology, random vs sequential writes - 5 year life span 8 dwpd is max throughput via bus

How are customers using the systems
can we save money by using lower endurance flash?
If 0.5% of drives wear out, endurance is OK
660K pulls per week growing 6-8 TB per month

## Why is the problem important?
The use of less expensive parts will increase profits and/or lower product cost.  But NetApp needs to ensure that the SSDs meet the requirements which can be driven by customer use of the server systems.

## What dataset has been made available?
ASUP file (a few columns of it), which contains diagnostic data sent by storage products in the field.


## What specific questions are being raised?
Ideally the storage products would be sending data daily, and each measurement would be near 24 hours in length. A quick scan of the data shows that the delta time varies wildly. How much of the data is "good"? What does "good" even mean? Is there any connection between Release, FW version or SW version and good and bad delta times?


# Q/A session

We will run a Q/A session. Before the session, compile a list of questions you
want to ask. Then, teams will take turn to ask Brian follow up questions.
Each team gets to ask one question each time. Write down the questions your team
wanted to ask and the answers you received. If another team happens to ask the
same question, simply write down the answer you heard.

## What are the basetime and endtime formats? 
in seconds but with an unkown start time and unkown time zone
(actually it seems to be Unix time)

## Range of dates? 
Jan - June 2015

## In theory (in a perfect world), should each system have an entry for every day?

Yes, but not all systems are reporting their data correctly.

# Approach

distribution of measure times
serial number delta times



Based on the information you've obtained during the Q/A session, come up with
plan how your team will tackle this problem.

## How should the dataset be imported into Tableau? 
As the CSV file.


## How should the work be distributed among the team members?
Each individual attack the problem and find interesting issues during the hackathon, then refine during our group time.

## What types of charts or visualizations to use to support the answer?
Charts that answer "how many"? How many of the delta times are good? How many are bad? - Bar charts help to visualize that.
Which FW and SW versions produce good delta times and which produce bad? Circle views.


## What questions may be too complex for Tableau and may require custom scripts to be written?
At first it seemed that tableau was having trouble with serial numbers that were numeric (as opposed to a combination of numbers and letters). But importing the 2nd CSV cleared up that problem. Other I came across no questions I couldn't answer with tableau. Learning how to create bins of varying sizes with a calculated field really helped my analysis.
