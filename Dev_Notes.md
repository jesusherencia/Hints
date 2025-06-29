# Dev Notes

## For refactoring over migration, security fixes, etc by applying existing recipes, we can use Openrewrite (https://docs.openrewrite.org/)

## For inspection, modification, enhance of java applications behavior we can use Java Telemetry API and java agents:

Instrumentation in Java refers to the ability to modify, enhance, and inspect the behavior of Java applications at runtime. 
It provides a dynamic mechanism to transform the bytecode of classes as they are loaded into the Java Virtual Machine (JVM). 
By utilizing the Instrumentation API, developers can intercept, modify, and analyze the bytecode to add functionality or gain insights into application behavior.

## When we want to create environment variables in linux through a bash file, we must execute the file with the source command

Ex:
> source env_vars.sh

## When declaring abstract methods in python, better fill it with "raise NotImplementedError" instead of "pass", as instantiation of the abstract class is possible (to confirm)

## In python, in order to enforce type checking use "typeguard" library

Ex:
> from typeguard import typechecked
> @typechecked
>    def method_to_be_type_checked(param1: str, param2: float, param3: Type1):

## Useful Docker commands

- To build an image from a Dockerfile

> docker build -t <image_name> .

Where <image_name> should be replaced by a value.

- To run a container from an image

> docker run -d --name <container_name> -v <host_dir>:<container_dir> -p <host_port>:<container_port> <image_name>

-d : run in detached mode  
-v : add volume linking a host directory to a container one  
*To use current directory as a value, os-related specific values are used for <host_dir>:  
cmd: %cd%  
powershell: ${PWD}  
linux: $(pwd)*  
-p : link a host port to a container one  
Notes:  
-v & -p can be used more than once within the same command  

## In Python, in order to run a module(sub-module) from app root folder while using absolute imports in module files, use "-m' option

> python3 -m <module_path>

Examples 'module-path':
- webapp (webapp.py)
- web.app (web/app.py)

## In Python, to create package namespaces in order to be used as imports

Having a project folder structure:

```
project_folder
├── com/package1
│   ├── subcpackage1
│       ├── module1.py
│   ├── subcpackage2
│       ├── module2.py
├── com/package2
│   ├── main.py
```

And adding package folders to PYTHONPATH environment variable

> export PYTHONPATH=$PYTHONPATH:<project_folder>/com/package1:<project_folder>/com/package2

We can import subpackages from any other module as they are already present in path.

```python
import subpackage1.module1
from subpackage2 import module2
...
```

## In Angular, update input value using DOM and update model

If we update an input by using DOM (input.value = 'test'), the angular would not listen and model would not be updated.
In order to trigger the model we need to dispatch an event.

```javascript
updateInput(inputElement: HTMLInputElement) {
  inputElement.value = 'Updated!';
  // Dispatch input event to trigger Angular's ngModel update
  inputElement.dispatchEvent(new Event('input'));
}
```

