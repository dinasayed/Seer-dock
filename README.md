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

   