# 🎯 What are we learning today?
- How modules work  
- How modules load into a page  
- How NodeJS handles multiple modules  
- `module.exports` and `require` functions  
<hr>

# 1. How modules work ?
```
function x () {
		const a = 10;
}
console.log(a);
```
**Q: If you execute this code, will you be able to access it outside the function?**  
→ You cannot access a value outside the function `x` because it is defined within the function's scope. Each function creates its own scope, so variables inside a function are not accessible from outside that function.  

> **Note:**  
> Modules in Node.js behave like function scopes. When you require a file, Node.js wraps that file's code inside a function.  
> This creates a contained scope—all variables and functions in the module stay private unless they're explicitly exported.  

> 💡 All the code of a module is wrapped inside a function when you call `require`.  
> This function is not a regular function; it’s a special type known as an IIFE  
> (Immediately Invoked Function Expression). Here’s how it works:  

 
 In Node.js, before passing the code to the V8 engine, it wraps the  
 module code inside an IIFE.  

```
(function () {
// All the code of the module runs inside here
})();
```     
**❓ Why IIFE?**  
- The function runs as soon as it is defined.  
- By encapsulating the code within the IIFE, it prevents variables and functions from interfering with other parts of the code.  
  This ensures that the code within the IIFE remains independent and private.  

> **Imp Questons**  
> ❓ How are variables and functions private in different modules?  
> **->**  Because of IIFE and the `require` (statement) wrapping code inside IIFE. <br><br> 
> **❓ How do you get access to module.exports? Where does this module come from?**  
> **->** In Node.js, when your code is wrapped inside a function, this function has a  
> parameter named `module`. This parameter is an object provided by Node.js that includes  
> `module.exports`.  
<hr><hr>

# 2. How require() Works Behind the Scenes

- There is 5 Step mechanism of require

### 1. Resolving the Module

- Node.js first determines the path of the module. It checks whether the
path is a local file ( ./local ), a JSON file ( .json ), or a module from the
node_modules directory, among other possibilities.

### 2. Loading the Module

- Once the path is resolved, Node.js loads the file content based on its type.
The loading process varies depending on whether the file is JavaScript, JSON, or another type.

### 3. Wrapping Inside an IIFE

- The module code is wrapped in an Immediately Invoked Function
Expression (IIFE). This wrapping helps encapsulate the module's scope,
keeping variables and functions private to the module.
- 

### 4. Code Evaluation and Module Exports

- After wrapping, Node.js evaluates the module’s code. During this
evaluation, module.exports is set to export the module's functionality or
data. This step essentially makes the module's exports available to other
files.

### 5. Caching

- Importance: Caching is crucial for performance. Node.js caches the result
of the require() call so that the module is only loaded and executed once.