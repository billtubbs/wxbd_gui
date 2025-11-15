# wxbd_gui - Block Diagram GUI for Control Systems

A wxPython-based graphical user interface for creating and editing control system block diagrams 
in Python which can then be used to generate source code for implementation on a micro-controller.

**Author**: Ryan Krauss (ryanGT)
**Version**: 1.1.6
**License**: MIT
**GitHub**: https://github.com/ryanGT/wxbd_gui

## Overview

wxbd_gui provides a visual interface for:
- Creating block diagram models for control systems (open-loop and closed-loop)
- Designing control systems for embedded platforms (Arduino and Raspberry Pi)
- Generating embedded C/C++ code from block diagram models
- Managing blocks, actuators, sensors, and their interconnections
- Visualizing block diagrams with matplotlib

## Installation

### Dependencies

```bash
pip install wxPython py_block_diagram krauss_misc numpy matplotlib
```

### Install wxbd_gui

From the package directory:
```bash
pip install -e .
```

Or install as part of a project's requirements:
```bash
# In requirements.txt
-e /path/to/wxbd_gui
```

## Quick Start

### Starting the GUI

Create a Python script with the following code:

```python
import wx
import wxbd_gui

# Create wxPython application
app = wx.App()

# Create the main window
window = wxbd_gui.Window("Block Diagram GUI")

# Start the event loop
app.MainLoop()
```

Run the script:
```bash
python your_launcher_script.py
```

## Creating a Block Diagram

### Basic Workflow

1. **Add Blocks** (Block Menu → Add Block)
   - Select block category (plant, controller, input, etc.)
   - Choose specific block type from the category
   - Enter a block name (auto-suggested)
   - Set block parameters (up to 6 parameters supported)
   - For plant blocks: optionally add actuators and/or sensors
   - Block is automatically placed using intelligent placement algorithm

2. **Set Block Connections** (Block Menu → Set Input(s))
   - Select which block will receive input
   - Choose which block(s) provide the input(s)
   - Supports blocks with 1-3 inputs
   - Creates visual wire connections automatically

3. **Position Blocks** (Block Menu → Edit Placement)
   - **Relative Mode**: Position relative to another block
     - Options: right, left, above, below
   - **Absolute Mode**: Set exact (x, y) coordinates

4. **Edit Block Parameters** (Block Menu → Edit Parameters)
   - Modify block parameters after creation
   - Change actuator/sensor for plant blocks
   - Rename blocks

5. **Save/Load Models** (File Menu)
   - Save diagram to CSV: `File → Save to CSV` (Ctrl+S)
   - Load diagram: `File → Load from CSV` (Ctrl+L)
   - CSV format is human-readable and version-control friendly

### Example: Open-Loop RC Circuit

Typical workflow for creating an open-loop RC circuit:

1. Create step input → auto-placed
2. Create PWM actuator
3. Create ADC sensor
4. Create plant block → auto-placed
5. Set the input for the plant
6. Visual wires are drawn automatically
7. Generate embedded code for your target platform

### Example: Closed-Loop Control System

For a closed-loop system, additionally:

1. Add summing junction
2. Place the summing junction
3. Adjust block placements as needed
4. Set both inputs for the summing junction
5. Draw wires (automatic)
6. Generate code

## Code Generation

wxbd_gui can generate embedded C/C++ code for Arduino and Raspberry Pi platforms.

### Arduino Code Generation

1. **Set template file**: `Code Generation → Arduino → Set Arduino Template File`
2. **Set output folder**: `Code Generation → Arduino → Set Arduino Output Folder`
3. **Generate code**: `Code Generation → Arduino → Generate Arduino Code`

### Raspberry Pi Code Generation

1. **Set template file**: `Code Generation → Raspberry Pi → Set Raspberry Pi Template File`
2. **Set output path**: `Code Generation → Raspberry Pi → Set Raspberry Pi Output Path`
3. **Generate code**: `Code Generation → Raspberry Pi → Generate Raspberry Pi Code`

## Key Features

- **Automatic Block Placement**: Intelligent algorithm positions blocks logically
- **Colorful Wire Connections**: Visual representation of signal flow
- **Persistent Configuration**: GUI settings saved between sessions in `gui_params_pybd.txt`
- **CSV Model Format**: Human-readable block diagram storage
- **Keyboard Shortcuts**:
  - `Ctrl+S` - Save diagram
  - `Ctrl+L` - Load diagram
- **Menu Parameters**: Configure which parameters appear in generated menu code
- **Print Blocks**: Select which blocks output to serial monitor in embedded code

## Menu System

### File Menu
- Save to CSV (Ctrl+S)
- Load from CSV (Ctrl+L)
- Exit

### Block Menu
- Add Block
- Replace Block
- Delete Block
- Edit Parameters
- Set Input(s)
- Edit Placement
- Set Menu Parameters
- Set Print Blocks

### Code Generation Menu
- **Arduino**
  - Set Template File
  - Set Output Folder
  - Generate Code
- **Python**
  - Set Template File
  - Set Output Path
  - Generate Code
- **Raspberry Pi**
  - Set Template File
  - Set Output Path
  - Generate Code

## Architecture

### Main Components

- **`wxbd_gui.Window`**: Main application window (wx.Frame)
  - Contains matplotlib plot panel for visualization
  - Block list display
  - Menu system
  - Manages block diagram instance

- **`PlotPanel`**: Matplotlib visualization panel
  - Displays block diagram graphically
  - Embedded matplotlib canvas in wxPython

### Dialog Windows

- **`AddBlockDialog`**: Add new blocks to diagram
- **`ReplaceBlockDialog`**: Replace existing block with different type
- **`EditBlockDialog`**: Edit block parameters and name
- **`SetInputsDialog`**: Configure block input connections
- **`PlacementDialog`**: Position blocks (absolute or relative)
- **`MenuParamsDialog`**: Configure menu parameters for code generation
- **`PrintBlocksDialog`**: Select which blocks print to serial monitor
- **`AddActuatorDialog`**: Add actuator to plant blocks
- **`AddSensorDialog`**: Add sensor to plant blocks

### Backend

wxbd_gui wraps the `py_block_diagram` library which handles:
- Block diagram logic and data structures
- Block types and categories
- Connection management
- Code generation templates

## Block Categories

Available block categories (from `py_block_diagram`):
- **Plant blocks** (with/without actuators, with 1-2 sensors)
- **Controllers** (PID, lead-lag, etc.)
- **Inputs** (step, ramp, sine, etc.)
- **Summing junctions**
- **Transfer functions**
- **Other control blocks**

## Configuration Files

- **`gui_params_pybd.txt`**: Stores GUI configuration
  - Template file paths (Arduino, Python, Raspberry Pi)
  - Output paths for generated code
  - Last used CSV model path
  - Auto-loads on startup, auto-saves on changes

## Troubleshooting

### GUI doesn't start
- Ensure wxPython is installed: `pip install wxPython`
- Check that all dependencies are installed
- Verify Python version compatibility (wxPython requires Python 3.6+)

### Block diagram doesn't display
- Check matplotlib backend compatibility with wxPython
- Ensure numpy is installed

### Code generation fails
- Verify template file paths are set correctly
- Check output directory permissions
- Ensure `py_block_diagram` is installed

## Development

### Project Structure

```
wxbd_gui/
├── __init__.py                          # Main GUI window (923 lines)
├── wx_add_block_dialog.py               # Add block dialog
├── wx_add_actuator_or_sensor_dialog.py  # Actuator/sensor dialogs
├── wx_edit_block_dialog.py              # Edit parameters dialog
├── wx_set_inputs_dialog.py              # Set inputs dialog
├── wx_placement_dialog.py               # Placement dialog
├── wx_menu_params_dialog.py             # Menu parameters dialog
├── wx_print_blocks_dialog.py            # Print blocks dialog
└── wxbd_utils.py                        # Utility classes
```

## License

MIT License - See LICENSE file for details

## Links

- GitHub Repository: https://github.com/ryanGT/wxbd_gui
- py_block_diagram: Core backend library
- wxPython Documentation: https://wxpython.org/

## Support

For issues, questions, or contributions, please visit the GitHub repository.
