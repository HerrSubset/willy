# Willy
Willy is our thesis' fourth member. Besides transforming data he's quite useless, but man, handling data he does. To get Willy's help, you'll need to have [python](https://www.python.org/) (3) installed. Once that's done, you can ask him for help at the command line. The format looks something like this:

```
  willy command file [-o OUTPUT_FILE] [-h]
```


## Usage
### DropCrap
``dropCrap`` is Willy's first command. It takes the data as input (you have to provide the path) and he will throw everything away except for the "IKL", "TRACE_ID", and "TYPE_OMSCHRIJVING" variables.

Here's an Example:

```
  $ willy dropCrap full_data.csv
  [INFO] I'm loading that file. Be patient, it's 19 million records and I don't know how to multithread.
  [INFO] Whoa, I can finally start writing to willyWroteThis.csv. This might take even longer though.
```
