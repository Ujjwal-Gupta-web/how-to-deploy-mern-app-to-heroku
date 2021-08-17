## How to deploy a MERN app on Heroku
**(All steps explained.)**

*Solution to some of the common errors*

*Link to some great resources can be found in here.*

> Before  starting all these steps i consider that you people have installed [heroku cli](https://devcenter.heroku.com/articles/heroku-cli) and git into your system.

*These requirements may vary according to your OS, but make sure, both these things are installed on your os in whatever way they are supposed to be.*

**All these steps i am sharing are performed by me on a windows machine, so there might be some differences in other OS.**


### Steps to follow, to deploy a mern app on heroku-


**Step 1:**

**Move the client folder inside the server folder.**

While developing a MERN app it is regular practice to keep two folder one containing client (REACT) code and another containing server (node/express) code.


**Step 2:**
**Make sure to remove the proxy from the client package.json.**


**Step 3:**
**remove .git file if it is present in your client folder.**


**Step 4:**

```
cd server/client    // no need to do this if you are already inside this directory.
npm run build.     // this to create a build file which contains all the React content to be served
```


**Step 5:**

**After this there must be build folder inside the client folder.**

**Here  comes the crazy part,**

Now we will have to do some tweakings inside the package.json inside the server.

In the scripts part add the following commands-

the scripts section should look like this-
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build":"cd client && npm run build",
    "install-client":"cd client && npm install",
    "heroku-postbuild":"npm run install-client && npm run build",
    "start": "node app.js",
    "client":"cd client && npm start"    
  }
```

*basically all these are scripts that will run while the app will be deployed into production.*


**Step 6 (for those who havenot done it in begining):**
come into the server.js file-
```
Install dotenv package.
npm i dotenv
```

Create a **config.env** file in your root directory i.e. server folder.

Add these commands in your main
(which is generally app.js or index.js or server.js) node file-

`dotenv.config({path:'./config.env'});`

push confidential details like DB url and Api keys in it.

add dynamic `port process.env.PORT`

now replace DB with `process.env.DB`

replace API_KEYS with `process.env.API_KEYS`.

you got the point which i want to make here.


**Step 7:**
**now create a .gitignore (if not present) in the root folder.**
in the .gitignore folder write node_modules and config.env

what it does is, it helps in ignoring the pushing of these files when you push your files to git and hence maintain secrecy of the documents.


**Step 8:**
now in server.js -

add this condition-
```
if(process.env.NODE_ENV==='production'){
    app.use(express.static("client/build"));
    const path=require("path");
    app.get("*",(req,res)=>{
        res.sendFile(path.resolve(__dirname,'client','build','index.html'));
    })
}
```

in our case, the app will be deployed to heroku and heroku automatically sets the value of process.env.NODE_ENV  to production.

this part-
```
   const path=require("path");
    app.get("*",(req,res)=>{
        res.sendFile(path.resolve(__dirname,'client','build','index.html'));
    })
```
is required to serve the build files and prevent any problem, in some cases you might not require it.

i tried not using this part and found that sometimes it was making trouble in my client side routing when accessed through url.


**Step 9:**
now in cli write 
```
heroku login        // this will log you inside the heroku cli.
```

This will make you login in into heroku.


**Step 10:**
Visit [heroku.com](https://www.heroku.com/)

Click on **NEW** button and create a new app with the name you want your website to have in its domain name.

Now heroku will provide you steps to be followed further.

Perform them.

**After all this your application will be live in a while.**

**Congratulations.**

> NOTE : if you change anything in your app, make sure to update and push it to git by using the following commands.

```
git add .
git commit -am "message"
git commit heroku master.
```

### Error: (the terrific part, but a regular thing in a developer’s life)

**Error 1:**

you may find sometimes-

fatal: unable to access 'https://git.heroku.com/appname.git/': Could not resolve host: git.heroku.com

if you encounter something like this, check your internet connection once

or

try 

heroku login (which log you again into your heroku cli)


**Error 2:**

While doing 

git push heroku master

you may get errors like build failed, 

in that case-

scroll up and try to read if they have mentioned some error somewhere or not-

sometimes you have a error in your react app , like you have used wrong folder structure and you might 

have used some wrong conventions.

sometimes, it happens due to missing of the version of node in your package.json file,

you can check the version of node by using the command node -v

copy this version and paste that into your package.json file.

this type of error can also come due to missing of Procfile.



### Resources:

1. [How to deploy a MERN Stack App to Heroku by Esterling Accime](https://youtu.be/5PaUiPyBDJY)

2. [How to Deploy (Host) MERN Stack App on Heroku For Free in 2021 by Thapa Technical](https://youtu.be/7il2CrBA5VY)

3. [React routing error - Stack Overflow](https://stackoverflow.com/questions/41772411/react-routing-works-in-local-machine-but-not-heroku)

4. [React Routing not working - FreeCodeCamp](https://forum.freecodecamp.org/t/react-router-not-working-on-heroku/442908/5)

5. [heroku push gets rejected - Stack Overflow](https://stackoverflow.com/questions/39267808/heroku-push-gets-rejected)










