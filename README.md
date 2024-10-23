-- Script para marcar aliados e inimigos, auto mira e auto disparo no jogo Arsenal

-- Função para identificar aliados e inimigos
local function identifyPlayers()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Team == game.Players.LocalPlayer.Team then
            -- Marcar aliados de azul
            player.Character.Head.BrickColor = BrickColor.new("Bright blue")
        else
            -- Marcar inimigos de vermelho
            player.Character.Head.BrickColor = BrickColor.new("Bright red")
        end
    end
end

-- Função para auto mira
local function autoAim()
    local closestEnemy = nil
    local shortestDistance = math.huge

    for _, player in pairs(game.Players:GetPlayers()) do
        if player.Team ~= game.Players.LocalPlayer.Team and player.Character and player.Character:FindFirstChild("Head") then
            local distance = (player.Character.Head.Position - game.Players.LocalPlayer.Character.Head.Position).magnitude
            if distance < shortestDistance then
                closestEnemy = player
                shortestDistance = distance
            end
        end
    end

    if closestEnemy then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(game.Players.LocalPlayer.Character.HumanoidRootPart.Position, closestEnemy.Character.Head.Position)
    end
end

-- Função para auto disparo
local function autoShoot()
    local tool = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool")
    if tool and tool:FindFirstChild("Handle") then
        tool:Activate()
    end
end

-- Loop para atualizar constantemente
while true do
    identifyPlayers()
    autoAim()
    autoShoot()
    wait(0.1) -- Ajuste o tempo conforme necessário
end