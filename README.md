# MedSeer

1. Clone recursively using SSH

   ```bash
   $ git clone --recurse-submodules git@github.com:dinasayed/medseer.git
   ```

   Or using HTTPS

   ```bash
   $ git clone --recurse-submodules https://github.com/dinasayed/medseer.git
   ```

2. Enter project directory

   ```bash
   $ cd medseer
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
   
10. Open https://localhost:8443/ in a browser and login with `admin`/`admin`

11. Crawl

12. Run CDI using

    ```bash
    $ docker exec -it -w /data/heritrix/jobs/ heritrix bash -c "ln -s BigDatapapers latest"
    docker exec -it -w /cdi/ heritrix bash -c "python cdi.py"
    ```

    ---

    # 

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

    