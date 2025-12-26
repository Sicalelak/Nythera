# Nythera

A UI library developed by some lazy people.

## Installation

Add this to the top of your script:

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Sicalelak/Sicalelak/refs/heads/main/NytheraRevampUI"))()
```

## Quick Start

```lua
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/Sicalelak/Sicalelak/refs/heads/main/NytheraRevampUI"))()

-- This is optional: Customize before creating window
Library.Config.AccentColor = Color3.fromRGB(70, 130, 240)
Library.Config.LogoImage = "rbxassetid://YOUR_LOGO_ID"
Library.Config.MobileButtonImage = "rbxassetid://YOUR_ICON_ID"

-- Create the window
local Window = Library:CreateWindow()

-- Create a tab
local MainTab = Window:CreateTab("Main", "rbxassetid://7072706620")

-- Create a section
local Section = MainTab:AddSection("Settings", "left") -- or right

-- Add components
local MyToggle = Section:AddToggle({
    Name = "Enable Feature",
    Default = false,
    Flag = "my_toggle",
    Callback = function(value)
        -- Your code here
    end
})
```

## Configuration Options

You can customize the library before creating the window:

```lua
Library.Config.AccentColor = Color3.fromRGB(70, 130, 240)  -- Theme color
Library.Config.LogoImage = "rbxassetid://YOUR_ID"          -- Top bar logo (the one on the top left I mean)
Library.Config.MobileButtonImage = "rbxassetid://YOUR_ID"  -- Mobile toggle button icon
```

## Creating Tabs

```lua
local Tab = Window:CreateTab("Tab Name", "rbxassetid://ICON_ID")
```

The icon parameter is optional. If not provided, a default icon will be used. (IDK if the default one works never tested it)

## Creating Sections

Sections organize your components. You can place them on the left or right side of the tab.

```lua
local LeftSection = Tab:AddSection("Left Section", "left")
local RightSection = Tab:AddSection("Right Section", "right")
```

## Components

### Toggle

A simple on/off switch.

```lua
local Toggle = Section:AddToggle({
    Name = "Toggle Name",
    Default = false,
    Flag = "unique_flag_name",
    Callback = function(value)
        print("Toggle is now:", value)
    end
})

-- Control it
Toggle:Set(true)
local value = Toggle:GetValue()
```

### Slider

A draggable slider.

```lua
local Slider = Section:AddSlider({
    Name = "Speed",
    Min = 0,
    Max = 100,
    Default = 50,
    Suffix = "%",
    Flag = "speed_slider",
    Callback = function(value)
        print("Slider value:", value)
    end
})

-- Control it
Slider:Set(75)
local value = Slider:GetValue()
```

### Button

A normal clickable button.

```lua
Section:AddButton({
    Name = "Click Me",
    Callback = function()
        print("Button clicked!")
    end
})
```

### Rectangular Toggle Button

A toggle button with a rectangular shape.

```lua
local RectToggle = Section:AddRectButton({
    Name = "Rect Toggle",
    Default = false,
    Width = 120,
    Height = 35,
    Flag = "rect_toggle",
    Callback = function(state)
        print("State:", state)
    end
})
```

### Dropdown (Single Select)

A dropdown menu where the user can select one option.

```lua
local Dropdown = Section:AddDropdown({
    Name = "Select Option",
    Items = {"Option 1", "Option 2", "Option 3"},
    DefaultSelections = {"Option 1"},
    MultiSelect = false,
    Flag = "single_dropdown",
    Callback = function(selected)
        print("Selected:", selected)
    end
})

-- Control it
Dropdown:Set("Option 2")
Dropdown:Refresh({"New 1", "New 2", "New 3"})  -- Update items
local value = Dropdown:GetValue()
```

### Dropdown (Multi Select)

A dropdown menu where the user can select multiple options.

```lua
local MultiDropdown = Section:AddDropdown({
    Name = "Select Features",
    Items = {"Feature A", "Feature B", "Feature C"},
    DefaultSelections = {"Feature A"},
    MultiSelect = true,
    MaxSelections = 3, -- This is to set how many options the user can select at once
    Flag = "multi_dropdown",
    Callback = function(selected)
        print("Selected:", table.concat(selected, ", "))
    end
})
```

### Text Input

A text input field.

```lua
local Input = Section:AddInputLetterAdd({
    Name = "Player Name",
    Default = "",
    PlaceholderText = "Enter name...",
    MaxLength = 20,
    Flag = "name_input",
    Callback = function(text)
        print("Input:", text)
    end
})

-- Control it
Input:SetText("NewValue")
local value = Input:GetValue()
```

### Combo Input

An input with text, number, and toggle combined. (IDK how to explain it better just try it out to see what I mean)

```lua
local Combo = Section:AddInput({
    Name = "Combo Input",
    Default = "Label",
    DefaultNumber = 5,
    MinValue = 1,
    MaxValue = 100,
    Flag = "combo_input",
    Callback = function(text, number, active)
        print(text, number, active)
    end
})
```

### Input Multi Dropdown

A dropdown that shows selections in an input field.

```lua
local InputDropdown = Section:AddInputMultiDropdown({
    Name = "Selection",
    Items = {"Item 1", "Item 2", "Item 3"},
    PlaceholderText = "Select items...",
    MaxSelections = 3,
    Flag = "input_dropdown",
    Callback = function(selected)
        print("Selected:", table.concat(selected, ", "))
    end
})

-- Control it
InputDropdown:Refresh({"New 1", "New 2"})
InputDropdown:Clear()
```

## Notifications

The library also has a notification system with three types.

```lua
Library:NotifySuccess("Operation completed!")
Library:NotifyWarning("Please be careful!")
Library:NotifyError("Something went wrong!")
```

Notifications automatically stack and dismiss after 3 seconds. Duplicate notifications will show a count instead of creating new ones. (like: Success! (3x))

## Configuration System

The library automatically creates a Config tab where users can save and load their settings. Configurations are saved per game using the game's PlaceId.

Some Features about the Config Tab:
- Automatic saving every 60 seconds when auto save is enabled
- Per game configuration isolation (If the player created a config in lets say Jailbreak the player wont be able to use it on Meepcity)
- Last used config is remembered and auto loaded on script execution

All components with a Flag parameter are automatically included in saves.

## Flags

Flags are unique identifiers for components. They are used for:
- Saving and loading configurations
- Giving Access to components

```lua
-- Access any component by flag
local toggleValue = Library.Flags["my_toggle"]:GetValue()
Library.Flags["my_slider"]:Set(50)
```

## Chained Dropdowns

You can create dropdowns that update based on another dropdown's selection.

```lua
local CategoryDropdown = Section:AddDropdown({
    Name = "Category",
    Items = {"Fruits", "Vegetables"},
    Callback = function(selected)
        local items = {}
        if selected == "Fruits" then
            items = {"Apple", "Banana", "Orange"}
        else
            items = {"Carrot", "Potato", "Tomato"}
        end
        ItemDropdown:Refresh(items)
    end
})

local ItemDropdown = Section:AddDropdown({
    Name = "Item",
    Items = {"Apple", "Banana", "Orange"}
})
```

## Mobile Support

The library includes a draggable mobile toggle button in the top left corner. This button allows users to show/hide the main UI window. The button color indicates the current state:
- Green: UI is visible
- Red: UI is hidden

Also every button in library had mobile support

## Credits

Nythera V3 UI Library was developed by the Nythera Team:
- troyney (Founder and Lead Developer)
- incw (Co-Developer)
- qababiii, gijsko, irenicos (Contributors)

## Notes

- The Config and Credits tabs are automatically created and cannot be removed
- Components without a Flag parameter will not be saved to configurations
- Flag names should be unique across your entire script (please dont be dumb and have two flag names as the same name)
- The library cleans up existing instances when CreateWindow is called (if the user executes the script again the already existing UI disappears)
