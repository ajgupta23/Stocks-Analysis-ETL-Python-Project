# Intraday Stock Data Pipeline
A Python project that retrieves incremental intraday stock data from IEXCloud API, performs more than 10 data transformations, and loads the transformed data using UPSERT method into a PostgreSQL database. This project also logs the metadata to a Database table, a test module to test the transformations. This project can be run on AWS Cloud using the Docker image that was build by the project team.

## Requirements
- iexfinance==0.5.0
- jupyter==1.0.0
- pandas==1.4.3
- requests==2.28.1
- SQLAlchemy==1.4.39
- pyarrow==8.0.0
- pg8000==1.29.1
- pytest==7.1.2
- pylint==2.14.4
- pyyaml==6.0
- Jinja2==3.1.2
- schedule==1.1.0

## Installation
```sh
pip install -r requirements.txt
```
## Configuration
- config_stock_code.yaml - Contains the list of Stock symbols to fetch the intraday stocks data
- config.bat - Configure this file with iex_api_key, db_user, db_password, db_server_name, db_database_name if you want to run the project locally in Windows
- config.sh - Configure this file with iex_api_key, db_user, db_password, db_server_name, db_database_name if you want to run the project locally in Linux/Mac
- config.yaml - Contains log table name
- .env file - This file is required to build and run the docker image on Cloud

## Usage

To run the pipeline in your local system, execute the following command:
```sh
cd <src folder in local system>
export PYTHONPATH=`pwd` # for windows: set PYTHONPATH=%cd%
python etl/pipeline/pipeline.py
```
The pipeline will retrieve intraday stock data for the specified stock symbol, apply transformations, and load the transformed data into the PostgreSQL database.

## Data Transformations
The following transformations are applied to the data:
- Group By
- Min
- Max
- Sum
- Mean
- Rename column
- Drop column
- Add new column
- Calculation to find difference between close and open
- Filter using WHERE
 
## PostgreSQL Database
The transformed data is loaded into a PostgreSQL database using UPSERT method with the following structure:
```sql
CREATE TABLE stocks_intraday (
)
```
## Docker
Build the docker image using the following command
```sh
docker build . -t <name_of_your_tag>
```
Run the docker image
```sh
docker run --rm --env-file .env <image_name>
```
