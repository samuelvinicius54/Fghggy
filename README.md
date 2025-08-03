-- LocalScript (StarterPlayerScripts)

local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer

function createESPBox(player)
	if player == localPlayer then return end
	local box = Drawing.new("Square")
	box.Visible = false
	box.Color = Color3.fromRGB(255, 0, 0)
	box.Thickness = 2
	box.Transparency = 1
	box.Filled = false

	RunService.RenderStepped:Connect(function()
		if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
			local root = player.Character.HumanoidRootPart
			local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(root.Position)
			if onScreen then
				local size = Vector2.new(50, 100) -- Tamanho da caixa
				box.Size = size
				box.Position = Vector2.new(pos.X - size.X/2, pos.Y - size.Y/2)
				box.Visible = true
			else
				box.Visible = false
			end
		else
			box.Visible = false
		end
	end)
end

for _, player in pairs(Players:GetPlayers()) do
	createESPBox(player)
end

Players.PlayerAdded:Connect(createESPBox)
