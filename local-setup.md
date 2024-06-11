# Backstage Local setup

Backstage requires the following dependencies to function:

- Git for desktop
- Node Version Manager (nvm)
- Node Package Manager (npm)
- Yarn
- Docker (Initially, it is not necessary)

## Install Git

Install Git using the link below:

[Download Git](https://git-scm.com/download/win)

## Install nvm

Install nvm using the link below:

[Download nvm for Windows](https://github.com/coreybutler/nvm-windows/releases)

Download and run any one of them:
- `nvm-setup.zip`
- `nvm-setup.exe`

## Configure nvm, Node.js, and npm

Open Git Bash in a new folder and run the following commands. (Versions may vary over time)

### Install Node.js

```sh
nvm install version_number
# Example:
nvm install 18.14.0
```

### Use the Installed Node.js Version

```sh
nvm use version_number
# Example:
nvm use 18.14.0
```

### Check Node.js and npm Installation

```sh
node -v
npm -v
```

## Install Yarn

```sh
npm install --global yarn
```

## Clone and Install Backstage

### Clone the Repository

```sh
# Start from your local development folder
git clone --depth 1 https://github.com/backstage/backstage.git
cd backstage
```

### Install Dependencies

```sh
yarn install
```

### Launch Backstage

```sh
# From your Backstage root directory, launch both the frontend and backend
yarn dev
```

## Install Postgres

Install Postgres from the link below:

[Download Postgres](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)

After installing, create your password and add it to the `app-config` file.

## Configure app-config File

In the `app-config` file inside the `database` section, replace the content with the following (maintain the indentation):

```yaml
database:
  client: 'pg'
  connection:
    host: 'localhost'
    port: 5432
    user: 'your_postgres_user'
    password: 'your_postgres_password'
    database: 'your_database_name'
```

Replace `your_postgres_user`, `your_postgres_password`, and `your_database_name` with your actual Postgres user, password, and database name.