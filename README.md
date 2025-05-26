# sqlinjection
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
SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![Screenshot 2025-05-26 082922](https://github.com/user-attachments/assets/f9befcb9-d9a3-4aaa-9e38-4dd788e10e6e)


Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![Screenshot 2025-05-26 082934](https://github.com/user-attachments/assets/282e1f32-a829-447f-b4ce-e38a819d096d)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![Screenshot 2025-05-26 083015](https://github.com/user-attachments/assets/29f3737c-2818-482d-8a55-7ae163dc7982)


Click on the menu Login/Register and register for an account

![Screenshot 2025-05-26 083031](https://github.com/user-attachments/assets/80f6c542-73f0-4f8d-a27b-aec9ca3db16a)


Click on “Create Account” to display the following page:


![Screenshot 2025-05-26 083044](https://github.com/user-attachments/assets/465e0ecb-960e-4190-a069-4dbbe44632b4)

![Screenshot 2025-05-26 083057](https://github.com/user-attachments/assets/0902064a-682e-4614-9a81-c9ea8aec29e0)
![Screenshot 2025-05-26 083107](https://github.com/user-attachments/assets/d64d02cc-214b-4d5c-93dc-e74ea98cb7c6)

![Screenshot 2025-05-26 083123](https://github.com/user-attachments/assets/837e186e-b6a7-4350-ad9a-f91c9c5a1bee)


![Screenshot 2025-05-26 083355](https://github.com/user-attachments/assets/89a038af-ca68-4768-9a4a-fe311556116b)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

![Screenshot 2025-05-26 083420](https://github.com/user-attachments/assets/fdbbfb1c-0d48-4368-8b6a-74b4f8ba5a08)

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page. Click “Login”.

![Screenshot 2025-05-26 083435](https://github.com/user-attachments/assets/49926e7b-94c2-43f1-b940-5904f979dbfc)

![Screenshot 2025-05-26 083447](https://github.com/user-attachments/assets/32d63658-d3bd-4649-ae95-0c19bdc23e2c)


![Screenshot 2025-05-26 083505](https://github.com/user-attachments/assets/3c35e02a-b58a-4cff-98b2-8e0e5a513c02)

The username field is vulnerable. Put (blaise’ #) or (blaise’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “blaise.” Now after logging out you will see the login page. In the login page give blaise’ # . You can see the page now enters into the administrator page as before when giving the password.

![Screenshot 2025-05-26 083519](https://github.com/user-attachments/assets/ee048626-36c5-4fb2-b974-5a2291442fd0)

![Screenshot 2025-05-26 083605](https://github.com/user-attachments/assets/403f9801-00c6-4fa4-ac59-f749fb3a44a4)

![Screenshot 2025-05-26 083620](https://github.com/user-attachments/assets/c82ede94-4618-4c7a-958b-5d231fdc6323)


Click the login button and you will see it enter into the administrator page

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![Screenshot 2025-05-26 083633](https://github.com/user-attachments/assets/5bf5d5ba-e683-4bb5-8388-ca27c0ba32a0)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.


Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below: http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27%order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details


After adding the order by 6 into the existing url , the following error statement will be obtained:

As it is having 5 columns the query worked fine and it provides the correct result


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5).


As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![Screenshot 2025-05-26 083355](https://github.com/user-attachments/assets/2160c71e-be20-46f3-a55e-36226b71a6ec)


Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database. http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2025-05-26 083420](https://github.com/user-attachments/assets/2528a8d1-3156-49ab-9967-170f63e5f915)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

![Screenshot 2025-05-26 083533](https://github.com/user-attachments/assets/5ae197a8-9ef6-4698-bd03-99255e3caaa1)

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’ http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

The url once executed will retrieve table names from the “owasp 10” database.

Extracting sensitive data such as passwords
When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

The column names of the accounts is displayed below for the following url: http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

![Screenshot 2025-05-26 083550](https://github.com/user-attachments/assets/d6a3eeb2-4822-4933-9f36-68b20875f2b2)

Ex: (union select 1,username,password,is_admin,5 from accounts). http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2025-05-26 083620](https://github.com/user-attachments/assets/58166eb2-bd01-4090-9c18-b8c88c6656dd)

Reading and writing files on the web-server
We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

![Screenshot 2025-05-26 083633](https://github.com/user-attachments/assets/876ed970-1a31-46b3-acef-5e6aea76cf24)

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null). http://192.168.1.10/mutillidae/index.php?page=user-info.php&username=blaise%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details


![Screenshot 2025-05-26 083702](https://github.com/user-attachments/assets/19ffac4c-1ce1-4ae2-ad64-299f8c47d899)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).



## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
