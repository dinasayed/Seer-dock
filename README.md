# MedSeer

1. Clone recursively using SSH

   ```bash
   $ git clone --recurse-submodules git@github.com:dinasayed/medseer.git
   ```

   Or using HTTPS

   ```bash
   $ git clone --recurse-submodules https://github.com/dinasayed/medseer.git
   ```

2. Build MySQL image

   ```bash
   $ docker build -t noureldin/csxmysql:8.0.16 -f Dockerfile.mysql .
   ```

3. Build Crawler image

   ```bash
   $ docker build -t noureldin/heritrix:3.4.0 -f Dockerfile.crawler .
   ```

4. Build Extractor image

   ```bash
   $ docker build -t noureldin/csxextractor:5.22.1 -f Dockerfile.extractor .
   ```

5. Build DOI image

   ```bash
   $ docker build -t noureldin/doi:1.7.9 -f Dockerfile.doi .
   ```

   