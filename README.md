# Backstepping Controller

A Python package for simulating control systems using backstepping control laws. This package provides a flexible and easy-to-use interface for designing, simulating, and analyzing control systems with options for trajectory tracking, plotting, and saving results.

## Installation

To install the package, simply run:

```bash
pip install backstepping_controller
```

## Usage

1. Importing the Package
To use the package, import the necessary functions:

```bash
from backstepping_controller import generic_backstepping_controller, simulate_system, plot_responses, save_responses
```

2. Defining the System
First, you need to define your system's state equations and parameters. For example, if you have a 3D system:

```bash
import sympy as sp
```

# Define system parameters
```bash
num_states = 3

x1, x2, x3 = sp.symbols('x1 x2 x3')
u = sp.Symbol('u')
a, b, c = sp.symbols('a b c')

# Define the state equations for a 3D system
state_equations = [
    a * x1 + x2,
    b * x2 + x3,
    c * x3 + u
]

# Define the gains
gains_vals = [1.0, 2.0, 3.0]
```

3. Creating the Control Law
Use the `generic_backstepping_controller` function to create the control law for your system:

```bash 
final_control_law, states, gains = generic_backstepping_controller(num_states, state_equations, 'u', gains_vals, tracking=False)
```

If you want to include trajectory tracking, you can enable the tracking parameter:

```bash 
final_control_law, states, gains, refs, errors = generic_backstepping_controller(num_states, state_equations, 'u', gains_vals, tracking=True)
```

4. Simulating the System
Simulate the system using the `simulate_system` function. You can configure it to print the control law, plot the results, and save the responses:

```bash 
# Simulation parameters
time = np.linspace(0, 10, 1000)  # 10 seconds of simulation with 1000 time steps
initial_conditions = [0.1, 0.0, 0.1]  # Initial conditions for x1, x2, x3
params_subs = {a: 1.0, b: 0.5, c: 0.2, 'k1': gains_vals[0], 'k2': gains_vals[1], 'k3': gains_vals[2]}

# Simulate the system
state_values, control_inputs = simulate_system(
    final_control_law, states, gains_vals, initial_conditions, time, state_equations, params_subs, 
    plot=True, save_path='results.json', print_law=True)
```

5. Plotting the Results
If you haven't plotted the results directly in the simulation, you can use the `plot_responses` function to plot the states and control inputs on separate plots:

```bash 
plot_responses(time, state_values, control_inputs)
```

6. Saving the Results
The results can be saved to a JSON file for later analysis. The simulate_system function allows you to save the results directly during the simulation by specifying the save_path:

```bash
simulate_system(final_control_law, states, gains_vals, initial_conditions, time, state_equations, params_subs, save_path='results.json')
```

Alternatively, you can save the results after the simulation using the `save_responses` function:

```bash
save_responses('results.json', time, state_values, control_inputs)
```

Example: Full Workflow
Here's a complete example that ties everything together:

```bash

import numpy as np
import sympy as sp
from backstepping_controller import generic_backstepping_controller, simulate_system, plot_responses, save_responses

# Define system parameters
num_states = 3
x1, x2, x3 = sp.symbols('x1 x2 x3')
u = sp.Symbol('u')
a, b, c = sp.symbols('a b c')

# Define the state equations for a 3D system
state_equations = [
    a * x1 + x2,
    b * x2 + x3,
    c * x3 + u
]

# Define the gains
gains_vals = [1.0, 2.0, 3.0]

# Create the control law
final_control_law, states, gains = generic_backstepping_controller(num_states, state_equations, 'u', gains_vals, tracking=False)

# Simulation parameters
time = np.linspace(0, 10, 1000)  # 10 seconds of simulation with 1000 time steps
initial_conditions = [0.1, 0.0, 0.1]  # Initial conditions for x1, x2, x3
params_subs = {a: 1.0, b: 0.5, c: 0.2, 'k1': gains_vals[0], 'k2': gains_vals[1], 'k3': gains_vals[2]}

# Simulate the system
state_values, control_inputs = simulate_system(
    final_control_law, states, gains_vals, initial_conditions, time, state_equations, params_subs, 
    plot=True, save_path='results.json', print_law=True
)

# Optional: Plot and save the results separately
plot_responses(time, state_values, control_inputs)
save_responses('results.json', time, state_values, control_inputs)
```

