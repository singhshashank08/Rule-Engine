Rule Engine with AST
Overview
This project is a 3-tier rule engine application that evaluates user eligibility based on attributes like age department income and experience. The engine uses an Abstract Syntax Tree (AST) to represent rules and dynamically create combine and evaluate them. The rules can be stored in a database and later evaluated against user data using an API.

Features
Dynamic Rule Creation: Users can create complex eligibility rules using strings like "age > 30 AND department = 'Sales'".
AST Representation: The rules are represented internally as Abstract Syntax Trees (AST) to allow flexible manipulation.
Rule Evaluation: Given user attributes the system can evaluate whether the user satisfies a particular rule.
Rule Storage: Rules are stored in a database (MongoDB by default) and can be retrieved and evaluated at any time.
API Interface: The engine provides a REST API to create and evaluate rules.
Project Structure
bash

rule_engine_project/
│
├── app.py                      # Main application entry point (Flask API)
├── ast_engine.py                # AST-based rule engine logic
├── database.py                  # Database handling logic (MongoDB)
├── tests.py                     # Test cases for the rule engine
├── requirements.txt             # Required libraries
└── README.md                    # Project documentation
Requirements
Python 3.7+
Flask for API handling
Pydantic for validation
MongoDB (can be replaced with SQLite or another DB)
Pytest for testing
Installation
Clone the repository:

bash


Install dependencies:

Create a virtual environment and install the dependencies from requirements.txt:

bash

python3 -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install -r requirements.txt
Database Setup (MongoDB):

Make sure MongoDB is running locally on localhost:27017. If using a different database modify the connection string in database.py.

Usage
1. Start the Flask server:
bash

python app.py
2. Create Rule
To create a rule send a POST request to /create_rule with a rule string. Example:

json

POST /create_rule

{
  "rule_id": "rule1"
  "rule": "age > 30 AND department = 'Sales'"
}
This will create and store an AST representing the rule.

3. Evaluate Rule
To evaluate a rule for a specific user send a POST request to /evaluate_rule with the rule ID and user data. Example:

json

POST /evaluate_rule

{
  "rule_id": "rule1"
  "user_data": {
    "age": 35
    "department": "Sales"
  }
}
The response will indicate whether the user satisfies the rule:

json

{
  "result": true
}
4. Run Tests
Run the test suite using pytest to ensure everything works correctly:

bash

pytest tests.py
Example
Sample Rule
json

{
  "rule_id": "rule1"
  "rule": "((age > 30 AND department = 'Sales') OR (age < 25 AND department = 'Marketing')) AND (salary > 50000 OR experience > 5)"
}
Sample User Data
json

{
  "age": 35
  "department": "Sales"
  "salary": 60000
  "experience": 3
}
Evaluation Result
json

{
  "result": true
}
API Endpoints
Method	Endpoint	Description
POST	/create_rule	Create and store a new rule
POST	/evaluate_rule	Evaluate a rule against user data
Future Enhancements
Error Handling: Add robust validation and error handling for invalid rule strings or missing attributes.
Rule Modification: Allow modifying existing rules (e.g. changing conditions or operators).
User-Defined Functions: Extend rule language to support custom functions for advanced conditions.
