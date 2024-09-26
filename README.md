-- Создаем GUI элементы
local ScreenGui = Instance.new("ScreenGui")
local IconButton = Instance.new("ImageButton")
local Panel = Instance.new("Frame")
local Slider1 = Instance.new("TextButton") -- Toggle Flight
local Slider2 = Instance.new("TextButton") -- Toggle Noclip
local Slider3 = Instance.new("TextButton") -- Toggle Head Transparency
local Slider4 = Instance.new("TextButton") -- Change Walk Speed
local CloseButton = Instance.new("TextButton") -- Close Panel

-- Настройки GUI
ScreenGui.Name = "CustomGui"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

IconButton.Name = "IconButton"
IconButton.Size = UDim2.new(0, 50, 0, 50)
IconButton.Position = UDim2.new(0.5, -25, 0.5, -25)
IconButton.Image = "rbxassetid://your_icon_asset_id_here" -- замените на ID вашего изображения
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

Slider4.Name = "SpeedChange"
Slider4.Size = UDim2.new(1, 0, 0, 50)
Slider4.Position = UDim2.new(0, 0, 0, 150)
Slider4.Text = "Change Speed (Current: " .. tostring(Humanoid.WalkSpeed) .. ")"
Slider4.Parent = Panel

CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.Text = "×"
CloseButton.Parent = Panel

-- Переменные для функций
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local Head = Character:WaitForChild("Head")
local Flying = false
local Noclip = false
local originalWalkSpeed = Humanoid.WalkSpeed

-- Функция для включения/выключения полета
local function toggleFlight()
    Flying = not Flying
    if Flying then
        Humanoid.PlatformStand = true
        while Flying do
            wait()
            Character.HumanoidRootPart.Velocity = Vector3.new(0, 50, 0) 
        end
    else
        Humanoid.PlatformStand = false
    end
end

-- Функция для включения/выключения ноклипа
local function toggleNoclip()
    Noclip = not Noclip
    for _, v in ipairs(Character:GetDescendants()) do
        if v:IsA("BasePart") then
            v.CanCollide = not Noclip
        end
    end
end

-- Функция для изменения прозрачности головы
local function toggleTransparency()
    Head.Transparency = (Head.Transparency == 0) and 1 or 0
end

-- Функция для изменения скорости
local function changeSpeed()
    -- Устанавливаем новую скорость через пользовательский ввод, здесь вы можете добавить свою логику
    Humanoid.WalkSpeed = (Humanoid.WalkSpeed == originalWalkSpeed) and (originalWalkSpeed * 2) or originalWalkSpeed
    Slider4.Text = "Change Speed (Current: " .. tostring(Humanoid.WalkSpeed) .. ")"
end

-- Обработчик нажатия на кнопку и закрытие панели
IconButton.MouseButton1Click:Connect(function()
    Panel.Visible = not Panel.Visible
end)

CloseButton.MouseButton1Click:Connect(function()
    Panel.Visible = false
end)

-- Обработчики нажатий на ползунки
Slider1.MouseButton1Click:Connect(toggleFlight)
Slider2.MouseButton1Click:Connect(toggleNoclip)
Slider3.MouseButton1Click:Connect(toggleTransparency)
Slider4.MouseButton1Click:Connect(changeSpeed)

-- Перемещение панели
local dragging = false
local dragStart = nil
local startPos = nil

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
        Panel.Position = startPos + UDim2.new(0, delta.X, 0, delta.Y)
    end
end)
