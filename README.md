# Setup

## Create project directory and clone project

### Create directory for assignment 12

```bash
mkdir assignment12
```

### Clone Module 12 github repo and copy files to project

```bash
git clone git@github.com:kaw393939/module12_is601.git .
```

**Check all files in directory**

```bash
ls -la

total 68
drwxr-xr-x  8 dbalicky dbalicky 4096 Nov 24 18:13 .
drwxr-xr-x 13 dbalicky dbalicky 4096 Nov 24 18:13 ..
drwxr-xr-x  7 dbalicky dbalicky 4096 Nov 24 18:13 .git
drwxr-xr-x  3 dbalicky dbalicky 4096 Nov 24 18:13 .github
-rw-r--r--  1 dbalicky dbalicky   71 Nov 24 18:13 .gitignore
drwxr-xr-x  2 dbalicky dbalicky 4096 Nov 24 18:13 .vscode
-rw-r--r--  1 dbalicky dbalicky 1092 Nov 24 18:13 Dockerfile
-rw-r--r--  1 dbalicky dbalicky 1061 Nov 24 18:13 LICENSE
-rw-r--r--  1 dbalicky dbalicky 5162 Nov 24 18:13 README.md
drwxr-xr-x  7 dbalicky dbalicky 4096 Nov 24 18:13 app
-rw-r--r--  1 dbalicky dbalicky 1628 Nov 24 18:13 docker-compose.yml
-rwxr-xr-x  1 dbalicky dbalicky  158 Nov 24 18:13 init-db.sh
-rw-r--r--  1 dbalicky dbalicky 1009 Nov 24 18:13 pytest.ini
-rw-r--r--  1 dbalicky dbalicky  899 Nov 24 18:13 requirements.txt
drwxr-xr-x  2 dbalicky dbalicky 4096 Nov 24 18:13 templates
drwxr-xr-x  5 dbalicky dbalicky 4096 Nov 24 18:13 tests
```

**Remove .git to set up new git repo**

```bash
rm -rf .git
```

### Open project directory in VSCode

```bash
code .
```

## Set up venv and github repo

### Set python version to 3.10

```bash
pyenv local 3.10
```

**Add .python-version to gitignore file**

### Create and activate venv

```bash
python3 -m venv venv

source venv/bin/activate
```

### Initialize git repo and set remote github repo

```bash
git init

git remote add origin git@github.com:dbalicky/FAIS601_m12.git
# or git remote set-url origin git@github.com:dbalicky/FAIS601_m12.git
```

### Initial commit and push

```bash
git add .

git commit -m 'Initial commit'

git push -u origin main
```

# GitHub Actions Issues

## Test Error on 'Install Dependencies'

### Error output

```text
Failed to install browsers
Error: ERROR: Playwright does not support chromium on ubuntu26.04-x64
Error: Process completed with exit code 1.
```

### Change ubuntu version in test.yml

```yml
runs-on: ubuntu-24.04
```

**Commit and push changes**

## Security Error on 'Run Trivy vulnerability scanner'

### Update vulnerable dependency versions

```ini
anyio==4.14.2
certifi==2026.10.18
cffi==2.0.0
cryptography==50.0.0
dnspython==2.9.2
fastapi==0.139.1
h11==0.16.0
httpcore==1.0.9
pyasn1==0.6.5
python-jose==3.5.0
python-multipart==0.0.30
# remove starlette==0.45.3
urllib3==2.8.0
```

Removed starlette dependency from requirements.txt to allow fastapi to automatically install compatible version.

### Update and test dependency version compatibility

```bash
pip install --upgrade -r requirements.txt

pip check
```

**Found version Error with dnspython:**
```text
ERROR: Ignored the following versions that require a different python version: 2.9.0 Requires-Python >=3.11; 2.9.1 Requires-Python >=3.11; 2.9.2 Requires-Python >=3.11; 7.17.0 Requires-Python >=3.11; 7.17.1 Requires-Python >=3.11; 7.17.2 Requires-Python >=3.11; 7.17.3 Requires-Python >=3.11; 7.17.4 Requires-Python >=3.11; 7.17.5 Requires-Python >=3.11
ERROR: Could not find a version that satisfies the requirement dnspython==2.9.2 (from versions: 1.11.0, 1.11.1, 1.12.0, 1.13.0, 1.14.0, 1.15.0, 1.16.0, 2.0.0rc1, 2.0.0rc2, 2.0.0, 2.1.0rc1, 2.1.0, 2.2.0rc1, 2.2.0, 2.2.1, 2.3.0rc1, 2.3.0, 2.4.0rc1, 2.4.0, 2.4.1, 2.4.2, 2.5.0rc1, 2.5.0, 2.6.0rc1, 2.6.0, 2.6.1, 2.7.0rc1, 2.7.0, 2.8.0rc1, 2.8.0)
ERROR: No matching distribution found for dnspython==2.9.2
```

### Update Python version

Using dnspython 2.9.2 requires at least python 3.11, so it was necessary to update python to 3.11.

```bash
deactivate
pyenv install 3.11.15
pyenv local 3.11.15
```

### Recreate venv

```bash
rm -rf venv
python -m venv venv
source venv/bin/activate
```

### Reinstall dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
pip check
```

### Update python version in test.yml

```yml
python-version: '3.11'
```

### Update python version in dockerfile

```dockerfile
FROM python:3.11-slim
```

### Fix stuck setuptools version

**Add to aquasecurity/trivy-action in test.yml**

```yml
skip-files: '**/pip/_vendor/bom.cdx.json'
```

**Commit and push**


## Test Error on 'Run tests'

```text
FAILED tests/integration/test_user_auth.py::test_password_hashing - TypeError: duplicate base class TimeoutError
FAILED tests/integration/test_user_auth.py::test_user_registration - TypeError: duplicate base class TimeoutError
FAILED tests/integration/test_user_auth.py::test_duplicate_user_registration - TypeError: duplicate base class TimeoutError
FAILED tests/integration/test_user_auth.py::test_user_authentication - TypeError: duplicate base class TimeoutError
FAILED tests/integration/test_user_auth.py::test_user_last_login_update - TypeError: duplicate base class TimeoutError
FAILED tests/integration/test_user_auth.py::test_unique_email_username - TypeError: duplicate base class TimeoutError
FAILED tests/integration/test_user_auth.py::test_token_creation_and_verification - TypeError: duplicate base class TimeoutError
FAILED tests/integration/test_user_auth.py::test_authenticate_with_email - TypeError: duplicate base class TimeoutError
```

### Replace aioredis with redis

```ini
# remove aioredis==2.0.1
redis==8.2.0
```

```bash
pip uninstall aioredis
pip install redis
```

### Update import in redis.py

```py
import redis.asyncio as aioredis
```

**Commit and push**


# Pushing to Dockerhub

**Create docker repo, then create and copy docker username and token to github secret keys**

## Building docker image

### Remove any currently running docker image
```bash
docker compose down -v
```

### Update postgres image version to 17 in test.yml

```yml
image: postgres:17
```

### Build docker image
```bash
docker compose up --build -d
```

**Update tags in test.yml to match docker repo**

### Tag and push to docker repo
```bash
docker tag a12-web:latest dbal7/fais601_m12:latest

docker push dbal7/fais601_m12:latest
```

### Update docker tags in test.yml

**Final push to github**