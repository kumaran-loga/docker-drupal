# docker
Docker environement setup for AWS Linux 2, Apache, PHP, MySQL Server, PHPMYADMIN
The Following Docker Script will Install Apache, PHP, MySQL Server, PHPMYADMIN in AWS Linux 2

1. Create Docker image and container to confirm.
	Open <Windows Power Shell＞ as Adminstrator
	cd C:\dev　// change to work location of docker folder		
	docker compose up -d --build

3. For Bash SQL login															
	docker exec -it dev-db-1 bash　

4. Restore SQL backups 
	① Place the SQL Dump file in the below path
		/infra/docker/mysql/test.sql	
	② Copy SQL DUMP file to SQL Container
		docker cp ./infra/docker/mysql/test.sql dev-db-1:/tmp/	
	③ Run SQL Dump file
		mysql -u root -p'your_password' your_database_name < /tmp/test.sql

4.For creating and giving privilages to SQL users.
  a. bash login to sql container
        docker exec -it dev-db-1 bash
  b. CREATE USER '<user-name>'@'%' IDENTIFIED BY '<user-password>';
  c. GRANT ALL PRIVILEGES ON *.* TO '<user-name>'@'%' WITH GRANT OPTION;
  d. FLUSH PRIVILEGES;
  e. To confirm Grant access,
      SHOW GRANTS FOR '<user-name>'@'%';

5. For bash Apache login
    docker exec -it dev-web-1 bash

6. Place source in ./backend folder

7. Create Docker	Image and Container
	①Docker container delete
	　　　　　docker compose down		
	②Docker container recreate		
	　　　　　docker compose up -d --build		

8. check localhost
