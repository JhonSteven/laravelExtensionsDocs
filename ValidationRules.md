# Validation Rules Package

Validation Rules Package, is a package for PHP to generate **RULES** in model, especially for [Laravel Framework](https://laravel.com/). Created by [ParraWeb](https://parraweb.com/)

### Create Rules in Models!
  - Create rules for validation in models, is an easy way to reuse them without creating more files.

## Installation

Validation Rules requires [PHP](https://php.net/) v 7.0+ to run.

Install the dependencies and devDependencies and start the server.

```sh
$ composer require parraweb/validation-rules --dev
```

## Usage

### Create rules in model

In dependencies of model:
```php
use ParraWeb\ValidationRules; 
```

Then put in Traits to use, like SoftDeletes:
```php
use ValidationRules;
```

Example in User Model:
```php
<?php
...
use ParraWeb\ValidationRules;

class User extends Authenticatable
{
    use ValidationRules;
    ...
}
```

Final Example in model:
```php
<?php
...
use ParraWeb\ValidationRules;

class User extends Authenticatable
{
    use ValidationRules;
    ...
    protected $rules = [
        'post,put' => [
            'name' => 'required|min:3',
            'email' => 'required|email',
            'role_id' => 'required|exists:countries,id',
        ],
        'delete,put' => [
            'id' => 'required|exists:users,id'
        ],
        'user_roles' => [
            'role' => 'required|string|min:2'
        ],
    ];
    ...
}
```
The first key of each correspond to methods, accepted methods are **"post, put, delete, get"**, they correspond to request()->method(), patch is replaced by put, also you can put custom methods like **"users_roles"** for specific $rules, you must separate methods by comma.


### Use rules in controller

You can use $rules in controller in differente ways:

1. Without parameters, the default method is request()->method(), "Is the method in route"

```php
class UserController extends Controller
{
    ...
    public function store(Request $request)
    {
        $request->validate(User::rules());        
        ....
        ..
    }
}
```
2. With parameter, the word 'post' in next example references to before text in validation, and the default method is request()->method(), "Is the method in route"

```php
class PostController extends Controller
{
    ...
    public function store(Request $request)
    {
        $request->validate(User::rules('article.user'));        
        ....
        ..
    }
}
```
Before example is the same way to write

```php
class PostController extends Controller
{
    ...
    public function store(Request $request)
    {
        $request->validate([
            'article.user.name' => 'required|min:3',
            'article.user.email' => 'required|email'
        ]);      
        ....
        ..
    }
}
```
3. Changing request()->method(), the parameter must be an array ['post'] or ['my_custom_rules'], if you only put post like string 'post', it will be before text like before way number 2.

```php
class PostController extends Controller
{
    ...
    public function store(Request $request)
    {
        $request->validate(User::rules(['post']));        
        ....
        ..
    }
}
```
4. Changing request()->method() and put before text
```php
class PostController extends Controller
{
    ...
    public function store(Request $request)
    {
        $request->validate(User::rules('before_text','method'));        
        ....
        ..
    }
}
```
5. Validate several models in same, you must merge array rules
```php
class PostController extends Controller
{
    ...
    public function store(Request $request)
    {
        $request->validate(array_merge(User::rules(['post']),Article::rules('article','post')));      
        ....
        ..
    }
}
```

You can use it with Validator dependency of Laravel too. Remember that this package generate array rules.

### Developing

If you want to contribuite, write to my email jhonstevenparrapena@gmail.com

License
----

MIT

**Made by [ParraWeb.com](https://www.parraweb.com)**