# atlassian
It's Atlassian docker compose file, to run Atlassian products with docker on one single machine.

```
jira.example.com                        wiki.example.com
       +                                        +
       |                                        |
       +----------------------------------------+
                           |
                           v
                         Nginx
                           +
       +-----------------------------------------+
       |                   |                     |
       v                                         v
Atlassian Jira                         Atlassian Confluence
[host:jira:8080]                      [host:Confluence:8090]
       +                                         |
       |                                         |
       +-----------------------------------------+
                           |
                           v
                        Postgres
                   [host:database:5432]
                           +
                           |
       +------------------------------------------+
       |                                          |
       v                                          v
   [db:jira]                                  [db:wiki]
```


Atlassian supported products:

- Jira `latest`
- Confluence `latest`


With:
- Postgres `9.4`
- Nginx `latest`

Requirements:

- Docker version 1.13.1+
- docker-compose version 1.10.0+

Docker image source files:

- [cptactionhank/atlassian-jira](https://hub.docker.com/r/cptactionhank/atlassian-jira/)
- [cptactionhank/atlassian-confluence](https://hub.docker.com/r/cptactionhank/atlassian-confluence/)
- [nginx](https://hub.docker.com/_/nginx/)
- [postgres](https://hub.docker.com/_/postgres/)

How to use:

1. Clone the atlassian folder:


    ```
    $ git clone https://github.com/Nosphorus/atlassian
    ```

2. Set environment variables:
   These will set :::::

    ```
    $ export DOMAIN=example.com
     ```

3. Run docker compose:


    ```
    $ docker-compose -p atlassian up
    ```

4. Set `DNS` according to the above `DOMAIN` value, on somewhere that you want to connect to host of `docker-compose`:


    ```
    $ vim /etc/hosts
        127.0.0.1 jira.example.com www.jira.example.com
        127.0.0.1 wiki.example.com www.wiki.example.com
        
    ```
Replace `127.0.0.1` with IP of host that `docker-compose` command run on it.

5. Create Databases:


    ```
    $ docker exec -it atlassian_database_1  psql -U postgres
       postgres=# CREATE DATABASE jira;
       postgres=# CREATE DATABASE wiki;
       postgres=# \l
       postgres-# \q
    ```

6. Browse Atlassian products:


        ```
        http://jira.example.com
        http://wiki.example.com
        ```

Notes:

Data persisted on the  named volumes, to see them:


       $ docker volume ls
       local               atlassian_confluence-data
       local               atlassian_jira-data
       local               atlassian_database-data
