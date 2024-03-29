<h1>
  <a href="https://marketplace.visualstudio.com/items?itemName=Thinker.create-app"><sub><img src="./images/ca-logo.png" height="40"></sub> Create App</a>
</h1>

Easily Create any UI App with Official Starter Templates or Boilerplate using CLI.

## Features

- Easy to create a boilerplate app using the interactive Create App view.
- Apps can also be created using the vscode quick picks.
- We can also add our custom apps and commands to generate a interactive view and quick pick.

## Supported Apps:

<span><sub><a href="https://angular.io/"><img src="./images/angular.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Angular</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://www.djangoproject.com/"><img src="./images/django.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Django</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://expressjs.com/"><img src="./images/expressJs.png" alt="" width="20"></a></sub>&nbsp;&nbsp;ExpressJs</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://www.gatsbyjs.com/"><img src="./images/gatsby.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Gatsby</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://ionicframework.com/"><img src="./images/ionic.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Ionic</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br/>
<span><sub><a href="https://nestjs.com/"><img src="./images/nestJs.png" alt="" width="20"></a></sub>&nbsp;&nbsp;NestJs</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://nextjs.org/"><img src="./images/nextJs.png" alt="" width="20"></a></sub>&nbsp;&nbsp;NextJs</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://preactjs.com/"><img src="./images/preact.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Preact</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://reactjs.org/"><img src="./images/react.png" alt="" width="20"></a></sub>&nbsp;&nbsp;React</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://reactnative.dev/"><img src="./images/react.png" alt="" width="20"></a></sub>&nbsp;&nbsp;React Native</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://svelte.dev/"><img src="./images/svelte.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Svelte</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br/>
<span><sub><a href="https://vitejs.dev/"><img src="./images/vite.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Vite</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://code.visualstudio.com/api"><img src="./images/vscode.png" alt="" width="20"></a></sub>&nbsp;&nbsp;VS Code Extension</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<span><sub><a href="https://vuejs.org/"><img src="./images/vue.png" alt="" width="20"></a></sub>&nbsp;&nbsp;Vue</span>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

## Interactive

- Give `Ctrl/Cmd+Shift+P` to open the command pallet and type `Create App: Interactive` to open the Create App view.
- This opens an interactive ui that prompts you tp pick the app name and provide configurations to create the app.

![Screen Capture in Action](./images/preview_interactive.gif)

## Quick

- Give `Ctrl/Cmd+Shift+P` to open the command pallet and type `Create App: Quick` to open the Create App quick prompt.
- This provides you the quick command pallet and prompt you the pick the app and its required minimal configurations to create the app

![Screen Capture in Action](./images/preview_quick.gif)

## Custom App JSON interface

```ts
interface Tags {
  label: string;
  description: string;
  command?: string; //  Provide a command to execute in terminal on click
  href?: string; // Provide the link to webpage to redirect on click
}

interface FieldProps {
  type: "textbox" | "checkbox" | "radio" | "browse" | "dropdown";
  label: string;
  prefix?: string; // set prefix of the field value. Ex: "prefix":"--template=\""
  suffix?: string; // set suffix of the field value. Ex: "prefix":"\""
  value?: string | boolean; // The prefix and suffix will be added to the value. Ex: --template="value"
  placeholder?: string;
  description?: string;
  buttonText?: string; // provide button text if the field type is "browse"
  required?: boolean; // By default all required fields will be prompted on command Create App: Quick
  pattern?: string; // Set pattern to validate the value
  prompt?: boolean; // If True this field will be prompted on command Create App: Quick
  canSelectFile?: boolean; // Set to true or false when field type is "browse"
  canSelectFolder?: boolean; // Set to true or false when field type is "browse"
  errors?: { required?: string; pattern?: string }; // provide the error message
  options?: Array<{
    // provide options if the field type is "radio" or "dropdown"
    label: string;
    value: any;
  }>;
}

interface AppProps {
  appName: string; // Provide a unique app name. This overrides the app configs if already exist with a same name.
  commandTemplate: string; // Provide a command template here. Ex: "commandTemplate": "ng new ${fields.appId} --defaults" or "ng new ${fields['*']} --defaults"
  fields?: Record<string, FieldProps>; // Provide the app configuration to generate a app form fields. Ex: "fields": { "appId": { "type": "textbox", "required": true, value: "hello-world" } }
  description?: string; // This description will be shown below the About section in the right side of the form.
  order?: number; // Provide the App order to display in the apps list
  hide?: boolean; // If true this app will not be shown in both interactive and quick commands
  logoPath?: string; // Provide the app logo path. If not provide it will show the create app logo
  prerequisites?: Tags[]; // Provide the list of prerequisites commands and site links
  additionalCommands?: Tags[]; // Provide the additional commands to execute in terminal
  resources?: Tags[]; // List of links to refer the app
  tags?: string[]; // Provide the list of stings that helps to find the app
}
```

### `commandTemplate`

- `commandTemplate` helps to generate the cli command.
- It takes the variable `fields` and populates its value based on the field name.
- Lets see the commandTemplate examples for the given fields

```json
{
  "fields": {
    "appId": {
      "label": "App Id",
      "type": "textbox",
      "placeholder": "Can have lowercase alphabets and hyphen (-). No spaces or special chars are allowed.",
      "value": "hello-world",
      "required": true,
      "pattern": "^[a-z]+(-[a-z]+)*$",
      "errors": {
        "required": "App Id is Required.",
        "pattern": "Invalid Id. No spaces or special chars are allowed."
      }
    }
    "routing": {
      "label": "Include Routing ?",
      "type": "radio",
      "value": "--routing",
      "options": [
        {
          "label": "Yes",
          "value": "--routing"
        },
        {
          "label": "No",
          "value": ""
        }
      ]
    }
  }
}
```

- Example 1:
  - If commandTemplate is `ng new ${fields.appId} --defaults`,
  - Then the output of the command for the above commandTemplate will be `ng new hello-world --defaults`.
  - As we have seen the `routing` value is not populated in the command as we have not used in it.
- Example 2:
  - To see the routing value in the command we need to provide the commandTemplate as `ng new ${fields.appId} ${fields.routing} --defaults`.
  - Now the output of the command for the above commandTemplate will be `ng new hello-world --routing --defaults`.
- Example 3:
  - Instead of explicitly setting each field value in commandTemplate we can use `${fields['*']}` to populate all the field values.
  - Set commandTemplate as `ng new ${fields['*']} --defaults`,
  - Now the output of the command for the above commandTemplate will be `ng new hello-world --routing --defaults`.
- Example 4:
  - We can also use conditional logic to generate the command from commandTemplate
  - Set commandTemplate as `ng new ${fields.appId} ${!fields.routing ? '--default-route' : fields.routing} --defaults`,
  - from the above commandTemplate, If the value of routing is empty the we set a default command as `--default-route`
