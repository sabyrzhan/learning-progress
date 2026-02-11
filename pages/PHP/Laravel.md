## [[Generate app key]]
	- `php artisan key:generate` - will update `.env` with generated key
	- `php artisan key:generate --show` - will generate and show in console without update
- ## [[Vue 3 and Laravel 11]]
	- HowTo
		- ```
		  //1 vite.config.js
		  import { defineConfig } from 'vite';
		  import vue from "@vitejs/plugin-vue";
		  import laravel from 'laravel-vite-plugin';
		  import {fileURLToPath, URL} from "node:url";
		  
		  export default defineConfig({
		      plugins: [
		          vue(),
		          laravel({
		              input: ['resources/css/app.css', 'resources/js/app.js'],
		              refresh: true,
		          }),
		      ],
		      resolve: {
		          alias: {
		              vue: "vue/dist/vue.esm-bundler.js",
		              '@': fileURLToPath(new URL('./resources/js', import.meta.url))
		          },
		      },
		  });
		  
		  
		  //2 package.json
		  {
		      "private": true,
		      "type": "module",
		      "scripts": {
		          "dev": "vite",
		          "build": "vite build"
		      },
		      "dependencies": {
		          "vue": "^3.4.21",
		          "vue-router": "^4.3.2",
		          "vuex": "^4.1.0"
		      },
		      "devDependencies": {
		          "laravel-vite-plugin": "^1.0",
		          "vite": "^5.0",
		          "@vitejs/plugin-vue": "^5.0.4"
		      }
		  }
		  
		  
		  //3 resources/js/app.js
		  import {createApp} from 'vue'
		  import App from './App.vue'
		  import store from "@/store/index.js";
		  import router from "@/router.js";
		  
		  
		  const app = createApp(App);
		  
		  app.use(store)
		  app.use(router)
		  router.isReady().then(() => {
		      app.mount('#app')
		  })
		  
		  
		  //4 resources/views/welcome.blade.php
		  ...
		  <div id="app"></div>
		  ...
		  
		  //5 routes/web.php
		  <?php
		  
		  use Illuminate\Support\Facades\Route;
		  
		  Route::get('/{vue_capture?}', function () {
		      return view('welcome');
		  })->where('vue_capture', '[\/\w\.-]*');
		  
		  //6 install VueJS deps and run
		  npm install
		  npm run dev
		  ```
	- Sources
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