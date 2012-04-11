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


