# Country Info API - AWS Lambda Project

## Overview
This project is an AWS Lambda function that provides country-specific information through an API Gateway. The Lambda function retrieves data from an external CSV file hosted on GitHub, which contains a list of countries along with their details. Users can query the API by specifying a country name, and the API returns relevant data for that country.

## Features
- Serverless Deployment: The function is deployed using AWS Lambda and exposed via an API Gateway.
- Dynamic Data Fetching: The function fetches real-time country data from a remote CSV file.
- Error Handling: The API gracefully handles missing or incorrect query parameters and returns appropriate error messages.
- CloudFormation Support: This project can be deployed and managed using AWS CloudFormation templates for automated infrastructure setup.

## How It Works
1. CSV Data Source: The function fetches country data from the following URL:
```
https://raw.githubusercontent.com/icyrockcom/country-capitals/master/data/country-list.csv
```
1. Query Parameter: The API expects a query parameter CountryName.
1. Response: The Lambda function responds with the corresponding country data in JSON format or an error message if the country is not found.
## API Endpoints
- GET /country-info
  - Query Parameter: CountryName
  - Example Request:
  ```bash
  curl -X GET 'https://your-api-gateway-url/country-info?CountryName=France'
  ```
  - Response:
  ```json
  {
      "country": "France",
      "capital": "Paris",
      "continent": "Europe"
  }
  ```
- Error Responses:
  -400 Bad Request: If the CountryName parameter is missing.
  ```json
  {
      "error": "CountryName query parameter is required"
  }
  ```
  - 404 Not Found: If the specified country is not in the data source.
  ```json
  {
      "error": "Country XYZ not found"
  }
  ```
## Code Explanation

### Lambda Handler
```python
# Lambda handler function
def lambda_handler(event, context):
```
This is the entry point for the AWS Lambda function. It handles incoming API requests and generates appropriate responses.
### Loading Country Data
```python
# Function to load country data from the CSV file
def load_country_data():
```
This function fetches and parses the CSV data from the remote URL, storing it in a dictionary for quick lookups.
### Error Handling
The code checks for missing query parameters and handles cases where the requested country is not found, ensuring the API responds with meaningful error messages.
## Deployment
### Prerequisites
- AWS account
- AWS CLI configured
- AWS CloudFormation template (if using infrastructure as code)
### Steps
1. Package the Lambda Function
```
zip lambda_function.zip lambda_function.py
```
1. Deploy using AWS CLI
```
aws lambda create-function \
  --function-name CountryInfoAPI \
  --runtime python3.8 \
  --role <execution-role-arn> \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://lambda_function.zip
```
1. Set Up API Gateway
- Create a new API in API Gateway.
- Define a GET method and integrate it with the Lambda function.
- Deploy the API to a stage.
1. Test the API
Use tools like curl, Postman, or your browser to test the endpoint.
### Using CloudFormation
If you have a CloudFormation template, deploy the stack using:
```
aws cloudformation deploy --template-file template.yaml --stack-name CountryInfoAPIStack
```
## Improvements
- Caching: Add caching to reduce the number of times the CSV file is fetched.
- Data Validation: Improve validation for country names to handle variations and misspellings.
- Logging: Add logging for better monitoring and debugging.
Unit Tests: Implement unit tests for the handler and data loading functions.
## Contributing
Contributions are welcome! If you'd like to contribute to this project, feel free to open a pull request or report issues.
## License
This project is licensed under the MIT License.
