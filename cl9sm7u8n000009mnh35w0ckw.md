# How to Use Environment Files (.env) in Node.js | Tushar Khatri

When I was learning back-end development I encountered some major problems while hosting my apps, one of these was how to upload the code to a remote repository and at the same time prevent my secrets from being exposed.

> Data such as `API keys` or `Database URI` or any key from cloud services like `AWS` or `Google Cloud` are counted as secrets and are not to be exposed under any circumstance.

We need to learn about 2 things in order to keep our secrets unexposed i.e.
- dotenv or .env
- .gitignore

### dotenv

[dotenv](https://www.npmjs.com/package/dotenv) is a [npm](https://www.npmjs.com/) package, it helps us store secrets in a separate file called `.env`

- Run `npm i dotenv` from the root of the project to install the package.
- import and configure dotenv package in your project `require('dotenv').config();`. Make sure to require it at the very top of your app.js file so that it can be used throughout the project.
- Create a `.env` file in your root and add all the secrets in this file as shown below:

```shell
API_KEY=anyApiKey
SECRET_KEY=YOURSECRETKEYGOESHERE # comment
SECRET_HASH="something-with-a-#-hash"
```
You must use double quotes when reserved keywords like hash are appearing in the secret.

- Finally, you can use these secrets anywhere in your project by using `process.env.API_KEY`.

you just got to add your variable name after `process.env`.

```javascript
//import and configure dotenv at the very top of your file.
require('dotenv').config();

app.get((req, res) => {
      const apiKey = process.env.API_KEY;
      const secretKey = process.env.SECRET_KEY;
      const secretHash = process.env.SECRET_HASH;
      console.log(`Secrets stored in .env file are ${apiKey} and ${secretKey} and ${secretHash}`);
});
```

### `.gitignore`, exclude unwanted files from git tracking.

Now once you have your secrets in place you need to make sure that these variables must not appear in your commit history. Whenever you commit a change `git` saves all the newly added files and lines of code.  We can ignore tracking of these files by following the steps below:

- Create a `.gitignore` file in the root of your project.
- Add the file or folder name you want to ignore.

```.gitignore
/node_modules
.env
```

Adding `.env` in the `.gitignore` file tells `git` to ignore this file and any changes made to this file. Thus, making your secrets real secrets. Likewise by adding `/node_modules` git will ignore the whole folder.

### How to run your apps without uploading `.env` file to the cloud or hosting platform?

Every hosting provider has a tab called `Variable` where you can add the secrets and your app will have access to them.

![dotenv in nodejs | Tushar Khatri.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666965817910/qlIVAi3lL.png align="left")

You don't need to upload `.env` file anywhere, we use the `.env` file to access and store our secrets separately in our local environment and to safeguard them while uploading code to remote repos.

If this article was helpful please consider following me on [Instagram](https://instagram.com/tusharkhatri.in) or [GitHub](https://github.com/tusharkhatriofficial). If you wish to find more such articles in your inbox please consider subscribing to my [newsletter](https://blog.tusharkhatri.in/newsletter). Your appreciation is my fuel 😌.












