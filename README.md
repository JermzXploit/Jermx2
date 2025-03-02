--// fix rename and library by khen.cc
getgenv().venus = {
    ["Enabled"] = true,
    ["AimPart"] = "Head",
    ["Prediction"] = 0.12588,
    ["Smoothness"] = 0.7,
    ["AutoPred"] = true,
    ["Loaded"] = false,
    ["AntiAimViewer"] = true,
    ["cframe"] = {
        ["enabled"] = false,
        ["speed"] = 2
    },
    ["TargetStrafe"] = {
        ["Enabled"] = false,
        ["StrafeSpeed"] = 10,
        ["StrafeRadius"] = 7,
        ["StrafeHeight"] = 3,
        ["RandomizerMode"] = false
    },
    ["targetaim"] = {
        ["Toggled"] = false,
        ["enabled"] = true,
        ["targetPart"] = "UpperTorso",
        ["prediction"] = 0.12588
    },
    ["FOV"] = {
        ["Enabled"] = true,
        ["Size"] = 13,
        ["Centered"] = false,
        ["Visible"] = true,
        ["Filled"] = false,
        ["Color"] = Color3.fromRGB(255, 0, 0)
    },
    ["desync"] = {
        ["sky"] = false,
        ["invis"] = true,
        ["jump"] = false,
        ["network"] = false
    },
    ["Misc"] = {
        ["LowGfx"] = false
    },
    ["FPSunlocker"] = {
        ["Enabled"] = true,
        ["FPSCap"] = 999
    }
}

local InnalillahiMataKiri = Instance.new("ScreenGui")
InnalillahiMataKiri.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
InnalillahiMataKiri.Parent = game:GetService("CoreGui") -- Ensures the GUI is parented to CoreGui

local Notifications_Frame = Instance.new("Frame")
Notifications_Frame.Name = "Notifications"
Notifications_Frame.BackgroundTransparency = 1
Notifications_Frame.Size = UDim2.new(1, 0, 1, 36)
Notifications_Frame.Position = UDim2.fromOffset(0, -36)
Notifications_Frame.ZIndex = 5
Notifications_Frame.Parent = InnalillahiMataKiri

local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")

local NotificationSystem = {}
local ActiveNotifications = {}

local function GetDictionaryLength(dictionary)
    local count = 0
    for _ in pairs(dictionary) do
        count += 1
    end
    return count
end

function NotificationSystem:Notify(Content: string, Delay: number)
    assert(typeof(Content) == "string", "missing argument #1, (string expected got " .. typeof(Content) .. ")")
    local Delay = typeof(Delay) == "number" and Delay or 3

    local Text = Instance.new("TextLabel")
    local Notification = {
        self = Text,
        Class = "Notification"
    }

    Text.Name = "Notification"
    Text.BackgroundTransparency = 1
    -- Adjusted position to move the notification further to the left with -190
    Text.Position = UDim2.new(0.5, -190, 1, -150 - (GetDictionaryLength(ActiveNotifications) * 15))  -- Change -170 to -190
    Text.Size = UDim2.new(0, 200, 0, 20)  -- Keeps the width of the notification as 200 pixels
    Text.Text = Content
    Text.Font = Enum.Font.SourceSans
    Text.TextSize = 17
    Text.TextColor3 = Color3.new(1, 1, 1)
    Text.TextStrokeTransparency = 0.2
    Text.TextTransparency = 1
    Text.RichText = true
    Text.ZIndex = 4
    Text.Parent = Notifications_Frame

    local function CustomTweenOffset(Offset: number)
        spawn(function()
            local Steps = 33
            for i = 1, Steps do
                Text.Position += UDim2.fromOffset(Offset / Steps, 0)
                RunService.RenderStepped:Wait()
            end
        end)
    end

    function Notification:Destroy()
        ActiveNotifications[Notification] = nil
        Text:Destroy()

        for _, v in pairs(ActiveNotifications) do
            v.self.Position += UDim2.fromOffset(0, 15)
        end
    end

    ActiveNotifications[Notification] = Notification

    local TweenIn = TweenService:Create(Text, TweenInfo.new(0.3, Enum.EasingStyle.Linear, Enum.EasingDirection.Out, 0, false, 0), { TextTransparency = 0 })
    local TweenOut = TweenService:Create(Text, TweenInfo.new(0.2, Enum.EasingStyle.Linear, Enum.EasingDirection.Out, 0, false, 0), { TextTransparency = 1 })

    TweenIn:Play()
    CustomTweenOffset(100)

    TweenIn.Completed:Connect(function()
        delay(Delay, function()
            TweenOut:Play()
            CustomTweenOffset(100)

            TweenOut.Completed:Connect(function()
                Notification:Destroy()
            end)
        end)
    end)
end

 repeat wait() until game:IsLoaded()




local repo = 'https://raw.githubusercontent.com/JermzXploit/JermxRemake.cc/refs/heads/main/README.md”))()'

loadstring(game:HttpGet(“https://raw.githubusercontent.com/JermzXploit/JermxRemake.cc/refs/heads/main/README.md”))()

local ThemeManager = loadstring(game:HttpGet(loadstring(game:HttpGet(“https://raw.githubusercontent.com/JermzXploit/JermxRemake.cc/refs/heads/main/README.md”))()

local SaveManager = loadstring(game:HttpGet(loadstring(game:HttpGet(“https://raw.githubusercontent.com/JermzXploit/JermxRemake.cc/refs/heads/main/README.md”))()


Library:Notify('Jermx Loading')
wait(1)

local Window = Library:CreateWindow({
    Title = "Jermx.CC",
    Center = true,
    AutoShow = true,
    Size = UDim2.new(0, 450, 0, 380)
})

local Tabs = {
        Main = Window:AddTab("Main"),
        Rage = Window:AddTab("Visuals"),
        ["UI Settings"] = Window:AddTab("Configuration")
    }

Library:SetWatermarkVisibility(true)

local FrameTimer = tick()
local FrameCounter = 0
local FPS = 60

local WatermarkConnection = game:GetService('RunService').RenderStepped:Connect(function()
    FrameCounter += 1

    if (tick() - FrameTimer) &gt;= 1 then
        FPS = FrameCounter
        FrameTimer = tick()
        FrameCounter = 0
    end

    local currentTime = os.date("%H:%M:%S")  -- Format the time as HH:MM:SS

    Library:SetWatermark(('Jermx.CC | %s fps | %s ms | Time: %s'):format(
        math.floor(FPS),
        math.floor(game:GetService('Stats').Network.ServerStatsItem['Data Ping']:GetValue()),
        currentTime
    ))
end)

Library:OnUnload(function()
    WatermarkConnection:Disconnect()

    print('Unloaded!')
    Library.Unloaded = true
end)

local assist = Tabs.Main:AddLeftGroupbox("Aim Assist")

local air = Tabs.Main:AddLeftGroupbox("Air Settings")

local spin = Tabs.Main:AddLeftGroupbox("Spin Bot")

local set = Tabs.Main:AddRightGroupbox("Prediction config")

local tar = Tabs.Main:AddRightGroupbox("Target Strafe")

local fov = Tabs.Main:AddLeftGroupbox("Fov")

local cframe = Tabs.Rage: AddRightGroupbox("Cframe")

local visuals = Tabs.Rage: AddLeftGroupbox("Visuals")

local fog = Tabs.Rage: AddRightGroupbox("Fog Costumization")

local esp = Tabs.Rage: AddLeftGroupbox("Esp")

local anti = Tabs.Rage: AddLeftGroupbox("Desync")

local net = Tabs.Rage: AddLeftGroupbox("Fast Flags")

local ant = Tabs.Rage: AddRightGroupbox("Anti Lock")

local fly = Tabs.Rage: AddRightGroupbox("Fly Costumization")

assist:AddToggle(
    "Enable Camlock",
    {
        Text = "Enable Aim assist",
        Default = true,
        Tooltip = "Enable",
        Callback = function(state)
            venus.Enabled = state
        end
    }
)

set:AddToggle(
    "Enable AutoPrediction",
    {
        Text = "Enable AutoPrediction",
        Default = false,
        Tooltip = "Enable",
        Callback = function(state)
            venus.AutoPred = state
            venus.CamlockEnabled = state -- Enable auto prediction for camlock
            venus.TargetAimEnabled = state -- Enable auto prediction for target aim
        end
    }
)

set:AddInput(
    "Prediction",
    {
        Default = "Prediction",
        Numeric = true,
        Finished = false,
        Text = "Prediction",
        Tooltip = "Change Prediction for Target and Camlock",
        Placeholder = "0.1",
        Callback = function(value)
            venus.Prediction = tonumber(value) or 1
        end
    }
)

set:AddInput(
        "Smoothness",
        {
            Default = "Smoothness",
            Numeric = false,
            Finished = false,
            Text = "Smoothness",
            Tooltip = "Change smoothing For Target",
            Placeholder = "0.1",
            Callback = function(value)
                venus.Smoothness = value
            end
        }
    )

assist:AddToggle(
    "Enable LookAt",
    {
        Text = "Enable Look At",
        Default = false,  -- Set default to true (enabled) or false (disabled)
        Tooltip = "Enable or disable the LookAt functionality",
        Callback = function(state)
            venus.LookAtEnabled = state
        end
    }
)

tar:AddToggle(
    "Enable Target Strafe",
    {
        Text = "Target Strafe",
        Default = false,
        Tooltip = "Toggle Target Strafe (Orbiting)",
        Callback = function(state)
            venus.cframe.TargetStrafe.Enabled = state
        end
    }
)

tar:AddInput(
    "Target Strafe Distance",
    {
        Default = "15",
        Numeric = true,
        Finished = false,
        Text = "Distance",
        Tooltip = "Adjust the distance for target strafe (orbit radius)",
        Placeholder = "20",
        Callback = function(value)
            venus.cframe.TargetStrafe.StrafeRadius = tonumber(value) or venus.cframe.TargetStrafe.StrafeRadius
        end
    }
)

tar:AddInput(
    "Target Strafe Speed",
    {
        Default = "5",
        Numeric = true,
        Finished = false,
        Text = "Speed",
        Tooltip = "Adjust the speed for target strafe (orbiting)",
        Placeholder = "10",
        Callback = function(value)
            venus.cframe.TargetStrafe.StrafeSpeed = tonumber(value) or venus.cframe.TargetStrafe.StrafeSpeed
        end
    }
)

tar:AddInput(
    "Target Strafe Height",
    {
        Default = "10",
        Numeric = true,
        Finished = false,
        Text = "Height",
        Tooltip = "Adjust the height for target strafe (orbiting)",
        Placeholder = "5",
        Callback = function(value)
            venus.cframe.TargetStrafe.Height = tonumber(value) or venus.cframe.TargetStrafe.Height
        end
    }
)

set:AddDropdown(
    "Hitpart",
    {
        Values = {"Head", "UpperTorso", "HumanoidRootPart", "LowerTorso", "LeftUpperArm", "RightUpperArm", "LeftLeg", "RightLeg"},
        Default = 1,  -- Default selection is "UpperTorso" (index 1)
        Multi = false,  -- Single selection (not multiple)
        Text = "Hitpart",
        Tooltip = "Choose the hit part",
        Callback = function(value)
            venus.AimPart = value  -- Update the AimPart for targeting
            camlock.AimPart = value -- Ensure camlock uses the same AimPart
        end
    }
)

assist:AddToggle(
    "Enable TargetAim",
    {
        Text = "Enable Target Aim",
        Default = true,
        Tooltip = "Enable",
        Callback = function(state)
            targetaim.enabled = state
        end
    }
)

air:AddToggle(
    "Enable Auto Air",
    {
        Text = "Auto Air",
        Default = false,
        Tooltip = "Toggle Auto Air",
        Callback = function(state)
            venus.AutoAirEnabled = state
        end
    }
)

air:AddInput(
    "JumpOffset",
    {
        Default = "Air Offset",
        Numeric = true,
        Finished = false,
        Text = "Offset",
        Tooltip = "Change Air Offset for Target and Camlock",
        Placeholder = "0",
        Callback = function(value)
            venus.JumpOffset = tonumber(value) or 0
        end
    }
)

cframe:AddToggle(
    "Enable cframe",
    {
        Text = "cframe",
        Default = false,
        Tooltip = "Enable CFrame Speed",
        Callback = function(state)
            venus.cframe.enabled = state
            if venus.cframe.enabled then
                -- If CFrame is enabled, start moving the character with speed
                while venus.cframe.enabled do
                    if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame + game.Players.LocalPlayer.Character.Humanoid.MoveDirection * venus.cframe.speed
                    end
                    game:GetService("RunService").Stepped:Wait()
                end
            end
        end
    }
)

cframe:AddSlider(
    "cframe speed",
    {
        Text = "CFrame Speed",
        Default = 0,
        Min = 0,
        Max = 50,
        Rounding = 1,
        Compact = false,
        Callback = function(Value)
            venus.cframe.speed = Value
        end
    }
)

game:GetService("UserInputService").InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.LeftBracket then
        venus.cframe.speed = venus.cframe.speed + 0.01
        print("Speed: " .. venus.cframe.speed)
    elseif input.KeyCode == Enum.KeyCode.RightBracket then
        venus.cframe.speed = venus.cframe.speed - 0.01
        print("Speed: " .. venus.cframe.speed)
    end
end)

local speed = 45
local LocalPlayer = game:GetService("Players").LocalPlayer
local RunService = game:GetService("RunService")

-- Initialize the state
local isToggled = false

spin:AddToggle(
    "Enable SpinBot",
    {
        Text = "Spin Bot",
        Default = false,
        Tooltip = "Enable",
        Callback = function(state)
            isToggled = state
        end
    }
)

-- Add a slider to control the speed with rounding
spin:AddSlider(
    "SpinBot Speed",
    {
        Text = "Speed",
        Min = 1,
        Max = 100,
        Default = speed,
        Rounding = 1,  -- Round the value to the nearest 1
        Compact = false,
        Tooltip = "Set the speed of the rotation",
        Callback = function(Value)
            speed = Value  -- Directly update speed with the rounded value
        end
    }
)

-- Factor to make the rotation faster (change the value to adjust speed)
local speedMultiplier = 12  -- You can adjust this multiplier for faster/slower rotation

RunService.RenderStepped:Connect(function(Delta)
    if isToggled then
        -- Apply a multiplier to increase the speed effect
        LocalPlayer.Character.HumanoidRootPart.CFrame = LocalPlayer.Character.HumanoidRootPart.CFrame * CFrame.Angles(0, math.rad(speed * speedMultiplier) * Delta, 0)
        LocalPlayer.Character:FindFirstChild("Humanoid").AutoRotate = false
    else
        LocalPlayer.Character:FindFirstChild("Humanoid").AutoRotate = true
    end
end)

-- Highest Roblox velocity is 128^2 or 16384
local velMax = (128 ^ 2)

-- Time to release and choke the replication packets
local timeRelease, timeChoke = 0.015, 0.105

-- Function aliases
local Property, Wait = sethiddenproperty, wait
local Radian, Random, Ceil = math.rad, math.random, math.ceil
local Angle = CFrame.Angles
local Vector = Vector3.new
local Service = game.GetService

-- Services
local Run = Service(game, 'RunService')
local statPing = Service(game, 'Stats').PerformanceStats.Ping
local Root = Service(game, 'Players').LocalPlayer.Character:WaitForChild("HumanoidRootPart")

-- Connections
local runRen, runBeat = Run.RenderStepped, Run.Heartbeat
local runRenWait, runRenCon = runRen.Wait, runRen.Connect
local runBeatCon = runBeat.Connect

-- Ping function
local Ping = statPing.GetValue

-- Client replication choking/sleeping
local function Sleep()
    Property(Root, 'NetworkIsSleeping', true)
end

-- Initialization function
local function Init()
    local rootVel = Root.Velocity
    local rootAng = Random(-180, 180)
    local rootOffset = Vector(
        Random(-velMax, velMax),
        -Random(0, velMax),
        Random(-velMax, velMax)
    )

    Root.CFrame *= Angle(0, Radian(rootAng), 0)
    Root.Velocity = rootOffset

    runRenWait(runRen) -- Sync velocity smoothly with render
    Root.CFrame *= Angle(0, Radian(-rootAng), 0)
    Root.Velocity = rootVel
end

-- Toggle control
local desyncEnabled = false
local desyncLoop

-- Function to toggle desync
local function toggleDesync(state)
    desyncEnabled = state
    if desyncEnabled then
        -- Start desync loop
        desyncLoop = Run.Heartbeat:Connect(function()
            Init()
            Wait(timeRelease)
            
            local chokeClient, chokeServer = runBeatCon(runBeat, Sleep), runRenCon(runRen, Sleep)
            Wait(Ceil(Ping(statPing)) / 1000)
            
            chokeClient:Disconnect()
            chokeServer:Disconnect()
        end)
    else
        -- Stop desync loop
        if desyncLoop then
            desyncLoop:Disconnect()
            desyncLoop = nil
        end
    end
end

-- Toggle button setup
anti:AddToggle(
    "Enable Desync",
    {
        Text = "Invisible Desync",
        Default = false,
        Tooltip = "Enable or Disable the desync feature",
        Callback = function(state)
            toggleDesync(state)  -- Enable or disable desync based on button state
        end
    }
)

-- UI setup (example using your UI structure)
net:AddToggle(
    "Enable Network Sleep",
    {
        Text = "Network FF",
        Default = false,
        Tooltip = "Toggle to simulate network sleep and enhance lag",
        Callback = function(state)
            if state then
                -- Enable the "network sleep" behavior
                setfflag("S2PhysicsSenderRate", 2)
                local UserInputService = game:GetService("UserInputService")
                local Players = game:GetService("Players")
                local Client = Players.LocalPlayer

                local MainThread = task.spawn(function()
                    while state do
                        if Client.Character and Client.Character:FindFirstChild("HumanoidRootPart") then
                            sethiddenproperty(Client.Character.HumanoidRootPart, "NetworkIsSleeping", true)
                            task.wait()
                            sethiddenproperty(Client.Character.HumanoidRootPart, "NetworkIsSleeping", false)
                        end
                        task.wait()
                    end
                end)
            else
                -- Disable the "network sleep" behavior
                setfflag("S2PhysicsSenderRate", 13)
                local Client = Players.LocalPlayer
                if Client.Character and Client.Character:FindFirstChild("HumanoidRootPart") then
                    sethiddenproperty(Client.Character.HumanoidRootPart, "NetworkIsSleeping", false)
                end
            end
        end
    }
)

-- Main Script
game:GetService("RunService").Heartbeat:Connect(function()
    local player = game.Players.LocalPlayer
    local character = player.Character

    if character and character:FindFirstChild("HumanoidRootPart") then
        local humanoidRootPart = character.HumanoidRootPart
        local vel = humanoidRootPart.Velocity

        -- CFrame Speed Toggle
        if getgenv().cframespeedtoggle == true then
            humanoidRootPart.CFrame = humanoidRootPart.CFrame +
                character.Humanoid.MoveDirection * getgenv().speedvalue / 0.5
        end

        -- Anti Lock Functionality
        if getgenv().Venus and getgenv().Venus.AntiEnabled then
            if getgenv().Venus.AntiLock == "Predbreaker" then
                humanoidRootPart.Velocity = Vector3.new(0, 0, 0)
            elseif getgenv().Venus.AntiLock == "Sky" then
                humanoidRootPart.Velocity = Vector3.new(0, 100, 0)
            end
        end

        game:GetService("RunService").RenderStepped:Wait()
        humanoidRootPart.Velocity = vel
    end
end)

-- UI Toggle Button to Enable Anti Lock
ant:AddToggle(
    "Enable Anti Lock",
    {
        Text = "Enable Anti Lock",
        Default = false,  -- Default state is disabled
        Tooltip = "Toggle Anti Lock",
        Callback = function(state)
            getgenv().Venus = getgenv().Venus or {}  -- Ensure Venus table exists
            getgenv().Venus.AntiEnabled = state  -- Set AntiEnabled based on toggle state
        end
    }
)

-- Dropdown for Anti Lock Modes
ant:AddDropdown(
    "AntiLockMode",
    {
        Values = {"Sky", "Predbreaker"},
        Default = 1,  -- Default selection is "Sky"
        Multi = false,  -- Single selection
        Text = "Anti Lock Mode",
        Tooltip = "Choose Anti Lock mode",
        Callback = function(value)
            getgenv().Venus = getgenv().Venus or {}  -- Ensure Venus table exists
            getgenv().Venus.AntiLock = value  -- Set AntiLock mode
        end
    }
)

-- Create the toggle button in the UI
anti:AddToggle(
    "Desync Velocity",
    {
        Text = "Velocity Manipulation",
        Default = false,
        Tooltip = "Toggles the manipulation of velocity",
        Callback = function(state)
            getgenv().demisethebest = state  -- Toggle the velocity manipulation state
        end
    }
)

-- The script that manipulates the velocity
game:GetService("RunService").heartbeat:Connect(function()
    if getgenv().demisethebest == true then 
        local abc = game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity
        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(1,1,1) * (2^16)
        game:GetService("RunService").RenderStepped:Wait()
        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = abc
    end 
end)

local Plr = game.Players.LocalPlayer
local StateEnabled = false  -- Tracks if the feature is enabled or not

-- Add the toggle button
anti:AddToggle(
    "Enable Freefall Speed",
    {
        Text = "Freefall Velocity Boost",
        Default = false,
        Tooltip = "Enable faster freefall",
        Callback = function(state)
            StateEnabled = state  -- Update the state when the toggle is pressed
        end
    }
)

-- StateChanged event listener with the toggle
Plr.Character:WaitForChild("Humanoid").StateChanged:Connect(function(old, new)
    if StateEnabled and new == Enum.HumanoidStateType.Freefall then
        wait(0.27)
        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, -15, 0)
    end
end)

-- Toggle button for enabling/disabling Underground
net:AddToggle(
    "Enable Underground",
    {
        Text = "Underground",
        Default = false,
        Tooltip = "Toggles the underground velocity manipulation",
        Callback = function(state)
            getgenv().Underground = state
        end
    }
)

-- Add an input to adjust the UndergroundAmount
net:AddInput(
    "Underground Amount",
    {
        Default = tostring(getgenv().UndergroundAmount),
        Numeric = true,
        Finished = true,
        Text = "Amount",
        Tooltip = "Adjust the downward velocity for the underground effect",
        Placeholder = "825",
        Callback = function(value)
            getgenv().UndergroundAmount = tonumber(value) or getgenv().UndergroundAmount
        end
    }
)

-- The script that manipulates underground velocity
getgenv().UndergroundAmount = 825
game:GetService("RunService").heartbeat:Connect(function()
    if getgenv().Underground then
        local vel = game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity
        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, -getgenv().UndergroundAmount, 0)
        game:GetService("RunService").RenderStepped:Wait()
        game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = vel
    end
end)

-- Set up initial variables for fly speed and control
local flySpeed = 50  -- Default speed
local flying = false  -- Set to false

-- Get the player and character
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:FindFirstChildOfClass("Humanoid")
local userInputService = game:GetService("UserInputService")

-- Function to start flying
local function startFlying()
    if not flying then
        flying = true
        humanoid.PlatformStand = true
        
        local bodyGyro = Instance.new("BodyGyro", character.PrimaryPart)
        local bodyVelocity = Instance.new("BodyVelocity", character.PrimaryPart)
        bodyGyro.P = 9e4
        bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
        bodyGyro.CFrame = workspace.CurrentCamera.CFrame
        bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)

        -- Update fly direction and speed based on camera orientation
        local connection
        connection = game:GetService("RunService").RenderStepped:Connect(function()
            if flying then
                bodyGyro.CFrame = workspace.CurrentCamera.CFrame
                
                local moveDirection = Vector3.new(0, 0, 0)
                if humanoid.MoveDirection.Magnitude &gt; 0 then
                    moveDirection = workspace.CurrentCamera.CFrame.LookVector * humanoid.MoveDirection.Magnitude
                end
                
                local verticalVelocity = 0
                if userInputService:IsKeyDown(Enum.KeyCode.Space) then
                    verticalVelocity = flySpeed -- Ascend
                elseif userInputService:IsKeyDown(Enum.KeyCode.LeftControl) then
                    verticalVelocity = -flySpeed -- Descend
                end
                
                bodyVelocity.Velocity = moveDirection * flySpeed + Vector3.new(0, verticalVelocity, 0)
                humanoid:ChangeState(Enum.HumanoidStateType.Freefall)
            else
                bodyVelocity.Velocity = Vector3.new(0, 0, 0) -- Stop movement when not flying
                connection:Disconnect() -- Disconnect the RenderStepped connection
            end
        end)
    end
end

-- Function to stop flying
local function stopFlying()
    if flying then
        flying = false
        humanoid.PlatformStand = false
        for _, v in pairs(character.PrimaryPart:GetChildren()) do
            if v:IsA("BodyGyro") or v:IsA("BodyVelocity") then
                v:Destroy()
            end
        end
    end
end

-- Toggle button setup
fly:AddToggle(
    "Enable Fly",
    {
        Text = "Enable Fly",
        Default = false,
        Tooltip = "Enable or disable flying",
        Callback = function(state)
            if state then
                startFlying()
            else
                stopFlying()
            end
        end
    }
)

-- Slider for adjusting fly speed
fly:AddSlider(
    "Fly Speed",
    {
        Text = "Adjust Fly Speed",
        Default = 50,
        Min = 0,
        Max = 100,
        Rounding = 1,
        Compact = false,
        Callback = function(value)
            flySpeed = value  -- Update the fly speed with the slider value
        end
    }
)

local lplr = game.Players.LocalPlayer
local camera = game:GetService("Workspace").CurrentCamera
local CurrentCamera = workspace.CurrentCamera
local worldToViewportPoint = CurrentCamera.worldToViewportPoint

local healthBarEnabled = false -- Use true or false to toggle health bars

-- Toggle Button for Venus (which also controls health bars)
esp:AddToggle(
    "Boxes &amp; Health Bars",
    {
        Text = "Boxes &amp; Health Bars",
        Default = false,
        Tooltip = "Enable or Disable Boxes and Health Bars",
        Callback = function(state)
            healthBarEnabled = state
        end
    }
)

local HeadOff = Vector3.new(0, 0.5, 0)
local LegOff = Vector3.new(0, 3, 0)

local function createJamkleBox(v)
    local BoxOutline = Drawing.new("Square")
    BoxOutline.Visible = false -- Keep this invisible
    BoxOutline.Color = Color3.new(0, 0, 0)
    BoxOutline.Thickness = 3
    BoxOutline.Transparency = 1
    BoxOutline.Filled = false

    local Box = Drawing.new("Square")
    Box.Visible = false -- This will be set to true when the player is visible
    Box.Color = Color3.new(1, 1, 1) -- White box
    Box.Thickness = 1
    Box.Transparency = 1
    Box.Filled = false

    local HealthBarOutline = Drawing.new("Line")
    HealthBarOutline.Thickness = 1.5 -- Stroke thickness
    HealthBarOutline.Color = Color3.new(0, 0, 0) -- Black stroke color
    HealthBarOutline.Visible = false

    local HealthBar = Drawing.new("Line")
    HealthBar.Thickness = 1.5 -- Line thickness for health bar
    HealthBar.Visible = false

    local function boxesp()
        game:GetService("RunService").RenderStepped:Connect(function()
            if v.Character and v.Character:FindFirstChild("Humanoid") and v.Character:FindFirstChild("HumanoidRootPart") and v.Character.Humanoid.Health &gt; 0 then
                local Vector, onScreen = camera:worldToViewportPoint(v.Character.HumanoidRootPart.Position)

                local RootPart = v.Character.HumanoidRootPart
                local Head = v.Character.Head
                local RootPosition, RootVis = worldToViewportPoint(CurrentCamera, RootPart.Position)
                local HeadPosition = worldToViewportPoint(CurrentCamera, Head.Position + HeadOff)
                local LegPosition = worldToViewportPoint(CurrentCamera, RootPart.Position - LegOff)

                if onScreen and healthBarEnabled then
                    -- Remove the BoxOutline drawing
                    -- BoxOutline.Size = Vector2.new(1000 / RootPosition.Z, HeadPosition.Y - LegPosition.Y)
                    -- BoxOutline.Position = Vector2.new(RootPosition.X - BoxOutline.Size.X / 2, RootPosition.Y - BoxOutline.Size.Y / 2)
                    -- BoxOutline.Visible = true -- Keep BoxOutline invisible

                    Box.Size = Vector2.new(1000 / RootPosition.Z, HeadPosition.Y - LegPosition.Y)
                    Box.Position = Vector2.new(RootPosition.X - Box.Size.X / 2, RootPosition.Y - Box.Size.Y / 2)
                    Box.Visible = true -- Only the white box is visible

                    -- Health bar
                    HealthBarOutline.From = Vector2.new(Box.Position.X - 6, Box.Position.Y)
                    HealthBarOutline.To = Vector2.new(Box.Position.X - 6, Box.Position.Y + (HeadPosition.Y - LegPosition.Y))
                    HealthBarOutline.Visible = true

                    local healthRatio = v.Character.Humanoid.Health / v.Character.Humanoid.MaxHealth
                    HealthBar.From = Vector2.new(Box.Position.X - 6, Box.Position.Y)
                    HealthBar.To = Vector2.new(Box.Position.X - 6, Box.Position.Y + (HeadPosition.Y - LegPosition.Y) * healthRatio)
                    HealthBar.Color = Color3.fromRGB(255 * (1 - healthRatio), 255 * healthRatio, 0) -- Health color
                    HealthBar.Visible = true
                else
                    Box.Visible = false -- Hide the white box if conditions aren't met
                    HealthBarOutline.Visible = false
                    HealthBar.Visible = false
                end
            else
                Box.Visible = false -- Hide the white box if the character isn't valid
                HealthBarOutline.Visible = false
                HealthBar.Visible = false
            end
        end)
    end
    coroutine.wrap(boxesp)()
end

for _, v in pairs(game.Players:GetChildren()) do
    createJamkleBox(v)
end

game.Players.PlayerAdded:Connect(function(v)
    createJamkleBox(v)
end)

local lplr = game.Players.LocalPlayer
local camera = game:GetService("Workspace").CurrentCamera
local CurrentCamera = workspace.CurrentCamera
local worldToViewportPoint = CurrentCamera.worldToViewportPoint

_G.TeamCheck = false -- Default team check is off
local tracersEnabled = false -- Default tracers are off

-- Add Toggle for Tracers
esp:AddToggle(
    "Tracers",
    {
        Text = "Tracers",
        Default = false,
        Tooltip = "Enable or disable Tracers",
        Callback = function(state)
            tracersEnabled = state
        end
    }
)

for i,v in pairs(game.Players:GetChildren()) do
    local Tracer = Drawing.new("Line")
    Tracer.Visible = false
    Tracer.Color = Color3.new(1, 1, 1)
    Tracer.Thickness = 1
    Tracer.Transparency = 1

    function lineesp()
        game:GetService("RunService").RenderStepped:Connect(function()
            if tracersEnabled and v.Character ~= nil and v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("HumanoidRootPart") ~= nil and v ~= lplr and v.Character.Humanoid.Health &gt; 0 then
                local Vector, OnScreen = camera:worldToViewportPoint(v.Character.HumanoidRootPart.Position)

                if OnScreen then
                    Tracer.From = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 1)
                    Tracer.To = Vector2.new(Vector.X, Vector.Y)

                    if _G.TeamCheck and v.TeamColor == lplr.TeamColor then
                        -- Teammates
                        Tracer.Visible = false
                    else
                        -- Enemies
                        Tracer.Visible = true
                    end
                else
                    Tracer.Visible = false
                end
            else
                Tracer.Visible = false
            end
        end)
    end
    coroutine.wrap(lineesp)()
end

game.Players.PlayerAdded:Connect(function(v)
    local Tracer = Drawing.new("Line")
    Tracer.Visible = false
    Tracer.Color = Color3.new(1, 1, 1)
    Tracer.Thickness = 1
    Tracer.Transparency = 1

    function lineesp()
        game:GetService("RunService").RenderStepped:Connect(function()
            if tracersEnabled and v.Character ~= nil and v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("HumanoidRootPart") ~= nil and v ~= lplr and v.Character.Humanoid.Health &gt; 0 then
                local Vector, OnScreen = camera:worldToViewportPoint(v.Character.HumanoidRootPart.Position)

                if OnScreen then
                    Tracer.From = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 1)
                    Tracer.To = Vector2.new(Vector.X, Vector.Y)

                    if _G.TeamCheck and v.TeamColor == lplr.TeamColor then
                        -- Teammates
                        Tracer.Visible = false
                    else
                        -- Enemies
                        Tracer.Visible = true
                    end
                else
                    Tracer.Visible = false
                end
            else
                Tracer.Visible = false
            end
        end)
    end
    coroutine.wrap(lineesp)()
end)

-- Separate functionality loop
game:GetService("RunService").Heartbeat:Connect(function()
    if getgenv().FeatureEnabled then
        -- Place your functionality code here
        -- This code will only execute if FeatureEnabled is true
    end
end)

-- Dropdown list for Crosshair Toggle
local CrosshairEnabled = false

visuals:AddToggle(
    "Crosshair",
    {
        Text = "Crosshair",
        Default = false,
        Tooltip = "Enable or disable Crosshair",
        Callback = function(state)
            CrosshairEnabled = state
        end
    }
)

-- Crosshair settings
getgenv().crosshair = {
    enabled = true,
    refreshrate = 0.015,
    mode = 'center',
    position = Vector2.new(0, 0),
    width = 2.5,
    length = 10,
    radius = 11,
    color = Color3.fromRGB(86, 87, 142),  -- Color for the crosshair lines
    spin = true,
    spin_speed = 150,
    spin_max = 340,
    spin_style = Enum.EasingStyle.Circular,
    resize = true,
    resize_speed = 150,
    resize_min = 5,
    resize_max = 22,
}

local old; old = hookfunction(Drawing.new, function(class, properties)
    local drawing = old(class)
    for i, v in next, properties or {} do
        drawing[i] = v
    end
    return drawing
end)

local runservice = game:GetService('RunService')
local inputservice = game:GetService('UserInputService')
local tweenservice = game:GetService('TweenService')
local camera = workspace.CurrentCamera

local last_render = 0

local drawings = {
    crosshair = {},
    text = {
        Drawing.new('Text', {Size = 13, Font = 2, Outline = true, Text = 'Jermx', Color = Color3.new(255, 255, 255)}),
        Drawing.new('Text', {Size = 13, Font = 2, Outline = true, Text = '.CC', Color = Color3.fromRGB(86, 87, 142)}),
    }
}

for idx = 1, 4 do
    drawings.crosshair[idx] = Drawing.new('Line')
    drawings.crosshair[idx + 4] = Drawing.new('Line')
end

function solve(angle, radius)
    return Vector2.new(
        math.sin(math.rad(angle)) * radius,
        math.cos(math.rad(angle)) * radius
    )
end

runservice.PostSimulation:Connect(function()
    local _tick = tick()

    if _tick - last_render &gt; crosshair.refreshrate then
        last_render = _tick

        local position = camera.ViewportSize / 2

        local text_1 = drawings.text[1]
        local text_2 = drawings.text[2]

        text_1.Visible = CrosshairEnabled
        text_2.Visible = CrosshairEnabled

        if CrosshairEnabled then
            local text_x = text_1.TextBounds.X + text_2.TextBounds.X

            text_1.Position = position + Vector2.new(-text_x / 2, crosshair.radius + (crosshair.resize and crosshair.resize_max or crosshair.length) + 5)
            text_2.Position = text_1.Position + Vector2.new(text_1.TextBounds.X, 0)
            text_2.Color = Color3.fromRGB(86, 87, 142)

            for idx = 1, 4 do
                local outline = drawings.crosshair[idx]
                local inline = drawings.crosshair[idx + 4]

                local angle = (idx - 1) * 90
                local length = crosshair.length

                if crosshair.spin then
                    local spin_angle = -_tick * crosshair.spin_speed % crosshair.spin_max
                    angle = angle + tweenservice:GetValue(spin_angle / 360, crosshair.spin_style, Enum.EasingDirection.InOut) * 360
                end

                if crosshair.resize then
                    local resize_length = tick() * crosshair.resize_speed % 180
                    length = crosshair.resize_min + math.sin(math.rad(resize_length)) * crosshair.resize_max
                end

                inline.Visible = true
                inline.Color = Color3.fromRGB(86, 87, 142)  -- Set color for all inline lines
                inline.From = position + solve(angle, crosshair.radius)
                inline.To = position + solve(angle, crosshair.radius + length)
                inline.Thickness = crosshair.width

                outline.Visible = true
                outline.From = position + solve(angle, crosshair.radius - 1)
                outline.To = position + solve(angle, crosshair.radius + length + 1)
                outline.Thickness = crosshair.width + 1.5    
            end
        else
            for idx = 1, 4 do
                drawings.crosshair[idx].Visible = false
                drawings.crosshair[idx + 4].Visible = false
            end
        end
    end
end)

local effectEnabled = false -- Variable to control the toggle state
local forceFieldColor = Color3.fromRGB(128, 0, 128) -- Default force field color (purple)

-- Toggle Button for Effect
visuals:AddToggle(
    "Visuals",
    {
        Text = "Force Field", -- 
        Default = false,
        Tooltip = "Enable or disable the effect on MeshParts ",
        Callback = function(state)
            effectEnabled = state
        end
    }
)

-- Color Picker for Force Field Color
Toggles.Visuals:AddColorPicker(
    "Color",
    {
        Default = Color3.fromRGB(128, 0, 128), -- Default purple color
        Title = "Force Field Color", -- Tooltip for the picker
        Callback = function(color)
            forceFieldColor = color -- Update the color variable dynamically
        end
    }
)

-- Apply effect if enabled
spawn(function()
    while wait() do
        for _, part in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
            if part:IsA("MeshPart") then
                if effectEnabled then
                    part.Material = Enum.Material.ForceField
                    part.Color = forceFieldColor -- Use the dynamically updated color
                else
                    -- Revert to original properties
                    part.Material = Enum.Material.SmoothPlastic -- Change this to the original material you want
                    part.Color = Color3.fromRGB(255, 255, 255) -- Change this to the original color you want (white)
                end
            end
        end
    end
end)

visuals:AddLabel('')

local Lighting = game:GetService("Lighting")
local fogColor = Color3.fromRGB(255, 0, 0) -- Default red fog color

-- Set default fog settings (no fog initially)
Lighting.FogColor = Color3.fromRGB(255, 255, 255) -- No fog color by default
Lighting.FogStart = 10000 -- No fog, set to a very distant start
Lighting.FogEnd = 10000 -- No fog, set to a very distant end
Lighting.Brightness = 1 -- Default brightness
Lighting.OutdoorAmbient = Color3.fromRGB(127, 127, 127) -- Default ambient light

local function enableFog()
    -- Set fog properties for close and tall fog effect
    Lighting.FogColor = fogColor -- Use selected color
    Lighting.FogStart = 0 -- Start fog immediately close to the character
    Lighting.FogEnd = 100 -- Make the fog denser by ending it close

    -- Adjust lighting to make the fog effect more noticeable
    Lighting.Brightness = 0.2 -- Lower brightness to enhance fog intensity
    Lighting.OutdoorAmbient = fogColor -- Match ambient color with fog

    print("Intense fog enabled with color:", fogColor)
end

local function disableFog()
    -- Reset fog properties to default (no fog)
    Lighting.FogColor = Color3.fromRGB(255, 255, 255) -- No fog color
    Lighting.FogStart = 10000 -- Far away, effectively disabling fog
    Lighting.FogEnd = 10000 -- No fog
    Lighting.Brightness = 1 -- Reset brightness to default
    Lighting.OutdoorAmbient = Color3.fromRGB(127, 127, 127) -- Default ambient color

    print("Fog disabled.")
end

-- Toggle button to enable/disable fog
fog:AddToggle(
    "Enable Fog",
    {
        Text = "Enable Fog",
        Default = false, -- Default is false (no fog)
        Tooltip = "Toggle fog effect",
        Callback = function(state)
            if state then
                enableFog() -- Enable fog when toggled on
            else
                disableFog() -- Disable fog when toggled off
            end
        end
    }
)

-- Color picker to change fog color
fog:AddLabel('Fog Color'):AddColorPicker('FogColorPicker', {
    Default = fogColor, -- Start with the default color
    Title = 'Select Fog Color', -- Custom color picker title
    Transparency = nil, -- Transparency disabled

    Callback = function(value)
        fogColor = value -- Update fog color
        if Lighting.FogStart == 0 then -- If fog is enabled
            Lighting.FogColor = fogColor -- Apply the new color
            Lighting.OutdoorAmbient = fogColor -- Update ambient color to match
        end
        print("[cb] Fog color changed to:", fogColor)
    end
})

fog:AddLabel('')

local circle  -- Declare the circle variable outside the function

-- Function to create or get the circle
local function getCircle()
    if not circle then
        -- Initialize the circle if it doesn't exist
        circle = Drawing.new("Circle")
        circle.Position = Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y / 2)  -- Centered on the screen
        circle.Radius = 120  -- Default radius
        circle.Color = Color3.fromRGB(255, 255, 255)  -- Default color (white)
        circle.Thickness = 1
        circle.Filled = false  -- Default filled state
    end
    return circle
end

-- State variable to track circle visibility
local circleEnabled = false

-- State variable to track circle fill
local circleFilled = false

-- Toggle button for circle visibility
fov:AddToggle(
    "Enable Circle", 
    {
        Text = "Enable Circle",
        Default = false,
        Tooltip = "Toggle to enable or disable the circle",
        Callback = function(state)
            circleEnabled = state
            local circleInstance = getCircle()
            circleInstance.Visible = circleEnabled
        end
    }
)

-- Toggle button for filling the circle
fov:AddToggle(
    "Filled",
    {
        Text = "Filled",
        Default = false,
        Tooltip = "Toggle to fill the circle",
        Callback = function(state)
            circleFilled = state
            local circleInstance = getCircle()  -- Get the circle instance
            circleInstance.Filled = circleFilled  -- Update filled state
        end
    }
)

-- Slider for circle size adjustment
fov:AddSlider(
    "Circle Size",
    {
        Min = 20,  -- Minimum size
        Max = 300,  -- Maximum size
        Default = 120,  -- Default size (initial radius)
        Rounding = 0,  -- Add rounding value
        Text = "Circle Size",
        Tooltip = "Adjust the size of the circle",
        Callback = function(size)
            local circleInstance = getCircle()  -- Get the circle instance
            if circleInstance then  -- Ensure circleInstance is valid
                circleInstance.Radius = size  -- Set the new radius
            end
        end
    }
)

-- Color Picker to change circle color
fov:AddLabel('Circle Color'):AddColorPicker('ColorPicker', {
    Default = circle and circle.Color or Color3.fromRGB(255, 255, 255),  -- Start with the current circle color or default to white
    Title = 'Circle Color',  -- Title for the color picker
    Transparency = nil,  -- Optional. Enables transparency changing for this color picker (leave as nil to disable)

    Callback = function(Value)
        local circleInstance = getCircle()  -- Get the circle instance
        if circleInstance then  -- Ensure circleInstance is valid
            circleInstance.Color = Value  -- Change the circle color
        end
        print('[cb] Color changed!', Value)
    end
})

fov:AddLabel('')

-- Initial drawing of the circle based on the default state
local circleInstance = getCircle()
circleInstance.Visible = circleEnabled








local MenuGroup = Tabs['UI Settings']:AddRightGroupbox('Menu')

MenuGroup:AddButton('Unload', function() Library:Unload() end)
MenuGroup:AddLabel('Menu bind'):AddKeyPicker('MenuKeybind', { Default = 'End', NoUI = true, Text = 'Menu keybind' })

Library.ToggleKeybind = Options.MenuKeybind

ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)

SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({ 'MenuKeybind' })

ThemeManager:SetFolder('failurty.CC')
SaveManager:SetFolder('failurty/configs')

SaveManager:BuildConfigSection(Tabs['UI Settings'])
ThemeManager:ApplyToTab(Tabs['UI Settings'])

SaveManager:LoadAutoloadConfig()










local player = game.Players.LocalPlayer

local userInputService = game:GetService("UserInputService")
local Players = game:GetService("Players")
local RS = game:GetService("RunService")
local WS = game:GetService("Workspace")
local GS = game:GetService("GuiService")


local LP = Players.LocalPlayer
local Mouse = LP:GetMouse()
local Camera = WS.CurrentCamera
local GetGuiInset = GS.GetGuiInset

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")

local LocalPlayer = Players.LocalPlayer
local Camera = Workspace.CurrentCamera


getgenv().venus = {
    ["Enabled"] = true,
    ["AimPart"] = "Head",
    ["Prediction"] = 0.12588,
    ["Smoothness"] = 1,
    ["ShakeValue"] = 0,
    ["AutoPred"] = true,
    ["Loaded"] = false,
    ["TTKO"] = false,
    ["Mode"] = "Controller",
    ["cframe"] = {
        ["enabled"] = false,
        ["speed"] = 2,
        ["TargetStrafe"] = {
            ["Enabled"] = false,
            ["StrafeSpeed"] = 10,
            ["StrafeRadius"] = 7,
            ["StrafeHeight"] = 3,
            ["RandomizerMode"] = true
        }
    }
}

getgenv().Fov = {
    ["FOVSize"] = 90,
    ["FOVColor"] = Color3.fromRGB(255, 0, 0),
    ["FOVVisible"] = true,
    ["FOVShape"] = "Circle"
}

getgenv().targetaim= {
    ["enabled"] = true,
    ["targetPart"] = "UpperTorso",
    ["prediction"] = 0.12588
}

getgenv().desync = {
    ["sky"] = false,
    ["invis"] = true,
    ["jump"] = false,
    ["network"] = false
}

getgenv().Misc = {
    ["LowGfx"] = false,
}

getgenv().FPSunlocker = {
    ["Enabled"] = true,
    ["FPSCap"] = 999
}

getgenv().Triggerbot = {
    ["ClosestPart"] = {
        ["HitParts"] = {"Head", "UpperTorso", "LowerTorso", "HumanoidRootPart", "RightFoot", "LeftFoot"}
    },
    ["FOV"] = {
        ["Enabled"] = true,
        ["Size"] = 13,
        ["Centered FOV"] = true,
        ["Visible"] = false,
        ["Filled"] = false,
        ["Color"] = Color3.fromRGB(255, 0, 0)
    },
    ["Settings"] = {
        ["Prediction"] = 0.111,
        ["Click Delay"] = 0.1,
        ["Activation Delay"] = 2,
        ["IgnoreFriends"] = false,
        ["Automatically Fire"] = false,
    }
}

local userInputService = game:GetService("UserInputService")

local AimlockState = true
local Locked = false
local Victim
local target

if venus.Loaded then
    notify("Venus.CC is Already Loaded!")
    return
end

venus.Loaded = true

local function GetClosestPlayer()
    local closestPlayer = nil
    local shortestScore = math.huge
    local centerScreen = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    local maxAngle = 30 -- Maximum angle limit in degrees (adjust this for wider or narrower locking range)

    for _, plr in pairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild(venus.AimPart) and plr.Character:FindFirstChild("Humanoid") and plr.Character.Humanoid.Health &gt; 0 then
            local part = plr.Character[venus.AimPart]
            local relativePos = part.Position - Camera.CFrame.Position
            local playerDistance = relativePos.Magnitude
            local screenPosition, onScreen = Camera:WorldToViewportPoint(part.Position)

            if onScreen then
                local angle = math.deg(math.acos(Camera.CFrame.LookVector:Dot(relativePos.Unit)))
                
                -- Only consider players within a specific angle range (in front of you)
                if angle &lt;= maxAngle then
                    local mouseDistance = (Vector2.new(screenPosition.X, screenPosition.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude

                    -- Adjust score to prioritize players more in front and reduce side locks
                    local score = mouseDistance * 0.02 + angle * 0.03 -- Increase angle weight to penalize wider angles
                    
                    if angle &lt; 5 then
                        score = score * 0.001 -- Further reduce score for players directly in front
                    end

                    local ray = Ray.new(Camera.CFrame.Position, relativePos.Unit * playerDistance)
                    local hitPart = WS:FindPartOnRayWithIgnoreList(ray, {player.Character})

                    if (not hitPart or hitPart:IsDescendantOf(plr.Character)) and score &lt; shortestScore then
                        closestPlayer = plr
                        shortestScore = score
                    end
                end
            end
        end
    end

    return closestPlayer
end

local notificationsEnabled = false -- Default state for notifications

assist:AddToggle(
    "Enable Notifications",
    {
        Text = "Notifications",
        Default = false,
        Tooltip = "Enable or disable notifications",
        Callback = function(state)
            notificationsEnabled = state
        end
    }
)

local function ToggleLock()
    if AimlockState then
        Locked = not Locked
        if Locked then
            -- Change image when locked
            if LockButton then
                LockButton.Image = "rbxassetid://78342062013795" -- Image for the lock when locked
            end
            Victim = GetClosestPlayer()
            target = Victim
            if Victim then
                if venus.Enabled and notificationsEnabled then
                    NotificationSystem:Notify("Camlock: Locked onto " .. tostring(Victim.Name), 5)
                elseif targetaim.enabled and notificationsEnabled then
                    NotificationSystem:Notify("Target Lock: Locked onto " .. tostring(target.Name), 5)
                end
            else
                if venus.Enabled and notificationsEnabled then
                    NotificationSystem:Notify("Camlock: No target found", 5)
                elseif targetaim.enabled and notificationsEnabled then
                    NotificationSystem:Notify("Target Lock: No target found", 5)
                end
            end
        else
            -- Change image when unlocked
            if LockButton then
                LockButton.Image = "rbxassetid://134820707156642" -- Image for the lock when unlocked
            end
            Victim = nil
            target = nil
            if venus.Enabled and notificationsEnabled then
                NotificationSystem:Notify("Camlock: Unlocked!", 5)
            elseif targetaim.enabled and notificationsEnabled then
                NotificationSystem:Notify("Target Lock: Unlocked!", 5)
            end
        end
    else
        if not venus.Enabled and notificationsEnabled then
            NotificationSystem:Notify("Camlock not enabled", 5)
        end
    end
end

local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local userInputService = game:GetService("UserInputService")

local LockButton

-- Function for the "Button" mode
function spawnButton()
    local playerGui = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    
    local function setupGui()
        local screenGui = playerGui:FindFirstChild("LockScreenGui")

        if not screenGui then
            screenGui = Instance.new("ScreenGui")
            screenGui.Name = "LockScreenGui"
            screenGui.Parent = playerGui
        end

        LockButton = screenGui:FindFirstChild("LockButton")

        if not LockButton then
            LockButton = Instance.new("ImageButton")
            LockButton.Name = "LockButton"
            LockButton.Size = UDim2.new(0, 80, 0, 80)
            LockButton.Position = UDim2.new(0.5, -250, 0.8, -225)
            LockButton.Image = "rbxassetid://134820707156642" -- Default image for the lock (not locked)
            LockButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
            LockButton.BackgroundTransparency = 0.450
            LockButton.Parent = screenGui
            LockButton.Active = true
            LockButton.Draggable = true

            local UICorner = Instance.new("UICorner")
            UICorner.CornerRadius = UDim.new(0, 10)
            UICorner.Parent = LockButton

            LockButton.MouseButton1Click:Connect(function()
                ToggleLock() -- Trigger ToggleLock when clicked
            end)
        end
    end

    setupGui()

    -- Re-setup the GUI when the character respawns
    game.Players.LocalPlayer.CharacterAdded:Connect(function()
        setupGui()
    end)
end

-- Immediately call the function to create the button
spawnButton()

-- Re-setup the GUI when the character respawns
game.Players.LocalPlayer.CharacterAdded:Connect(function()
    spawnButton()
end)

RS.RenderStepped:Connect(function()
    if AimlockState and Victim and Victim.Character and Victim.Character:FindFirstChild(venus.AimPart) then
        local aimPart = Victim.Character[venus.AimPart]

        -- Single Prediction Calculation
        local predictedPosition = aimPart.Position 
            + aimPart.Velocity * venus.Prediction
            + Vector3.new(0, venus.JumpOffset, 0)

        -- Look at the target smoothly only if LookAt is enabled and target aim is active
        if venus.LookAtEnabled and targetaim.enabled then
            local lookPosition = CFrame.new(Camera.CFrame.p, predictedPosition)
            Camera.CFrame = Camera.CFrame:Lerp(lookPosition, venus.Smoothness)

            local playerHRP = player.Character.HumanoidRootPart
            playerHRP.CFrame = CFrame.new(playerHRP.Position, Vector3.new(predictedPosition.X, playerHRP.Position.Y, predictedPosition.Z))
        end

        -- Camlock functionality
        if AimlockState then
            local playerHRP = player.Character.HumanoidRootPart
            local camlockPosition = CFrame.new(Camera.CFrame.p, predictedPosition)
            Camera.CFrame = Camera.CFrame:Lerp(camlockPosition, venus.Smoothness)

            -- Target strafe functionality (Orbiting)
            if venus.cframe.TargetStrafe.Enabled then
                local lp = player.Character
                local targpos = Victim.Character.HumanoidRootPart.Position
                
                -- Calculate strafe offset for orbiting
                local angle = tick() * venus.cframe.TargetStrafe.StrafeSpeed
                local strafeOffset = Vector3.new(
                    math.cos(angle) * venus.cframe.TargetStrafe.StrafeRadius,
                    venus.cframe.TargetStrafe.Height,
                    math.sin(angle) * venus.cframe.TargetStrafe.StrafeRadius
                )

                local strafePosition = targpos + strafeOffset
                strafePosition = Vector3.new(strafePosition.X, math.max(strafePosition.Y, targpos.Y), strafePosition.Z)
                
                lp:SetPrimaryPartCFrame(CFrame.new(strafePosition))
                playerHRP.CFrame = CFrame.new(
                    playerHRP.Position,
                    Vector3.new(targpos.X, playerHRP.Position.Y, targpos.Z)
                )
            end
        end

        -- Auto Air functionality
        if venus.AutoAirEnabled then
            local TargetRootPart = Victim.Character:FindFirstChild("HumanoidRootPart")
            if TargetRootPart then
                local TargetVel = TargetRootPart.Velocity
                if TargetVel.Y &gt; 25 then
                    local Character = LocalPlayer.Character
                    if Character then
                        local Tool = Character:FindFirstChildOfClass("Tool")
                        if Tool then
                            Tool:Activate()
                        end
                    end
                end
            end
        end
    end
end)

-- Metatable Hook for Target Aim Adjustments
local mt = getrawmetatable(game)
local oldNameCall = mt.__namecall
setreadonly(mt, false)

mt.__namecall = newcclosure(function(Self, ...)
    local args = {...}
    local methodName = getnamecallmethod()
    if not checkcaller() and methodName == "FireServer" and targetaim.enabled then
        for i, Argument in ipairs(args) do
            if typeof(Argument) == "Vector3" and target and target.Character then
                local targetPart = target.Character[targetaim.targetPart]
                if targetPart then
                    args[i] = targetPart.Position 
                        + targetPart.Velocity * venus.Prediction
                        + Vector3.new(0, venus.JumpOffset, 0)
                    return oldNameCall(Self, unpack(args))
                end
            end
        end
    end
    return oldNameCall(Self, ...)
end)

setreadonly(mt, true)

while task.wait() do
    if venus.Enabled and venus.AutoPred then
        local pingValue = game:GetService("Stats").Network.ServerStatsItem["Data Ping"]:GetValueString()
        local ping = tonumber((pingValue:match("%d+")))

        if ping then
            if ping &gt; 225 then
                venus.Prediction = 0.166547
            elseif ping &gt; 215 then
                venus.Prediction = 0.15692
            elseif ping &gt; 205 then
                venus.Prediction = 0.165732
            elseif ping &gt; 190 then
                venus.Prediction = 0.1690
            elseif ping &gt; 185 then
                venus.Prediction = 0.1235666
            elseif ping &gt; 180 then
                venus.Prediction = 0.16779123
            elseif ping &gt; 175 then
                venus.Prediction = 0.165455312399999
            elseif ping &gt; 170 then
                venus.Prediction = 0.16
            elseif ping &gt; 165 then
                venus.Prediction = 0.15
            elseif ping &gt; 160 then
                venus.Prediction = 0.1223333
            elseif ping &gt; 155 then
                venus.Prediction = 0.125333
            elseif ping &gt; 150 then
                venus.Prediction = 0.1652131
            elseif ping &gt; 145 then
                venus.Prediction = 0.129934
            elseif ping &gt; 140 then
                venus.Prediction = 0.1659921
            elseif ping &gt; 135 then
                venus.Prediction = 0.1659921
            elseif ping &gt; 130 then
                venus.Prediction = 0.12399
            elseif ping &gt; 125 then
                venus.Prediction = 0.15465
            elseif ping &gt; 110 then
                venus.Prediction = 0.142199
            elseif ping &gt; 105 then
                venus.Prediction = 0.141199
            elseif ping &gt; 100 then
                venus.Prediction = 0.134143
            elseif ping &gt; 90 then
                venus.Prediction = 0.1433333333392
            elseif ping &gt; 80 then
                venus.Prediction = 0.143214443
            elseif ping &gt; 70 then
                venus.Prediction = 0.14899911
            elseif ping &gt; 60 then
                venus.Prediction = 0.148325
            elseif ping &gt; 50 then
                venus.Prediction = 0.128643
            elseif ping &gt; 40 then
                venus.Prediction = 0.12766
            elseif ping &gt; 30 then
                venus.Prediction = 0.124123
            elseif ping &gt; 20 then
                venus.Prediction = 0.12435
            elseif ping &gt; 10 then
                venus.Prediction = 0.1234555
            elseif ping &lt; 10 then
                venus.Prediction = 0.1332
            else
                venus.Prediction = 0.1342
            end
        end
    end
end






if desync.sky == true then
    getgenv().VenusSky = true 
    getgenv().SkyAmount = 90

    game:GetService("RunService").Heartbeat:Connect(function()
        if getgenv().VenusSky then 
            local vel = game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity
            game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0, getgenv().SkyAmount, 0) 
            game:GetService("RunService").RenderStepped:Wait()
            game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = vel
        end
    end)
end

if desync.jump == true then
    getgenv().jumpanti = true
    
    game:GetService("RunService").Heartbeat:Connect(function()
        if getgenv().jumpanti then    
            local CurrentVelocity = game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity
            game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(1000, 1000, 1000)
            game:GetService("RunService").RenderStepped:Wait()
            game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = CurrentVelocity
        end
    end)
end

if desync.jump == true then

-- Maximum Roblox velocity (128^2 or 16384)
local velMax = (128 ^ 2)

local timeRelease, timeChoke = 0.015, 0.105

local Property, Wait = sethiddenproperty, task.wait
local Radian, Random, Ceil = math.rad, math.random, math.ceil
local Angle = CFrame.Angles
local Vector = Vector3.new
local Service = game.GetService

local Run = Service(game, 'RunService')
local Stats = Service(game, 'Stats')
local Players = Service(game, 'Players')
local LocalPlayer = Players.LocalPlayer
local statPing = Stats.PerformanceStats.Ping
local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
local Root = Character:WaitForChild("HumanoidRootPart")
local Mouse = LocalPlayer:GetMouse()

local runRen, runBeat = Run.RenderStepped, Run.Heartbeat
local runRenWait, runRenCon = runRen.Wait, runRen.Connect
local runBeatCon = runBeat.Connect

local function Ping()
    return statPing:GetValue()
end

local function Sleep()
    Property(Root, 'NetworkIsSleeping', true)
end

local function FireGun()
    local tool = LocalPlayer.Character:FindFirstChildOfClass("Tool")
    if tool and tool:FindFirstChild("Shoot") then
        local ShootEvent = tool.Shoot
        ShootEvent:FireServer(Mouse.Hit.Position)
    end
end

local function Init()
    if not Root then return end

    local rootVel = Root.Velocity
    local rootCFrame = Root.CFrame

   
    local rootAng = Random(-180, 180)
    local rootOffset do
        local X = Random(-velMax, velMax)
        local Y = Random(0, velMax)
        local Z = Random(-velMax, velMax)
        rootOffset = Vector(X, -Y, Z)
    end

    Root.CFrame = Angle(0, Radian(rootAng), 0)
    Root.Velocity = rootOffset

   
    FireGun()


    runRenWait(runRen)
    Root.CFrame = rootCFrame
    Root.Velocity = rootVel
end

runBeatCon(runBeat, Init)

-- Main loop for choking replication
while Wait(timeRelease) do
    -- Stable replication packets
    local chokeClient, chokeServer = runBeatCon(runBeat, Sleep), runRenCon(runRen, Sleep)

    Wait(Ceil(Ping()) / 1000)

    chokeClient:Disconnect()
    chokeServer:Disconnect()

end
end

if desync.network == true then
local RunService = game:GetService("RunService")

local function onHeartbeat()
    setfflag("S2PhysicsSenderRate", 1)
end

RunService.Heartbeat:Connect(onHeartbeat)
end

if Misc.LowGfx == true then
game:GetService("CorePackages").Packages:Destroy()
end
