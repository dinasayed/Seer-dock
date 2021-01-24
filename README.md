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

---

Push your commits recursively using

```bash
$ git push --recurse-submodules=on-demand
```

