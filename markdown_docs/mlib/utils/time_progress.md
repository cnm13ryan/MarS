## ClassDef TimeProgress
**TimeProgress**: Show the time progress given start time and end time.

attributes:
· start_time: The starting timestamp of the time period being tracked.
· end_time: The ending timestamp of the time period being tracked.
· description: A string describing the task or process for which the time progress is being shown.
· unit: The unit of time used to divide the time range between start_time and end_time. Default is "min" (minutes).
· refresh_per_second: The frequency at which the progress bar updates in seconds. Default is 0.2 seconds.

Code Description: TimeProgress is a class designed to visually represent the progression of time within a specified period, from start_time to end_time. It utilizes the `progress` library to create and manage a progress bar that reflects how much of the total time has elapsed based on the current timestamp provided during updates. The class initializes with a description of the task, the unit for dividing the time range, and the refresh rate for updating the progress bar.

The units attribute is a list of timestamps generated using pandas' `date_range` function, which divides the interval from start_time to end_time into equal parts based on the specified unit. This list serves as a reference to determine how much time has passed relative to the total duration.

The update method calculates the number of completed time units by comparing each element in the units list against the current timestamp provided during updates. If an element is less than or equal to the current timestamp, it signifies that this unit of time has been completed and is removed from the list. The progress bar is then updated with the new completion status.

Note: Usage points include scenarios where tracking the progression of a task over time is necessary, such as in simulations, real-time data processing, or any application requiring temporal monitoring. TimeProgress can be integrated into larger systems by creating an instance of it and calling its update method periodically with the current timestamp to reflect progress accurately.
### FunctionDef __init__(self, start_time, end_time, description, unit, refresh_per_second)
**__init__**: Initializes a TimeProgress object to manage and display progress over a specified time range.

parameters:
· start_time: The starting timestamp of the time period.
· end_time: The ending timestamp of the time period.
· description: A string describing the task or process being tracked.
· unit: A string representing the time unit for dividing the time period (default is "min" for minutes).
· refresh_per_second: A float indicating how often the progress display should be refreshed in seconds (default is 0.2).

Code Description: The __init__ method sets up a TimeProgress object to track and visualize progress over a defined time range from start_time to end_time. It divides this period into units specified by the 'unit' parameter, creating a list of timestamps that represent these divisions. A Progress object from the rich library is initialized with specific columns for displaying task descriptions, progress bars, remaining time, and elapsed time. The refresh rate for updating the display is set according to the 'refresh_per_second' parameter. A task is added to this Progress object with the provided description and a total count of units as its completion goal. This setup allows for real-time tracking and visualization of how much of the specified time period has been completed.

Note: Usage points include initializing a TimeProgress object by providing start and end times, an optional description, and optionally specifying the unit of time division and refresh rate. This is particularly useful in scenarios where tasks are expected to span over a known duration, and progress needs to be monitored or reported visually.
***
### FunctionDef update(self, current_time)
**update**: This function updates the progress tracking mechanism by checking how many time units have been completed up to a given current timestamp.

**parameters**:
· current_time: A Timestamp object representing the current point in time during the process being tracked.

**Code Description**: The `update` method is designed to incrementally track the completion of tasks based on timestamps. It starts by initializing a variable `completed` to zero, which will count how many units have been completed since the last update. Instead of using the potentially slow `(current_time - start_time).total_seconds()` for comparison, it directly compares each unit in the `units` list with the `current_time`. If a unit is less than or equal to `current_time`, it signifies that this unit has been completed, and thus it is removed from the `units` list using `pop(0)`, and `completed` is incremented. This process continues until a unit is found that is greater than `current_time`, indicating that no further units have been completed at this point in time.

If any units were marked as completed (`completed > 0`), the method updates the total number of completed units by adding `completed` to `total_completed`. It then calls the `update` method on the `progress` object, passing it the task identifier, the updated count of completed units, and a description string that includes the current time. This step is crucial for reflecting the progress in real-time during the execution of tasks.

**Note**: The function assumes that `self.units`, `self.total_completed`, `self.progress`, `self.task`, and `self.description` are properly initialized elsewhere in the class. It is typically called within a loop where events or time intervals are processed, as seen in the `run` method of `Engine` and the `env` generator method of `Env`. Each call to `update` should be made with the current timestamp corresponding to the event being processed, ensuring that the progress tracking remains accurate.
***
