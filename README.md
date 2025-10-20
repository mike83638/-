

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")

local player = Players.LocalPlayer

-- utilitário para criar instâncias rápido
local function new(className, props)
    local obj = Instance.new(className)
    if props then
        for k, v in pairs(props) do
            if k == "Parent" then
                obj.Parent = v
            else
                obj[k] = v
            end
        end
    end
    return obj
end

-- Root GUI
local screenGui = new("ScreenGui", {Parent = player:WaitForChild("PlayerGui"), Name = "FlyTeleportMenu", ResetOnSpawn = false})

-- Botão flutuante para abrir/fechar o menu
local toggleBtn = new("TextButton", {
    Parent = screenGui,
    Name = "ToggleButton",
    Size = UDim2.new(0, 120, 0, 30),
    Position = UDim2.new(0, 12, 0, 12),
    BackgroundColor3 = Color3.fromRGB(35,35,35),
    TextColor3 = Color3.fromRGB(240,240,240),
    Text = "Fly & Teleport",
    Font = Enum.Font.SourceSansBold,
    TextSize = 14,
})
new("UICorner", {Parent = toggleBtn, CornerRadius = UDim.new(0,6)})

-- Janela principal
local window = new("Frame", {
    Parent = screenGui,
    Name = "Window",
    Size = UDim2.new(0, 520, 0, 360),
    Position = UDim2.new(0.1, 0, 0.18, 0),
    BackgroundColor3 = Color3.fromRGB(28,28,28),
    Visible = false,
})
new("UICorner", {Parent = window, CornerRadius = UDim.new(0, 8)})
new("UIListLayout", {Parent = window, FillDirection = Enum.FillDirection.Horizontal})

-- Header (arrastar)
local header = new("Frame", {Parent = window, Size = UDim2.new(1,0,0,36), BackgroundTransparency = 1})
new("TextLabel", {
    Parent = header,
    Text = "Fly e Teleport   (arraste aqui)",
    Size = UDim2.new(1, -12, 1, 0),
    Position = UDim2.new(0,8,0,0),
    BackgroundTransparency = 1,
    TextColor3 = Color3.fromRGB(240,240,240),
    Font = Enum.Font.SourceSansBold,
    TextSize = 14,
    TextXAlignment = Enum.TextXAlignment.Left,
})

-- Painel esquerdo: opções
local leftPanel = new("Frame", {
    Parent = window,
    Size = UDim2.new(0, 160, 1, 0),
    BackgroundColor3 = Color3.fromRGB(20,20,20),
})
new("UICorner", {Parent = leftPanel, CornerRadius = UDim.new(0,8)})
new("UIListLayout", {Parent = leftPanel, Padding = UDim.new(0,6)})
new("UIPadding", {Parent = leftPanel, PaddingLeft = UDim.new(0,8), PaddingTop = UDim.new(0,8)})

local titleLeft = new("TextLabel", {
    Parent = leftPanel,
    Text = "Opções",
    Size = UDim2.new(1, -16, 0, 28),
    BackgroundTransparency = 1,
    TextColor3 = Color3.fromRGB(220,220,220),
    Font = Enum.Font.SourceSansBold,
    TextSize = 16,
    TextXAlignment = Enum.TextXAlignment.Left,
})

local function makeOption(name)
    local btn = new("TextButton", {
        Parent = leftPanel,
        Text = name,
        Size = UDim2.new(1, -16, 0, 36),
        BackgroundColor3 = Color3.fromRGB(42,42,42),
        TextColor3 = Color3.fromRGB(230,230,230),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
        AutoButtonColor = true,
    })
    new("UICorner", {Parent = btn, CornerRadius = UDim.new(0,6)})
    return btn
end

local optFlyTeleport = makeOption("Fly e teleport")

-- Painel direito: conteúdo (scrollable)
local rightPanel = new("Frame", {
    Parent = window,
    Size = UDim2.new(1, -160, 1, 0),
    BackgroundColor3 = Color3.fromRGB(26,26,26),
})
new("UICorner", {Parent = rightPanel, CornerRadius = UDim.new(0,8)})
new("UIPadding", {Parent = rightPanel, PaddingLeft = UDim.new(0,8), PaddingTop = UDim.new(0,8)})

local scroll = new("ScrollingFrame", {
    Parent = rightPanel,
    Size = UDim2.new(1, -16, 1, -16),
    Position = UDim2.new(0,8,0,8),
    BackgroundTransparency = 1,
    CanvasSize = UDim2.new(0, 0, 0, 600),
    ScrollBarThickness = 6,
})
local contentLayout = new("UIListLayout", {Parent = scroll, Padding = UDim.new(0,12)})
new("UIPadding", {Parent = scroll, PaddingTop = UDim.new(0,8), PaddingLeft = UDim.new(0,8), PaddingRight = UDim.new(0,8)})

-- Conteúdo "Fly e teleport"
local function createFlyTeleport(parent)
    local container = new("Frame", {
        Parent = parent,
        Size = UDim2.new(1, -24, 0, 360),
        BackgroundColor3 = Color3.fromRGB(40,40,40),
    })
    new("UICorner", {Parent = container, CornerRadius = UDim.new(0,6)})

    new("TextLabel", {
        Parent = container,
        Text = "Fly e teleport",
        Size = UDim2.new(1, -16, 0, 28),
        Position = UDim2.new(0,8,0,8),
        BackgroundTransparency = 1,
        TextColor3 = Color3.fromRGB(240,240,240),
        Font = Enum.Font.SourceSansBold,
        TextSize = 16,
        TextXAlignment = Enum.TextXAlignment.Left,
    })

    -- Fly: velocidade textbox + aplicar
    new("TextLabel", {
        Parent = container,
        Text = "Velocidade de voo:",
        Size = UDim2.new(0, 150, 0, 24),
        Position = UDim2.new(0,8,0,48),
        BackgroundTransparency = 1,
        TextColor3 = Color3.fromRGB(220,220,220),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
    })

    local flyBox = new("TextBox", {
        Parent = container,
        Name = "FlyBox",
        Text = "50",
        Size = UDim2.new(0, 120, 0, 26),
        Position = UDim2.new(0, 160, 0, 48),
        BackgroundColor3 = Color3.fromRGB(255,255,255),
        ClearTextOnFocus = false,
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = flyBox, CornerRadius = UDim.new(0,4)})

    local applyFly = new("TextButton", {
        Parent = container,
        Text = "Aplicar velocidade (Ativar fly)",
        Size = UDim2.new(0, 220, 0, 30),
        Position = UDim2.new(0, 8, 0, 86),
        BackgroundColor3 = Color3.fromRGB(60,60,60),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = applyFly, CornerRadius = UDim.new(0,6)})

    local disableFly = new("TextButton", {
        Parent = container,
        Text = "Desativar fly",
        Size = UDim2.new(0, 120, 0, 28),
        Position = UDim2.new(0, 236, 0, 86),
        BackgroundColor3 = Color3.fromRGB(60,60,60),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = disableFly, CornerRadius = UDim.new(0,6)})

    -- Espaço para forçar scroll
    new("Frame", {Parent = container, Size = UDim2.new(1, -16, 0, 140), Position = UDim2.new(0,8,0,126), BackgroundTransparency = 1})

    -- Teleport: distância + botões de direção
    new("TextLabel", {
        Parent = container,
        Text = "Distância de teleporte:",
        Size = UDim2.new(0, 200, 0, 24),
        Position = UDim2.new(0,8,0,276),
        BackgroundTransparency = 1,
        TextColor3 = Color3.fromRGB(220,220,220),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
    })
    local tpBox = new("TextBox", {
        Parent = container,
        Name = "TeleportBox",
        Text = "10",
        Size = UDim2.new(0, 120, 0, 26),
        Position = UDim2.new(0, 216, 0, 276),
        BackgroundColor3 = Color3.fromRGB(255,255,255),
        ClearTextOnFocus = false,
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = tpBox, CornerRadius = UDim.new(0,4)})

    local btnFrente = new("TextButton", {
        Parent = container,
        Text = "Frente",
        Size = UDim2.new(0, 88, 0, 28),
        Position = UDim2.new(0, 8, 0, 310),
        BackgroundColor3 = Color3.fromRGB(60,60,60),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = btnFrente, CornerRadius = UDim.new(0,6)})

    local btnTras = new("TextButton", {
        Parent = container,
        Text = "Atrás",
        Size = UDim2.new(0, 88, 0, 28),
        Position = UDim2.new(0, 104, 0, 310),
        BackgroundColor3 = Color3.fromRGB(60,60,60),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = btnTras, CornerRadius = UDim.new(0,6)})

    local btnDireita = new("TextButton", {
        Parent = container,
        Text = "Direita",
        Size = UDim2.new(0, 88, 0, 28),
        Position = UDim2.new(0, 200, 0, 310),
        BackgroundColor3 = Color3.fromRGB(60,60,60),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = btnDireita, CornerRadius = UDim.new(0,6)})

    local btnEsquerda = new("TextButton", {
        Parent = container,
        Text = "Esquerda",
        Size = UDim2.new(0, 88, 0, 28),
        Position = UDim2.new(0, 296, 0, 310),
        BackgroundColor3 = Color3.fromRGB(60,60,60),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
    })
    new("UICorner", {Parent = btnEsquerda, CornerRadius = UDim.new(0,6)})

    -- Funções utilitárias
    local function getCharacterParts()
        local char = player.Character
        if not char then return nil end
        local hrp = char:FindFirstChild("HumanoidRootPart") or char:FindFirstChild("Torso") or char:FindFirstChild("UpperTorso")
        local humanoid = char:FindFirstChildOfClass("Humanoid")
        return char, hrp, humanoid
    end

    -- FLY: implementamos um "fly" simples usando BodyVelocity e BodyGyro
    local flyActive = false
    local flyBV, flyBG
    local flySpeed = 50 -- default

    local function enableFly(speed)
        if flyActive then return end
        local char, hrp = getCharacterParts()
        if not char or not hrp then return end

        flyActive = true
        flySpeed = math.clamp(speed or tonumber(flyBox.Text) or 50, 0, 2000)

        -- BodyVelocity para mover; BodyGyro para manter orientação (opcional)
        flyBV = Instance.new("BodyVelocity")
        flyBV.MaxForce = Vector3.new(1e5, 1e5, 1e5)
        flyBV.Velocity = Vector3.new(0,0,0)
        flyBV.P = 1e4
        flyBV.Parent = hrp

        flyBG = Instance.new("BodyGyro")
        flyBG.MaxTorque = Vector3.new(4e5,4e5,4e5)
        flyBG.CFrame = hrp.CFrame
        flyBG.Parent = hrp

        -- Control: WASD + Space / LeftCtrl (move up/down)
        local moveVec = Vector3.new(0,0,0)
        local upDown = 0

        local function updateVelocity()
            if not flyBV or not hrp then return end
            local cam = workspace.CurrentCamera
            local forward = cam.CFrame.LookVector
            local right = cam.CFrame.RightVector
            local vel = (forward * moveVec.Z + right * moveVec.X) * flySpeed + Vector3.new(0, upDown * flySpeed, 0)
            flyBV.Velocity = vel
            if flyBG then flyBG.CFrame = CFrame.new(hrp.Position, hrp.Position + cam.CFrame.LookVector) end
        end

        -- Inputs
        local keys = {}
        local function onInputBegan(input, gp)
            if input.UserInputType == Enum.UserInputType.Keyboard then
                keys[input.KeyCode] = true
                -- update movement components
                moveVec = Vector3.new((keys[Enum.KeyCode.D] and 1 or 0) - (keys[Enum.KeyCode.A] and 1 or 0),
                                      0,
                                      (keys[Enum.KeyCode.W] and 1 or 0) - (keys[Enum.KeyCode.S] and 1 or 0))
                upDown = (keys[Enum.KeyCode.Space] and 1 or 0) - (keys[Enum.KeyCode.LeftControl] and 1 or 0)
            end
        end
        local function onInputEnded(input, gp)
            if input.UserInputType == Enum.UserInputType.Keyboard then
                keys[input.KeyCode] = nil
                moveVec = Vector3.new((keys[Enum.KeyCode.D] and 1 or 0) - (keys[Enum.KeyCode.A] and 1 or 0),
                                      0,
                                      (keys[Enum.KeyCode.W] and 1 or 0) - (keys[Enum.KeyCode.S] and 1 or 0))
                upDown = (keys[Enum.KeyCode.Space] and 1 or 0) - (keys[Enum.KeyCode.LeftControl] and 1 or 0)
            end
        end

        local conn1 = UserInputService.InputBegan:Connect(onInputBegan)
        local conn2 = UserInputService.InputEnded:Connect(onInputEnded)
        local conn3
        conn3 = RunService.RenderStepped:Connect(function()
            if not flyActive then
                if conn1 then conn1:Disconnect() end
                if conn2 then conn2:Disconnect() end
                if conn3 then conn3:Disconnect() end
                return
            end
            updateVelocity()
        end)

        -- guard references para desligar depois
        flyBV:SetAttribute("disconnect1", tostring(tick())) -- marcador, não usado diretamente
        -- salvamos conexões no objects para desconectar quando desativar
        flyBV:SetAttribute("conn1", 1)
        -- armazenar as conexões como campos (não ótimos, mas simples)
        flyBV.Parent = hrp
        flyBG.Parent = hrp

        -- guardando as conexões para limpeza
        flyBV:SetAttribute("___conn1", conn1 and true or false)
        -- armazenar no objeto container
        container._flyConns = {conn1, conn2, conn3}
    end

    local function disableFly()
        if not flyActive then return end
        flyActive = false
        local char, hrp = getCharacterParts()
        if hrp then
            for _, v in ipairs(container._flyConns or {}) do
                if v and typeof(v) == "RBXScriptConnection" then
                    pcall(function() v:Disconnect() end)
                end
            end
            if hrp:FindFirstChildOfClass("BodyVelocity") then hrp:FindFirstChildOfClass("BodyVelocity"):Destroy() end
            if hrp:FindFirstChildOfClass("BodyGyro") then hrp:FindFirstChildOfClass("BodyGyro"):Destroy() end
        end
        container._flyConns = nil
    end

    -- Teleport function: only frente/atrás/direita/esquerda relative to character look vector
    local function doTeleport(direction)
        local num = tonumber(tpBox.Text)
        if not num then
            tpBox.Text = "Valor inválido"
            return
        end
        local dist = math.clamp(num, 0, 1000) -- limita para evitar teleporte absurdo
        local char, hrp = getCharacterParts()
        if not hrp then return end
        local rootCFrame = hrp.CFrame
        local forward = rootCFrame.LookVector
        local right = rootCFrame.RightVector
        local offset = Vector3.new(0,0,0)
        if direction == "frente" then
            offset = forward * dist
        elseif direction == "tras" then
            offset = -forward * dist
        elseif direction == "direita" then
            offset = right * dist
        elseif direction == "esquerda" then
            offset = -right * dist
        end
        -- Mover o HRP mantendo a altura atual (somente XZ + Y mantém)
        local target = CFrame.new(hrp.Position + Vector3.new(offset.X, 0, offset.Z), hrp.Position + Vector3.new(offset.X, 0, offset.Z) + forward)
        -- tentativa de evitar colidir com objetos: colocamos HRP no target CFrame e ajustamos Y ligeiramente
        pcall(function()
            hrp.CFrame = target + Vector3.new(0, 0.5, 0)
        end)
    end

    -- Conexões dos botões
    applyFly.MouseButton1Click:Connect(function()
        local num = tonumber(flyBox.Text)
        if not num then
            flyBox.Text = "Valor inválido"
            return
        end
        enableFly(num)
    end)

    disableFly.MouseButton1Click:Connect(function()
        disableFly()
    end)

    btnFrente.MouseButton1Click:Connect(function() doTeleport("frente") end)
    btnTras.MouseButton1Click:Connect(function() doTeleport("tras") end)
    btnDireita.MouseButton1Click:Connect(function() doTeleport("direita") end)
    btnEsquerda.MouseButton1Click:Connect(function() doTeleport("esquerda") end)

    -- Atualiza campos ao spawnar personagem
    player.CharacterAdded:Connect(function()
        wait(0.2)
        local char, hrp, humanoid = getCharacterParts()
        if humanoid then
            flyBox.Text = tostring(50)
            tpBox.Text = tostring(10)
        end
    end)

    return container, {EnableFly = enableFly, DisableFly = disableFly, Teleport = doTeleport}
end

local flyContainer, flyAPI = createFlyTeleport(scroll)
flyContainer.Visible = false

-- Handler de opções (ativa painel)
local function clearPanels()
    flyContainer.Visible = false
end

optFlyTeleport.MouseButton1Click:Connect(function()
    clearPanels()
    flyContainer.Visible = true
    optFlyTeleport.BackgroundColor3 = Color3.fromRGB(70,70,70)
end)

-- abrir o menu com o botão flutuante
toggleBtn.MouseButton1Click:Connect(function()
    window.Visible = not window.Visible
end)

-- tornar janela arrastável pelo header
local dragging = false
local dragStartPos = Vector2.new()
local windowStartPos = window.Position

header.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStartPos = input.Position
        windowStartPos = window.Position
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if not dragging then return end
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        local delta = input.Position - dragStartPos
        local newPos = windowStartPos + UDim2.new(0, delta.X, 0, delta.Y)
        -- simples clamp para manter na tela
        local screen = workspace.CurrentCamera.ViewportSize
        local w, h = window.AbsoluteSize.X, window.AbsoluteSize.Y
        local px = math.clamp(newPos.X.Offset, 0, screen.X - w)
        local py = math.clamp(newPos.Y.Offset, 0, screen.Y - h)
        window.Position = UDim2.new(0, px, 0, py)
    end
end)

-- Ajusta CanvasSize conforme conteúdo
local function updateCanvas()
    local size = contentLayout.AbsoluteContentSize
    scroll.CanvasSize = UDim2.new(0, 0, 0, size.Y + 24)
end
contentLayout:GetPropertyChangedSignal("AbsoluteContentSize"):Connect(updateCanvas)
RunService.Heartbeat:Once(updateCanvas)

-- Limpeza ao sair do jogo (desliga fly se necessário)
player.AncestryChanged:Connect(function()
    -- quando o player for removido do jogo, garantir limpeza
    if not player:IsDescendantOf(game) then
        if flyAPI and flyAPI.DisableFly then pcall(flyAPI.DisableFly) end
    end
end)

-- DICA: as teclas W/A/S/D + Espaço + Ctrl controlam o voo enquanto o fly estiver ativo.
-- Use os botões "Desativar fly" para parar o voo.
