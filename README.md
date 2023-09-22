
![BrewLogo](https://i.hizliresim.com/khtflke.png)

# BrewScript

BrewScript is an easy to use Haxe script tool that aims to be simple while supporting all Haxe structures.

## Installation
`haxelib install BrewScript`

Enter this command in command prompt to get the latest release from Haxe library.


`haxelib git BrewScript https://github.com/tahirk786/BrewScript.git`

Enter this command in command prompt to get the latest git release from Github. 
Git releases have the latest features but they are unstable and can cause problems.

After installing BrewScript, don't forget to add it to your Haxe project.

------------

### OpenFL projects
Add this to `Project.xml` to add BrewScript to your OpenFL project:
```xml
<haxelib name="BrewScript"/>
<haxedef name="hscriptPos"/>
```
### Haxe Projects
Add this to `build.hxml` to add BrewScript to your Haxe build.
```hxml
-lib BrewScript
-D hscriptPos
```

Flag `hscriptPos` is needed for error handling at runtime. It is optional but definitely recommended.

## Usage
To use BrewScript, you will need a file or a script. Using a file is recommended.

### Using without a file
```haxe
var script:brew.BrewScript = {}; // Create a new BrewScript class
script.doScript("
	function returnRandom():Float
		return Math.random() * 100;
"); // Implement the script
var call = script.call('returnRandom');
var randomNumber:Float = call.returnValue; // Access the returned value with returnValue
```
Usage of `doString` should be minimalized.

### Using with a file
```haxe
var script:brew.BrewScript = new brew.BrewScript("script.hx"); // Has the same contents with the script above
var randomNumber:Float = script.call('returnRandom').returnValue;
```

## Using Haxe 4.3.0 Syntaxes
BrewScript supports both `?.` and `??` sytnaxes including `??=`.

```haxe
import tea.BrewScript;
class Main 
{
	static function main()
	{
		var script:BrewScript = {};
		script.doString("
			var string:String = null;
			trace(string.length); // Throws an error
			trace(string?.length); // Doesn't throw an error and returns null
			trace(string ?? 'ss'); // Returns 'ss';
			trace(string ??= 'ss'); // Returns 'ss' and assigns it to `string` variable
		");
	}
}
```

## Extending BrewScript
You can create a class extending BrewScript to customize it better.
```haxe
class BrewScriptEx extends tea.BrewScript
{  
	override function preset():Void
	{
		super.preset();
		
		// Only use 'set', 'setClass' or 'setClassString' in preset
		// Macro classes are not allowed to be set
		setClass(StringTools);
		set('NaN', Math.NaN);
		setClassString('sys.io.File');
	}
}
```
Extend other functions only if you know what you're doing.

## Calling Methods from Brew's
You can call methods and receive their return value from Brew's using `call` function.
It needs one obligatory argument (function name) and one optional argument (function arguments array).

using `call` will return a structure that contains the return value, if calling has been successful, exceptions if it did not, called function name and script file name of the Brew.

Example:
```haxe
var brew:brew.BrewScript = {};
brew.doString('
	function method()
	{
		return 2 + 2;
	}
');
var call = brew.call('method');
trace(call.returnValue); // 4

brew.doString('
	function method()
	{
		var num:Int = 1.1;
		return num;
	}
')

var call = brew.call('method');
trace(call.returnValue, call.exceptions[0]); // null, Float should be Int
```

## Global Variables
With BrewScript, you can set variables to all running Brew's.
Example:

```haxe
var brew:brew.BrewScript = {};
brew.set('variable', 1);
brew.doScript('
	function returnVar()
	{
		return variable + variable2;
	}
');

brew.BrewScript.globalVariables.set('variable2', 2);
trace(brew.call('returnVar').returnValue); // 3
```

## Contact
If you have any questions or requests, open an issue here or message me on my Discord (tahirk786).