## ClassDef PvInfo
**PvInfo**: This class represents a named tuple containing price and volume information, typically used in financial data analysis to store individual price-volume pairs.

attributes:
· price: An integer representing the price of an asset or item.
· volume: An integer representing the quantity of an asset or item traded at the specified price.

Code Description: The PvInfo class inherits from Python's NamedTuple, which is a factory function for creating tuple subclasses with named fields. This means that instances of PvInfo will have 'price' and 'volume' attributes accessible by name as well as position. The class includes a static method get_vwap, which calculates the Volume Weighted Average Price (VWAP) from a list of PvInfo objects. VWAP is a trading metric used to determine the average price at which a security has traded throughout the day, based on both volume and price.

The get_vwap method takes a list of PvInfo instances as input. It first asserts that the list is not empty. Then, it calculates the total money by summing up the product of price and volume for each item in the list. Similarly, it computes the total volume by summing up all volumes. The method includes assertions to ensure that both total_money and total_volume are greater than zero, preventing division by zero errors. Finally, it returns the VWAP by dividing the total money by the total volume.

Note: Usage points include creating instances of PvInfo with specific price and volume values and using these instances as input to the get_vwap method for calculating the Volume Weighted Average Price from a series of trades or quotes.

Output Example: Mock up a possible appearance of the code's return value.
Suppose we have the following list of trades:
- Trade 1: price = 10, volume = 5
- Trade 2: price = 15, volume = 10

Creating PvInfo instances for these trades and passing them to get_vwap would look like this:

```python
trade1 = PvInfo(price=10, volume=5)
trade2 = PvInfo(price=15, volume=10)

vwap = PvInfo.get_vwap([trade1, trade2])
```

The calculated VWAP for these trades would be 13.33 (rounded to two decimal places).
### FunctionDef get_vwap(pv_infos)
**get_vwap**: This function calculates the Volume Weighted Average Price (VWAP) from a list of price-volume information objects.

parameters:
· pv_infos: A list of "PvInfo" objects, where each object contains at least two attributes - 'price' and 'volume'. These represent the price and trading volume for a particular period or transaction.

Code Description: The function starts by asserting that the input list 'pv_infos' is not empty. It then calculates the total money traded by summing up the product of price and volume for each item in the list. Similarly, it computes the total volume by summing up all the volumes in the list. After ensuring both total money and total volume are greater than zero (to avoid division by zero), it returns the VWAP by dividing the total money by the total volume.

Note: This function assumes that the 'PvInfo' objects have correctly populated 'price' and 'volume' attributes. It is crucial to ensure these values are valid numbers before passing them to this function. The function will raise an AssertionError if the input list is empty or if any of the calculated totals (total money or total volume) are not positive.

Output Example: If the input list contains two "PvInfo" objects with prices and volumes as follows:
- First object: price = 10, volume = 5
- Second object: price = 20, volume = 10

The function will calculate the total money as (10*5) + (20*10) = 250 and the total volume as 5 + 10 = 15. The returned VWAP would be 250 / 15 ≈ 16.67.
***
