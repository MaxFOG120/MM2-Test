local ScreenGui = Instance.new("ScreenGui")
local IconButton = Instance.new("ImageButton")
local CloseButton = Instance.new("TextButton")
local Panel = Instance.new("Frame")
local Slider1 = Instance.new("TextButton")
local Slider2 = Instance.new("TextButton")
local Slider3 = Instance.new("TextButton")
local SpeedButton = Instance.new("TextButton")

-- Настройки GUI
ScreenGui.Name = "CustomGui"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

IconButton.Name = "IconButton"
IconButton.Size = UDim2.new(0, 50, 0, 50)
IconButton.Position = UDim2.new(0.5, -25, 0.5, -25)
IconButton.Image = "rbxassetid://your_icon_asset_id_here" -- замените на ID Вашего изображения
IconButton.Parent = ScreenGui

Panel.Name = "ControlPanel"
Panel.Size = UDim2.new(0, 200, 0, 200)
Panel.Position = UDim2.new(0.5, -100, 0.5, -100)
Panel.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
Panel.Visible = false
Panel.Parent = ScreenGui

CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -30, 0, 5)
CloseButton.Text = "X"
CloseButton.Parent = Panel

Slider1.Name = "FlightToggle"
Slider1.Size = UDim2.new(1, 0, 0, 50)
Slider1.Position = UDim2.new(0, 0, 0, 0)
Slider1.Text = "Toggle Flight"
Slider1.Parent = Panel

Slider2.Name = "NoclipToggle"
Slider2.Size = UDim2.new(1, 0, 0, 50)
Slider2.Position = UDim2.new(0, 0, 0, 50)
Slider2.Text = "Toggle Noclip"
Slider2.Parent = Panel

Slider3.Name = "TransparencyToggle"
Slider3.Size = UDim2.new(1, 0, 0, 50)
Slider3.Position = UDim2.new(0, 0, 0, 100)
Slider3.Text = "Toggle Head Transparency"
Slider3.Parent = Panel

SpeedButton.Name = "SpeedToggle"
SpeedButton.Size = UDim2.new(1, 0, 0, 50)
SpeedButton.Position = UDim2.new(0, 0, 0, 150)
SpeedButton.Text = "Increase Speed"
SpeedButton.Parent = Panel

-- Переменные для функций
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local Head = Character:WaitForChild("Head")
local Flying = false
local Noclip = false
local DefaultWalkSpeed = 16
local DefaultJumpPower = 50

-- Функция для включения/выключения полета
local function toggleFlight()
    Flying = not Flying
    if Flying then
        Humanoid.PlatformStand = true
        while Flying do
            wait()
            Character.HumanoidRootPart.Velocity = Vector3.new(Character.HumanoidRootPart.Velocity.X, 50, Character.HumanoidRootPart.Velocity.Z)
        end
    else
        Humanoid.PlatformStand = false
    end
end

-- Функция для включения/выключения ноклипа
local function toggleNoclip()
    Noclip = not Noclip
    if Noclip then
        for _, v in ipairs(Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = false
            end
        end
    else
        for _, v in ipairs(Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = true
            end
        end
    end
end

-- Функция для изменения прозрачности головы
local function toggleTransparency()
    if Head.Transparency == 0 then
        Head.Transparency = 1 -- делает голову прозрачной
    else
        Head.Transparency = 0 -- восстанавливает нормальную видимость
    end
end

-- Функция для изменения скорости
local function increaseSpeed()
    Humanoid.WalkSpeed = Humanoid.WalkSpeed == DefaultWalkSpeed and 50 or DefaultWalkSpeed -- Увеличиваем скорость
end

-- Обработчик нажатия на кнопку
IconButton.MouseButton1Click:Connect(function()
    Panel.Visible = not Panel.Visible
end)

-- Обработчик нажатия на кнопку закрытия панели
CloseButton.MouseButton1Click:Connect(function()
    Panel.Visible = false
end)

-- Обработчики нажатий на кнопки
Slider1.MouseButton1Click:Connect(toggleFlight)
Slider2.MouseButton1Click:Connect(toggleNoclip)
Slider3.MouseButton1Click:Connect(toggleTransparency)
SpeedButton.MouseButton1Click:Connect(increaseSpeed)

-- Возможность перемещения панели
local dragToggle = nil
local dragInput = nil
local dragStart = nil
local startPos = nil

local function update(input)
    local delta = input.Position - dragStart
    Panel.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

Panel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragToggle = true
        dragStart = input.Position
        startPos = Panel.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragToggle = false
            end
        end)
    end
end)

Panel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

game:GetService("UserInputService").InputChanged:Connect(function(input)
    if input == dragInput and dragToggle then
        update(input)
    end
end)

-- Подсветка игроков на карте
local function highlightPlayers()
    local players = game.Players:GetPlayers()
    for _, player in ipairs(players) do
        if player ~= Player and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local highlight = Instance.new("Highlight")
            highlight.Adornee = player.Character
            highlight.FillColor = Color3.new(1, 0, 0) -- Красный цвет подсветки
            highlight.Parent = player.Character
        end
    end
end

highlightPlayers()
