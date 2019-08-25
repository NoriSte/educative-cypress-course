# The Realworld project

We leveraged both the React frontend and the Node.js backend of the [RealWorld](http://realworld.io) project. You can play with it with an [online hosted demo](https://react-redux.realworld.io). We cloned both the repositories and adapted them for the purpose of this course:

- we shared some constants between the source code and the tests code
- we delayed every backend operation to simulate E2E testing bottlenecks
- we added the option to wipe out the entire DB
- we made the execrices network-independent by storing all the assets locally

The RealWorld projects are stored into the `realworld` directory. To run them you need to have installed

Some notes:

- you need to have [Docker](https://docs.docker.com/install/) and [MongoDB](https://docs.mongodb.com/manual/installation/#tutorials) installed to run the projects and the tests locally (not for the book itself)
- the DB data could not be persisted. If you stop the Mongo process the frontend was logged in, you need to [manually delete](https://developers.google.com/web/tools/chrome-devtools/storage/localstorage) the stored JWT. Otherwise, the frontend will not work anymore. The Cypress tests already managed it by clearing every local state before every test.