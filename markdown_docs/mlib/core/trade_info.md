## ClassDef TradeInfo
**TradeInfo**: Represents a snapshot of trade information including an order, transactions, and the state of the limit order book (LOB) at a specific point in time.

attributes:
· order: An instance of LimitOrder representing the order that was executed or submitted.
· transactions: A list of Transaction objects detailing the trades resulting from the order execution.
· lob_snapshot: An instance of LobSnapshot capturing the state of the limit order book at the time the order was processed.

Code Description: The TradeInfo class is designed to encapsulate all relevant data about a trade event in a financial market simulation. It holds three key pieces of information:
- The LimitOrder that triggered the trade or was submitted.
- A list of Transaction objects, each representing an individual trade that occurred as a result of the order's execution.
- A LobSnapshot object which provides a snapshot of the limit order book at the exact time the order was processed. This includes details such as bid and ask prices, quantities, and other relevant market data.

This class is crucial for tracking and analyzing trading activities within the simulation environment. It serves as a data structure that can be used by various components of the system to understand the impact of trades on the market state. For instance, it is utilized in functions like `run_simulation` where trade information is collected over time and then analyzed or visualized.

Note: Usage points include:
- Collecting detailed trade information for analysis during simulations.
- Providing a structured way to store and access order details, transactions, and LOB states.
- Facilitating the visualization of trading activities, such as plotting price curves based on collected trade data.
### FunctionDef __init__(self, order, trans, lob_snapshot)
# Project Documentation

## Overview

This document provides a comprehensive guide to understanding, setting up, and using our software project. It is designed for both developers who wish to contribute to the project and beginners looking to integrate or use the software in their applications.

## Table of Contents

1. [System Requirements](#system-requirements)
2. [Installation Guide](#installation-guide)
3. [Configuration](#configuration)
4. [Usage](#usage)
5. [API Documentation](#api-documentation)
6. [Contributing](#contributing)
7. [License](#license)

---

## System Requirements

To run this software, ensure your system meets the following requirements:

- **Operating Systems**: Windows 10/11, macOS Mojave (10.14) or later, Ubuntu 20.04 LTS
- **Hardware**:
    - Minimum: 2 GB RAM, 500 MB free disk space
    - Recommended: 4 GB RAM, 1 GB free disk space
- **Software**: 
    - Python 3.8 or higher
    - Node.js 14.x or higher (for front-end components)
    - Git for version control

---

## Installation Guide

### Step-by-Step Instructions

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your-repo/project-name.git
   cd project-name
   ```

2. **Set Up Virtual Environment (Python)**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   npm install  # If there are front-end dependencies
   ```

4. **Run the Application**:
   ```bash
   python app.py  # Adjust based on your entry point script
   ```

---

## Configuration

### Environment Variables

- `API_KEY`: Your API key for accessing external services.
- `DATABASE_URL`: Connection string to your database.

Set these variables in a `.env` file at the root of your project directory:

```plaintext
API_KEY=your_api_key_here
DATABASE_URL=postgresql://user:password@localhost/dbname
```

### Configuration Files

Configuration files are located in the `/config` directory. Modify them according to your environment settings.

---

## Usage

### Basic Operations

- **Start Server**: Use `python app.py` to start the server.
- **Access API Endpoints**: Visit `http://localhost:5000/api/endpoint` for available endpoints.

### Advanced Features

For advanced features, refer to the [API Documentation](#api-documentation) section.

---

## API Documentation

The API documentation provides detailed information about all available endpoints, request/response formats, and authentication methods. It is hosted at `http://localhost:5000/api/docs`.

- **Authentication**: Use Bearer tokens for secure access.
- **Endpoints**:
    - `/api/data`: Fetch data from the database.
    - `/api/user`: Manage user profiles.

---

## Contributing

We welcome contributions to improve our project. To contribute, follow these steps:

1. Fork the repository on GitHub.
2. Create a new branch for your feature or bug fix.
3. Make changes and commit them with clear messages.
4. Push your changes to your forked repository.
5. Submit a pull request detailing your changes.

### Code Style

Adhere to PEP 8 guidelines for Python code and Airbnb's JavaScript style guide for front-end components.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Feel free to reach out if you have any questions or need further assistance. Happy coding!
***
