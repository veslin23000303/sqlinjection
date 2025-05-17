# SQL-INJECTION
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. 
Identify IP address using ifconfig in Metasploitable2

![eth 1](https://github.com/user-attachments/assets/b4b24e48-2a5b-4458-a638-6c09203a098f)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.
![eth 2](https://github.com/user-attachments/assets/9321a31c-d3e1-4e86-89f8-d13131bd0626)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![eth 3](https://github.com/user-attachments/assets/d53e0639-45cc-4134-a800-2d781da7b37e)

Click on the menu Login/Register and register for an account


![eth 4](https://github.com/user-attachments/assets/0df0b2b0-ede2-4d78-b65e-f40b6f3730ec)

Click on the link “Please register here”
![eth 5](https://github.com/user-attachments/assets/a0d62123-cc15-44b8-98f7-261f5aef3f46)

Click on “Create Account” to display the following page:
![eth 6](https://github.com/user-attachments/assets/94959104-e793-4a97-b024-ecb352a63195)

The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;).
 For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.


![eth 7](https://github.com/user-attachments/assets/bfd773f8-bbf1-4140-b41f-a0b40d029bf2)


Click “Login”. The logged in page will show as below:
![eth 8](https://github.com/user-attachments/assets/579cbbb1-e87e-45c0-a171-78969021bbc7)



##Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password. 

![eth 9](https://github.com/user-attachments/assets/bca4de09-9e64-4e1b-8d8d-1209de3f1861)

Click the login button and you will see it enter into the administrator page.
![eth 10](https://github.com/user-attachments/assets/cd8f1fb7-8ade-429a-bd06-6c41d54697ba)


## Union-based SQL injection
UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. 
we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info” 

After logging out, Now choose the menu as shown below:
![eth 11](https://github.com/user-attachments/assets/6cf42cb0-3a38-4794-9ff8-61f164fb325a)

![eth 12](https://github.com/user-attachments/assets/de253853-ba44-4f5a-8ad4-93daba3c303e)


![eth 13](https://github.com/user-attachments/assets/6987283a-41fe-4dee-9324-0a74a7061f0a)

![eth 14](https://github.com/user-attachments/assets/c3a6a88f-21a8-4a39-b5a1-b36159bb995c)


![eth 15](https://github.com/user-attachments/assets/3a2264b9-f99b-4ba7-b0db-9c5dcf849ff8)

![eth 16](https://github.com/user-attachments/assets/337a5f06-7aad-49ce-bffa-8344629b833e)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.
![eth 17](https://github.com/user-attachments/assets/b9dea006-4b7b-4cd5-926f-2e0c51c0bdc4)


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:



![eth 18](https://github.com/user-attachments/assets/0663c319-804d-40dd-a495-f0bf260f52c1)

After adding the order by 6 into the existing url , the following error statement will be obtained:
![eth 19](https://github.com/user-attachments/assets/5bf3bf4b-b506-42cc-b27f-329240cc0e6b)



When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.
![eth 20](https://github.com/user-attachments/assets/292a406b-2147-4f61-ade7-9e9f2aeadca9)


 As it is having 5 columns the query worked fine and it provides the correct result


![eth 21](https://github.com/user-attachments/assets/6a2e4ebd-4cc3-4395-9464-c5b5236081aa)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![eth 22](https://github.com/user-attachments/assets/925e5d0a-8563-4b0b-8136-55e342260fbc)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.
![image](https://github.com/vatsan143/sqlinjection/assets/147368204/7079a658-8985-4a79-b6ce-fc9fc82ab461)
 Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.



![eth 23](https://github.com/user-attachments/assets/99167fd6-c5ec-4ee3-8dac-66216191339d)

![image](https://github.com/vatsan143/sqlinjection/assets/147368204/aa264de1-ed65-498d-96fd-98da9d720b16)
The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5.
In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one:
union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’



![image](https://github.com/vatsan143/sqlinjection/assets/147368204/6cccd0b2-1c0a-41a3-b336-e9566552a16b)

The url once executed will  retrieve table names from the “owasp 10” database.
##Extracting sensitive data such as passwords 

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

The column names of the accounts is displayed below for the following url:

button=View+Account+Details 


Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

![eth 23](https://github.com/user-attachments/assets/5c8bb7fe-2dee-44c9-849a-dcd8688faa1d)


## Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

![eth 24](https://github.com/user-attachments/assets/8722c01f-f884-419a-ab92-c770a4010660)

![eth 25](https://github.com/user-attachments/assets/1fba1f32-b37d-4f6f-8113-eef2f60f4b5d)


the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server.
Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
