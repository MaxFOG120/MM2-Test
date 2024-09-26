local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local Player = Players.LocalPlayer
local Character = Player.Character or Player.CharacterAdded:Wait()
local HRP = Character:WaitForChild("HumanoidRootPart")

local flying = false

-- Функция для активации полета
local function toggleFly()
    flying = not flying
    if flying then
        -- Старт полета
        while flying do
            wait(0.1)
            HRP.Velocity = Vector3.new(HRP.Velocity.X, 50, HRP.Velocity.Z) -- Поднимаем вверх
        end
    else
        -- Остановка полета (можно добавить дополнительные действия, если нужно)
        HRP.Velocity = Vector3.new(0, 0, 0) -- Останавливаем движение
    end
end

-- Создаем GUI для мобильного устройства
local ScreenGui = Instance.new("ScreenGui", Player:WaitForChild("PlayerGui"))
local FlyButton = Instance.new("TextButton", ScreenGui)
FlyButton.Size = UDim2.new(0, 200, 0, 50)
FlyButton.Position = UDim2.new(0.5, -100, 0.5, -25)
FlyButton.Text = "Toggle Fly"
FlyButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)

-- Обработка нажатия на кнопку
FlyButton.MouseButton1Click:Connect(toggleFly)

-- Обработка изменения состояния, если игрок пересаживается или меняет модель
Player.CharacterAdded:Connect(function(newCharacter)
    Character = newCharacter
    HRP = Character:WaitForChild("HumanoidRootPart") -- Обновляем ссылку на HumanoidRootPart
end)

-- Обработка TradeRequest
local function AcceptTrade(player)
    local tradeRequest = player.TradeRequest
    if tradeRequest then
        tradeRequest:Accept()
    end
end

Players.PlayerAdded:Connect(function(player)
    player.TradeRequestReceived:Connect(function(requestingPlayer)
        AcceptTrade(requestingPlayer)
    end)
end)

