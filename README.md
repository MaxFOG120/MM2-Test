local Players = game:GetService("Players")

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
