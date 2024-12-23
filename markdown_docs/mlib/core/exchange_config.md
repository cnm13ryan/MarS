## ClassDef ExchangeConfig
**ExchangeConfig**: This class represents the configuration settings for an exchange, detailing various time intervals for market operations such as open and close times, auction periods, and continuous trading sessions.

attributes:
· symbols: A list of strings representing the financial instruments or assets traded on the exchange.
· open_time: A pandas Timestamp indicating when the market opens.
· close_time: A pandas Timestamp indicating when the market closes.
· open_auction_start_time: An optional pandas Timestamp marking the start of the opening auction period. If not provided, it is set to None.
· open_auction_end_time: An optional pandas Timestamp marking the end of the opening auction period. If not provided, it is set to None.
· continuous_auction_start_time: An optional pandas Timestamp indicating when the continuous trading session begins. If not provided, it is set to None.
· continuous_auction_end_time: An optional pandas Timestamp indicating when the continuous trading session ends. If not provided, it is set to None.
· close_auction_start_time: An optional pandas Timestamp marking the start of the closing auction period. If not provided, it is set to None.
· close_auction_end_time: An optional pandas Timestamp marking the end of the closing auction period. If not provided, it is set to None.

Code Description: The ExchangeConfig class inherits from NamedTuple, which means it is an immutable data structure that can be used to store configuration details for an exchange in a structured way. This class encapsulates all necessary time-related information about when different market activities occur, including the opening and closing of the market, as well as specific auction periods designed to facilitate trading at the start and end of the day.

The attributes of this class are defined with clear types, ensuring that they hold appropriate data for their intended use. For example, symbols is a list of strings representing the financial instruments traded on the exchange, while open_time and close_time are pandas Timestamps indicating when the market opens and closes, respectively. The auction start and end times are optional, allowing flexibility in how different exchanges handle these periods.

This configuration class is utilized by other parts of the system to schedule and manage market events accurately. For instance, the create_exchange_events function uses an ExchangeConfig object to generate a list of event objects that represent significant moments during the trading day, such as the start and end of auctions or continuous trading sessions. Similarly, the Exchange class initializes with an ExchangeConfig object, which it uses to set up its internal state and manage order books for each traded symbol.

Note: When creating an instance of ExchangeConfig, ensure that all required timestamps (open_time and close_time) are provided as pandas Timestamps. Optional auction times can be omitted if not applicable to the exchange's trading rules. This class is essential for maintaining consistency in market operations across different exchanges and facilitating automated event generation and management within the trading system.
## FunctionDef create_Chinese_stock_exchange_config(date, symbols)
**create_Chinese_stock_exchange_config**: This function generates a configuration object specifically tailored for the Chinese stock exchange market on a given date, detailing various time intervals for market operations such as open and close times, auction periods, and continuous trading sessions.

parameters:
· date: A datetime.date object representing the specific date for which the exchange configuration is being created.
· symbols: A list of strings where each string represents a financial instrument or asset traded on the Chinese stock exchange.

Code Description: The function constructs an instance of the ExchangeConfig class, which encapsulates all necessary time-related information about when different market activities occur. It uses the provided date and a predefined set of times to create Timestamp objects for various key events in the trading day using the get_ts helper function. These timestamps include the opening and closing times of the market, as well as specific auction periods designed to facilitate trading at the start and end of the day.

The open time is set to 9:10 AM, and the close time is set to 3:00 PM on the specified date. The function also defines the opening auction period from 9:15 AM to 9:25 AM, where all orders placed during this interval are matched at 9:25 AM. Similarly, the closing auction period is defined from 2:57 PM to 3:00 PM, with matching occurring at 3:00 PM. The continuous trading session starts at 9:30 AM and ends at 2:56:59.999999 PM.

Note: When using this function, ensure that the date parameter is a valid datetime.date object and that the symbols list contains strings representing the financial instruments traded on the Chinese stock exchange. This configuration object can be used by other parts of the system to schedule and manage market events accurately.

Output Example: If called with `create_Chinese_stock_exchange_config(datetime.date(2023, 10, 5), ["600519"])`, the function would return an ExchangeConfig object representing the configuration for October 5, 2023, for the stock symbol "600519", with all specified times set according to Chinese stock exchange rules.
## FunctionDef create_exchange_config_without_call_auction(market_open, market_close, symbols)
**create_exchange_config_without_call_auction**: This function generates an exchange configuration without specifying any auction periods. It is designed to set up trading sessions for a market where continuous trading occurs between specified open and close times, with no designated opening or closing auctions.

parameters:
· market_open: A pandas Timestamp indicating when the market opens.
· market_close: A pandas Timestamp indicating when the market closes.
· symbols: A list of strings representing the financial instruments or assets traded on the exchange.

Code Description: The function initializes an instance of the ExchangeConfig class, which encapsulates all necessary time-related information about when different market activities occur. In this specific configuration, the open and close times are set according to the provided parameters, while all auction start and end times are explicitly set to None, indicating that no auctions will take place during these trading sessions. The continuous trading session is defined to run from the market open time to the market close time.

Note: This function is particularly useful for setting up exchanges where continuous trading occurs without any designated auction periods at the beginning or end of the day. It ensures that the exchange configuration is correctly set up for such scenarios, facilitating accurate scheduling and management of market events.

Output Example: A possible appearance of the code's return value would be an instance of ExchangeConfig with the following attributes:
symbols: ['AAPL', 'GOOGL']
open_time: Timestamp('2024-01-01 09:30:00')
close_time: Timestamp('2024-01-01 16:00:00')
open_auction_start_time: None
open_auction_end_time: None
continuous_auction_start_time: Timestamp('2024-01-01 09:30:00')
continuous_auction_end_time: Timestamp('2024-01-01 16:00:00')
close_auction_start_time: None
close_auction_end_time: None

This output indicates that the market for 'AAPL' and 'GOOGL' opens at 9:30 AM and closes at 4:00 PM, with continuous trading throughout these hours and no auction periods.
