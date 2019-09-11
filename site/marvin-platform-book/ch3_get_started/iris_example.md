---
layout: page
title: Starter Guilde
description: Project Community Page
group: nav-right
---
<!--
{% comment %}
Licensed to the Apache Software Foundation (ASF) under one or more
contributor license agreements.  See the NOTICE file distributed with
this work for additional information regarding copyright ownership.
The ASF licenses this file to you under the Apache License, Version 2.0
(the "License"); you may not use this file except in compliance with
the License.  You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
{% endcomment %}
-->

{% include JB/setup %}

# Iris Species Classification

## Overview

Tutorial for create the Iris Classification example on Marvin without using the ready-made engines.


## Requirements

 - Python 3.7.3
 - Pandas 0.25.0
 - Scikit-learn 0.21.3

## Development

### Getting started

First, create a new engine for this project. If you don't know how, access *[here](/marvin-platform-book/ch3_get_started/create_engine)*.

The engine was named "iris-classification". 

![](/assets/get-started/iris-create-engine.png)

Now, to be able to work on the project, use the following command

```
workon iris-classification-engine-env
```

You are now ready to code.

Note: If the workon command not work, type `source ~/.bash_profile` and try again. 

### Running tests

This project uses *[py.test](http://pytest.org/)* as test runner and *[Tox](https://tox.readthedocs.io)* to manage virtualenvs.

To run all tests use the following command

```
marvin test
```

### Writting documentation

The project documentation is written using *[Jupyter](http://jupyter.readthedocs.io/)* notebooks. 
You can start the notebook server from the command line by running the following command

```
marvin notebook
```

The notebook is accessed by the browser using the address `localhost:8888`.

**You need to organize the code into cells so that each corresponds an one action of Design Pattern *[DASFE](/marvin-platform-book/ch1_main_components/dasfe)* of Marvin.**

**Note that at the end of each cell there is a reserved variable named "marvin_", it will be responsible for creating the artifacts. Also note that you need to import the libraries that will be used in each cell.**

First, you need to load the dataset. This cell it's the *Acquisitor and Cleaner*. 

```python
#Data Acquisitor
import marvin_iris_classification_engine
from marvin_python_toolbox.common.data import MarvinData
import pandas as pd

file_path = MarvinData.download_file(url="https://s3.amazonaws.com/marvin-engines-data/Iris.csv")

iris = pd.read_csv(file_path)
iris.drop('Id',axis=1,inplace=True)

marvin_initial_dataset = iris
```

In the next cell occurs the dataset split, preparing for training. This cell it's the *Training Preparator*.

```python
#Training Preparator
from sklearn.model_selection import train_test_split

train, test = train_test_split(marvin_initial_dataset, test_size = 0.3)

train_X = train[['SepalLengthCm','SepalWidthCm','PetalLengthCm','PetalWidthCm']]
train_y = train.Species

test_X = test[['SepalLengthCm','SepalWidthCm','PetalLengthCm','PetalWidthCm']]
test_y = test.Species

marvin_dataset = {'train_X': train_X, 'train_y': train_y, 'test_X': test_X, 'test_y': test_y}
```

Next is the model training. In this tutorial was used the Support Vector Machine (SVM), but you can use the algorithm of your choice. This phase is the *Trainer*.

```python
#Model Traning
from sklearn import svm

clf = svm.SVC()
model = clf.fit(marvin_dataset['train_X'], marvin_dataset['train_y'])

marvin_model = model
```

The evaluation will now be performed by testing the model prediction. This is the *Metrics Evaluator*.

```python
#Model Evaluation
from sklearn.metrics import accuracy_score

predicted = marvin_model.predict(marvin_dataset['test_X'])
metric = accuracy_score(marvin_dataset['test_y'], predicted)

marvin_metrics = metric
```

The following message does not enter the DASFE Architecture, it serves as Jupyter's own tests. In this way, it will be in an isolated cell and will not receive any markup.

```python
input_message = ["12", "34", "10", "23"]
```

This is where the transformation and reading of the message that will be passed to the predictor. Since the message is already prepared, *input_message* is not modified, receiving itself. This is the *Prediction Preparator*.

```python
#Prediction Preparator
input_message = input_message
```

The following cell performs the prediction, being the end result. This stage is the *Predictor*.

```python
# Predictor
final_prediction = marvin_model.predict([input_message])[0] 
```

Like on the test message, this cell doesn't enter the DASFE Architecture. This cell is only for checking the result within Jupyter itself.

```python
print(final_prediction)
```

### Mark cells on DASFE Architecture

To make the marks on cells, use a window at the top of the Jupyter code as the image below.

![](/assets/get-started/dasfe-marvin.png)

Once the markup is done, the code should look like this

![](/assets/get-started/marked.png)

If everything is correct, save the changes and quit Jupyter.

### Running the Dryrun

Marvin dryrun is a way to test your code against DASFE standards.

**By default, a *String* message is sent to dryrun, but because the Iris Classifier message is a list of numbers, you must change it to be compatible. As Iris sends four characteristics, you must change the message to a four-number list.**

**That way, access the *engine.messages* file inside the folder `../marvin/iris-classification-engine/`**

As explained, will find the file with the following *String* message 

```
[{
	"msg1": "Hello from marvin engine!"
}]
```

So it's necessary delete everything and put the following message

```
[[1,2,3,4]]
```

Now it's possible perform the correct dryrun of the code. So at the terminal, type

```
marvin engine-dryrun
```

### Http Server

With dryrun without errors, it's possible generate the project API. Thus, the following command is used

```
marvin engine-httpserver
```

The server is accessed by the browser using the address `localhost:8000/docs`.

To test the API, go to *Predictor*, click in *Post*, then in *Try it out*, enter the message and click on *Execute* as in the image below.

![](/assets/get-started/predictor.png)

Test message for this example

```
{"message": ["4.8", "3.4", "1.9", "0.2"]}
```

----

* [Get Started](/marvin-platform-book/ch3_get_started/overview)
