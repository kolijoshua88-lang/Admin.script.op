# ALL IN ONE ADMIN UI

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local SoundService = game:GetService("SoundService")
local Debris = game:GetService("Debris")

local lp = Players.LocalPlayer

-- 🔑 KEY
local KEY = "Key7_90e9w"
local unlocked = false

-- 🔊 SFX SYSTEM
local function sfx(id, vol)
	local s = Instance.new("Sound")
	s.SoundId = "rbxassetid://"..id
	s.Volume = vol or 1
	s.Parent = SoundService
	return s
end

local SFX = {
	Hover = sfx(9118823101,0.5),
	Click = sfx(9118828567,0.7),
	Toggle = sfx(9113430510,0.8),
	TP = sfx(9125402735,1),
	Aura = sfx(5419098672,0.9),
}

-- 🎨 GUI
local gui = Instance.new("ScreenGui", lp.PlayerGui)
gui.Name = "AllInOneAdmin"
gui.Enabled = false
gui.ResetOnSpawn = false

-- ☰ LEFT BUTTON
local openBtn = Instance.new("TextButton", gui)
openBtn.Size = UDim2.fromScale(0.07,0.1)
openBtn.Position = UDim2.fromScale(0,0.45)
openBtn.Text = "☰"
openBtn.TextScaled = true
openBtn.Font = Enum.Font.GothamBold
openBtn.BackgroundColor3 = Color3.fromRGB(20,20,20)
openBtn.TextColor3 = Color3.fromRGB(255,80,80)
openBtn.Active = true
openBtn.Draggable = true
Instance.new("UICorner", openBtn)

-- 📦 MAIN PANEL
local panel = Instance.new("Frame", gui)
panel.Size = UDim2.fromScale(0.45,0.65)
panel.Position = UDim2.fromScale(-0.6,0.18)
panel.BackgroundColor3 = Color3.fromRGB(25,25,25)
panel.Active = true
panel.Draggable = true
Instance.new("UICorner", panel)

-- 🏷 TITLE
local title = Instance.new("TextLabel", panel)
title.Size = UDim2.fromScale(1,0.1)
title.BackgroundTransparency = 1
title.Text = "ADMIN PANEL"
title.TextScaled = true
title.Font = Enum.Font.GothamBold
title.TextColor3 = Color3.fromRGB(255,90,90)

-- 📜 SCROLL
local scroll = Instance.new("ScrollingFrame", panel)
scroll.Position = UDim2.fromScale(0,0.12)
scroll.Size = UDim2.fromScale(1,0.88)
scroll.CanvasSize = UDim2.fromScale(0,2)
scroll.ScrollBarImageTransparency = 0.2
scroll.BackgroundTransparency = 1

local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0,8)

-- 🔘 BUTTON MAKER (ANIM + SFX)
local function makeButton(text, callback)
	local b = Instance.new("TextButton", scroll)
	b.Size = UDim2.fromScale(0.9,0.08)
	b.Text = text
	b.TextScaled = true
	b.Font = Enum.Font.GothamBold
	b.BackgroundColor3 = Color3.fromRGB(40,40,40)
	b.TextColor3 = Color3.fromRGB(240,240,240)
	Instance.new("UICorner", b)

	b.MouseEnter:Connect(function()
		SFX.Hover:Play()
		TweenService:Create(b,TweenInfo.new(0.15),{
			BackgroundColor3 = Color3.fromRGB(60,60,60)
		}):Play()
	end)

	b.MouseLeave:Connect(function()
		TweenService:Create(b,TweenInfo.new(0.15),{
			BackgroundColor3 = Color3.fromRGB(40,40,40)
		}):Play()
	end)

	b.MouseButton1Click:Connect(function()
		SFX.Click:Play()
		callback()
	end)

	return b
end

-- ♾ INFINITE JUMP
local infJump = false
makeButton("♾ Infinite Jump", function()
	infJump = not infJump
	SFX.Toggle:Play()
end)

UIS.JumpRequest:Connect(function()
	if infJump and lp.Character and lp.Character:FindFirstChild("Humanoid") then
		lp.Character.Humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
	end
end)

-- 🔥 FIRE SELF
makeButton("🔥 Fire Self", function()
	SFX.Aura:Play()
	local char = lp.Character
	if not char then return end
	local hrp = char:FindFirstChild("HumanoidRootPart")
	if not hrp then return end

	local fire = Instance.new("Fire", hrp)
	fire.Size = 15
	fire.Heat = 20
	Debris:AddItem(fire,5)
end)

-- 🛠 TELEPORT TOOL (WITH SOUND)
makeButton("🛠 Teleport Tool", function()
	local tool = Instance.new("Tool")
	tool.Name = "Teleport"
	tool.RequiresHandle = false

	tool.Activated:Connect(function()
		local char = lp.Character
		if not char then return end
		local hrp = char:FindFirstChild("HumanoidRootPart")
		if not hrp then return end

		local mouse = lp:GetMouse()
		if not mouse.Hit then return end

		local s = SFX.TP:Clone()
		s.Parent = hrp
		s:Play()
		Debris:AddItem(s,2)

		hrp.CFrame = CFrame.new(mouse.Hit.Position + Vector3.new(0,3,0))
	end)

	tool.Parent = lp.Backpack
end)

-- 📂 OPEN / CLOSE
local open = false
openBtn.MouseButton1Click:Connect(function()
	gui.Enabled = true
	open = not open
	TweenService:Create(panel,TweenInfo.new(0.4,Enum.EasingStyle.Quart),{
		Position = open and UDim2.fromScale(0.28,0.18) or UDim2.fromScale(-0.6,0.18)
	}):Play()
end)

-- 🔑 KEY TOGGLE
UIS.InputBegan:Connect(function(i,g)
	if g then return end
	if i.KeyCode == Enum.KeyCode.RightShift then
		if not unlocked then
			if KEY == "Key7_90e9w" then
				unlocked = true
				gui.Enabled = true
			end
		else
			open = not open
			TweenService:Create(panel,TweenInfo.new(0.35),{
				Position = open and UDim2.fromScale(0.28,0.18) or UDim2.fromScale(-0.6,0.18)
			}):Play()
		end
	end
end)
