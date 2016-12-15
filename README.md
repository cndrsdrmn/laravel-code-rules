# Laravel Code Rules

This is my code rules when I code in [Laravel](https://laravel.com). All the rules here are based on my experiences and from online resources, especially [Laracasts](https://laracasts.com)!

## Prerequisites

- Follows the [Laravel coding style](https://laravel.com/docs/contributions#coding-style) with the [documentation block](https://laravel.com/docs/contributions#phpdoc)
- Learn SOLID Principles

## Simple Rules for Simpler Code

- No abbreviations
```php
$addr = 'Jalan Cikajang no. 17'; // Don't
$address = 'Jalan Cikajang no. 17'; // Do
```
- Don't use `else` when your codes have early returns
```php
// Don't
if ($condition) {
    return $this->doThis();
} else {
    return $this->doThat();
}

// Do
if ($condition) {
    return $this->doThisThing();
}

return $this->doThat();
```
- One level of indentation
```php
// Don't
public function store(Request $request)
{
    //

    if ($condition1) { // Level 0
        // Level 1

        while ($condition2) {
            // Level 2
        }
    }
}

// Do
public function store(Request $request)
{
    //

    if ($condition1) { // Level 0
        // Level 1

        $this->runWhile($condition2);
    }
}

protected function runWhile($condition)
{
    // Level 0
    while ($condition) {
        // Level 1
    }
}
```
- Limit your instance variables (max. 2)
```php
// Don't
class UserController
{
    protected $user;

    protected $post;

    protected $maps;

    public function __construct(User $user, Post $post, Maps $maps)
    {
        $this->user = $user;
        $this->post = $post;
        $this->maps = $maps;
    }
}

// Do
class UserController
{
    protected $user;

    protected $maps;

    public function __construct(UserRepository $user, Maps $maps)
    {
        $this->user = $user;
        $this->maps = $maps;
    }
}

class UserRepository
{
    protected $user;

    protected $post;

    public function __construct(User $user, Post $post)
    {
        $this->user = $user;
        $this->post = $post;
    }
}
```
- Wrap primitives (sometimes)
```php
// Don't
class Performance extends Model
{
    public function revenue()
    {
        return $this->revenue; // ex: 8800
    }

    public function revenueInDollars()
    {
        return $this->revenue / 100; // ex: 88
    }

    public function revenueAsCurrency()
    {
        return money_format('$%i', $this->revenueInDollars()); // $88.00
    }
}

// Do
class Performance extends Model
{
    public function revenue()
    {
        return new Revenue($this->revenue);
    }
}

class Revenue
{
    protected $revenue;

    public function __construct($revenue)
    {
        $this->revenue = $revenue;
    }

    public function inDollars()
    {
        return $this->revenue / 100;
    }

    public function asCurrency()
    {
        return money_format('$%i', $this->inDollars());
    }

    public function __toString()
    {
        return (string) $this->revenue;
    }
}
```
