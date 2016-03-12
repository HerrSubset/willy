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


### Numbrify
SPMF needs numerical input for most of its algorithms. Numbrify can map all values of "TYPE_OMSCHRIJVING" to numbers. The way you do this is as follows:

```
  $ willy numbrify willyWroteThis.csv
  [INFO] Seems you want some numbrification done. Fine, I'll park it in numbrified.csv
  [INFO] In case you want to check things, the mappings I made are in mapping_0.txt
```

If you go check out the output file, you can see that all action values are now numbers. If you want to know what action a number refers to, check out the "mapping_X.txt" file that willy referred to.

The output of this command can be used as input for the ``sequencify`` command, so there's no problem there.



## Typical Use Case
Let's say we want to analyze our data using the BIDE+ algorithm in SPMF. First of all we will clean up our data using the ``dropCrap`` command so that the following steps go a bit faster.

```
  $ willy dropCrap full_data.csv -d ";" -o full_cleaned.csv
  [INFO] I'm loading your file. Be patient, it's a lot of records and I don't know how to multithread.
  [INFO] Yo dude, I put all that good stuff you asked for in full_cleaned.csv
```

Note that for the full data it's necessary to specify ``-d ";"`` because the semicolon is the delimiter in the original datafile.

Now that our cleaned data is in "full_clean.csv", we can filter it down a bit. Let's say we want the algorithm to run on 20% of our data.

```
  $ willy filter full_cleaned.csv -p 0.2 -o twenty_percent_clean.csv
  [INFO] Just lemme get all the peeps that are in here real quick.
  [INFO] Yo man, I selected 32853 dudes and dudettes for you, hope you're happy with them.
  [INFO] Let me see if I can find back all these peeps.
  [INFO] About damn time, let the printing begin!
```

We now have 20% of the users selected in "twenty_percent_clean.csv", but the file is still not in the right format for SPMF to understand it. Since we also want to run an algorithm that needs numerical input values, we first have to ``numbrify`` the data. Here's how that's done:

```
  $ willy numbrify twenty_percent_clean.csv -o twenty_percent_numbers.csv
  [INFO] Seems you want some numbrification done. Fine, I'll park it in twenty_percent_numbers.csv
  [INFO] In case you want to check things, the mappings I made are in mapping_0.txt
```

Remember to check in what file the mappings were stored by Willy. We will need them later on.

Our data is now all numeric, so the last transformation step is to make sequences out of the data:

```
  $ willy sequencify twenty_percent_numbers.csv -o numeric_sequences.txt -f sequence
  [INFO] Putting all them sequences in a dictionary
```

Don't forget to tell Willy that you want sequences by typing ``-f sequence``. Otherwise he will complain.

The data that is in "numeric_sequences.txt" can now be used by spmf. As an example we'll run the BIDE+ algorithm with 50% support.

```
  $ spmf run BIDE+ numeric_sequences.txt 50_bide_output.txt 50%
  ============  BIDE+ - SPMF 0.99b - 2016 - STATISTICS =====
   Total time ~ 677 ms
   Frequent sequences count : 8
   Max memory (mb) : 176.756553649902348
  ==========================================================


```

You now have the output of SPMF, but there's only numbers in there. To make reading it a little more easy, Willy can translate it back to text using the mapping file it created earlier. Here's how you do that:

```
  willy translate 50_bide_output.txt -m mapping_0.txt -o 50_bide_translated_output.txt
```

The output is now in "50_bide_translated_output.txt". Have fun interpreting the results!
