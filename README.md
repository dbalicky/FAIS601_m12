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

## Setting up venv and github repo

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
