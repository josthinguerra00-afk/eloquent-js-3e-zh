local player = game.Players.LocalPlayer
local PlayerGui = player:WaitForChild("PlayerGui")

-- Crear GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "JossHub"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = PlayerGui

-- Crear marco principal
local Frame = Instance.new("Frame")
Frame.Size = UDim2.new(0, 230, 0, 220)
Frame.Position = UDim2.new(0.05, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Frame.Active = true
Frame.Draggable = true
Frame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = Frame

-- Barra de título
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 30)
TitleBar.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
TitleBar.Parent = Frame

local Title = Instance.new("TextLabel")
Title.Text = "🌀 Joss Hub"
Title.Size = UDim2.new(1, -30, 1, 0)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 16
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TitleBar

local CloseButton = Instance.new("TextButton")
CloseButton.Text = "✖"
CloseButton.Size = UDim2.new(0, 30, 1, 0)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 16
CloseButton.Parent = TitleBar

local savedPos = nil

-- Función crear botón
local function createButton(name, yPos, color, callback)
	local btn = Instance.new("TextButton")
	btn.Text = name
	btn.Size = UDim2.new(0.9, 0, 0, 40)
	btn.Position = UDim2.new(0.05, 0, 0, yPos)
	btn.BackgroundColor3 = color
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.Gotham
	btn.TextSize = 14
	btn.Parent = Frame

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = btn

	btn.MouseButton1Click:Connect(callback)
end

-- Botón Tp
createButton("📍 Tp (Guardar)", 40, Color3.fromRGB(70, 130, 180), function()
	local char = player.Character or player.CharacterAdded:Wait()
	local root = char:WaitForChild("HumanoidRootPart")
	savedPos = root.CFrame
	game.StarterGui:SetCore("SendNotification", {Title="Joss Hub", Text="Ubicación guardada ✔️", Duration=2})
end)

-- Botón Tp2
createButton("🚀 Tp2 (Ir)", 90, Color3.fromRGB(100, 180, 100), function()
	if savedPos then
		local char = player.Character or player.CharacterAdded:Wait()
		local root = char:WaitForChild("HumanoidRootPart")
		root.CFrame = savedPos
		game.StarterGui:SetCore("SendNotification", {Title="Joss Hub", Text="Teletransportado ✅", Duration=2})
	else
		game.StarterGui:SetCore("SendNotification", {Title="Joss Hub", Text="Primero guarda una ubicación ❌", Duration=2})
	end
end)

-- Botón TRAS
createButton("🧱 TRAS (Traspasar)", 140, Color3.fromRGB(200, 100, 100), function()
	local char = player.Character or player.CharacterAdded:Wait()
	for _, part in pairs(char:GetDescendants()) do
		if part:IsA("BasePart") then
			part.CanCollide = not part.CanCollide
		end
	end
	game.StarterGui:SetCore("SendNotification", {Title="Joss Hub", Text="Modo traspaso cambiado ⚙️", Duration=2})
end)

-- Cerrar hub
CloseButton.MouseButton1Click:Connect(function()
	Frame.Visible = false
end)