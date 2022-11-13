# learning-diary
<details>
<summary><b>Set data type in JS</b></summary>
<br>
A JavaScript set is a collection, it's ordered, and it contains only unique values. <br>
We provide some initial values to Set's constructor, then <code>.add</code> more values later. We can also ask a set whether it <code>.has</code> a given value.

       const names = new Set(['Amir', 'Betty', 'Cindy']);
       names.has('Amir'); // true

<br>
 JavaScript sets are ordered, they'll always come back in the order that they were inserted.
 
       const names = new Set(['Amir', 'Betty']);
       names.add('Cindy');
       names.has('Cindy');
       
 To get the elements out of the set, we can use the <code>.values</code> method. It returns an iterator and then by converting the iterator into an array with Array.from(someSet)
 
       const names = new Set(['Betty', 'Amir']);
       Array.from(names.values()); // ['Betty', 'Amir']
       names.add('Cindy');
       Array.from(names.values()); // ['Betty', 'Amir', 'Cindy']
       
 Elements can be deleted from a set with the <code>.delete</code> method. And the entire set can be cleared with <code>.clear</code>.
       
       names.delete('Cindy'); //['Betty', 'Amir']
       
Sets have a <code>.size</code> property that returns the number of items in the set.
       
       names.size; //2
       
A set's size reflects the number of unique values that it holds. Duplicates passed to the constructor or added with .add don't contribute to the size.
       
       const names = new Set(['Amir', 'Betty', 'Amir']);
       names.size;// 2
       
An array's .includes method slows down as the array gets larger. Likewise for many other array methods. But sets don't have that problem! A set's .has method is a constant O(1): it always takes the same amount of time regardless of how many elements there are.
       
</details>
<hr>


<details>
<summary><b>JSON</b></summary>
<br>
The stringify method turns a JavaScript object or value into a JSON string. The parse method turns a JSON string back into an object.
       
       JSON.stringify({a: 2}) // '{"a":2}' 
       JSON.parse('{"a":2}') // {a: 2} 
       
By combining the two methods, we can "round-trip" an object to JSON and back, ending up with the same object that we started with.
    
    JSON.parse(JSON.stringify({name: 'Amir'})); // {name: 'Amir'}
       
<code>undefined</code> not allowed in JSON. When we call stringify, any undefineds in the original object will be turned into nulls. If we then parse the resulting JSON, we'll get a null out. 

    JSON.stringify([1, undefined, 2]); // '[1,null,2]'
    JSON.parse(JSON.stringify([1, undefined, 2])); // [1, null, 2]
    
Sometimes, we want an object to specify how it should be serialized to JSON. We can do that by putting a function in its toJSON property. JSON.stringify will automatically call that function and use its result rather than the original object.

    
    const user = {
      name: 'Amir',
      toJSON: () => 'This is Amir!'
    };
    JSON.parse(JSON.stringify(user));
    // 'This is Amir!'
    
The toJSON function isn't responsible for actually converting to JSON; stringify will still do that part. Instead, toJSON returns a new JavaScript value to be serialized in place of the original.

    JSON.parse(JSON.stringify({
        name: 'Amir',
        toJSON: () => ({thisWas: 'Amir'})
        })
    ); // {thisWas: 'Amir'}

</details>
<hr>
<b>isSafeInteger</b><br>
All numbers in JavaScript are floating point, which means that they become imprecise past a certain threshold. This can be especially dangerous when dealing with numbers that we think of as integers.<br>
MIN_SAFE_INTEGER and MAX_SAFE_INTEGER (9007199254740991, -9007199254740991)

    Number.isSafeInteger(value);//true or false
value is a number that is a safe integer

<hr>
<b>regex</b><br>
{8,} means "at least eight characters"<br>

    /^[fho]{3,}$/.test('hoof'); //true, rule at least 3 chars
    
If we need five or fewer characters, we can say .{0,5}<br>

<code>/[hbd-fa]/</code> can be thought of as <code>/(h|b|[d-f]|a)/</code> <br>
Regexes provide a way to match the word boundary <code>\b</code><br>
<code>\b</code> only matches where a word character is next to a non-word character.

    /\bcat\b/.test("Where's the cat's toy?");
    RESULT:
    true
    
    
    /\bcat\b/.test('It was difficult to locate, but');
    RESULT:
    false

The ^ is only special if it's the first character in the set. There, it means "negate this set". But a ^ anywhere else in the set is just another literal character.

    /[^b]/.test('b');//starts with b
    RESULT:
    false
    >
    /[b^]/.test('b');//b|^
    RESULT:
    true

Write a regex that recognizes dogs and cats that are big or fluffy.

    /^(big|fluffy) (cat|dog)$/.test('big dog'); //true
    
? operator matches a character zero or one times, but not more than one. It is useful when part of a string is optional.

    /^(\d{3}-)?\d{3}-\d{4}$/.test('555-555-5555');//true
    
 <code>(\d{3}-)?</code> all before <code>?</code> is optional vs <code>\d{3}-?</code> only <code>-</code> before <code>?</code> is optional

<hr>
<b>Tagged template literals</b><br>
The tag function has access to the text and template values in the string, and can modify or replace the string.<br>

The first argument is an array of the literal strings in the template literal. Literal strings are the parts that are not inside of a <code>${...}</code>. The rest parameter, <code>...values</code>, collects all of the interpolated values into an array. Interpolated values are the parts that are inside of a  <code>${...}</code>.
If the template literal ends in an interpolated value, like `age: ${age}`, JavaScript will insert one more empty string, '', at the end. As a result, strings always has exactly one more element than ...values has.

    function returnsItsArguments(strings, ...values) {
        return {
            strings: strings,
            values: values,
            };
     }
    returnsItsArguments`the numbers ${1} and ${1 + 1}`;
    RESULT:
    {strings: ['the numbers ', ' and ', ''], values: [1, 2]}
https://www.executeprogram.com/courses/modern-javascript/lessons/tagged-template-literals

<hr>
<b> ::before ::after </b><br>
Do ::before and ::after elements only display if we use position absolute?
- If content: ''; 
    -  We don't need to have absolute positioning. We just need to set display property for the ::before and ::after pseudo-elements
display: block, inline-block, flex... => we need to give it size / shape for pseudo element<br>
 <code>.div-el::before {<br>
            content: '';<br>
            width: 60vw;<br>
            height: 20vh;<br>
            background-color: rgb(98, 0, 255);<br>
            display: block;<br>
        } </code><br> 
Or<br>
position: absolute, fixed...
- If content: 'some text, this prop is not empty'; //if the content of pseudo selectors is not an empty string
    - We don't need to have nor display, nor position, these will be regular display on HTML <br>
    ::before will be rendered before main content of element, ::after will be rendered after main content of element
    
      <code>  .div-el::before {
            content: 'Layer 1';
        } </code> 
        
<i>before => content => after</i>   these are layers of one element and the content will be inserted in this order*/<br>
Note: if pseudo el has position absolute, don't forget to add position: relative; to the element, which has these pseudo selectors.<br>
If no position is provided the pseudo el takes the root el as parent

<hr>
test-helper.js

    function equal(actual, expected, message) {
      if (actual === expected) {
        const defaultMessage = `Expected ${expected} and received ${actual}`;
        console.info("Pass: " + (message || defaultMessage));
      } else {
        const defaultMessage = `Expected ${expected} but received ${actual} instead`;
        console.error("Fail: " + (message || defaultMessage));
      }
    }

    function notEqual(actual, expected, message) {
      if (actual !== expected) {
        const defaultMessage = `${expected} is different to ${actual}`;
        console.info("Pass: " + (message || defaultMessage));
      } else {
        const defaultMessage = `${expected} is the same as ${actual}`;
        console.error("Fail: " + (message || defaultMessage));
      }
    }

    function test(name, testFunction) {
      console.group(name);
      testFunction();
      console.groupEnd(name);
    }

<hr>
Testing<br>
<b>Unit testing</b> is a methodology where units of code are tested in isolation from the rest of the application. <br>
<b>Integration tests</b>, which can be collaboration tests between two or more units.<br>
<b>Full end-to-end</b> tests of the whole running application with browser interaction.<br> 

There are 3 kind of tools we can use for testing
1. Unit and integration => Test runner (Execute your tests, summarize results), we can use Mocha
2. Unit and integration => Assertion library (Define testing logic, conditions, expectations), we can use Chai
3. End to End => Simulates browser interaction, we can use Puppeteer
Jest library for 1 and 2


<hr>
All async functions return promises, even if they don't await. So, the function's return value will automatically be wrapped in a fulfilled promise.

    async function double(n) {
      return n * 2;
    }
    double(5);
     ASYNC RESULT:
    {fulfilled: 10}
    
An async function without any awaits is allowed. All async functions return promises, even if they don't await. So, the function's return value will automatically be wrapped in a fulfilled promise.<br>
<b>Summarize the rules of exceptions in async/await functions:</b><br>

- async functions always return promises.
- await turns rejected promises into exceptions.
- Exceptions inside async functions turn into rejected promises.

Exceptions in async functions: there's an important difference between <i> return somePromise</i> and <i>return await somePromise</i>.<br>
The await translates rejected promises into exceptions. If we directly return the promise, rejections aren't translated into exceptions at all.


    async function fail() {
      try {
        return await Promise.reject(new Error('oh no'));
      } catch (e) {
        return 'caught the error';
      }
    }
    fail();
    ASYNC RESULT:
    {fulfilled: 'caught the error'}

The second example returns the promise directly: return aRejectedPromise. There's no await, so the rejection doesn't turn into an exception, so there's nothing for our catch to catch. Instead, this example returns the original rejected promise.


    async function fail() {
      try {
        return Promise.reject(new Error('oh no'));
      } catch (e) {
        return 'caught the error';
      }
    }
    fail();
     ASYNC RESULT:
    {rejected: 'Error: oh no'}


<hr>
<b>Webpack</b><br>
webpack is a module bundler. Its main purpose is to bundle JavaScript files for usage in a browser into one js file<br>
1. 

    npm install --save-dev webpack webpack-cli
npm will go and install all dependencies in node_modules

in package.json we can see new lines
  
    "devDependencies": {
        "webpack": "^5.74.0",
        "webpack-cli": "^4.10.0"
    }

2. for webpack to work we should tell weback tool where is our entry point
the entry point in my case is index.js<br>
webpack will go to file index.js and analyzes this file, most impotantly analyzes import lines in index.js
then it will go to that files from where we import and will analyze its imports dependencies too.
it will tell webpack all dependency files we need for our app.<br>


For this we need to create webpack.config.js file on the top level<br>

    webpack.config.js 
this is the file which webpack use to do it's job. In this file we should provide configuration for webpack


this file under the hood will be executed by NodeJS, so we should write instructions for our webpack in NodeJS syntax

    module.export={}; //NodeJS uses this syntax so that it exposes this object {} ouside of this file<br>
and webpack tool will go ahead and will import this object<br>
so this is a configuration object where we configure webpack<br>

For this we should restructure our code
we should split where our input sourse files are and our output generated files will be
for this create <br>
new folder src, where our input sourse filese will be stored.
new folder assets=>scripts folders will be for our output files<br>

so back to webpack.config.js

    const path = require('path');//path is a built package in NodeJS, so path features will be stored in variable path
    module.exports={
        mode: 'development',
        entry:"./src/index.js",
        output: {
            filename: "app.js",
            path:path.resolve(__dirname, "assets", "scripts")
        }

    };

__dirname is global NodeJS constant, this gives us absolute path where this config files lives in<br>
(__dirname, assets, scripts)
absoluteWebpackConfigFilePath/assets/scripts
3. package.json

    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1",
        "build": "webpack" //add this line, that gives instructions to webpack, so it will go to search webpack.config.js and takes code inside it
      }

"build": "webpack"  => will use "webpack-cli" tool under the hood, which itself use "webpack" tool

4. in terminal

    npm run build

5. it will fail because we should remove in all files extention .js!
import { Timer } from './assets/scripts/Timer.js'; => import { Timer } from './assets/scripts/Timer';

6. in assets/scripts/app.js file was created by webpack

<hr>
<b>ESLint</b><br>
I installed ESLint extension in VScode. To make our project to be managible by npm, we should 
    
    npm init    
after answering all questions in the terminal package.json file was created by npm. Now we can install packages with npm 


    npm install <package name>  
all dependenses will be stored in package.json. If someone downloads my project from github, this package.json will show all dependences.
installe eslint

    
    npm instal --save-dev eslint 
--save-dev  because it is not a part of my project, eslint is not what I want to upload on server, it is just a development package to optimize code during development
node-modules folder was created on the project by npm, where eslint is and other packages that work with eslint.<br>
never change node-modules, it is third party package and it managed by npm<br>
if we want to share our code we should delete it<br>
because all dependencies info are in package.json and package-lock.json
In my package.json new dependency appeared

        "devDependencies": {
            "eslint": "^8.25.0"
        }
we also have package-lock.json that holds info dependency and all the dependencies of this dependency...<br>
ctrl+shift+p =>(opens command panel) and we should choose create eslint configuration<br>
on the terminal, there will be questions and we should answer them<br>
Now I can use ESLint in my project

<hr>
Although setTimeout() and then() are asynchronous the Promise then will executes first eventhough setTimeout set to 0


    setTimeout(() => console.log('asynchronous 1'), 0);
    Promise.resolve().then(() => console.log('asynchronous 2'));
<br>
