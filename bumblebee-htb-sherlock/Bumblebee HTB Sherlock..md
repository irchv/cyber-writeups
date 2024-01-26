
##### An external contractor has accessed the internal forum here at Forela via the Guest WiFi and they appear to have stolen credentials for the administrative user! We have attached some logs from the forum and a full database dump in sqlite3 format to help you in your investigation.


#### *Question* - What was the username of the external contractor?

##### *Answer* - apoole1

##### After opening the phpbb.splite3 file we can get to a table phpbb_users. There we can find a the username *apoole1* and we reassure our answer by looking in the user_email column where we can see the mail address *apoole1@contractor .net*. 
![[Pasted image 20240126155951.png]]

#### *Question* - What IP address did the contractor use to create their account?
#####  *Answer* - 10.10.0.78 
##### Using the access.log file we can trace the ip addresses and the the traffic that we need for this question. 


#### *Question* - What is the post_id of the malicious post that the contractor made?

##### *Answer* - 9 

##### By accessing the provided phbb.sqlite3 file and the phpbb_posts table we can see the id's of the posts that were made. 


#### *Question* - What is the full URI that the credential stealer sends its data to?

##### *Answer* - http:[//]10.10.0.78 [/] update.php 

##### After analyzing the phbb_posts table we can spot a snippet in the post-text column which looks like HTML code. After further look up we can find the link 
![[Pasted image 20240126155412.png]]


#### *Question* - In the forum there are plaintext credentials for the LDAP connection, what is the password?

#### *Answer* - Passw0rd1 

##### Given that the password is in plain text we need to think where it can be stored. As it is a LDAP password i think that it can be stored in the config of the sqlite3 database. After further review we successfully have found the password in the phpbb_config table.  
![[Pasted image 20240126160537.png]]


#### *Question* - What is the user agent of the Administrator user?

#### *Answer* - Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36

##### We can find the user agent for the Administrator user from the access.log file. Given that the IP address for the Admin user is 10.255.254.2, we can just simply check the User agent. 
![[Pasted image 20240126161929.png]]


#### *Question* - What time did the contractor add themselves to the Administrator group? (UTC)

##### *Answer* - 26/04/2023 10:53:51 

##### Again we can check the access.log file. In a simple logic we should be searching for a POST request as well as for a something related to "mode" that should be changed. Well it seems that this is enough to see the exact time. We just need to convert it to UTC. 
![[Pasted image 20240126162429.png]]

#### *Question* - What time did the contractor download the database backup? (UTC)

#### *Answer* - 26/04/2023 11:01:38 

##### Again working withe the access.log file. With a little thinking we assume that the DB should be downloaded as some type of archive. After searching for the most known file extensions we have found a place where we can see a GET request for a file with the name  backup_1682506471_dcsr71p7fyijoyq8.sql.gz. Bingo we got it. 
![[Pasted image 20240126162845.png]]

#### *Question* - What was the size in bytes of the database backup as stated by access.log?

##### *Answer* - 34707

##### Again looking the same snippet. We can see that we get a status code 200 for the request and after that we get the size of the archive. GGWP.
