-- Demo GUI educativo (uso local / para aprender)
-- NÃO é um cheat. Não interage com outros jogadores nem com objetos de terceiros.

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Cria ScreenGui
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DemoToolsGui"
screenGui.ResetOnSpawn = false
screenGui.Parent = playerGui

-- Função utilitária para criar botões
local function createButton(parent, name, text, posY)
    local btn = Instance.new("TextButton")
    btn.Name = name
    btn.Text = text
    btn.Size = UDim2.new(0,200,0,40)
    btn.Position = UDim2.new(0,20,0,posY)
    btn.BackgroundTransparency = 0.15
    btn.Parent = parent
    return btn
end

-- Container
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0,240,0,180)
frame.Position = UDim2.new(0,10,0,80)
frame.BackgroundTransparency = 0.2
frame.Parent = screenGui

local title = Instance.new("TextLabel")
title.Text = "Demo Tools (educativo)"
title.Size = UDim2.new(1, -10, 0, 30)
title.Position = UDim2.new(0,5,0,5)
title.BackgroundTransparency = 1
title.Parent = frame

-- Estados
local states = {
    InstaSteal = false, -- demo only
    AntiSlap = false,   -- demo only
    AutoTeleport = false -- demo only
}

-- Botões
local btn1 = createButton(frame, "InstaStealBtn", "Insta Steal (demo): OFF", 40)
local btn2 = createButton(frame, "AntiSlapBtn", "Anti Slap (demo): OFF", 85)
local btn3 = createButton(frame, "AutoTeleportBtn", "Auto Teleport (demo): OFF", 130)

-- Atualiza texto do botão de acordo com o estado
local function updateButtonText()
    btn1.Text = "Insta Steal (demo): " .. (states.InstaSteal and "ON" or "OFF")
    btn2.Text = "Anti Slap (demo): " .. (states.AntiSlap and "ON" or "OFF")
    btn3.Text = "Auto Teleport (demo): " .. (states.AutoTeleport and "ON" or "OFF")
end

-- Funções demo (seguras) — aqui só exibimos mensagens / efeitos locais
local function onToggleInstaSteal()
    states.InstaSteal = not states.InstaSteal
    updateButtonText()
    if states.InstaSteal then
        warn("InstaSteal DEMO ativado — apenas para aprendizado. Não executa ações em servidores.")
    else
        warn("InstaSteal DEMO desativado.")
    end
end

local function onToggleAntiSlap()
    states.AntiSlap = not states.AntiSlap
    updateButtonText()
    if states.AntiSlap then
        warn("AntiSlap DEMO ativado — demonstra como ligar um comportamento local (não afeta outros jogadores).")
    else
        warn("AntiSlap DEMO desativado.")
    end
end

local function onToggleAutoTeleport()
    states.AutoTeleport = not states.AutoTeleport
    updateButtonText()
    if states.AutoTeleport then
        warn("AutoTeleport DEMO ativado — para testes em seu próprio mapa.")
    else
        warn("AutoTeleport DEMO desativado.")
    end
end

btn1.MouseButton1Click:Connect(onToggleInstaSteal)
btn2.MouseButton1Click:Connect(onToggleAntiSlap)
btn3.MouseButton1Click:Connect(onToggleAutoTeleport)

-- Exemplo seguro: função local que "teleporta" o próprio jogador para um ponto de teste
-- ATENÇÃO: use só em mapas de teste onde você tem permissão
local function safeTeleportToTestBase()
    if not states.AutoTeleport then return end
    local character = player.Character
    if not character then return end
    local hrp = character:FindFirstChild("HumanoidRootPart")
    if not hrp then return end

    -- posição de exemplo (edite para um lugar do seu mapa de teste)
    local testPosition = CFrame.new(0, 10, 0) -- posição segura de exemplo
    hrp.CFrame = testPosition
    warn("Demo: teleported to test base (local). Use apenas em seu mapa de teste.")
end

-- Trigger de exemplo: pressione a tecla "T" para executar o teleport de demonstração
local UserInputService = game:GetService("UserInputService")
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.T then
        safeTeleportToTestBase()
    end
end)

-- Info inicial
updateButtonText()
warn("Demo GUI carregada. Use os botões para alternar estados. Pressione T para testar o teleport local (se AutoTeleport estiver ON).")
