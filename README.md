# learning-diary
Although setTimeout() and then() are asynchronous the Promise then will executes first eventhough setTimeout set to 0


    setTimeout(() => console.log('asynchronous 1'), 0);
    Promise.resolve().then(() => console.log('asynchronous 2'));
<br>
<i>21/10/22</i><br>
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

