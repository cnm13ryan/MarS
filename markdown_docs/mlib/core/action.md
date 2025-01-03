## ClassDef Action
**Action**: Represents an action taken by an agent in a trading simulation environment. It encapsulates details such as the time of the action, the identifier of the agent performing the action, the list of orders generated by the agent, and the next scheduled wakeup time for the agent.

attributes:
· time: A Timestamp indicating when the action was performed.
· agent_id: An integer identifier for the agent that took the action.
· orders: A list of BaseOrder objects representing the orders placed by the agent as part of this action.
· next_wakeup_time: An optional Timestamp specifying when the agent should be woken up next to take further actions. If None, it indicates no future wakeups are scheduled.

Code Description: The Action class is defined as a NamedTuple, which means it is an immutable data structure that holds a fixed set of attributes. This immutability ensures that once an instance of Action is created, its values cannot be altered, providing consistency and safety in the simulation environment. The time attribute records when the action occurred, while agent_id identifies the specific agent responsible for the action. The orders attribute contains a list of BaseOrder objects, each representing an order placed by the agent during this action. These orders could be of various types (e.g., market, limit) and are intended to be processed further in the simulation. Lastly, next_wakeup_time is used to schedule when the agent should be re-activated for subsequent actions. If no future wakeups are needed, this attribute can be set to None.

Note: Usage points include creating instances of Action within methods that determine an agent's behavior based on current observations or states in a trading simulation environment. For example, agents might generate orders based on market conditions and schedule their next wakeup times accordingly. The Engine component processes these actions by handling the orders and scheduling future wakeups for agents as specified by the next_wakeup_time attribute. This class is integral to managing agent activities within the simulation framework, ensuring that all actions are recorded accurately with associated timestamps and order details.
