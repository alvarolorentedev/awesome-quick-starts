docker-compose.yml

```yml
version: '2'

services:
  postgress:
    image: postgres
    ports:
      -  '5432:5432'
  myApp:
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ./scripts/wait-for-it.sh:/usr/local/bin/wait-for-it.sh
    command: ["wait-for-it.sh", "postgress:5432","-t","100", "--", "java", "-jar", "app.jar"]
    depends_on:
      - postgress
```