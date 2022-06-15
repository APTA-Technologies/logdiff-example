# logdiff example usage

## Prerequisites

### API key or database endpoint
Get a free account and API key from https://logdiff.apta.tech (or alternatively, set up an endpoint to store and retrieve models and set the endpoint-url parameter for the logdiff service).

### Logging format
The logdiff action looks for a CSV file with two columns, ignoring all other columns: `id` ad `symb`, containing a trace identifier and a logged event.

## How to use logdiff?

(1) Run your test cases and collect the logs
(2) Give your logs to the apta logdiff action
(3) Collect

### Running your tests to get logs

In our example repository, we run `bash test.sh`. It generates an output file named `1_training.txt.dat`. 


### Setting up logdiff action


In `.github/workflows/run.yml`, we pass the output file to the logdiff action.
 

```
jobs:
  run-tests-and-logdiff:
    runs-on: ubuntu-latest
 
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Run the tests
        run: |
          /bin/bash test.sh
  
 
      - name: Run logdiff
        id: logdiff
        uses: APTA-Technologies/logdiff@v1
        with:
          tracefile: 1_training.txt.dat
          inifile: edsm.ini
``` 

### Access results

Once the action ran, it will publish an artifact named `result` with a zipped file containing the alignment of the current run with the model from the previous run. It will also publish a model file of this run to compare against in the next run.
