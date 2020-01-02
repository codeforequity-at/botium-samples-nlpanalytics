# Botium NLP Analytics Sample

This repository is accompanying [this blog article](https://chatbotslife.com/tutorial-benchmark-your-chatbot-on-watson-dialogflow-wit-ai-and-more-92885b4fbd48) and showcases the NLP analytics capabilities of [Botium](https://www.botium.at).

## Prerequisites

* Node.js (> 8.13)
* Clone this repository (_git clone ..._)

## Prepare Botium Connectors

In the _config_ directory you will find several _*.botium.json_ files for the currently supported Botium connectors. Add the required information there. For most of the connectors this means setting up an account with the provider and add access keys or secrets to those files.

**You are free to only configure the Botium connectors the results are of interest to you.**

### IBM Watson
See https://github.com/codeforequity-at/botium-connector-watson

### Wit.ai
See https://github.com/codeforequity-at/botium-connector-witai

_Trained Wit.ai projects cannot be removed automatically, have to be done manually_

### Dialogflow
See https://github.com/codeforequity-at/botium-connector-dialogflow

_Create a separated Google Project for doing this analytics. The attached Dialogflow agent will be overwritten._

### NLP.js
See https://github.com/codeforequity-at/botium-connector-nlpjs

### Lex
See https://github.com/codeforequity-at/botium-connector-lex

## Prepare Data

The _convos/smalltalk_ directory already contains a dataset for a simple _Smalltalk_ chatbot in plain text files.

**See [Botium Wiki](https://botium.atlassian.net/wiki/spaces/BOTIUM/pages/491664/Botium+Scripting+-+BotiumScript) for details about other supported file formats like YAML, Excel, CSV, JSON**

* Filename has to end in _.utterances.txt*_, otherwise name does not matter
* First line of the file contains the intent name
* Each other line contains a user example for this intent

It makes sense to use your own data to make this evaluation. If you already have the data available in one of the supported platforms, you can use this tool to extract the data and save it for further evaluation - one of the following commands will extract it and save the intents and user examples in the _convos_ directory:

    > npm run nlpextract:watson
    > npm run nlpextract:lex
    > npm run nlpextract:witai
    > npm run nlpextract:dialogflow

**You are free to use any other tool to prepare the data, you can even write it manually (if you have the time)**

## Split utterances into a training and a test set

An easy way to separate training data from test data is to split an existing dataset into two separate datasets. In package.json, there is a sample task splitting the _Smalltalk_ into 80% training data and 20% test data:

    > npm run nlpsplit

## Run Test Set Validation on Training Set

When you separated your test data into a training and a test set, either manually or by using the _nlpsplit_ task, you can now train a model with the training set and validate with the test set. In package.json, there are tasks defined to run the validation for the included _Smalltalk_ dataset.

    > npm run validate:watson
    > npm run validate:lex
    > npm run validate:witai
    > npm run validate:dialogflow
    > npm run validate:nlpjs

Or you can run it all in parallel:

    > npm run validate

After a while, you will find the results for each platform in the _results_ directory. For each platform:

* validate-_platform_.txt to hold the validation summary
* validate-_platform_.err.txt for processing error output
* validate-_platform_.csv for listing the score details
* validate-_platform_-predictions.csv for listing the predicted intents for all utterances

## Run K-Fold Cross Validation

In package.json, there are tasks defined to run the K-Fold Cross Validation for the included _Smalltalk_ dataset.

    > npm run kfold:watson
    > npm run kfold:lex
    > npm run kfold:witai
    > npm run kfold:dialogflow
    > npm run kfold:nlpjs

Or you can run it all in parallel:

    > npm run kfold

After a while, you will find the results for each platform in the _results_ directory. For each platform:

* k-fold-_platform_.txt to hold the validation summary
* k-fold-_platform_.err.txt for processing error output
* k-fold-_platform_.csv for listing the score details
* k-fold-_platform_-predictions.csv for listing the predicted intents for all folds and utterances

**See [here](https://medium.com/analytics-vidhya/quality-metrics-for-nlu-chatbot-training-data-part-1-confusion-matrix-91ac71b90270) for an introduction how to interpret the results**
