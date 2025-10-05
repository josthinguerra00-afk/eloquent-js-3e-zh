
local gui = Instance.new("ScreenGui")
gui.Name = "BelicoHub"
gui.ResetOnSpawn = false
gui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 200, 0, 150)
frame.Position = UDim2.new(0.1, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
frame.BackgroundTransparency = 0.2
frame.BorderSizePixel = 0
frame.Parent = gui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "💥 Hub Bélico 💥"
title.TextColor3 = Color3.fromRGB(255, 0, 0)
title.Font = Enum.Font.GothamBold
title.TextSize = 18
title.Parent = frame

-- Variables
local savedPos = nil
local noclip = false
local player = game.Players.LocalPlayer
local char = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = char:WaitForChild("HumanoidRootPart")

-- Crear función de botón
local function createButton(name, text, order, callback)
	local btn = Instance.new("TextButton")
	btn.Name = name
	btn.Size = UDim2.new(1, -20, 0, 30)
	btn.Position = UDim2.new(0, 10, 0, 35 + (order * 35))
	btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	btn.TextColor3 = Color3.fromRGB(255, 255, 255)
	btn.Font = Enum.Font.GothamBold
	btn.TextSize = 16
	btn.Text = text
	btn.Parent = frame

	btn.MouseButton1Click:Connect(callback)
end

-- Botón Tp (guardar posición)
createButton("Tp", "Guardar posición", 0, function()
	if char and humanoidRootPart then
		savedPos = humanoidRootPart.CFrame
		game.StarterGui:SetCore("SendNotification", {
			Title = "Bélico 💥";
			Text = "Ubicación guardada.";
			Duration = 2;
		})
	end
end)

-- Botón Tp2 (teletransportar)
createButton("Tp2", "Ir a posición", 1, function()
	if savedPos and humanoidRootPart then
		humanoidRootPart.CFrame = savedPos
		game.StarterGui:SetCore("SendNotification", {
			Title = "Bélico 💥";
			Text = "Teletransportado con éxito.";
			Duration = 2;
		})
	else
		game.StarterGui:SetCore("SendNotification", {
			Title = "Bélico ⚠️";
			Text = "No hay ubicación guardada.";
			Duration = 2;
		})
	end
end)

-- Botón Tras (atravesar paredes)
createButton("Tras", "Traspasar pared", 2, function()
	noclip = not noclip
	game.StarterGui:SetCore("SendNotification", {
		Title = "Bélico 💥";
		Text = noclip and "Modo traspaso ACTIVADO." or "Modo traspaso DESACTIVADO.";
		Duration = 2;
	})

	while noclip do
		task.wait()
		for _, part in pairs(char:GetDescendants()) do
			if part:IsA("BasePart") then
				part.CanCollide = false
			end
		end
	end
end)