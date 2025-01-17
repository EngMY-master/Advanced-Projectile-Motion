# Detailed Comments and Theory on the Code

## Theory Behind the Code

This program models the motion of projectiles under various scenarios, such as landing on flat ground or an inclined plane. It uses symbolic computations to solve equations of motion and determine unknown variables based on user inputs.

### 1. **Equations of Motion**
   - The horizontal displacement is given by:
     ```
     R = s0 + v0 * cos(theta) * t
     ```
   - The vertical displacement is given by:
     ```
     y = y0 + v0 * sin(theta) * t - 0.5 * g * t^2
     ```
   - For inclined plane landings:
     ```
     y = x * tan(alpha) + y0
     ```
   - Maximum height is calculated as:
     ```
     h = (v0 * sin(theta))^2 / (2 * g)
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
   - **Flat Ground**: If `y0 = y` and no incline (`alpha`), the equations simplify.
   - **Inclined Plane**: If `alpha` is provided, an additional equation models the incline.

---

## Example Input and Output

### Example Input:
```plaintext
Enter the known variables. Leave blank if unknown.
Available variables: s0 (m), y0 (m), v0 (m/s), theta (degrees),
R (m), y (m), h (m), t (s), g (m/s²), alpha (degrees)
Note: 'alpha' is the incline angle. Leave blank for flat ground.

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

### Example Output:
```plaintext
Calculated Unknowns:
R: 40.77 m
h: 10.20 m
t: 2.04 s
```

---

## Detailed Comments on the Code

### Defining Symbols:
```python
s0, y0, v0, theta, R, y, h, t, g, alpha = sp.symbols('s0 y0 v0 theta R y h t g alpha')
```
Defines symbolic variables for use in equations of motion.

---

### Degree to Radian Conversion:
```python
def deg_to_rad(angle_deg: float) -> float:
    return angle_deg * math.pi / 180
```
Converts angles from degrees to radians for use in trigonometric calculations.

---

### Defining Projectile Equations:
```python
def get_equations(include_alpha: bool, flat_ground: bool):
```
Returns equations for different scenarios:
1. **Flat Ground**:
   ```
   R = s0 + v0 * cos(theta) * t
   y = y0 + v0 * sin(theta) * t - 0.5 * g * t^2
   h = (v0 * sin(theta))^2 / (2 * g)
   ```
2. **Inclined Plane**:
   ```
   y = x * tan(alpha) + y0
   ```

---

### Solving the System:
```python
def solve_projectile(knowns: dict, include_alpha: bool):
```
1. **Substitutes Known Values**:
   Substitutes user-provided inputs into the symbolic equations.
2. **Identifies Unknown Variables**:
   Determines the variables to solve for based on inputs.
3. **Filters Physical Solutions**:
   Ensures solutions are physically valid (e.g., time must be positive).
4. **Handles Special Cases**:
   Adjusts equations for flat ground or inclined planes.

---

### User Input and Main Execution:
```python
def main():
```
1. **Prompts User for Inputs**:
   Requests known variables and identifies the scenario (flat ground or incline).
2. **Solves for Unknowns**:
   Calls `solve_projectile` with the inputs.
3. **Displays Results**:
   Outputs calculated values in a user-friendly format.

---

## Additional Notes
- The program handles edge cases, such as missing gravity (`g`) or unphysical solutions.
- Solutions are filtered to ensure they are meaningful (e.g., positive time values).
- Maximum height (`h`) is computed separately for flat ground scenarios.
