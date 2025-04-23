**BANK**

⚫️ The bank has a variety of branches throughout the Italian area. Each branch is identified by a 
code, and it is characterized by the name, the address and the telephone number. The address 
is composed by street, number and city. The bank's customers are characterized by Social 
Security Number (SSN), full name (name and surname), date of birth, address and email. For 
each customer the database stores the list of branches in which he/she has at least a bank 
account. Each customer cannot have different bank accounts in the same branch, but he/she 
may have different bank accounts in different branches.  


⚫️ The bank has area managers. Each area manager is identified by an alphanumeric code, and 
he/she is characterized by name, email, phone number and mobile phone number (if available). 
For each area manager the title of the highest obtained degree and the date when he/she received 
that title are also known. 

⚫️ Each branch organizes some meetings periodically. Each meeting is identified by a code and it 
is characterized by the branch at which it takes place, the meeting date and time, and list of 
addressed topics. For each meeting, it is known the list of area managers which attended it. The 
meetings are classified as ordinary and extraordinary meetings. For ordinary meetings the 
motivation for which the meeting was called is stored, whereas for extraordinary ones, the level 
of priority is known. A report is written at the end of each meeting. Each report is identified by 
a code, and it is characterized by the meeting for which it was written, a brief description and 
the meeting duration. 

⚫️ Each branch manages different savings plan. Each savings plan is identified by a code that is 
unique within all savings plan referring to the same branch and it is characterized by the 
subscribing customer and a brief description. Each savings plan is associated with a set of 
deposits. Each deposit is identified by a code unique within the corresponding savings plan and 
it is characterized by the due date of payment and the corresponding amount. 

⚫️ Customers can buy or sell shares. Each share is identified by a univocal alphanumeric code, 
and it is characterized by the type and name of the company that issued it. For each transaction 
the hour, the date, the type (buy or sell), the unitary price and the quantity are known. Each 
customer can perform more transactions, for each share or for different shares, within the same 
day, and each share can be sold or bought by every customer. 

⚫️ For each branch the database stores the list of its area manager over time with the corresponding 
period of time (start and end date). Please note that a branch can not have more than one area 
manager at a time, while each area manager can manage more branches in the same time period. 

⚫️ The cleaning of the branch premises is carried out in specific days of the week. For each branch 
the database stores the days of the week on which the cleaning activity takes place and the 
corresponding time slot (i.e., start hour and end hour). 