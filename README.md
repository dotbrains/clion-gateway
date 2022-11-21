# CLion Gateway

The setup makes use of [JetBrains Gateway](https://www.jetbrains.com/remote-development/gateway/) which *is a compact desktop app that allows you to work remotely with a JetBrains IDE without even downloading one*.

The idea is that you are essentially installing the IDE on the container itself and connecting to it from your host via SSH - it is a relatively new technique that is still in *beta* at the moment. This allows you to use gdb debugger and step through the lines of code using the IDE with minimal configuration. Essentially its like you are using [Gitpod](https://www.gitpod.io/). You don't have to configure all of this through the use of the **remote-host** method through **toolchains**, **cmake profiles** for debugging, and **deployments** on CLion. Just connect and code. Docker integration on JetBrains IDEs provide you with the ability to build images, create containers, view configuration, etc., however you have to go through complex steps to get debugging to work correctly. The idea is what if you could run the IDE directly off of the container or VM itself. Then it would require minimal configuration.

## Requirements

- [JetBrains Gateway](https://www.jetbrains.com/remote-development/gateway/)
- [docker](https://www.docker.com/)

## Installation

To build the image and start the container in the background run the following command:

```bash
docker compose build && docker compose up -d
```

Once the container is up and accessible, download [JetBrains Gateway](https://www.jetbrains.com/remote-development/gateway/).

Once JetBrains Gateway has been installed, open it.

We are going to set up an SSH connection to CLion from within our Docker Container.

Click on **SSH** and **New Connection**.

Enter the following for the various fields:

- Username: `root`
- Password: `root`
- Port: 7776

Click on **Check Connection and Continue**.

Select **CLion** in the dropdown for **IDE Version**.

Select your project directory from within the container (e.g. `/code`).

Finally, select **Start IDE and Connect**.

This will start the process of downloading the IDE to the VM and starting the CLion Backend daemon so we can connect to the IDE.

If it finishes successfully, you will have a CLion IDE window come up with the selected project directory.