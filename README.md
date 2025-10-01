-- Menu Boost FPS para Roblox (LocalScript)
-- Adiciona opção "Reduzir Gráficos" que diminui qualidade visual

local player = game:GetService("Players").LocalPlayer
local UIS = game:GetService("UserInputService")
local Lighting = game:GetService("Lighting")

local RAIO_IMAGE_ID = "rbxassetid://11797037673"

-- Guardar estado original dos efeitos para restaurar corretamente
local originalEffects = {}
local graphicsReduced = false
local originalLighting = {}

local function saveEffects()
    originalEffects = {}
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
            originalEffects[obj] = obj.Enabled
        end
        if obj:IsA("Decal") then
            originalEffects[obj] = obj.Transparency
        end
    end
    originalLighting = {
        GlobalShadows = Lighting.GlobalShadows,
        FogEnd = Lighting.FogEnd,
        FogStart = Lighting.FogStart,
        Brightness = Lighting.Brightness,
        Ambient = Lighting.Ambient,
        ColorShift_Bottom = Lighting.ColorShift_Bottom,
        ColorShift_Top = Lighting.ColorShift_Top
    }
end

local function setBoostFPS(on)
    if on then
        saveEffects()
        Lighting.GlobalShadows = false
        Lighting.FogEnd = 100000
        Lighting.Brightness = 1
        for obj,_ in pairs(originalEffects) do
            if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
                obj.Enabled = false
            end
            if obj:IsA("Decal") then
                obj.Transparency = 1
            end
        end
    else
        -- Restaurar estado original dos efeitos
        if originalLighting then
            Lighting.GlobalShadows = originalLighting.GlobalShadows
            Lighting.FogEnd = originalLighting.FogEnd
            Lighting.Brightness = originalLighting.Brightness
            Lighting.FogStart = originalLighting.FogStart
            Lighting.Ambient = originalLighting.Ambient
            Lighting.ColorShift_Bottom = originalLighting.ColorShift_Bottom
            Lighting.ColorShift_Top = originalLighting.ColorShift_Top
        end
        for obj, val in pairs(originalEffects) do
            if typeof(obj) == "Instance" and obj.Parent then
                if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
                    obj.Enabled = val
                end
                if obj:IsA("Decal") then
                    obj.Transparency = val
                end
            end
        end
    end
end

-- Reduzir Gráficos
local function setGraphicsReduced(on)
    graphicsReduced = on
    if on then
        saveEffects()
        -- Lighting configs para gráficos mínimos
        Lighting.Brightness = 1
        Lighting.FogEnd = 500
        Lighting.FogStart = 100
        Lighting.GlobalShadows = false
        Lighting.Ambient = Color3.fromRGB(120,120,120)
        Lighting.ColorShift_Bottom = Color3.fromRGB(0,0,0)
        Lighting.ColorShift_Top = Color3.fromRGB(0,0,0)
        -- Desativa todos os efeitos visuais
        for obj,_ in pairs(originalEffects) do
            if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
                obj.Enabled = false
            end
            if obj:IsA("Decal") then
                obj.Transparency = 1
            end
        end
    else
        -- Restaurar Lighting
        if originalLighting then
            Lighting.Brightness = originalLighting.Brightness
            Lighting.FogEnd = originalLighting.FogEnd
            Lighting.FogStart = originalLighting.FogStart
            Lighting.GlobalShadows = originalLighting.GlobalShadows
            Lighting.Ambient = originalLighting.Ambient
            Lighting.ColorShift_Bottom = originalLighting.ColorShift_Bottom
            Lighting.ColorShift_Top = originalLighting.ColorShift_Top
        end
        -- Restaurar efeitos
        for obj, val in pairs(originalEffects) do
            if typeof(obj) == "Instance" and obj.Parent then
                if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Fire") or obj:IsA("Smoke") or obj:IsA("Sparkles") then
                    obj.Enabled = val
                end
                if obj:IsA("Decal") then
                    obj.Transparency = val
                end
            end
        end
    end
end

-- Painel
local painelName = "BoostFPSPanel"
local painelGui = Instance.new("ScreenGui")
painelGui.Name = painelName
painelGui.Parent = player:WaitForChild("PlayerGui")

local menuFrame = Instance.new("Frame", painelGui)
menuFrame.Size = UDim2.new(0, 320, 0, 240)
menuFrame.Position = UDim2.new(0.5, -160, 0.5, -120)
menuFrame.BackgroundColor3 = Color3.fromRGB(20, 22, 28)
menuFrame.Active = true
menuFrame.Draggable = true
menuFrame.Visible = true
local menuCorner = Instance.new("UICorner", menuFrame)
menuCorner.CornerRadius = UDim.new(0, 14)

-- Bolinha branca com raio centralizado
local bolaBtn = Instance.new("ImageButton", painelGui)
bolaBtn.Size = UDim2.new(0, 68, 0, 68)
bolaBtn.Position = UDim2.new(0, 30, 0.5, -34)
bolaBtn.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
bolaBtn.Image = ""
bolaBtn.Visible = false
bolaBtn.AutoButtonColor = true
bolaBtn.Active = true
bolaBtn.ZIndex = 10
local bolaCorner = Instance.new("UICorner", bolaBtn)
bolaCorner.CornerRadius = UDim.new(1,0)
local raioImg = Instance.new("ImageLabel", bolaBtn)
raioImg.Size = UDim2.new(0, 40, 0, 40)
raioImg.Position = UDim2.new(0.5, -20, 0.5, -20)
raioImg.BackgroundTransparency = 1
raioImg.Image = RAIO_IMAGE_ID
raioImg.ZIndex = 11

bolaBtn.MouseButton1Click:Connect(function()
    menuFrame.Visible = true
    bolaBtn.Visible = false
end)

local titleLabel = Instance.new("TextLabel", menuFrame)
titleLabel.Size = UDim2.new(1, 0, 0, 38)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "BOOST FPS MENU"
titleLabel.TextColor3 = Color3.fromRGB(255,255,255)
titleLabel.Font = Enum.Font.GothamBlack
titleLabel.TextSize = 24

-- FPS BOOST Toggle
local boostActive = false
local boostBtn = Instance.new("TextButton", menuFrame)
boostBtn.Size = UDim2.new(0, 220, 0, 36)
boostBtn.Position = UDim2.new(0, 50, 0, 55)
boostBtn.BackgroundColor3 = Color3.fromRGB(60,160,80)
boostBtn.Text = "FPS Boost: OFF"
boostBtn.TextColor3 = Color3.fromRGB(255,255,255)
boostBtn.Font = Enum.Font.GothamBlack
boostBtn.TextSize = 18
local boostCorner = Instance.new("UICorner", boostBtn)
boostCorner.CornerRadius = UDim.new(1,0)

boostBtn.MouseButton1Click:Connect(function()
    boostActive = not boostActive
    boostBtn.Text = boostActive and "FPS Boost: ON" or "FPS Boost: OFF"
    boostBtn.BackgroundColor3 = boostActive and Color3.fromRGB(120,220,80) or Color3.fromRGB(60,160,80)
    setBoostFPS(boostActive)
end)

-- Reduzir Gráficos Toggle
local graphicsBtn = Instance.new("TextButton", menuFrame)
graphicsBtn.Size = UDim2.new(0, 220, 0, 36)
graphicsBtn.Position = UDim2.new(0, 50, 0, 100)
graphicsBtn.BackgroundColor3 = Color3.fromRGB(180,120,50)
graphicsBtn.Text = "Reduzir Gráficos: OFF"
graphicsBtn.TextColor3 = Color3.fromRGB(255,255,255)
graphicsBtn.Font = Enum.Font.GothamBlack
graphicsBtn.TextSize = 18
local graphicsCorner = Instance.new("UICorner", graphicsBtn)
graphicsCorner.CornerRadius = UDim.new(1,0)

graphicsBtn.MouseButton1Click:Connect(function()
    graphicsReduced = not graphicsReduced
    graphicsBtn.Text = graphicsReduced and "Reduzir Gráficos: ON" or "Reduzir Gráficos: OFF"
    graphicsBtn.BackgroundColor3 = graphicsReduced and Color3.fromRGB(220,180,80) or Color3.fromRGB(180,120,50)
    setGraphicsReduced(graphicsReduced)
end)

-- Mostrar FPS Toggle
local showFPS = false
local fpsBtn = Instance.new("TextButton", menuFrame)
fpsBtn.Size = UDim2.new(0, 220, 0, 36)
fpsBtn.Position = UDim2.new(0, 50, 0, 145)
fpsBtn.BackgroundColor3 = Color3.fromRGB(60,80,160)
fpsBtn.Text = "Mostrar FPS: OFF"
fpsBtn.TextColor3 = Color3.fromRGB(255,255,255)
fpsBtn.Font = Enum.Font.GothamBlack
fpsBtn.TextSize = 18
local fpsCorner = Instance.new("UICorner", fpsBtn)
fpsCorner.CornerRadius = UDim.new(1,0)

fpsBtn.MouseButton1Click:Connect(function()
    showFPS = not showFPS
    fpsBtn.Text = showFPS and "Mostrar FPS: ON" or "Mostrar FPS: OFF"
    fpsBtn.BackgroundColor3 = showFPS and Color3.fromRGB(120,120,220) or Color3.fromRGB(60,80,160)
end)

-- Fechar menu
local closeBtn = Instance.new("TextButton", menuFrame)
closeBtn.Size = UDim2.new(0, 36, 0, 36)
closeBtn.Position = UDim2.new(1, -44, 0, 10)
closeBtn.BackgroundColor3 = Color3.fromRGB(60, 20, 20)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255,255,255)
closeBtn.Font = Enum.Font.GothamBlack
closeBtn.TextSize = 22
closeBtn.AutoButtonColor = false
local closeCorner = Instance.new("UICorner", closeBtn)
closeCorner.CornerRadius = UDim.new(1,0)
closeBtn.MouseButton1Click:Connect(function()
    menuFrame.Visible = false
    bolaBtn.Visible = true
end)

-- Atalho LeftCtrl para abrir/fechar
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.LeftControl then
        local abrir = not menuFrame.Visible
        menuFrame.Visible = abrir
        bolaBtn.Visible = not abrir
    end
end)

-- FPS Viewer
local lastTime = tick()
local frameCount = 0
local currentFPS = 60
local fpsLabel = Instance.new("TextLabel", painelGui)
fpsLabel.Name = "FPSLabel"
fpsLabel.Size = UDim2.new(0, 100, 0, 28)
fpsLabel.Position = UDim2.new(0, 10, 0, 10)
fpsLabel.BackgroundTransparency = 0.5
fpsLabel.BackgroundColor3 = Color3.fromRGB(0,0,0)
fpsLabel.TextColor3 = Color3.fromRGB(255,255,255)
fpsLabel.Font = Enum.Font.GothamBlack
fpsLabel.TextSize = 16
fpsLabel.Visible = false

game:GetService("RunService").RenderStepped:Connect(function()
    frameCount = frameCount + 1
    if tick() - lastTime >= 1 then
        currentFPS = frameCount
        frameCount = 0
        lastTime = tick()
    end
    fpsLabel.Text = "FPS: "..currentFPS
    fpsLabel.Visible = showFPS
end)
