--Thy hacker gui v2
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")
local playerGui = player:WaitForChild("PlayerGui")
local runService = game:GetService("RunService")

-- GUI Creation
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ThyHackerGUI"
screenGui.Parent = playerGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 300, 0, 400)
frame.Position = UDim2.new(0.5, -150, 0.5, -200)
frame.BackgroundTransparency = 0.3
frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
frame.Active = true
frame.Draggable = true
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.Text = "Thy Hacker GUI"
title.TextColor3 = Color3.new(1, 1, 1)
title.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
title.TextScaled = true
title.Parent = frame

local function createButton(name, text, pos, callback)
    local button = Instance.new("TextButton")
    button.Size = UDim2.new(0.9, 0, 0, 40)
    button.Position = pos
    button.Text = text
    button.TextColor3 = Color3.new(1, 1, 1)
    button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    button.TextScaled = true
    button.Parent = frame
    button.MouseButton1Click:Connect(callback)
    return button
end

-- Speed Changer
createButton("SpeedBoost", "Set Speed to 100", UDim2.new(0.05, 0, 0, 50), function()
    humanoid.WalkSpeed = 100
end)

-- Jump Boost
createButton("JumpBoost", "Set Jump Power to 150", UDim2.new(0.05, 0, 0, 100), function()
    humanoid.JumpPower = 150
end)

-- Teleport to Players
createButton("Teleport", "Teleport to Random Player", UDim2.new(0.05, 0, 0, 150), function()
    local otherPlayers = {}
    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            table.insert(otherPlayers, plr)
        end
    end

    if #otherPlayers > 0 then
        local targetPlayer = otherPlayers[math.random(1, #otherPlayers)]
        character:MoveTo(targetPlayer.Character.HumanoidRootPart.Position)
    else
        warn("No players to teleport to.")
    end
end)

-- Fling Functionality
createButton("Fling", "Fling Nearest Player", UDim2.new(0.05, 0, 0, 200), function()
    local closestPlayer
    local closestDistance = math.huge

    for _, plr in pairs(game.Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (character.HumanoidRootPart.Position - plr.Character.HumanoidRootPart.Position).Magnitude
            if distance < closestDistance then
                closestDistance = distance
                closestPlayer = plr
            end
        end
    end

    if closestPlayer and closestPlayer.Character then
        local rootPart = closestPlayer.Character:FindFirstChild("HumanoidRootPart")
        if rootPart then
            local bodyVelocity = Instance.new("BodyVelocity")
            bodyVelocity.Velocity = Vector3.new(0, 200, 0)
            bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
            bodyVelocity.Parent = rootPart
            game.Debris:AddItem(bodyVelocity, 0.5)
        end
    end
end)

-- Reset Speed and Jump
createButton("Reset", "Reset Speed & Jump", UDim2.new(0.05, 0, 0, 250), function()
    humanoid.WalkSpeed = 16
    humanoid.JumpPower = 50
end)

-- Close GUI
createButton("Close", "Close GUI", UDim2.new(0.05, 0, 0, 300), function()
    screenGui:Destroy()
end)

-- Rainbow Title Effect
spawn(function()
    while true do
        for i = 0, 255 do
            title.TextColor3 = Color3.fromHSV(i / 255, 1, 1)
            runService.Heartbeat:Wait()
        end
    end
end)
