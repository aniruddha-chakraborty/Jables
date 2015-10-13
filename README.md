# Jables - BETA
> For Laravel 5

Write your Database Schema in JSON, with clean naming conventions and store them nicely in Version Control.

# Features
- [x] Schema in JSON
- [x] Laravel Integration
- [x] All Laravel Field Types supported.
- [x] Checking JSON Syntax
- [x] Checking Foreign key References.
- [x] Checking Unique key Constraints.
- [x] Automatic Table Deconstruction
- [ ] JSON Syntax Prettifyer
- [ ] Schema Diff (build changes not complete reconstructions)
- [ ] Automatic Documentation Builder
- [ ] JSON to Migration Transpiler

# Installation
Grab it through Composer.
```
composer require hedronium/jables
```

You must add Jables to Laravel as a service provider. To do this, open up your `config/app.php` and add `hedronium\Jables\JablesServiceProvider` to the Service Providers list. Like:

```PHP
'providers' => [
    // .... other providers and stuff ...
    hedronium\Jables\JablesServiceProvider::class
]
```

## Testing Installation
On the command line run
```
php artisan
```
and check to see if the `jables` command and the `jables` section shows up.

# Usage
## Commands
`php artisan jables` - Checks your JSON files then creates your Database Tables  
`php artisan jables:check` - Checks your JSON files and reports errors.  
`php artisan jables:refresh` - Destroys all the tables then recreates them from your (possibly updated) JSON files. (warning. risk of data loss)  
`php artisan jables:destroy` - Removes all the tables that jables created from  Database.
`php artisan jables:create-table` - Creates Jables own tracking table in database.  

All the commands accept the  `--database=[connection]` option. You can use it to override which connection Jables uses to do its business.

example
```
php artisan jables --database=memory
```

## Writing Schemas
The Schema files are usually stored in the `database/jables` (you need to create it) folder unless you configure it to be otherwise. The Filename **is** your table name. So if you were to create a `users` table, your file name would be `users.json` nested under `database/jables`

### Getting Started

`food.json`

```JSON
{
    "fields": {
        "name": {
            "type": "string"
        }
    }
}
```

now run `php artisan jables`. This will create a table named `food` with a field named `name` with the type being `varchar`.

### The Formal Breakdown
Well you define all your fields in the `fields` property on your root object of your json file. The `fields` property itself is an object and every property of the `fields` object corresponds to a table field.

Each property (which are the field definitions) within the `fields` object is once again another object. The only hard requirement for it all is the `type` property on them. This tells jables what is the type of the field.

in our 'hello world' example the type of `string` corresponds to `varchar` just like in Laravel migrations (Jables uses Laravel's [Schema Builder](http://laravel.com/docs/5.1/migrations)).

## Types Available
Heres a list

- [big-integer](#big-integer)
- [binary](#binary)
- [boolean](#boolean)
- [char](#char)
- [date](#date)
- [date-time](#date-time)
- [decimal](#decimal)
- [double](#double)
- [enum](#enum)
- [float](#float)
- [integer](#integer)
- [json](#json)
- [jsonb](#jsonb)
- [long-text](#long-text)
- [medium-integer](#medium-integer)
- [medium-text](#medium-text)
- [morphs](#morphs)
- [small-integer](#small-integer)
- [string](#string)
- [text](#text)
- [time](#time)
- [tiny-integer](#tiny-integer)
- [timestamp](#timestamp)

### integer
```JSON
"awesomeness": {
    "type": "integer"
}
```

You can write `attributes` which is a list. Currently only supports one attribute, the `unsigned` attribute

like...
```JSON
{
    "type": "integer",
    "attributes": [
        "unsigned"
    ]
}
```

You can only set it to auto-increment with the `ai` (`boolean`) property like...
```JSON
{
    "type": "integer",
    "ai" : true
}
```

### big-integer
Same as the average integer just write the type different like...
```JSON
{
    "type": "big-integer"
}
```

### medium-integer
Same as the average integer just write the type different like...
```JSON
{
    "type": "medium-integer"
}
```

### small-integer
Same as the average integer just write the type different like...
```JSON
{
    "type": "small-integer"
}
```

### tiny-integer
Same as the average integer just write the type different like...
```JSON
{
    "type": "tiny-integer"
}
```

### float
The `FLOAT` type equivalent.
```JSON
{
    "type": "float"
}
```

### double
The `DOUBLE` type equivalent. It requires you to set the `digits` & `precision` properties.

```JSON
{
    "type": "double",
    "digits": 10,
    "precision": 5
}
```

### decimal
The `DECIMAL` type. Properties same as `double`.

```JSON
{
    "type": "decimal",
    "digits": 10,
    "precision": 5
}
```

### string
`string` is the `VARCHAR` type, and it accepts a `length` property like...

```JSON
{
    "type": "string",
    "length": 50
}
```

but the `length` property isn't required.

### char
Its exactly like string it just uses the `CHAR` type but the `length` property is absolutely required.

```JSON
{
    "type": "char",
    "length": 10
}
```


### text
Text doesn't require any special properties.

```JSON
{
    "type": "text"
}
```

### long-text
Same as `text`.
```JSON
{
    "type": "long-text"
}
```

### medium-text
Same as `text`.
```JSON
{
    "type": "medium-text"
}
```

### date
No Special Properties.
```JSON
{
    "type": "date"
}
```

### time
No Special Properties.
```JSON
{
    "type": "time"
}
```

### date-time
No Special Properties.
```JSON
{
    "type": "date-time"
}
```

### timestamp
No Special Properties.
```JSON
{
    "type": "timestamp"
}
```

### enum
for the `ENUM` type. It is required that you set the `values`(`list`) property.
```JSON
{
    "type": "enum",
    "values": ["wizard", "muggle"]
}
```

### boolean
No special properties.
```JSON
{
    "type": "boolean"
}
```

### json
No special properties.
```JSON
{
    "type": "json"
}
```

### jsonb
No special properties.
```JSON
{
    "type": "jsonb"
}
```

### morphs
No special properties.
```JSON
{
    "type": "morphs"
}
```

### binary
No special properties.
```JSON
{
    "type": "binary"
}
```

## Timestamps
Just like in Schema Builder, you can create the two fields `created_at` and `updated_at` in a simple way.

Create a special `timestamps` property in yours `fields` object and set it to true.

Like:
```JSON
{
    "fields": {
        "user_id": {
            "type": "integer",
            "attributes": [
                "unsigned"
            ]
        },
        "burger_id": {
            "type": "integer",
            "attributes": [
                "unsigned"
            ]
        },
        "timestamps": true
    }
}
```

## Default Values
All field definitions accept the `default` property for when you want to set the default value of a field.

Used like...
```JSON
{
    "type": "string",
    "default": "cake"
}
```

## Nullable Fields
All field definitions accept the `nullable`(`boolean`) property. If set to true, the field can be left null.

Used like...
```JSON
{
    "type": "string",
    "nullable": true
}
```

## Primary Keys
if you set the `ai` to true on a `integer` type or similar field. That field automatically becomes the primary key (its a Laravel thing).

Apart from that, you can set the `primary` property on any field to true like...
```JSON
{
    "type": "string",
    "primary": true
}
```

### Composite Primary Keys
More that one field makes your primary key? No Problem! Just create a `primary`(`list`) property on your root object (sibling to your `fields` property) like...

```JSON
{
    "fields": {
        "region": {
            "type": "string"
        },
        "house": {
            "type": "string"
        }
    },
    "primary": ["region", "house"]
}
```

## Unique Constraints
All field definitions accept the `unique` property. set it to `true` to make it an unique field like...

```JSON
{
    "type": "string",
    "length": 20,
    "unique": true
}
```

### Composite Unique Constraint
You can created unique constraints across many fields. Just create a `unique`(`list`) property on your root object (sibling to your `fields` property) like...

```JSON
{
    "fields": {
        "region": {
            "type": "string"
        },
        "house": {
            "type": "string"
        }
    },
    "unique": [
        ["region", "house"]
    ]
}
```

Yes, it is a list inside a list, for when you want multiple composite constraints.

## Foreign Key Constraints
Got you covered! All fields accept the `foreign` property. You can set it to a string containing the name of the table and the name of the field of that table separated by a dot. (eg. `users.id`)

```JSON
"user_id": {
    "type": "integer",
    "attributes": [
        "unsigned"
    ],
    "foreign": "users.id"
}
```

this `user_id` field will now reference `id` on the `users` table.

You could also define them like you define unique constraints like...
```JSON
{
    "fields": {
        "user_id": {
            "type": "integer",
            "attributes": [
                "unsigned"
            ]
        },
        "burger_id": {
            "type": "integer",
            "attributes": [
                "unsigned"
            ]
        }
    },
    "foreign": {
        "user_id": "users.id",
        "burger_id": "burgers.id"
    }
}
```
This will work totally fine.

# Documenting Tables
All field and table definitions accept `title` and `description` properties that can be used to document your database schema.

We're also working on a feature that generates HTML documentation from your JSON files but for now, you gotta create it on your own. (Maybe even send us a pull request. PLEASE. WE'RE DESPERATE.)

example:
```JSON
{
    "title": "Food",
    "description": "FOOOD! GLORIOUS FOOD!",
    "fields": {
        "name": {
            "type": "string",
            "title": "Name",
            "description": "Name of the food."
        }
    }
}
```

# Configuration
Jables usually works right out of the box with no configuration required, but two option does exist.

First publish the configuration files. with
```
php artisan vendor:publish
```

after running that a `jables.php` should show up in your `config` folder with the following contents...

```PHP
<?php
return [
    'table' => 'jables',
    'folder' => 'jables'
];
```

## Options
- `table` - The name of the special table jables creates for tracking.
- `folder` - The name of the folder within which you store your table schemas. The name is relative to your Laravel installation's `database` folder.

> It is recommend that you store your schema in a folder within your application's `database` folder.
