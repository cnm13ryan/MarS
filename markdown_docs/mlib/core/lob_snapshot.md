## ClassDef LobSnapshot
**LobSnapshot**: Represents a snapshot of a limit order book (LOB) at a specific point in time, capturing essential details such as prices and volumes for both ask and bid sides.

attributes:
· time: A pandas Timestamp indicating when this snapshot was taken.
· max_level: An integer representing the maximum depth level of the LOB that is captured in this snapshot.
· last_price: The most recent transaction price before this snapshot was taken, represented as an integer.
· ask_prices: A list of integers representing the prices at which sellers are willing to sell, ordered from lowest to highest.
· ask_volumes: A list of integers corresponding to the volumes available at each ask price in ask_prices.
· bid_prices: A list of integers representing the prices at which buyers are willing to buy, ordered from highest to lowest.
· bid_volumes: A list of integers corresponding to the volumes available at each bid price in bid_prices.

Code Description: The LobSnapshot class is a NamedTuple that encapsulates a snapshot of a limit order book. It includes attributes for capturing the timestamp, maximum level of detail, last transaction price, and lists of ask and bid prices along with their respective volumes. The class provides three properties to compute derived values from these attributes:
- spread: Calculates the difference between the best ask and bid prices. If either list is empty, it defaults to a predefined interval multiplied by ten.
- mid_price: Computes the midpoint price based on the best available ask and bid prices. It ensures that the returned price is a valid tick on the LOB. In cases where one side of the book is empty, it uses the last known price or the other side's best price as a fallback.
- float_mid_price: Provides a floating-point representation of the midpoint price without ensuring it aligns with valid ticks on the LOB. It handles scenarios where either ask or bid lists are empty by returning the available price from the non-empty list.
- float_weighted_mid_price: Calculates a weighted midpoint price based on the volumes at the best ask and bid prices. This method also manages cases of empty ask or bid lists similarly to float_mid_price.

Note: The LobSnapshot class is utilized in various parts of the project, particularly within state management and orderbook handling components. It plays a crucial role in capturing the state of the market at specific points in time, which can be used for analysis, simulation, and decision-making processes.

Output Example: A possible appearance of the code's return value when accessing its attributes and properties could look like this:
- time: 2023-10-05 14:30:00
- max_level: 10
- last_price: 15000
- ask_prices: [15005, 15010, 15015]
- ask_volumes: [200, 150, 300]
- bid_prices: [14995, 14990, 14985]
- bid_volumes: [250, 175, 225]
- spread: 10
- mid_price: 15000
- float_mid_price: 15000.0
- float_weighted_mid_price: 14998.333333333334
### FunctionDef spread(self)
**spread**: Calculates the spread between the best ask price and the best bid price from a limit order book (LOB) snapshot.
parameters:
· None: The function does not accept any parameters.

Code Description: The `spread` method is designed to compute the difference between the best available ask price and the best available bid price in a LOB snapshot. It first checks if both `ask_prices` and `bid_prices` lists are populated. If they are, it calculates the spread by subtracting the first element of `bid_prices` (the best bid) from the first element of `ask_prices` (the best ask). This difference represents the smallest price increment at which a trade could occur based on current market conditions.

If either `ask_prices` or `bid_prices` is empty, indicating an imbalance in the order book where one side lacks orders, the function assigns a default spread value. The default spread is calculated as ten times the `real_price_interval`, which is set to 100. This results in a default spread of 1000 units.

Note: Usage points. The `spread` method is crucial for understanding the liquidity and pricing dynamics in a market. It provides insight into how much more sellers are asking compared to what buyers are willing to pay, which can influence trading strategies. Additionally, this method is referenced by other functions within the same class, such as `mid_price`, where it plays a role in determining the midpoint price of the order book.

Output Example: Mock up a possible appearance of the code's return value.
- If `ask_prices = [1050]` and `bid_prices = [1040]`, then `spread()` would return `10`.
- If either `ask_prices` or `bid_prices` is empty, for example, `ask_prices = []` and `bid_prices = [1040]`, then `spread()` would return `1000`.
***
### FunctionDef mid_price(self)
**mid_price**: Get mid price of LOB snapshot, the returned price is a valid tick on LOB.
parameters:
· None: The function does not accept any parameters.

Code Description: The `mid_price` method calculates the midpoint price from a limit order book (LOB) snapshot. This midpoint represents an average price between the best bid and ask prices, adjusted to be a valid tick according to market rules. 

The calculation begins by determining the spread, which is the difference between the best ask price and the best bid price. If both `ask_prices` and `bid_prices` are available, the mid price is calculated as the best bid plus half of the spread, rounded down to the nearest valid tick (using integer division). This ensures that the mid price remains a valid market price.

If only one side of the order book has prices (either asks or bids), the method defaults to using the available price. Specifically, if there are no bid prices, it uses the best ask price as the mid price; conversely, if there are no ask prices, it uses the best bid price. 

In scenarios where both `ask_prices` and `bid_prices` are empty but a last traded price (`last_price`) is available, this method returns the `last_price`. If none of these conditions are met (i.e., all lists are empty and there is no last traded price), the function raises a `ValueError`, indicating that it cannot determine a valid mid price.

Note: The `mid_price` method is essential for determining a fair midpoint in the order book, which can be used for various trading strategies or analyses. It relies on the `spread` method to calculate the difference between the best bid and ask prices, ensuring accurate and market-aligned pricing.

Output Example:
- If `ask_prices = [1050]`, `bid_prices = [1040]`, and `last_price = 1045`, then `mid_price()` would return `1045`. Here, the spread is `10` (1050 - 1040), so the mid price is calculated as `1040 + (10 // 2) * 100 = 1045`.
- If `ask_prices = []`, `bid_prices = [1040]`, and `last_price = 1045`, then `mid_price()` would return `1040`. Since there are no ask prices, the best bid price is used.
- If `ask_prices = [1050]`, `bid_prices = []`, and `last_price = 1045`, then `mid_price()` would return `1050`. Since there are no bid prices, the best ask price is used.
- If `ask_prices = []`, `bid_prices = []`, and `last_price = 1045`, then `mid_price()` would return `1045`. Both sides of the order book are empty, so the last traded price is used.
- If `ask_prices = []`, `bid_prices = []`, and `last_price = 0`, then `mid_price()` would raise a `ValueError` because there is no valid mid price to determine.
***
### FunctionDef float_mid_price(self)
**float_mid_price**: This function calculates and returns the mid price of a Limit Order Book (LOB) snapshot as a float value. The mid price is defined as the average of the best bid price and the best ask price. If there are no available bid or ask prices, it handles these cases by returning the last known valid price.

**parameters**:
· No parameters: This function does not accept any external parameters; it operates on the internal state of the LobSnapshot object.

**Code Description**: The function begins by checking if both `ask_prices` and `bid_prices` lists are empty. If they are, it prints a warning message indicating that the LOB is empty and returns the last known price (`last_price`). It asserts that `last_price` is not None and not NaN to ensure data integrity.

If only one of the `ask_prices` or `bid_prices` lists is empty, the function returns the best available price from the non-empty list. For instance, if there are no ask prices, it returns the highest bid price (first element in `bid_prices`). Conversely, if there are no bid prices, it returns the lowest ask price (first element in `ask_prices`).

The function then asserts that both the first bid volume and the first ask volume are greater than zero to ensure that the best bid and ask prices are valid. It calculates the mid price by averaging the best bid and ask prices. Another assertion checks that the calculated mid price is indeed between the best bid and ask prices, ensuring logical consistency.

**Note**: Usage points include calling this method on an instance of `LobSnapshot` to retrieve the current mid price based on the snapshot's data. It is crucial that the `LobSnapshot` object has been properly populated with bid and ask prices before invoking this function to avoid incorrect or unexpected results.

**Output Example**: If the best bid price is 100.5 and the best ask price is 101.5, the function will return a mid price of 101.0. If there are no bid prices and the last known price was 99.8, it would return 99.8 instead.
***
### FunctionDef float_weighted_mid_price(self)
**float_weighted_mid_price**: Get weighted-mid price of LOB (Limit Order Book) snapshot.
parameters:
· No parameters: This function does not accept any external parameters; it operates on the internal state of the LobSnapshot object.

Code Description: The function calculates and returns the weighted mid-price from a Limit Order Book (LOB) snapshot. It first checks if both ask prices and bid prices are available in the LOB. If neither is present, it prints a warning message and asserts that there is a valid last price which is not NaN, returning this last price as a fallback.

If only bid prices are available, it returns the highest bid price (first element of bid_prices). Conversely, if only ask prices are available, it returns the lowest ask price (first element of ask_prices).

The function then ensures that both the first bid volume and the first ask volume are greater than zero. It calculates the weighted mid-price using the formula: 

\[ \text{weighted\_mid\_price} = \frac{\text{ask\_prices}[0] * \text{bid\_volumes}[0] + \text{bid\_prices}[0] * \text{ask\_volumes}[0]}{\text{bid\_volumes}[0] + \text{ask\_volumes}[0]} \]

This formula takes into account the volumes at the best bid and ask prices to compute a mid-price that reflects the relative importance of these two levels. The function asserts that the calculated weighted mid-price lies between the best bid price and the best ask price, ensuring logical consistency.

Note: Usage points. This function is particularly useful in financial markets for determining a fair price point based on current market conditions as reflected in the LOB snapshot. It can be used by traders, analysts, or automated trading systems to make informed decisions about buying or selling assets.

Output Example: Mock up a possible appearance of the code's return value.
Assuming the following internal state:
- ask_prices = [105, 106]
- bid_prices = [103, 104]
- ask_volumes = [2, 3]
- bid_volumes = [5, 7]

The function would calculate the weighted mid-price as follows:

\[ \text{weighted\_mid\_price} = \frac{(105 * 5) + (103 * 2)}{(5 + 2)} = \frac{525 + 206}{7} = \frac{731}{7} \approx 104.43 \]

Thus, the function would return approximately 104.43 as the weighted mid-price for this LOB snapshot.
***
