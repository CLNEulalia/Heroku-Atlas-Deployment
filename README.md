# Deployment for Node-Mongo-Express: Heroku & Atlas

## Learning Objectives

- Describe the difference between development, test, and production environments
- Use environmental variables to keep sensitive data out of code
- Set up a Mongo database using MongoDB's Atlas cloud database
- Deploy a Node/Express API using Heroku

## About Deployment

### What is Deployment?

Deployment is the act of putting an app up on one or more internet-connected
servers that allow users to access and use the app.

_Question:_ What changes in an application when it is deployed?

### Requirements for Deployment

There are generally a few things we need for an app to be properly deployed:

- **Server** - the server(s) must be on and connected to the internet
- **Executable Code** - we must get our code onto the server and be able to run
  it
- **Dependencies** - the server(s) must have the proper dependencies installed
- **Services** - the server(s) must be running the correct services (web,
  database, email, etc.)
- **Configuration** - we must configure our running app with respect to its
  deployment environment

### Deployment Approaches

There are lots of ways to do each of these steps. For example, we can get our
code onto a server by...

- Using FTP (File Transfer Protocol) to transfer the files onto the server
- Adding a git remote and using git push to transmit files (like with GH pages)
- Putting the files on a flash drive, fastening it to a homing pigeon's leg,
  then having an operator receive the pigeon and copy the files over to the
  server

## Environments and Environment Variables

### Environments

Application environments are an important part of the context in which an
application runs. Each environment is configured to support a certain usage of
the application.

Typical application environments include:

- **Development** - environment where an app or new feature(s) are created and
  run locally
- **Test** - environment where code and UI is tested for functionality and
  performance
- **Production** - environment for complete and tested code to be hosted online
  for clients to use

So far, we've been using the `development` environment by default. Today we'll
look at deploying to the `production` environment, a.k.a. the public, published
version of our site.

Each environment has a different set of configurations, things that vary
depending on how we're running or using our app. We could be developing,
testing, or deploying our apps. Configuration settings often include...

- The name of the database
- The username/password to connect to the database
- API authentication keys (e.g. to connect to twitter API)
- Whether or not to reload code on each request (for debugging vs performance)
- Where to save log information (error logs, etc)

### Environment Variables

We do not want to keep configuration in our codebase (e.g. the code we see when
we push to Github) for several reasons:

- We do not want to expose private information such as passwords and API Keys.
- When we push the same code to different environments, we need a way to
  dynamically tell which environment we're in.

Node manages application environments using "environment variables".

Environment variables store data and configuration information that is defined
outside of your codebase and pertain to the phase of the application's
development.

Storing this information separately protects sensitive information like API keys
and passwords because it is not visible from your project directory.

This is accomplished using `process`, a global object that comes with all node
projects. `process` has a property `env` where we store environment variables.

We can view a project's environment variables using the node REPL in our
terminal.

```bash
$ cd <your-node-project>
$ node
> process.env
```

- _Question:_ What are some of the listed variables? Why would they be stored
  here?

You can create a new environmental variable in the terminal too! (terminal, not
node!)

```bash
$ export <YOUR_ENVIRONMENTAL_VARIABLE_NAME>=<variableValue>
```

> **NOTE:**
>
> - Make sure there are no spaces next to the equals sign!
> - Environment variables tend to be SCREAMING_SNAKE_CASE by convention.

To test, try logging the following code from the node repl.

```js
> process.env.<YOUR_ENVIRONMENTAL_VARIABLE_NAME> = // whatever
```

If you'd like, you can also remove the environmental variable by exiting out of
the Node CLI and entering this command in the normal terminal:

```bash
$ unset <YOUR_ENVIRONMENTAL_VARIABLE_NAME>
```

### Bonus: dotenv

[dotenv](https://github.com/motdotla/dotenv) is a node package used to store
sensitive information in the environment. It's a handy tool but not strictly
necessary for this deployment.

## Solving Deployment Issues

**Not working?** Don't worry! Debugging is a part of your life now, and
deployment issues are normal. Check out these tips on solving deployment issues.

### Google is Your Best Friend

More often that not, solving deployment issues requires a good deal of Googling.
Don't expect to find a silver bullet -- often we must go through many different
issues other users may have encountered to understand our own.

What should you Google?

- If you aren't able to deploy, Google the error that shows up in your terminal
  after trying to push your app.
- If you are able to deploy but your app doesn't load/function properly, see
  what shows up after running `$ heroku logs --tail` in the terminal.

### Heroku Errors v. Node Errors

You may notice that the errors you receive after running `$ heroku logs --tail`
are not always detailed or particularly helpful.

- Instead of focusing on the Heroku errors, pay attention to the Node errors in
  your terminal. They will often provide you with more direction.
- Still stuck? Check out the
  [Heroku Error Codes Documentation](https://devcenter.heroku.com/articles/error-codes).

> **NOTE:**
>
> - A common error that students come across is file name case sensitivity.
>   Check out
>   [this documentation](https://stackoverflow.com/questions/10523849/changing-capitalization-of-filenames-in-git)
>   on changing the capitalization of filenames in Git.

### Help Each Other Out!

If you notice somebody running into the same problem as you, try working
together on debugging it!

# Deploying Node-Express-Mongoose Applications

Deploying our Node-Express-Mongoose application consists of 2 sets of steps.
First, we'll create a Heroku app and then set up a Mongo cloud database that our
Heroku app can connect to. Next, we'll verify our Node-Express-Mongoose
applications are able to connect to this new cloud-hosted database and, finally,
configure and deploy our application to Heroku by using Git to push our code to
their servers.

## You Do: Deploy Book-e JSON

Today, we will be deploying the solution repo of our Book-e JSON exercise. You
can download it from the link
[here](https://git.generalassemb.ly/sei-921/book-e-json).

1. Clone down the repo into your `sei/sandbox` directory.
1. Change into the project directory with `cd book-e-json`
1. Install dependencies with `npm i`.

### Heroku

We'll be using Heroku to deploy our Express apps because it simplifies and
expedites the process for deployment. Heroku automatically does the following...

- Starts up a new server when we run `heroku create`, and installs all the
  necessary services
- Adds a new remote to our Git repo, so we can just run `git push heroku main`
  to copy our code over
- Detects our database
- Detects the language our program is written in and chooses a buildpack
- Automatically installs our app's dependencies, and starts our app
- Easily change configuration information using `heroku config`

Heroku also has a FREE pricing tier!

### Mongo Atlas

[Mongo Atlas](https://cloud.mongodb.com) is a cloud-based database-as-a-service
that hosts a protected MongoDB instance that you can easily integrate with an
application deployed on Heroku.

Today, you will need to set up an Atlas account and database to host our
`book-e` database.

## Deployment Steps

Deployment is essentially an exercise in following directions. Follow the
step-by-step instructions below to deploy your Book-e JSON App. Pay attention to
the notes following each prompt! Fully read each prompt (including the notes)
before executing each step.

### Step A. Set up Heroku

1. Login to Heroku from the Terminal with `heroku login`.
1. From your project directory, create an app on Heroku

```bash
heroku create
```

> **NOTE:**
>
> - Make sure you are in your Book-e JSON App project directory before you run
>   this command
> - The `heroku create` command prepares Heroku to receive your code. Heroku
>   will randomly create an app name for you if you don't specify one. You may
>   need to login before you can create an app.
> - If you choose to name your application with `heroku create <your-app-name>`,
>   you will need to use a unique name. If the name is taken, Heroku will prompt
>   you to choose something new.
> - Running into an error that reads:
>   `! Unable to connect to Heroku API, please check internet connectivity and try again.`
>   See how to debug:
>   https://help.heroku.com/GOLWIGSP/why-can-t-i-connect-or-authenticate-with-the-heroku-command-line-cli

### Step B. Set up MongoDB Atlas

1. Go to [Mongo Atlas](https://cloud.mongodb.com) and sign up for an account, or
   sign in if you have one already.
1. Customize your organization and project names if you wish, then click
   continue.
   ![Name your organization and project](https://i.imgur.com/U8QCYLA.png)
1. Next you'll be presented with multiple pricing options. Choose the "Shared
   Cluster" FREE option, clicking the **Create a cluster** button.
1. On the next screen accept the default options for the cluster and click
   **Create cluster**. This will take a few minutes while your cluster is
   created and servers provisioned.
   ![Create your starter cluster](https://i.imgur.com/1uCgK16.png)
1. Click **Create your first database user** in the Getting Started wizard which
   will prompt you to choose **Database Access** from the left menu.
   ![Create your first database user](https://i.imgur.com/HySFYhY.png)
1. Click the **Add New Database User**. In the modal, make sure the
   **Authentication Method** is set to **Password**. Choose a username and then
   click the button for **Autogenerate Secure Password**. Before closing the
   modal, click **Copy** to copy the password and store it somewhere safe
   temporarily, such as in your notes app or text editor. **You'll need the
   username and password you use for your database, in a later step!** Leave the
   default value for Database User Privileges set to **Read and write to any
   database**. Don't forget to scroll to the bottom and click **Add User**!!!
   ![Create a database user](https://i.imgur.com/izeWdf0.png)

   > **NOTE:**
   >
   > - This is **not** the user with which you logged in to Atlas. "User" refers
   >   to an app that has access to your database, and **not your Atlas
   >   account/username**.
   > - Create a Database username and Database password that you will remember,
   >   or write it down somewhere. You will need this information again later.
   > - Do not use any special characters! Special characters can complicate the
   >   process when configuring your Atlas database with Heroku.
   > - Do not check 'Make read-only'. Full CRUD functionality will not work with
   >   a read-only database.

1. Click next option in the Getting Started wizard, **Whitelist your IP
   address**. This will prompt you to choose **Network Access** from the left
   menu.
1. Click the **Add IP Address** button, then in the modal, click the **Allow
   Access from Anywhere** button (or you can enter `0.0.0.0/0` manually) and
   press the green **Confirm** button.
   ![Add IP Access List Entry](https://i.imgur.com/0K8mC6L.png)
1. Skip the optional Load Sample Data step in the Getting Started wizard and
   instead choose the last option: **Connect to your cluster**.
1. In the connection modal, click the option to **Connect your application**.
   We'll come back here in a minute...

### Step C. Heroku & Node Setup

Next we need to let our Node app know _when_ to use Mongo Atlas as our database,
and when to use our local DB. **You do not need to make any changes to your
book-e solution code, but pay close attention to how we set up our app to
accommodate deployment.**

A built-in environment variable, `NODE_ENV` available under
`process.env.NODE_ENV`, is used to define the application environment. When a
Node app is deployed to Heroku, Heroku automatically sets the `NODE_ENV`
variable to `'production'`.

The code below is stating that we should use the Mongo Atlas URI (in other
words, the link that connects us to the Atlas database) when in production, and
the local database at all other times.

In other words, the code in our book-e Node app's `db/connection.js` file, the
environment will determine the `mongoURI` for our application to connect to.

```javascript
const mongoURI =
  //check if the node environment is production
  process.env.NODE_ENV === "production"
    ? //if so, use DB_URL as the database location
      process.env.DB_URL
    : //if not, use the book-e db on the MongoDB's local server
      "mongodb://localhost/book-e";
```

> **NOTE:**
>
> - In the example above, the link to the MongoDB includes the name of the
>   database we are using, which in this case, is `book-e`. When using a
>   different database for your own projects, make sure you include the name of
>   the actual database you want to connect.

Similarly, when Heroku starts your app it will automatically assign a port to
`process.env.PORT` (an environmental variable!) to be used in production. In our
book-e `index.js` file, notice that we use `app.listen` to accommodate Heroku's
production port and our own local development port.

```js
app.set("port", process.env.PORT || 8000);

app.listen(app.get("port"), () => {
  console.log(`✅ PORT: ${app.get("port")} 🌟`);
});
```

Heroku looks for instruction when starting your app. In this case, that
instruction is to run `node index.js`. In `package.json` under `scripts`, note
that we have the following:

```json
"scripts": {
    "start": "node index.js"
}
```

### Step D. Heroku & Atlas Configuration

1. Go back to your Atlas database in your browser. Click "Connect", the button
   next to "Metrics" and "Collections". In the connection modal that pops up,
   click "Connect your application", then click the copy button to copy the
   connection string. Paste the connection string in your notes app or text
   editor to edit it.
1. Replace the `<password>` portion of the url with your auto-generated password
   that you saved when you created your database user earlier. Do not include
   the angle brackets `<>` around your password.
1. Set the URI you just copied **with your password added** as an environment
   variable called `DB_URL` using `heroku config:set` in your terminal where you
   created your Heroku app. Use the following commands as an example (make sure
   to run the config:set command in your project's directory and replace
   `<password>` with your auto-generated password):

```bash
heroku config:set DB_URL="mongodb+srv://yourusername:<password>@cluster0-zoapi.mongodb.net/test?retryWrites=true&w=majority"
# CONFIRM
heroku config
```

![Configuring your Heroku app's DB_URL variable](https://i.imgur.com/2HeSXMd.png)

Note: You can also set environmental variables for your Heroku app from within
Heroku. Log into your Heroku account, go to your app's dashboard, click on
settings, and click "Reveal Config Vars". Use the pencil icon to edit existing
ones or add new key-value pairs manually.

## Step E. Deploying to Heroku

1. Run `git status` to make sure that there are no changes to add or commit and
   then push your code to Heroku remote with `git push heroku main`.

1. Seed your Atlas database by running the command:

```bash
heroku run node db/seeds.js
```

> **NOTE:**
>
> - `heroku run` allows you to run js files on the heroku server. We can seed
>   our database on heroku using the same seed file we used locally.

1. Open your application! Run the command `$ heroku open` in your terminal. This
   will launch your production app in a new browser tab.

1. You just successfully deployed your first app! You should be proud, so pat
   yourself on the back, give your neighbor a high five, call your parents, and
   share this milestone with someone you love!

   ![Michelle Tanner](https://media.giphy.com/media/YJ5OlVLZ2QNl6/giphy.gif)

## Additional Resources

- [About Environments](about-environments.md)
- [12-Factor Apps in Plain English](http://www.clearlytech.com/2014/01/04/12-factor-apps-plain-english/)
- [12-Factor Principles](https://12factor.net/)

## License

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
2. All software code is licensed under GNU GPLv3. For commercial use or
   alternative licensing, please contact [legal@ga.co.](mailto:legal@ga.co)
