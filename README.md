Firebase Command Line Interface (CLI)
-------------------------------------

The following content describes how to install and use the Firebase Command Line Interface (CLI) to manage, test, and deploy your Firebase projects—using the command line. You can use the Firebase CLI to accomplish any of the following tasks:

* To deploy code and assets to your Firebase projects
* To run a local web server for your Firebase Hosting site
* To interact with data in your Firebase database
* To import/Export users into/from Firebase Auth

To get started with the Firebase CLI, you can read the full list of commands below or at [hosting-specific CLI documentation](https://firebase.google.com/docs/hosting/quickstart).

## Installation

Before you can install the CLI, you need to install [npm](https://npmjs.org/) first, which typically comes with [Node.js](http://nodejs.org/).

After npm is installed on your local machine, you can now get the Firebase CLI by following these steps:

1. Open terminal shell and run the following npm command:
    ```bash
    npm install -g firebase-tools
    ```
    **Note**: This will provide you with the globally accessible `firebase` command.
    
2. Verify that the Firebase CLI has been installed correctly on your local machine by running the following command:
    ```bash
    firebase --version
    ```
    Make sure that the version of the Firebase CLI is v4.1.0 or later.

3. After installing the CLI, you must authenticate. you can sign into Firebase using your Google account by running the following command:
    ```bash
    firebase login
    ```
    This command connects your local machine to Firebase and grants you access to your Firebase projects.

## Commands

**The command `firebase --help` lists the available commands and `firebase <command> --help` shows more details for an individual command.**

If a command is project-specific, you must either be inside a project directory with an
active project alias or specify the Firebase project id with the `-P <project_id>` flag.

Below is a brief list of the available commands and their function:

### Administrative Commands

Command | Description
------- | -----------
**login** | Authenticate to your Firebase account. Requires access to a web browser.
**logout** | Sign out of the Firebase CLI.
**login:ci** | Generate an authentication token for use in non-interactive environments.
**projects:list** | Print a list of all of your Firebase projects.
**setup:web** | Print out SDK setup information for the Firebase JS SDK.
**use** | Set active Firebase project, manage project aliases.
**open** | Quickly open a browser to relevant project resources.
**init** | Setup a new Firebase project in the current directory. This command will create a `firebase.json` configuration file in your current directory.
**help** | Display help information about the CLI or specific commands.

Append `--no-localhost` to login (i.e., `firebase login --no-localhost`) to copy and paste code instead of starting a local server for authentication. A use case might be if you SSH into an instance somewhere and you need to authenticate to Firebase on that machine.

### Deployment and Local Development

These commands let you deploy and interact with your Firebase services.

Command | Description
------- | -----------
**deploy** | Deploys your Firebase project. Relies on `firebase.json` configuration and your local project folder.
**serve** | Start a local server with your Firebase Hosting configuration and HTTPS-triggered Cloud Functions. Relies on `firebase.json`.

### Auth Commands

Command | Description
------- | -----------
**auth:import** | Batch importing accounts into Firebase from data file.
**auth:export** | Batch exporting accounts from Firebase into data file.

Detailed doc is [here](https://firebase.google.com/docs/cli/auth).

### Database Commands

Command | Description
------- | -----------
**database:get** | Fetch data from the current project's database and display it as JSON. Supports querying on indexed data.
**database:set** | Replace all data at a specified location in the current project's database. Takes input from file, STDIN, or command-line argument.
**database:push** | Push new data to a list at a specified location in the current project's database. Takes input from file, STDIN, or command-line argument.
**database:remove** | Delete all data at a specified location in the current project's database.
**database:update** | Perform a partial update at a specified location in the current project's database. Takes input from file, STDIN, or command-line argument.
**database:profile** | Profile database usage and generate a report.

### Cloud Firestore Commands

Command | Description
------- | -----------
**firestore:delete** | Delete documents or collections from the current project's database. Supports recursive deletion of subcollections.
**firestore:indexes** | List all deployed indexes from the current project.

### Cloud Functions Commands

Command | Description
------- | -----------
**functions:log** | Read logs from deployed Cloud Functions.
**functions:config:set** | Store runtime configuration values for the current project's Cloud Functions.
**functions:config:get** | Retrieve existing configuration values for the current project's Cloud Functions.
**functions:config:unset** | Remove values from the current project's runtime configuration.
**functions:config:clone** | Copy runtime configuration from one project environment to another.
**functions:delete** | Delete one or more Cloud Functions by name or group name.
**functions:shell** | Locally emulate functions and start Node.js shell where these local functions can be invoked with test data.

### Hosting Commands

Command | Description
------- | -----------
**hosting:disable** | Stop serving Firebase Hosting traffic for the active project. A "Site Not Found" message will be displayed at your project's Hosting URL after running this command.

## Using with CI Systems

The Firebase CLI requires a browser to complete authentication, but is fully
compatible with CI and other headless environments.

1. On a machine with a browser, install the Firebase CLI.
2. Run `firebase login:ci` to log in and print out a new [refresh token](https://developers.google.com/identity/protocols/OAuth2)
   (the current CLI session will not be affected).
3. Store the output token in a secure but accessible way in your CI system.

There are two ways to use this token when running Firebase commands:

1. Store the token as the environment variable `FIREBASE_TOKEN` and it will
   automatically be utilized.
2. Run all commands with the `--token <token>` flag in your CI system.

The order of precedence for token loading is flag, environment variable, active project.

On any machine with the Firebase CLI, running `firebase logout --token <token>`
will immediately revoke access for the specified token.

## Using as a Module

The Firebase CLI can also be used programmatically as a standard Node module. Each command is exposed as a function that takes an options object and returns a Promise. For example:

```js
var client = require('firebase-tools');
client.list().then(function(data) {
  console.log(data);
}).catch(function(err) {
  // handle error
});

client.deploy({
  project: 'myfirebase',
  token: process.env.FIREBASE_TOKEN,
  force: true,
  cwd: '/path/to/project/folder'
}).then(function() {
  console.log('Rules have been deployed!')
}).catch(function(err) {
  // handle error
});
```
## License

[MIT License](https://github.com/chriso23/firebase-cli/blob/master/LICENSE)


