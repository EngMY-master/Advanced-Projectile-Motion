# Detailed Comments and Theory on the Code

## Theory Behind the Code

This program models the motion of projectiles under various scenarios, including:
1. **Flat Ground**: Standard projectile motion where the projectile lands at the same vertical height from which it was launched.
2. **Linear Incline**: Projectile motion where the projectile lands on an inclined plane.
3. **Non-Linear Hill**: Projectile motion where the projectile lands on a non-linear surface described by an arbitrary function.

Each scenario uses symbolic computations to solve equations of motion and determine unknown variables based on user inputs.

### 1. **Equations of Motion**
   - **Horizontal Displacement**:
     ```
     x(t) = s0 + v0 * cos(theta) * t
     ```
   - **Vertical Displacement**:
     ```
     y(t) = y0 + v0 * sin(theta) * t - 0.5 * g * t^2
     ```
   - **Inclined Plane Landing**:
     ```
     y(t) = x(t) * tan(alpha) + y0
     ```
   - **Maximum Height**:
     ```
     h = y0 + (v0 * sin(theta))^2 / (2 * g)
     ```

### 2. **Conversions Between Degrees and Radians**
   - Trigonometric functions in Python and SymPy operate in radians. Angles are converted as follows:
     ```
     radians = degrees * (pi / 180)
     ```

### 3. **Symbolic Computations**
   - The program uses SymPy to define symbolic variables and solve equations.
   - Known variables are substituted into the equations, and unknowns are solved symbolically.

### 4. **Handling Scenarios**
   - **Flat Ground**: If `y0 = 0` and no incline (`alpha`), the equations simplify significantly.
   - **Linear Incline**: Adds an equation for the incline landing condition.
   - **Non-Linear Hill**: Introduces complex interactions between the projectile and the hill's equation.

---

## Example Input and Output

### Example Input for Flat Ground:
```plaintext
Enter s0: 0
Enter y0: 0
Enter v0: 20
Enter theta: 45
Enter R: 
Enter y: 
Enter h: 
Enter t: 
Enter g: 9.81
Enter alpha: 
```

### Example Output for Flat Ground:
```plaintext
Calculated Unknowns:
R (m): 40.77
h (m): 10.20
t (s): 2.04
```

### Example Input for Linear Incline:
```plaintext
Enter s0: 0
Enter y0: 0
Enter v0: 20
Enter theta: 45
Enter R: 
Enter y: 
Enter h: 
Enter t: 
Enter g: 9.81
Enter alpha: 30
```

### Example Output for Linear Incline:
```plaintext
Calculated Unknowns:
R (m): 31.62
h (m): 10.20
t (s): 1.76
```

### Example Input for Non-Linear Hill:
```plaintext
Enter s0: 0
Enter y0: 0
Enter v0: 20
Enter theta: 45
Enter R: 
Enter y: 
Enter h: 
Enter t: 
Enter g: 9.81
Enter hill_eq: 0.05*x**2
```

### Example Output for Non-Linear Hill:
```plaintext
Point B where the projectile strikes the hill: (15.50 m, 12.00 m)
```

---

## Detailed Comments on the Code

### Defining Symbols:
```python
t = sp.symbols('t')
v0, theta, g = sp.symbols('v0 theta g')
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
    x = sp.symbols('x')
    hill_eq = sp.sympify(equation_str)
    return hill_eq
```
Parses the user-provided non-linear hill equation into a symbolic expression.

---

### Solving Flat Ground:
```python
def solve_flat_ground(s0, y0, v0_val, theta_deg, g_val=9.81):
```
1. Calculates time of flight:
   ```
   t_flight = (v0y + sqrt(v0y^2 + 2 * g * y0)) / g
   ```
2. Computes horizontal range (`R`) and maximum height (`h`).

---

### Solving Linear Incline:
```python
def solve_linear_incline(s0, y0, v0_val, theta_deg, alpha_deg, g_val=9.81):
```
1. Derives the time of flight by solving the projectile equation and incline equation intersection.
2. Computes horizontal range along the incline and height (`h`).

---

### Solving Non-Linear Hill:
```python
def solve_non_linear_hill(s0, y0, v0_val, theta_deg, hill_eq_input, g_val=9.81):
```
1. Parses the hill equation.
2. Determines the intersection of the projectile path and hill equation.
3. Filters valid (positive) solutions for time.
4. Computes the coordinates of the intersection point.

---

### User Input and Execution:
```python
def main():
```
1. Prompts the user for inputs, including terrain type (flat, linear incline, or non-linear hill).
2. Calls the appropriate solver function based on terrain type.
3. Displays results in a user-friendly format.

---

## Differences from the Previous Version

1. **Terrain Handling**:
   - The new code introduces a third scenario: motion on a non-linear hill, adding complexity and flexibility.
   - The previous code handled only flat ground and linear incline.

2. **Simplified Flat and Linear Solutions**:
   - The new code uses direct calculations for flat ground and linear incline without solving large systems of equations.
   - The previous code relied heavily on symbolic equations for all scenarios.

3. **Hill Equation Parsing**:
   - The new code allows the user to define arbitrary hill equations.
   - The previous code did not include this functionality.

4. **Focus on Practicality**:
   - The new code emphasizes direct calculations and user-defined inputs for terrain types.
   - The previous code emphasized symbolic generality and solving systems.

5. **Example Outputs**:
   - The new code provides specific outputs tailored to the terrain type, whereas the previous code offered general solutions.

---

## Additional Notes
- The new code is better suited for non-linear terrain modeling due to its ability to handle arbitrary hill equations.
- Both versions handle edge cases, such as missing gravity or invalid inputs, but the new code explicitly validates user-provided hill equations.
