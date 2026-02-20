
--[[
    ╔══════════════════════════════════════════════════════════╗
    ║                 Muscle Legends Companion                 ║
    ║                    Version: 4.0.0                        ║
    ║                 Developed by: MyersDev                   ║
    ║      All rights reserved © 2024 - Professional Edition    ║
    ╚══════════════════════════════════════════════════════════╝
]]

-- ================================================================
--                          INITIALIZATION
-- ================================================================

local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xccxk/LIBPHEOX/refs/heads/main/BYMYERS", true))()

local window = library:AddWindow("Private MyersDev", {
    main_color = Color3.fromRGB(0, 0, 0),
    min_size = Vector2.new(650, 870),
    can_resize = true,
})

-- ================================================================
--                          SERVICES
-- ================================================================

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local Stats = game:GetService("Stats")
local Lighting = game:GetService("Lighting")
local VirtualInputManager = game:GetService("VirtualInputManager")
local Workspace = game:GetService("Workspace")
local StarterGui = game:GetService("StarterGui")

local LocalPlayer = Players.LocalPlayer

-- ================================================================
--                           UTILITIES
-- ================================================================

local function FormatNumber(value)
    local suffixes = {"", "K", "M", "B", "T", "Qa", "Qi"}
    local index = 1
    while value >= 1000 and index < #suffixes do
        value = value / 1000
        index = index + 1
    end
    return string.format("%.2f%s", value, suffixes[index])
end

local function FormatNumberWithCommas(value)
    local formatted = tostring(value):reverse():gsub("(%d%d%d)", "%1,"):reverse()
    return formatted:gsub("^,", "")
end

local function FormatDisplay(value)
    local normal = FormatNumberWithCommas(value)
    local abbreviated = FormatNumber(value)
    return "[ " .. normal .. " | " .. abbreviated .. " ]"
end

-- ================================================================
--                           AUTO HP BILLBOARD
-- ================================================================

task.spawn(function()
    repeat task.wait() until game:IsLoaded()
    task.wait(1)

    local suffixes = {
        {"Qi", 1e18},
        {"Qa", 1e15},
        {"T",  1e12},
        {"B",  1e9},
        {"M",  1e6},
        {"K",  1e3},
    }

    local function formatNumber(n)
        for _, s in ipairs(suffixes) do
            local name, val = s[1], s[2]
            if n >= val then
                return string.format("%.2f%s", n / val, name)
            end
        end
        return tostring(math.floor(n))
    end

    local function createBillboard(humanoid, character)
        if not character:FindFirstChild("Head") then return end

        local old = character:FindFirstChild("HP_BILLBOARD")
        if old then old:Destroy() end

        local billboard = Instance.new("BillboardGui")
        billboard.Name = "HP_BILLBOARD"
        billboard.Size = UDim2.new(6, 0, 1.2, 0)
        billboard.StudsOffset = Vector3.new(0, 3.5, 0)
        billboard.AlwaysOnTop = true
        billboard.MaxDistance = 200
        billboard.Parent = character.Head

        local frame = Instance.new("Frame")
        frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
        frame.BackgroundTransparency = 0.3
        frame.Size = UDim2.new(1, 0, 1, 0)
        frame.BorderSizePixel = 0
        frame.Parent = billboard

        local corner = Instance.new("UICorner")
        corner.CornerRadius = UDim.new(0, 8)
        corner.Parent = frame

        local bar = Instance.new("Frame")
        bar.BackgroundColor3 = Color3.fromRGB(255, 70, 70)
        bar.Size = UDim2.new(1, 0, 1, 0)
        bar.BorderSizePixel = 0
        bar.Parent = frame

        local gradient = Instance.new("UIGradient")
        gradient.Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
            ColorSequenceKeypoint.new(1, Color3.fromRGB(0, 0, 0))
        })
        gradient.Parent = bar

        local stroke = Instance.new("UIStroke")
        stroke.Thickness = 2
        stroke.Color = Color3.fromRGB(0, 0, 0)
        stroke.Transparency = 0.7
        stroke.Parent = bar

        local label = Instance.new("TextLabel")
        label.BackgroundTransparency = 1
        label.Size = UDim2.new(1, 0, 1, 0)
        label.TextColor3 = Color3.new(1, 1, 1)
        label.Font = Enum.Font.GothamBold
        label.TextSize = 18
        label.TextStrokeTransparency = 0.5
        label.Parent = frame

        local textConstraint = Instance.new("UITextSizeConstraint")
        textConstraint.MaxTextSize = 24
        textConstraint.MinTextSize = 14
        textConstraint.Parent = label

        local function updateHP()
            if humanoid and humanoid.Parent then
                local hp = humanoid.Health
                local max = humanoid.MaxHealth
                bar.Size = UDim2.new(hp / max, 0, 1, 0)
                label.Text = formatNumber(hp) .. " / " .. formatNumber(max)
            end
        end

        humanoid.HealthChanged:Connect(updateHP)
        updateHP()
    end

    local function onCharacterAdded(character)
        local humanoid = character:WaitForChild("Humanoid")
        createBillboard(humanoid, character)
    end

    for _, plr in ipairs(Players:GetPlayers()) do
        if plr.Character then
            onCharacterAdded(plr.Character)
        end
        plr.CharacterAdded:Connect(onCharacterAdded)
    end

    Players.PlayerAdded:Connect(function(plr)
        plr.CharacterAdded:Connect(onCharacterAdded)
    end)
end)

-- ================================================================
--                            TAB: FARM OP
-- ================================================================

local AutoFarm = window:AddTab("Farm Op")

-- Strength Op for High Ping (250ms+)
getgenv()._AutoRepFarmEnabled = false

AutoFarm:AddSwitch("Strength Op (Minimum ping: 250ms+)", function(state)
    getgenv()._AutoRepFarmEnabled = state
    warn("[Auto Rep Farm] Status:", state and "ON" or "OFF")
end)

-- Configuration
local PET_NAME = "Swift Samurai"
local ROCK_NAME = "Rock5M"
local PROTEIN_EGG_NAME = "ProteinEgg"
local PROTEIN_EGG_INTERVAL = 30 * 60
local REPS_PER_CYCLE = 10
local REP_DELAY = 0.01
local ROCK_INTERVAL = 5
local MAX_PING = 450
local MIN_PING = 250

-- Internal Variables
local HumanoidRootPart
local lastProteinEggTime = 0
local lastRockTime = 0

-- Functions
local function getPing()
    local success, ping = pcall(function()
        return Stats.Network.ServerStatsItem["Data Ping"]:GetValue()
    end)
    return success and ping or 999
end

local function updateCharacterRefs()
    local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    HumanoidRootPart = character:WaitForChild("HumanoidRootPart", 5)
end

local function equipPet()
    local petsFolder = LocalPlayer:FindFirstChild("petsFolder")
    if petsFolder and petsFolder:FindFirstChild("Unique") then
        for _, pet in pairs(petsFolder.Unique:GetChildren()) do
            if pet.Name == PET_NAME then
                ReplicatedStorage.rEvents.equipPetEvent:FireServer("equipPet", pet)
                break
            end
        end
    end
end

local function eatProteinEgg()
    if LocalPlayer:FindFirstChild("Backpack") then
        for _, item in pairs(LocalPlayer.Backpack:GetChildren()) do
            if item.Name == PROTEIN_EGG_NAME then
                ReplicatedStorage.rEvents.eatEvent:FireServer("eat", item)
                break
            end
        end
    end
end

local function hitRock()
    local rock = Workspace:FindFirstChild(ROCK_NAME)
    if rock and HumanoidRootPart then
        HumanoidRootPart.CFrame = rock.CFrame * CFrame.new(0, 0, -5)
        ReplicatedStorage.rEvents.hitEvent:FireServer("hit", rock)
    end
end

-- Main Loop
task.spawn(function()
    updateCharacterRefs()
    equipPet()
    lastProteinEggTime = tick()
    lastRockTime = tick()

    local farmingPaused = false

    while true do
        if getgenv()._AutoRepFarmEnabled then
            local ping = getPing()

            if ping > MAX_PING then
                if not farmingPaused then
                    warn("[Auto Rep Farm] High ping ("..math.floor(ping).."ms), pausing...")
                    farmingPaused = true
                end
            end

            if ping <= MIN_PING then
                if farmingPaused then
                    warn("[Auto Rep Farm] Low ping ("..math.floor(ping).."ms), resuming...")
                    farmingPaused = false
                end
            end

            if not farmingPaused then
                if LocalPlayer:FindFirstChild("muscleEvent") then
                    for i = 1, REPS_PER_CYCLE do
                        LocalPlayer.muscleEvent:FireServer("rep")
                    end
                end

                if tick() - lastProteinEggTime >= PROTEIN_EGG_INTERVAL then
                    eatProteinEgg()
                    lastProteinEggTime = tick()
                end

                if tick() - lastRockTime >= ROCK_INTERVAL then
                    hitRock()
                    lastRockTime = tick()
                end
            end
        end

        task.wait(REP_DELAY)
    end
end)

-- Strength Op for Low Ping (150ms-)
getgenv()._AutoRepFarmEnabled2 = false

AutoFarm:AddSwitch("Strength Op (Minimum ping: <150ms)", function(state)
    getgenv()._AutoRepFarmEnabled2 = state
    warn("[Auto Rep Farm 2] Status:", state and "ON" or "OFF")
end)

-- Configuration 2
local REPS_PER_CYCLE2 = 40
local MAX_PING2 = 5000
local MIN_PING2 = 100

-- Second Main Loop
task.spawn(function()
    local lastProteinEggTime2 = tick()
    local lastRockTime2 = tick()
    local farmingPaused2 = false

    while true do
        if getgenv()._AutoRepFarmEnabled2 then
            local ping = getPing()

            if ping > MAX_PING2 then
                if not farmingPaused2 then
                    warn("[Auto Rep Farm 2] High ping ("..math.floor(ping).."ms), pausing...")
                    farmingPaused2 = true
                end
            end

            if ping <= MIN_PING2 then
                if farmingPaused2 then
                    warn("[Auto Rep Farm 2] Low ping ("..math.floor(ping).."ms), resuming...")
                    farmingPaused2 = false
                end
            end

            if not farmingPaused2 then
                if LocalPlayer:FindFirstChild("muscleEvent") then
                    for i = 1, REPS_PER_CYCLE2 do
                        LocalPlayer.muscleEvent:FireServer("rep")
                    end
                end

                if tick() - lastProteinEggTime2 >= PROTEIN_EGG_INTERVAL then
                    eatProteinEgg()
                    lastProteinEggTime2 = tick()
                end

                if tick() - lastRockTime2 >= ROCK_INTERVAL then
                    hitRock()
                    lastRockTime2 = tick()
                end
            end
        end

        task.wait(REP_DELAY)
    end
end)

-- Auto Eat Protein Egg (30 minutes)
local autoEatEnabled = false

AutoFarm:AddSwitch("Auto Eat Egg (30 Min)", function(state)
    autoEatEnabled = state
end)

task.spawn(function()
    while true do
        if autoEatEnabled then
            eatProteinEgg()
            task.wait(1800)
        else
            task.wait(1)
        end
    end
end)

-- Spin Fortune Wheel
AutoFarm:AddSwitch("Spin Fortune Wheel", function(state)
    _G.AutoSpinWheel = state

    if state then
        spawn(function()
            while _G.AutoSpinWheel and task.wait(0.1) do
                ReplicatedStorage.rEvents.openFortuneWheelRemote:InvokeServer(
                    "openFortuneWheel",
                    ReplicatedStorage.fortuneWheelChances["Fortune Wheel"]
                )
            end
        end)
    end
end)

-- Visual Optimization
AutoFarm:AddSwitch("Hide All Frames", function(bool)
    for _, obj in pairs(ReplicatedStorage:GetChildren()) do
        if obj.Name:match("Frame$") then
            obj.Visible = not bool
        end
    end
end)

-- Anti Lag Button
AutoFarm:AddButton("Anti Lag", function()
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("ParticleEmitter") or v:IsA("Smoke") or v:IsA("Fire") or v:IsA("Sparkles") then
            v.Enabled = false
        end
    end

    Lighting.GlobalShadows = false
    Lighting.FogEnd = 9e9
    Lighting.Brightness = 0

    settings().Rendering.QualityLevel = 1

    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("Decal") or v:IsA("Texture") then
            v.Transparency = 1
        elseif v:IsA("BasePart") and not v:IsA("MeshPart") then
            v.Material = Enum.Material.SmoothPlastic
            if v.Parent and (v.Parent:FindFirstChild("Humanoid") or v.Parent.Parent:FindFirstChild("Humanoid")) then
            else
                v.Reflectance = 0
            end
        end
    end

    for _, v in pairs(Lighting:GetChildren()) do
        if v:IsA("BlurEffect") or v:IsA("SunRaysEffect") or v:IsA("ColorCorrectionEffect") or v:IsA("BloomEffect") or v:IsA("DepthOfFieldEffect") then
            v.Enabled = false
        end
    end

    StarterGui:SetCore("SendNotification", {
        Title = "Anti Lag Activated",
        Text = "Full optimization applied!",
        Duration = 5
    })
end)

-- Full Optimization Button
AutoFarm:AddButton("Full Optimization", function()
    local playerGui = LocalPlayer:WaitForChild("PlayerGui")

    for _, gui in pairs(playerGui:GetChildren()) do
        if gui:IsA("ScreenGui") then
            gui:Destroy()
        end
    end

    local function darkenSky()
        for _, v in pairs(Lighting:GetChildren()) do
            if v:IsA("Sky") then
                v:Destroy()
            end
        end

        local darkSky = Instance.new("Sky")
        darkSky.Name = "DarkSky"
        darkSky.SkyboxBk = "rbxassetid://0"
        darkSky.SkyboxDn = "rbxassetid://0"
        darkSky.SkyboxFt = "rbxassetid://0"
        darkSky.SkyboxLf = "rbxassetid://0"
        darkSky.SkyboxRt = "rbxassetid://0"
        darkSky.SkyboxUp = "rbxassetid://0"
        darkSky.Parent = Lighting

        Lighting.Brightness = 0
        Lighting.ClockTime = 0
        Lighting.TimeOfDay = "00:00:00"
        Lighting.OutdoorAmbient = Color3.new(0, 0, 0)
        Lighting.Ambient = Color3.new(0, 0, 0)
        Lighting.FogColor = Color3.new(0, 0, 0)
        Lighting.FogEnd = 100

        task.spawn(function()
            while true do
                wait(5)
                if not Lighting:FindFirstChild("DarkSky") then
                    darkSky:Clone().Parent = Lighting
                end
                Lighting.Brightness = 0
                Lighting.ClockTime = 0
                Lighting.OutdoorAmbient = Color3.new(0, 0, 0)
                Lighting.Ambient = Color3.new(0, 0, 0)
                Lighting.FogColor = Color3.new(0, 0, 0)
                Lighting.FogEnd = 100
            end
        end)
    end

    local function removeParticleEffects()
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("ParticleEmitter") then
                obj:Destroy()
            end
        end
    end

    local function removeLightSources()
        for _, obj in pairs(Workspace:GetDescendants()) do
            if obj:IsA("PointLight") or obj:IsA("SpotLight") or obj:IsA("SurfaceLight") then
                obj:Destroy()
            end
        end
    end

    removeParticleEffects()
    removeLightSources()
    darkenSky()
end)

-- Equip Swift Samurai Button
AutoFarm:AddButton("Equip Swift Samurai (Max 8)", function()
    local petsFolder = LocalPlayer:FindFirstChild("petsFolder")
    if not petsFolder then return end

    for _, folder in pairs(petsFolder:GetChildren()) do
        if folder:IsA("Folder") then
            for _, pet in pairs(folder:GetChildren()) do
                ReplicatedStorage.rEvents.equipPetEvent:FireServer("unequipPet", pet)
            end
        end
    end
    task.wait(0.1)

    local equipped = 0
    local maxEquip = 8
    for _, folder in pairs(petsFolder:GetChildren()) do
        if folder:IsA("Folder") then
            for _, pet in pairs(folder:GetChildren()) do
                if pet.Name == "Swift Samurai" then
                    ReplicatedStorage.rEvents.equipPetEvent:FireServer("equipPet", pet)
                    equipped += 1

                    if equipped >= maxEquip then
                        return
                    end
                end
            end
        end
    end
end)

-- Teleport Buttons
AutoFarm:AddButton("Jungle Squat", function()
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")
    hrp.CFrame = CFrame.new(-8371.4336, 6.7981, 2858.8853)
    task.wait(0.2)
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
    task.wait(0.05)
    VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
end)

AutoFarm:AddButton("Jungle Lift", function()
    local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
    local hrp = char:WaitForChild("HumanoidRootPart")
    hrp.CFrame = CFrame.new(-8652.8672, 29.2667, 2089.2617)
    task.wait(0.2)
    VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
    task.wait(0.05)
    VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
end)

-- Rebirths Tracking
AutoFarm:AddLabel("Rebirths Gained").TextSize = 23

local fastStrengthFolder = AutoFarm:AddFolder("Fast Rebirths Functions")

local leaderstats = LocalPlayer:WaitForChild("leaderstats")
local rebirthsStat = leaderstats:WaitForChild("Rebirths")

local footerTimeLabel = fastStrengthFolder:AddLabel("0d 0h 0m 0s")
footerTimeLabel.TextSize = 18
local footerRebirthsLabel = fastStrengthFolder:AddLabel("Rebirths: 0")
footerRebirthsLabel.TextSize = 18
local footerRebirthsGainedLabel = fastStrengthFolder:AddLabel("Rebirths Gained: 0")
footerRebirthsGainedLabel.TextSize = 18

local startTime = tick()
local initialRebirths = rebirthsStat.Value

task.spawn(function()
    while true do
        local elapsed = tick() - startTime
        local days = math.floor(elapsed / 86400)
        local hours = math.floor((elapsed % 86400) / 3600)
        local minutes = math.floor((elapsed % 3600) / 60)
        local seconds = math.floor(elapsed % 60)
        footerTimeLabel.Text = string.format("%dd %dh %dm %ds", days, hours, minutes, seconds)
        task.wait(1)
    end
end)

local function updateRebirths()
    local current = rebirthsStat.Value
    local gained = current - initialRebirths
    footerRebirthsLabel.Text = "Rebirths: " .. current
    footerRebirthsGainedLabel.Text = "Rebirths Gained: " .. gained
end

rebirthsStat.Changed:Connect(updateRebirths)
updateRebirths()

-- Fast Rebirth System
getgenv().AutoFarming = false

local function getGoldenRebirthCount()
    local ultimates = LocalPlayer:FindFirstChild("ultimatesFolder")
    if ultimates and ultimates:FindFirstChild("Golden Rebirth") then
        return ultimates["Golden Rebirth"].Value
    end
    return 0
end

local function getStrengthRequiredForRebirth()
    local rebirths = LocalPlayer.leaderstats.Rebirths.Value
    local baseStrength = 10000 + (5000 * rebirths)
    local golden = getGoldenRebirthCount()
    if golden >= 1 and golden <= 5 then
        baseStrength = baseStrength * (1 - golden * 0.1)
    end
    return math.floor(baseStrength)
end

fastStrengthFolder:AddSwitch("Fast Rebirth", function(state)
    getgenv().AutoFarming = state

    if state then
        warn("⚡ AutoFarming ACTIVATED")

        task.spawn(function()
            while getgenv().AutoFarming do
                local requiredStrength = getStrengthRequiredForRebirth()

                -- Farming Phase
                for _, folder in pairs(LocalPlayer.petsFolder:GetChildren()) do
                    if folder:IsA("Folder") then
                        for _, pet in pairs(folder:GetChildren()) do
                            ReplicatedStorage.rEvents.equipPetEvent:FireServer("unequipPet", pet)
                        end
                    end
                end
                task.wait(0.1)

                local FarmPet = "Swift Samurai"
                for _, folder in pairs(LocalPlayer.petsFolder:GetChildren()) do
                    if folder:IsA("Folder") then
                        for _, pet in pairs(folder:GetChildren()) do
                            if pet.Name == FarmPet then
                                ReplicatedStorage.rEvents.equipPetEvent:FireServer("equipPet", pet)
                            end
                        end
                    end
                end

                while LocalPlayer.leaderstats.Strength.Value < requiredStrength and getgenv().AutoFarming do
                    for _ = 1, 27 do
                        LocalPlayer.muscleEvent:FireServer("rep")
                    end
                    task.wait()
                end

                if not getgenv().AutoFarming then break end

                -- Rebirth Phase
                for _, folder in pairs(LocalPlayer.petsFolder:GetChildren()) do
                    if folder:IsA("Folder") then
                        for _, pet in pairs(folder:GetChildren()) do
                            ReplicatedStorage.rEvents.equipPetEvent:FireServer("unequipPet", pet)
                        end
                    end
                end
                task.wait(0.1)

                local RebirthPet = "Tribal Overlord"
                for _, folder in pairs(LocalPlayer.petsFolder:GetChildren()) do
                    if folder:IsA("Folder") then
                        for _, pet in pairs(folder:GetChildren()) do
                            if pet.Name == RebirthPet then
                                ReplicatedStorage.rEvents.equipPetEvent:FireServer("equipPet", pet)
                            end
                        end
                    end
                end

                local oldRebirths = LocalPlayer.leaderstats.Rebirths.Value
                repeat
                    ReplicatedStorage.rEvents.rebirthRemote:InvokeServer("rebirthRequest")
                    task.wait(0.1)
                until LocalPlayer.leaderstats.Rebirths.Value > oldRebirths or not getgenv().AutoFarming
            end

            print("🛑 AutoFarming STOPPED")
        end)

    else
        warn("🛑 AutoFarming DEACTIVATED")
    end
end)

-- Rebirths Without Packs
local rebirthFolder = AutoFarm:AddFolder("Rebiths Without Packs")

local targetRebirthAmount = nil
local farmingTarget = false

rebirthFolder:AddTextBox("Target number of Rebirths", function(value)
    local amount = tonumber(value) or 1
    local currentRebirths = leaderstats.Rebirths.Value
    if amount >= currentRebirths then
        targetRebirthAmount = amount
    end
end, {clear = false})

local targetRebirthToggle = rebirthFolder:AddSwitch("Start", function(enabled)
    farmingTarget = enabled
    if enabled then
        task.spawn(function()
            while farmingTarget do
                local currentRebirths = leaderstats.Rebirths.Value
                local requiredStrength = 10000 + 5000 * currentRebirths

                if targetRebirthAmount and currentRebirths >= targetRebirthAmount then
                    targetRebirthToggle:Set(false)
                    farmingTarget = false
                    break
                end

                if leaderstats.Strength.Value >= requiredStrength then
                    ReplicatedStorage.rEvents.rebirthRemote:InvokeServer("rebirthRequest")
                    task.wait(0.7)
                else
                    for i = 1, 10 do
                        LocalPlayer.muscleEvent:FireServer("rep")
                    end
                end

                task.wait(0.1)
            end
        end)
    end
end)

rebirthFolder:AddSwitch("Auto Teleport to the King", function(enabled)
    if enabled then
        _G.AutoTeleportKing = true
        local targetPosition = Vector3.new(-8750.1318359375, 124.37654876708984, -5861.2666015625)

        while _G.AutoTeleportKing do
            local char = LocalPlayer.Character
            if char then
                local rootPart = char:FindFirstChild("HumanoidRootPart")
                if rootPart then
                    rootPart.CFrame = CFrame.new(targetPosition)
                    rootPart.Velocity = Vector3.new(0, 0, 0)
                    rootPart.RotVelocity = Vector3.new(0, 0, 0)
                end
            end
            task.wait(0.1)
        end
    else
        _G.AutoTeleportKing = false
    end
end)

rebirthFolder:AddSwitch("Auto Size 2", function(enabled)
    if enabled then
        _G.AutoSize2 = true
        while _G.AutoSize2 do
            ReplicatedStorage.rEvents.changeSpeedSizeRemote:InvokeServer("changeSize", 2)
            task.wait(1)
        end
    else
        _G.AutoSize2 = false
    end
end)

-- Auto Tools
local autoEquipToolsFolder = AutoFarm:AddFolder("Auto Tools")

autoEquipToolsFolder:AddButton("Gamepass AutoLift", function()
    local gamepassFolder = ReplicatedStorage.gamepassIds
    for _, gamepass in pairs(gamepassFolder:GetChildren()) do
        local value = Instance.new("IntValue")
        value.Name = gamepass.Name
        value.Value = gamepass.Value
        value.Parent = LocalPlayer.ownedGamepasses
    end
end)

local function createAutoToolSwitch(toolName, globalVar)
    autoEquipToolsFolder:AddSwitch("Auto " .. toolName, function(Value)
        _G[globalVar] = Value
        
        if Value then
            local tool = LocalPlayer.Backpack:FindFirstChild(toolName)
            if tool then
                LocalPlayer.Character.Humanoid:EquipTool(tool)
            end
        else
            local equipped = LocalPlayer.Character:FindFirstChild(toolName)
            if equipped then
                equipped.Parent = LocalPlayer.Backpack
            end
        end
        
        task.spawn(function()
            while _G[globalVar] do
                if not _G[globalVar] then break end
                LocalPlayer.muscleEvent:FireServer("rep")
                task.wait(0.1)
            end
        end)
    end)
end

createAutoToolSwitch("Weight", "AutoWeight")
createAutoToolSwitch("Pushups", "AutoPushups")
createAutoToolSwitch("Handstands", "AutoHandstands")
createAutoToolSwitch("Situps", "AutoSitups")

autoEquipToolsFolder:AddSwitch("Auto Punch", function(Value)
    _G.fastHitActive = Value
    
    if Value then
        task.spawn(function()
            while _G.fastHitActive do
                local punch = LocalPlayer.Backpack:FindFirstChild("Punch")
                if punch then
                    punch.Parent = LocalPlayer.Character
                    if punch:FindFirstChild("attackTime") then
                        punch.attackTime.Value = 0
                    end
                end
                task.wait(0.1)
            end
        end)
        
        task.spawn(function()
            while _G.fastHitActive do
                LocalPlayer.muscleEvent:FireServer("punch", "rightHand")
                LocalPlayer.muscleEvent:FireServer("punch", "leftHand")
                
                local punchTool = LocalPlayer.Character:FindFirstChild("Punch")
                if punchTool then
                    punchTool:Activate()
                end
                task.wait()
            end
        end)
    else
        local equipped = LocalPlayer.Character:FindFirstChild("Punch")
        if equipped then
            equipped.Parent = LocalPlayer.Backpack
        end
    end
end)

autoEquipToolsFolder:AddSwitch("Fast Tools", function(Value)
    _G.FastTools = Value
    
    local toolSettings = {
        {"Punch", "attackTime", Value and 0 or 0.35},
        {"Ground Slam", "attackTime", Value and 0 or 6},
        {"Stomp", "attackTime", Value and 0 or 7},
        {"Handstands", "repTime", Value and 0 or 1},
        {"Pushups", "repTime", Value and 0 or 1},
        {"Weight", "repTime", Value and 0 or 1},
        {"Situps", "repTime", Value and 0 or 1}
    }
    
    local backpack = LocalPlayer:WaitForChild("Backpack")
    
    for _, toolInfo in ipairs(toolSettings) do
        local tool = backpack:FindFirstChild(toolInfo[1])
        if tool and tool:FindFirstChild(toolInfo[2]) then
            tool[toolInfo[2]].Value = toolInfo[3]
        end
        
        local equippedTool = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild(toolInfo[1])
        if equippedTool and equippedTool:FindFirstChild(toolInfo[2]) then
            equippedTool[toolInfo[2]].Value = toolInfo[3]
        end
    end
end)

-- ================================================================
--                            TAB: STATS
-- ================================================================

local estadisticas = window:AddTab("Stats")

local SelectPlayerName = ""

local PlayerDrop = estadisticas:AddDropdown("Select Player", function(Value)
    SelectPlayerName = Value:match("| (.+)")
end)

local Playerslist = {}
for _, Plr in pairs(Players:GetPlayers()) do
    local displayName = Plr.DisplayName .. " | " .. Plr.Name
    table.insert(Playerslist, displayName)
end
for _, AddPlr in ipairs(Playerslist) do
    PlayerDrop:Add(AddPlr)
end

-- Stats Labels
local Update = estadisticas:AddLabel("")
local Update1 = estadisticas:AddLabel("")
local Update2 = estadisticas:AddLabel("")
local Update3 = estadisticas:AddLabel("")
local Update4 = estadisticas:AddLabel("")
local Update5 = estadisticas:AddLabel("")
local Update6 = estadisticas:AddLabel("")
local Update9 = estadisticas:AddLabel("")
local Update10 = estadisticas:AddLabel("")
local Update11 = estadisticas:AddLabel("")
local Update12 = estadisticas:AddLabel("")
local Update13 = estadisticas:AddLabel("")

task.spawn(function()
    while task.wait(0) do
        if SelectPlayerName ~= "" then
            local player = Players:FindFirstChild(SelectPlayerName)
            if player then
                if player:FindFirstChild("Gems") then
                    Update1.Text = "Gems: " .. FormatDisplay(player.Gems.Value)
                end
                if player:FindFirstChild("Agility") then
                    Update3.Text = "Agility: " .. FormatDisplay(player.Agility.Value)
                end
                if player:FindFirstChild("Durability") then
                    Update4.Text = "Durability: " .. FormatDisplay(player.Durability.Value)
                end
                if player:FindFirstChild("muscleKingTime") then
                    Update6.Text = "Muscle King Time: " .. FormatDisplay(player.muscleKingTime.Value)
                end
                if player:FindFirstChild("customSize") then
                    Update10.Text = "Custom Size: " .. FormatDisplay(player.customSize.Value)
                end
                if player:FindFirstChild("customSpeed") then
                    Update11.Text = "Custom Speed: " .. FormatDisplay(player.customSpeed.Value)
                end
                if player:FindFirstChild("evilKarma") then
                    Update12.Text = "Evil Karma: " .. FormatDisplay(player.evilKarma.Value)
                end
                if player:FindFirstChild("goodKarma") then
                    Update13.Text = "Good Karma: " .. FormatDisplay(player.goodKarma.Value)
                end

                local leaderstats = player:FindFirstChild("leaderstats")
                if leaderstats then
                    if leaderstats:FindFirstChild("Strength") then
                        Update.Text = "Strength: " .. FormatDisplay(leaderstats.Strength.Value)
                    end
                    if leaderstats:FindFirstChild("Rebirths") then
                        Update2.Text = "Rebirth: " .. FormatDisplay(leaderstats.Rebirths.Value)
                    end
                    if leaderstats:FindFirstChild("Kills") then
                        Update5.Text = "Kills: " .. FormatDisplay(leaderstats.Kills.Value)
                    end
                end

                if player:FindFirstChild("currentMap") then
                    Update9.Text = "Current Map: " .. tostring(player.currentMap.Value)
                else
                    Update9.Text = "Current Map: No data"
                end
            end
        end
    end
end)

-- Advanced Stats
estadisticas:AddLabel("————————————————————————————")
estadisticas:AddLabel("Advanced Stats:").TextSize = 24

local enemyHealthLabel = estadisticas:AddLabel("Enemy Health: N/A")
enemyHealthLabel.TextSize = 20
enemyHealthLabel.TextColor3 = Color3.fromRGB(255, 255, 255)

local playerDamageLabel = estadisticas:AddLabel("Your Damage: N/A")
playerDamageLabel.TextSize = 20
playerDamageLabel.TextColor3 = Color3.fromRGB(255, 255, 255)

local hitsToKillLabel = estadisticas:AddLabel("Hits to Kill: N/A")
hitsToKillLabel.TextSize = 20
hitsToKillLabel.TextColor3 = Color3.fromRGB(255, 255, 255)

local function calculateEnemyHealth(targetPlayer)
    if not targetPlayer then return 0 end
    local baseDura = 0
    local durabilityStat = targetPlayer:FindFirstChild("Durability") or 
                         (targetPlayer:FindFirstChild("leaderstats") and targetPlayer.leaderstats:FindFirstChild("Durability"))
    if durabilityStat then baseDura = durabilityStat.Value end
    
    local totalMultiplier = 1
    local ultFolder = targetPlayer:FindFirstChild("ultimatesFolder")
    if ultFolder then
        local infernalHealth = ultFolder:FindFirstChild("Infernal Health")
        if infernalHealth then
            local upgrades = infernalHealth.Value or 0
            totalMultiplier = totalMultiplier + 0.15 * upgrades
        end
    end
    
    local backpack = targetPlayer:FindFirstChild("Backpack")
    if backpack then
        local equippedPets = backpack:FindFirstChild("EquippedPets") or backpack:FindFirstChild("equippedPets")
        if equippedPets then
            for _, pet in ipairs(equippedPets:GetChildren()) do
                if string.lower(tostring(pet)):match("mighty") and string.lower(tostring(pet)):match("monster") then
                    totalMultiplier = totalMultiplier + 0.5
                    break
                end
            end
        end
    end
    
    return baseDura * totalMultiplier
end

local function calculateLocalPlayerDamage()
    local strengthStat = LocalPlayer.leaderstats and LocalPlayer.leaderstats:FindFirstChild("Strength")
    if not strengthStat then return 0 end
    
    local baseDamage = strengthStat.Value * 0.0667
    local totalMultiplier = 1
    
    local ultFolder = LocalPlayer:FindFirstChild("ultimatesFolder")
    if ultFolder then
        local demonDamage = ultFolder:FindFirstChild("Demon Damage")
        if demonDamage then
            local upgrades = demonDamage.Value or 0
            totalMultiplier = totalMultiplier + math.min(0.1 * upgrades, 0.5)
        end
    end
    
    local backpack = LocalPlayer:FindFirstChild("Backpack")
    if backpack then
        local equippedPets = backpack:FindFirstChild("EquippedPets") or backpack:FindFirstChild("equippedPets")
        if equippedPets then
            for _, pet in ipairs(equippedPets:GetChildren()) do
                if string.lower(tostring(pet)):match("wild") and string.lower(tostring(pet)):match("wizard") then
                    totalMultiplier = totalMultiplier + 0.5
                    break
                end
            end
        end
    end
    
    return baseDamage * totalMultiplier
end

local function calculateHitsToKill(health, damage)
    if damage <= 0 then return "∞" end
    local hits = math.ceil(health / damage)
    if hits > 50 then
        return "∞"
    elseif hits < 1 then
        return 1
    else
        return hits
    end
end

local function updateAdvancedStats(targetPlayer)
    if not targetPlayer then
        enemyHealthLabel.Text = "Enemy Health: N/A"
        playerDamageLabel.Text = "Your Damage: N/A"
        hitsToKillLabel.Text = "Hits to Kill: N/A"
        return
    end
    
    local enemyHealth = calculateEnemyHealth(targetPlayer)
    local playerDamage = calculateLocalPlayerDamage()
    local hitsToKill = calculateHitsToKill(enemyHealth, playerDamage)
    
    enemyHealthLabel.Text = "Enemy Health: " .. FormatDisplay(enemyHealth)
    playerDamageLabel.Text = "Your Damage: " .. FormatDisplay(playerDamage)
    hitsToKillLabel.Text = "Hits to Kill: " .. tostring(hitsToKill)
end

task.spawn(function()
    while true do
        local selectedPlayer = Players:FindFirstChild(SelectPlayerName)
        if selectedPlayer then
            updateAdvancedStats(selectedPlayer)
        else
            updateAdvancedStats(nil)
        end
        task.wait(0.2)
    end
end)

-- ================================================================
--                            TAB: KILLER
-- ================================================================

local Killer = window:AddTab("Kill")

-- Variables
local playerWhitelist = {}
local targetPlayerNames = {}
local autoGoodKarma = false
local autoBadKarma = false
autoKill = false
killTarget = false
spying = false
autoEquipPunch = false
autoPunchNoAnim = false
following = false
followTarget = nil
godModeToggle = false
godDamageActive = false



local petSwitch = Killer:AddSwitch("Auto Replace Pets", function(state)
    _G.AutoCyclePetsActive = state
    
    if state then
        if _G.PetCycleConnection then
            _G.PetCycleConnection:Disconnect()
        end
        
        local current = 1
        _G.PetCycleLoop = true
        
        local function cyclePets()
            while _G.PetCycleLoop and _G.AutoCyclePetsActive do
                pcall(function()
                    local player = game.Players.LocalPlayer
                    local petsFolder = player:FindFirstChild("petsFolder")
                    
                    if petsFolder then
                        local uniqueFolder = petsFolder:FindFirstChild("Unique")
                        if uniqueFolder then
                            local petName = current == 1 and "Wild Wizard" or "Mighty Monster"
                            
                            pcall(function()
                                for _, folder in pairs(petsFolder:GetChildren()) do
                                    if folder:IsA("Folder") then
                                        for _, pet in pairs(folder:GetChildren()) do
                                            game:GetService("ReplicatedStorage").rEvents.equipPetEvent:FireServer("unequipPet", pet)
                                        end
                                    end
                                end
                            end)
                            
                            task.wait(0.1)
                            
                            local equipped = 0
                            for _, pet in pairs(uniqueFolder:GetChildren()) do
                                if pet.Name == petName and equipped < 8 then
                                    game:GetService("ReplicatedStorage").rEvents.equipPetEvent:FireServer("equipPet", pet)
                                    equipped = equipped + 1
                                    task.wait(0.05)
                                end
                            end
                            
                            current = current == 1 and 2 or 1
                        end
                    end
                end)
                
                local waitTime = 0
                while waitTime < 2 and _G.PetCycleLoop and _G.AutoCyclePetsActive do
                    task.wait(0.1)
                    waitTime = waitTime + 0.1
                end
            end
        end
        
        task.spawn(cyclePets)
        
        _G.PetCycleConnection = game.Players.LocalPlayer.CharacterAdded:Connect(function()
            task.wait(2)
            if _G.AutoCyclePetsActive and _G.PetCycleLoop then
                cyclePets()
            end
        end)
        
    else
        _G.AutoCyclePetsActive = false
        _G.PetCycleLoop = false
        
        if _G.PetCycleConnection then
            _G.PetCycleConnection:Disconnect()
            _G.PetCycleConnection = nil
        end
    end
end)

-- Pet Selection
Killer:AddLabel("Select damage or durability pet").TextSize = 18

local dropdown = Killer:AddDropdown("Select Pet", function(text)
    local petsFolder = LocalPlayer.petsFolder
    for _, folder in pairs(petsFolder:GetChildren()) do
        if folder:IsA("Folder") then
            for _, pet in pairs(folder:GetChildren()) do
                ReplicatedStorage.rEvents.equipPetEvent:FireServer("unequipPet", pet)
            end
        end
    end
    task.wait(0.2)

    local petName = text
    local petsToEquip = {}

    for _, pet in pairs(LocalPlayer.petsFolder.Unique:GetChildren()) do
        if pet.Name == petName then
            table.insert(petsToEquip, pet)
        end
    end

    local maxPets = 8
    local equippedCount = math.min(#petsToEquip, maxPets)

    for i = 1, equippedCount do
        ReplicatedStorage.rEvents.equipPetEvent:FireServer("equipPet", petsToEquip[i])
        task.wait(0.1)
    end
end)

dropdown:Add("Wild Wizard")
dropdown:Add("Mighty Monster")

-- Karma System
Killer:AddSwitch("Auto Good Karma", function(bool)
    autoGoodKarma = bool
    task.spawn(function()
        while autoGoodKarma do
            local playerChar = LocalPlayer.Character
            local rightHand = playerChar and playerChar:FindFirstChild("RightHand")
            local leftHand = playerChar and playerChar:FindFirstChild("LeftHand")
            if playerChar and rightHand and leftHand then
                for _, target in ipairs(Players:GetPlayers()) do
                    if target ~= LocalPlayer then
                        local evilKarma = target:FindFirstChild("evilKarma")
                        local goodKarma = target:FindFirstChild("goodKarma")
                        if evilKarma and goodKarma and evilKarma:IsA("IntValue") and goodKarma:IsA("IntValue") and evilKarma.Value > goodKarma.Value then
                            local rootPart = target.Character and target.Character:FindFirstChild("HumanoidRootPart")
                            if rootPart then
                                firetouchinterest(rightHand, rootPart, 1)
                                firetouchinterest(leftHand, rootPart, 1)
                                firetouchinterest(rightHand, rootPart, 0)
                                firetouchinterest(leftHand, rootPart, 0)
                            end
                        end
                    end
                end
            end
            task.wait(0.01)
        end
    end)
end)

Killer:AddSwitch("Auto Bad Karma", function(bool)
    autoBadKarma = bool
    task.spawn(function()
        while autoBadKarma do
            local playerChar = LocalPlayer.Character
            local rightHand = playerChar and playerChar:FindFirstChild("RightHand")
            local leftHand = playerChar and playerChar:FindFirstChild("LeftHand")
            if playerChar and rightHand and leftHand then
                for _, target in ipairs(Players:GetPlayers()) do
                    if target ~= LocalPlayer then
                        local evilKarma = target:FindFirstChild("evilKarma")
                        local goodKarma = target:FindFirstChild("goodKarma")
                        if evilKarma and goodKarma and evilKarma:IsA("IntValue") and goodKarma:IsA("IntValue") and goodKarma.Value > evilKarma.Value then
                            local rootPart = target.Character and target.Character:FindFirstChild("HumanoidRootPart")
                            if rootPart then
                                firetouchinterest(rightHand, rootPart, 1)
                                firetouchinterest(leftHand, rootPart, 1)
                                firetouchinterest(rightHand, rootPart, 0)
                                firetouchinterest(leftHand, rootPart, 0)
                            end
                        end
                    end
                end
            end
            task.wait(0.01)
        end
    end)
end)

-- Whitelist System
local friendWhitelistActive = false

Killer:AddSwitch("Auto Whitelist Friends", function(state)
    friendWhitelistActive = state

    if state then
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and LocalPlayer:IsFriendsWith(player.UserId) then
                playerWhitelist[player.Name] = true
            end
        end

        Players.PlayerAdded:Connect(function(player)
            if friendWhitelistActive and player ~= LocalPlayer and LocalPlayer:IsFriendsWith(player.UserId) then
                playerWhitelist[player.Name] = true
            end
        end)
    else
        for name in pairs(playerWhitelist) do
            local friend = Players:FindFirstChild(name)
            if friend and LocalPlayer:IsFriendsWith(friend.UserId) then
                playerWhitelist[name] = nil
            end
        end
    end
end)

Killer:AddTextBox("Whitelist", function(text)
    local target = Players:FindFirstChild(text)
    if target then
        playerWhitelist[target.Name] = true
    end
end)

Killer:AddTextBox("UnWhitelist", function(text)
    local target = Players:FindFirstChild(text)
    if target then
        playerWhitelist[target.Name] = nil
    end
end)

-- Auto Kill
Killer:AddSwitch("Auto Kill", function(bool)
    autoKill = bool

    task.spawn(function()
        while autoKill do
            local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
            local rightHand = character:FindFirstChild("RightHand")
            local leftHand = character:FindFirstChild("LeftHand")

            local punch = LocalPlayer.Backpack:FindFirstChild("Punch")
            if punch and not character:FindFirstChild("Punch") then
                punch.Parent = character
            end

            if rightHand and leftHand then
                for _, target in ipairs(Players:GetPlayers()) do
                    if target ~= LocalPlayer and not playerWhitelist[target.Name] then
                        local targetChar = target.Character
                        local rootPart = targetChar and targetChar:FindFirstChild("HumanoidRootPart")
                        if rootPart then
                            pcall(function()
                                firetouchinterest(rightHand, rootPart, 1)
                                firetouchinterest(leftHand, rootPart, 1)
                                firetouchinterest(rightHand, rootPart, 0)
                                firetouchinterest(leftHand, rootPart, 0)
                            end)
                        end
                    end
                end
            end

            task.wait(0.05)
        end
    end)
end)

-- Target Selection System
local selectedTarget = nil
local targetDropdownItems = {}

local targetDropdown = Killer:AddDropdown("Select Target", function(displayName)
    for _, player in ipairs(Players:GetPlayers()) do
        if player.DisplayName == displayName then
            if not table.find(targetPlayerNames, player.Name) then
                table.insert(targetPlayerNames, player.Name)
            end
            selectedTarget = player.Name
            break
        end
    end
end)

for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        targetDropdown:Add(player.DisplayName)
        targetDropdownItems[player.Name] = player.DisplayName
    end
end

Players.PlayerAdded:Connect(function(player)
    if player ~= LocalPlayer then
        targetDropdown:Add(player.DisplayName)
        targetDropdownItems[player.Name] = player.DisplayName
    end
end)

Players.PlayerRemoving:Connect(function(player)
    if targetDropdownItems[player.Name] then
        targetDropdownItems[player.Name] = nil
        targetDropdown:Clear()
        for _, displayName in pairs(targetDropdownItems) do
            targetDropdown:Add(displayName)
        end
    end

    for i = #targetPlayerNames, 1, -1 do
        if targetPlayerNames[i] == player.Name then
            table.remove(targetPlayerNames, i)
        end
    end
end)

Killer:AddButton("Remove Selected Target", function()
    if selectedTarget then
        for i, v in ipairs(targetPlayerNames) do
            if v == selectedTarget then
                table.remove(targetPlayerNames, i)
                break
            end
        end
        selectedTarget = nil
    end
end)

Killer:AddSwitch("Start Kill Target", function(state)
    killTarget = state

    task.spawn(function()
        while killTarget do
            local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()

            local punch = LocalPlayer.Backpack:FindFirstChild("Punch")
            if punch and not character:FindFirstChild("Punch") then
                punch.Parent = character
            end

            local rightHand = character:FindFirstChild("RightHand")
            local leftHand = character:FindFirstChild("LeftHand")

            if rightHand and leftHand then
                for _, name in ipairs(targetPlayerNames) do
                    local target = Players:FindFirstChild(name)
                    if target and target ~= LocalPlayer and target.Character then
                        local rootPart = target.Character:FindFirstChild("HumanoidRootPart")
                        local humanoid = target.Character:FindFirstChild("Humanoid")
                        if rootPart and humanoid and humanoid.Health > 0 then
                            pcall(function()
                                firetouchinterest(rightHand, rootPart, 1)
                                firetouchinterest(leftHand, rootPart, 1)
                                firetouchinterest(rightHand, rootPart, 0)
                                firetouchinterest(leftHand, rootPart, 0)
                            end)
                        end
                    end
                end
            end

            task.wait(0.05)
        end
    end)
end)

-- Spy System
local spyTargetDropdownItems = {}
local targetPlayerName = nil

local spyTargetDropdown = Killer:AddDropdown("Select View Target", function(displayName)
    for _, player in ipairs(Players:GetPlayers()) do
        if player.DisplayName == displayName then
            targetPlayerName = player.Name
            break
        end
    end
end)

for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        spyTargetDropdown:Add(player.DisplayName)
        spyTargetDropdownItems[player.Name] = player.DisplayName
    end
end

Players.PlayerAdded:Connect(function(player)
    if player ~= LocalPlayer then
        spyTargetDropdown:Add(player.DisplayName)
        spyTargetDropdownItems[player.Name] = player.DisplayName
    end
end)

Killer:AddSwitch("View Player", function(bool)
    spying = bool
    if not spying then
        local cam = Workspace.CurrentCamera
        cam.CameraSubject = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") or LocalPlayer
        return
    end
    task.spawn(function()
        while spying do
            local target = Players:FindFirstChild(targetPlayerName)
            if target and target ~= LocalPlayer then
                local humanoid = target.Character and target.Character:FindFirstChild("Humanoid")
                if humanoid then
                    Workspace.CurrentCamera.CameraSubject = humanoid
                end
            end
            task.wait(0.1)
        end
    end)
end)

-- Punch Animation Control
local function setupAnimationBlocking()
    local char = LocalPlayer.Character
    if not char or not char:FindFirstChild("Humanoid") then return end

    local humanoid = char:FindFirstChild("Humanoid")

    for _, track in pairs(humanoid:GetPlayingAnimationTracks()) do
        if track.Animation then
            local animId = track.Animation.AnimationId
            local animName = track.Name:lower()

            if animId == "rbxassetid://3638729053" or animId == "rbxassetid://3638767427" or
               animName:match("punch") or animName:match("attack") or animName:match("right") then
                track:Stop()
            end
        end
    end

    if not _G.AnimBlockConnection then
        _G.AnimBlockConnection = humanoid.AnimationPlayed:Connect(function(track)
            if track.Animation then
                local animId = track.Animation.AnimationId
                local animName = track.Name:lower()

                if animId == "rbxassetid://3638729053" or animId == "rbxassetid://3638767427" or
                   animName:match("punch") or animName:match("attack") or animName:match("right") then
                    track:Stop()
                end
            end
        end)
    end
end

Killer:AddButton("Remove Punch Anim", function()
    setupAnimationBlocking()
end)

function RecoveryPunch()
    if _G.AnimBlockConnection then
        _G.AnimBlockConnection:Disconnect()
        _G.AnimBlockConnection = nil
    end
end

Killer:AddButton("Recover Punch Anim", function()
    RecoveryPunch()
end)

-- Auto Punch Systems
Killer:AddSwitch("Auto Equip Punch", function(state)
    autoEquipPunch = state
    task.spawn(function()
        while autoEquipPunch do
            local punch = LocalPlayer.Backpack:FindFirstChild("Punch")
            if punch then
                punch.Parent = LocalPlayer.Character
            end
            task.wait(0.1)
        end
    end)
end)

Killer:AddSwitch("Auto Punch [Without Animation]", function(state)
    autoPunchNoAnim = state
    task.spawn(function()
        while autoPunchNoAnim do
            local punch = LocalPlayer.Backpack:FindFirstChild("Punch") or LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Punch")
            if punch then
                if punch.Parent ~= LocalPlayer.Character then
                    punch.Parent = LocalPlayer.Character
                end
                LocalPlayer.muscleEvent:FireServer("punch", "rightHand")
                LocalPlayer.muscleEvent:FireServer("punch", "leftHand")
            else
                autoPunchNoAnim = false
            end
            task.wait(0.01)
        end
    end)
end)

Killer:AddSwitch("Auto Punch", function(state)
    _G.fastHitActive = state
    if state then
        task.spawn(function()
            while _G.fastHitActive do
                local punch = LocalPlayer.Backpack:FindFirstChild("Punch")
                if punch then
                    punch.Parent = LocalPlayer.Character
                    if punch:FindFirstChild("attackTime") then
                        punch.attackTime.Value = 0
                    end
                end
                task.wait(0.1)
            end
        end)
        task.spawn(function()
            while _G.fastHitActive do
                local punch = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Punch")
                if punch then
                    punch:Activate()
                end
                task.wait(0.1)
            end
        end)
    else
        local punch = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Punch")
        if punch then
            punch.Parent = LocalPlayer.Backpack
        end
    end
end)

Killer:AddSwitch("Fast Punch", function(state)
    _G.autoPunchActive = state
    if state then
        task.spawn(function()
            while _G.autoPunchActive do
                local punch = LocalPlayer.Backpack:FindFirstChild("Punch")
                if punch then
                    punch.Parent = LocalPlayer.Character
                    if punch:FindFirstChild("attackTime") then
                        punch.attackTime.Value = 0
                    end
                end
                task.wait()
            end
        end)
        task.spawn(function()
            while _G.autoPunchActive do
                local punch = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Punch")
                if punch then
                    punch:Activate()
                end
                task.wait()
            end
        end)
    else
        local punch = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Punch")
        if punch then
            punch.Parent = LocalPlayer.Backpack
        end
    end
end)

-- Sistema de Auto Area Travel
local autoAreaTravelEnabled = false

-- Botão para ativar/desativar
Killer:AddSwitch("Auto GODMODE Join Tiny island", function(state)
    autoAreaTravelEnabled = state
    
    if state then
        warn("🔄 Auto Area Travel ATIVADO - Tentando viajar para área...")
        -- Executa imediatamente ao ativar
        task.spawn(function()
            local success, result = pcall(function()
                local Event = game:GetService("ReplicatedStorage").rEvents.areaTravelRemote
                return Event:InvokeServer("travelToArea", workspace.areaCircles.areaCircle)
            end)
            
            if success then
                warn("✅ Viagem de área executada com sucesso!")
                StarterGui:SetCore("SendNotification", {
                    Title = "Area Travel",
                    Text = "Viagem realizada com sucesso!",
                    Duration = 5
                })
            else
                warn("❌ Erro ao viajar para área:", result)
                StarterGui:SetCore("SendNotification", {
                    Title = "Area Travel",
                    Text = "Erro: " .. tostring(result),
                    Duration = 5
                })
            end
        end)
    else
        warn("⛔ Auto Area Travel DESATIVADO")
    end
end)

-- Loop contínuo (opcional - se quiser que tente periodicamente)
task.spawn(function()
    while true do
        if autoAreaTravelEnabled then
            -- Espera um tempo entre tentativas (ajustável)
            task.wait(10) -- 10 segundos entre tentativas
            
            local success, result = pcall(function()
                local Event = game:GetService("ReplicatedStorage").rEvents.areaTravelRemote
                return Event:InvokeServer("travelToArea", workspace.areaCircles.areaCircle)
            end)
            
            if success then
                warn("🔄 Tentativa de viagem automática realizada")
            end
        else
            task.wait(1)
        end
    end
end)

-- Botão manual (opcional - para teste rápido)
Killer:AddButton("GODMODE Tiny island (Button)", function()
    local success, result = pcall(function()
        local Event = game:GetService("ReplicatedStorage").rEvents.areaTravelRemote
        return Event:InvokeServer("travelToArea", workspace.areaCircles.areaCircle)
    end)
    
    if success then
        warn("✅ Viagem manual executada com sucesso!")
        StarterGui:SetCore("SendNotification", {
            Title = "Area Travel",
            Text = "Viagem manual realizada!",
            Duration = 5
        })
    else
        warn("❌ Erro na viagem manual:", result)
    end
end)

-- God Mode
Killer:AddSwitch("GOD MODE Brawl", function(State)
    godModeToggle = State
    if State then
        task.spawn(function()
            while godModeToggle do
                ReplicatedStorage.rEvents.brawlEvent:FireServer("joinBrawl")
                task.wait()
            end
        end)
    end
end)

-- Follow System
local function followPlayer(targetPlayer)
    local myChar = LocalPlayer.Character
    local targetChar = targetPlayer.Character

    if not (myChar and targetChar) then return end
    local myHRP = myChar:FindFirstChild("HumanoidRootPart")
    local targetHRP = targetChar:FindFirstChild("HumanoidRootPart")

    if myHRP and targetHRP then
        local followPos = targetHRP.Position - (targetHRP.CFrame.LookVector * 3)
        myHRP.CFrame = CFrame.new(followPos, targetHRP.Position)
    end
end

local followDropdown = Killer:AddDropdown("Teleport Player", function(selectedDisplayName)
    if selectedDisplayName and selectedDisplayName ~= "" then
        local target = nil
        for _, plr in ipairs(Players:GetPlayers()) do
            if plr.DisplayName == selectedDisplayName then
                target = plr
                break
            end
        end

        if target then
            followTarget = target.Name
            following = true
            followPlayer(target)
        end
    end
end)

for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        followDropdown:Add(player.DisplayName)
    end
end

Players.PlayerAdded:Connect(function(player)
    if player ~= LocalPlayer then
        followDropdown:Add(player.DisplayName)
    end
end)

Killer:AddButton("Stop Following", function()
    following = false
    followTarget = nil
end)

task.spawn(function()
    while task.wait(0.01) do
        if following and followTarget then
            local target = Players:FindFirstChild(followTarget)
            if target then
                followPlayer(target)
            else
                following = false
                followTarget = nil
            end
        end
    end
end)

-- Auto Slam/Stomp
Killer:AddSwitch("Auto Slams", function(state)
    godDamageActive = state
    if state then
        task.spawn(function()
            while godDamageActive do
                local groundSlam = LocalPlayer.Backpack:FindFirstChild("Ground Slam") or (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Ground Slam"))

                if groundSlam then
                    if groundSlam.Parent == LocalPlayer.Backpack then
                        groundSlam.Parent = LocalPlayer.Character
                    end
                    if groundSlam:FindFirstChild("attackTime") then
                        groundSlam.attackTime.Value = 0
                    end
                    LocalPlayer.muscleEvent:FireServer("slam")
                    groundSlam:Activate()
                end

                task.wait(0.1)
            end
        end)
    end
end)

Killer:AddSwitch("Auto Stomp", function(state)
    godDamageActive = state
    if state then
        task.spawn(function()
            while godDamageActive do
                local stomp = LocalPlayer.Backpack:FindFirstChild("Stomp") or (LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Stomp"))

                if stomp then
                    if stomp.Parent == LocalPlayer.Backpack then
                        stomp.Parent = LocalPlayer.Character
                    end
                    if stomp:FindFirstChild("attackTime") then
                        stomp.attackTime.Value = 0
                    end
                    LocalPlayer.muscleEvent:FireServer("slam")
                    stomp:Activate()
                end

                task.wait(0.1)
            end
        end)
    end
end)

-- Water Freeze Button
Killer:AddButton("Freeze the Water", function()
    local positions = {
        Vector3.new(11, -9, 78),
        Vector3.new(-110, -9,-999),
        Vector3.new(-110, -9,1999),
        Vector3.new(-110, -9,-1999),
        Vector3.new(-110, -9,2999),
        Vector3.new(-110, -9,-3999),
        Vector3.new(-110, -9,4999),
        Vector3.new(-110, -9,-5999),
        Vector3.new(-110, -9,-6999),
        Vector3.new(-110, -9,-7999),
        Vector3.new(-110, -9,-8999),
        Vector3.new(-110, -9,-9999),
        Vector3.new(-110, -9,-10999),
        Vector3.new(-110, -9,-11999),
        Vector3.new(-110, -9,-12999),
        Vector3.new(-110, -9,-13999),
        Vector3.new(-110, -9,-14999),
        Vector3.new(-110, -9,-15999),
        Vector3.new(-110, -9,-16999),
        Vector3.new(-110, -9,-17999),
        Vector3.new(-110, -9,-18999),
        Vector3.new(-110, -9,-19999),
        Vector3.new(-110, -9,-20999),
        Vector3.new(-110, -9,-21999),
        Vector3.new(-110, -9,-22999),
        Vector3.new(-110, -9,-23999),
        Vector3.new(-8922.482421875, -9.848660469055176, -6233.65380859375),
        Vector3.new(-1107.2977294921875, -9.7848615646362305, -4127.03369140625),
        Vector3.new(-2054.99365234375, -9.43027928471565247, -5135.06201171875),
        Vector3.new(-3072.01806640625, -9.645418643951416, -6136.02392578125),
        Vector3.new(-4088.711181640625, -9.43027925491333, -5125.28076171875),
        Vector3.new(-5100.0576171875, -9.215141296386719, -5128.03369140625),
        Vector3.new(-6103.42626953125, -9.000000953674316, -5590.5859375),
        Vector3.new(-7096.3935546875, -9.784859657287598, -6403.66015625),
        Vector3.new(-9882.7021484375, -9.633520126342773, -5228.537109375),
        Vector3.new(-8889.23046875, -9.418381690979004, -4767.06689453125),
        Vector3.new(-7888.4443359375, -9.203240394592285, -4648.048828125),
        Vector3.new(-6868.951171875, -9.988101005554199, -4124.4384765625),
        Vector3.new(-5883.841796875, -9.7729620933532715, -4073.54736328125),
        Vector3.new(-9873.0966796875, -9.26787185668945, -6775.55859375),
        Vector3.new(-8453.767578125, -9.26786804199219, -7212.89013671875),
        Vector3.new(-7482.234375, -9.84866714477539, -8082.72021484375),
        Vector3.new(-6483.5791015625, -9.26786422729492, -9055.9033203125),
        Vector3.new(-6561.2578125, -9.84867858886719, -10038.5146484375)
    }

    for _, pos in ipairs(positions) do
        local WalkPart = Instance.new("Part")
        WalkPart.Parent = Workspace
        WalkPart.Anchored = true
        WalkPart.Position = pos
        WalkPart.Size = Vector3.new(20000, 0, 20000)
    end
end)

-- Size NaN Button
Killer:AddButton("Size NaN", function()
    ReplicatedStorage.rEvents.changeSpeedSizeRemote:InvokeServer("changeSize", 0/0)
end)

-- Dead Hit Activation
Killer:AddButton("Activate Dead Hit", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/xccxk/MAIN/refs/heads/main/1-2-3-ALL-STEPS"))()
end)

-- Time Changer
local timeOptions = {
    "Morning",
    "Noon",
    "Afternoon",
    "Sunset",
    "Night",
    "Midnight",
    "Dawn",
    "Early Morning"
}

local timeDropdown = Killer:AddDropdown("Change Time", function(selection)
    Lighting.Brightness = 2
    Lighting.FogEnd = 100000
    Lighting.Ambient = Color3.fromRGB(127,127,127)

    if selection == "Morning" then
        Lighting.ClockTime = 6
        Lighting.Brightness = 2
        Lighting.Ambient = Color3.fromRGB(200, 200, 255)
    elseif selection == "Noon" then
        Lighting.ClockTime = 12
        Lighting.Brightness = 3
        Lighting.Ambient = Color3.fromRGB(255, 255, 255)
    elseif selection == "Afternoon" then
        Lighting.ClockTime = 16
        Lighting.Brightness = 2.5
        Lighting.Ambient = Color3.fromRGB(255, 220, 180)
    elseif selection == "Sunset" then
        Lighting.ClockTime = 18
        Lighting.Brightness = 2
        Lighting.Ambient = Color3.fromRGB(255, 150, 100)
        Lighting.FogEnd = 500
    elseif selection == "Night" then
        Lighting.ClockTime = 20
        Lighting.Brightness = 1.5
        Lighting.Ambient = Color3.fromRGB(100, 100, 150)
        Lighting.FogEnd = 800
    elseif selection == "Midnight" then
        Lighting.ClockTime = 0
        Lighting.Brightness = 1
        Lighting.Ambient = Color3.fromRGB(50, 50, 100)
        Lighting.FogEnd = 400
    elseif selection == "Dawn" then
        Lighting.ClockTime = 4
        Lighting.Brightness = 1.8
        Lighting.Ambient = Color3.fromRGB(180, 180, 220)
    elseif selection == "Early Morning" then
        Lighting.ClockTime = 2
        Lighting.Brightness = 1.2
        Lighting.Ambient = Color3.fromRGB(100, 120, 180)
    end
end)

for _, option in ipairs(timeOptions) do
    timeDropdown:Add(option)
end

-- ================================================================
--                            TAB: TELEPORT
-- ================================================================

local teleport = window:AddTab("Teleport")

local teleportLocations = {
    ["Spawn"] = CFrame.new(2, 8, 115),
    ["Secret Area"] = CFrame.new(1947, 2, 6191),
    ["Tiny Island"] = CFrame.new(-34, 7, 1903),
    ["Frozen Island"] = CFrame.new(-2600.00244, 3.67686558, -403.884369, 0.0873617008, 1.0482899e-09, 0.99617666, 3.07204253e-08, 1, -3.7464023e-09, -0.99617666, 3.09302628e-08, 0.0873617008),
    ["Mythical Island"] = CFrame.new(2255, 7, 1071),
    ["Hell Island"] = CFrame.new(-6768, 7, -1287),
    ["Legend Island"] = CFrame.new(4604, 991, -3887),
    ["Muscle King Island"] = CFrame.new(-8646, 17, -5738),
    ["Jungle Island"] = CFrame.new(-8659, 6, 2384),
    ["Brawl Lava"] = CFrame.new(4471, 119, -8836),
    ["Brawl Desert"] = CFrame.new(960, 17, -7398),
    ["Brawl Regular"] = CFrame.new(-1849, 20, -6335)
}

for locationName, cframe in pairs(teleportLocations) do
    teleport:AddButton(locationName, function()
        local char = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local hrp = char:WaitForChild("HumanoidRootPart")
        hrp.CFrame = cframe
        
        StarterGui:SetCore("SendNotification", {
            Title = "Teleport",
            Text = "Teleported to " .. locationName,
            Duration = 3
        })
    end)
end

-- ================================================================
--                          TAB: CALCULATOR
-- ================================================================

local Calculadora = window:AddTab("Calculator")

-- Damage Calculator
local FolderDamage = Calculadora:AddFolder("Pack Damage Calculator")

local baseStrength = 0
local resultadoLabelsDamage = {}

FolderDamage:AddTextBox("Base Strength (e.g., 1.27Qa, T, B)", function(text)
    local unidades = { ["T"] = 1e12, ["Q"] = 1e15, ["B"] = 1e9 }
    text = text:upper()
    for u, m in pairs(unidades) do
        if text:find(u) then
            local num = tonumber(text:match("(%d+%.?%d*)"))
            if num then
                baseStrength = num * m
                return
            end
        end
    end
    baseStrength = tonumber(text:match("(%d+%.?%d*)")) or 0
end)

local mensajeLabelDamage = FolderDamage:AddLabel("")

for i = 1, 8 do
    resultadoLabelsDamage[i] = FolderDamage:AddLabel(string.format("%d pack(s): -", i))
end

FolderDamage:AddButton("Calculate Damage", function()
    if baseStrength <= 0 then
        mensajeLabelDamage.Text = "Enter a valid value."
        for i = 1, 8 do
            resultadoLabelsDamage[i].Text = string.format("%d pack(s): -", i)
        end
        return
    end

    mensajeLabelDamage.Text = ""

    local danoAjustado = baseStrength * 0.10
    local incremento = 0.335

    for pack = 1, 8 do
        local mult = 1 + (pack * incremento)
        local valor = danoAjustado * mult

        local disp
        if valor >= 1e15 then
            disp = string.format("%.3f Qa", valor / 1e15)
        elseif valor >= 1e12 then
            disp = string.format("%.2f T", valor / 1e12)
        elseif valor >= 1e9 then
            disp = string.format("%.2f B", valor / 1e9)
        else
            disp = tostring(math.floor(valor))
        end

        resultadoLabelsDamage[pack].Text = string.format("%d pack(s): %s", pack, disp)
    end
end)

-- Durability Calculator
local FolderDurabilidad = Calculadora:AddFolder("Pack Durability Calculator")

local baseDurabilidad = 0
local resultadoLabelsDurabilidad = {}

FolderDurabilidad:AddTextBox("Base Durability (e.g., 1.27Qa, T, B)", function(text)
    local unidades = { ["T"] = 1e12, ["Q"] = 1e15, ["B"] = 1e9 }
    text = text:upper()
    for u, m in pairs(unidades) do
        if text:find(u) then
            local num = tonumber(text:match("(%d+%.?%d*)"))
            if num then
                baseDurabilidad = num * m
                return
            end
        end
    end
    baseDurabilidad = tonumber(text:match("(%d+%.?%d*)")) or 0
end)

local mensajeLabelDurabilidad = FolderDurabilidad:AddLabel("")

for i = 1, 8 do
    resultadoLabelsDurabilidad[i] = FolderDurabilidad:AddLabel(string.format("%d pack(s): -", i))
end

FolderDurabilidad:AddButton("Calculate Durability", function()
    if baseDurabilidad <= 0 then
        mensajeLabelDurabilidad.Text = "Enter a valid value."
        for i = 1, 8 do
            resultadoLabelsDurabilidad[i].Text = string.format("%d pack(s): -", i)
        end
        return
    end

    mensajeLabelDurabilidad.Text = ""

    local incremento = 0.335
    local adicional = 1.5

    for pack = 1, 8 do
        local mult = 1 + (pack * incremento)
        local valor = baseDurabilidad * mult * adicional

        local disp
        if valor >= 1e15 then
            disp = string.format("%.3f Qa", valor / 1e15)
        elseif valor >= 1e12 then
            disp = string.format("%.2f T", valor / 1e12)
        elseif valor >= 1e9 then
            disp = string.format("%.2f B", valor / 1e9)
        else
            disp = tostring(math.floor(valor))
        end

        resultadoLabelsDurabilidad[pack].Text = string.format("%d pack(s): %s", pack, disp)
    end
end)

-- ================================================================
--                          TAB: CALCULATE STATS
-- ================================================================

local features = window:AddTab("Calculate Stats")

local strengthStat = leaderstats:WaitForChild("Strength")
local durabilityStat = LocalPlayer:WaitForChild("Durability")

local stopwatchLabel = features:AddLabel("Fast Rep Time: 0d 0h 0m 0s")
stopwatchLabel.TextSize = 20

local projectedStrengthLabel = features:AddLabel("Strength Rate: 0 /Hour | 0 /Day | 0 /Week | 0 /Month")
projectedStrengthLabel.TextSize = 20

local projectedDurabilityLabel = features:AddLabel("Durability Rate: 0 /Hour | 0 /Day | 0 /Week | 0 /Month")
projectedDurabilityLabel.TextSize = 20

features:AddLabel("").TextSize = 10

local statsLabel = features:AddLabel("Stats:")
statsLabel.TextSize = 24

local strengthLabel = features:AddLabel("Strength: 0 | Gained: 0")
strengthLabel.TextSize = 20

local durabilityLabel = features:AddLabel("Durability: 0 | Gained: 0")
durabilityLabel.TextSize = 20

local startTime = tick()
local initialStrength = strengthStat.Value
local initialDurability = durabilityStat.Value
local trackingStarted = false

local strengthHistory = {}
local durabilityHistory = {}
local calculationInterval = 10

task.spawn(function()
    local lastCalcTime = tick()
    while true do
        local currentTime = tick()
        local currentStrength = strengthStat.Value
        local currentDurability = durabilityStat.Value

        if not trackingStarted and (currentStrength - initialStrength) >= 100e9 then
            trackingStarted = true
            startTime = tick()
            strengthHistory = {}
            durabilityHistory = {}
        end

        if trackingStarted then
            local elapsedTime = currentTime - startTime
            local days = math.floor(elapsedTime / (24 * 3600))
            local hours = math.floor((elapsedTime % (24 * 3600)) / 3600)
            local minutes = math.floor((elapsedTime % 3600) / 60)
            local seconds = math.floor(elapsedTime % 60)

            stopwatchLabel.Text = string.format("Fast Rep Time: %dd %dh %dm %ds", days, hours, minutes, seconds)

            local sessionStrengthDelta = currentStrength - initialStrength
            local sessionDurabilityDelta = currentDurability - initialDurability

            strengthLabel.Text = "Strength: " .. FormatNumber(currentStrength) .. " | Gained: " .. FormatNumber(sessionStrengthDelta)
            durabilityLabel.Text = "Durability: " .. FormatNumber(currentDurability) .. " | Gained: " .. FormatNumber(sessionDurabilityDelta)

            table.insert(strengthHistory, {time = currentTime, value = currentStrength})
            table.insert(durabilityHistory, {time = currentTime, value = currentDurability})

            while #strengthHistory > 0 and currentTime - strengthHistory[1].time > calculationInterval do
                table.remove(strengthHistory, 1)
            end
            while #durabilityHistory > 0 and currentTime - durabilityHistory[1].time > calculationInterval do
                table.remove(durabilityHistory, 1)
            end

            if currentTime - lastCalcTime >= calculationInterval then
                lastCalcTime = currentTime

                if #strengthHistory >= 2 then
                    local strengthDelta = strengthHistory[#strengthHistory].value - strengthHistory[1].value
                    local strengthPerSecond = strengthDelta / calculationInterval
                    local strengthPerHour = math.floor(strengthPerSecond * 3600)
                    local strengthPerDay = math.floor(strengthPerSecond * 86400)
                    local strengthPerWeek = math.floor(strengthPerSecond * 604800)
                    local strengthPerMonth = math.floor(strengthPerSecond * 2592000)

                    projectedStrengthLabel.Text = "Strength Rate: " .. FormatNumber(strengthPerHour) .. "/Hour | " .. FormatNumber(strengthPerDay) .. "/Day | " .. FormatNumber(strengthPerWeek) .. "/Week | " .. FormatNumber(strengthPerMonth) .. "/Month"
                end

                if #durabilityHistory >= 2 then
                    local durabilityDelta = durabilityHistory[#durabilityHistory].value - durabilityHistory[1].value
                    local durabilityPerSecond = durabilityDelta / calculationInterval
                    local durabilityPerHour = math.floor(durabilityPerSecond * 3600)
                    local durabilityPerDay = math.floor(durabilityPerSecond * 86400)
                    local durabilityPerWeek = math.floor(durabilityPerSecond * 604800)
                    local durabilityPerMonth = math.floor(durabilityPerSecond * 2592000)

                    projectedDurabilityLabel.Text = "Durability Rate: " .. FormatNumber(durabilityPerHour) .. "/Hour | " .. FormatNumber(durabilityPerDay) .. "/Day | " .. FormatNumber(durabilityPerWeek) .. "/Week | " .. FormatNumber(durabilityPerMonth) .. "/Month"
                end
            end
        end

        task.wait(0.05)
    end
end)

-- ================================================================
--                            TAB: CRYSTALS
-- ================================================================

local pets = window:AddTab("Crystals")

local crystalData = {
    ["Blue Crystal"] = {
        {name = "Blue Birdie", rarity = "Basic"},
        {name = "Orange Hedgehog", rarity = "Basic"},
        {name = "Blue Aura", rarity = "Basic"},
        {name = "Red Kitty", rarity = "Basic"},
        {name = "Dark Vampy", rarity = "Advanced"},
        {name = "Blue Bunny", rarity = "Basic"},
        {name = "Red Aura", rarity = "Basic"},
        {name = "Blue Aura", rarity = "Basic"},
        {name = "Green Aura", rarity = "Basic"},
        {name = "Purple Aura", rarity = "Basic"},
        {name = "Red Aura", rarity = "Basic"},
        {name = "Yellow Aura", rarity = "Basic"}
    },
    ["Green Crystal"] = {
        {name = "Silver Dog", rarity = "Basic"},
        {name = "Green Aura", rarity = "Advanced"},
        {name = "Dark Golem", rarity = "Advanced"},
        {name = "Green Butterfly", rarity = "Advanced"},
        {name = "Crimson Falcon", rarity = "Rare"},
        {name = "Red Aura", rarity = "Basic"},
        {name = "Blue Aura", rarity = "Basic"},
        {name = "Green Aura", rarity = "Basic"},
        {name = "Purple Aura", rarity = "Basic"},
        {name = "Red Aura", rarity = "Basic"},
        {name = "Yellow Aura", rarity = "Basic"}
    },
    ["Frost Crystal"] = {
        {name = "Yellow Butterfly", rarity = "Advanced"},
        {name = "Purple Dragon", rarity = "Rare"},
        {name = "Blue Pheonix", rarity = "Epic"},
        {name = "Orange Pegasus", rarity = "Rare"},
        {name = "Lightning", rarity = "Rare"},
        {name = "Electro", rarity = "Advanced"}
    },
    ["Mythical Crystal"] = {
        {name = "Purple Falcon", rarity = "Rare"},
        {name = "Red Dragon", rarity = "Rare"},
        {name = "Blue Firecaster", rarity = "Epic"},
        {name = "Golden Pheonix", rarity = "Epic"},
        {name = "Power Lightning", rarity = "Rare"},
        {name = "Dark Lightning", rarity = "Epic"}
    },
    ["Inferno Crystal"] = {
        {name = "Red Firecaster", rarity = "Epic"},
        {name = "Infernal Dragon", rarity = "Unique"},
        {name = "White Pegasus", rarity = "Rare"},
        {name = "Golden Pheonix", rarity = "Epic"},
        {name = "Inferno", rarity = "Epic"},
        {name = "Dark Storm", rarity = "Unique"}
    },
    ["Legends Crystal"] = {
        {name = "Ultra Birdie", rarity = "Unique"},
        {name = "Magic Butterfly", rarity = "Unique"},
        {name = "Green Firecaster", rarity = "Epic"},
        {name = "White Pheonix", rarity = "Epic"},
        {name = "Supernova", rarity = "Epic"},
        {name = "Purple Nova", rarity = "Unique"}
    },
    ["Muscle Elite Crystal"] = {
        {name = "Frostwave Legends Penguin", rarity = "Rare"},
        {name = "Phantom Genesis Dragon", rarity = "Rare"},
        {name = "Dark Legends Manticore", rarity = "Epic"},
        {name = "Ultimate Supernova Pegasus", rarity = "Epic"},
        {name = "Aether Spirit Bunny", rarity = "Unique"},
        {name = "Cybernetic Showdown Dragon", rarity = "Unique"}
    },
    ["Galaxy Oracle Crystal"] = {
        {name = "Eternal Strike Leviathan", rarity = "Rare"},
        {name = "Lightning Strike Phantom", rarity = "Epic"},
        {name = "Darkstar Hunter", rarity = "Unique"},
        {name = "Muscle King", rarity = "Unique"},
        {name = "Azure Tundra", rarity = "Epic"},
        {name = "Ultra Inferno", rarity = "Rare"}
    },
    ["Jungle Crystal"] = {
        {name = "Entropic Blast", rarity = "Unique"},
        {name = "Muscle Sensei", rarity = "Unique"},
        {name = "Grand Supernova", rarity = "Epic"},
        {name = "Neon Guardian", rarity = "Unique"},
        {name = "Eternal Megastrike", rarity = "Unique"},
        {name = "Golden Viking", rarity = "Epic"},
        {name = "Astral Electro", rarity = "Epic"},
        {name = "Dark Electro", rarity = "Epic"},
        {name = "Enchanted Mirage", rarity = "Epic"},
        {name = "Ultra Mirage", rarity = "Unique"},
        {name = "Unstable Mirage", rarity = "Unique"}
    }
}

local function findCrystalForItem(itemName)
    for crystalName, pets in pairs(crystalData) do
        for _, pet in ipairs(pets) do
            if pet.name == itemName then
                return crystalName
            end
        end
    end
    return nil
end

pets:AddLabel("=== Buy Pets and Auras ===")

-- Pet Dropdown
local selectedPet = ""
local petDropdown = pets:AddDropdown("Select Pet", function(text)
    selectedPet = text
    local crystal = findCrystalForItem(text)
    print("Pet selected: " .. text .. " (Found in: " .. (crystal or "Unknown") .. ")")
end)

-- Add Pets
petDropdown:Add("Blue Birdie (Basic)")
petDropdown:Add("Orange Hedgehog (Basic)")
petDropdown:Add("Red Kitty (Basic)")
petDropdown:Add("Blue Bunny (Basic)")
petDropdown:Add("Silver Dog (Basic)")
petDropdown:Add("Dark Vampy (Advanced)")
petDropdown:Add("Dark Golem (Advanced)")
petDropdown:Add("Green Butterfly (Advanced)")
petDropdown:Add("Yellow Butterfly (Advanced)")
petDropdown:Add("Crimson Falcon (Rare)")
petDropdown:Add("Purple Dragon (Rare)")
petDropdown:Add("Orange Pegasus (Rare)")
petDropdown:Add("Purple Falcon (Rare)")
petDropdown:Add("Red Dragon (Rare)")
petDropdown:Add("White Pegasus (Rare)")
petDropdown:Add("Frostwave Legends Penguin (Rare)")
petDropdown:Add("Phantom Genesis Dragon (Rare)")
petDropdown:Add("Eternal Strike Leviathan (Rare)")
petDropdown:Add("Blue Pheonix (Epic)")
petDropdown:Add("Blue Firecaster (Epic)")
petDropdown:Add("Golden Pheonix (Epic)")
petDropdown:Add("Red Firecaster (Epic)")
petDropdown:Add("Green Firecaster (Epic)")
petDropdown:Add("White Pheonix (Epic)")
petDropdown:Add("Dark Legends Manticore (Epic)")
petDropdown:Add("Ultimate Supernova Pegasus (Epic)")
petDropdown:Add("Lightning Strike Phantom (Epic)")
petDropdown:Add("Golden Viking (Epic)")
petDropdown:Add("Infernal Dragon (Unique)")
petDropdown:Add("Ultra Birdie (Unique)")
petDropdown:Add("Magic Butterfly (Unique)")
petDropdown:Add("Aether Spirit Bunny (Unique)")
petDropdown:Add("Cybernetic Showdown Dragon (Unique)")
petDropdown:Add("Darkstar Hunter (Unique)")
petDropdown:Add("Muscle Sensei (Unique)")
petDropdown:Add("Neon Guardian (Unique)")

-- Aura Dropdown
local selectedAura = ""
local auraDropdown = pets:AddDropdown("Select Aura", function(text)
    selectedAura = text
    local crystal = findCrystalForItem(text)
    print("Aura selected: " .. text .. " (Found in: " .. (crystal or "Unknown") .. ")")
end)

auraDropdown:Add("Blue Aura (Basic)")
auraDropdown:Add("Green Aura (Basic)")
auraDropdown:Add("Purple Aura (Basic)")
auraDropdown:Add("Red Aura (Basic)")
auraDropdown:Add("Yellow Aura (Basic)")
auraDropdown:Add("Ultra Inferno (Rare)")
auraDropdown:Add("Azure Tundra (Epic)")
auraDropdown:Add("Grand Supernova (Epic)")
auraDropdown:Add("Muscle King (Unique)")
auraDropdown:Add("Entropic Blast (Unique)")
auraDropdown:Add("Eternal Megastrike (Unique)")

pets:AddLabel("=== Auto Buy System ===")

pets:AddSwitch("Auto Buy Pet", function(bool)
    _G.AutoBuyPet = bool
    
    if bool and selectedPet ~= "" then
        local petName = selectedPet:match("^(.-)%s*%(") or selectedPet
        spawn(function()
            while _G.AutoBuyPet and selectedPet ~= "" do
                local petToBuy = ReplicatedStorage.cPetShopFolder:FindFirstChild(petName)
                if petToBuy then
                    ReplicatedStorage.cPetShopRemote:InvokeServer(petToBuy)
                end
                task.wait(0.1)
            end
        end)
    end
end)

pets:AddSwitch("Auto Buy Aura", function(bool)
    _G.AutoBuyAura = bool
    
    if bool and selectedAura ~= "" then
        local auraName = selectedAura:match("^(.-)%s*%(") or selectedAura
        spawn(function()
            while _G.AutoBuyAura and selectedAura ~= "" do
                local auraToBuy = ReplicatedStorage.cPetShopFolder:FindFirstChild(auraName)
                if auraToBuy then
                    ReplicatedStorage.cPetShopRemote:InvokeServer(auraToBuy)
                end
                task.wait(0.1)
            end
        end)
    end
end)

-- Ultimates System
pets:AddLabel("=== Buy Ultimates ===")

local ultimateOptions = {
    "+1 Daily Spin",
    "+1 Pet Slot",
    "+10 Item Capacity",
    "+5% Rep Speed",
    "Demon Damage",
    "Galaxy Gains",
    "Golden Rebirth",
    "Jungle Swift",
    "Muscle Mind",
    "x2 Chest Rewards",
    "x2 Quest Rewards"
}

local selectedUltimate = ""
local ultimateDropdown = pets:AddDropdown("Select Ultimate", function(text)
    selectedUltimate = text
end)

for _, ultimate in ipairs(ultimateOptions) do
    ultimateDropdown:Add(ultimate)
end

pets:AddSwitch("Auto Buy Ultimates", function(bool)
    _G.AutoUpgradeUltimate = bool
    
    if bool and selectedUltimate ~= "" then
        spawn(function()
            while _G.AutoUpgradeUltimate and selectedUltimate ~= "" do
                ReplicatedStorage.rEvents.ultimatesRemote:InvokeServer("upgradeUltimate", selectedUltimate)
                task.wait(1)
            end
        end)
    end
end)

-- ================================================================
--                            TAB: ROCK
-- ================================================================

local farmTab = window:AddTab("Rock")
farmTab:AddLabel("Rock Farming")

local function gettool()
    for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
        if v.Name == "Punch" and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid:EquipTool(v)
        end
    end
    LocalPlayer.muscleEvent:FireServer("punch", "leftHand")
    LocalPlayer.muscleEvent:FireServer("punch", "rightHand")
end

local rockSwitches = {
    ["Tiny Island Rock"] = 0,
    ["Starter Island Rock"] = 100,
    ["Legend Beach Rock"] = 5000,
    ["Frost Gym Rock"] = 150000,
    ["Mythical Gym Rock"] = 400000,
    ["Eternal Gym Rock"] = 750000,
    ["Legend Gym Rock"] = 1000000,
    ["Muscle King Gym Rock"] = 5000000,
    ["Ancient Jungle Rock"] = 10000000
}

for rockName, neededDurability in pairs(rockSwitches) do
    farmTab:AddSwitch(rockName, function(bool)
        getgenv().autoFarm = bool

        if bool then
            spawn(function()
                while getgenv().autoFarm do
                    task.wait()
                    if LocalPlayer.Durability.Value >= neededDurability then
                        for i, v in pairs(Workspace.machinesFolder:GetDescendants()) do
                            if v.Name == "neededDurability" and v.Value == neededDurability and 
                               LocalPlayer.Character:FindFirstChild("LeftHand") and 
                               LocalPlayer.Character:FindFirstChild("RightHand") then

                                firetouchinterest(v.Parent.Rock, LocalPlayer.Character.RightHand, 0)
                                firetouchinterest(v.Parent.Rock, LocalPlayer.Character.RightHand, 1)
                                firetouchinterest(v.Parent.Rock, LocalPlayer.Character.LeftHand, 0)
                                firetouchinterest(v.Parent.Rock, LocalPlayer.Character.LeftHand, 1)
                                gettool()
                            end
                        end
                    end
                end
            end)
        end
    end)
end

-- ================================================================
--                            TAB: GIFT
-- ================================================================

local Gift = window:AddTab("Gift")

-- Protein Eggs Gifting
Gift:AddLabel("Gifting Protein Eggs:").TextSize = 22

local proteinEggLabel = Gift:AddLabel("Protein Eggs: 0")
proteinEggLabel.TextSize = 20

local selectedEggPlayer = nil
local eggCount = 0

local eggDropdown = Gift:AddDropdown("Player to Gift Eggs", function(selectedDisplayName)
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr.DisplayName == selectedDisplayName then
            selectedEggPlayer = plr
            break
        end
    end
end)

for _, plr in ipairs(Players:GetPlayers()) do
    if plr ~= LocalPlayer then
        eggDropdown:Add(plr.DisplayName)
    end
end

Players.PlayerAdded:Connect(function(plr)
    if plr ~= LocalPlayer then
        eggDropdown:Add(plr.DisplayName)
    end
end)

Gift:AddTextBox("Amount of Eggs", function(text)
    eggCount = tonumber(text) or 0
end)

Gift:AddButton("Gift Eggs", function()
    if selectedEggPlayer and eggCount > 0 then
        for i = 1, eggCount do
            local egg = LocalPlayer.consumablesFolder:FindFirstChild("Protein Egg")
            if egg then
                ReplicatedStorage.rEvents.giftRemote:InvokeServer("giftRequest", selectedEggPlayer, egg)
                task.wait(0.1)
            end
        end
    end
end)

-- Tropical Shakes Gifting
Gift:AddLabel("Gifting Tropical Shakes:").TextSize = 22

local tropicalShakeLabel = Gift:AddLabel("Tropical Shakes: 0")
tropicalShakeLabel.TextSize = 18

local selectedShakePlayer = nil
local shakeCount = 0

local shakeDropdown = Gift:AddDropdown("Player to Gift Tropical Shakes", function(selectedDisplayName)
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr.DisplayName == selectedDisplayName then
            selectedShakePlayer = plr
            break
        end
    end
end)

for _, plr in ipairs(Players:GetPlayers()) do
    if plr ~= LocalPlayer then
        shakeDropdown:Add(plr.DisplayName)
    end
end

Players.PlayerAdded:Connect(function(plr)
    if plr ~= LocalPlayer then
        shakeDropdown:Add(plr.DisplayName)
    end
end)

Gift:AddTextBox("Tropical Shakes Gift", function(text)
    shakeCount = tonumber(text) or 0
end)

Gift:AddButton("Gift Tropical Shakes", function()
    if selectedShakePlayer and shakeCount > 0 then
        for i = 1, shakeCount do
            local shake = LocalPlayer.consumablesFolder:FindFirstChild("Tropical Shake")
            if shake then
                ReplicatedStorage.rEvents.giftRemote:InvokeServer("giftRequest", selectedShakePlayer, shake)
                task.wait(0.1)
            end
        end
    end
end)

local function updateItemCount()
    local proteinEggCount = 0
    local tropicalShakeCount = 0

    local backpack = LocalPlayer:WaitForChild("Backpack")
    if backpack then
        for _, item in ipairs(backpack:GetChildren()) do
            if item.Name == "Protein Egg" then
                proteinEggCount = proteinEggCount + 1
            elseif item.Name == "Tropical Shake" or item.Name == "Piñas" then
                tropicalShakeCount = tropicalShakeCount + 1
            end
        end
    end

    proteinEggLabel.Text = "Protein Eggs: " .. proteinEggCount
    tropicalShakeLabel.Text = "Tropical Shakes: " .. tropicalShakeCount
end

task.spawn(function()
    while true do
        updateItemCount()
        task.wait(0.25)
    end
end)

-- ================================================================
--                            TAB: CREDITS
-- ================================================================

local Credits = window:AddTab("Credits")

Credits:AddLabel("Professional Edition")
Credits:AddLabel("Script Developed by:")
Credits:AddLabel("MyersDev👑")
Credits:AddLabel("All rights reserved © 2024")
Credits:AddLabel("For bugs or suggestions visit our Discord server")

-- ================================================================
--                          FINAL MESSAGE
-- ================================================================

print("╔══════════════════════════════════════════════════════════╗")
print("║          Muscle Legends Companion Loaded!               ║")
print("║                 Version: 4.0.0                          ║")
print("║               Developed by: MyersDev                    ║")
print("╚══════════════════════════════════════════════════════════╝")

-- ================================================================
--                          AUTOEXEC COMPLETE
-- ================================================================
