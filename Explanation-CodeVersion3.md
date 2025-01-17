# Detailed Comments and Theory on the Code

## Theory Behind the Code

This program models projectile motion under specific scenarios, including:

1. **Projectile on a Non-Linear Hill**: Solves for the point where the projectile intersects a user-defined hill described by a mathematical equation.
2. **Ball Kicked from Point A**: Solves for the initial and final speeds of a ball launched from the ground to a specified landing point.

Each scenario uses symbolic computations to solve equations of motion and determine unknown variables based on user inputs.

### 1. **Equations of Motion**
   - **Horizontal Displacement**:
     ```
     x(t) = v0 * cos(theta) * t
     ```
   - **Vertical Displacement**:
     ```
     y(t) = v0 * sin(theta) * t - 0.5 * g * t^2
     ```
   - **Intersection with Hill**:
     ```
     y(t) = hill_eq(x(t))
     ```
   - **Landing Point Equations**:
     ```
     x_landing = v0 * cos(theta) * t
     y_landing = v0 * sin(theta) * t - 0.5 * g * t^2
     ```

### 2. **Conversions Between Degrees and Radians**
   - Trigonometric functions in Python and SymPy operate in radians. Angles are converted as follows:
     ```
     radians = degrees * (pi / 180)
     ```

### 3. **Symbolic Computations**
   - The program uses SymPy to define symbolic variables and solve equations.
   - Known variables are substituted into the equations, and unknowns are solved symbolically.

---

## Example Input and Output

### Example Input for Non-Linear Hill:
```plaintext
Enter initial velocity (v0) in ft/s: 150
Enter launch angle (theta) in degrees: 45
Enter the hill equation in terms of x (e.g., 0.05*x**2): 0.05*x**2
Enter acceleration due to gravity (g) in ft/s² [Press Enter for default 32.174]:
```

### Example Output for Non-Linear Hill:
```plaintext
Point B where the projectile strikes the hill: (250.00 ft, 3125.00 ft)
```

### Example Input for Ball Kicked from Point A:
```plaintext
Enter landing x-coordinate (x) in ft: 500
Enter landing y-coordinate (y) in ft: 100
Enter launch angle (theta) in degrees: 30
Enter acceleration due to gravity (g) in ft/s² [Press Enter for default 32.174]:
```

### Example Output for Ball Kicked from Point A:
```plaintext
Initial Speed (v0): 122.47 ft/s
Final Speed (vf): 126.99 ft/s
```

---

## Detailed Comments on the Code

### Defining Symbols:
```python
t = sp.Symbol('t', real=True)
v0_sym, theta_sym, g_sym = sp.symbols('v0 theta g', real=True)
```
Defines symbolic variables for time (`t`), initial velocity (`v0`), launch angle (`theta`), and gravity (`g`).

---

### Degree to Radian Conversion:
```python
def deg_to_rad(angle_deg: float) -> float:
    return angle_deg * math.pi / 180
```
Converts angles from degrees to radians for use in trigonometric calculations.

---

### Parsing Non-Linear Hill Equation:
```python
def parse_hill_equation(equation_str: str):
    x = sp.Symbol('x', real=True)
    return sp.sympify(equation_str)
```
Parses the user-provided hill equation into a symbolic expression.

---

### Solving Non-Linear Hill:
```python
def solve_custom_hill_projectile(v0_val, theta_deg, hill_eq_input, g_val=32.174):
```
1. Parses the hill equation.
2. Sets up parametric equations for the projectile motion (`x(t)` and `y(t)`).
3. Solves for the time of intersection between the projectile path and the hill equation.
4. Filters valid (positive) solutions for time.
5. Computes the intersection coordinates (`x`, `y`).

---

### Solving Ball Kicked from Point A:
```python
def solve_ball_kicked_from_A(x_landing, y_landing, theta_deg, g_val=32.174):
```
1. Sets up parametric equations for the projectile motion.
2. Simultaneously solves for initial speed (`v0`) and time of flight (`t`).
3. Computes the final velocity based on horizontal and vertical components.

---

### User Input and Execution:
```python
def main():
```
1. Prompts the user to select a scenario (Non-Linear Hill or Ball Kicked from Point A).
2. Collects inputs specific to the chosen scenario.
3. Calls the appropriate solver function and displays results.

---

## Differences from Previous Versions

1. **Scenario Handling**:
   - This version focuses on two specific scenarios:
     - Projectile on a non-linear hill.
     - Ball kicked from point A.
   - The previous versions included additional scenarios, such as flat ground and linear incline.

2. **Physics Units**:
   - This version uses feet (ft) and ft/s² for inputs and computations, whereas the previous versions primarily used meters (m) and m/s².

3. **Hill Equation Parsing**:
   - Similar to the second version, this version allows for arbitrary hill equations but emphasizes practical computations.

4. **Simplified Outputs**:
   - Provides direct outputs for the intersection point or initial and final speeds, making the program user-friendly for specific use cases.

5. **Focus on Practicality**:
   - The program is streamlined for clear and precise results, suitable for educational or real-world applications involving specific scenarios.

---

## Additional Notes
- This version is better suited for solving targeted problems with minimal input complexity.
- It introduces a unique focus on the final velocity in the "Ball Kicked from Point A" scenario, which was not present in previous versions.
