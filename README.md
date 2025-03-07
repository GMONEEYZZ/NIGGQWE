local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local UserInputService = game:GetService("UserInputService")

local gui = CoreGui:FindFirstChild("ThugGuiScreen")
if gui then gui:Destroy() end

local ThugGuiScreen = Instance.new("ScreenGui")
ThugGuiScreen.Name = "ThugGuiScreen"
ThugGuiScreen.Parent = CoreGui

local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Parent = ThugGuiScreen
Main.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
Main.Position = UDim2.new(0.5, -250, 0.5, -175)
Main.Size = UDim2.new(0, 500, 0, 350)
Main.ClipsDescendants = true
Main.Active = true

local GradientBG = Instance.new("Frame")
GradientBG.Name = "GradientBG"
GradientBG.Parent = Main
GradientBG.BackgroundTransparency = 0.8
GradientBG.Size = UDim2.new(2, 0, 2, 0)
GradientBG.Position = UDim2.new(-0.5, 0, -0.5, 0)

local UIGradient = Instance.new("UIGradient")
UIGradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 100)),
    ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 150, 255)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 0, 100))
})
UIGradient.Parent = GradientBG

local gradientRotation = 0
game:GetService("RunService").RenderStepped:Connect(function()
    gradientRotation = gradientRotation + 1
    UIGradient.Rotation = gradientRotation
end)

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 10)
MainCorner.Parent = Main

local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Parent = Main
TopBar.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
TopBar.Size = UDim2.new(1, 0, 0, 35)

local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    Main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
end

TopBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = Main.Position
        
        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

TopBar.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)

local TopBarGlow = Instance.new("ImageLabel")
TopBarGlow.Name = "Glow"
TopBarGlow.Parent = TopBar
TopBarGlow.BackgroundTransparency = 1
TopBarGlow.Position = UDim2.new(0, -15, 0, -15)
TopBarGlow.Size = UDim2.new(1, 30, 1, 30)
TopBarGlow.Image = "rbxasset://textures/ui/Glow.png"
TopBarGlow.ImageColor3 = Color3.fromRGB(255, 0, 100)
TopBarGlow.ImageTransparency = 0.8

local Logo = Instance.new("TextLabel")
Logo.Name = "Logo"
Logo.Parent = TopBar
Logo.BackgroundTransparency = 1
Logo.Position = UDim2.new(0, 10, 0, 0)
Logo.Size = UDim2.new(0, 35, 1, 0)
Logo.Font = Enum.Font.GothamBold
Logo.Text = "☄️"
Logo.TextColor3 = Color3.fromRGB(255, 255, 255)
Logo.TextSize = 22

local function animateLogo()
    local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, -1, true)
    local tween = TweenService:Create(Logo, tweenInfo, {Rotation = 15})
    tween:Play()
    
    spawn(function()
        while wait(0.05) do
            Logo.Position = UDim2.new(0, 10 + math.random(-2, 2), 0, math.random(-2, 2))
        end
    end)
end
animateLogo()

local Title = Instance.new("TextLabel")
Title.Name = "Title"
Title.Parent = TopBar
Title.BackgroundTransparency = 1
Title.Position = UDim2.new(0, 45, 0, 0)
Title.Size = UDim2.new(0, 200, 1, 0)
Title.Font = Enum.Font.GothamBold
Title.Text = ""
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.TextSize = 14
Title.TextXAlignment = Enum.TextXAlignment.Left

local titleText = "Perplex🐉 | WratZ"
local function typeWrite()
    for i = 1, #titleText do
        Title.Text = string.sub(titleText, 1, i)
        wait(0.05)
    end
end
spawn(typeWrite)

local ContentArea = Instance.new("Frame")
ContentArea.Name = "ContentArea"
ContentArea.Parent = Main
ContentArea.BackgroundTransparency = 1
ContentArea.Position = UDim2.new(0, 120, 0, 35)
ContentArea.Size = UDim2.new(1, -120, 1, -35)

local Pages = {}

local function CreateButton(parent, text, position)
    local Button = Instance.new("TextButton")
    Button.Name = text.."Button"
    Button.Parent = parent
    Button.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    Button.Position = position
    Button.Size = UDim2.new(0, 160, 0, 30)
    Button.Font = Enum.Font.GothamSemibold
    Button.Text = text
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.TextSize = 12
    Button.AutoButtonColor = false

    local ButtonCorner = Instance.new("UICorner")
    ButtonCorner.CornerRadius = UDim.new(0, 6)
    ButtonCorner.Parent = Button

    Button.MouseEnter:Connect(function()
        TweenService:Create(Button, TweenInfo.new(0.3), {
            BackgroundColor3 = Color3.fromRGB(255, 0, 100)
        }):Play()
    end)

    Button.MouseLeave:Connect(function()
        TweenService:Create(Button, TweenInfo.new(0.3), {
            BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        }):Play()
    end)

    return Button
end

local function CreatePage(name)
    local Page = Instance.new("ScrollingFrame")
    Page.Name = name.."Page"
    Page.Parent = ContentArea
    Page.BackgroundTransparency = 1
    Page.Position = UDim2.new(0, 0, 0, 0)
    Page.Size = UDim2.new(1, 0, 1, 0)
    Page.Visible = false
    Page.ScrollBarThickness = 2
    Page.ScrollingDirection = Enum.ScrollingDirection.Y
    
    if name == "Main" then
        CreateButton(Page, "Aimbot Toggle", UDim2.new(0, 10, 0, 10))
        CreateButton(Page, "Wallhack Toggle", UDim2.new(0, 10, 0, 50))
        CreateButton(Page, "ESP Toggle", UDim2.new(0, 10, 0, 90))
    elseif name == "Player" then
        CreateButton(Page, "Speed Hack", UDim2.new(0, 10, 0, 10))
        CreateButton(Page, "Jump Boost", UDim2.new(0, 10, 0, 50))
        CreateButton(Page, "Infinite Jump", UDim2.new(0, 10, 0, 90))
        CreateButton(Page, "No Clip", UDim2.new(0, 10, 0, 130))
    elseif name == "Combat" then
        CreateButton(Page, "Silent Aim", UDim2.new(0, 10, 0, 10))
        CreateButton(Page, "Trigger Bot", UDim2.new(0, 10, 0, 50))
        CreateButton(Page, "No Recoil", UDim2.new(0, 10, 0, 90))
        CreateButton(Page, "Rapid Fire", UDim2.new(0, 10, 0, 130))
    elseif name == "Visuals" then
        CreateButton(Page, "Player ESP", UDim2.new(0, 10, 0, 10))
        CreateButton(Page, "Box ESP", UDim2.new(0, 10, 0, 50))
        CreateButton(Page, "Skeleton ESP", UDim2.new(0, 10, 0, 90))
        CreateButton(Page, "Chams", UDim2.new(0, 10, 0, 130))
    elseif name == "Misc" then
        CreateButton(Page, "Auto Bunny Hop", UDim2.new(0, 10, 0, 10))
        CreateButton(Page, "Third Person", UDim2.new(0, 10, 0, 50))
        CreateButton(Page, "FOV Changer", UDim2.new(0, 10, 0, 90))
    elseif name == "Settings" then
        CreateButton(Page, "Save Config", UDim2.new(0, 10, 0, 10))
        CreateButton(Page, "Load Config", UDim2.new(0, 10, 0, 50))
        CreateButton(Page, "Reset All", UDim2.new(0, 10, 0, 90))
    end
    
    Pages[name] = Page
    return Page
end

local SideMenu = Instance.new("Frame")
SideMenu.Name = "SideMenu"
SideMenu.Parent = Main
SideMenu.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
SideMenu.Position = UDim2.new(0, 0, 0, 35)
SideMenu.Size = UDim2.new(0, 120, 1, -35)

local function CreateTab(name, position, icon)
    local TabButton = Instance.new("TextButton")
    TabButton.Name = name.."Tab"
    TabButton.Parent = SideMenu
    TabButton.BackgroundColor3 = Color3.fromRGB(35, 35, 40)
    TabButton.Position = UDim2.new(0, 8, 0, position)
    TabButton.Size = UDim2.new(0.9, -8, 0, 32)
    TabButton.Font = Enum.Font.GothamSemibold
    TabButton.Text = icon.." "..name
    TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    TabButton.TextSize = 12
    TabButton.AutoButtonColor = false
    
    local TabCorner = Instance.new("UICorner")
    TabCorner.CornerRadius = UDim.new(0, 6)
    TabCorner.Parent = TabButton
    
    local Glow = Instance.new("ImageLabel")
    Glow.Name = "Glow"
    Glow.Parent = TabButton
    Glow.BackgroundTransparency = 1
    Glow.Position = UDim2.new(0, -15, 0, -15)
    Glow.Size = UDim2.new(1, 30, 1, 30)
    Glow.Image = "rbxasset://textures/ui/Glow.png"
    Glow.ImageColor3 = Color3.fromRGB(255, 0, 100)
    Glow.ImageTransparency = 1
    
    local page = CreatePage(name)
    if name == "Main" then page.Visible = true end
    
    TabButton.MouseButton1Click:Connect(function()
        for _, p in pairs(Pages) do
            p.Visible = false
        end
        Pages[name].Visible = true
    end)
    
    TabButton.MouseEnter:Connect(function()
        TweenService:Create(TabButton, TweenInfo.new(0.3), {
            BackgroundColor3 = Color3.fromRGB(255, 0, 100)
        }):Play()
        TweenService:Create(Glow, TweenInfo.new(0.3), {
            ImageTransparency = 0.8
        }):Play()
    end)
    
    TabButton.MouseLeave:Connect(function()
        TweenService:Create(TabButton, TweenInfo.new(0.3), {
            BackgroundColor3 = Color3.fromRGB(35, 35, 40)
        }):Play()
        TweenService:Create(Glow, TweenInfo.new(0.3), {
            ImageTransparency = 1
        }):Play()
    end)
end

CreateTab("Main", 10, "🎯")
CreateTab("Player", 50, "👤")
CreateTab("Combat", 90, "⚔️")
CreateTab("Visuals", 130, "👁️")
CreateTab("Misc", 170, "🎮")
CreateTab("Settings", 210, "⚙️")

Main.Size = UDim2.new(0, 0, 0, 0)
Main.BackgroundTransparency = 1
TopBar.BackgroundTransparency = 1
SideMenu.BackgroundTransparency = 1

TweenService:Create(Main, TweenInfo.new(0.5, Enum.EasingStyle.Back), {
    Size = UDim2.new(0, 500, 0, 350),
    BackgroundTransparency = 0
}):Play()

wait(0.2)
TweenService:Create(TopBar, TweenInfo.new(0.3), {
    BackgroundTransparency = 0
}):Play()

wait(0.2)
TweenService:Create(SideMenu, TweenInfo.new(0.3), {
    BackgroundTransparency = 0
}):Play()

local function toggleGui()
    if Main.Visible then
        TweenService:Create(Main, TweenInfo.new(0.5), {
            Size = UDim2.new(0, 0, 0, 0),
            BackgroundTransparency = 1
        }):Play()
        wait(0.5)
        Main.Visible = false
    else
        Main.Size = UDim2.new(0, 0, 0, 0)
        Main.BackgroundTransparency = 1
        Main.Visible = true
        TweenService:Create(Main, TweenInfo.new(0.5, Enum.EasingStyle.Back), {
            Size = UDim2.new(0, 500, 0, 350),
            BackgroundTransparency = 0
        }):Play()
    end
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if not gameProcessed and input.KeyCode == Enum.KeyCode.P then
        toggleGui()
    end
end)
