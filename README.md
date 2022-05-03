
# Homework for lesson 1. PostgreSQL cloud solutions.

### 1. Created virtual machine in yandex cloud:![virtual_machine](https://user-images.githubusercontent.com/104854113/166566290-65cff7c9-7ff7-48de-b567-10c78bda07b6.png)

### 2. Install postgres-14:
```
  wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - &&
  echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" | sudo tee /etc/apt/sources.list.d/postgresql-pgdg.list > /dev/null &&
  sudo apt update &&
  sudo DEBIAN_FRONTEND=noninteractive apt -y install postgresql-14 &&
  sudo systemctl status postgresql
```
  [![asciicast](https://asciinema.org/a/4K9Cx0HWZ6l2Xv0ijxfthpHy0.svg)](https://asciinema.org/a/4K9Cx0HWZ6l2Xv0ijxfthpHy0)
  
### 3. First task. Created initial data and examine 'read committed' isolation level.
  -- first terminal
  [![asciicast](https://asciinema.org/a/g3fBDzzQ7CR9Mco5qn1MMH4JQ.svg)](https://asciinema.org/a/g3fBDzzQ7CR9Mco5qn1MMH4JQ)
  -- second terminal
  [![asciicast](https://asciinema.org/a/tqDPGJdTAZgt3RbR6PcbbUcrP.svg)](https://asciinema.org/a/tqDPGJdTAZgt3RbR6PcbbUcrP)
  Results:
    In second terminal we can see new record only after commit in first terminal. SELECT query (without a FOR UPDATE/SHARE clause) sees only data committed before the query began. However, SELECT does see the effects of previous updates executed within its own transaction, even though they are not yet committed. (postgres documentation 'https://postgrespro.com/docs/postgresql/14/transaction-iso#XACT-READ-COMMITTED' :sunglasses: )
### 4. Second task. Examine 'repeatable read' isolation level.
  -- first terminal
  [![asciicast](https://asciinema.org/a/2qwJopaAn8a0s6OX3tWguTpFk.svg)](https://asciinema.org/a/2qwJopaAn8a0s6OX3tWguTpFk)
  --second terminal
  [![asciicast](https://asciinema.org/a/oN9ZbSyzCzj9oopQ7updvXhFk.svg)](https://asciinema.org/a/oN9ZbSyzCzj9oopQ7updvXhFk)
  Results:
    In second terminal we can see new record only after commit in both terminals. Even after commit command in first terminal second transaction cannot access it. The Repeatable Read isolation level only sees data committed before the transaction began. (postgres documentation 'https://postgrespro.com/docs/postgresql/14/transaction-iso#XACT-REPEATABLE-READ' :sunglasses: )
