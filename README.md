-- VK Hub Script by your Assistant

-- Services
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Workspace = game:GetService("Workspace")
local VirtualUser = game:GetService("VirtualUser")
local UserInputService = game:GetService("UserInputService")
local Camera = workspace.CurrentCamera

-- Variables
local AutoFarm = false
local AutoBounty = false
local ESP = false
local FruitSteal = false
local SelectedTarget = "All"
local IsUnlocked = false

-- UI
local UI = Instance.new("ScreenGui")
local LoaderFrame = Instance.new("Frame")
local MainFrame = Instance.new("Frame")
local AutoFarmToggle = Instance.new("TextButton")
local AutoBountyToggle = Instance.new("TextButton")
local ESPToggle = Instance.new("TextButton")
local FruitStealToggle = Instance.new("TextButton")
local SelectedTargetDropdown = Instance.new("Dropdown")
local PasswordEntry = Instance.new("TextBox")
local UnlockButton = Instance.new("TextButton")

-- Functions
local function New(text)
    local t = Instance.new("TextLabel")
    t.Text = text
    t.Size = UDim2.new(1, 0, 0, 50)
    t.BackgroundTransparency = 1
    t.TextColor3 = Color3.new(1, 1, 1)
    t.Font = Enum.Font.SourceSans
    t.TextSize = 18
    t.TextXAlignment = Enum.TextXAlignment.Left
    return t
end

local function NewToggle(text, callback)
    local t = Instance.new("TextButton")
    t.Text = text
    t.Size = UDim2.new(1, 0, 0, 50)
    t.BackgroundTransparency = 0.5
    t.TextColor3 = Color3.new(1, 1, 1)
    t.Font = Enum.Font.SourceSans
    t.TextSize = 18
    t.TextXAlignment = Enum.TextXAlignment.Left
    t.MouseButton1Click:Connect(callback)
    return t
end

-- UI Creation
UI.Name = "VKHubUI"
UI.Parent = LocalPlayer.PlayerGui
UI.ResetOnSpawn = false

LoaderFrame.Parent = UI
LoaderFrame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
LoaderFrame.BorderColor3 = Color3.new(0.2, 0.2, 0.2)
LoaderFrame.BorderSizePixel = 0
LoaderFrame.Size = UDim2.new(0, 300, 0, 250)
LoaderFrame.Position = UDim2.new(0, 10, 0, 10)
LoaderFrame.Visible = true

PasswordEntry.Parent = LoaderFrame
PasswordEntry.Size = UDim2.new(1, 0, 0, 50)
PasswordEntry.Position = UDim2.new(0, 0, 0, 100)
PasswordEntry.PlaceholderText = "Enter Password"
PasswordEntry.TextColor3 = Color3.new(1, 1, 1)
PasswordEntry.Font = Enum.Font.SourceSans
PasswordEntry.TextSize = 18

UnlockButton.Parent = LoaderFrame
UnlockButton.Size = UDim2.new(1, 0, 0, 50)
UnlockButton.Position = UDim2.new(0, 0, 0, 150)
UnlockButton.Text = "Unlock"
UnlockButton.BackgroundTransparency = 0.5
UnlockButton.TextColor3 = Color3.new(1, 1, 1)
UnlockButton.Font = Enum.Font.SourceSans
UnlockButton.TextSize = 18
UnlockButton.TextXAlignment = Enum.TextXAlignment.Center
UnlockButton.MouseButton1Click:Connect(function()
    if PasswordEntry.Text == "1" then
        PasswordEntry.Visible = false
        UnlockButton.Visible = false
        LoaderFrame.Visible = false
        MainFrame.Visible = true
        IsUnlocked = true
    end
end)

MainFrame.Parent = UI
MainFrame.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
MainFrame.BorderColor3 = Color3.new(0.2, 0.2, 0.2)
MainFrame.BorderSizePixel = 0
MainFrame.Size = UDim2.new(0, 300, 0, 200)
MainFrame.Position = UDim2.new(0, 10, 0, 10)
MainFrame.Visible = false

AutoFarmToggle.Parent = MainFrame
AutoFarmToggle.Text = "Auto Farm: OFF"

AutoBountyToggle.Parent = MainFrame
AutoBountyToggle.Text = "Auto Bounty: OFF"

ESPToggle.Parent = MainFrame
ESPToggle.Text = "ESP: OFF"

FruitStealToggle.Parent = MainFrame
FruitStealToggle.Text = "Fruit Steal: OFF"

SelectedTargetDropdown.Parent = MainFrame
SelectedTargetDropdown.Size = UDim2.new(0.5, 0, 0, 50)
SelectedTargetDropdown.Position = UDim2.new(0, 0, 0, 150)
SelectedTargetDropdown.Text = "Select Target"

-- Auto Farming Functions
local function GetNearbyMobs()
    local nearbyMobs = {}
    for _, v in ipairs(Workspace.Mobs:GetChildren()) do
        if v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 and (v.Position - LocalPlayer.Character.PrimaryPart.Position).Magnitude < 50 then
            table.insert(nearbyMobs, v)
        end
    end
    return nearbyMobs
end

local function AttackMob(mob)
    VirtualUser:ClickButton1Down(Vector2.new(mob.Head.Position.X, mob.Head.Position.Y))
end

-- ESP Function
local function DrawESP( target )
    local billboardGui = Instance.new("BillboardGui")
    billboardGui.Name = "BillBoard"
    billboardGui.Adornee = target
    billboardGui.Size = UDim2.new(1, 0, 1, 0)
    billboardGui.AlwaysOnTop = true

    local textLabel = Instance.new("TextLabel")
    textLabel.Parent = billboardGui
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.new(1, 0, 0)
    textLabel.TextStrokeTransparency = 0
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.Text = target.Name
    textLabel.FontSize = "Size52"
    textLabel.TextSize = UDim2.new(1, 0, 1, 0)
    textLabel.Font = Enum.Font.SourceSansBold
end

-- Auto Farming and Cheats Threads
spawn(function()
    while wait(1) do
        if AutoFarm and IsUnlocked then
            local nearbyMobs = GetNearbyMobs()
            for _, mob in ipairs(nearbyMobs) do
                AttackMob(mob)
            end
        end
    end
end)

spawn(function()
    while wait(0.1) do
        if ESP and IsUnlocked then
            for _, plr in ipairs(Players:GetPlayers()) do
                if plr ~= LocalPlayer then
                    DrawESP(plr.Character)
                end
            end
        end
    end
end)

-- Toggles Callbacks
AutoFarmToggle.MouseButton1Click:Connect(function()
    AutoFarm = not AutoFarm
    AutoFarmToggle.Text = "Auto Farm: " .. (AutoFarm and "ON" or "OFF")
end)

AutoBountyToggle.MouseButton1Click:Connect(function()
    AutoBounty = not AutoBounty
    AutoBountyToggle.Text = "Auto Bounty: " .. (AutoBounty and "ON" or "OFF")
end)

ESPToggle.MouseButton1Click:Connect(function()
    ESP = not ESP
    ESPToggle.Text = "ESP: " .. (ESP and "ON" or "OFF")
end)

FruitStealToggle.MouseButton1Click:Connect(function()
    FruitSteal = not FruitSteal
    FruitStealToggle.Text = "Fruit Steal: " .. (FruitSteal and "ON" or "OFF")
end)

-- Target Dropdown Options (Replace with actual options)
SelectedTargetDropdown:UpdateList({
    "All",
    "Option 1",
    "Option 2",
    "Option 3"
})

SelectedTargetDropdown.Changed:Connect(function(selectedTarget)
    SelectedTarget = selectedTarget.Value
end)
