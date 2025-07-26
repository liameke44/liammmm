import cadquery as cq

# Afmetingen
lego_unit = 8.0
brick_height = 9.6
plate_height = 3.2
total_mount_height = brick_height + 2 * plate_height # 16 mm
stud_diameter = 4.8
stud_height = 1.8
base_length = 40.0 # 5 studs
base_width = 32.0 # 4 studs
base_height = plate_height # 1 plate
motor_diameter = 36.0
motor_radius = motor_diameter / 2
motor_length = 50.0
mount_thickness = 5.0
screw_spacing = 12.0 # vanaf midden, dus 24 mm totaal
screw_diameter = 3.2

# Basisplaat
base = cq.Workplane("XY").box(base_length, base_width, base_height)

# Studs toevoegen
for x in range(5):
    for y in range(4):
        base = base.faces(">Z").workplane().center(
            -base_length / 2 + (x + 0.5) * lego_unit,
            -base_width / 2 + (y + 0.5) * lego_unit
        ).circle(stud_diameter / 2).extrude(stud_height)

# Beugelblok erbovenop
mount = (
    cq.Workplane("XY")
    .workplane(offset=base_height)
    .center(0, 0)
    .rect(motor_length, mount_thickness)
    .extrude(total_mount_height)
)

# Motoruitsparing (gat)
cutout = (
    cq.Workplan
