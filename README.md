## Shortcode Boilerplate ##
The Shortcode Boilerplate helps to keep you independently focused on two things, your shortcode logic and the visual design of your shortcode with the use of templates. Everything else is handled by the boilerplate. Setup is simple and you should be able to configure and customize the boilerplate to fit your situation without much effort. My goal for this boilerplate was to design it in such a way where a lot of shortcodes can be created quickly. I also wanted the ability to add flexiblity to my shortcodes without too much difficulty.  The base class starts off with a simple structure and can be augmented to add complexity to be shared by all shortcodes. Each shortcode has its own class that extends the base class which should provide some good ground for creating your shortcodes. 

## Features
* Easily create simple or complex templates and sub templates per shortcode.
* Allow users to select and load multiple versions of the same template.
* Generate multiple templates and sub-templates. *(useful for posts or galleries)*
* Easily assign multiple shortcodes to one file. *(Example: accordion & tabs)*
* Nest as many templates as you want without much effort.
* Modify template loading behavior to suit your own naming convention.
* Includes demonstration on how to register a large volume of shortcodes to one file. *(Example: dropcaps)*
* Option to auto-generate a default id for each shortcode.
* Includes easy to understand sample shortcodes.
* Includes functional dropcap shortcodes you can customize.
* Extend the base class for more complexity.
* Code is clean and easy to follow. *(I think)*.
* Well documented code using WordPress best practices.


## Borrowed Technologies
* [WordPress Plugin Boilerplate](https://github.com/DevinVinson/WordPress-Plugin-Boilerplate)
* [Gamajo-Template-Loader](https://github.com/GaryJones/Gamajo-Template-Loader)

## Contributing
This boilerplate isn't perfect so any contribution is welcomed.

## Installation
1. Clone the master branch of this respository or download the files into your WordPress plugins folder on your local machine.
<pre>$ git clone https://github.com/FiGOBLAC/shortcode-boilerplate shortcode-bp</pre>

2. Visit your WordPresss plugins page and activate the Shortcodes BP Plugin.

## Demo
There are two sample shortcodes and sample dropcap shortcode included demonstrating the use of templates, template alternates and nested post templates. Copy and paste any one or all of the sample shortcodes below onto a page. 
<pre>[sample_shortcode]Sample Shortcode using a template generated by the boilerplate[/sample_shortcode]</pre>

<pre>[sample_shortcode version='alternate']Another sample shortcode using an alternate template![/sample_shortcode]</pre>

<pre>[sample_posts]</pre>

<pre>[sample_posts version = 'alternate']</pre>

## Getting Started
After downloading the files into your wordpress plugin directory you can rename all your files to the name of your plugin or whatever name you prefer and replace all instances of ```Shortcode_BP```,```shortcode_bp``` and ```sbp``` with your own.

### Register Shortcodes
Edit ```/includes/class-shortcode-bp-configuration.php``` and add the name of the shortcode(s) and the shortcode's assigned class file used to process the shortcode to the ```$shortcodes``` array. You can name and prefix the file whatever you like as long the file name matches the name of the class handling the shortcode. If you have shortcode class file named ```Sample_Shortcode``` Then your configuration would be as follows:
```php
$shortcodes = array(
    'sample-shortcode'	=> 'class-sample-shortcode',
    'sample-posts'      => 'class-sample-posts',
);
```
**Note** Hyphens and underscores do not need to match the class file name just the words.

### Setup Shortcode Class File
Copy and rename the ```barbone.php``` class file located in ```/shortcodes-bp/public/shortcodes/```. The barebone class file should include a class that extends the ```Shortcode_BP``` base class and starts off with two important components: a public property named ```$defaults``` *(Your shortcode options)* and a public method named ```shortcode()``` responsible for returning your completed shortcode. At this point you can build up your shortcode logic any way you like. Your class file should resemble the example below.
```php
if( ! class_exists( 'Barebone' ) ) {

    class Barebone extends Shortcode_BP_Shortcode_Base {

        /**
         * The defaults for this shortcode
         *
         * @since   1.0.0
         * @access  public
         */
        public $defaults = array(
            'id'                => '',
            'class'             => '',
        );

        /**
         * Returns the shortcode defined with or without a template.
         *
         * @since       1.0.0
         * @access      public
         *
         * @return  string   Shortcode.
         */
        public function shortcode() {

            // User defined attributes.
            $attributes = $this->attributes;

            // Defined shortcode id.
            $id = $attributes['id'];

            // Defined shortcode class.
            $class = $attributes['class'];

            // Content passed into the shortcode.
            $content = $this->content;

            // The shortcode's main template map matching the variables in the template.
            $map =  compact( 'id', 'class', 'content');

            // Return a fully furnished shortcode template.
            return  $this->get_shortcode_template( $map );

	}
    }
}

```

## Building Your Shortcode
As previously mentioned your class should start off with two components: ```$defaults``` and a public method named ```shortcode()```. The internals and definitions of both are briefly explanined below.

### Defaults Class Property
Your defaults should be just as plain as you see in the example below. Do not use ```extract()``` or ```shortcode_atts() ``` as they are not needed since everything is already appropriately handled within the base class. Just add the defaults your shortcode will use.
```PHP
.....        
/**
 * The defaults for this shortcode
 *
 * @since   1.0.0
 * @access  public
 */
public $defaults = array(
    'id'	=> '',
    'class'	=> '',
    'my-opton'	=> '',
);
  ........
 ```
 ### Shortcode Class Method
The ```shortcode()```  class method returns your shortcode based on logic you build within and/or around it. Once you build your logic all you need to do is return the shortcode as you would normally do. If your shortcode uses a template then you will create an associative array called a *Template Map* and pass it to the ```get_shortcode_template()``` method provided by the base class. Creating *Templates* and *Template Maps* will be explained later. If you're familiar with building shortcodes then the example below should be somewhat self explanitory.
```PHP
........
/**
 * Returns the shortcode defined with or without a template.
 *
 * @since       1.0.0
 * @access      public
 *
 * @return  string   Shortcode.
 */
public function shortcode() {

    // User defined attributes.
    $attributes = $this->attributes;

    // Defined shortcode id.
    $id = $attributes['id'];

    // Defined shortcode class.
    $class = $attributes['class'];

    // Content passed into the shortcode.
    $content = $this->content;

    // The shortcode's main template map matching the variables in the template.
    $map =  compact( 'id', 'class', 'content');

    // Return a fully furnished shortcode template.
    return  $this->get_shortcode_template( $map );
}
........
```
### Accessing Callback Arguments
The callback arguments for the shortcode ***(atts, content, shortcode)*** are built into the base class and can be accessed anywhere in your class. Use  ```$this->attributes``` when you need to acccess any of the callback arguments as shown below. 

```PHP
// User defined attributes.
$attributes = $this->attributes;

// Define shortcode id.
$id = $attributes['id'];

// Define shortcode class.
$class = $attributes['class'];

// Define some arbitrary attribute
$my_custom_attribute = $attributes['my-custom-attribute'];

// Get the shortcode that called this file
$shortcode_name = $this->shortcode;

// Content passed to the shortcode.
$content = $this->content;

// The shortcode's main template map matching the variables in the template.
$map =	compact( 'id', 'class', 'content' );
 
```
## Building Shortcode Templates
Creating templates for your shortcodes makes it possible for you to separate logic from presentation in situations where it makes the most sense. This is not necessary in all cases especially if you are only returning a single line of code. Setting up a template for your shortcode is really easy. Once you create the html template for your shortcode you just need to create a *Template Map* that merges shortcode content with your template.

### The Template Directory
Before creating your shortcode's template its a good idea to confirm where you want the boilerplate to look when loading a template. The default directory is ```public/templates/your-shortcode-template.php``` To change this you only need to add the property below to your shortcode's class pointing to the name of the directory where you want the boilerplate to look.
 ```php
 public $template_directory ='directory-name';
 ```
 The bolierplate will now load your template from ```public/templates/directory-name```.
 If you want to change the full directory then you just need to edit [shortcode-bp-template-loader.php](https://github.com/FiGOBLAC/Shortcode-Boilerplate/blob/master/public/class-shortcode-bp-template-loader.php) and modify the line below:
 ```php
 protected $plugin_template_directory = 'public/templates';
 ```

### The Template Map
A template map is an associative array that contains content that you want to display in a template. The *keys* in the array represent the name of the variables placed in the body of your template. The value stored in each *key* will replace any variables that matches the *key* in the array. Here we use the ```compact()``` function for our template map because its easier and cleaner than explicitly writing out an associate array.
```php
// Some arbitrary value to display in the shortcode template. 
$content_1 = 'Some nice content goes here';
                        
// Another arbitrary value to display in the shortcode template. 
$content_2 = 'And some other really nice content goes here';
    
// The shortcode's main template map matching the variables in the template.
$template_map = compact( 'content_1',' content_2' );
```
### The Shortcode Template
Create your new template file within the ```/public/templates/``` directory using the same name as the shortcode it applies to converting underscores to hyphens. *(You can easily change this to your own naming convention later)*. If your shortcode name is ```sample_shortcode``` your template file name should be ```sample-shortcode.php```Place the variables from your *Template Map* in the locations of the template where you want the values to be displayed. Not all variables in your *Map* need to be used by the same template. You can reserved variables for alternate templates that will be using the same map.
```html
    <div id="$id" class="shortcodes $class">
        <div class="sample-shortcode">
            <span> $content_1 </span>
            <span> $content_2 </span>
        </div>
    </div>
```
## Loading Shortcode Templates
Templates are loaded and displayed via the ```get_shortcode_template()``` method provided by the base class. By default each template you create for your shortcode will be loaded based on the name of the shortcode that called it. Options for modifying the template loading behavior are explained below.

### Load Template Using Shortcode Name (Default)
To simply load shortcode templates using the default naming convention make sure the template file name matches the name of the shortcode that will be using the template. *Example:* ```/public/templates/sample-shortcode.php```. Then pass in an associative array called a *Template Map* used to display the shortcode's content as a parameter in the ```get_shortcode_template()``` function.
```php
// Some arbitrary value to display in the shortcode template. 
$content_1 = 'Some nice content goes here';
                        
// Another arbitrary value to display in the shortcode template. 
$content_2 = 'And some other really nice content goes here';

// Define the content for your shortcode's template.
$template_map = compact( 'content_value_1',' content_value_2' )

// Return a fully furnished shortcode template.
return $this->get_shortcode_template( template_map );
```
### Load Template Using Custom Name
Instead of using the default method of loading templates you can use your own custom template naming convention per shortcode by passing in the name of the custom template as a second parameter to the ```get_shortcode_template()``` method. This will load a template for the current shortcode using the name you provided instead of the default naming convention.

```php
// Return a fully furnished shortcode template.
return $this->get_shortcode_template( $map, 'my-custom-template-name' );
```
## Loading Alternate Templates
You can create multiple versions of the same template and allow users to switch between them which could make your shortcode a bit more flexible. All you need to do is copy your base template *(or create a new one)*, make your changes and suffix the file name with a hypen followed by your custom version name. The boilerplate will now recognize this file as an alternate to the original.

*Base Template.*
```php
'my-sample-shortcode.php
```
*Template Alternate.*
```php
'my-sample-shortcode-my-alternate.php
```
Then you will need to add the key ```version``` to your list of default options. 
 ```PHP
  public  $defaults = array(
    'id'            => '',
    'class'         => '',
    'version'       => '',
  );
   ```
The user can now switch versions by adding the *version name* as a value to the shortcode's ```version``` option:
<pre>[my_sample_shortcode version ="my-alternate"] content [/my_sample_shortcode]</pre>
 
 If you prefer to use a different option name other than ```version``` you can change this either globally or on the fly per shortcode:
 
 *To change ```version``` handle for the current shortcode add the property to your class*:
```php
 public $version_handle = 'my-new-option-handle';
 ```
 *To change```version``` handle globally affecting all shortcodes:* 
 
 Edit [class-shortcode-bp-shortcode-base.php](https://github.com/FiGOBLAC/Shortcode-Boilerplate/blob/master/public/class-shortcode-bp-shortcode-base.php)and modify this line changing the default option handle to your own custom handle.
```php
 public $version_handle = 'my-new-option-handle';
 ```

#### Changing Templates And Versions on the fly
Sometimes you may not want the user to change a template version but instead you may want to change the version internally based on some arbitrary condition.

Just create your template version and pass the version name as the third parameter of the ```get_shortcode_template()``` method.

```php
if ( $your_condition ) {

	$custom_version = 'my-version';
}

return $this->get_shortcode_template( $map, $this->shortcode, $custom_version );
```

You could also create coniditions for loading specific templates as well...

```php
if ( $your_condition ) {

	$alternate_template = 'alternate-version';

	// Load my custom template's alternate version.
    	return $this->get_shortcode_template( $map, 'template-name', $alternate_template );
	
} else if ( $your_condition ) {

	// Load my custom template.
	return $this->get_shortcode_template( $map , 'template-name' );
	
} else {

	// Load the default base template.
	return $this->get_shortcode_template( $map );
}
```
#### Modifying Template Loading behavior
To limit the ```get_shortcode_template()``` function to check only for the base name and ignore the ```version``` option value just add 'disable' as the third parameter.This is useful when your making multiple calls or you need each function to pull a specific template without worrying about each call reacting to the value added to ```version``` option.
```php
// Will search shortcode-name.php only.
return $this->get_shortcode_template( $map, '', 'disable'  );
```
You can also limit this function to check only for ```version``` *(sub-temlplate)* combined with base name especially in situations where you only want to return false instead of falling back to the base template when a template version searched for can't be found.
```php
// Will search for shortcode-name-version.php and shortcode-name-version-alt.php
return $this->get_shortcode_template( $map, '_sub' );
```
To prevent this function from checking for alteranate version for sub templates add false to the third parameter.
```php
// Will only search for shortcode-name-version.php
return $this->get_shortcode_template( $map, '_sub',false );
```

### Generating Multiple Templates
Generating multiple templates is useful for things like posts or galleries. Just call ```get_shortcode_template() ``` method within a loop. Edit the [sample post shortcode](https://github.com/FiGOBLAC/Shortcode-Boilerplate/blob/master/public/shortcodes/class-sample-posts.php) included in the boilerplate to see a demonstration on how to do this.

### Template Nesting (Sub-Templates)
You can nest as many templates as you want if you can find a good a reason to do so. Just place the calls to ```get_shortcode_template()``` inside of your template map as follows.

```php

$content1 = 'Random content string';
$content2 = 'Another random content string';

$nested1 = 'content string that will be nested';
$nested2 = 'Another content string that will be nested';

// Nested template map.
$map = compact( $content1, $content2, $this->get_template_map( compact( $nested1, $nested2 ) );

// Return a fully furnished shortcode template.
return $this->get_shortcode_template( $map );
```
 ### Shortcodes Without A Template
If you decide not to use a template for a specific shortcode or maybe you've created a small template within the shortcode class then all you need to do is return the actual shortcode instead of an array of values. For example..
```php
...{
 $shortcode = '<div id="$id" class="shortcodes $class" role=""><div class="sample-shortcode">$content</div></div>';
 return $shortcode;
 }
```
