# Debugging Ansible Module with VS Code

Debugging Ansible modules with VS code as the most interactive way,

#### Getting the module:

After getting the module installed from ansible-galaxy,or git clone the collection with all necessary dependencies

```
ansible-galaxy collection install cisco.ios
```

Look for the path where the module sits and establish VS code at the same location.

```
┌[machine@machine]-(~/.a/c/a/c/ios)
└> pwd
~/.ansible/collections/ansible_collections/cisco/ios
┌[machine@machine]-(~/.a/c/a/c/ios)
└> code.
```

#### Configuring VS code:

_(Use the same setting with a different port for debugging multiple modules)_
Once we are in vs code with the specific module we wish to debug, either use the debug option followed by the settings sign tagged ‘Open launch.json’ to generate the launch configuration file

![Alt text](./images/image_debugPanel.png?raw=true "Debugging option")

_Or_

Use the following commands to the root of the collection for generating the same

```
┌[machine@machine]-(~/.a/c/a/c/i)
└> mkdir .vscode
┌[machine@machine]-(~/.a/c/a/c/i/.vscode)
└> nano launch.json
```

Drop the below config in your launch.json

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Attach",
            "type": "python",
            "request": "attach",
            "port": 3000 ,
            "host": "127.0.0.1",
            "justMyCode": false
        },
    ]
}
```

Get debugpy and install it within the scope of your environment.

`pip install debugpy`

To start debugging go to the intended module and put the following lines on top of it,

```
import debugpy
debugpy.listen(3000)
debugpy.wait_for_client()
```

Well, the last step

Your inventory file should contain the specified variable

```
[all:vars]
ansible_network_import_modules=True
```

Now right after running your playbook using ansible-playbook

```
[machine@machine]-(~/W/debug_example)
└> ansible-playbook debug_playbook.yml
```

Expect the execution to be stuck at the task execution level waiting for you to manually start the module and add it to port 3000 (or any configured port).

Navigating to the module where debugpy import was used press F5 to start debugging

![Alt text](./images/image_debugging.png?raw=true "Debugging demo")

At this point, the debugger should hit your break points when in process.

For debugging ansible-navigator check a cool doc [here]: https://github.com/shatakshiiii/debuggingNavigator/blob/main/README.md

Refer to the VS code debugging guide for more information-
https://code.visualstudio.com/docs/editor/debugging

#### Works with:

- older Network Modules
- newer Network Resource Modules
- execution with Ansible Navigator

## Happy Debugging !!!
