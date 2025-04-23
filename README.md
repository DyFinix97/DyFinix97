local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Custom theme (opsional)
local CustomTheme = {
    TextColor = Color3.fromRGB(240, 240, 240),
    Background = Color3.fromRGB(25, 25, 25),
    Topbar = Color3.fromRGB(34, 34, 34),
    Shadow = Color3.fromRGB(20, 20, 20),
    NotificationBackground = Color3.fromRGB(20, 20, 20),
    NotificationActionsBackground = Color3.fromRGB(230, 230, 230),
    TabBackground = Color3.fromRGB(80, 80, 80),
    TabStroke = Color3.fromRGB(85, 85, 85),
    TabBackgroundSelected = Color3.fromRGB(210, 210, 210),
    TabTextColor = Color3.fromRGB(240, 240, 240),
    SelectedTabTextColor = Color3.fromRGB(50, 50, 50),
    ElementBackground = Color3.fromRGB(35, 35, 35),
    ElementBackgroundHover = Color3.fromRGB(40, 40, 40),
    SecondaryElementBackground = Color3.fromRGB(25, 25, 25),
    ElementStroke = Color3.fromRGB(50, 50, 50),
    SecondaryElementStroke = Color3.fromRGB(40, 40, 40),
    SliderBackground = Color3.fromRGB(50, 138, 220),
    SliderProgress = Color3.fromRGB(50, 138, 220),
    SliderStroke = Color3.fromRGB(58, 163, 255),
    ToggleBackground = Color3.fromRGB(30, 30, 30),
    ToggleEnabled = Color3.fromRGB(0, 146, 214),
    ToggleDisabled = Color3.fromRGB(100, 100, 100),
    ToggleEnabledStroke = Color3.fromRGB(0, 170, 255),
    ToggleDisabledStroke = Color3.fromRGB(125, 125, 125),
    ToggleEnabledOuterStroke = Color3.fromRGB(100, 100, 100),
    ToggleDisabledOuterStroke = Color3.fromRGB(65, 65, 65),
    DropdownSelected = Color3.fromRGB(40, 40, 40),
    DropdownUnselected = Color3.fromRGB(30, 30, 30),
    InputBackground = Color3.fromRGB(30, 30, 30),
    InputStroke = Color3.fromRGB(65, 65, 65),
    PlaceholderColor = Color3.fromRGB(178, 178, 178)
}

local Window = Rayfield:CreateWindow({
    Name = "Rayfield Example Window",
    Icon = 0,
    LoadingTitle = "Rayfield Interface Suite",
    LoadingSubtitle = "by Sirius",
    Theme = CustomTheme,
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false,
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "Big Hub"
    },
    Discord = {
        Enabled = false,
        Invite = "noinvitelink",
        RememberJoins = true
    },
    KeySystem = false,
    KeySettings = {
        Title = "Untitled",
        Subtitle = "Key System",
        Note = "No method of obtaining the key is provided",
        FileName = "Key",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = {"Hello"}
    }
})

local Tab = Window:CreateTab("Main Tab", 4483362458)
Tab:CreateSection("Main Features")
Tab:CreateDivider()

Rayfield:Notify({
    Title = "Welcome",
    Content = "UI Loaded Successfully!",
    Duration = 5,
    Image = 4483362458
})

-- Button
local Button = Tab:CreateButton({
    Name = "Button Example",
    Callback = function()
        print("Button clicked!")
    end,
})
Button:Set("Button Example")

-- Hitbox Logic
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local toggleActive = false

local function CreateHitboxForCharacter(char)
    for _, part in pairs(char:GetChildren()) do
        if part:IsA("BasePart") and not part:FindFirstChild("HitboxOverlay") then
            local box = Instance.new("BoxHandleAdornment")
            box.Name = "HitboxOverlay"
            box.Adornee = part
            box.AlwaysOnTop = true
            box.ZIndex = 5
            box.Size = part.Size
            box.Color3 = Color3.new(1, 0, 0)
            box.Transparency = 0.6
            box.Parent = part
        end
    end
end

local function RemoveHitboxFromCharacter(char)
    for _, part in pairs(char:GetChildren()) do
        if part:IsA("BasePart") then
            local adorn = part:FindFirstChild("HitboxOverlay")
            if adorn then
                adorn:Destroy()
            end
        end
    end
end

local function SetupCharacterMonitoring(player)
    local function OnCharacterAdded(character)
        if toggleActive then
            CreateHitboxForCharacter(character)
        end
    end
    player.CharacterAdded:Connect(OnCharacterAdded)
    if player.Character then
        OnCharacterAdded(player.Character)
    end
end

local function OnPlayerAdded(player)
    if player ~= LocalPlayer then
        SetupCharacterMonitoring(player)
    end
end

for _, player in ipairs(Players:GetPlayers()) do
    OnPlayerAdded(player)
end

Players.PlayerAdded:Connect(OnPlayerAdded)

-- Toggle (Renamed to Hitbox Player)
local Toggle = Tab:CreateToggle({
    Name = "Hitbox Player",
    CurrentValue = false,
    Flag = "Toggle1",
    Callback = function(Value)
        toggleActive = Value
        print("Toggle is now", Value)

        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character then
                if Value then
                    CreateHitboxForCharacter(player.Character)
                else
                    RemoveHitboxFromCharacter(player.Character)
                end
            end
        end
    end,
})
Toggle:Set(false)

-- Color Picker
local ColorPicker = Tab:CreateColorPicker({
    Name = "Color Picker",
    Color = Color3.fromRGB(255,255,255),
    Flag = "ColorPicker1",
    Callback = function(Value)
        print("Selected color:", Value)
    end
})
ColorPicker:Set(Color3.fromRGB(255,255,255))

-- Slider
local Slider = Tab:CreateSlider({
    Name = "Slider Example",
    Range = {0, 100},
    Increment = 10,
    Suffix = "Bananas",
    CurrentValue = 10,
    Flag = "Slider1",
    Callback = function(Value)
        print("Slider value:", Value)
    end,
})
Slider:Set(10)

-- Input
local Input = Tab:CreateInput({
    Name = "Input Example",
    CurrentValue = "",
    PlaceholderText = "Input Placeholder",
    RemoveTextAfterFocusLost = false,
    Flag = "Input1",
    Callback = function(Text)
        print("Input value:", Text)
    end,
})
Input:Set("New Text")

-- Dropdown
local Dropdown = Tab:CreateDropdown({
    Name = "Dropdown Example",
    Options = {"Option 1", "Option 2"},
    CurrentOption = {"Option 1"},
    MultipleOptions = false,
    Flag = "Dropdown1",
    Callback = function(Options)
        print("Selected option:", table.concat(Options, ", "))
    end,
})
Dropdown:Set({"Option 2"})
Dropdown:Refresh({"Option 1", "Option 2", "Option 3"})
