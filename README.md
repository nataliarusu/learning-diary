# learning-diary
Although setTimeout() and then() are asynchronous the Promise then will executes first eventhough setTimeout set to 0


    setTimeout(() => console.log('asynchronous 1'), 0);
    Promise.resolve().then(() => console.log('asynchronous 2'));
<br>
<i>21/10/22</i><br>
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

<i>22/10/22</i><br>
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

<i>25/1/22</i><br>
An async function without any awaits is allowed. All async functions return promises, even if they don't await. So, the function's return value will automatically be wrapped in a fulfilled promise.
