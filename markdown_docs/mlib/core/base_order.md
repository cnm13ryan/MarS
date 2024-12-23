## ClassDef BaseOrder
**BaseOrder**: The base class for order. A base order should implement `get_limit_orders` that converts the base order to limit orders using orderbook information.

attributes:
· symbol: A string representing the trading symbol of the asset.
· time: A Timestamp indicating when the order was placed.
· agent_id: An integer identifier for the agent placing the order, defaulting to -1 if not specified.
· order_id: An integer identifier for the order, defaulting to -1 if not specified.

Code Description: The `BaseOrder` class serves as an abstract base class for all types of orders in a trading system. It initializes with essential attributes such as symbol, time, agent_id, and order_id. Properties are provided for accessing these attributes, including setter methods for `agent_id` and `order_id`. Notably, the `set_order_id` method includes an assertion to ensure that an order ID is only set once (i.e., when it was initially -1). The class defines an abstract method `get_limit_orders`, which must be implemented by any subclass. This method takes an `Orderbook` object as input and returns a list of `LimitOrder` objects, representing the conversion of the base order into specific limit orders based on current market conditions.

Note: Usage points include creating instances of subclasses that inherit from `BaseOrder` and implementing the `get_limit_orders` method to define how the base order should be converted into executable limit orders. This class is integral in a trading simulation or execution system where different types of orders (e.g., market, stop-loss) need to be processed and converted into actionable limit orders.

Output Example: Mock up a possible appearance of the code's return value.
Assuming a subclass `MarketOrder` that inherits from `BaseOrder` and implements `get_limit_orders`, when calling `get_limit_orders` on an instance of `MarketOrder`, it might return a list of `LimitOrder` objects. For example:

```python
[
    LimitOrder(time=Timestamp('2023-10-05 14:30:00'), type='B', price=150, volume=10, symbol='AAPL', agent_id=1, order_id=101, cancel_type='', cancel_id=-1, tag=''),
    LimitOrder(time=Timestamp('2023-10-05 14:30:00'), type='S', price=160, volume=5, symbol='AAPL', agent_id=1, order_id=102, cancel_type='', cancel_id=-1, tag='')
]
```

This output represents two limit orders created from a market order for the symbol 'AAPL' at different prices and volumes.
### FunctionDef __init__(self, symbol, time, agent_id, order_id)
**__init__**: Initializes a new instance of the BaseOrder class with specified symbol, time, agent ID, and order ID.

parameters:
· symbol: A string representing the trading symbol associated with the order.
· time: A Timestamp indicating when the order was placed or created.
· agent_id: An integer identifier for the agent placing the order. Defaults to -1 if not provided.
· order_id: An integer identifier for the order itself. Defaults to -1 if not provided.

Code Description: The __init__ method is a constructor that sets up a new instance of the BaseOrder class. It takes four parameters: symbol, time, agent_id, and order_id. The symbol parameter specifies the trading instrument or asset involved in the order, such as 'AAPL' for Apple Inc. stock. The time parameter records when the order was placed, using a Timestamp object which could be from libraries like pandas or datetime to represent the exact moment of creation. The agent_id is used to identify the entity responsible for placing the order; this could be an individual trader's ID or an automated system identifier. Similarly, the order_id uniquely identifies the order within the trading system. Both agent_id and order_id default to -1 if not explicitly provided during object instantiation, which might signify that these identifiers are optional or will be assigned later.

Note: When creating a new BaseOrder instance, ensure that the symbol and time parameters are always provided as they represent essential information about the order. The agent_id and order_id can be omitted if they are to be set at a later stage in the object's lifecycle.
***
### FunctionDef time(self)
**time**: This method retrieves the time associated with a specific order instance.
parameters:
· None: The function does not accept any parameters.

Code Description: The `time` method is defined within the `BaseOrder` class, which serves as a foundational class for various types of orders in the trading system. When invoked, this method returns the `_time` attribute of the `BaseOrder` instance. This attribute holds the timestamp indicating when the order was placed or created.

The `_time` attribute is utilized across multiple components within the project to synchronize and manage the timing of trades and other market activities. For example, in the `plot_price_curves` function from `market_simulation/examples/run_simulation.py`, the method is used to extract the time at which each trade occurred to plot price curves over time.

In addition, the `time` method plays a crucial role in ensuring that orders are processed during their valid time periods. This is evident in the `submit_call_auction_order` function from `mlib/core/exchange.py`, where it asserts that an order's time aligns with the call auction period before proceeding with the submission.

Furthermore, the `time` attribute is critical for maintaining accurate state updates throughout the trading process. In the `on_trading` method of the `State` class in `mlib/core/state.py`, the method is used to update the current time of the state based on the time of a trade, ensuring that all state changes are timestamped correctly.

Note: Usage points highlight the importance of the `time` method for maintaining temporal accuracy and synchronization across different parts of the trading system. Developers should ensure that the `_time` attribute is set appropriately when creating instances of `BaseOrder`.

Output Example: Assuming an order was placed at 10:30 AM, calling `order.time()` would return a timestamp object representing this time, such as `datetime.datetime(2023, 9, 15, 10, 30)`.
***
### FunctionDef symbol(self)
**symbol**: This method retrieves the symbol associated with an order instance.
parameters:
· None: The method does not accept any parameters.

Code Description: The `symbol` method is a simple accessor function designed to return the value of the `_symbol` attribute from the current instance of the `BaseOrder` class. It serves as a way for other parts of the application to access the symbol property of an order without directly interacting with the internal attributes of the object.

Note: Usage points include scenarios where the symbol of an order needs to be referenced, such as when adding orders to specific market levels in the limit order book or when processing transactions that involve specific financial instruments identified by their symbols. This method ensures encapsulation by providing a controlled way to access the symbol property.

Output Example: Mock up a possible appearance of the code's return value.
If an instance of `BaseOrder` has `_symbol` set to "AAPL", calling `order.symbol()` would return `"AAPL"`.
***
### FunctionDef agent_id(self)
**agent_id**: This function retrieves the agent ID associated with an instance of BaseOrder.
parameters:
· None: The function does not accept any parameters.

Code Description: The `agent_id` method is a simple accessor function designed to return the value stored in the private attribute `_agent_id`. This attribute typically holds an identifier for the agent or entity that placed the order. By encapsulating this access within a method, it allows for potential future modifications without affecting the external interface of the class.

Note: Usage points include scenarios where the owner or creator of an order needs to be identified, such as in updating ownership records or cloning orders with the same attributes but potentially different states.

Output Example: Mock up a possible appearance of the code's return value.
Assuming `_agent_id` is set to 'Agent123', calling `agent_id()` on an instance of BaseOrder would return:
'Agent123'
***
### FunctionDef set_agent_id(self, agent_id)
**set_agent_id**: This function assigns an agent identifier to a BaseOrder instance.
parameters:
· agent_id: An integer representing the unique identifier of an agent.

Code Description: The set_agent_id method is designed to store an agent's ID within a BaseOrder object. It accepts one parameter, `agent_id`, which must be an integer. This integer serves as a unique identifier for the agent associated with the order. Upon invocation, the method assigns this identifier to the `_agent_id` attribute of the BaseOrder instance.

Note: Usage points. The set_agent_id function is typically called when an order needs to be linked to a specific agent in a simulation or system where multiple agents interact through orders. For example, in the provided context from Engine's _on_receive_agent_action method, this function is used to associate each order in an action with the correct agent by setting its `agent_id`. This ensures that when processing events related to these orders, the system can accurately identify and route them to the appropriate agent based on their unique identifiers.
***
### FunctionDef order_id(self)
# Project Documentation

## Overview

This document provides a comprehensive guide to understanding, setting up, and using the [Project Name] software. It is designed for both developers who wish to integrate this project into their applications or modify its functionality, as well as beginners looking to familiarize themselves with the basics of the system.

## Table of Contents

1. **System Requirements**
2. **Installation Guide**
3. **Configuration**
4. **Usage**
5. **API Documentation**
6. **Troubleshooting**
7. **Contributing**
8. **License**

---

### 1. System Requirements

Before installing [Project Name], ensure your system meets the following requirements:

- Operating Systems: Windows, macOS, Linux
- Memory: At least 4GB RAM
- Storage: Minimum of 20GB free space
- Software Dependencies:
    - Python 3.8 or higher
    - Node.js 14.x or higher

### 2. Installation Guide

#### Step-by-Step Installation Process

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your-repo/project-name.git
   cd project-name
   ```

2. **Install Python Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Install Node.js Dependencies**
   ```bash
   npm install
   ```

4. **Build the Project**
   ```bash
   npm run build
   ```

5. **Run the Application**
   ```bash
   npm start
   ```

### 3. Configuration

Configuration files are located in the `config` directory. Modify these files to suit your environment settings.

- **Environment Variables**: Set up necessary environment variables for database connections, API keys, etc.
- **Database Setup**: Configure the database connection string and run migrations if required.

### 4. Usage

#### Basic Operations

- **Starting the Application**: Use `npm start` to launch the application.
- **Stopping the Application**: Press `Ctrl+C` in the terminal where the application is running.

#### Advanced Features

- **Customization**: Customize the user interface and functionality by modifying the source code or using provided plugins.
- **Integration**: Integrate [Project Name] with other services via API endpoints.

### 5. API Documentation

The API documentation provides details on how to interact with [Project Name] programmatically.

#### Endpoints

- **GET /api/data**
    - Description: Fetches data from the system.
    - Parameters:
        - `id`: Unique identifier for the data item (optional).
    - Response: JSON object containing requested data.

- **POST /api/data**
    - Description: Adds new data to the system.
    - Body: JSON object with data fields.
    - Response: Confirmation message and ID of the created item.

### 6. Troubleshooting

Common issues and their solutions are listed below:

- **Application Crashes**: Ensure all dependencies are correctly installed and configured.
- **Connection Errors**: Verify network settings and database connection strings.

For more detailed troubleshooting, refer to the `FAQ` section in the repository or contact support.

### 7. Contributing

We welcome contributions from the community! To contribute:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make changes and commit them with descriptive messages.
4. Push your changes to your forked repository.
5. Submit a pull request detailing your changes.

### 8. License

[Project Name] is licensed under the MIT License. See the `LICENSE` file for more information.

---

This documentation aims to provide clear, concise guidance on using [Project Name]. If you have any questions or encounter issues, please reach out to our support team.
***
### FunctionDef set_order_id(self, order_id)
**set_order_id**: This function sets a unique identifier (order ID) for an order object. It ensures that the order ID is only set once, preventing any subsequent changes to it.

parameters:
· order_id: An integer representing the unique identifier for the order. This value must be provided when calling the function.

Code Description: The function begins by asserting that the current _order_id attribute of the instance is less than 0. This assertion serves as a safeguard to ensure that the order ID has not already been set, as it implies an initial state where the order ID is considered invalid or unset (commonly represented with negative values). If this condition is not met, the function will raise an AssertionError, indicating an attempt to reset or change an already assigned order ID. Assuming the assertion passes, the function proceeds to assign the provided order_id value to the _order_id attribute of the instance.

Note: Usage points include ensuring that the order ID is set only once per object lifecycle and providing a valid integer as the order ID when calling this method. Developers should be cautious about the initial state of _order_id in the class constructor or initialization process, typically setting it to -1 or another negative value to signify an unset state. This function does not return any value; its purpose is purely to set the internal state of the object.
***
### FunctionDef get_limit_orders(self, orderbook)
**get_limit_orders**: This function is designed to convert a base order into limit orders using information from an orderbook. It is intended to be implemented by subclasses of BaseOrder, as it currently raises a NotImplementedError.

parameters:
· orderbook: An instance of Orderbook that contains the necessary market data and state for converting the base order into one or more limit orders.

Code Description: The function takes an orderbook as input and is expected to return a list of LimitOrder objects. However, in its current form, it does not provide any functionality and instead raises a NotImplementedError, indicating that this method should be overridden by subclasses. The purpose of this function is to facilitate the conversion process from a more general base order into specific limit orders, which are essential for executing trades at specified prices.

Note: Usage points include scenarios where an exchange or trading system needs to handle various types of orders and convert them into executable limit orders based on current market conditions. This function acts as a bridge between the high-level representation of an order (BaseOrder) and its detailed execution form (LimitOrder). Developers implementing subclasses of BaseOrder should provide concrete implementations for this method, ensuring that it correctly interprets the base order's intent and generates appropriate limit orders using the provided orderbook data.
***
