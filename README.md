local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local ContextActionService = game:GetService("ContextActionService")

local player = Players.LocalPlayer
local camera = workspace.CurrentCamera
local character = player.Character or player.CharacterAdded:Wait()
local hrp = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

local active = false
local camSpeed = 1.5
local camPos = camera.CFrame.Position
local camRot = Vector2.new()
local moving = Vector3.zero
local dragging = false

-- GUI
local gui = Instance.new("ScreenGui", game.CoreGui)
gui.Name = "FreecamGui_PIXOTE"

local toggle = Instance.new("TextButton", gui)
toggle.Size = UDim2.new(0, 140, 0, 40)
toggle.Position = UDim2.new(0, 20, 0, 200)
toggle.Text = "FREECAM: OFF"
toggle.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
toggle.TextColor3 = Color3.new(1,1,1)
toggle.Font = Enum.Font.GothamBold
toggle.TextSize = 14

local lookArea = Instance.new("Frame", gui)
lookArea.Size = UDim2.new(0.5, 0, 1, 0)
lookArea.Position = UDim2.new(0.5, 0, 0, 0)
lookArea.BackgroundTransparency = 1
lookArea.ZIndex = 10

-- Travar personagem
local function freezeCharacter()
	hrp.Anchored = true
	humanoid:ChangeState(Enum.HumanoidStateType.Physics)
end

local function unfreezeCharacter()
	hrp.Anchored = false
	humanoid:ChangeState(Enum.HumanoidStateType.Running)
end

-- Ativar/Desativar
toggle.MouseButton1Click:Connect(function()
	active = not active
	toggle.Text = "FREECAM: " .. (active and "ON" or "OFF")
	if active then
		camera.CameraType = Enum.CameraType.Scriptable
		camPos = camera.CFrame.Position
		freezeCharacter()
	else
		camera.CameraType = Enum.CameraType.Custom
		unfreezeCharacter()
	end
end)

-- Girar câmera com toque do lado direito
lookArea.InputChanged:Connect(function(input)
	if active and input.UserInputType == Enum.UserInputType.Touch then
		local delta = input.Delta
		camRot = camRot + Vector2.new(-delta.Y, -delta.X) * 0.003
	end
end)

-- Movimento com analógico esquerdo (Mobile)
ContextActionService:BindAction("MoveFreecam", function(_, inputState, inputObj)
	if not active then return end
	if inputState == Enum.UserInputState.Begin or inputState == Enum.UserInputState.Change then
		moving = Vector3.new(inputObj.Position.X, 0, -inputObj.Position.Y)
	elseif inputState == Enum.UserInputState.End then
		moving = Vector3.zero
	end
end, false, Enum.PlayerActions.CharacterForward)

-- Atualização da câmera
RunService.RenderStepped:Connect(function()
	if active then
		local rotCF = CFrame.Angles(0, camRot.Y, 0) * CFrame.Angles(camRot.X, 0, 0)
		if moving.Magnitude > 0.05 then
			local moveVec = rotCF:VectorToWorldSpace(moving.Unit) * camSpeed
			camPos += moveVec
		end
		camera.CFrame = CFrame.new(camPos) * rotCF
	end
end)
