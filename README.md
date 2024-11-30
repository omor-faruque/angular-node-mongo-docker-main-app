# docker compose for Two Separate Repositories

- repo 1 (frontend): https://github.com/omor-faruque/angular-universal-docker.git 
- repo 2 (backend): https://github.com/omor-faruque/angular-universal-docker-backend.git 

## Using a Parent Repository to Coordinate Both Repos

### OPTION 1

** Instructions **

- Clone the repositories into a parent directory:
```sh
mkdir project-root
cd project-root
git submodule add https://github.com/your-username/frontend-repo frontend
git submodule add https://github.com/your-username/backend-repo backend

```

- Add a docker-compose.yml file: Create a docker-compose.yml file in the project-root directory to manage both the frontend and backend services:

```sh
version: '3.8'

services:
  frontend:
    build:
      context: ./frontend
    ports:
      - "4200:4200"
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
    ports:
      - "3000:3000"
    environment:
      - DB_HOST=mongo
      - DB_PORT=27017
    depends_on:
      - mongo

  mongo:
    image: mongo:6.0
    container_name: mongo
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:

```
- Run Docker Compose: Run `docker-compose up --build` or `docker compose up --build` from the project-root directory. This will build and start both the frontend and backend services as well as the MongoDB container.


### OPTION 2

- Clone Repositories Individually and Run Docker Compose Locally

** command **
- Fetch Latest Changes in Submodules: To fetch the latest changes from the submodule repositories:
`git submodule update --remote --merge`