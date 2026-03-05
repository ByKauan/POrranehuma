local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- ScreenGui (TPS)
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "Tps"
screenGui.Parent = player:WaitForChild("PlayerGui")
screenGui.ResetOnSpawn = false
screenGui.DisplayOrder = 10

-- GUI PRINCIPAL (APENAS VISUAL)
local toggleBtn = Instance.new("ImageButton")
toggleBtn.Name = "ToggleBtn"
toggleBtn.Size = UDim2.new(0, 100, 0, 100)
toggleBtn.Position = UDim2.new(0, 20, 0.5, -50)
toggleBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
toggleBtn.Image = "rbxassetid://113040653096663"
toggleBtn.ImageColor3 = Color3.new(1,1,1)
toggleBtn.ImageTransparency = 0
toggleBtn.ScaleType = Enum.ScaleType.Fit
toggleBtn.ZIndex = 1
toggleBtn.Parent = screenGui

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(0, 50)
toggleCorner.Parent = toggleBtn

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 400, 0, 300)
mainFrame.Position = UDim2.new(0.5, -200, 0.5, -150)
mainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
mainFrame.BackgroundTransparency = 0.05
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false
mainFrame.ZIndex = 0
mainFrame.Parent = screenGui

local mainCorner = Instance.new("UICorner")
mainCorner.CornerRadius = UDim.new(0, 18)
mainCorner.Parent = mainFrame

local backgroundImage = Instance.new("ImageLabel")
backgroundImage.Name = "BackgroundImage"
backgroundImage.Size = UDim2.new(1, 0, 1, 0)
backgroundImage.Position = UDim2.new(0, 0, 0, 0)
backgroundImage.BackgroundTransparency = 1
backgroundImage.Image = "rbxassetid://95900650574559"
backgroundImage.ImageTransparency = 0.2
backgroundImage.ScaleType = Enum.ScaleType.Tile
backgroundImage.ZIndex = 0
backgroundImage.Parent = mainFrame

local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(10, 10, 15)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 30))
}
gradient.Rotation = 45
gradient.Parent = mainFrame

local header = Instance.new("Frame")
header.Name = "Header"
header.Size = UDim2.new(1, 0, 0, 65)
header.BackgroundTransparency = 1
header.Parent = mainFrame

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 40, 0, 40)
closeBtn.Position = UDim2.new(1, -50, 0, 12)
closeBtn.BackgroundColor3 = Color3.fromRGB(255, 80, 80)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.TextScaled = true
closeBtn.Font = Enum.Font.GothamBold
closeBtn.ZIndex = 600
closeBtn.Parent = header

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 20)
closeCorner.Parent = closeBtn

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, -60, 1, 0)
title.Position = UDim2.new(0, 20, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Gengar Hub"
title.TextColor3 = Color3.new(1,1,1)
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.TextXAlignment = Enum.TextXAlignment.Left
title.ZIndex = 600
title.Parent = header

-- EVENTOS GUI (APENAS VISUAIS)
toggleBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
    toggleBtn.BackgroundColor3 = mainFrame.Visible and Color3.fromRGB(50, 255, 50) or Color3.fromRGB(255, 50, 50)
end)

closeBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    toggleBtn.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
end)

-- ARRASTO
local dragging, dragStart, startPos = false
header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = mainFrame.Position
    end
end)

header.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

header.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
