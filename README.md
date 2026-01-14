-- hog blue - sistema REAL para jogo próprio

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local player = Players.LocalPlayer

-- =========================
-- CONFIG
-- =========================
local SPEED_MULT = 100
local JUMP_MULT = 10

-- =========================
-- CORES
-- =========================
local AMARELO = Color3.fromRGB(255, 215, 0)
local PRETO = Color3.fromRGB(15, 15, 15)
local CINZA = Color3.fromRGB(45, 45, 45)

-- =========================
-- GUI
-- =========================
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.ResetOnSpawn = false
gui.Name = "hog_blue"

local painel = Instance.new("Frame", gui)
painel.Size = UDim2.new(0, 260, 0, 320)
painel.Position = UDim2.new(0.5, -130, 0.5, -160)
painel.BackgroundColor3 = PRETO
painel.BorderSizePixel = 0
Instance.new("UICorner", painel).CornerRadius = UDim.new(0, 14)

local titulo = Instance.new("TextLabel", painel)
titulo.Size = UDim2.new(1, 0, 0, 45)
titulo.BackgroundColor3 = AMARELO
titulo.Text = "hog blue"
titulo.TextColor3 = PRETO
titulo.Font = Enum.Font.GothamBlack
titulo.TextSize = 20
Instance.new("UICorner", titulo).CornerRadius = UDim.new(0, 14)

local function botao(texto, y)
	local b = Instance.new("TextButton", painel)
	b.Size = UDim2.new(1, -30, 0, 40)
	b.Position = UDim2.new(0, 15, 0, y)
	b.BackgroundColor3 = CINZA
	b.TextColor3 = AMARELO
	b.Font = Enum.Font.GothamBold
	b.TextSize = 15
	b.Text = texto
	Instance.new("UICorner", b).CornerRadius = UDim.new(0, 10)
	return b
end

local btnSpeed = botao("⚡ SPEED", 60)
local btnJump = botao("🦘 PULO", 110)
local btnInvis = botao("👻 INVISÍVEL", 160)
local btnAntiLag = botao("🧹 ANTI LAG", 210)
local btnSteal = botao("🧠 COLETAR BRAINROT", 260)

-- =========================
-- FUNÇÕES REAIS
-- =========================

local function humanoid()
	local char = player.Character
	if char then
		return char:FindFirstChildOfClass("Humanoid")
	end
end

-- SPEED
btnSpeed.MouseButton1Click:Connect(function()
	local h = humanoid()
	if h then
		h.WalkSpeed = 16 * SPEED_MULT
	end
end)

-- PULO
btnJump.MouseButton1Click:Connect(function()
	local h = humanoid()
	if h then
		h.JumpPower = 50 * JUMP_MULT
	end
end)

-- INVISIBILIDADE REAL
btnInvis.MouseButton1Click:Connect(function()
	local char = player.Character
	if not char then return end
	for _, v in pairs(char:GetDescendants()) do
		if v:IsA("BasePart") then
			v.Transparency = 1
			v.CanCollide = false
		end
	end
end)

-- ANTI LAG REAL
btnAntiLag.MouseButton1Click:Connect(function()
	for _, v in pairs(workspace:GetDescendants()) do
		if v:IsA("ParticleEmitter")
		or v:IsA("Trail")
		or v:IsA("Smoke")
		or v:IsA("Fire") then
			v.Enabled = false
		end
	end
end)

-- COLETAR BRAINROT (REAL)
btnSteal.MouseButton1Click:Connect(function()
	local pasta = workspace:FindFirstChild("Brainrots")
	if not pasta then return end

	for _, brainrot in pairs(pasta:GetChildren()) do
		if brainrot:IsA("BasePart") then
			brainrot:Destroy()
		end
	end
end)

-- =========================
-- MOBILE: ARRASTAR PAINEL
-- =========================
local dragging = false
local dragStart, startPos

painel.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = painel.Position
	end
end)

painel.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.Touch then
		dragging = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.Touch then
		local delta = input.Position - dragStart
		painel.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)
