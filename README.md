# jPackage.js
## Package Autoloader for javascript packages

jPackage can be used to autoload javascript packages at runtime.  

Basic Usage:  

```
<script language='javascript' src='jPackage.js'></script>

/* myPackage now holds the package */
var myPackage = jPackage("name.spaced.myPackage");

```

Configuration:

```
<script language='javascript' src='jPackage.js'></script>

jPackage.config({
	method: 'POST',					=> method for XMLHttpRequest [default: 'GET']
	path: 'http://hostname/path/to/packages',	=> basedir for packages	[default: '']
	suffix: 'jPackage'				=> Filename suffix for jPackage Files [default: 'js']
});

/* Rewrite Package Names to math path on server. This one will make Package a.b.c become [path]/a_b_c.[suffix] */
jPackage.registerRewriteHandler(function(path, pkg, suffix) {

	return path + '/' + pkg.replace(/\./g, "_") + "." + suffix;

});

/* Prevent other scripts from changing configuration or adding RewriteHandlers */
jPackage.seal();

/* myPackage now holds the package */
var myPackage = jPackage("name.spaced.myPackage");

```

Package Example:

```
[File: /packages/namespace/example.jPackage]
jPackage({
	name: "namespace.example", // Package Name. Must matched the name of the requested package
	author: "AUTHOR", // Optional 
	license: "LICENSE", // Optional 
	requires: ["namespace.example2", "namespace.example3"] // All required packages are tried to be loaded before the package is loaded.
	init: function() { // This is called upon load. 'this' var point to the package itself. so you can add datastructures here or something like that

		this.namespace = {};
		
	}
});

[File: /index.html]

<script language='javascript' src='jPackage.js'></script>

jPackage.config({
	path: '/packages',
	suffix: 'jPackage'
});

var p = jPackage("namespace.example"); // <- loads namespace.example, namespace.example2 and namespace.example3

```
