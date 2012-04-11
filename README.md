This repository was used to demonstrate simple Object Oriented
Javascript.

## Javascript only has two scopes
1  Global Scope

    function setGlobal(){
      config = "my setting";
    }
    
    function getGlobal(){
      console.log(config);
    }

2  Function Scope (brief on variable hositing)

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
1	Object Context

	var o = {
		x:10,
		m: function(){
			var x = 1;
			console.log(x, this.x);
		}
	}
	o.m();
**Whats *this?***

2	Variable Context


##  Immediate Functions and Modules

1 Immediate Functions

    // Some say module and immediate function are interchangable terms
    (function(){
      console.log("I get ran immidiately");
    })();

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
