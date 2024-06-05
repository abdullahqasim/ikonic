# Ikonic feedback

This is Ikonic feedback system

## Installation

Clone the Project:

```bash
git clone git@github.com:abdullahqasim/ikonic.git
```

Go into the project root directory
    ```bash
    cd ikonic
    ```

# Laravel

Go into the project backend directory
    ```bash
    cd ikonic-backend
    ```

Install composer:

```bash
composer install
```

Create .env file:

```bash
cp .env.example .env
```

```bash
php artisan key:generate
```

Migrate database (Note: connect your project with database first then run the following command)

```bash
php artisan migrate
```

Seed the database to create user

```bash
php artisan db:seed
```

Run server

```bash
php artisan serve
```

# Vue

change directory

```bash
cd ikonic-frontend
```

Install dependencies:

```bash
npm install
```

To launch the project

```bash
npm run dev
```
