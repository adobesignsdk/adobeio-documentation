# Setting up the Adobe I/O Runtime CLI

Since Adobe I/O Runtime is built on OpenWhisk, you&rsquo;ll need to use the Adobe I/O and OpenWhisk CLIs to interact with the system. 

To install the Adobe I/O CLI:
1. `npm install aio-cli`
2. Magic.

Once you have the Adobe I/O CLI installed and configured, you can set up the OpenWhisk CLI:
1. Download the executable from the [OpenWhisk GitHub repository](https://github.com/apache/incubator-openwhisk-cli/releases "OpenWhisk CLI on GitHub"). Choose the version that matches your operating system and download the compressed archive. 
2. Extract the executable from the compressed archive and place it in a folder of your choice.
3. Add the folder into which you placed the executable to your `PATH` environment variable. This enables you to call the CLI from any command-line window.

Alternatively, you can choose to build OpenWhisk locally; this produces a copy of the CLI. See the [OpenWhisk documentation](https://github.com/apache/incubator-openwhisk/blob/master/docs/cli.md "Setting up the OpenWhisk CLI") for instructions.

The OpenWhisk CLI uses a `.wskprops` file to configure the commands to work with a specific namespace. If you're using the Adobe I/O CLI, the `.wskprops` file will be configured automatically for whatever integration you select. You can also use the OpenWhisk CLI by itself, but it requires manual configuration. You will have to create a `.wskprops` file containing your authorization key, your namespace, and the path to the Runtime API host. Here&rsquo;s an example: 

```
AUTH=<Your UUID>
APIHOST=runtime.adobe.io
NAMESPACE=<Your namespace>
```

You place this file in your user folder, where the CLI can access it to set those properties when it starts. 

An alternative method to configure your instance of the CLI is to do it through a CLI command. Open a command-line window and type the following command:

`wsk property set --apihost runtime.adobe.io --auth` &nbsp;_`<Your auth code from the Runtime team>`_ &nbsp;`--namespace` &nbsp;_`<Your namespace from the Runtime team>`_

Once you&rsquo;ve configured the CLI, you should test it. You&rsquo;re ready to [deploy your first function](deploy.md).