# MoS2

![Views of MoS2](https://github.com/JaredKeithAveritt/Pymol_examples/blob/main/MoS2/1L_MoS2.png)  
- [Link to all HIRES files on Google Drive](https://drive.google.com/drive/folders/16f-_w0aw-7A-QUvQAzcg7gblBw0ajrcG?usp=sharing)  



# Mg(OH)2

## Links and Resources

![Views of Mg(OH)2](https://github.com/JaredKeithAveritt/Pymol_examples/blob/main/Mg(OH)2/2L_Mg(OH)2.png)  

- [Link to all HIRES files on Google Drive](https://drive.google.com/drive/folders/16f-_w0aw-7A-QUvQAzcg7gblBw0ajrcG?usp=sharing)
  
# Graphene

![Views of Graphene](https://github.com/JaredKeithAveritt/Pymol_examples/blob/main/graphene/Graphene.png)  
- [Link to all HIRES files on Google Drive](https://drive.google.com/drive/folders/16f-_w0aw-7A-QUvQAzcg7gblBw0ajrcG?usp=sharing)  

## Structures from Materials Project that was used 

- [Mg(OH)2 Structure from the Materials Project](https://next-gen.materialsproject.org/materials/mp-626143?formula=MgO2H2)  
- [MoS2 Structure from the Materials Project](https://next-gen.materialsproject.org/materials/mp-2815?formula=MoS2)  






Using Pymol:
```
cmd.color("red",  "index 157")
cmd.color("red",  "index 158")
cmd.color("blue",  "index 141")
unbond index 168 , index 169
unbond index 167, index 168
unbond index 167, index 169


#Remove double bonds
set valence, 0

#GA
unbond index 142 , index 143
unbond index 141, index 142
unbond index 141, index 143
cmd.color("blue",  "index 144")

#GPNO
cmd.color("blue",  "index 157")
cmd.color("red",  "index 158")
cmd.color("white",  "index 167")
unbond index 167 , index 147

color white, all
set orthoscopic, on
set sphere_scale, 0.2
show sticks
show spheres
set stick_radius, 0.11
color white, element H
color orange, element Mg
color gray, element C
color green, element N
color red, element O
run /Users/jarkeith/Desktop/make_Mg_O_bonds.py
set sphere_scale, 0.21, (element O)

set sphere_scale, 0.19, (element H)
set sphere_scale, 0.35, (element Mg)
set sphere_scale, 0.21, (element S)
set sphere_scale, 0.35, (element Mo)
```

## Code to fix bonds between Mg and O

```python
# filename: make_Mg_O_bonds.py
from pymol import cmd

# Adjust the cutoff distance as necessary
cutoff_distance = 2.5  # in Angstroms

# Replace 'POSCAR' with the actual name of the molecule object in PyMOL
object_name = "POSCAR"  # Adjust this line with your object's name

# Fetch the indices of Mg and O atoms
mg_indices = cmd.identify(f"{object_name} and elem Mg")
o_indices = cmd.identify(f"{object_name} and elem O")

for mg_idx in mg_indices:
    for o_idx in o_indices:
        mg_sel = f"id {mg_idx}"
        o_sel = f"id {o_idx}"
        
        # Calculate distance between Mg and O atom
        distance = cmd.get_distance(mg_sel, o_sel)
        
        # If distance is less than cutoff, create a bond
        if distance <= cutoff_distance:
            cmd.bond(mg_sel, o_sel)
            cmd.rebuild()  # Update the display

cmd.sort()
cmd.rebuild()
```

## Code to color atoms based on their index

```python
from pymol import cmd, stored

def color_atoms_by_index():
    stored.indices = []
    # Get all atom indices in the object "POSCAR"
    cmd.iterate("POSCAR", "stored.indices.append(index)")
    
    # Loop through the indices and apply conditions
    for index in stored.indices:
        if index <= 140:
            cmd.hide("spheres", f"index {index}")
            cmd.color("gray10", f"index {index}")
            cmd.set("stick_radius", 0.5, f"index {index}")
        else:
            cmd.show("spheres", f"index {index}")

# Call the function
color_atoms_by_index()

```

## Orientations and save files as high res:

```

draw 12800,12800
set ray_opaque_background, 0
set ray_shadows,off  
ray 12800, 12800
png ~/Desktop/GPNO-side.png



png ~/Desktop/Mg_O2H2_top.png



orient
rotate z,90
rotate x,90.8
draw 12800,12800
set ray_opaque_background, 0
set ray_shadows,off  
ray 12800, 12800
png ~/Desktop/Mg_O2H2_side1.png
```


