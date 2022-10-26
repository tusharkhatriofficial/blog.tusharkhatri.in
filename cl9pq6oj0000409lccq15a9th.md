# Send automated emails from your node.js app | Google OAuth2 and Nodemailer.

I was searching for a viable option for the contact form of my personal website, believe me this is the best you can do for your forms.

# Get all the required secrets from Google Developer Console. 

Before doing anything in our code we have to first create a project in the [Google Developers Console](https://console.developers.google.com/).

### Create Project.

![Screenshot 2022-10-26 at 11.41.58 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666765066614/cpDweiQpH.png align="left")

### Rename the project.

![Screenshot 2022-10-26 at 11.51.19 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666765347345/kbTl9TgYA.png align="left")

### Configure the OAuth consent screen in the `Test Mode`.
> You may need to verify your website to make it work in production mode as test secrets are only valid for 7 days.

- Select User Type as External and click CREATE.

![Screenshot 2022-10-26 at 11.53.38 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666765679111/lMkzenIqS.png align="left")

- Fill only the mandatory fields for now as we want to stay in the test mode.


![Screenshot 2022-10-26 at 12.04.27 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666766195922/8u8JPV8jT.png align="left")
![Screenshot 2022-10-26 at 12.05.04 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666766208360/SeT3GGM-i.png align="left")

- Make no changes to the Scopes, click SAVE AND CONTINUE.


![Screenshot 2022-10-26 at 12.10.06 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666766493306/vp5Bq6_lJ.png align="left")

- Enter the email address you want to send automatic emails from.


![Screenshot 2022-10-26 at 12.14.24 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666766929879/ix5d3Dcc7.png align="left")

- Check the Summary and click BACK TO DASHBOARD.

### Create Credentials.

- Navigate to the `Credentials` tab on the sidebar and click on `+ CREATE CREDENTIALS` and `OAuth client ID`.


![Screenshot 2022-10-26 at 12.32.10 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666767821333/dkg0PpcdB.png align="left")

- Fill all the mandatory fields and set `Authorized redirect URIs` to `https://developers.google.com/oauthplayground`.


![Screenshot 2022-10-26 at 12.38.35 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666768172373/QG6Z5x8Vj.png align="left")

- Copy your `Client Id` and `Client Secret` to a safe place as we will use them later.

![Screenshot 2022-10-26 at 12.41.17 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666768425701/2luj0MUUL.png align="left")

### Authorize API

- Head over to `https://developers.google.com/oauthplayground` and fill in the recently generated `Client Id` and `Client Secret` in the `OAuth 2.0 configuration` shown below.

![Screenshot 2022-10-26 at 12.59.01 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666769530319/l06od8jUl.png align="left")

- fill in `https://main.google.com` in the `Authorize API's field` and click `Authorize API's`.

![Screenshot 2022-10-26 at 1.09.39 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666770011781/-AQvO_rZb.png align="left")

As you hit `Authorize API's` button, Google will ask you to verify your Gmail Id. Make sure to verify the Id you entered as the `Test User` while configuring the consent screen.

![Screenshot 2022-10-26 at 1.14.38 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666771046683/TaywJB7bN.png align="left")
![Screenshot 2022-10-26 at 1.15.18 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666771291617/EjODr4Lv4.png align="left")
![Screenshot 2022-10-26 at 1.15.52 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666771311526/6pSXp6gSb.png align="left")

- Click on `Exchange Authorization code for tokens` and copy the `Refresh token` from here.

We don't need to copy the `Access token` from here as it is not permanent and keeps on expiring. We will generate an `Access token` on the fly as and when needed from our node app.

![Screenshot 2022-10-26 at 1.16.39 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666771328531/8MD3NDz8R.png align="left")

### Create a `.env` file in the root folder of your project and include all the secrets listed below in the same format.
> you have to install and configure `dotenv` package to make this work. If you are not sure what that is, please check the [installation guide here!](https://www.freecodecamp.org/news/how-to-use-node-environment-variables-with-a-dotenv-file-for-node-js-and-npm/#:~:text=To%20use%20DotEnv%2C%20first%20install,config()%20.)

```shell
CLIENT_ID="clientIdFromGoogleDeveloperConsole"
CLIENT_SECRET="clientSecretFromGoogleDeveloperConsole"
REDIRECT_URI="https://developers.google.com/oauthplayground"
REFRESH_TOKEN="refreshTokenFromGoogleDeveloperConsole"
```

### Create a form with the `post` method.

```html
<form action="/submit" method="post">
      <input type="text" name="name" placeholder="Your Name" id="name">
      <input type="email" name="email" placeholder="Email" id="email">
      <textarea name="message" rows="6" id="message" placeholder="Message"></textarea>
      <button type="submit">
</form>
```

### Install the required packages

Run the following command from the root of your project to install `googleapis` and `nodemailer` [npm](https://www.npmjs.com/) packages.

```shell
npm i googleapis nodemailer
```

### Configure oAuth2Client using the secrets stored in the `.env` file.

```javascript
const oAuth2Client = new google.auth.OAuth2(process.env.CLIENT_ID, process.env.CLIENT_SECRET, process.env.REDIRECT_URI);

oAuth2Client.setCredentials({ refresh_token: process.env.REFRESH_TOKEN});
```

### Handle `post` request and write the function to send emails

The form we created recently will send a `post` request to our server with the required form data, we will store this form data in Javascript constants.

```javascript
app.post("/submit", (req, res) => {
       const sender name = req.body.name;
       const senderEmail = req.body.email;
       const senderMessage = req.body.message;
});
```
We will now write a function that takes `senderName, senderEmail, senderMessage` as parameters. 

- Create a try-catch block inside the function
- In the try block generate the ACCESS_TOKEN on the fly using the `oAuth2Client.getAccessToken();`
- Create a  transport using `nodemailer.createTransport({});`
- Create an object `mailOptions` with the following key-value pairs.
```javascript
const mailOptions = {
      from: "",
      to: "",
      subject: ``,
      text: ``,
    }
```
- Use `transport.sendMail(mailOptions);` to send the email.
- Catch the error if any in the catch block.

### This is how the function must look after putting everything together.

```javascript
async function sendMail(senderName, senderEmail, senderMessage){
  try{
    const ACCESS_TOKEN = await oAuth2Client.getAccessToken();
    const transport = nodemailer.createTransport({
      service: 'gmail',
      auth: {
        type: 'OAuth2',
        user: 'developersonline.org@gmail.com',
        clientId: process.env.CLIENT_ID,
        clientSecret: process.env.CLIENT_SECRET,
        refreshToken: process.env.REFRESH_TOKEN,
        accessToken: ACCESS_TOKEN,
      },
    });

    const mailOptions = {
      from: "Bot <developersonline.org@gmail.com>",
      to: "hello@tusharkhatri.in",
      subject: `${senderEmail} sent you a message`,
      text: `Message from ${senderName}: ${senderMessage}`,
    }

    const result = await transport.sendMail(mailOptions);
    return result;


  }catch (error) {
    return error;
  }
}
```

### Call the function. ✌️

```javascript
app.post("/submit", (req, res) => {
       const sender name = req.body.name;
       const senderEmail = req.body.email;
       const senderMessage = req.body.message;
      
       sendMail(senderName, senderEmail, senderMessage)
       .then(result =>  console.log("Message Sent"))
       .catch(error => console.log(error.message));
});
```

If this article was helpful please consider following me on [Instagram](https://instagram.com/tusharkhatri.in) or [GitHub](https://github.com/tusharkhatriofficial). If you wish to find more such articles in your inbox please consider subscribing to my [newsletter](https://blog.tusharkhatri.in/newsletter). Your appreciation is my fuel 😌.












