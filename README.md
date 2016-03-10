# Willy
Willy is our thesis' fourth member. Besides transforming data he's quite useless, but man, handling data he does. To get Willy's help, you'll need to have [python](https://www.python.org/) (3) installed. Once that's done, you can ask him for help at the command line. The format looks something like this:

```
  willy command file [-h] [-o OUTPUT_FILE] [-d DELIMITER] [-p PERCENTAGE]
```


## Usage
### DropCrap
``dropCrap`` is Willy's first command. It takes the data as input (you have to provide the path) and he will throw everything away except for the "IKL", "TRACE_ID", "TIMESTAMP" and "TYPE_OMSCHRIJVING" variables.

Here's an Example:

```
  $ willy dropCrap full_data.csv -d ";"
  [INFO] I'm loading your file. Be patient, it's a lot of records and I don't know how to multithread.
  [INFO] Yo dude, I put all that good stuff you asked for in willyWroteThis.csv
```


### Filter
Willy can also make the dataset a little smaller for you. You can ask for this with the ``filter`` command. Willy will group all data belonging to one user together and then asks you which percentage of users he should keep. The users he keeps are selected at random.

Once again, here's an example:

```
  $ willy filter willyWroteThis.csv -p 0.1
  [INFO] Just lemme get all the peeps that are in here real quick.
  [INFO] Just lemme get all the peeps that are in here real quick.
  [INFO] Yo man, I selected 17920 dudes and dudettes for you, hope you're happy with them.
  [INFO] Let me see if I can find back all these peeps.
  [INFO] About damn time, let the printing begin!
```
