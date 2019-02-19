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
> What changes in an application when it is deployed?

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
> What are some of the listed variables? Why would they be stored here?

You can create a new environmental variable in the terminal too! (terminal, not node!)

```bash
$ export <YOUR_ENVIRONMENTAL_VARIABLE_NAME>=<variableValue>
```
> Make sure there are no spaces next to the equals sign!

> Environment variables tend to be SCREAMING_SNAKE_CASE by convention.

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
> When you clone a repo, it will clone down the master branch. Make sure you `git checkout solution` to switch to the ***solution*** branch once you've cloned!

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

### Deployment Step-by-Step

Deployment is essentially an exercise in following directions. Follow the step-by-step instructions below to deploy your Todo App. Pay attention to the notes and hints following each prompt! Fully read each prompt (including the notes) before executing each step.

#### Set up Heroku

1. Sign up for a free [Heroku](https://www.heroku.com/) account.

2. Follow the instructions [here](https://devcenter.heroku.com/articles/heroku-cli) to download the Heroku CLI.
    > In your terminal, you will run the command `brew tap heroku/brew && brew install heroku`

3. From your project directory, create an app on Heroku `$ heroku create <your-app-name>`
    > Make sure you are in your Todo App project directory before you run this command

    > `heroku create` prepares Heroku to receive your code. Heroku will randomly create an app name for you if you don't specify one

    > If you choose to name your application, you will need to use a unique name (something someone else has not used before). If the name is taken, Heroku will prompt you to choose something new.

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
    > This is *not* the user with which you logged in to MLab. "User" refers to an app that has access to your database, and ***not your mlab account/username***.

    > Create a Database username and Database password that you will remember, or write it down somewhere. You will need this information again later.

    > Do not use any special characters! Special characters can complicate the proceess when configuring your MLab database with Heroku.
    
    > Do not check 'Make read-only'. Full CRUD functionality will not work with a read-only database.

// ADD SCREEN SHOT HERE


