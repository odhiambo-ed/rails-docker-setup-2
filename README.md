# Rails App with PostgreSQL and Docker

## Description

> This repository contains the source code for a Ruby on Rails application configured to use PostgreSQL as the database and run within Docker containers.


## Prerequisites

Before you begin, ensure you have the following installed on your machine:

- Docker
- Docker Compose

## Getting Started

### Step 1: Create a New Rails Application

```
rails new myapp --database=postgresql
cd myapp
```

### Step 2: Add PostgreSQL as the Database
```
# Gemfile
gem 'pg', '>= 0.18', '< 2.0'
```

```
bundle install
```

### Step 3: Install the dockerfile-rails Gem

```
# Gemfile
group :development do
  gem 'dockerfile-rails', '~> 1.6', '>= 1.6.17'
end
```
```
bundle install
```

### Step 4: Generate Docker and Docker Compose Files

```
bin/rails generate dockerfile --compose --postgresql
```

### Step 5: Set Up the .env File

```
touch .env
```

Add the following content to the .env file:

```
POSTGRES_USER=postgres
POSTGRES_PASSWORD=yourpassword
POSTGRES_DB=myapp_development
DATABASE_URL=postgres://postgres:yourpassword@db:5432/myapp_development
```

### Step 6: Build and Run the Docker Containers

```
docker-compose build
docker-compose up
```

### Final Steps

```
docker-compose run web rake db:create db:migrate
```

## Generate a Secret Key Base:You can generate a new secret key base using the Rails command:

```
rails secret
```

#### docker-compose.yml

```
version: '3.8'
services:
  db:
    image: postgres:latest
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: ${POSTGRES_DB}

  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bin/rails server -b 0.0.0.0"
    volumes:
      - .:/third
    ports:
      - "3000:3000"
    depends_on:
      - db
    environment:
      DATABASE_URL: ${DATABASE_URL}
      SECRET_KEY_BASE: "your_generated_secret_key_base_here"

volumes:
  postgres_data:

```


## Author(s)

  <a href="https://github.com/odhiambo-ed" target="blank"><img align="center"
        src="https://github.com/white3d/GitHub-User-Content/blob/main/Passport_Ed-M.png"
        alt="Edward" height="80" width="80"/></a>   **Edward Odhiambo**

- GitHub: [@whit3d](https://github.com/odhiambo-ed)
- Twitter: [@odhiambo_ed](https://twitter.com/odhiambo_ed)
- LinkedIn: [Edward Odhiambo](https://www.linkedin.com/in/edward-odhiambo/)
- Portfolio: [Edward Odhiambo](https://edwardodhiambo.com/)

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!!!

Feel free to check the [issues page]https://github.com/odhiambo-ed/rails-docker-setup-2/issues)

## Show your support

Give a ⭐️ if you like this project!

## Acknowledgments

- Hat tip to anyone whose code was used

## 📝 License

This project is [MIT](https://github.com/white3d/GitHub-User-Content/blob/main/LICENSE) licensed.

