#local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

local LocalPlayer = Players.LocalPlayer
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")

local MAX_DISTANCE = 100

local function GetClosestHead()
	local closestHead = nil
	local closestDistance = MAX_DISTANCE

	for _, player in ipairs(Players:GetPlayers()) do
		if player ~= LocalPlayer and player.Character then
			local head = player.Character:FindFirstChild("Head")
			local humanoid = player.Character:FindFirstChild("Humanoid")

			if head and humanoid and humanoid.Health > 0 then
				local distance = (head.Position - HumanoidRootPart.Position).Magnitude

				if distance < closestDistance then
					closestDistance = distance
					closestHead = head
				end
			end
		end
	end

	return closestHead
end

RunService.RenderStepped:Connect(function()
	local head = GetClosestHead()

	if head then
		HumanoidRootPart.CFrame = CFrame.lookAt(
			HumanoidRootPart.Position,
			head.Position
		)
	end
end)
