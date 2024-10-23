-- Script para marcar aliados e inimigos, auto mira e auto disparo no jogo Arsenal
-- Adicionada funcionalidade para ligar/desligar com a tecla "B"
-- Adicionado FOV e círculo no centro da tela

local enabled = true

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

-- Função para auto mira na cabeça dos inimigos
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

-- Função para alternar o estado do script
local function toggleScript()
    enabled = not enabled
end

-- Conectar a tecla "B" para ligar/desligar o script
game:GetService("UserInputService").InputBegan:Connect(function(input, gameProcessed)
    if input.KeyCode == Enum.KeyCode.B and not gameProcessed then
        toggleScript()
    end
end)

-- Função para desenhar o círculo no centro da tela
local function drawCircle()
    local screenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
    local circle = Instance.new("Frame", screenGui)
    circle.Size = UDim2.new(0, 100, 0, 100)
    circle.Position = UDim2.new(0.5, -50, 0.5, -50)
    circle.BackgroundTransparency = 1
    local uiCorner = Instance.new("UICorner", circle)
    uiCorner.CornerRadius = UDim.new(1, 0)
    local uiStroke = Instance.new("UIStroke", circle)
    uiStroke.Thickness = 2
    uiStroke.Color = Color3.new(1, 1, 1)
end

-- Desenhar o círculo no centro da tela
drawCircle()

-- Loop para atualizar constantemente
while true do
    if enabled then
        identifyPlayers()
        autoAim()
        autoShoot()
    end
    wait(0.1) -- Ajuste o tempo conforme necessário
end
