# Currency Converter

## Overview

This Currency Converter script provides a simple way to convert an amount from one currency to another using the ExchangeRate API. It involves user input for currency codes and amounts, makes a request to an external API to fetch exchange rates, and then calculates and displays the converted amount.

## Features

- **User Input Handling**: Prompts users to input the amount and the source and target currencies.
- **API Integration**: Fetches real-time exchange rates from an external API.
- **Error Handling**: Manages invalid input and API response errors.

## Code Breakdown

### 1. Imports

```python
import requests
```

- `requests`: This module is used to make HTTP requests to the ExchangeRate API to get currency conversion rates.

### 2. Class Definition: `currency_converter`

The `currency_converter` class contains methods for handling user input and performing currency conversion.

#### Method: `get_user_input()`

```python
def get_user_input():
    try:
        amount = input("Amount: ")
        if amount.isdigit():
            amount = int(amount)
        else:
            amount = float(amount)
            if amount <= 0:
                raise ValueError("Amount must be greater than 0")
    except ValueError:
        print("Invalid amount. Please enter a valid number greater than 0.")
    curr1 = str(input("From: ").upper())
    curr2 = str(input("To: ").upper())
    return curr1, curr2, amount
```

- **Purpose**: Collects the amount to be converted and the currency codes from the user.
- **Input Handling**:
  - **Amount**: The input is first checked to see if it’s a digit. If so, it is converted to an integer. Otherwise, it is converted to a float. The script also checks if the amount is positive, raising a `ValueError` if it is not.
  - **Currency Codes**: Converts the user input for currency codes to uppercase for consistency.
- **Return Value**: Returns the source currency code (`curr1`), target currency code (`curr2`), and the amount to be converted.

#### Method: `bring_currencies(curr1, curr2, amount)`

```python
def bring_currencies(curr1, curr2, amount):
    api_url = "api_key"
    api_url = "https://v6.exchangerate-api.com/v6/api_key/latest/pair/{curr1}/{curr2}/{amount}"
    response = requests.get(api_url)
    data = response
    if response.status_code == 200:
        data = response.json()
        print("{amount} {curr1} is equal to {converted_amount:.2f} {curr2}")
        try:
            conversion_rate = data['rates'][curr2]
            converted_amount = amount * conversion_rate
            print(converted_amount)
        except KeyError:
            print("Error: Invalid currency code or conversion rate not found.")
    else:
        print("Error:", response.status_code)
        return None
```

- **Purpose**: Fetches the conversion rate from the ExchangeRate API and calculates the converted amount.
- **API URL**: Constructs the API URL with the provided currency codes and amount. Note that `api_key` should be replaced with a valid API key.
- **HTTP Request**: Uses `requests.get` to make a GET request to the API.
- **Response Handling**:
  - **Status Code 200**: Indicates a successful request. The response is converted to JSON format.
    - **Conversion Calculation**: Retrieves the conversion rate for the target currency and calculates the converted amount.
    - **Error Handling**: If the target currency code is not found in the response, a `KeyError` is caught.
  - **Other Status Codes**: Prints an error message with the status code if the request fails.

### 3. Main Execution

```python
if __name__ == '__main__':
    curr1, curr2, amount = currency_converter.get_user_input()
    converted_amount = currency_converter.bring_currencies(amount, curr1, curr2)
    if converted_amount is not None:
        print("{amount} {curr1} is equal to {converted_amount:.2f} {curr2}")
```

- **Purpose**: Executes the script when run as a standalone program.
- **Flow**:
  - Calls `get_user_input()` to gather data from the user.
  - Passes the collected data to `bring_currencies()` to perform the conversion.
  - Prints the result if the conversion was successful.

## Important Notes

- **API Key**: Replace `"api_key"` in the URL with your actual API key from the ExchangeRate API.
- **Error Handling**: Ensure that valid currency codes are used and that the API key is correct to avoid errors.
- **Dependencies**: This script requires the `requests` module, which can be installed using `pip install requests`.

## Usage

1. **Run the Script in the shell**:
   ```
   python currency_converter.py
   ```

2. **Provide Input**:
   - Enter the amount to convert.
   - Provide the source and target currency codes.

3. **View Results**:
   - The script will display the converted amount if the input and API request are valid.
