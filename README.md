# BIRD
SQL generation pipeline for the BIRD benchmark dataset. Uses prompt-engineered agents (generator and repair with tool calling) to achieve 68% accuracy.

## Running the code
Install the BIRD dev dataset in postgreSQL.

Install the required libraries:

```
pip install "pyautogen[openai]" psycopg2-binary
```

Set up your `OPENAI_API_KEY` in the enviroment variables.

Define credentials and path to your database and the question set in the code.

```
DATABASE = ""
DB_USER = ""
DB_HOST = ""
DB_PASSWORD = ""
DB_PORT = ""
```

Also add the path to the json with the quetions. This is the mini_dev_postgresql.json file.  

```
JSON_QUESTIONS = ""
```
