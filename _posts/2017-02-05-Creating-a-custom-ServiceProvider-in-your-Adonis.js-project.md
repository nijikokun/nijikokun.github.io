---
layout: post
date: 2017-02-05 04:28:20
title: Creating a custom ServiceProvider in your Adonis.js project
---

Recently found [Nuxt][0], and by extension re-discovered a wonderful node framework named [Adonis][1]. Since then 
I've been slowly developing a small project to learn both frameworks and I've found that while Adonis has 
beautiful documentation it's not as helpful as it could be.

When you're using a third-party library such as [Firebase][2], you don't want to require the module and pass 
your configuration values every single time you need it. Adonis has **IoC** and **ServiceProviders** for that. 

The problem is that Adonis doesn't describe _where_ to put your custom services, _when_ to use an **IoC**, 
or _how_ to tell your existing application to know they exist.

So here is how you create and setup your own custom service.

## Create a **ServiceProvider** and place it inside of your `providers/` directory.

```js
const ServiceProvider = require('adonis-fold').ServiceProvider

class FirebaseProvider extends ServiceProvider {
  * register () {
    // Register our ServiceProvider on the namespace: Adonis/Services/Firebase
    // Obtain application reference: app
    this.app.bind('Adonis/Services/Firebase', (app) => {
      // Include our firebase-admin module
      // Find out more at: https://firebase.google.com/docs/admin/setup
      const Firebase = require('firebase-admin')

      // Obtain application configuration in config/
      const Config = app.use('Adonis/Src/Config')

      // Obtain Firebase configuration from config/services.js
      // Create the file if it doesn't exist and Adonis will pick it up
      const FirebaseConfig = Config.get('services.firebase') 

      // Initialize our priviledged firebase admin application
      Firebase.initializeApp({
        credential: Firebase.credential.cert({
          projectId: FirebaseConfig.credentials.projectId,
          clientEmail: FirebaseConfig.credentials.clientEmail,
          privateKey: FirebaseConfig.credentials.privateKey
        }),
        databaseURL: FirebaseConfig.databaseURL
      })

      // Return our Firebase to be registered on the namespace
      return Firebase
    })
  }
}
```

Great, we now have a service provider, how do we tell our application how to use it?

## Define our **ServiceProvider** inside `bootstrap/app.js` on the **aliases** object.

```js
const aliases = {
  // ...
  Firebase: 'Adonis/Services/Firebase',
  // ...
}
```

## Start using your **ServiceProvider**

Now inside of our **Controllers** or **Models** we can use our Firebase service like so:

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
