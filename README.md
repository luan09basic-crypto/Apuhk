
--[[
    MAGIRO HUB - ULTIMATE KAITUN
    Open Source Project
    Language: English
    Platform: Roblox Studio
]]

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")

local Player = Players.LocalPlayer

-- Configuration
local Settings = {
    AutoFarm = true,
    AutoMission = true,
    AutoRepeatMission = false,
    KillAura = true,
    AutoCollectFruit = true,
    AutoSpinFruit = true,
    AutoStats = true
}

-- Create Interface
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "MagiroHub"
ScreenGui.ResetOnSpawn = false

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.fromOffset(330, 390)
Main.Position = UDim2.new(0.5, -165, 0.5, -195)
Main.BackgroundColor3 = Color3.fromRGB(20, 20, 28)
Main.BorderSizePixel = 0
Main.Parent = ScreenGui

local Corner = Instance.new("UICorner")
Corner.CornerRadius = UDim.new(0, 12)
Corner.Parent = Main

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 45)
Title.BackgroundTransparency = 1
Title.Text = "MAGIRO HUB"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 23
Title.Font = Enum.Font.GothamBold
Title.Parent = Main

-- Create Toggle
local function CreateToggle(name, setting, position)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0.9, 0, 0, 38)
    Button.Position = UDim2.new(0.05, 0, 0, position)
    Button.BackgroundColor3 = Settings[setting]
        and Color3.fromRGB(30, 130, 80)
        or Color3.fromRGB(40, 40, 52)

    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.GothamSemibold
    Button.TextSize = 14

    Button.Text = name ..
        (Settings[setting] and ": ON" or ": OFF")

    Button.Parent = Main

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 7)
    ButtonCorner.Parent = Button

    Button.MouseButton1Click:Connect(function()
        Settings[setting] = not Settings[setting]

        Button.Text = name ..
            (Settings[setting] and ": ON" or ": OFF")

        Button.BackgroundColor3 = Settings[setting]
            and Color3.fromRGB(30, 130, 80)
            or Color3.fromRGB(40, 40, 52)
    end)

    return Button
end

-- Main Features
CreateToggle("Auto Farm", "AutoFarm", 60)
CreateToggle("Auto Mission", "AutoMission", 105)
CreateToggle("Repeat Mission", "AutoRepeatMission", 150)
CreateToggle("Kill Aura", "KillAura", 195)
CreateToggle("Auto Collect Fruit", "AutoCollectFruit", 240)
CreateToggle("Auto Spin Fruit", "AutoSpinFruit", 285)
CreateToggle("Auto Stats", "AutoStats", 330)

-- Drag Interface
local dragging = false
local dragStart
local startPosition

Title.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        dragging = true
        dragStart = input.Position
        startPosition = Main.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then

        local delta = input.Position - dragStart

        Main.Position = UDim2.new(
            startPosition.X.Scale,
            startPosition.X.Offset + delta.X,
            startPosition.Y.Scale,
            startPosition.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        dragging = false
    end
end)

ScreenGui.Parent = Player:WaitForChild("PlayerGui")

print("MAGIRO HUB loaded successfully!")
