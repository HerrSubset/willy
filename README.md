# Willy
Willy is our thesis' fourth member. Besides transforming data he's quite useless, but man, handling data he does. To get Willy's help, you'll need to have [python](https://www.python.org/) (3) installed. Once that's done, you can ask him for help at the command line. The format looks something like this:

```
  willy command file [-h] [-o OUTPUT_FILE] [-d DELIMITER] [-p PERCENTAGE] [-s SORT]
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


### Sequencify
This command transforms data into itemsets or sequences in a format so that [SPMF](http://www.philippe-fournier-viger.com/spmf/) can work with it. The input data will need the "TRACE_ID", "TYPE_OMSCHRIJVING" and "TIMESTAMP" attributes. The output format has to be provided with the ``-f`` flag. Possible values at this point are "itemset" and "sequence".

As usual, an example:
```
  $ willy sequencify filteredByWilly.csv -f sequence
  [INFO] Putting all them sequences in a dictionary
```


## Typical Use Case
Let's say we want to use our dataset, stored in the file "full_data.csv" in my case, to generate frequent itemsets with [SPMF](http://www.philippe-fournier-viger.com/spmf/). The first, optional step, would be to drop some variables we don't need. The following steps won't have to load as much data this way.

```
  $ willy dropCrap full_data.csv -d ";" -o reducedDataSet.csv
  [INFO] I'm loading your file. Be patient, it's a lot of records and I don't know how to multithread.
  [INFO] Yo dude, I put all that good stuff you asked for in reducedDataSet.csv
```

Note that we have to tell Willy to use ';' as a delimiter. This is what the ``-d ";"`` part of the command does. We also provided an output file by typing ``-o reducedDataSet.csv``, so that's where our new data will be stored. In case you don't tell Willy what file to write to, he creates a new one by default so your original data doesn't get lost.

At this point we still have about 19 million records in our dataset, so that might be a bit too much to quickly test algorithms on. The ``filter`` command allows us to reduce that number by selecting only a certain % of users that are retained.

```
  $ willy filter reducedDataSet.csv -p 0.02
  [INFO] Just lemme get all the peeps that are in here real quick.
  [INFO] Yo man, I selected 3866 dudes and dudettes for you, hope you're happy with them.
  [INFO] Let me see if I can find back all these peeps.
  [INFO] About damn time, let the printing begin!
```

The ``-p 0.02`` flag means that we want to keep 2% of the users' data in the file. Note that we didn't provide an output file, so willy created one for us. It's called ``filteredByWilly.csv``.

The last step is to actually generate itemsets out of our data. Otherwise SPMF can't do anything with it.

```
  $ willy sequencify filteredByWilly.csv -f itemset
  [INFO] Putting all them sequences in a dictionary
```

The itemsets can be found in the file ``sequences.txt``. Note that Willy can only generate frequent itemsets at this point. That is, lines where every item is separated by a space.
