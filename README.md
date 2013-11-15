# Auto Publish

A small package that exposes a Laravel facade to compile HAML files using [MtHaml](https://github.com/arnaud-lb/MtHaml).

## Installation

1. Add it to your composer.json (`"bkwld/laravel-haml": "~1.0"`) and do a composer install.

2. Add the service provider to your app.php config file providers: `'Bkwld\LaravelHaml\ServiceProvider',`

3. Create an alias to the facade: `'HAML' => 'Bkwld\LaravelHaml\Facade',`

## Usage

Manually invoke the LaravelHaml compiler before you render your view.  For instance, here is a Laravel controller action:

	public function index() {
		HAML::compile('home.index');
		$this->layout->nest('content', 'home.index');
	}

When this action is requested, LaravelHaml will:

1. Check that the enviornment is "local".  If it isn't, it will exit.
2. Look for a file at /app/views/home/index.haml.  If not found, an exception is raised.
3. Compile the the template using [MtHaml](https://github.com/arnaud-lb/MtHaml).  But only if there isn't an existing compiled view that is still valid.
4. Save the compiled haml into a php file at /app/views/home/index.php.

Thus, when Laravel goes to nest the `home.index` view, it will find the compiled php view.