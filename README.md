
-- Создаем GUI элементы
local ScreenGui = Instance.new("ScreenGui")
local IconButton = Instance.new("ImageButton")
local Panel = Instance.new("Frame")
local Slider1 = Instance.new("TextButton")
local Slider2 = Instance.new("TextButton")
local Slider3 = Instance.new("TextButton")

-- Настройки GUI
ScreenGui.Name = "CustomGui"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

IconButton.Name = "IconButton"
IconButton.Size = UDim2.new(0, 50, 0, 50)
IconButton.Position = UDim2.new(0.5, -25, 0.5, -25)
IconButton.Image = "rbxassetid://your_icon_asset_id_here" -- замените на ID вашего изображения
IconButton.Parent = ScreenGui

Panel.Name = "ControlPanel"
Panel.Size = UDim2.new(0, 200, 0, 150)
Panel.Position = UDim2.new(0.5, -100, 0.5, -75)
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

-- Переменные для функций
local Player = game.Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local Humanoid = Character:WaitForChild("Humanoid")
local Head = Character:WaitForChild("Head")
local Flying = false
local Noclip = false

-- Функция для включения/выключения полета
local function toggleFlight()
    Flying = not Flying
    if Flying then
        Humanoid.PlatformStand = true
        while Flying do
            wait()
            Character.HumanoidRootPart.Velocity = Vector3.new(0, 50, 0) -- поднимает игрока
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

-- Обработчик нажатия на кнопку
IconButton.MouseButton1Click:Connect(function()
    Panel.Visible = not Panel.Visible
end)

-- Обработчики нажатий на ползунки
Slider1.MouseButton1Click:Connect(toggleFlight)
Slider2.MouseButton1Click:Connect(toggleNoclip)
Slider3.MouseButton1Click:Connect(toggleTransparency)

