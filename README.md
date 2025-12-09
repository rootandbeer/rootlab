# rootlab

**Download the `docker-compose.yml` file:**
```shell
wget https://github.com/rootandbeer/rootlab/blob/fe55b0eae065a0e13d7c7a775af457e3599d924e/docker-compose.yml
```


Option 1: **Run all containers including kali (for those who do not currently have Kali setup):**
```shell
docker compose up -d
```

- Kali Linux Web UI: https://172.18.1.10:6901
- Username: *kasm_user*
- Password: *password*


Option 2: **If you already have a Kali Linux VM setup, then run this inside your VM:**
```shell
docker compose config --services | grep -v kali | xargs docker compose up -d
```

