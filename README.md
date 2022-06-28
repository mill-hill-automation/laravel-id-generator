<h1 align="center">Laravel ID Generator</h1>
<p align="center">
    <a href="https://packagist.org/packages/haruncpi/laravel-id-generator"><img src="https://badgen.net/packagist/v/haruncpi/laravel-id-generator" /></a>
    <a href="https://creativecommons.org/licenses/by/4.0/"><img src="https://badgen.net/badge/licence/CC BY 4.0/23BCCB" /></a>
     <a href=""><img src="https://badgen.net/packagist/dt/haruncpi/laravel-id-generator"/></a>
    <a href="https://twitter.com/laravelarticle"><img src="https://badgen.net/badge/twitter/@laravelarticle/1DA1F2?icon&label" /></a>
    <a href="https://facebook.com/laravelarticle"><img src="https://badgen.net/badge/facebook/laravelarticle/3b5998"/></a>
</p>
<p align="center">Easy way to generate custom ID from database table in Laravel framework</p>

![Image description](preview.png)

## Documentation
# Laravel custom ID generator
---

The unique ID is an important part of our application. Someone like to generate application ID as auto incremental and someone like to generate their unique ID in a custom format.

### **How to use it?**

You can use it in your controller code like as a helper or inside your model by defining a boot method.

**Example with controller**

Import ID generator

```
use Haruncpi\LaravelIdGenerator\IdGenerator;
```

```
public function store(Request $request){

	$id = IdGenerator::generate(['table' => 'todos', 'length' => 6, 'prefix' => date('y')]);

	$todo = new Todo();
	$todo->id = $id;
	$todo->title = $request->get('title');
	$todo->save();

}
```

N.B: If you generate ID to the table `id` field then you must have to set the `id` field as fillable and `public $incrementing = false;` inside your model.

**Example with the model**

In your model add a `boot` method. The ID will automatically generate when a new record will be added.

```
public static function boot()
{
    parent::boot();
    self::creating(function ($model) {
        $model->uuid = IdGenerator::generate(['table' => $this->table, 'length' => 6, 'prefix' =>date('y')]);
    });
}
```

**Parameters**

You must pass an associative array into `generate` function with `table, length` and `prefix` key.

`table` = Your table name.

`field` = Optional. By default, it works on the id field. You can set other field names also.

`length` = Your ID length

`prefix` = Define your prefix. It can be a year prefix, month or any custom letter.

`reset_on_prefix_change` = Optional, default false. If you want to reset id from 1 on prefix change then set it true.

**Example: 01**

```
$config = [
    'table' => 'todos',
    'length' => 6,
    'prefix' => date('y')
];

// now use it
$id = IdGenerator::generate($config);

// use within single line code
$id = IdGenerator::generate(['table' => 'todos', 'length' => 6, 'prefix' => date('y')]);

// output: 160001
```

**Example 02:** INV-000001 for prefix string. Your field must be varchar.

```
$id = IdGenerator::generate(['table' => 'invoices', 'length' => 10, 'prefix' =>'INV-']);
//output: INV-000001
```

**Example 03:** YYMM000001

```
$id = IdGenerator::generate(['table' => 'invoices', 'length' => 10, 'prefix' =>date('ym')]);
//output: 1910000001
```

**Example 04:** By default, this package works on the ID field. You can set another field to generate an ID. Make sure your selected field must be unique and also proper data type.

```
$id = IdGenerator::generate(['table' => 'products','field'=>'pid', 'length' => 6, 'prefix' =>date('P')]);
//output: P00001
```

**Example 05:** By default, this package won't reset your ID when you change your prefix of ID. If you want to reset your ID from 1 on every prefix changes then pass `reset_on_prefix_change => true`

Reset ID yearly

```
$id = IdGenerator::generate(['table' => 'invoices', 'length' => 10, 'prefix' =>date('y')]);
//output: 2000000001,2000000002,2000000003
//output: 2100000001,2100000002,2100000003
```

Reset ID monthly

```****
$id = IdGenerator::generate(['table' => 'invoices', 'length' => 10, 'prefix' =>date('ym')]);
//output: 1912000001,1912000002,1912000003
//output: 2001000001,2001000002,2001000003
```

Or any prefix change

```
$id = IdGenerator::generate(['table' => 'products', 'length' => 6, 'prefix' => $prefix]);
//output: A00001,A00002,B00001,B00002
```

## Other Packages
- [Laravel Log Reader](https://github.com/haruncpi/laravel-log-reader) - A simple and beautiful laravel log reader.
- [Laravel H](https://github.com/haruncpi/laravel-h) - A helper package for Laravel Framework.
- [Laravel Simple Filemanager](https://github.com/haruncpi/laravel-simple-filemanager) - A simple filemanager for Laravel.
- [Laravel Option Framework](https://github.com/haruncpi/laravel-option-framework) - Option framework for Laravel.
