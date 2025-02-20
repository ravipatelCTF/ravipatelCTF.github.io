---
title: "How to build a RESTful API using PHP, Laravel and SQLite database."
seoTitle: "Build RESTful API with PHP, Laravel & SQLite"
seoDescription: "Learn to build a RESTful API using PHP, Laravel, and SQLite with a step-by-step guide on creating, designing, and seeding databases"
datePublished: Thu Feb 20 2025 03:02:38 GMT+0000 (Coordinated Universal Time)
cuid: cm7crbblf000109l71v0cgimu
slug: how-to-build-a-restful-api-using-php-laravel-and-sqlite-database
tags: laravel, php, rest-api, sqlite, build-in-public

---

### Step 1 : Getting Started

#### Creating The Project :

* Using Laravel Herd `$ laravel new hometutornearme`
    

#### Designing and Seeding the Database :

`$ php artisan make:model Customer --all` `$ php artisan make:model Invoice --all`

```php
// app>Models>Customer.php

public function invoices() {
	return $this->hasMany(Invoice::class);
}
```

```php
// app>Models>Invoice.php

public function customer() {
	return $this->belongsTo(Customer::class);
}
```

```php
/**
*database\migrations\2025_02_14_071343_create_customers_table.php
*/

// wbrs
$table->string('name');
$table->string('type'); // Individual or business
$table->string('email');
$table->string('address');
$table->string('city');
$table->string('state');
$table->string('postal_code');
// wbrc
```

```php
/**
*database\migrations\2025_02_14_071407_create_invoices_table.php
*/

// wbrs
$table->integer('customer_id');
$table->integer('amount');
$table->string('status'); // Billed , Paid, Void
$table->dateTime('billed_date');
$table->dateTime('paid_date')->nullable();
// wbrc
```

```php
// database\factories\CustomerFactory.php

// wbrs
use App\Models\Customer;
// wbrc

// wbrs
$type = $this->faker->randomElement(['I', 'B']);
$name = $type == 'I' ? $this->faker->name() : $this->faker->company();
// wbrc

// wbrs
'name' => $name,
'type' => $type,
'email' => $this->faker->email(),
'address' => $this->faker->streetAddress(),
'city' => $this->faker->city(),
'state' => $this->faker->state(),
'postal_code' => $this->faker->postCode()
// wbrc
```

```php
// database\factories\InvoiceFactory.php
// wbrs
use App\Models\Invoice;
use App\Models\Customer;
// wbrc

// wbrs
$status = $this->faker->randomElement(['B', 'P', 'V']);
// wbrc
        
// wbrs
'customer_id' => Customer::factory(),
'amount' => $this->faker->numberBetween(100, 20000),
'status' => $status,
'billed_date' => $this->faker->dateTimeThisDecade(),
'paid_date' => $status == 'p' ? $this->faker->dateTimeThisDecade() : NULL
// wbrc
```

```php
// database\seeders\CustomerSeeder.php

// wbrs
use App\Models\Customer;
// wbrc

// wbrs
Customer::factory()
	->count(25)
	->hasInvoices(10)
	->create();

Customer::factory()
	->count(100)
	->hasInvoices(5)
	->create();

Customer::factory()
	->count(100)
	->hasInvoices(3)
	->create();

Customer::factory()
	->count(5)
	->create();
// wbrc
```

```php
// database\seeders\DatabaseSeeder.php

// wbrs
$this->call([
	CustomerSeeder::class
]);
// wbrc
```

`$ php artisan migrate:fresh --seed`

### Step 2 : Providing Data

#### Versioning and Defining Routes :

```php
// app\Http\Controllers\Api\V1\CustomerController.php

// wbrs
namespace App\Http\Controllers\Api\V1;
// wbrc

// wbrs
use App\Http\Controllers\Controller;
use App\Http\Resources\V1\CustomerResource;
use App\Http\Resources\V1\CustomerCollection;
// wbrc

// wbrs
public function index(Request $request)
{
	$filter = new CustomerQuery();
	//[['column', 'operator', 'value']]
	$queryItems = $filter->transform($request);

	if (count($queryItems) == 0) {
	   return new CustomerCollection(Customer::paginate());
	} else {
		return new CustomerCollection(Customer::where($queryItems)->paginate());
	}
}
// wbrc

// wbrs
	return new CustomerResource($customer);
// wbrc
```

```php
// app\Http\Controllers\Api\V1\InvoiceController.php

// wbrs
namespace App\Http\Controllers\Api\V1;
use App\Http\Resources\V1\InvoiceResource;
use App\Http\Resources\V1\InvoiceCollection;
// wbrc

// wbrs
use App\Http\Controllers\Controller;
//wbrc

// wbrs
return new InvoiceCollection(Invoice::paginate());
// wbrc

// wbrs
return new InvoiceResource($invoice);
//wbrc
```

```php
// routes\api.php

// wbrs
// api/v1
Route::group(['prefix' => 'v1', 'namespace' => 'App\Http\Controllers\Api\V1'], function() {
    Route::apiResource('customers', CustomerController::class);
    Route::apiResponse('invoices', InvoiceController::class);

});
// wbrc
```

#### Transforming Database Data Into JSON

`$ php artisan make:resource V1\CustomerResource`

```php
// app\Http\Resources\V1\CustomerResource.php

// wbrs
return [
	'id' => $this->id,
	'name' => $this->name,
	'type' => $this->type,
	'email' => $this->email,
	'address' => $this->address,
	'city' => $this->city,
	'state' => $this->state,
	'postalCode' => $this->postal_code
]
// wbrc
```

`$ php artisan make:resource V1\CustomerCollection`

```php
// app\Http\Resources\V1\CustomerCollection.php
```

`$ php artisan make:resource V1\InvoiceResource`

```php
// app\Http\Resources\V1\InvoiceResource.php

// wbrs
 return [
	'id' => $this->id,
	'customerId' => $this->customer_id,
	'amount' => $this->amount,
	'status' => $this->status,
	'billedDate' => $this->billed_date,
	'paidDate' => $this->paid_date,
];
// wbrc
```

`$ php artisan make:resource V1\InvoiceCollection`

```php
// app\Http\Resources\V1\InvoiceCollection.php
```