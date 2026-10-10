A Roblox UI library for script menus — window, tabs, sections, toggles, sliders, dropdowns, colorpickers and a custom window background with adjustable transparency.

Author: egormaligin12-hue (Kyoko HUB) Configs folder: KyokoGUI_Configs

Installation
Option 1 — load from URL (recommended)
lua


local Kyoko = loadstring(game:HttpGet("https://raw.githubusercontent.com/egormaligin12-hue/Kyoko-hub/main/KyokoGUI.lua"))()

local Window = Kyoko:Window({
    Title = "Kyoko HUB",
    Subtitle = "by Kyoko",
    Keybind = Enum.KeyCode.RightControl,
})
Option 2 — paste the source
Copy everything from KyokoGUI.lua into your script and put your own menu code below it, in the same file. Kyoko is a local variable, so the library must come first.

Option 3 — local file in the executor workspace
lua


local Kyoko = loadstring(readfile("KyokoGUI.lua"))()
Quick start
lua


local Window = Kyoko:Window({
    Title = "Kyoko HUB",
    Subtitle = "by Kyoko",
    Size = UDim2.fromOffset(868, 650),
    DragStyle = 1,
    ShowUserInfo = true,
    Keybind = Enum.KeyCode.RightControl,
    AcrylicBlur = true,

    -- custom background
    Background = "rbxassetid://79471669118314",
    Transparency = 0.2,
})

local tabGroup = Window:TabGroup()

local tabs = {
    Main = tabGroup:Tab({ Name = "Main", Image = "rbxassetid://18821914323" }),
    Settings = tabGroup:Tab({ Name = "Settings", Image = "rbxassetid://10734950309" }),
}

local section = tabs.Main:Section({ Side = "Left" })

section:Header({ Text = "General" })

section:Button({
    Name = "Click me",
    Callback = function()
        Window:Notify({ Title = "Kyoko HUB", Description = "It works!", Lifetime = 4 })
    end,
})

section:Toggle({
    Name = "Toggle",
    Default = false,
    Callback = function(state) print(state) end,
}, "MyToggle")

Kyoko:SetFolder("KyokoGUI_Configs")
tabs.Settings:InsertConfigSection("Left")
Kyoko:LoadAutoLoadConfig()
Custom window background
Set it when creating the window:

lua


local Window = Kyoko:Window({
    Title = "Kyoko HUB",
    Background = "rbxassetid://79471669118314", -- Roblox asset id
    Transparency = 0.2,                         -- 0 = fully visible, 1 = invisible
})
Or as a table:

lua


Background = { Image = "rbxassetid://79471669118314", Transparency = 0.2, Mode = "Fill" }
Or at runtime, after the window has been created:

Method	What it does
Window:SetBackground(Image, Transparency)	set image and transparency at once
Window:SetBackgroundImage(Image)	change the image
Window:SetBackgroundTransparency(0.35)	change transparency only (0…1)
Window:SetBackgroundMode("Fill")	image mode: Fill, Fit, Crop, Stretch, Tile, Slice (default Crop)
Window:SetBackgroundState(bool)	show / hide the background
Window:GetBackground()	returns Image, Transparency, Visible
Window:GetBackgroundTransparency()	returns transparency
Window:GetBackgroundState()	returns visibility
Example — a transparency slider right inside the menu:

lua


section:Slider({
    Name = "Background transparency",
    Default = 20,
    Minimum = 0,
    Maximum = 100,
    DisplayMethod = "Percent",
    Precision = 0,
    Callback = function(value)
        Window:SetBackgroundTransparency(value / 100)
    end,
}, "BackgroundTransparency")
Picking an image
The default window is 868×650, so a 4:3 picture (1600×1200, 1920×1440) fits with no cropping at all. With any other ratio the image is cropped in the middle while Mode = "Crop" is used, so keep the important part centered.
Use PNG or JPG, ideally at least twice the window size (1736×1300) so it stays sharp on large monitors.
Only rbxassetid:// works — Roblox does not accept plain http links, and animated GIFs will not play.
About asset IDs. Both an Image ID and a Decal ID work — the engine resolves a decal to its image. A freshly uploaded asset stays in moderation first: while its status is InReview, the background will not show for anyone. To let other people use your image, open the asset in the Creator Dashboard and set it to Open Use; otherwise only the asset owner sees it. Note that a Decal cannot be set to Open Use while its parent Image is still Restricted — open the Image first.

Elements
Button, Toggle, Slider, Input, Keybind, Dropdown (with search and multi-select), Colorpicker (with alpha), Header, Label, SubLabel, Paragraph, Divider, Spacer.

Also available: Window:Notify, Window:Dialog, Window:GlobalSetting, Window:TabGroup, Tab:Section, Tab:Select, Window:Unload, Window.onUnloaded.

Configs
lua


Kyoko:SetFolder("KyokoGUI_Configs")        -- folder for config files
Kyoko:SaveConfig("my_config")              -- save
Kyoko:LoadConfig("my_config")              -- load
Kyoko:LoadAutoLoadConfig()                 -- auto-load the last used config
tabs.Settings:InsertConfigSection("Left")  -- ready-made config section inside the menu
Inside Roblox Studio the config system is unavailable (there are no filesystem functions) — the library works as a UI only.

Files
File	Description
KyokoGUI.lua	the library itself
KyokoGUI_Example.lua	a full example menu: tabs, elements, background controls, configs
Loading the example in an executor — the library has to be shared through getgenv(), otherwise a separate script cannot see it:

lua


local Kyoko = loadstring(game:HttpGet("https://raw.githubusercontent.com/egormaligin12-hue/Kyoko-hub/main/KyokoGUI.lua"))()
getgenv().Kyoko = Kyoko

loadstring(game:HttpGet("https://raw.githubusercontent.com/egormaligin12-hue/Kyoko-hub/main/KyokoGUI_Example.lua"))()
raw.githubusercontent.com is cached by GitHub, so right after an update the old version may still be served for a minute or two.

Requirements
An executor that provides writefile / readfile / isfile / isfolder / makefolder / listfiles (needed for configs), plus cloneref / gethui for proper GUI placement. Without them the library still runs, but configs are disabled.

Credits
Based on MacLib — the whole base UI structure (window, tabs, sections, elements, config system, acrylic blur) comes from it. Here it was renamed to Kyoko GUI and extended with the custom window background feature described above.
