# Command Line Log Analysis


<h2>Description</h2>
In this linux command line log analysis task, I used various commands like grep, wc, sort, | , -d " ", etc to filter out an ip address using a brute force hydra attack in the access.log file. 


<h2>Languages and Utilities Used</h2>

- <b>linux CLI</b>


<h2>Environments Used </h2>

- <b>Unbuntu</b> 

<br />
<br />
I used "ls -lh access.log" to list the contents of the access.log and -l for longlisting format and -h for human readable format like KB,MB,orGB. I used "wc -l" to list only the lines in the file, "wc -w" to list the words in the file, and "wc -c" to count the bytes in the file. 

![1) showings stats](https://github.com/user-attachments/assets/7430a093-e3a0-4b1a-8c0b-105c9611b5e4)

<br />
<br />
Using "head access.log" will list the first 10 lines of the access.log and "head access.log -n 1" to only list the first line from the access.log. 

![2) head, head n1](https://github.com/user-attachments/assets/4ac7f79b-01cb-4aa3-bfae-610348e1ef1f)

<br />
<br />  
Using "tail access.log -n 1" will list only the last line of the access.log.

![3) tail n1](https://github.com/user-attachments/assets/d629ace9-669e-41db-a5c2-410e95675e06)

<br />
<br />
Using the "cut access.log -d " " -f 1 | sort | uniq -c" to cut out the first section with delimiter " ", sorting, and counting the unique ip address will show 2920 hits for ip 62.122.201.246.

![4) sort uniq count shows 1 ip  used a lot](https://github.com/user-attachments/assets/83638592-ab5d-4e27-a6b4-ed9c411bd846)

<br />
<br />
Using "cut access.log -d " " -f 1 | sort | uniq -c | sort -nr | grep -v " 1 " " will cut out only the first field separated by elimiter " ", sort it, count the unique entries, then grep -v to remove ip address with only 1 count " 1 " infront of it to show two remaining ip address with multiple hits sorted from numerical reverse. 

![5) removing ips with 1 entry shows 2 results](https://github.com/user-attachments/assets/7930286a-baa1-431d-baf5-3a26ab06ceb7)

<br />
<br />
Looking upo the 62.122.201.245 ip shows a whois-domain from Ukraine. 

![6) whois domain](https://github.com/user-attachments/assets/7276e770-6dfe-47a7-8342-37bdb56e1a5a)

<br />
<br />
Using "cut -d "\"" -f 6 access.log | sort | uniq -c | sort -nr" extracts the 6th field from each line of the access.log file, using double quotes as the delimiter typically this field often contains the user agent string. Then sorted in reverse numerical order with a count for each unique entry. We can notice a 2891 counts for User-Agent Hydra and 29 counts for Nmap Scripting Engine. 


![7) hydra and nmap identified ( scan and brute force) ](https://github.com/user-attachments/assets/0587874e-fb08-4164-86ef-228cf165e944)

<br />
<br />  
Using "grep "Mozilla/5.0 (Hydra)" access.log | awk '{print $1}' | sort | uniq -c" to analyze and summarize specific IP addresses from an access log file. grep "Mozilla/5.0 (Hydra)" access.log: searches for lines in the access.log file containing the user agent string "Mozilla/5.0 (Hydra)". awk '{print $1}': extracts the first field from each matching line, which typically represents the IP address in access logs. Using "grep "Mozilla/5.0 (Hydra)" access.log | awk '$9 > 200' " to filter and analyze specific entries in the access.log file. awk '$9 > 200': This filters the grep results, selecting only lines where the 9th field is greater than 200. In a typical access log format, the 9th field often represents the HTTP status code. By filtering for values greater than 200, this command is likely looking for non-successful HTTP responses, which include:
3xx (Redirection)
4xx (Client errors)
5xx (Server errors)
This command is particularly useful for:
Identifying potentially problematic requests from the specific "Mozilla/5.0 (Hydra)" user agent.
Detecting unsuccessful attempts or errors associated with this user agent.
Analyzing the frequency and types of non-200 status codes for requests from this particular client.

![8) grepping for ip using hydra brute force and grep awk for 9th field   200](https://github.com/user-attachments/assets/f6cfb834-f6ca-4897-99ce-5fbf10001de5)

<br />
<br />
Using "grep "17/Jul/2024: 18: 49:02 -0400" access.log" shows the GET/POST request to "login.php" over http. 

![9) grepping for timestamp](https://github.com/user-attachments/assets/14851bd3-fa89-4a66-b149-f609e9c8886a)

<br />
<br />
Finally, using "grep "17/Jul/2024: 18: 49:02 -0400" access.log | grep -v "login.php" " will filter out the login.php showing only a GET /admin.php request from this brute force attack. Furthur investigation is required. 

![10) grp out loginphp shows admin php get ](https://github.com/user-attachments/assets/b98dc82a-5dfe-4db5-a848-f37d3b637609)

<br />
<br />  
