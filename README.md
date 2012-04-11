This repository was used to demonstrate simple Object Oriented
Javascript.

## Javascript only has two scopes
###1  Global Scope

    function setGlobal(){
      config = "my setting";
    }
    
    function getGlobal(){
      console.log(config);
    }

###2  Function Scope (brief on variable hositing)

    function setGlobal(){
      var config = "my setting";
    }
   
    // This throw error "config is not defined" 
    function getGlobal(){
      console.log(config);
    }

    // Hoisting - When false still "defined", even if not assigned
    function itsIffy() {
      if (false) {
        var iffy = "whatev's";
      }
      console.log(iffy);
    }
    
    
    // Functions can scope within functions
    function outerScope(){
      var closure;
      function innerScope(){
        closure = "I'm told I am in a closure?";
        console.log(closure)
      }
      innerScope();
    }

    outerScope();
    // shows closure is not on global scope
    console.log(closure); // ERRRRORRR

## Context 
### 1	Object Context

	var o = {
		x:10,
		m: function(){
			var x = 1;
			console.log(x, this.x);
		}
	}
	o.m();
*What's the output?* <!--outputs 1, 10-->

*What's **this**?*<!-- o -->

### 2	Global Context

	var o = {
		x:10,
		m: function(){
			var x = 1;
	 		var f = function(){
	 			console.log(x, this.x);
	 		}
	 		f();
	 	}
	 }
	 o.m();

*What's the output?* <!--outputs 1, undefined-->

*What's **this**?* <!-- Global Context (window) -->

####Alternatives:

You could reference global object to get to x


```
	//...
	var f = function(){
		console.log(x, o.x); 
	}
	//...
```

But if you are in protptype function that may not be possible

```
	var C = function(){};
	C.prototype = {
		x:10,
		m: function(){
			var x = 1;
			var f = function(){
				console.log(x, this.x);
			}
			f();
		}
	}
	var instance1 = new C();
	instance1.m();	
```

Your first thought may be to break out f and attach it to the prototype

```
	//...
	m: function(){
		var x = 1;
		this.f();
	},
	f: function(){
		console.log(x, this.x); // Reference ERROR!!	}
	//...
```
But now there is a scoping issue

#### Use Context to Fix Scope
Attach the function to the object context
	
	//...
	m: function(){
		var x = 1;
		this.f = function(){
			console.log(x, this.x); // outputs 1, 10
		}
		this.f();
	}
	//...
	
### Callbacks
We commonly find nested functions used as callbacks

	var o = {
		x:10,
		onTimeout: function(){
			console.log("x:", this.x);
		},
		m: function(){
			setTimeout(function(){
				this.onTimeout(); // ERROR
			}, 1);
		}
	}
	o.m();

#### Use Scope to Fix Context
A common solution to this problem is to save off a reference to **this**
	
	//...
	m: function(){
		var self = this;
		setTimeout(function(){
			self.onTimeout(); 
		}, 1);
	}
	//...

##  Immediate Functions and Modules

1 Immediate Functions

    // Some say module and immediate function are interchangable terms
    (function(){
      console.log("I get ran immidiately");
    })();

    // Small performance gain by passing in globals to closures
    (function($){
      console.log($);
    })(jQuery);
    // Do this thousands of times, woot!!

2 Returning or Exporting

    // Returning Object Literals
    var myModule = (function(){
      var privateClosure = 0;
      function voodoo(){
        console.log("voodoo count: " + ++privateClosure);
        // And I always return myself, when `new`
      };
      return {
        voodoo: voodoo,
        static: "Imma String, woopedy doo"
      };
    })();

    myModule.voodoo();
    myModule.voodoo();
    myModule.voodoo();

    // Returning Functions
    var myModule = (function(){
      function ImmaConstructor(){
        console.log('I build stuff');
        // And I always return myself, when `new`
      };
      return ImmaConstructor;
    })();

    new myModule();

