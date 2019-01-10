
<p align="center">
<img src="https://avatars0.githubusercontent.com/u/38014179?s=200&v=4" alt="Ksquare" class="img-responsive" width="275" height="52">
</p>

<h3 align="center">Kreate</h3>

<p align="center">
Krate - CSS design system from Ksquare to the community
</p>

## Quick Start


### Development environment

The project includes a development environment that will help us to build our css files. These css files will be auto prefixed to achieve better browser compatibilty .

There are two scripts that will provide us two environments, _dev_ and _production_.


#### Installing dev environment

Our ```package.json```file have all the dependencies needed, you just need to install them. You can use _npm_ or _yarn_.

* ```yarn install``` / ```yarn```
* ```npm install```

#### Dev mode

Dev mode will build our css files keeping block structure and comments. Dev mode will keep listening to changes in _*.scss_ files and will rebuild when needed.

All dev css files will be located at ```dev/css/```folder. Folder structure is explained below.

To run dev mode you need to run dev script using _npm_ or _yarn_:

* ```yarn run dev```
* ```npm run dev```

#### Production mode

Production mode will build a prefixed, minified and uncommented css file. 

All production css files will be located at ```dist/css/```folder. Folder structure is explained below.

To run dev mode you need to run dev script using _npm_ or _yarn_:

* ```yarn run build```
* ```npm run build```

## Project Structure

We will keep a clean and organized files structure, keeping general and specific styling separated making them easy to read, develop and maintain.

### Folder Structure

```
├── dev
│   └── css                          # CSS file built for development
├── dist
│   └── css                          # CSS file built for production
├── src                              # All SASS files must be stored here
│   ├── assets
│   ├── scss
│   │   ├── component-name
│   │   │   ├── Modules
│   │   │   ├── Partials
│   │   │   │   ├── _base.scss
│   │   │   │   └── _category.scss         # A category name following the requirements described below
│   │   │   └──  Vendor
│   │   ├── Modules                    
│   │   ├── Partials                     
│   │   ├── Vendor                    
│   │   ├── component-name.scss            # Primary file for each specific style
│   │   ├── _reset.scss
│   │   └── main.scss
├── package.json                     # Script for dependencies and runing scripts
└── README.md
```

#### Assets

In the assets forlder we must store all files that aren't code, like images or fonts.

#### src

##### component-name
Even though we will make almost every styling reusable, sometime we may need an specific styling, it could be for a component, container or _skeletons_, such as Wordpress. For these cases we must make an auto-contained styles folder with the same structure as the main folder. This structure will be explained ahead. 

##### Modules

This folder contains all SASS files that does not output css styling by themselves, just when they are imported/used.

I.E.
* _variables.scss
* _mixins.scss
* _functions.scss

##### Partials

This folder contains all the .scss files that will output css styling. Files must be separated into categories. This categories must be ease to read and understand, helping the reading, developing, maintenance and introduction to new developers.

I.E.
* _typography.scss
* _buttons.scss
* _alerts.scss
* _grid.scss

##### main.scss

A base file is used to import all global mixins, functions and variables that a partial may use.

##### _reset.scss

We implemented a reset file to achieve a efficient implementation across browsers, removing inconsistencies in things like default line heights, margins and font sizes of headings, and so on. This aproach have being used by different UI frameworks like Bootstrap.
<https://meyerweb.com/eric/tools/css/reset/>

#### Vendor

This folder contains all third party styling files or packages that may be needed.

I.E.
* bootstrap.css

## Conventions

Before explaining the code conventions, i highly recommend using VSCode as your text editor and endorse the usage of SCSS Formater extension. It uses Prettier behind curtains and will help us keeping our scss code nice and tight. 

You can get it on VSCode marketplace or here <https://marketplace.visualstudio.com/items?itemName=sibiraj-s.vscode-scss-formatter>



### Coding and naming

#### Class naming

We are aligning with the community using hyphen-case syntax for classes. This syntax help us defining relationships like parent-child or subclassing.

I.E.

* Parent-child
```
.list { /**/ }

.list-item { /**/ }
```
* Sub-classing

```
.button {
	/**/
	&:hover {
		/**/
	}
}

.dropdown-button {
	/**/
}

# HTML usage
<button class="button dropdown-button">
Dropdown
</button>

```

#### Location

We will always search for simplicity to maintain and ease to use, with this statement in mind, all url declarations must be written first on ```_locations.scss```. This aproach will help us change urls fast and efficiently. 

For a single url we use a normal variable, and for files that belongs to a single context we must use a map.

For a single url we use them just calling them directly. For location maps we must use a function that will search and return our value.

I.E.
* Single
	+ Declaration 	
      ```
      $img-url: '.../route/to/file';
      ```
    + Usage
	  ```
      .my-class{
      	background-image: url("#{$img-url}");
      }
	  ```
* Multiple urls on a single context
	+ Declaration 	
      ```
      $random-font-family: (
              font1:'../route/to/font',
              font2:'../route/to/font2'
          );
      ```
    + Usage
	  ```
		#_functions.scss
        @function get-font($font-map, $font) {
            @return map-get-strict($font-map, $font);
        }
        
        #_fonts.scss
        @font-face {
          font-family: 'Random Font';
          src: url("#{get-font($random-font-family, 'font1')}") format('woff2')
          font-weight: normal;
          font-style: italic;
      }
      ```

#### SASS nesting

SASS nesting is a feature very useful yet easy to abuse. Working for a cleaner and reusable code, we must avoid deep nesting and use it only when necesary.

SASS guidelines dictate that the most deep level we should use is 3. Nevertheless there will be time when we can use it. Those cases will be on specific component and they will be created on a new folder, as shown in folder structure.

The most use case for light nesting will be _pseudo-elements_, _pseudo-classes_ or modifiers (will be explained below).

I.E.

* Pseudo-elements <https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements>
```
.link-button {
  /**/
  &::after {
  	/**/
  }
}
```

* Pseudo-classes <https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes>
```
.nav-button {
  /**/
  &:hover {
  	/**/
  }
}
```

#### Modifiers

Modifiers are a syntactical way to define changes on another class. This classes should be self-explanatory. We must try to keep modifiers as smallest as we can.

There will be two types of modifiers, _global_ and _specific_.

##### Global modifiers

Global modifiers are the ones that can be reused on more than one classes. They will be written outside any other class. If the effect of its modification its difficult to infer, it must be documented with comments.

I.E.

```
.other-class {}

.another-class {}


/* weird-modification: Changes content into a highly animated figure */
.weird-modification {
	/**/
}

```

##### Specific modifiers

Specific modifiers are the ones that modifies just one class. This approach should be avoided but is not prohibited. 

Specific modifiers should comply with nesting convention and must avoid heavy nesting.

```
.dr-banner {
  /**/
  &.gamma-rays {
  	/* turns green */
  }
}
```

#### Extends, placeholders, mixins and functions.

Extends, placeholders and mixins are syntactic sugar made to help us reduce repeating code and keeping our stylesheets nice and clean.

##### Extends

An extend is a class which properties could be shared with other classes **and** it can be used by its own. An extend is written as any other class and is called using ```@extend```.

I.E.

```
.main-font { /**/}

# SASS
.some-component {
@extend .main-font;
}

# HTML
<p class="main-font">This is text</p>
```

##### Placeholders

Just like extends, a placeholder is a class which properties could be shared with other classes, the difference is that a placeholder just works inside our sass files, this class is **not** outputted on the css build.

To write a placeholder you should use a percent sign, ```%```, before naming it and is called using ```@extend```.

I.E.
```
%reset-button {/**/}


.main-button {
	@extend %reset-button;
}

```

##### Mixins

Just like the two options described before, mixins are made to reuse code, the advantage that mixins have is you can use parameters to mold the shared properties.

To use it you need to declare them using ```@mixin```. To use them yo need to call them using ```@include```.

I.E.

```
@mixin noto-sans-bold ($style) {
  font-family: 'Noto Sans';
  font-weight: bold;
  font-style: $style;
}


.my-text {
	@include noto-bold('italic');
}
```

##### Functions

Functions will help us to create values, manipulate maps, or create style blocks in a clean and organize way. All functions must be commented on their use.

I.E.
```
/** 
* @function get-font
*
* @param $font-map: Map where font will be searched
* @param $font: Font that will be searched
*
* @returns Returns the specified font or an error
* */

@function get-font($font-map, $font) {
    @return map-get-strict($font-map, $font);
}
```

#### Comments

All comments must be using slash-asterisk syntax, important comments must use exclamation mark. 

I.E.

* Normal comment

```/* This is a comment*/ ```
* Important comment 

```/* ! This is very important */ ```



### Non self-documentable properties

There are some properties which name and usage doesn't explain when, how or where they are used, for such cases we must document the effects of such properties using comments, which format has been explained before.

I.E.

* z-index
* Browser hacks








