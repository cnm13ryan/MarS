## ClassDef Observation
**Observation**: Represents an observation made by an agent within a market simulation environment. It encapsulates essential information about the current state of the simulation, including the timestamp, the agent making the observation, and whether the observation is related to the market opening.

attributes:
· time: A Timestamp indicating when the observation was made.
· agent: An instance of BaseAgent representing the agent that made the observation.
· is_market_open_wakeup: A boolean flag indicating if the observation corresponds to a wakeup event at market open.

Code Description: The Observation class is defined as a NamedTuple, which means it is an immutable data structure. This immutability ensures that once an instance of Observation is created, its attributes cannot be altered, providing consistency and safety in the simulation environment. The time attribute holds a Timestamp object, representing the exact moment when the observation was recorded. The agent attribute is a reference to the BaseAgent instance that made the observation, allowing for easy access to the agent's properties and methods. Lastly, the is_market_open_wakeup boolean flag is used to distinguish between regular wakeup events and those occurring at market open, which might require special handling in the simulation logic.

Note: The Observation class plays a crucial role in the interaction between agents and the environment in a market simulation. It serves as a carrier of information that agents use to make decisions about their actions. For instance, in the get_action method of BaseAgent, the is_market_open_wakeup flag is used to determine whether to return an empty order list or proceed with generating orders based on the current state of the limit order book (LOB). Similarly, in the NoiseAgent's get_action method, the observation's time attribute is utilized to decide when to start and stop generating actions. The Engine class constructs instances of Observation by gathering relevant data from agents and the simulation environment, ensuring that each agent receives up-to-date information necessary for making informed decisions.
