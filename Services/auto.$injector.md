# $injector @1.4.7
- service in module auto

`$injector` is used to retrieve object instances as defined by provider, instantiate types, invoke methods, and load modules.

The following always holds true:

``` javascript
var $injector = angular.injector();
expect($injector.get('$injector')).toBe($injector);
expect($injector.invoke(function($injector) {
  return $injector;
})).toBe($injector);
```

# Injection Function Annotation

JavaScript does not have annotations, and annotations are needed for dependency injection. The following are all valid ways of annotating function with injection arguments and are equivalent.

``` javascript
// inferred (only works if code not minified/obfuscated)
$injector.invoke(function(serviceA){});

// annotated
function explicit(serviceA) {};
explicit.$inject = ['serviceA'];
$injector.invoke(explicit);

// inline
$injector.invoke(['serviceA', function(serviceA){}]);

```

### Inference

In JavaScript calling toString() on a function returns the function definition. The definition can then be parsed and the function arguments can be extracted. This method of discovering annotations is disallowed when the injector is in strict mode. NOTE: This does not work with minification, and obfuscation tools since these tools change the argument names.

### $inject Annotation

By adding an $inject property onto a function the injection parameters can be specified.

### Inline

As an array of injection names, where the last item in the array is the function to call.


## 方法

### 1.`get(name, [caller])`
