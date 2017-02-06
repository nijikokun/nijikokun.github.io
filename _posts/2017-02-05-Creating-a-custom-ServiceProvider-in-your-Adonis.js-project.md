---
layout: post
date: 2017-02-05 04:28:20
title: Creating a custom ServiceProvider in your Adonis.js project
---

I recently found [Nuxt][0], and by extension re-discovered a wonderful Node.js framework named [Adonis][1]. Since then 
I've been slowly developing a small project as a learning experience and I would like to share with you something I found to be 
missing from the Adonis documentation.

When you're using a third-party library such as [Firebase][2] in your project, usually you include the module and instantiate 
the module with configuration values each time you use it or you create a global. Adonis has two features called **IoC** 
and **ServiceProviders** to prevent you from having to do that and a centralized configuration directory to prevent configuration 
values being hardcoded into your codebase. 

The problem is that Adonis doesn't describe _where_ to put your custom services, _when_ to use these features, 
or _how_ to tell your Adonis application they exist to allow you to use them.

So I'd like to share with you what I've learned about creating a custom **ServiceProvider** in Adonis.

## Creating the **ServiceProvider** 

Service providers are placed within the `providers/` directory in your Adonis application. The difference between a **Simple Provider** 
and a **ServiceProvider** is that the **Simple Provider** doesn't get hoisted onto the Adonis application and it doesn't have access 
to the Adonis application context object. **Simple Providers** are also stored in the `providers/` directory and **ServiceProviders** 
can include them to hoist them onto the Adonis application.

First we create our **Simple Provider** for Firebase in a file named `providers/firebaseService.js`:

```js
class FirebaseService {
  constructor (Config) {
    // Include our firebase-admin module
    // Find out more at: https://firebase.google.com/docs/admin/setup
    const FirebaseAdmin = require('firebase-admin')

    // Obtain Firebase configuration from config/services.js
    // Create the file if it doesn't exist and Adonis will pick it up
    const FirebaseConfig = Config.get('services.firebase') 

    // Initialize our priviledged firebase admin application
    FirebaseAdmin.initializeApp({
      credential: FirebaseAdmin.credential.cert({
        projectId: FirebaseConfig.credentials.projectId,
        clientEmail: FirebaseConfig.credentials.clientEmail,
        privateKey: FirebaseConfig.credentials.privateKey
      }),

      databaseURL: FirebaseConfig.databaseURL
    })

    // Return our FirebaseAdmin object
    return FirebaseAdmin
  }
}

module.exports = FirebaseService
```

Now we need to create our **ServiceProvider** that consumes our **Simple Provider** we just created in a new file named 
`providers/firebaseServiceProvider.js`:

```js
const ServiceProvider = require('adonis-fold').ServiceProvider
const FirebaseService = require('./firebaseService')

class FirebaseProvider extends ServiceProvider {
  * register () {
    // Register our ServiceProvider on the namespace: Adonis/Services/Firebase
    // Obtain application reference: app
    this.app.bind('Adonis/Services/Firebase', (app) => {
      // Obtain application configuration in config/
      const Config = app.use('Adonis/Src/Config')

      // Export our service
      return new FirebaseService(Config)
    })
  }
}

module.exports = FirebaseProvider
```

Great, we now have our service provider, how do we tell our Adonis application how to use it?

## Create our **ServiceProvider** definition in our Adonis application

Service definitions are setup in the `bootstrap/app.js` file on the **aliases** object contained within. 
We don't need to reference the file location of our **ServiceProvider**, simply the namespace.

```js
const aliases = {
  // ...
  Firebase: 'Adonis/Services/Firebase',
  // ...
}
```

## Start using our **ServiceProvider**

Now inside of our **Controllers** or **Models** we can use our Firebase **ServiceProvider** like so:

```js
const Firebase = use('Adonis/Services/Firebase')

// Retrieve services from Firebase
const FirebaseAuth = Firebase.auth()
const FirebaseDatabase = Firebase.database()

// Initialize a second application
const SecondFirebaseApp = Firebase.initializeApp(/* ... config ... */, "second")
```

Want to learn more about [firebase-admin][3]? Just click that link.

Now you're ready to implement your own custom services, good luck and until next time... Stay safe!

[0]: https://github.com/nuxt/nuxt.js
[1]: http://adonisjs.com/
[2]: https://firebase.google.com/
[3]: https://firebase.google.com/docs/admin/setup
