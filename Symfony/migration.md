// main command
php bin/console




// Create or Edit Entity
php bin/console make:entity EntityName


// create migration file
php bin/console make:migration

// migration
php bin/console doctrine:migrations:list
php bin/console doctrine:migrations:migrate


// clear-cache
php bin/console cache:clear
// or
php bin/console c:c

// list all command
php bin/console list

// doctrine
php bin/console doctrine:cache:clear-metadata
php bin/console doctrine:schema:validate

php bin/console debug:autowiring
php bin/console d:a

php bin/console debug:config
php bin/console d:c

php bin/console debug:event-dispatcher
php bin/console d:e

php bin/console debug:messenger
php bin/console d:m

php bin/console debug:router
php bin/console d:r

php bin/console debug:twig
php bin/console d:t




