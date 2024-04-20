# Machine Learning Pipeline for Short-Term Rental Prices in NYC

ML DevOps project: Imagine you are working for a property management company renting rooms and properties for short periods of time on various rental platforms. Estimate the typical price for a given property based 
on the price of similar properties. New data is received in bulk every week. The model needs to be retrained with the same cadence, necessitating an end-to-end pipeline that can be reused.
In this project you will build such a pipeline.


## Starter kit
Data has been downloaded from the [Udacity build-ml-pipeline-for-short-term-rental-prices starter kit](https://github.com/udacity/build-ml-pipeline-for-short-term-rental-prices.git).

Clone the repository locally:

```
git clone https://github.com/[your github username]/build-ml-pipeline-for-short-term-rental-prices.git
```

and go into the repository:

```
cd build-ml-pipeline-for-short-term-rental-prices
```

## Environment
Make sure to have conda installed and ready, then create a new environment using the ``environment.yml`` file provided in the root of the repository and activate it:

```bash
$ conda env create -f environment.yml
$ conda activate nyc_airbnb_dev
```

## Get API key for Weights and Biases
Let's make sure we are logged in to Weights & Biases. Get your API key from W&B by going to 
[https://wandb.ai/authorize](https://wandb.ai/authorize) and click on the + icon (copy to clipboard), 
then paste your key into this command:

```bash
$ wandb login [your API key]
```

You should see a message similar to:
```
wandb: Appending key for api.wandb.ai to your netrc file: /home/[your username]/.netrc
```

## Cookie cutter
You can use to create stubs for new pipeline components. It is not required that you use this, but it might save you from a bit of 
boilerplate code.

```bash
$ cookiecutter cookie-mlflow-step -o src

step_name [step_name]: basic_cleaning
script_name [run.py]: run.py
job_type [my_step]: basic_cleaning
short_description [My step]: This steps cleans the data
long_description [An example of a step using MLflow and Weights & Biases]: Performs basic cleaning on the data and save the results in Weights & Biases
parameters [parameter1,parameter2]: parameter1,parameter2,parameter3
```

This will create a step called ``basic_cleaning`` under the directory ``src`` with the following structure:

```bash
$ ls src/basic_cleaning/
conda.yml  MLproject  run.py
```

You can now modify the script (``run.py``), the conda environment (``conda.yml``) and the project definition 
(``MLproject``) as you please.

The script ``run.py`` will receive the input parameters ``parameter1``, ``parameter2``,
``parameter3`` and it will be called like:

```bash
$ mlflow run src/step_name -P parameter1=1 -P parameter2=2 -P parameter3="test"
```

## Running the entire pipeline or just a selection of steps
In order to run the pipeline execute:

```bash
$  mlflow run .
```
This will run the entire pipeline.

To run only the ``download`` step, execute the below:

```bash
$ mlflow run . -P steps=download
```
If you want to run the ``download`` and the ``basic_cleaning`` steps, you can similarly do:
```bash
$ mlflow run . -P steps=download,basic_cleaning
```
You can override any other parameter in the configuration file using the Hydra syntax, by
providing it as a ``hydra_options`` parameter. For example, say that we want to set the parameter
modeling -$ random_forest -> n_estimators to 10 and etl->min_price to 50:

```bash
$ mlflow run . \
  -P steps=download,basic_cleaning \
  -P hydra_options="modeling.random_forest.n_estimators=10 etl.min_price=50"
```

## License

[License](LICENSE.txt)
