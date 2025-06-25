local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local TeleportService = game:GetService("TeleportService")
local HttpService = game:GetService("HttpService")

local player = Players.LocalPlayer 
local playerGui = player:WaitForChild("PlayerGui")

local screenGui = Instance.new("ScreenGui", playerGui)
screenGui.Name = "HurryGUI"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- Reduced main frame height from 340 to 280 to eliminate empty space
local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 320, 0, 380) -- Increased height to accommodate new buttons
mainFrame.Position = UDim2.new(0, 20, 0, 20)
mainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
mainFrame.BorderSizePixel = 0
local mainCorner = Instance.new("UICorner", mainFrame)
mainCorner.CornerRadius = UDim.new(0, 16)

-- Purple border
local purpleBorder = Instance.new("Frame", mainFrame)
purpleBorder.Position = UDim2.new(0, -2, 0, -2)
purpleBorder.Size = UDim2.new(1, 4, 1, 4)
purpleBorder.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
purpleBorder.ZIndex = -1
local borderCorner = Instance.new("UICorner", purpleBorder)
borderCorner.CornerRadius = UDim.new(0, 18)

local titleBar = Instance.new("Frame", mainFrame)
titleBar.Size = UDim2.new(1, 0, 0, 45)
titleBar.BackgroundTransparency = 1

local titleLabel = Instance.new("TextLabel", titleBar)
titleLabel.Size = UDim2.new(1, -55, 1, 0)
titleLabel.Position = UDim2.new(0, 15, 0, 0)
titleLabel.BackgroundTransparency = 1
titleLabel.Text = "⚡ REN HUB"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextSize = 18
titleLabel.TextXAlignment = Enum.TextXAlignment.Left

local minimizeBtn = Instance.new("TextButton", titleBar)
minimizeBtn.Size = UDim2.new(0, 35, 0, 35)
minimizeBtn.Position = UDim2.new(1, -45, 0, 5)
minimizeBtn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
minimizeBtn.Text = "−"
minimizeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minimizeBtn.Font = Enum.Font.GothamBold
minimizeBtn.TextSize = 18
minimizeBtn.BorderSizePixel = 0
local minCorner = Instance.new("UICorner", minimizeBtn)
minCorner.CornerRadius = UDim.new(0, 8)

-- Tab System
local tabFrame = Instance.new("Frame", mainFrame)
tabFrame.Size = UDim2.new(1, -20, 0, 40)
tabFrame.Position = UDim2.new(0, 10, 0, 50)
tabFrame.BackgroundTransparency = 1

local function createModernTab(parent, text, position)
    local tab = Instance.new("TextButton", parent)
    tab.Size = UDim2.new(0.33, -7, 1, 0)
    tab.Position = position
    tab.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
    tab.Text = text
    tab.TextColor3 = Color3.fromRGB(255, 255, 255)
    tab.Font = Enum.Font.GothamBold
    tab.TextSize = 14
    tab.BorderSizePixel = 0
    
    local corner = Instance.new("UICorner", tab)
    corner.CornerRadius = UDim.new(0, 10)
    
    return tab
end

local mainTab = createModernTab(tabFrame, "MAIN", UDim2.new(0, 0, 0, 0))
local espTab = createModernTab(tabFrame, "ESP", UDim2.new(0.33, 3, 0, 0))
local moreTab = createModernTab(tabFrame, "MORE", UDim2.new(0.66, 6, 0, 0))

-- Content Frames for each tab - adjusted height to fit optimized frame
local mainContent = Instance.new("Frame", mainFrame)
mainContent.Size = UDim2.new(1, -20, 1, -100)
mainContent.Position = UDim2.new(0, 10, 0, 95)
mainContent.BackgroundTransparency = 1
mainContent.Visible = true

local espContent = Instance.new("Frame", mainFrame)
espContent.Size = UDim2.new(1, -20, 1, -100)
espContent.Position = UDim2.new(0, 10, 0, 95)
espContent.BackgroundTransparency = 1
espContent.Visible = false

local moreContent = Instance.new("Frame", mainFrame)
moreContent.Size = UDim2.new(1, -20, 1, -100)
moreContent.Position = UDim2.new(0, 10, 0, 95)
moreContent.BackgroundTransparency = 1
moreContent.Visible = false

-- Modern button creation function with reduced button height
local function createModernButton(parent, text, position, size)
    local button = Instance.new("TextButton", parent)
    button.Size = size or UDim2.new(1, 0, 0, 45) -- Reduced from 50 to 45
    button.Position = position
    button.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    button.Text = text
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.Font = Enum.Font.GothamBold
    button.TextSize = 16
    button.BorderSizePixel = 0
    
    local corner = Instance.new("UICorner", button)
    corner.CornerRadius = UDim.new(0, 12)
    
    -- Hover effect
    button.MouseEnter:Connect(function()
        local tween = TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(138, 43, 226)})
        tween:Play()
    end)
    
    button.MouseLeave:Connect(function()
        local tween = TweenService:Create(button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(25, 25, 25)})
        tween:Play()
    end)
    
    return button
end

-- Main Tab Buttons with tighter spacing
local stealBtn = createModernButton(mainContent, "💰 STEAL (works randomly)", UDim2.new(0, 0, 0, 0))
local floatBtn = createModernButton(mainContent, "🟣 FLOAT", UDim2.new(0, 0, 0, 50))
local freecamBtn = createModernButton(mainContent, "🎥 FREECAM", UDim2.new(0, 0, 0, 100))
local antikickBtn = createModernButton(mainContent, "🛡️ ANTI-KICK", UDim2.new(0, 0, 0, 150))
local godmodeBtn = createModernButton(mainContent, "⚡ GOD MODE", UDim2.new(0, 0, 0, 200))

-- ESP Tab Button
local espBtn = createModernButton(espContent, "🧠 ESP", UDim2.new(0, 0, 0, 0))

-- More Tab Buttons with tighter spacing
local analyzeBtn = createModernButton(moreContent, "📊 ANALYZE", UDim2.new(0, 0, 0, 0))
local serverhopBtn = createModernButton(moreContent, "🔄 SERVER HOP", UDim2.new(0, 0, 0, 50)) -- Reduced spacing from 60 to 50

-- Credits label in More tab - moved up to eliminate space
local creditsLabel = Instance.new("TextLabel", moreContent)
creditsLabel.Size = UDim2.new(1, 0, 0, 40)
creditsLabel.Position = UDim2.new(0, 0, 0, 105) -- Moved up from 130 to 105
creditsLabel.BackgroundTransparency = 1
creditsLabel.Text = "💜 Credits to HurrySDM"
creditsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
creditsLabel.Font = Enum.Font.GothamBold
creditsLabel.TextSize = 14
creditsLabel.TextXAlignment = Enum.TextXAlignment.Center

-- Tab System Logic
local currentTab = "Main"

local function switchTab(tab)
    -- Reset all tab colors
    local inactiveColor = Color3.fromRGB(20, 20, 20)
    local activeColor = Color3.fromRGB(138, 43, 226)
    
    mainTab.BackgroundColor3 = inactiveColor
    mainTab.TextColor3 = Color3.fromRGB(255, 255, 255)
    espTab.BackgroundColor3 = inactiveColor
    espTab.TextColor3 = Color3.fromRGB(255, 255, 255)
    moreTab.BackgroundColor3 = inactiveColor
    moreTab.TextColor3 = Color3.fromRGB(255, 255, 255)
    
    -- Hide all content frames
    mainContent.Visible = false
    espContent.Visible = false
    moreContent.Visible = false
    
    -- Show selected tab
    if tab == "Main" then
        mainTab.BackgroundColor3 = activeColor
        mainTab.TextColor3 = Color3.fromRGB(255, 255, 255)
        mainContent.Visible = true
    elseif tab == "ESP" then
        espTab.BackgroundColor3 = activeColor
        espTab.TextColor3 = Color3.fromRGB(255, 255, 255)
        espContent.Visible = true
    elseif tab == "More" then
        moreTab.BackgroundColor3 = activeColor
        moreTab.TextColor3 = Color3.fromRGB(255, 255, 255)
        moreContent.Visible = true
    end
    
    currentTab = tab
end

mainTab.MouseButton1Click:Connect(function() switchTab("Main") end)
espTab.MouseButton1Click:Connect(function() switchTab("ESP") end)
moreTab.MouseButton1Click:Connect(function() switchTab("More") end)

-- STEAL BUTTON FUNCTIONALITY
stealBtn.MouseButton1Click:Connect(function()
    local originalText = stealBtn.Text
    stealBtn.Text = "💰 STEALING..."
    stealBtn.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
    
    local char = player.Character
    if char and char:FindFirstChild("HumanoidRootPart") and char:FindFirstChild("Humanoid") then
        -- Ragdoll the character first
        local humanoid = char.Humanoid
        humanoid.PlatformStand = true
        humanoid:ChangeState(Enum.HumanoidStateType.Physics) -- Makes character ragdoll
        
        -- Wait a brief moment for ragdoll to take effect
        task.wait(0.1)
        
        -- Then teleport
        local pos = CFrame.new(0, -500, 0)
        local startT = os.clock()
        while os.clock() - startT < 1 do
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character.HumanoidRootPart.CFrame = pos
            end
            task.wait()
        end
        
        -- Un-ragdoll after teleportation
        if char and char:FindFirstChild("Humanoid") then
            humanoid.PlatformStand = false
            humanoid:ChangeState(Enum.HumanoidStateType.Running)
        end
    end
    
    -- Reset button after operation
    task.wait(0.5)
    stealBtn.Text = originalText
    stealBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
end)

-- FLOAT FUNCTIONALITY
local floatOn = false
local floatForce
floatBtn.MouseButton1Click:Connect(function()
    local char = player.Character or player.CharacterAdded:Wait()
    local root = char:WaitForChild("HumanoidRootPart")
    if floatOn then
        if floatForce then floatForce:Destroy() end
        floatBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
        floatBtn.Text = "🟣 FLOAT"
    else
        floatForce = Instance.new("BodyVelocity")
        floatForce.Velocity = Vector3.new(0, 20, 0)
        floatForce.MaxForce = Vector3.new(0, 1e5, 0)
        floatForce.P = 1000
        floatForce.Parent = root
        floatBtn.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
        floatBtn.Text = "🟣 FLOATING"
    end
    floatOn = not floatOn
end)

-- FREECAM FUNCTIONALITY
freecamBtn.MouseButton1Click:Connect(function()
    loadstring(game:HttpGet('https://raw.githubusercontent.com/GhostPlayer352/Test4/main/Freecam'))()
end)

-- ANTI-KICK FUNCTIONALITY
antikickBtn.MouseButton1Click:Connect(function()
    local originalText = antikickBtn.Text
    antikickBtn.Text = "🛡️ LOADING..."
    antikickBtn.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
    
    pcall(function()
        loadstring(game:HttpGet("https://pastefy.app/dAjYZBnq/raw"))()
    end)
    
    task.wait(1)
    antikickBtn.Text = "🛡️ ANTI-KICK ON"
    antikickBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 127)
end)

-- GOD MODE FUNCTIONALITY
local godModeOn = false
godmodeBtn.MouseButton1Click:Connect(function()
    local char = player.Character
    if not char then return end
    
    local humanoid = char:FindFirstChild("Humanoid")
    if not humanoid then return end
    
    godModeOn = not godModeOn
    
    if godModeOn then
        -- Enable god mode
        humanoid.MaxHealth = math.huge
        humanoid.Health = math.huge
        
        -- Protect against health changes
        local connection
        connection = humanoid.HealthChanged:Connect(function()
            if godModeOn then
                humanoid.Health = math.huge
            else
                connection:Disconnect()
            end
        end)
        
        -- Visual feedback
        godmodeBtn.Text = "⚡ GOD MODE ON"
        godmodeBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 127)
    else
        -- Disable god mode
        humanoid.MaxHealth = 100
        humanoid.Health = 100
        
        -- Visual feedback
        godmodeBtn.Text = "⚡ GOD MODE"
        godmodeBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    end
end)

-- ESP FUNCTIONALITY
local espOn = false
local espInstances = {}
local function createESPForPlayer(target)
    if target == player or espInstances[target] then return end
    local character = target.Character or target.CharacterAdded:Wait()
    local head = character:WaitForChild("Head")
    local esp = Instance.new("BillboardGui")
    esp.Adornee = head
    esp.AlwaysOnTop = true
    esp.Size = UDim2.new(0, 100, 0, 40)
    esp.StudsOffset = Vector3.new(0, 3, 0)
    esp.Parent = CoreGui
    local label = Instance.new("TextLabel")
    label.Text = target.Name
    label.TextColor3 = Color3.fromRGB(255, 255, 255)
    label.TextStrokeTransparency = 0
    label.TextStrokeColor3 = Color3.fromRGB(138, 43, 226)
    label.BackgroundTransparency = 1
    label.Size = UDim2.new(1, 0, 1, 0)
    label.Font = Enum.Font.GothamBold
    label.TextScaled = true
    label.Parent = esp
    espInstances[target] = esp
end

local function clearAllESP()
    for _, gui in pairs(espInstances) do
        if gui then gui:Destroy() end
    end
    espInstances = {}
end

espBtn.MouseButton1Click:Connect(function()
    espOn = not espOn
    if espOn then
        espBtn.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
        espBtn.Text = "🧠 ESP ON"
        for _, p in pairs(Players:GetPlayers()) do
            createESPForPlayer(p)
        end
    else
        espBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
        espBtn.Text = "🧠 ESP"
        clearAllESP()
    end
end)

-- ANALYZE FUNCTIONALITY
analyzeBtn.MouseButton1Click:Connect(function()
    local highest = 0
    local richest = nil
    for _, p in pairs(Players:GetPlayers()) do
        local stats = p:FindFirstChild("leaderstats")
        if stats and stats:FindFirstChild("Cash") then
            local cash = stats.Cash.Value
            if type(cash) == "number" and cash > highest then
                highest = cash
                richest = p
            end
        end
    end
    if richest then
        analyzeBtn.Text = "📊 RICHEST: " .. richest.Name
        analyzeBtn.BackgroundColor3 = Color3.fromRGB(138, 43, 226)
    else
        analyzeBtn.Text = "📊 NO DATA FOUND"
        analyzeBtn.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
    end
    
    -- Reset after 3 seconds
    task.wait(3)
    analyzeBtn.Text = "📊 ANALYZE"
    analyzeBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
end)

-- SERVER HOP FUNCTIONALITY
local function serverHop()
    local originalText = serverhopBtn.Text
    serverhopBtn.Text = "🔄 SEARCHING..."
    serverhopBtn.BackgroundColor3 = Color3.fromRGB(255, 165, 0)
    
    local function getServers()
        local servers = {}
        local success, result = pcall(function()
            return HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?sortOrder=Asc&limit=100"))
        end)
        
        if success and result and result.data then
            for _, server in ipairs(result.data) do
                if server.playing and type(server.playing) == "number" and server.id ~= game.JobId then
                    table.insert(servers, server.id)
                end
            end
        end
        
        return servers
    end
    
    local function tryTeleport(servers)
        if #servers > 0 then
            serverhopBtn.Text = "🔄 JOINING..."
            serverhopBtn.BackgroundColor3 = Color3.fromRGB(0, 255, 127)
            TeleportService:TeleportToPlaceInstance(game.PlaceId, servers[math.random(1, #servers)])
        else
            -- If no servers found, try again after a short delay
            task.wait(1)
            servers = getServers()
            tryTeleport(servers)
        end
    end
    
    local servers = getServers()
    tryTeleport(servers)
    
    -- In case teleport fails, reset button after 5 seconds
    task.delay(5, function()
        if serverhopBtn then
            serverhopBtn.Text = originalText
            serverhopBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
        end
    end)
end

serverhopBtn.MouseButton1Click:Connect(serverHop)

-- MINIMIZE FUNCTIONALITY - updated with new frame height
local minimized = false
minimizeBtn.MouseButton1Click:Connect(function()
    if minimized then
        mainFrame:TweenSize(UDim2.new(0, 320, 0, 380), "Out", "Quad", 0.3, true) -- Updated height
        tabFrame.Visible = true
        if currentTab == "Main" then
            mainContent.Visible = true
        elseif currentTab == "ESP" then
            espContent.Visible = true
        elseif currentTab == "More" then
            moreContent.Visible = true
        end
        minimizeBtn.Text = "−"
    else
        mainFrame:TweenSize(UDim2.new(0, 320, 0, 45), "Out", "Quad", 0.3, true)
        tabFrame.Visible = false
        mainContent.Visible = false
        espContent.Visible = false
        moreContent.Visible = false
        minimizeBtn.Text = "+"
    end
    minimized = not minimized
end)

-- DRAG FUNCTIONALITY
local dragging, dragStart, startPos = false

local function beginDrag(input)
    dragging = true
    dragStart = input.Position
    startPos = mainFrame.Position
end

local function updateDrag(input)
    if dragging then
        local delta = input.Position - dragStart
        mainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end

local function endDrag()
    dragging = false
end

titleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        beginDrag(input)
    end
end)

titleBar.InputChanged:Connect(function(input)
    if dragging then
        updateDrag(input)
    end
end)

titleBar.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        endDrag()
    end
end)
