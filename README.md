# ALL IN ONE ADMIN UI (UI FRAMEWORK)

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UIS = game:GetService("UserInputService")
local SoundService = game:GetService("SoundService")
local plr = Players.LocalPlayer

-- KEY SYSTEM
local KEY = "Key7_90e9w"
local unlocked = false

-- SOUNDS
local function sfx(id,vol)
	local s=Instance.new("Sound")
	s.SoundId="rbxassetid://"..id
	s.Volume=vol or 1
	s.Parent=SoundService
	return s
end
local hoverS = sfx(9118823101,.6)
local clickS = sfx(9118828567,.8)

-- GUI
local gui = Instance.new("ScreenGui", plr.PlayerGui)
gui.Name = "AdminUI"
gui.Enabled = false

-- LEFT TOGGLE
local openBtn = Instance.new("TextButton", gui)
openBtn.Size = UDim2.fromScale(.07,.1)
openBtn.Position = UDim2.fromScale(0,.45)
openBtn.Text = "☰"
openBtn.TextScaled = true
openBtn.BackgroundColor3 = Color3.fromRGB(20,20,20)
openBtn.TextColor3 = Color3.fromRGB(255,80,80)
openBtn.Active=true
openBtn.Draggable=true
Instance.new("UICorner",openBtn)

-- MAIN FRAME
local frame = Instance.new("Frame", gui)
frame.Size = UDim2.fromScale(.45,.55)
frame.Position = UDim2.fromScale(-.6,.2)
frame.BackgroundColor3 = Color3.fromRGB(25,25,25)
frame.Active=true
frame.Draggable=true
Instance.new("UICorner",frame)

-- TITLE
local title = Instance.new("TextLabel", frame)
title.Size = UDim2.fromScale(1,.12)
title.BackgroundTransparency=1
title.Text="ADMIN PANEL"
title.TextScaled=true
title.TextColor3=Color3.fromRGB(255,90,90)

-- SCROLL
local scroll = Instance.new("ScrollingFrame", frame)
scroll.Position = UDim2.fromScale(0,.14)
scroll.Size = UDim2.fromScale(1,.86)
scroll.CanvasSize = UDim2.fromScale(0,2)
scroll.ScrollBarImageTransparency=.2
scroll.BackgroundTransparency=1

local layout = Instance.new("UIListLayout", scroll)
layout.Padding = UDim.new(0,10)

-- BUTTON MAKER
local function makeButton(txt)
	local b=Instance.new("TextButton",scroll)
	b.Size=UDim2.fromScale(.9,.08)
	b.Text=txt
	b.TextScaled=true
	b.BackgroundColor3=Color3.fromRGB(35,35,35)
	b.TextColor3=Color3.fromRGB(240,240,240)
	Instance.new("UICorner",b)

	b.MouseEnter:Connect(function()
		hoverS:Play()
		TweenService:Create(b,TweenInfo.new(.15),{
			BackgroundColor3=Color3.fromRGB(55,55,55)
		}):Play()
	end)

	b.MouseLeave:Connect(function()
		TweenService:Create(b,TweenInfo.new(.15),{
			BackgroundColor3=Color3.fromRGB(35,35,35)
		}):Play()
	end)

	b.MouseButton1Click:Connect(function()
		clickS:Play()
		TweenService:Create(b,TweenInfo.new(.1),{
			Size=b.Size-UDim2.fromScale(.01,.01)
		}):Play()
	end)

	return b
end

-- SAMPLE BUTTONS (HOOK YOUR LOGIC)
makeButton("Infinite Jump")
makeButton("Teleport Tool")
makeButton("Highlight Players")
makeButton("Aura Toggle")
makeButton("Freeze (UI)")
makeButton("Extra Tool")
makeButton("More Soon...")

-- OPEN / CLOSE
local open=false
openBtn.MouseButton1Click:Connect(function()
	gui.Enabled=true
	open=not open
	TweenService:Create(frame,TweenInfo.new(.4,Enum.EasingStyle.Quart),{
		Position=open and UDim2.fromScale(.28,.2) or UDim2.fromScale(-.6,.2)
	}):Play()
end)

-- KEY UNLOCK
UIS.InputBegan:Connect(function(i,g)
	if g then return end
	if i.KeyCode==Enum.KeyCode.RightShift then
		if unlocked then gui.Enabled=not gui.Enabled return end
		if KEY=="Key7_90e9w" then
			unlocked=true
			gui.Enabled=true
		end
	end
end)
