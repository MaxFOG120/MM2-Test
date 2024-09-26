-- Создаем GUI элементы
local ScreenGui = Instance.new("ScreenGui")
local IconButton = Instance.new("ImageButton")
local Panel = Instance.new("Frame")
local CloseButton = Instance.new("TextButton")
local Slider1 = Instance.new("TextButton")
local Slider2 = Instance.new("TextButton")
local Slider3 = Instance.new("TextButton")
local SpeedButton = Instance.new("TextButton")
local HighJumpButton = Instance.new("TextButton")
local HighlightButton = Instance.new("TextButton")

-- Настройки GUI
ScreenGui.Name = "CustomGui"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

IconButton.Name = "IconButton"
IconButton.Size = UDim2.new(0, 50, 0, 50)
IconButton.Position = UDim2.new(0.5, -25, 0.5, -25)
IconButton.Image = "rbxassetid://your_icon_asset_id_here" -- замените на ID Вашего изображения
IconButton.Parent = ScreenGui

Panel.Name = "ControlPanel"
Panel.Size = UDim2.new(0, 200, 0, 250)
Panel.Position = UDim2.new(0.5, -100, 0.5, -125)
Panel.BackgroundColor3 = Color3.fromRGB(200, 200, 200)
Panel.Visible = false
Panel.Parent = ScreenGui

CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.Text = "X"
CloseButton.Parent = Panel

Slider1.Name = "FlightToggle"
Slider1.Size = UDim2.new(1, 0, 0, 50)
Slider1.Position = UDim2.new(0, 0, 0, 30)
Slider1.Text = "Toggle Flight"
Slider1.Parent = Panel

Slider2.Name = "NoclipToggle"
Slider2.Size = UDim2.new(1, 0, 0, 50)
Slider2.Position = UDim2.new(0, 0, 0, 80)
Slider2.Text = "Toggle Noclip"
Slider2.Parent = Panel

Slider3.Name = "TransparencyToggle"
Slider3.Size = UDim2.new(1, 0, 0, 50)
Slider3.Position = UDim2.new(0, 0, 0, 130)
Slider3.Text = "Toggle Head Transparency"
Slider3.Parent = Panel

SpeedButton.Name = "SpeedToggle"
SpeedButton.Size = UDim2.new(1, 0, 0, 50)
SpeedButton.Position = UDim2.new(0, 0, 0, 180)
SpeedButton.Text = "Increase Speed"
SpeedButton.Parent = Panel

HighJumpButton.Name = "HighJumpToggle"
HighJumpButton.Size = UDim2.new(1, 0, 0, 50)
HighJumpButton.Position = UDim2.new(0, 0, 0, 230)
HighJumpButton.Text = "Toggle High Jump"
HighJumpButton.Parent = Panel

HighlightButton.Name = "HighlightToggle"
HighlightButton.Size = UDim2.new(1, 0, 0, 50)
HighlightButton.Position = UDim2.new(0, 0, 0, 280)
HighlightButton.Text = "Highlight Players"
HighlightButton.Parent = Panel


-- Переменные для функций
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local Head = Character:WaitForChild("Head")
local Flying = false
local Noclip = false
local HighJumpEnabled = false
local HighlightEnabled = false

-- Функция для включения/выключения полета
local function toggleFlight()
    Flying = not Flying
    if Flying then
        Humanoid.PlatformStand = true
        while Flying do
            wait(0.1)
            Character.HumanoidRootPart.Velocity = Vector3.new(0, 50, 0) -- поднимает игрока
            -- Управление полетом
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                Character.HumanoidRootPart.Velocity = Character.HumanoidRootPart.CFrame.LookVector * 50 -- движение вперед
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                Character.HumanoidRootPart.Velocity = -Character.HumanoidRootPart.CFrame.LookVector * 50 -- движение назад
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                Character.HumanoidRootPart.Velocity = -Character.HumanoidRootPart.CFrame.RightVector * 50 -- движение влево
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                Character.HumanoidRootPart.Velocity = Character.HumanoidRootPart.CFrame.RightVector * 50 -- движение вправо
            end
        end
    else
        Humanoid.PlatformStand = false
    end
end

-- Функция для включения/выключения ноклипа
local function toggleNoclip()
    Noclip = not Noclip
    while Noclip do
        for _, v in ipairs(Character:GetDescendants()) do
            if v:IsA("BasePart") then
                v.CanCollide = false
            end
        end
        wait()
    end
    for _, v in ipairs(Character:GetDescendants()) do
        if v:IsA("BasePart") then
            v.CanCollide = true
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

-- Функция для изменение скорости
local function changeSpeed()
    Humanoid.WalkSpeed = Humanoid.WalkSpeed + 10 -- увеличивает скорость на 10
end

-- Функция для высоких прыжков
local function toggleHighJump()
    HighJumpEnabled = not HighJumpEnabled
    if HighJumpEnabled then
        Humanoid.JumpPower = 100 -- увеличиваем силу прыжка
    else
        Humanoid.JumpPower = 50 -- восстанавливаем нормальную силу
    end
end

-- Функция для подсветки игроков
local function toggleHighlight()
    HighlightEnabled = not HighlightEnabled
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= Player then
            local highlight = Instance.new("Highlight")
            highlight.Parent = player.Character
            if HighlightEnabled then
                highlight.FillColor = Color3.fromRGB(255, 0, 0) -- красный цвет подсветки
            else
                highlight:Destroy()
            end
        end
    end
end

-- Обработчик нажатия на кнопку закрытия
CloseButton.MouseButton1Click:Connect(function()
    Panel.Visible = false
end)

-- Обработчик нажатия на иконку
IconButton.MouseButton1Click:Connect(function()
    Panel.Visible = not Panel.Visible
end)

-- Обработчики нажатий на ползунки
Slider1.MouseButton1Click:Connect(toggleFlight)
Slider2.MouseButton1Click:Connect(toggleNoclip)
Slider3.MouseButton1Click:Connect(toggleTransparency)
SpeedButton.MouseButton1Click:Connect(changeSpeed)
HighJumpButton.MouseButton1Click:Connect(toggleHighJump)
HighlightButton.MouseButton1Click:Connect(toggleHighlight)

-- Перемещение панели
local UserInputService = game:GetService("UserInputService")
local dragging = false
local dragInput
local dragStart
local startPos

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
    if input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)

game:GetService("RunService").RenderStepped:Connect(function()
    if dragging and dragInput then
        local delta = dragInput.Position - dragStart
        Panel.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)
