# Seerdock

1. Clone recursively using SSH

   ```bash
   $ git clone --recurse-submodules git@github.com:dinasayed/Seerdock.git
   ```

   Or using HTTPS

   ```bash
   $ git clone --recurse-submodules https://github.com/dinasayed/Seerdock.git
   ```

2. Enter project directory

   ```bash
   $ cd Seerdock
   ```

3. Build MySQL image

   ```bash
   $ docker build -t noureldin/csxmysql:8.0.16 -f Dockerfile.mysql .
   ```

4. Build Crawler image

   ```bash
   $ docker build -t noureldin/heritrix:3.4.0 -f Dockerfile.crawler .
   ```

5. Build Extractor image

   ```bash
   $ docker build -t noureldin/csxextractor:5.22.1 -f Dockerfile.extractor .
   ```

6. Build DOI image

   ```bash
   $ docker build -t noureldin/doi:1.7.9 -f Dockerfile.doi .
   ```

7. Build Solr image

   ```bash
   $ docker build -t noureldin/csxsolr:4.9.0 -f Dockerfile.solr .
   ```

8. Build Apache httpd image

   ```bash
   $ docker build -t noureldin/csxhttpd:2.4 -f Dockerfile.httpd .
   ```
   
9. Run

   ```bash
   $ docker-compose up -d
   ```
   
10. Verify that all containers started successfully

    ```bash
    $ docker ps --all --filter name=Seerdock # OR
    $ docker container ls --all --filter name=Seerdock #OR
    $ docker-compose ps
    ```
    
    *Note: The extractor container will fail to start if this is the first time to run Seerdock. That is because it depends on a database table which will be created later by the CDI.*
      `[java] WARNING: java.sql.SQLSyntaxErrorException: Table 'citeseerx_crawl.main_crawl_document' doesn't exist`
   
11. Open https://localhost:8443/ in a browser and login with `admin`/`admin`

12. Create a new crawl job or add an existing one under `data/heritrix/jobs/`

13. Run CDI using

    ```bash
    $ docker exec -it -w /data/heritrix/jobs/ medseer_heritrix_1 bash -c "rm latest" # Delete latest link if already exists
    $ docker exec -it -w /data/heritrix/jobs/ medseer_heritrix_1 bash -c "ln -s <job_name> latest"
    $ docker exec -it -w /cdi/ medseer_heritrix_1 bash -c "python cdi.py"
    ```

14. Verify crawl output exists under `data\crawl\rep`

15.  Restart the extractor if it is not already running

    ```bash
    $ docker-compose start extractor # OR
    $ docker-compose down && docker-compose up -d # Restart everything
    ```

16. Verify extractor output under `data\exports\ex<today's date>`

17. Copy the extracted data

    ```bash
    $ mkdir data/ingest/  
    $ cp -r data/exports/ex<today's date> data/ingest/
    ```

18. Import the data

    ```bash
    $ docker exec -it -w /CiteSeerX/bin medseer_extractor_1 bash -c "./createXML.pl /data/ingest/ex<today's date>"
    $ docker exec -it -w /CiteSeerX/bin medseer_extractor_1 bash -c "./batchImport /data/ingest/ex<today's date>"
    ```

19. Verify that papers and authors were imported in the databases

    ```bash
    $ docker exec -it medseer_mysql_1 mysql -ppasswd 
    ```

    ```sql
    mysql> SHOW DATABASES;
    mysql> USE citeseerx;
    mysql> SHOW TABLES;
    mysql> SELECT * FROM citeseerx.papers;
    mysql> SELECT * FROM citeseerx.authors;
    ```

20. Run indexer

    ```bash
    $ chmod a+w data/solr/citeseerx/
    $ docker exec -it -w /CiteSeerX/bin medseer_extractor_1 bash -c "./updateIndex"
    ```
    
21. Open Solr http://localhost:8080/solr/

22. Open Search Application http://localhost:8080/citeseerx/index

23. Create Django database tables

    ```bash
    $ docker exec -it -w /root/csxbot-0.3/citeseerx_crawl medseer_httpd_1 bash -c "python manage.py syncdb"
    ```

24. Open Crawler http://localhost:8880/

25. ***DOESN'T WORK:*** Generate static file
    https://django.readthedocs.io/en/1.3.X/howto/static-files.html

    ```bash
    $ docker exec -it -w /root/csxbot-0.3/citeseerx_crawl medseer_httpd_1 bash -c "python manage.py collectstatic"
    ```

---

Push your commits recursively using

```bash
$ git push --recurse-submodules=on-demand
```

---

## Run MySQL separately

```bash
docker run -d --name mysql --rm -e MYSQL_ROOT_PASSWORD=passwd -e CSXDOMAIN=% -p 3306:3306 csxmysql:8.0.16 --default-authentication-plugin=mysql_native_password
```

- `-d` for detach: Run container in background and print container ID
- `--name` Assign a name to the container so that we can manage it easily using `docker exec` and `docker stop`
- `--rm` Automatically remove the container when it exits
- `-e` Set environment variables
  - `MYSQL_ROOT_PASSWORD`
  - `CSXUSER` defaults to `csx-devel`
  - `CSXDOMAIN` defaults to `ist.psu.edu`
  - `CSXPASS` defaults to `csx-devel`
  - `SITEID` defaults to `10`
  - `DEPID` defaults to `1`
- `-p <host port>:<container port>` Publish a container's port(s) to the host
- `-v /my/own/datadir:/var/lib/mysql`  Mounts the `/my/own/datadir` directory from the underlying host system as `/var/lib/mysql` inside the container, where MySQL by default will write its data files
- `-v my_volume_name:/var/lib/mysql`  Mounts `/var/lib/mysql`  as a persistent volume

### Run MySQL Client from a new container

```bash
docker run -it --rm mysql:8.0.16 mysql -ppasswd -h 10.10.10.50
```

- where `10.10.10.50` is the host machine ip address

### To stop MySQL

```bash
docker stop mysqlserver
```

---

## Run Heritrix separately

```bash
docker run --name heritrix --rm -p 8443:8443 -v ./data:/data heritrix:3.4.0
```

---

1. csxmysql

2. Heritrix

3. Httpd

4. data/crawl/rep/

5. Httpd

6. Extractor

7. Extractor

8. DOI

9. MySQL

10. -

11. /data/ingest

12. Solr

13. Solr

14. -

15. -

    
