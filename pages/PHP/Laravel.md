## [[Generate app key]]
	- `php artisan key:generate` - will update `.env` with generated key
	- `php artisan key:generate --show` - will generate and show in console without update
- ## [[Vue 3 and Laravel 11]]
	- https://vueschool.io/articles/vuejs-tutorials/the-ultimate-guide-for-using-vue-js-with-laravel/
	- https://laraveldaily.com/post/how-to-add-vue-3-laravel-10-vite-quick-tutorial
- ## [[Fill model with data array]]
	- Before filling with data the model must allow filling. For example below we are completely allowing filling. Instead better to whitelist only allowed fields:
	  ```
	  protected $fillable = ['name']; // allow this fields
	  protected $guarded = []; // OR making empty array lets mass assignment
	  ```
	- For creating models from a single item array:
	  ```
	  $Topic = new Topic();
	  $Topic->fill($array);
	  
	  // OR
	  Topic::create($array)
	  ```
	- For creating a collection from an array of items:
	  ```
	  $Topic::hydrate($result);
	  ```