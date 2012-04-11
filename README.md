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

    // myModule is a singleton
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

# Objects and Prototypes

1.  Prototypes are not blueprints like classes in C# or Java
2.  JavaScript inherits from functions or prototypes
3.  Nothing magic about prototype they are simply an object instance
    that we are inheriting from

## Object Literals

#### Object Literals have many names
1.  Object Literals
2.  Hash
3.  Associative Array
4.  Key/Value Pair
5.  JSON

Object literals are singleton objecst with attributes and methods.
It is an object that is itself an object instance, you cannot `new` an
object literal.

    // Object literals are evaluated immediatly
    // This is important to note if you are using jQuery
    function stringMe(){ return "string"; };
    var literal = {
      key: 'value',
      method: function() {
        console.log("imma function");
      },
      assignedImmediatlyAsString: stringMe()
    };

    console.log(literal.method());
    console.log(literal['assignedImmediatlyAsString']);
    console.log(literal['method']());

#### Constructor Functions

1.  Every function in javascript returns something, even if that
    something is `undefined`;

    function evenThoughIDontSpecifyReturnIReturnUndefined () {
      console.log("look for undefined after me");
    };

    evenThoughIDontSpecifyReturnIReturnUndefined();

2.  Constructor functions have a convention of starting with a capital
    letter.

    // Never return anything from a constructor function
    function ImmaConstructor() {
      return {
        bad: "idea",
        my:  "context",
        was: "lost"
      };
    };

    var ohNo = new ImmaConstructor();
    console.log(ohNo);

3.  You can tack things on to a new constructor instance
    
    // This is horrible on memory, there is a better way
    function ImmaConstructor() {
      this.works = function () {console.log('this works')};
      this.alsoWorks = function () {console.log('this also works')};
    };

    var imma = new ImmaConstructor();
    imma.works();
    imma.alsoWorks();

####  Prototypes

1.  Prototypes cannot be changed once an object is instantiated, thus
    you cannot change the prototype of an object literal.

2.  Prototypes have simple rules, look at instance, look at prototype,
    look at prototypes...  up the chain.

    var duck = {
      quack: "quack",
      goNorth: function() { this.fly(); }
    }

    var mallord = Object.create(duck);
    mallord.fly = function() { console.log("flapping wings"); };

    var fakeMallord = Object.create(mallord);
    fakeMallord.fly = function() { console.log("I can't"); };
    fakeMallord.goNorth();

    // don't work
    duck.goNorth();

3.  Prototypes are shared

    function Duck () {};

    mallord = new Duck();

    Duck.prototype.fly = function() {console.log("flapping wings"); }

    mallord.fly();

    // instances are not shared
    rubberDuck = new Duck();
    rubberDuck.fly = function(){console.log("I can't");};

    rubberDuck.fly();

    uglyDuck = new Duck();
    uglyDuck.fly();


