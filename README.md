# Laravel v10

This project has been created to experiment with the **Laravel version 10**.

## Packages

The following packages have been used:

- [Laravel Breeze](https://jetstream.laravel.com/2.x/introduction.html) - Authentication starter kit. Livewire
  version, including Teams.

### Dev Tooling

- [Easy Coding Standard (ECS)](https://github.com/symplify/easy-coding-standard) - Preferred coding standard for this
  project, set to PSR-12 plus other standards. Pint is also an option for coding standards.
- [Larastan](https://github.com/nunomaduro/larastan) - Static analysis for Laravel using PhpStan.
- [Parallel-Lint](https://github.com/php-parallel-lint/PHP-Parallel-Lint) - This application checks syntax of PHP files
  in parallel
- [IDE helper](https://github.com/barryvdh/laravel-ide-helper) - helper for IDE auto-completion
- [Laravel debug bar](https://github.com/barryvdh/laravel-debugbar) - debug bar for views, shows models, db calls etc.
- [GrumPHP](https://github.com/phpro/grumphp) - pre-commit hook to run the above tools before committing code

## Requirements

This is a Laravel 9 project. The requirements are the same as a
new [Laravel 10 project](https://laravel.com/docs/master).

- [8.1+](https://www.php.net/downloads.php)
- [Composer](https://getcomposer.org)

Recommended:

- [Git](https://git-scm.com/downloads)

## Clone

Clone the project repository

e.g.

```sh
git clone git@gitlab.com:heiw/laravel/laravel-v10.git
```

## Install

Install all the dependencies using composer

```sh
cd laravel-v10
composer install
```

## Create .env

Create an `.env` file from `.env.example`

```shell script
cp .env.example .env
```

## Configure Laravel

This project uses models and seeders to generate the tables for the database. Tests will use the seeded data. Configure
the Laravel **.env** file with the **database**, updating **username** and**password** as per you local setup.

```text
APP_NAME="laravel-v10"

APP_URL=https://laravel-v10.test

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel_v10
DB_USERNAME=YourDatabaseUserName (root)
DB_PASSWORD=YourDatabaseUserPassword

MAIL_MAILER=log
```

## Generate APP_KEY

Generate an APP_KEY using the artisan command

```shell script
php artisan key:generate
```

## Create the database

The database will need to be manually created e.g.

```shell
mysql -u YourDatabaseUserName (root)
CREATE DATABASE laravel_v10 CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
exit
```

## Install Database

This project uses models and seeders to generate the tables for the database. Tests will use the seeded data.

```shell
php artisan migrate --seed
# or if previously migrated: 
php artisan migrate:fresh --seed 
```

## Vite

The first time you pull the project run install:

```shell
npm install
```

Compile your CSS / JavaScript for development and recompile on change:

```shell
npm run dev
```

When ready to deploy, compile your CSS / JavaScript for production:

```shell
npm run build
```

## Run tests

To make it easy to run all the PHPUnit tests a composer script has been created in **composer.json**. From the root of
the projects, run:

```shell script
composer tests
```

You should see the results in testDoc format:

```text
PHPUnit 9.5.28 by Sebastian Bergmann and contributors.

Example (Tests\Unit\Example)
 ✔ That true is true

Authentication (Tests\Feature\Auth\Authentication)
 ✔ Login screen can be rendered
 ✔ Users can authenticate using the login screen
 ✔ Users can not authenticate with invalid password
 ✔ Users can logout

Email Verification (Tests\Feature\Auth\EmailVerification)
 ✔ Email verification screen can be rendered
 ✔ Email can be verified
 ✔ Email verification notification can be sent
 ✔ Email is not verified with invalid hash

Password Confirmation (Tests\Feature\Auth\PasswordConfirmation)
 ✔ Confirm password screen can be rendered
 ✔ Password can be confirmed
 ✔ Password is not confirmed with invalid password

Password Reset (Tests\Feature\Auth\PasswordReset)
 ✔ Reset password link screen can be rendered
 ✔ Reset password link can be requested
 ✔ Reset password screen can be rendered
 ✔ Password can be reset with valid token

Password Update (Tests\Feature\Auth\PasswordUpdate)
 ✔ Password can be updated
 ✔ Correct password must be provided to update password

Registration (Tests\Feature\Auth\Registration)
 ✔ Registration screen can be rendered
 ✔ New users can register

Example (Tests\Feature\Example)
 ✔ The application returns a successful response

Profile (Tests\Feature\Profile)
 ✔ Profile page is displayed
 ✔ Profile information can be updated
 ✔ Email verification status is unchanged when the email address is unchanged
 ✔ User can delete their account
 ✔ Correct password must be provided to delete account

Time: 00:04.148, Memory: 46.00 MB

OK (26 tests, 67 assertions)
```

Easy Coding Standard (ECS) is used to check for style and code standards, [PSR-12](https://www.php-fig.org/psr/psr-12/)
is used. Regularly run code standard checks to automatically clean up your code. In particular run before committing any
code.

To make it easy to run Easy Coding Standard (ECS) a composer script has been created in **composer.json**. From the root
of the projects, run:

```shell script
composer check-cs
```

You should see the results:

```text
 [OK] No errors found. Great job - your code is shiny in style!
```

If there are any warning, ECS will advise you to run --fix to fix them, this also has a composer script:

```shell
composer fix-cs
```

Sometimes the fix command needs to be run several times, as one fix will identify more problems, keep running the fix-cs
until you get the OK message.

## Static Analysis

PhpStan is used to run static analysis checks. Larastan has been installed, which is PhpStan and Laravel rules.
Regularly run static analysis checks to help identify problems. In particular run before committing any code.

To make it easy to run PhpStan a composer script has been created in **composer.json**. From the root of the project
run:

```shell script
composer phpstan
```

You should see the results:

```text

 [OK] No errors

```

If PhpStan identifies any problems then review and fix them one by one. See
the [documentation for help](https://phpstan.org/user-guide/getting-started).

### PhpStan Known problems

PhpStan has a dislike for translation strings! There will often a case, when using Filament, that the string being
translated **_may_** return mixed. This is due to the way the translation function **\__('string to be translated')**
works.

- If you pass the function **null**, it will return **null**.
- If you pass a **string** it will return a **string**.
- If you pass an **array** it will return an **array**.

Filament 'labels' will expect a string or closure. As we are passing in a **string** we will get a **string** back,
unfortunately PhpStan doesn't understand, the doc block states the return time is **mixed**.

Typical error message:

```text
Parameter #1 $label of static method Filament\\Widgets\\StatsOverviewWidget\\Card::make() expects string, mixed given
```

The fix is to ignore this, type of error:

```shell
composer phpstan-baseline
```

This will generate a **phpstan-baseline.neon** file, which should be added to version control. Only run this commend
once other PhpStan errors have been fixed!

## Commit hook

[GrumPHP](https://github.com/phpro/grumphp) has been installed and configured to run a pre-commit hook, when you
`git commit` any code ECS, PhpStan and PHPUnit will be automatically run, if any of these fail the commit will be
rejected. You can always write a rule to bypass the failing code, but it is better to fix the problem.

## IDE Helper

- [Laravel IDE Helper Generator](https://github.com/barryvdh/laravel-ide-helper)

To help autocompletion in PhpStorm laravel IDE helper has been installed, to update models run:

```shell
composer ide
```

## Contributing

This is a **personal project**. Contributions are not required. Anyone interested in developing this project are welcome
to fork or clone for your own use.

## Credits

- Michael Pritchard (AKA Pen-y-Fan).

## Troubleshooting

The minimum required version of PHP is 8.1.0, if you try to run command lines with a lower PHP version you will receive
an error advising the minimum version. The workaround is to temporarily set the PATH to the version of PHP:

```shell
php -v
# PHP 7.4.19 (cli) (built: May  4 2021 14:24:38) ( ZTS Visual C++ 2017 x64 )
set PATH=C:\laragon\bin\php\php-8.1.10-Win32-vs16-x64\;%PATH%
php -v
# PHP 8.1.10 (cli) (built: Aug 30 2022 18:05:49) (ZTS Visual C++ 2019 x64)
```
