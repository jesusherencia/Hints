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
