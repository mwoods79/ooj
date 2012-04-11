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


