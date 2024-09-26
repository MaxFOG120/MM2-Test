local Players = game:GetService("Players")
local StarterGui = game:GetService("StarterGui") -- используем для создания GUI
local player = Players.LocalPlayer -- получаем текущего игрока
local screenGui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))

local function createButton(text, position, callback)
    local button = Instance.new("TextButton")
    button.Text = text
    button.Size = UDim2.new(0, 100, 0, 50)
    button.Position = position
    button.BackgroundColor3 = Color3.new(1, 1, 1)
    button.MouseButton1Click:Connect(callback)
    button.Parent = screenGui
    return button
end

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

-- Создаем кнопки на панели
createButton("Принять трейд", UDim2.new(0, 10, 0, 10), function()
    -- Логика для принятия трейда
    local tradeRequest = player.TradeRequest
    if tradeRequest then
        tradeRequest:Accept()
    end
end)

createButton("Отклонить трейд", UDim2.new(0, 120, 0, 10), function()
    -- Логика для отклонения трейда
    local tradeRequest = player.TradeRequest
    if tradeRequest then
        tradeRequest:Decline()
    end
end)

