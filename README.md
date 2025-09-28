local lp = game.Players.LocalPlayer
repeat wait() until lp.Character and lp.Character:FindFirstChild("HumanoidRootPart")

local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "InterfaceFELIPE"

local click = Instance.new("Sound", gui)
click.SoundId = "rbxassetid://12221967"
click.Volume = 1

local abrir = Instance.new("TextButton", gui)
abrir.Size = UDim2.new(0, 120, 0, 40)
abrir.Position = UDim2.new(0, 10, 1, -50)
abrir.Text = "📂 Abrir Painel"
abrir.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
abrir.TextColor3 = Color3.new(1, 1, 1)
abrir.Font = Enum.Font.GothamBold
abrir.TextSize = 18
abrir.Visible = false

local painel = Instance.new("Frame", gui)
painel.Size = UDim2.new(0, 300, 0, 420)
painel.Position = UDim2.new(0.5, -150, 0.5, -210)
painel.BackgroundColor3 = Color3.fromRGB(255, 140, 0)
painel.AnchorPoint = Vector2.new(0.5, 0.5)
painel.Active = true
painel.Draggable = true
painel.BackgroundTransparency = 1

game:GetService("TweenService"):Create(painel, TweenInfo.new(0.5), {
    BackgroundTransparency = 0
}):Play()

local fechar = Instance.new("TextButton", painel)
fechar.Size = UDim2.new(0, 30, 0, 30)
fechar.Position = UDim2.new(0, -15, 0, -15)
fechar.Text = "❌"
fechar.TextColor3 = Color3.fromRGB(255, 0, 0)
fechar.BackgroundTransparency = 1
fechar.Font = Enum.Font.GothamBold
fechar.TextSize = 24
fechar.MouseButton1Click:Connect(function()
    painel.Visible = false
    abrir.Visible = true
    click:Play()
end)

abrir.MouseButton1Click:Connect(function()
    painel.Visible = true
    abrir.Visible = false
    click:Play()
end)

local aba = Instance.new("TextButton", painel)
aba.Size = UDim2.new(0, 280, 0, 30)
aba.Position = UDim2.new(0, 10, 0, 10)
aba.Text = "⚡ Poderes"
aba.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
aba.TextColor3 = Color3.new(1, 1, 1)
aba.Font = Enum.Font.GothamBold
aba.TextSize = 16

local frame = Instance.new("Frame", painel)
frame.Size = UDim2.new(1, -20, 1, -60)
frame.Position = UDim2.new(0, 10, 0, 50)
frame.BackgroundTransparency = 1
frame.Visible = true

aba.MouseButton1Click:Connect(function()
    frame.Visible = true
    click:Play()
end)

local function criarBotao(txt, cor, ordem, func)
    local b = Instance.new("TextButton", frame)
    b.Size = UDim2.new(1, -20, 0, 40)
    b.Position = UDim2.new(0, 10, 0, 10 + (ordem - 1) * 50)
    b.Text = txt
    b.BackgroundColor3 = cor
    b.TextColor3 = Color3.new(0, 0, 0)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 18
    b.MouseButton1Click:Connect(function()
        func()
        click:Play()
    end)
end

-- 🏃‍♂️ Velocidade 250
criarBotao("🏃‍♂️ Velocidade 250", Color3.fromRGB(255,255,0), 1, function()
    lp.Character.Humanoid.WalkSpeed = 250
end)

-- 🧱 Plataforma Inteligente
local ativa, plataforma, con, altura = false, nil, nil, nil
criarBotao("🧱 Plataforma Inteligente", Color3.fromRGB(0,255,255), 2, function()
    if not ativa then
        ativa = true
        plataforma = Instance.new("Part", workspace)
        plataforma.Size = Vector3.new(6,1,6)
        plataforma.Anchored = true
        plataforma.CanCollide = true
        plataforma.Material = Enum.Material.Neon
        plataforma.Color = Color3.fromRGB(0,255,255)
        altura = lp.Character.HumanoidRootPart.Position.Y - 3
        con = game:GetService("RunService").Heartbeat:Connect(function()
            local pos = lp.Character.HumanoidRootPart.Position
            if pos.Y > altura + 5 then altura = pos.Y - 3 end
            plataforma.Position = Vector3.new(pos.X, altura, pos.Z)
        end)
    else
        ativa = false
        if con then con:Disconnect() end
        if plataforma then plataforma:Destroy() end
    end
end)

-- 👁️ Visão de Jogadores
criarBotao("👁️ Visão de Jogadores", Color3.fromRGB(100,255,100), 3, function()
    for _, p in pairs(game.Players:GetPlayers()) do
        if p ~= lp and p.Character and p.Character:FindFirstChild("HumanoidRootPart") then
            for _, part in pairs(p.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.Transparency = 0
                    part.Material = Enum.Material.SmoothPlastic
                end
            end
            if not p.Character:FindFirstChild("NomeFlutuante") then
                local tag = Instance.new("BillboardGui", p.Character)
                tag.Name = "NomeFlutuante"
                tag.Size = UDim2.new(0, 200, 0, 50)
                tag.Adornee = p.Character:FindFirstChild("Head")
                tag.AlwaysOnTop = true
                local texto = Instance.new("TextLabel", tag)
                texto.Size = UDim2.new(1, 0, 1, 0)
                texto.BackgroundTransparency = 1
                texto.Text = p.Name
                texto.TextColor3 = Color3.new(1, 1, 1)
                texto.Font = Enum.Font.GothamBold
                texto.TextScaled = true
            end
        end
    end
end)
