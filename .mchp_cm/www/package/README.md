# Content Manager UI

The root project is bootstrapped by [Create React App](https://create-react-app.dev/) which manages the production build and development workflow. The main website entrypoint is `/src/index.tsx`.

The data layer is [Redux](https://redux.js.org/) with most logic located under `/src/`. Developers are highly encouraged to install the [Redux DevTools Extension](https://github.com/reduxjs/redux-devtools) on their browser when needing to debug.

Coding style and linting rules inside the `/src` directory are enforced automatically.

The `/java/` folder contains a [Netbeans](https://netbeans.apache.org/) project using `JDK 1.8_202` to bundle the production version of the website, and contains the following APIs for use as a dependency in other java projects:
* [Actions.java](./java/src/main/java/com/microchip/mcc/contentmanager/Actions.java): The request/response data contract to be kept in-sync with the Typescript project's [actions.ts](./src/data/actions.ts).
* [Router.java](./java/src/main/java/com/microchip/mcc/contentmanager/CMTRouter.java): Typesafe helper class to route all request/responses. See `RouterTest.java`.

## Deployment

The production website is deployed as a node module to [Microchip's NPM Registry](https://artifacts.microchip.com/artifactory/api/npm/npm). The production website is bundled with some java APIs in a jar file, and deployed with the same version to [Microchip's Maven Repository](https://artifacts.microchip.com/artifactory/mcu8-maven).
```
<dependency>
    <groupId>com.microchip.mcc</groupId>
    <artifactId>content-manager-ui</artifactId>
    <version>x.y.z</version>
</dependency>
```
----

## Getting Started : Development

* (Required) Install [Node JS](https://nodejs.org/en/download/)
* (Required) Install [Yarn](https://classic.yarnpkg.com/en/docs/install#windows-stable)
   * Microchip developers should configure their installation to use the internal Artifactory NPM registry.\
   `yarn config set registry https://artifacts.microchip.com/artifactory/api/npm/npm/`
* (Required) Install [Maven](https://maven.apache.org/download.cgi)

* (Recommended) Install [VSCode](https://code.visualstudio.com/)
   * (Recommended) Install the [ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) to automatically fix coding style and common errors on save.
* (Recommended) Install the browser [Redux Devtools Extension](https://github.com/reduxjs/redux-devtools) to investigate the graph's state.
* (Recommended) Install the [Debugger for Chrome Extension](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome) to be able to set breakpoints directly in VSCode while debugging

----
### On checkout: `yarn`
Always run this first in the project directory to install the project's dependencies.
### Build frontend and backend server : `yarn build-all`
### Start Development: `yarn start-all`
Starts both the backend server and the frontend app.
Note: Hot-reloading is only available on the frontend.
----

## Available Scripts

### `yarn start`

Runs Content Manager UI in **development mode**. Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits. You will also see any lint errors in the console.

### `yarn lint`

Runs the linter and fixes many issues automatically.

**Recommended**: Use VSCode and install the [ESLint Plugin](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) to automatically fix lint issues on save!

### `yarn test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `yarn build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### Build Java backend server: `yarn build-server`
Builds the backend server with dependencies so that it can run independently of MCC

### `yarn start-server`

Launches the development backend server.\ 
It Can be used in combination with `yarn start` to show both servers in separate windows.

### `yarn analyze`

Generates and displays a tree map of the production bundle structure.\
See [code splitting](https://reactjs.org/docs/code-splitting.html).

----

# Command Line Interface (CLI)

To start the CLI, run the following command: 
```sh
> java -jar path/to/jar/content-manager-ui-0.0.0-develop.jar [--device, -d] [--update, -u] [--type, -t]
```
### Options

Note: All of these arguments are optional

### `--device`

Used as a one shot command to acquire all content for a given device regEx
Starts the CLI and performs the following steps
1. Runs the `launch-device` command with the provided `--device` as the regEx
2. Exit CLI

```sh
> java -jar java -jar path/to/jar/content-manager-ui-0.0.0-develop.jar --device=PIC18F47Q43
```

### `--update`

Downloads the latest available version of the Content Manager to the `<mcc-root>/content-manager/<version>/` folder

```sh
> java -jar java -jar path/to/jar/content-manager-ui-0.0.0-develop.jar --update=true
```

### `--type`

Restricts the acquired content to the specified type.
Options are: `MELODY`, `CLASSIC`, or `HARMONY`
Note: If no type is specified, CMT will download available content for all content types

```sh
> java -jar java -jar path/to/jar/content-manager-ui-0.0.0-develop.jar --type=MELODY
```

## Commands

## `launch-device <regEx>`

> Downloads all content that is supported by devices matching `<regEx>`\
Saves a map of name/path in a file called content.json at the cwd root

#### Usage 
```sh 
> launch-device PIC18F47Q43|PIC16F18855
```

## `set-registries <registryList>`

> Sets the npm registries used by the content manager to `<registryList>`\
This list should be provided as a semicolon `;` deliminated string

#### Usage
```sh
> set-registries http://registry.npmjs.org/;https://artifacts.microchip.com/artifactory/api/npm/npm/
```
Also works with a single entry:
```sh
> set-registries https://artifacts.microchip.com/artifactory/api/npm/npm/
```

## `update`

> Causes the Content manager to download the latest version of itself.\
Once complete, the CLI quits, allowing the new version to be invoked.

Note: This command can also be executed via argument when starting the CLI using either `-u` or `--update`

####Example
```sh
java -jar path/to/jar/content-manager-ui-0.0.0-develop.jar --update=true
```

## `launch-file <filePath>`

> Downloads all content specified by the json file at `<filePath>`\
The supplied path is relative to the terminal cwd.\
Saves a map of name/path in a file called content.json at the cwd root
#### Usage
```sh
launch-file ./input.json
```

## `download-devices <regEx>`

> Downloads all content supported by the devices that match `<regEx>`
#### Usage
```sh
> download-devices ^PIC16.*
```

## `generate-file <deviceName>`

> Generates a json file based on the content supported by `<deviceName>`\
This json file can serve as an example for other input.json files to be used by the `launch-file` command.

#### Usage
```sh
> generate-file PIC18F47Q43` generates a file in the cwd called `<deviceName>.json
```

## `quit`
> Exits the CLI.

#### Usage
```sh
> quit
```
----

## Learn More

* Project Configuration and Build: [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).
* Browser and DOM Library: [React documentation](https://reactjs.org/). 
* Client State Management and Async Server Handshaking: [Redux documentation](https://redux.js.org/).
* Source Control Quality Enforcement: [ESLint](https://eslint.org/) + [Typescript ESLint](https://github.com/typescript-eslint/typescript-eslint#readme), [Prettier](https://prettier.io/), [Husky](https://github.com/typicode/husky#readme), [Lint-staged](https://github.com/okonet/lint-staged)

To learn Typescript, see these Getting Started guides:
* [TS for Javascript Developers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
* [TS for Java/C# Developers](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes-oop.html)
* [General Documentation](https://www.typescriptlang.org/docs)

#
