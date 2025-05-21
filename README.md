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
end)ENGAGE

1. Listen and read about the writer of the most famous fictional detective, Sherlock Holmes

20

Arthur Conan Doyle was born on May 22, 1859 in Edinburgh, Scotland into a prosperous Irish family

At school, Arthur developed a strong interest in the books written by Sir Walter Scott and Edgar Allan Poe. He attended Edinburgh University. He studied to be a doctor, earning his degree in 1881. He then settled in Portsmouth on the English south coast With very few patients, he divided his time between medicine and writing

Sherlock Holmes made his first appearance in A study in scarlet, published in Beeton's Christmas Annual in 1887. Its success encouraged Conan Doyle to write more stories involving Holmes. He visited police museums to gain inspiration for his stories. He was really interested in turning real life crimes into a really good story. But, in 1893, Conan Doyle killed off Holmes, hoping to concentrate on more serious writing. A public protest later made him resurrect Holmes

Conan Doyle also wrote a number of other novels, including The lost world and various non-fictional works. These included a pamphlet justifying Britain's involvement in the Boes War and histories of the First World War, in which his son, brother and two of his nephews were killed. After his son's death, he became very interested in spiritualism

Doyle ran for parliament twice, 1900 and 1906. Although he received a respectable number of votes both times, he was not elected

Conan Doyle died of a heart attack on July 7, 1930. He collapsed in his gardere clutching his heart with one hand and holding a flower in the other. His last words were te his wife. He whispered to her "You are wonderful

Sell Anthur Conan Doyle (1859-1930) BBC Available at www.ibc.co.uk/hdon Rotork figures/conan or arthur doyle chand. Accessed Nov 201

2. What/who do these words refer to?

a) his

bj his

c) Its-

d) These

e) her-
