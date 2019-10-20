# Original batteries
My EQ3 valves were supplied with Gassner batteries. I have 8 valves in the house and I have now replaced the batteries in all of them after about 2 years. Not all of the cells have been exhausted - six of the battery replacements were due to leakage. One of my valves now has no LCD as the leaking battery caused part of the PCB that drives the LCD to be corroded. The valve still operates although it is difficult to re-enable BLE after changing the batteries - I have to follow the correct sequence of button presses and hope they all work.

# Battery life
The two valves where the batteries expired without leaking lasted about 2 years which I am happy with.

# Battery drain
There are three operations that will affect the battery life: 
* Continual LCD display and monitoring of the room temperature 
* BLE communication
* Motor drive to open/close the valve  
Temperature monitoring and LCD display will always be active so cannot be optimised. The drain will be minimal.
The largest drain will come from driving the motor so ideally this should be minimised. BLE will not use much power but regular communication will have an effect on battery life.

# Extending battery life
Regular polling of a valve via BLE will affect battery life. Consideration should be made on how often to poll a valve for its current status to avoid excessive BLE communication.
When a radiator is not expected to produce heat for an extended period (i.e. a room will be vacant for a few hours/days) and the ambient temperature is averaging a 'normal' set temperature it may be worthwhile setting the required temperature to a low value. This will prevent the valve from continually opening and closing as the ambient temperature changes but no heat is being supplied. 
If separate temperature monitors are used for zones to control 'call for heat' it may be worthwhile using set temperatures higher than that required to heat a radiator. This should cause the radiator to produce maximum heat until the room is at the required temperature. The required temperature can then be reduced to a value below that required. The result of this is that the valve will drive completely open and completely closed (driven by the heating automation) and thus not use the variable control implemented in the valve software.