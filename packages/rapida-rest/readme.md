![Rapida Logo](https://user-images.githubusercontent.com/13030990/71755098-db251e80-2e91-11ea-8ce9-25349e6c087f.png)

**Rapida REST** - An abstraction helper for REST API that creates an abstract controller that communicate with the database in a consistent way.

**Warning** - Rapida REST has been deprecated and it's no longer maintained.

This package is part of [Rapida](https://github.com/yahiarefaiea/rapida).

## Usage
In your project directory, install `express`, `mongoose`, and `@rapida/rest` by running:
```
npm i express mongoose @rapida/rest --save
```

Import `express`, `mongoose`, and `@rapida/rest` to your project:
```js
const express = require('express')
const mongoose = require('mongoose')
const AbstractController = require('@rapida/rest')
```

After setting up express and mongoose, create a new instance of the `AbstractController` and pass a model to it:
```js
const app = express()
mongoose.connect('mongodb://localhost:27017/rapida-rest')

const Model = mongoose.model('Book', new mongoose.Schema({title: String, author: String}))
const controller = new AbstractController(Model)
```

Now, you have all the endpoints needed for you to use:
```js
app.get('/book', controller.getAll)
```

## Methods
The available methods are:
+ `controller.getAll`: Get (read) all resources.
+ `controller.post`: Post (create) a new resource.
+ `controller.get`: Get (read) an existing resource.
+ `controller.patch`: Patch (update) an existing resource.
+ `controller.delete`: Delete an existing resource.

**Note**: The `get`, `patch`, and `delete` methods must have an `:id` specified in the Express params object (`req.params.id`). Check the [Advanced Usage](#advanced-usage) to see an example.

## Advanced Usage
You can extend the `AbstractController` to update a specific endpoint (or maybe create a new one):
```js
// controller.js
const AbstractController = require('@rapida/rest')
const Model = require('./path/to/model')

class Controller extends AbstractController {
  constructor(model) {
    super(model)
  }

  getAll(req, res, next) {
    // do something here before calling the super.getAll()
    super.getAll(req, res, next)
  }
}

module.exports = new Controller(Model)
```

**Note**: The code above uses ES6 syntax which uses the `class` keyword. You might need to use [babel](https://github.com/babel/babel) to act as your transpiler.

And now you can use the controller anywhere in your project:
```js
// anywhere.js
const controller = require('./path/to/controller')
const router = express.Router()

router.route('/')
  .get(controller.getAll)
  .post(controller.post)

router.route('/:id')
  .get(controller.get)
  .patch(controller.patch)
  .delete(controller.delete)
```

## Demo
To run a working demo, run the following command:
```
npm start -s
```

You can find the demo inside `demo/index.js`.

## License
Copyright (c) 2020 Nuotron.
Released under the [MIT license](https://github.com/github/choosealicense.com/blob/gh-pages/LICENSE.md).
