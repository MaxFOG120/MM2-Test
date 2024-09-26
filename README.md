local ScreenGui = Instance.new("ScreenGui")
local IconButton = Instance.new("ImageButton")
local Panel = Instance.new("Frame")
local Slider1 = Instance.new("TextButton")
local Slider2 = Instance.new("TextButton")
local Slider3 = Instance.new("TextButton")
local CloseButton = Instance.new("TextButton")
local SpeedSlider = Instance.new("TextButton")

-- Настройки GUI
ScreenGui.Name = "CustomGui"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

IconButton.Name = "IconButton"
IconButton.Size = UDim2.new(0, 50, 0, 50)
IconButton.Position = UDim2.new(0.5, -25, 0.5, -25)
IconButton.Image = "rbxassetid://104742316583503" -- замените на ID Вашего изображения
IconButton.Parent = ScreenGui

Panel.Name = "ControlPanel"
Panel.Size = UDim2.new(0, 200, 0, 200)
Panel.Position = UDim2.new(0.5, -100, 0.5, -100)
Panel.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
Panel.Visible = false
Panel.Parent = ScreenGui

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

CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 50, 0, 25)
CloseButton.Position = UDim2.new(1, -55, 0, 0)
CloseButton.Text = "X"
CloseButton.Parent = Panel

SpeedSlider.Name = "SpeedToggle"
SpeedSlider.Size = UDim2.new(1, 0, 0, 50)
SpeedSlider.Position = UDim2.new(0, 0, 0, 150)
SpeedSlider.Text = "Change Speed"
SpeedSlider.Parent = Panel

-- Переменные для функций
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local Head = Character:WaitForChild("Head")
local Flying = false
local Noclip = false
local defaultSpeed = 16 -- стандартная скорость
local flyingSpeed = 50 -- скорость во время полета
local isMoving = false

-- Функция для включения/выключения полета
local function toggleFlight()
    Flying = not Flying
    if Flying then
        Humanoid.PlatformStand = true
        while Flying do
            wait()
            Character.HumanoidRootPart.Velocity = Vector3.new(0, flyingSpeed, 0) -- поднимает игрока
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

-- Функция для изменения скорости игрока
local function changeSpeed()
    if Humanoid.WalkSpeed == defaultSpeed then
        Humanoid.WalkSpeed = 100 -- новая скорость
    else
        Humanoid.WalkSpeed = defaultSpeed -- восстановление стандартной скорости
    end
end

-- Обработчик нажатия на кнопку
IconButton.MouseButton1Click:Connect(function()
    Panel.Visible = not Panel.Visible
end)

-- Обработчик нажатия на кнопку закрытия
CloseButton.MouseButton1Click:Connect(function()
    Panel.Visible = false
end)

-- Обработчики нажатий на ползунки
Slider1.MouseButton1Click:Connect(toggleFlight)
Slider2.MouseButton1Click:Connect(toggleNoclip)
Slider3.MouseButton1Click:Connect(toggleTransparency)
SpeedSlider.MouseButton1Click:Connect(changeSpeed)

-- Функция для перемещения панели
local dragging, dragInput, dragStart, startPos
Panel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = Panel.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

Panel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and dragging then
        local delta = input.Position - dragStart
        Panel.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
