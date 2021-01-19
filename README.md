# How to set up ESLint and Prettier in a React Native app.

This project was bootstrapped with [React Native CLI](https://reactnative.dev/docs/environment-setup#creating-a-new-application).

## Available Scripts

To see how you can run this app for iOS or Android please see the [Running your React Native application](https://reactnative.dev/docs/environment-setup#running-your-react-native-application) section from the React Native Documentation. For this sample, we'll ignore those details.

## Install and setup ESLint

1. `npm uninstall @react-native-community/eslint-config` - React Native 0.63.4 (at the moment of this writing) comes with a predefined eslint config, but we'll remove that package because we'll create our own config.

2. `npm uninstall eslint` - React Native 0.63.4 (at the moment of this writing) doesn't come with the latest [eslint](https://www.npmjs.com/package/eslint) version, so we'll simply uninstall it and then install it again to pull the latest version.

3. `npm install eslint --save-dev` - install the latest `eslint` package version.

4. `npx eslint --init` - set up a configuration file for `eslint`. This command will ask you a few questions via CLI. Here's a list of them, and the answers we'll need to choose (`✔` and `❯` symbols indicate the selected answers):

```bash
# question 1:
? How would you like to use ESLint? …
  To check syntax only
  To check syntax and find problems
❯ To check syntax, find problems, and enforce code style

# question 2:
? Which framework does your project use? …
❯ React
  Vue.js
  None of these

# question 3 (select "No", because we won't add TypeScript support for this project):
? Does your project use TypeScript? › No / Yes

# question 4:
? Where does your code run? …
  Browser
✔ Node

# question 5:
? How would you like to define a style for your project? …
❯ Use a popular style guide
  Answer questions about your style
  Inspect your JavaScript file(s)

# question 6 (we'll rely on Airbnb's JavaScript style guide here):
? Which style guide do you want to follow? …
❯ Airbnb: https://github.com/airbnb/javascript
  Standard: https://github.com/standard/standard
  Google: https://github.com/google/eslint-config-google

# question 7:
? What format do you want your config file to be in? …
  JavaScript
  YAML
❯ JSON

# the final prompt here is where eslint will ask you if you want to install all the necessary dependencies. Select "Yes" and hit enter:
Checking peerDependencies of eslint-config-airbnb@latest
The config that you have selected requires the following dependencies:

eslint-plugin-react@^7.21.5 eslint-config-airbnb@latest eslint@^5.16.0 || ^6.8.0 || ^7.2.0 eslint-plugin-import@^2.22.1 eslint-plugin-jsx-a11y@^6.4.1 eslint-plugin-react-hooks@^4 || ^3 || ^2.3.0 || ^1.7.0

? Would you like to install them now with npm? › No / Yes
```

As a result, you'll end up having a `.eslintrc.json` file in the root of your project, which looks like so (we'll modify it a little bit later on):

```json
{
    "env": {
        "es2021": true,
        "node": true,
    },
    "extends": [
        "plugin:react/recommended",
        "airbnb"
    ],
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": 12,
        "sourceType": "module"
    },
    "plugins": [
        "react"
    ],
    "rules": {
    }
}
```

**NOTE:** React Native comes (at the moment of this writing) with a `.eslintrc.js` config, and if that's still the case for you, then simply remove it, since we'll use the JSON format of that config.

6. [React Hooks](https://reactjs.org/docs/hooks-intro.html) are pretty popular at this point and most likely every new React Native project will make use of them, so let's add the recommended ESLint rules for hooks. For that, update the `extends` section of your `.eslintrc.json` file like so:

```jsonc
{
    //...
    "extends": [
        "plugin:react/recommended",
        "airbnb",
        "airbnb/hooks"
    ],
    //...
}
```

7. `npm install --save-dev eslint-plugin-react-native` - add support for [React Native specific ESLint rules](https://www.npmjs.com/package/eslint-plugin-react-native#list-of-supported-rules). After installing this package, update the `plugins` section of your `.eslintrc.json` file like so:

```jsonc
{
    //...
    "plugins": [
        "react",
        "react-native"
    ],
    //...
}
```

8. Create a `.eslintignore` file in the root on your project to tell ESLint to ignore specific files and directories, and then add the following contents:

```bash
node_modules/
```

In our case, we simply want to ignore the `node_modules` folder from being linted.

9. Last step here, is to configure your code editor to lint the code for you. If you're a [VS Code](https://code.visualstudio.com/) user, simply install the [ESLint VS Code extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint). The only ESLint configuration you need to add to your VS Code settings is the following:

```json
{
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
    }
}
```

**NOTE:** You may want to add these VS Code settings as Workspace settings instead of User settings, meaning that they will only apply to this specific project and not to any project you'll open in VS Code. Read more on the [VS Code Documentation](https://code.visualstudio.com/docs/getstarted/settings) page.

## Install and setup Prettier

1. `npm install --save-dev --save-exact prettier` - add the [prettier](https://www.npmjs.com/package/prettier) package as a dev dependency to your project. We'll also install and use the [Prettier VS Code extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) later on, which already comes with Prettier package bundled in, but in general it's a good practice to add the npm package to your project directly as well, just in case some people won't use VS Code as their code editor of choice.

2. `npm install --save-dev eslint-config-prettier` - we'll use Prettier for code formatting concerns, and ESLint for code-quality concerns, so we need to turns off all ESLint rules that are unnecessary or might conflict with Prettier. Once the package has been installed, we need to update the `extends` section of your `.eslintrc.json` file like so:

```jsonc
{
    //...
    "extends": [
        "plugin:react/recommended",
        "airbnb",
        "airbnb/hooks",
        "prettier",
        "prettier/react"
    ],
    //...
}
```

3. Create a `.prettierrc.json` configuration file in the root of your project with the following options:

```json
{
    "arrowParens": "always",
    "bracketSpacing": true,
    "jsxBracketSameLine": false,
    "jsxSingleQuote": false,
    "quoteProps": "as-needed",
    "singleQuote": true,
    "semi": true,
    "printWidth": 100,
    "useTabs": false,
    "tabWidth": 2,
    "trailingComma": "es5"
}
```

For more Prettier configuration options and an explanation of what each option does, see [Prettier Documentation](https://prettier.io/docs/en/options.html).

**NOTE:** React Native comes (at the moment of this writing) with a `.prettierrc` config already, and if that's still the case for you, then you can still use that config file instead of creating a new `.prettierrc.json` one. They both use the JSON format for specifying configuration options.

4. Last step here, is to configure your code editor to format your code using Prettier. If you're a [VS Code](https://code.visualstudio.com/) user, simply install the [Prettier VS Code extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode). After the extension has been installed, add the following settings to your VS Code JSON config:

```json
{
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
}
```

## Final step

If you're planning to have your React Native components written in `.js` files (which how this sample React Native project does), then you'll need to update the `react/jsx-filename-extension` ESLint rule for React to allow JSX in `.js` files. The final version of your `.eslintrc.json` file should look like [this](https://github.com/vasilestefirta/react-native-eslint-prettier-example/blob/master/.eslintrc.json).

You can enable/disable/modify any ESLint rules within the `rules` section of your `.eslintrc.json` file.
