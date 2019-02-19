# Deployment for Node-Mongo-Express: Heroku & MLab

## Learning Objectives

- Define deployment
- Describe the difference between development, test, and production environments
- Deploy a Node App using Heroku
- Set up a Mongoose database using MLab
- Describe the major points of a 12-factor application
- Use environmental variables to keep sensitive data out of code

## About Deployment

### What is Deployment?

Deployment is the act of putting an app up on one or more internet-connected servers that allow users to access and use the app.
> NOTE: What changes in an application when it is deployed?

### Requirements for Deployment

There are generally a few things we need for an app to be properly deployed:

- **Server** - the server(s) must be on and connected to the internet
- **Services** - the server(s) must be running the correct services (web, database, email, etc.)
- **Dependencies** - the server(s) must have the proper dependencies installed
- **Executable Code** - we must get our code onto the server and be able to run it
- **Configuration** - we must configure our running app with respect to its deployment environment

### Deployment Approaches

There are lots of ways to do each of these steps. For example, we can get our code onto a server by...

- Using FTP (File Transfer Protocol) to transfer the files onto the server
- Adding a git remote and using git push to transmit files (like with GH pages)
- Putting the files on a flash drive, fastening it to a homing pigeon's leg, then having an operator receive the pigeon and copy the files over to the server

## Environments and Environmental Variables

### Environments

Application environments are an important part of the context in which an application runs. Each environment is configured to support a certain usage of the application.

Typical application environments include:

- **Development** - environment where an app or new feature(s) are created and run locally
- **Test** - environment where code and UI is tested for functionality and performance
- **Production** - environment for complete and tested code to be hosted online for clients to use

So far, we've been using the `development` environment by default. Today we'll look at deploying to the `production` environment, a.k.a. the public, published version of our site.

Each environment has a different set of configurations, things that vary depending on how we're running or using our app. We could be developing, testing, or deploying our apps. Configuration settings often include...

- The name of the database
- The username/password to connect to the database
- API authentication keys (e.g. to connect to twitter API)
- Whether or not to reload code on each request (for debugging vs performance)
- Where to save log information (error logs, etc)

### Environmental Variables

 Node manages application environments using 'environmental variables'. Environmental variables store data and configuration information that is defined outside of your codebase and pertain to the phase of the application's development. Storing this information separately protects sensitive information like API keys and passwords because it is not visible from your project directory.

This is accomplished using `process`, a global object that comes with all node projects. `process` has a property `env` where we store environmental variables.

We can view a project's environmental variables using the node repl in our terminal. In your terminal, 

```bash
$ cd <your-node-project>
$ node
> console.log(process.env)
```
> NOTE: What are some of the listed variables? Why would they be stored here?

You can create a new environmental variable in the terminal too! (terminal, not node!)

```bash
$ export <YOUR_ENVIRONMENTAL_VARIABLE_NAME>=<variableValue>
```
> NOTE: Make sure there are no spaces next to the equals sign!

> NOTE: Environment variables tend to be SCREAMING_SNAKE_CASE by convention.

To test, try logging the following code from the node repl.
```bash
process.env.<YOUR_ENVIRONMENTAL_VARIABLE_NAME> 
```

#### **Bonus: dotenv**

[dotenv](https://github.com/motdotla/dotenv) is a node package used to store sensitive information in the environment. It's a handy tool but not strictly necessary for this deployment. It is a fantastic practice, and accords with [12-factor principles](https://12factor.net/).

## Break (10 min)

## Deploying Node-Express-Mongoose Applications

Deploying our Node-Express-Mongoose application consists of 2 sets of steps. First, we'll sign up for Heroku, download the Heroku CLI, and set up a Mongo database that our app can connect to. Next, we'll configure our Node-Express-Mongoose applications to connect to this new cloud-hosted database and finally, deploy our application to Heroku.

## You Do: Deploy 'Todos App'

Today, we will be deploying the 'solution' branch of our Todo excercise. You can clone it [here](https://git.generalassemb.ly/dc-wdi-node-express/express-to-do/tree/solution).
> NOTE: When you clone a repo, it will clone down the master branch. Make sure you `git checkout solution` to switch to the ***solution*** branch once you've cloned!

### Heroku

We'll be using a service called Heroku to deploy our apps, because it makes all the steps for deployment easy, simplifying and expediting the process. For example, Heroku automatically does the following...

- Starts up a new server when we run `heroku create`, and installs all the necessary services
- Adds a new remote to our Git repo, so we can just run `git push heroku master` to copy our code over
- Detects our database
- Detects the language our program is written in and chooses a buildpack
- Automatically installs our app's dependencies, and starts our app
- Easily change configuration information using `heroku config`

Heroku also has a FREE pricing tier!

### MLab

MLab (`mlab.com`) is a cloud-based database service that hosts a protected MongoDB that can be easily integrated with a Heroku application.

Today, we will set up an MLab account and database to host our `todo` database from MongoDB.

### Deployment in 20 Easy Steps

Deployment is essentially an exercise in following directions. Follow the step-by-step instructions below to deploy your Todo App. Pay attention to the notes and hints following each prompt! Fully read each prompt (including the notes) before executing each step.

#### Set up Heroku

1. Sign up for a free [Heroku](https://www.heroku.com/) account.

2. Follow the instructions [here](https://devcenter.heroku.com/articles/heroku-cli) to download the Heroku CLI.
    > NOTE: In your terminal, you will run the command `brew tap heroku/brew && brew install heroku`

3. From your project directory, create an app on Heroku `$ heroku create <your-app-name>`
    > NOTE: Make sure you are in your Todo App project directory before you run this command

    > NOTE: `heroku create` prepares Heroku to receive your code. Heroku will randomly create an app name for you if you don't specify one

    > NOTE: If you choose to name your application, you will need to use a unique name (something someone else has not used before). If the name is taken, Heroku will prompt you to choose something new.

#### Set up MLab

4. Go to [MLab](https://mlab.com/) and sign up for an account, or sign in if you have one already.

5. Create a new database by clicking the '+ Create new' button in MongoDB Deployments.

// ADD SCREEN SHOT HERE

6. Select AWS (Amazon Web Services) as your Cloud Provider and "Sandbox" (the free version!) as the Plan Type. Click Continue at the right bottom of the screen.

// ADD SCREEN SHOT HERE

7. Choose your region and click Continue.

8. Name your database and click Continue. You will be redirected to the Order Confirmation page. Review your order (make sure it's free!) and click Submit Order.

9. You will be redirected to your MLab account homepage, where your databases are listed. Click on the database you've just created.

10. Next, you will create a new user. Click on the Users tab and click 'Add database user'. You will be prompted to provide a Database username and Database password.
    > NOTE: This is **not** the user with which you logged in to MLab. "User" refers to an app that has access to your database, and **not your MLab account/username**.

    > NOTE: Create a Database username and Database password that you will remember, or write it down somewhere. You will need this information again later.

    > NOTE: Do not use any special characters! Special characters can complicate the proceess when configuring your MLab database with Heroku.

    > NOTE: Do not check 'Make read-only'. Full CRUD functionality will not work with a read-only database.

// ADD SCREEN SHOT HERE

#### Heroku & Node Setup

Next we need to let our Node app know *when* to use MLab as our database, and when to use our local DB.

A built-in environment variable, `NODE_ENV` available under `process.env.NODE_ENV`, is used to define the application environment. When a Node app is deployed to Heroku, Heroku automatically sets the `NODE_ENV` variable is set as `'production'`.

The code below is stating that we should use the MLab URI (in other words, the link that connects us to the MLab database) when in production, and the local database at all other times.

11. In your Node app's `db/connection.js` file, or wherever you have `mongoose.connect()`, add the following...

    ```js
    if (process.env.NODE_ENV == "production") {
        mongoose.connect (process.env.MLAB_URL)
    } else {
        mongoose.connect("mongodb://localhost/todo")
    }
    ```
    > NOTE: In the example above, the link to the MongoDB includes the name of the database we are using, which in this case, is `todo`. When using a different database for your own projects, make sure you include the name of the actual database you want to connect.

12. Next, you will need to make a minor change to `index.js`. When Heroku starts your app it will automatically assign a port to `process.env.PORT` (an environmental variable!) to be used in production. We can modify `app.listen` to accomodate Heroku's production port and our own local development port. Add the following to `index.js`...

    ```js
    app.set('port', process.env.PORT || 3001)

    app.listen(app.get('port'), () => {
        console.log(`✅ PORT: ${app.get('port')} 🌟`)
    })
    ```

13. Heroku looks for instruction when starting your app. In this case, that instruction is to run `node index.js`. In `package.json` under `script`, add the following...

    ```js
    `"start": "node index.js"`
    ```
    > NOTE: Another way to do this is to define a [Procfile](https://devcenter.heroku.com/articles/getting-started-with-nodejs#define-a-procfile) in the root of your directory and include the line `web: node index.js`.

14. Add, commit and push to the **solution branch** of the Todo App.

#### Heroku & MLab Configuration

15. Go back to your MLab database in your browser. At the top of the page under **To connect using a driver via the standard MongoDB URI**, copy the MongoDB URI.

    > NOTE: You must copy this from your own database to capture your unique database id numbers.

    > NOTE: You will still need to manually substitue the `dbuser` and `dbpassword` with the one you created in the next step.

16. Set the URI you just copied as an environment variable called `MLAB_URL` using `heroku config:set`, filling in the `dbuser` and `dbpassword` you created on the "Users" page. Run the following in your project folder...

    ```bash
    $ heroku config:set MLAB_URL=mongodb://<your_user_login>:<your_user_password>@ds012345.mlab.com:12345/<your_db_name>
    ```
    > NOTE: Do not copy the above command. The database id numbers in the URI should match the URI that you copied from your own MLab database. The **sample** command above includes id numbers of `012345` and `12345`. Does your own MLab URI match this? Probably not.

    > NOTE: Your database name will be included in the URI you copied from your MLab database. You will need to manually add the `dbuser` and `dbpassword` that you created in Step 10.

    > NOTE: Assigning environmental variables using `heroku config:set` is very similar to using `export`, the difference being accessibility. Variables assigned using the heroku command are only accessible from the production app deployed on heroku.

#### Deploying to Heroku

17. Push your code to Heroku remote. Because you are on the `solution` branch of the Todo App, you will need to run `$ git push heroku solution:master` in your terminal. This ensures that your most up-to-date code -- a.k.a. our `solution` branch is deployed.
    > NOTE: If you are deploying to Heroku from the master branch, you can run the command `$ git push heroku master`.

18. Seed your MLab database by running the command `$ heroku run node db/seed.js`.
    > NOTE: `heroku run` allows you to run js files on the heroku server. We can seed our database on heroku using the same seed file we used locally.

19. Open your application! Run the command `$ heroku open` in your terminal. This will launch your production app in a new browser tab.

20. You just successfully deployed your first app! You should be proud, so pat yourself on the back, give your neighbor a high five, call your parents, and share this milestone with someone you love.

## Solving Delpoyment Issues

**Not working?** Don't worry! Debugging is a part of your life now. Check out these tips on solivng deployment issues.

### Google is Your Best Friend

More often that not, solving deployment issues requires a good deal of Googling. Don't expect to find a silver bullet -- often we must go through many different issues other users may have encountered to understand our own.

What should you Google?

- If you aren't able to deploy, Google the error that shows up in your terminal after trying to push your app.
- If you are able to deploy but your app doesn't load/function properly, see what shows up after running `$ heroku logs --tail` in the terminal.

### Heroku Errors v. Node Errors

You may notice that the errors you receive after running `$ heroku logs --tail` are not always detailed or particularly helpful.

- Instead of focusing on the Heroku errors, pay attention to the Node errors in your terminal. They will often provide you with more direction.
- Still stuck? Check out the [Heroku Error Codes Documentation](https://devcenter.heroku.com/articles/error-codes).

> NOTE: A common error that students come across is file name case sensitivity. Check out [this documentation](https://stackoverflow.com/questions/10523849/changing-capitalization-of-filenames-in-git) on changing the capitalization of filenames in Git.

### Help Each Other Out!

If you notice somebody running into the same problem as you, try working together on debugging it!
