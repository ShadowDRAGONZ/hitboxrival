local UserInputService = game:GetService("UserInputService")

-- Rainbow Color Function
local function getRainbowColor(hueShift)
	local h = (tick() * 0.1 + hueShift) % 1
	return Color3.fromHSV(h, 1, 1)
end

local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
ScreenGui.Name = "CustomRainbowMenu"

-- Menu chính
local MenuFrame = Instance.new("Frame")
MenuFrame.Size = UDim2.new(0, 350, 0, 270)
MenuFrame.Position = UDim2.new(0.5, -175, 0.5, -135)
MenuFrame.AnchorPoint = Vector2.new(0.5, 0.5)
MenuFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
MenuFrame.BorderSizePixel = 0
MenuFrame.Visible = false
MenuFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner", MenuFrame)
UICorner.CornerRadius = UDim.new(0, 12)

-- Thanh tiêu đề
local TitleBar = Instance.new("TextLabel", MenuFrame)
TitleBar.Size = UDim2.new(1, 0, 0, 35)
TitleBar.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
TitleBar.Text = "Menu Chỉnh Style"
TitleBar.TextColor3 = Color3.new(1, 1, 1)
TitleBar.Font = Enum.Font.GothamBold
TitleBar.TextSize = 18

local TitleCorner = Instance.new("UICorner", TitleBar)
TitleCorner.CornerRadius = UDim.new(0, 12)

-- Kéo MenuFrame bằng TitleBar
local dragging = false
local dragStart, startPos

TitleBar.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = MenuFrame.Position
	end
end)

TitleBar.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		MenuFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

-- TextBox nhập style thủ công
local StyleBox = Instance.new("TextBox", MenuFrame)
StyleBox.Size = UDim2.new(0, 240, 0, 35)
StyleBox.Position = UDim2.new(0.5, -120, 0, 55)
StyleBox.PlaceholderText = "Nhập style (vd: Rin)"
StyleBox.TextColor3 = Color3.new(1, 1, 1)
StyleBox.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
StyleBox.Font = Enum.Font.Gotham
StyleBox.TextSize = 16
StyleBox.ClearTextOnFocus = false

local StyleCorner = Instance.new("UICorner", StyleBox)
StyleCorner.CornerRadius = UDim.new(0, 8)

-- Nút áp dụng style từ TextBox
local ApplyButton = Instance.new("TextButton", MenuFrame)
ApplyButton.Size = UDim2.new(0, 140, 0, 35)
ApplyButton.Position = UDim2.new(0.5, -70, 0, 100)
ApplyButton.Text = "Áp dụng Style"
ApplyButton.TextColor3 = Color3.new(1, 1, 1)
ApplyButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
ApplyButton.Font = Enum.Font.GothamBold
ApplyButton.TextSize = 16

local ApplyCorner = Instance.new("UICorner", ApplyButton)
ApplyCorner.CornerRadius = UDim.new(0, 8)

-- Danh sách style nhanh
local styleList = {"King", "Nagi", "Bachira", "Rin", "Sae", "Shidou"}
for i, name in ipairs(styleList) do
	local button = Instance.new("TextButton", MenuFrame)
	button.Size = UDim2.new(0, 100, 0, 30)
	button.Position = UDim2.new(0, 15 + ((i - 1) % 3) * 110, 0, 150 + math.floor((i - 1) / 3) * 40)
	button.Text = name
	button.TextColor3 = Color3.new(1, 1, 1)
	button.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
	button.Font = Enum.Font.GothamBold
	button.TextSize = 15
	
	local corner = Instance.new("UICorner", button)
	corner.CornerRadius = UDim.new(0, 6)

	button.MouseButton1Click:Connect(function()
		spawn(function()
			game:GetService("Players").LocalPlayer.Stats.Style.Value = name
		end)
	end)
end

-- Nút cầu vồng bật/tắt menu
local ToggleButton = Instance.new("TextButton", ScreenGui)
ToggleButton.Size = UDim2.new(0, 45, 0, 45)
ToggleButton.Position = UDim2.new(0, 30, 0.5, -22)
ToggleButton.Text = "☢"
ToggleButton.TextColor3 = Color3.new(1, 1, 1)
ToggleButton.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 20
ToggleButton.Active = true
ToggleButton.Draggable = true

local BtnCorner = Instance.new("UICorner", ToggleButton)
BtnCorner.CornerRadius = UDim.new(1, 0)

-- Rainbow hiệu ứng cho nút toggle
spawn(function()
	while true do
		ToggleButton.BackgroundColor3 = getRainbowColor(0)
		wait()
	end
end)

-- Toggle menu hiển thị
ToggleButton.MouseButton1Click:Connect(function()
	MenuFrame.Visible = not MenuFrame.Visible
end)

-- Áp dụng style từ TextBox
ApplyButton.MouseButton1Click:Connect(function()
	local styleText = StyleBox.Text
	if styleText ~= "" then
		spawn(function()
			game:GetService("Players").LocalPlayer.Stats.Style.Value = styleText
		end)
	end
end)
