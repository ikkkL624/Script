local v1 = loadstring(game:HttpGet('https://raw.githubusercontent.com/xccxk/LIBPHEOX/refs/heads/main/BYMYERS', true))()
local _LocalPlayer = game.Players.LocalPlayer
local v3 = v1:AddWindow('MIRAGE PAID | Cracked By Rey Ghost | Bienvenido  ' .. (_LocalPlayer.DisplayName or _LocalPlayer.Name), {
    main_color = Color3.fromRGB(45, 125, 220),
    min_size = Vector2.new(630, 900),
    can_resize = false,
})
local _LocalPlayer2 = game:GetService('Players').LocalPlayer

game:GetService('RunService')

repeat
    task.wait()
until _LocalPlayer2 and _LocalPlayer2.Character

_G.whitelistedPlayers = _G.whitelistedPlayers or {}
_G.targetPlayer = _G.targetPlayer or ''

local _FarmOP = v3:AddTab('Farm OP')

_FarmOP:Show()

getgenv()._AutoRepFarmEnabled = false

_FarmOP:AddSwitch('Fuerza OP', function(p6)
    getgenv()._AutoRepFarmEnabled = p6

    warn('[Auto Rep Farm] Estado cambiado a:', p6 and 'ON' or 'OFF')
end)

local _ReplicatedStorage = game:GetService('ReplicatedStorage')
local _Players = game:GetService('Players')
local _Stats = game:GetService('Stats')
local _LocalPlayer3 = _Players.LocalPlayer
local u11 = 'Swift Samurai'
local u12 = 'Rock5M'
local u13 = 'ProteinEgg'
local u14 = 1800
local u15 = 500
local u16 = 0.01
local u17 = 1
local u18 = 500
local u19 = 300
local u20 = nil
local u21 = 0
local u22 = 0

local function u25()
    local v23, v24 = pcall(function()
        return _Stats.Network.ServerStatsItem['Data Ping']:GetValue()
    end)

    return v23 and v24 and v24 or 999
end
local function u26()
    u20 = (_LocalPlayer3.Character or _LocalPlayer3.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart', 5)
end
local function u32()
    local _petsFolder = _LocalPlayer3:FindFirstChild('petsFolder')

    if _petsFolder and _petsFolder:FindFirstChild('Unique') then
        local v28, v29, v30 = pairs(_petsFolder.Unique:GetChildren())

        while true do
            local v31

            v30, v31 = v28(v29, v30)

            if v30 == nil then
                break
            end
            if v31.Name == u11 then
                _ReplicatedStorage.rEvents.equipPetEvent:FireServer('equipPet', v31)

                break
            end
        end
    end
end
local function u37()
    if _LocalPlayer3:FindFirstChild('Backpack') then
        local v33, v34, v35 = pairs(_LocalPlayer3.Backpack:GetChildren())

        while true do
            local v36

            v35, v36 = v33(v34, v35)

            if v35 == nil then
                break
            end
            if v36.Name == u13 then
                _ReplicatedStorage.rEvents.eatEvent:FireServer('eat', v36)

                break
            end
        end
    end
end
local function u39()
    local v38 = workspace:FindFirstChild(u12)

    if v38 and u20 then
        u20.CFrame = v38.CFrame * CFrame.new(0, 0, -5)

        _ReplicatedStorage.rEvents.hitEvent:FireServer('hit', v38)
    end
end

task.spawn(function()
    u26()
    u32()

    u21 = tick()
    u22 = tick()

    local v40 = false

    while true do
        if getgenv()._AutoRepFarmEnabled then
            local v41 = u25()

            if u18 < v41 and not v40 then
                warn('[Auto Rep Farm] Ping alto (' .. math.floor(v41) .. 'ms), pausando farmeo...')

                v40 = true
            end
            if v41 <= u19 and v40 then
                warn('[Auto Rep Farm] Ping bajo (' .. math.floor(v41) .. 'ms), reanudando farmeo...')

                v40 = false
            end
            if not v40 then
                if _LocalPlayer3:FindFirstChild('muscleEvent') then
                    for _ = 1, u15 do
                        _LocalPlayer3.muscleEvent:FireServer('rep')
                    end
                end
                if u14 <= tick() - u21 then
                    u37()

                    u21 = tick()
                end
                if u17 <= tick() - u22 then
                    u39()

                    u22 = tick()
                end
            end
        end

        task.wait(u16)
    end
end)

local u42 = false

local function u47()
    local _LocalPlayer4 = game.Players.LocalPlayer
    local _Backpack = _LocalPlayer4:WaitForChild('Backpack')
    local v45 = _LocalPlayer4.Character or _LocalPlayer4.CharacterAdded:Wait()
    local _ProteinEgg = _Backpack:FindFirstChild('Protein Egg')

    if _ProteinEgg then
        _ProteinEgg.Parent = v45

        pcall(function()
            _ProteinEgg:Activate()
        end)
        print('[AutoEgg] Protein Egg consumido.')
    else
        warn('[AutoEgg] No se encontr\u{f3} Protein Egg en Backpack.')
    end
end

task.spawn(function()
    while true do
        while u42 do
            u47()
            task.wait(3600)
        end

        task.wait(1)
    end
end)
_FarmOP:AddSwitch('Eat Egg (60 Min)', function(p48)
    u42 = p48

    print(p48 and '[AutoEgg] Activado.' or '[AutoEgg] Desactivado.')
end)

local u49 = false

local function u54()
    local _LocalPlayer5 = game.Players.LocalPlayer
    local _Backpack2 = _LocalPlayer5:WaitForChild('Backpack')
    local v52 = _LocalPlayer5.Character or _LocalPlayer5.CharacterAdded:Wait()
    local _ProteinEgg2 = _Backpack2:FindFirstChild('Protein Egg')

    if _ProteinEgg2 then
        _ProteinEgg2.Parent = v52

        pcall(function()
            _ProteinEgg2:Activate()
        end)
        print('[AutoEgg] Protein Egg consumido.')
    else
        warn('[AutoEgg] No se encontr\u{f3} Protein Egg en Backpack.')
    end
end

task.spawn(function()
    while true do
        while u49 do
            u54()
            task.wait(1800)
        end

        task.wait(1)
    end
end)
_FarmOP:AddSwitch('Eat Egg (30 Min)', function(p55)
    u49 = p55

    print(p55 and '[AutoEgg] Activado.' or '[AutoEgg] Desactivado.')
end)
_FarmOP:AddSwitch('Hide All Frames', function(p56)
    local _ReplicatedStorage2 = game:GetService('ReplicatedStorage')
    local v58, v59, v60 = pairs(_ReplicatedStorage2:GetChildren())

    while true do
        local v61

        v60, v61 = v58(v59, v60)

        if v60 == nil then
            break
        end
        if v61.Name:match('Frame$') then
            v61.Visible = not p56
        end
    end
end)
_FarmOP:AddSwitch('Anti Lag (Baja Graficos)', function()
    local v62, v63, v64 = pairs(game:GetDescendants())

    while true do
        local v65

        v64, v65 = v62(v63, v64)

        if v64 == nil then
            break
        end
        if v65:IsA('ParticleEmitter') or v65:IsA('Smoke') or (v65:IsA('Fire') or v65:IsA('Sparkles')) then
            v65.Enabled = false
        end
    end

    local _Lighting = game:GetService('Lighting')

    _Lighting.GlobalShadows = false
    _Lighting.FogEnd = 9000000000
    _Lighting.Brightness = 0
    settings().Rendering.QualityLevel = 1

    local v67, v68, v69 = pairs(game:GetDescendants())

    while true do
        local v70

        v69, v70 = v67(v68, v69)

        if v69 == nil then
            break
        end
        if v70:IsA('Decal') or v70:IsA('Texture') then
            v70.Transparency = 1
        elseif v70:IsA('BasePart') and not v70:IsA('MeshPart') then
            v70.Material = Enum.Material.SmoothPlastic

            if not (v70.Parent and (v70.Parent:FindFirstChild('Humanoid') or v70.Parent.Parent:FindFirstChild('Humanoid'))) then
                v70.Reflectance = 0
            end
        end
    end

    local v71, v72, v73 = pairs(_Lighting:GetChildren())

    while true do
        local v74

        v73, v74 = v71(v72, v73)

        if v73 == nil then
            break
        end
        if v74:IsA('BlurEffect') or v74:IsA('SunRaysEffect') or (v74:IsA('ColorCorrectionEffect') or v74:IsA('BloomEffect')) or v74:IsA('DepthOfFieldEffect') then
            v74.Enabled = false
        end
    end

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'anti lag activado',
        Text = 'Full optimization applied!',
        Duration = 5,
    })
end)
_FarmOP:AddSwitch('Anti Lag (Full Black)', function(p75)
    local _Lighting2 = game:GetService('Lighting')

    if p75 then
        local v77, v78, v79 = pairs(_LocalPlayer3.PlayerGui:GetChildren())

        while true do
            local v80

            v79, v80 = v77(v78, v79)

            if v79 == nil then
                break
            end
            if v80:IsA('ScreenGui') then
                v80:Destroy()
            end
        end

        local v81, v82, v83 = pairs(workspace:GetDescendants())

        while true do
            local v84

            v83, v84 = v81(v82, v83)

            if v83 == nil then
                break
            end
            if v84:IsA('ParticleEmitter') or v84:IsA('PointLight') or (v84:IsA('SpotLight') or v84:IsA('SurfaceLight')) then
                v84:Destroy()
            end
        end

        local v85, v86, v87 = pairs(_Lighting2:GetChildren())

        while true do
            local v88

            v87, v88 = v85(v86, v87)

            if v87 == nil then
                break
            end
            if v88:IsA('Sky') then
                v88:Destroy()
            end
        end

        local _Sky = Instance.new('Sky')

        _Sky.Name = 'DarkSky'
        _Sky.SkyboxBk = 'rbxassetid://0'
        _Sky.SkyboxDn = 'rbxassetid://0'
        _Sky.SkyboxFt = 'rbxassetid://0'
        _Sky.SkyboxLf = 'rbxassetid://0'
        _Sky.SkyboxRt = 'rbxassetid://0'
        _Sky.SkyboxUp = 'rbxassetid://0'
        _Sky.Parent = _Lighting2
        _Lighting2.Brightness = 0
        _Lighting2.ClockTime = 0
        _Lighting2.TimeOfDay = '00:00:00'
        _Lighting2.OutdoorAmbient = Color3.new(0, 0, 0)
        _Lighting2.Ambient = Color3.new(0, 0, 0)
        _Lighting2.FogColor = Color3.new(0, 0, 0)
        _Lighting2.FogEnd = 100

        task.spawn(function()
            while p75 do
                task.wait(5)

                if not _Lighting2:FindFirstChild('DarkSky') then
                    _Sky:Clone().Parent = _Lighting2
                end
            end
        end)
    end
end)
_FarmOP:AddSwitch('Anti AFK', function(p90)
    if p90 then
        if getgenv().AntiAfkExecuted and thisoneissocoldww then
            getgenv().AntiAfkExecuted = false
            getgenv().zamanbaslaticisi = false

            game.CoreGui.thisoneissocoldww:Destroy()
        end

        getgenv().AntiAfkExecuted = true

        local _ScreenGui = Instance.new('ScreenGui')
        local _Frame = Instance.new('Frame')
        local _UICorner = Instance.new('UICorner')
        local _TextButton = Instance.new('TextButton')
        local _TextLabel = Instance.new('TextLabel')
        local _TextLabel2 = Instance.new('TextLabel')
        local _TextLabel3 = Instance.new('TextLabel')
        local _TextLabel4 = Instance.new('TextLabel')
        local _TextLabel5 = Instance.new('TextLabel')
        local _TextLabel6 = Instance.new('TextLabel')
        local _Frame2 = Instance.new('Frame')
        local _UICorner2 = Instance.new('UICorner')
        local _TextLabel7 = Instance.new('TextLabel')

        _ScreenGui.Name = 'thisoneissocoldww'
        _ScreenGui.Parent = game.CoreGui
        _ScreenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
        _Frame.Name = 'madebybloodofbatus'
        _Frame.Parent = _ScreenGui
        _Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
        _Frame.Position = UDim2.new(0.085, 0, 0.13, 0)
        _Frame.Size = UDim2.new(0, 225, 0, 96)
        _UICorner.Parent = _Frame
        _TextButton.Name = 'DestroyButton'
        _TextButton.Parent = _Frame
        _TextButton.BackgroundTransparency = 1
        _TextButton.Position = UDim2.new(0.87, 0, 0.02, 0)
        _TextButton.Size = UDim2.new(0, 27, 0, 15)
        _TextButton.Font = Enum.Font.SourceSans
        _TextButton.Text = 'X'
        _TextButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextButton.TextSize = 14

        _TextButton.MouseButton1Click:Connect(function()
            getgenv().AntiAfkExecuted = false

            wait(0.1)
            _ScreenGui:Destroy()
        end)

        _TextLabel.Parent = _Frame
        _TextLabel.BackgroundTransparency = 1
        _TextLabel.Position = UDim2.new(0.3, 0, 0, 0)
        _TextLabel.Size = UDim2.new(0, 95, 0, 24)
        _TextLabel.Font = Enum.Font.SourceSans
        _TextLabel.Text = 'Anti Afk V2 BY MVX_DeathKillerX'
        _TextLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextLabel.TextSize = 14
        _TextLabel2.Parent = _Frame
        _TextLabel2.BackgroundTransparency = 1
        _TextLabel2.Position = UDim2.new(0.65, 0, 0.68, 0)
        _TextLabel2.Size = UDim2.new(0, 60, 0, 24)
        _TextLabel2.Font = Enum.Font.SourceSans
        _TextLabel2.Text = '0:0:0'
        _TextLabel2.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextLabel2.TextSize = 14
        _TextLabel3.Parent = _Frame
        _TextLabel3.BackgroundTransparency = 1
        _TextLabel3.Position = UDim2.new(0.03, 0, 0.37, 0)
        _TextLabel3.Size = UDim2.new(0, 29, 0, 24)
        _TextLabel3.Font = Enum.Font.SourceSans
        _TextLabel3.Text = 'Ping: '
        _TextLabel3.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextLabel3.TextSize = 14
        _TextLabel4.Parent = _Frame
        _TextLabel4.BackgroundTransparency = 1
        _TextLabel4.Position = UDim2.new(0.72, 0, 0.35, 0)
        _TextLabel4.Size = UDim2.new(0, 55, 0, 24)
        _TextLabel4.Font = Enum.Font.SourceSans
        _TextLabel4.Text = '0'
        _TextLabel4.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextLabel4.TextSize = 14
        _TextLabel5.Parent = _Frame
        _TextLabel5.BackgroundTransparency = 1
        _TextLabel5.Position = UDim2.new(0.5, 0, 0.35, 0)
        _TextLabel5.Size = UDim2.new(0, 26, 0, 24)
        _TextLabel5.Font = Enum.Font.SourceSans
        _TextLabel5.Text = 'Fps: '
        _TextLabel5.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextLabel5.TextSize = 14
        _TextLabel6.Parent = _Frame
        _TextLabel6.BackgroundTransparency = 1
        _TextLabel6.Position = UDim2.new(0.2, 0, 0.37, 0)
        _TextLabel6.Size = UDim2.new(0, 55, 0, 24)
        _TextLabel6.Font = Enum.Font.SourceSans
        _TextLabel6.Text = '0'
        _TextLabel6.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextLabel6.TextSize = 14
        _TextLabel6.TextWrapped = true
        _Frame2.Parent = _Frame
        _Frame2.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        _Frame2.Position = UDim2.new(0.004, 0, 0.24, 0)
        _Frame2.Size = UDim2.new(0, 224, 0, 5)
        _UICorner2.CornerRadius = UDim.new(0, 50)
        _UICorner2.Parent = _Frame2
        _TextLabel7.Parent = _Frame
        _TextLabel7.BackgroundTransparency = 1
        _TextLabel7.Position = UDim2.new(0.05, 0, 0.81, 0)
        _TextLabel7.Size = UDim2.new(0, 95, 0, 12)
        _TextLabel7.Font = Enum.Font.SourceSans
        _TextLabel7.Text = 'Anti-Afk Auto Enabled'
        _TextLabel7.TextColor3 = Color3.fromRGB(255, 255, 255)
        _TextLabel7.TextSize = 14

        local u104 = _Frame
        local _TweenService = game:GetService('TweenService')
        local _UserInputService = game:GetService('UserInputService')
        local u107 = nil
        local u108 = nil
        local u109 = nil
        local u110 = nil

        local function u114(p111)
            local v112 = p111.Position - u109
            local v113 = {
                Position = UDim2.new(u110.X.Scale, u110.X.Offset + v112.X, u110.Y.Scale, u110.Y.Offset + v112.Y),
            }

            _TweenService:Create(u104, TweenInfo.new(0.04, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), v113):Play()
        end

        u104.InputBegan:Connect(function(p115)
            if p115.UserInputType == Enum.UserInputType.MouseButton1 or p115.UserInputType == Enum.UserInputType.Touch then
                u107 = true
                u109 = p115.Position
                u110 = u104.Position

                p115.Changed:Connect(function()
                    if p115.UserInputState == Enum.UserInputState.End then
                        u107 = false
                    end
                end)
            end
        end)
        u104.InputChanged:Connect(function(p116)
            if p116.UserInputType == Enum.UserInputType.MouseMovement or p116.UserInputType == Enum.UserInputType.Touch then
                u108 = p116
            end
        end)
        _UserInputService.InputChanged:Connect(function(p117)
            if p117 == u108 and u107 then
                u114(p117)
            end
        end)

        local _VirtualUser = game:GetService('VirtualUser')

        game.Players.LocalPlayer.Idled:Connect(function()
            _VirtualUser:CaptureController()
            _VirtualUser:ClickButton2(Vector2.new())
        end)

        local _RunService = game:GetService('RunService')
        local u120 = tick()
        local u121 = {}

        _RunService.RenderStepped:Connect(function()
            local v122 = tick()

            for v123 = #u121, 1, -1 do
                u121[v123 + 1] = u121[v123] >= v122 - 1 and u121[v123] or nil
            end

            u121[1] = v122
            _TextLabel4.Text = math.floor(tick() - u120 >= 1 and #u121 or #u121 / (tick() - u120))
        end)
        spawn(function()
            while getgenv().AntiAfkExecuted do
                wait(1)

                _TextLabel6.Text = math.floor(tonumber(game:GetService('Stats'):FindFirstChild('PerformanceStats').Ping:GetValue()))
            end
        end)

        local u124 = 0
        local u125 = 0
        local u126 = 0

        getgenv().zamanbaslaticisi = true

        spawn(function()
            while getgenv().zamanbaslaticisi do
                u124 = u124 + 1

                wait(1)

                if u124 >= 60 then
                    u124 = 0
                    u125 = u125 + 1
                end
                if u125 >= 60 then
                    u125 = 0
                    u126 = u126 + 1
                end

                _TextLabel2.Text = u126 .. ':' .. u125 .. ':' .. u124
            end
        end)
    else
        getgenv().AntiAfkExecuted = false
        getgenv().zamanbaslaticisi = false

        if game.CoreGui:FindFirstChild('thisoneissocoldww') then
            game.CoreGui.thisoneissocoldww:Destroy()
        end
    end
end)
_FarmOP:AddSwitch('Auto Spin', function(p127)
    _G.AutoSpinWheel = p127

    if p127 then
        spawn(function()
            while _G.AutoSpinWheel and task.wait(0.1) do
                game:GetService('ReplicatedStorage').rEvents.openFortuneWheelRemote:InvokeServer('openFortuneWheel', game:GetService('ReplicatedStorage').fortuneWheelChances['Fortune Wheel'])
            end
        end)
    end
end)
_FarmOP:AddButton('Equip Swift Samurai', function()
    print('Bot\u{f3}n presionado: equipando 8 Swift Samurai')

    local _LocalPlayer6 = game:GetService('Players').LocalPlayer
    local _ReplicatedStorage3 = game:GetService('ReplicatedStorage')
    local _petsFolder2 = _LocalPlayer6:FindFirstChild('petsFolder')

    if _petsFolder2 then
        local v131, v132, v133 = pairs(_petsFolder2:GetChildren())

        while true do
            local v134

            v133, v134 = v131(v132, v133)

            if v133 == nil then
                break
            end
            if v134:IsA('Folder') then
                local v135, v136, v137 = pairs(v134:GetChildren())

                while true do
                    local v138

                    v137, v138 = v135(v136, v137)

                    if v137 == nil then
                        break
                    end

                    _ReplicatedStorage3.rEvents.equipPetEvent:FireServer('unequipPet', v138)
                end
            end
        end

        task.wait(0.1)

        local v139, v140, v141 = pairs(_petsFolder2:GetChildren())
        local v142 = 8
        local v143 = 0

        while true do
            local v144

            v141, v144 = v139(v140, v141)

            if v141 == nil then
                break
            end
            if v144:IsA('Folder') then
                local v145, v146, v147 = pairs(v144:GetChildren())

                while true do
                    local v148

                    v147, v148 = v145(v146, v147)

                    if v147 == nil then
                        break
                    end
                    if v148.Name == 'Swift Samurai' then
                        _ReplicatedStorage3.rEvents.equipPetEvent:FireServer('equipPet', v148)

                        v143 = v143 + 1

                        print('Equipado Swift Samurai #' .. v143)

                        if v142 <= v143 then
                            return
                        end
                    end
                end
            end
        end

        print('Se equiparon ' .. v143 .. ' Swift Samurai')
    end
end)
_FarmOP:AddButton('Jungle Squat', function()
    local _Character = _LocalPlayer3.Character

    if _Character and _Character:FindFirstChild('HumanoidRootPart') then
        _Character:SetPrimaryPartCFrame(CFrame.new(-8374.25586, 34.5933418, 2932.44995))

        local _machinesFolder = workspace:FindFirstChild('machinesFolder')
        local v151 = _machinesFolder and _machinesFolder:FindFirstChild('Jungle Squat') and _machinesFolder['Jungle Squat']:FindFirstChild('interactSeat')

        if v151 then
            game:GetService('ReplicatedStorage').rEvents.machineInteractRemote:InvokeServer('useMachine', v151)
        end

        print('[Jungle Squat] Acci\u{f3}n ejecutada.')
    else
        warn('[Jungle Squat] Personaje no encontrado o no tiene HumanoidRootPart.')
    end
end)

local _Rebirth = _FarmOP:AddFolder('Rebirth')

_Rebirth:AddSwitch('Fast Rebirths', function(p153)
    getgenv().AutoFarming = p153

    if p153 then
        task.spawn(function()
            local u154 = _ReplicatedStorage
            local u155 = _LocalPlayer3

            local function v166(p156)
                local _petsFolder3 = u155:FindFirstChild('petsFolder')

                if _petsFolder3 then
                    local v158, v159, v160 = pairs(_petsFolder3:GetChildren())

                    while true do
                        local v161

                        v160, v161 = v158(v159, v160)

                        if v160 == nil then
                            break
                        end
                        if v161:IsA('Folder') then
                            local v162, v163, v164 = pairs(v161:GetChildren())

                            while true do
                                local v165

                                v164, v165 = v162(v163, v164)

                                if v164 == nil then
                                    break
                                end
                                if v165.Name == p156 then
                                    u154.rEvents.equipPetEvent:FireServer('equipPet', v165)
                                end
                            end
                        end
                    end
                end
            end
            local function v176()
                local _petsFolder4 = u155:FindFirstChild('petsFolder')

                if _petsFolder4 then
                    local v168, v169, v170 = pairs(_petsFolder4:GetChildren())

                    while true do
                        local v171

                        v170, v171 = v168(v169, v170)

                        if v170 == nil then
                            break
                        end
                        if v171:IsA('Folder') then
                            local v172, v173, v174 = pairs(v171:GetChildren())

                            while true do
                                local v175

                                v174, v175 = v172(v173, v174)

                                if v174 == nil then
                                    break
                                end

                                u154.rEvents.equipPetEvent:FireServer('unequipPet', v175)
                            end
                        end
                    end

                    task.wait(0.1)
                end
            end
            local function u178()
                local _ultimatesFolder = u155:FindFirstChild('ultimatesFolder')

                return not (_ultimatesFolder and _ultimatesFolder:FindFirstChild('Golden Rebirth')) and 0 or _ultimatesFolder['Golden Rebirth'].Value
            end
            local function v181()
                local v179 = 10000 + 5000 * u155.leaderstats.Rebirths.Value
                local v180 = u178()

                if 1 <= v180 and v180 <= 5 then
                    v179 = v179 * (1 - v180 * 0.1)
                end

                return math.floor(v179)
            end

            while getgenv().AutoFarming do
                local v182 = v181()

                v176()
                v166('Swift Samurai')

                while u155.leaderstats.Strength.Value < v182 and getgenv().AutoFarming do
                    for _ = 1, 10 do
                        u155.muscleEvent:FireServer('rep')
                    end

                    task.wait()
                end

                if getgenv().AutoFarming then
                    v176()
                    v166('Tribal Overlord')

                    local _Value = u155.leaderstats.Rebirths.Value

                    repeat
                        u154.rEvents.rebirthRemote:InvokeServer('rebirthRequest')
                        task.wait(0.1)
                    until _Value < u155.leaderstats.Rebirths.Value or not getgenv().AutoFarming
                end

                task.wait()
            end
        end)
    end
end)
_Rebirth:AddSwitch('Lock Position', function(p184)
    if p184 then
        local _CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame

        getgenv().posLock = game:GetService('RunService').Heartbeat:Connect(function()
            if game.Players.LocalPlayer.Character:FindFirstChild('HumanoidRootPart') then
                game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = _CFrame
            end
        end)
    elseif getgenv().posLock then
        getgenv().posLock:Disconnect()

        getgenv().posLock = nil
    end
end)
_Rebirth:AddSwitch('Hide All Frames', function(p186)
    local _ReplicatedStorage4 = game:GetService('ReplicatedStorage')
    local v188, v189, v190 = pairs(_ReplicatedStorage4:GetChildren())

    while true do
        local v191

        v190, v191 = v188(v189, v190)

        if v190 == nil then
            break
        end
        if v191.Name:match('Frame$') then
            v191.Visible = not p186
        end
    end
end)
_Rebirth:AddButton('Anti Lag', function()
    local v192, v193, v194 = pairs(game:GetDescendants())

    while true do
        local v195

        v194, v195 = v192(v193, v194)

        if v194 == nil then
            break
        end
        if v195:IsA('ParticleEmitter') or v195:IsA('Smoke') or (v195:IsA('Fire') or v195:IsA('Sparkles')) then
            v195.Enabled = false
        end
    end

    local _Lighting3 = game:GetService('Lighting')

    _Lighting3.GlobalShadows = false
    _Lighting3.FogEnd = 9000000000
    _Lighting3.Brightness = 0
    settings().Rendering.QualityLevel = 1

    local v197, v198, v199 = pairs(game:GetDescendants())

    while true do
        local v200

        v199, v200 = v197(v198, v199)

        if v199 == nil then
            break
        end
        if v200:IsA('Decal') or v200:IsA('Texture') then
            v200.Transparency = 1
        elseif v200:IsA('BasePart') and not v200:IsA('MeshPart') then
            v200.Material = Enum.Material.SmoothPlastic

            if not (v200.Parent and (v200.Parent:FindFirstChild('Humanoid') or v200.Parent.Parent:FindFirstChild('Humanoid'))) then
                v200.Reflectance = 0
            end
        end
    end

    local v201, v202, v203 = pairs(_Lighting3:GetChildren())

    while true do
        local v204

        v203, v204 = v201(v202, v203)

        if v203 == nil then
            break
        end
        if v204:IsA('BlurEffect') or v204:IsA('SunRaysEffect') or (v204:IsA('ColorCorrectionEffect') or v204:IsA('BloomEffect')) or v204:IsA('DepthOfFieldEffect') then
            v204.Enabled = false
        end
    end

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Anti Lag Activado',
        Text = 'Full optimization applied!',
        Duration = 5,
    })
end)
_Rebirth:AddButton('Spin Auto', function(p205)
    _G.AutoSpinWheel = p205

    if p205 then
        spawn(function()
            while _G.AutoSpinWheel and task.wait(0.1) do
                game:GetService('ReplicatedStorage').rEvents.openFortuneWheelRemote:InvokeServer('openFortuneWheel', game:GetService('ReplicatedStorage').fortuneWheelChances['Fortune Wheel'])
            end
        end)
    end
end)
_Rebirth:AddButton('Jungle Lift', function()
    local _LocalPlayer7 = game.Players.LocalPlayer;

    (_LocalPlayer7.Character or _LocalPlayer7.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(-8652.8672, 29.2667, 2089.2617)

    task.wait(0.2)

    local _VirtualInputManager = game:GetService('VirtualInputManager')

    _VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.E, false, game)
    task.wait(0.05)
    _VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.E, false, game)
    print('[Jungle Lift] Teletransport ejecutado correctamente.')
end)
_FarmOP:AddLabel('Farm Sin Packs')

local _AutoTools = _FarmOP:AddFolder('Auto Tools')

_AutoTools:AddSwitch('Auto Pushups y Pegarle Ala Roca de 10M', function(p209)
    getgenv().autoPushups10M = p209

    task.spawn(function()
        while getgenv().autoPushups10M and _LocalPlayer3.Character do
            local v210 = _LocalPlayer3.Backpack:FindFirstChild('Pushups') or _LocalPlayer3.Character:FindFirstChild('Pushups')

            if v210 then
                v210.Parent = _LocalPlayer3.Character

                _ReplicatedStorage.muscleEvent:FireServer('punch', 'rightHand')
                _ReplicatedStorage.muscleEvent:FireServer('punch', 'leftHand')

                local _KingRock = Workspace.machinesFolder:FindFirstChild('King Rock')

                if _KingRock and (_LocalPlayer3:FindFirstChild('Durability') and 10000000 <= _LocalPlayer3.Durability.Value) then
                    firetouchinterest(_KingRock.Rock, _LocalPlayer3.Character.RightHand, 0)
                    firetouchinterest(_KingRock.Rock, _LocalPlayer3.Character.LeftHand, 0)
                    task.wait()
                    firetouchinterest(_KingRock.Rock, _LocalPlayer3.Character.RightHand, 1)
                    firetouchinterest(_KingRock.Rock, _LocalPlayer3.Character.LeftHand, 1)
                end
            end

            task.wait(0.01)
        end
    end)
end)
_AutoTools:AddSwitch('Fuerza OP', function(p212)
    getgenv()._AutoRepFarmEnabled = p212

    warn('[Auto Rep Farm] Estado cambiado a:', p212 and 'ON' or 'OFF')
end)

local _ReplicatedStorage5 = game:GetService('ReplicatedStorage')
local _Players2 = game:GetService('Players')
local _Stats2 = game:GetService('Stats')
local _LocalPlayer8 = _Players2.LocalPlayer
local u217 = 'Swift Samurai'
local u218 = 'Rock5M'
local u219 = 'ProteinEgg'
local u220 = 1800
local u221 = 160
local u222 = 0.01
local u223 = 5
local u224 = 700
local u225 = nil
local u226 = 0
local u227 = 0
local u228 = workspace:FindFirstChild(u218)

local function u231()
    local v229, v230 = pcall(function()
        return _Stats2.Network.ServerStatsItem['Data Ping']:GetValue()
    end)

    return v229 and v230 and v230 or 999
end
local function u232()
    u225 = (_LocalPlayer8.Character or _LocalPlayer8.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart', 5)
end
local function u238()
    local _petsFolder5 = _LocalPlayer8:FindFirstChild('petsFolder')

    if _petsFolder5 and _petsFolder5:FindFirstChild('Unique') then
        local v234, v235, v236 = pairs(_petsFolder5.Unique:GetChildren())

        while true do
            local v237

            v236, v237 = v234(v235, v236)

            if v236 == nil then
                break
            end
            if v237.Name == u217 then
                _ReplicatedStorage5.rEvents.equipPetEvent:FireServer('equipPet', v237)

                break
            end
        end
    end
end
local function u243()
    if _LocalPlayer8:FindFirstChild('Backpack') then
        local v239, v240, v241 = pairs(_LocalPlayer8.Backpack:GetChildren())

        while true do
            local v242

            v241, v242 = v239(v240, v241)

            if v241 == nil then
                break
            end
            if v242.Name == u219 then
                _ReplicatedStorage5.rEvents.eatEvent:FireServer('eat', v242)

                break
            end
        end
    end
end
local function u244()
    if not (u228 and u228.Parent) then
        u228 = workspace:FindFirstChild(u218)
    end
    if u228 and u225 then
        u225.CFrame = u228.CFrame * CFrame.new(0, 0, -5)

        _ReplicatedStorage5.rEvents.hitEvent:FireServer('hit', u228)
    end
end

if not getgenv()._AutoRepFarmLoop then
    getgenv()._AutoRepFarmLoop = true

    task.spawn(function()
        u232()
        u238()

        u226 = tick()
        u227 = tick()

        while true do
            while getgenv()._AutoRepFarmEnabled do
                local v245 = u231()

                if u224 >= v245 then
                    if _LocalPlayer8:FindFirstChild('muscleEvent') then
                        for _ = 1, u221 do
                            _LocalPlayer8.muscleEvent:FireServer('rep')
                        end
                    end
                    if u220 <= tick() - u226 then
                        u243()

                        u226 = tick()
                    end
                    if u223 <= tick() - u227 then
                        u244()

                        u227 = tick()
                    end

                    task.wait(u222)
                else
                    warn('[Auto Rep Farm] Ping alto (' .. math.floor(v245) .. 'ms), pausando 5s...')
                    task.wait(5)
                end
            end

            task.wait(1)
        end
    end)
end

_AutoTools:AddButton('Gamepass AutoLift', function()
    local _gamepassIds = game:GetService('ReplicatedStorage').gamepassIds
    local _LocalPlayer9 = game:GetService('Players').LocalPlayer
    local v248, v249, v250 = pairs(_gamepassIds:GetChildren())

    while true do
        local v251

        v250, v251 = v248(v249, v250)

        if v250 == nil then
            break
        end

        local _IntValue = Instance.new('IntValue')

        _IntValue.Name = v251.Name
        _IntValue.Value = v251.Value
        _IntValue.Parent = _LocalPlayer9.ownedGamepasses
    end
end)

local function v258(p253, p254)
    _AutoTools:AddSwitch('Auto ' .. p253, function(p255)
        _G[p254] = p255

        if p255 then
            local v256 = _LocalPlayer8.Backpack:FindFirstChild(p253)

            if v256 then
                _LocalPlayer8.Character.Humanoid:EquipTool(v256)
            end
        else
            local v257 = _LocalPlayer8.Character:FindFirstChild(p253)

            if v257 then
                v257.Parent = _LocalPlayer8.Backpack
            end
        end

        task.spawn(function()
            while _G[p254] and _G[p254] do
                _LocalPlayer8.muscleEvent:FireServer('rep')
                task.wait(0.1)
            end
        end)
    end)
end

v258('Weight', 'AutoWeight')
v258('Pushups', 'AutoPushups')
v258('Handstands', 'AutoHandstands')
v258('Situps', 'AutoSitups')
_AutoTools:AddSwitch('Auto Punch', function(p259)
    _G.fastHitActive = p259

    if p259 then
        task.spawn(function()
            while _G.fastHitActive and _G.fastHitActive do
                local _Punch = _LocalPlayer8.Backpack:FindFirstChild('Punch')

                if _Punch then
                    _Punch.Parent = _LocalPlayer8.Character

                    if _Punch:FindFirstChild('attackTime') then
                        _Punch.attackTime.Value = 0
                    end
                end

                task.wait(0.1)
            end
        end)
        task.spawn(function()
            while _G.fastHitActive and _G.fastHitActive do
                _LocalPlayer8.muscleEvent:FireServer('punch', 'rightHand')
                _LocalPlayer8.muscleEvent:FireServer('punch', 'leftHand')

                local _Character2 = _LocalPlayer8.Character
                local v262 = _Character2 and _Character2:FindFirstChild('Punch')

                if v262 then
                    v262:Activate()
                end

                task.wait()
            end
        end)
    else
        local _Punch2 = _LocalPlayer8.Character:FindFirstChild('Punch')

        if _Punch2 then
            _Punch2.Parent = _LocalPlayer8.Backpack
        end
    end
end)
_AutoTools:AddSwitch('Fast Tools', function(p264)
    _G.FastTools = p264

    local _Backpack3 = _LocalPlayer8:WaitForChild('Backpack')
    local v266, v267, v268 = ipairs({
        {
            'Punch',
            'attackTime',
            p264 and 0 or 0.35,
        },
        {
            'Ground Slam',
            'attackTime',
            p264 and 0 or 6,
        },
        {
            'Stomp',
            'attackTime',
            p264 and 0 or 7,
        },
        {
            'Handstands',
            'repTime',
            p264 and 0 or 1,
        },
        {
            'Pushups',
            'repTime',
            p264 and 0 or 1,
        },
        {
            'Weight',
            'repTime',
            p264 and 0 or 1,
        },
        {
            'Situps',
            'repTime',
            p264 and 0 or 1,
        },
    })

    while true do
        local v269

        v268, v269 = v266(v267, v268)

        if v268 == nil then
            break
        end

        local v270 = _Backpack3:FindFirstChild(v269[1])

        if v270 and v270:FindFirstChild(v269[2]) then
            v270[v269[2] ].Value = v269[3]
        end

        local _Character3 = _LocalPlayer8.Character

        if _Character3 then
            _Character3 = _LocalPlayer8.Character:FindFirstChild(v269[1])
        end
        if _Character3 and _Character3:FindFirstChild(v269[2]) then
            _Character3[v269[2] ].Value = v269[3]
        end
    end
end)

local _RockFarming = _FarmOP:AddFolder('Rock Farming')

local function u277()
    local _LocalPlayer10 = game.Players.LocalPlayer
    local _Character4 = _LocalPlayer10.Character

    if _Character4 then
        local _Backpack4 = _LocalPlayer10:FindFirstChild('Backpack')

        if _Backpack4 then
            local _Punch3 = _Backpack4:FindFirstChild('Punch')

            if _Punch3 and _Character4:FindFirstChildOfClass('Humanoid') then
                _Character4:FindFirstChildOfClass('Humanoid'):EquipTool(_Punch3)
            end

            task.spawn(function()
                for _ = 1, 5 do
                    _LocalPlayer10.muscleEvent:FireServer('punch', 'leftHand')
                    _LocalPlayer10.muscleEvent:FireServer('punch', 'rightHand')
                    task.wait()
                end
            end)
        end
    else
        return
    end
end
local function v287(p278, p279)
    return _RockFarming:AddSwitch('Farm ' .. p278, function(p280)
        selectrock = p278
        getgenv().autoFarm = p280

        if p280 then
            task.spawn(function()
                while getgenv().autoFarm do
                    task.wait(0.001)

                    local _LocalPlayer11 = game.Players.LocalPlayer

                    if _LocalPlayer11 and p279 <= _LocalPlayer11.Durability.Value then
                        local v282, v283, v284 = pairs(workspace.machinesFolder:GetDescendants())

                        while true do
                            local v285

                            v284, v285 = v282(v283, v284)

                            if v284 == nil then
                                break
                            end
                            if v285.Name == 'neededDurability' and v285.Value == p279 then
                                local _Character5 = _LocalPlayer11.Character

                                if _Character5 and _Character5:FindFirstChild('LeftHand') and _Character5:FindFirstChild('RightHand') then
                                    firetouchinterest(v285.Parent.Rock, _Character5.RightHand, 0)
                                    firetouchinterest(v285.Parent.Rock, _Character5.RightHand, 1)
                                    firetouchinterest(v285.Parent.Rock, _Character5.LeftHand, 0)
                                    firetouchinterest(v285.Parent.Rock, _Character5.LeftHand, 1)
                                    u277()
                                end
                            end
                        end
                    end
                end
            end)
        end
    end)
end

v287('Tiny Island Rock', 0)
v287('Starter Island Rock', 100)
v287('Legend Beach Rock', 5000)
v287('Frost Gym Rock', 150000)
v287('Mythical Gym Rock', 400000)
v287('Eternal Gym Rock', 750000)
v287('Legend Gym Rock', 1000000)
v287('Muscle King Gym Rock', 5000000)
v287('Ancient Jungle Rock', 10000000)

getgenv().StarterIslandBenchPress = false
getgenv().StarterIslandSquat = false
getgenv().StarterIslandDeadlift = false
getgenv().StarterIslandPullUp = false
getgenv().StarterIslandBoulder = false
getgenv().LegendBeachBenchPress = false
getgenv().LegendBeachSquat = false
getgenv().LegendBeachDeadlift = false
getgenv().LegendBeachPullUp = false
getgenv().LegendBeachBoulder = false
getgenv().FrostGymBenchPress = false
getgenv().FrostGymSquat = false
getgenv().FrostGymDeadlift = false
getgenv().FrostGymPullUp = false
getgenv().FrostGymBoulder = false
getgenv().MythicalGymBenchPress = false
getgenv().MythicalGymSquat = false
getgenv().MythicalGymDeadlift = false
getgenv().MythicalGymPullUp = false
getgenv().MythicalGymBoulder = false
getgenv().EternalGymBenchPress = false
getgenv().EternalGymSquat = false
getgenv().EternalGymDeadlift = false
getgenv().EternalGymPullUp = false
getgenv().EternalGymBoulder = false
getgenv().LegendGymBenchPress = false
getgenv().LegendGymSquat = false
getgenv().LegendGymDeadlift = false
getgenv().LegendGymPullUp = false
getgenv().LegendGymBoulder = false
getgenv().MuscleKingGymBenchPress = false
getgenv().MuscleKingGymSquat = false
getgenv().MuscleKingGymDeadlift = false
getgenv().MuscleKingGymPullUp = false
getgenv().MuscleKingGymBoulder = false
getgenv().JungleGymBenchPress = false
getgenv().JungleGymSquat = false
getgenv().JungleGymDeadlift = false
getgenv().JungleGymPullUp = false
getgenv().JungleGymBoulder = false

local u288 = {
    StarterIsland = {
        BenchPress = CFrame.new(-17.0609932, 3.31417918, -2.48164988),
        Squat = CFrame.new(-48.8711243, 3.31417918, -11.8831778),
        Deadlift = CFrame.new(-48.8711243, 3.31417918, -11.8831778),
        PullUp = CFrame.new(-33.3047485, 3.31417918, -11.8831778),
        Boulder = CFrame.new(-33.3047485, 3.31417918, -11.8831778),
    },
    LegendBeach = {
        BenchPress = CFrame.new(470.334656, 3.31417966, -321.053925),
        Squat = CFrame.new(470.334656, 3.31417966, -321.053925),
        Deadlift = CFrame.new(470.334656, 3.31417966, -321.053925),
        PullUp = CFrame.new(470.334656, 3.31417966, -321.053925),
        Boulder = CFrame.new(470.334656, 3.31417966, -321.053925),
    },
    FrostGym = {
        BenchPress = CFrame.new(-3013.24194, 39.2158546, -335.036926),
        Squat = CFrame.new(-2933.47998, 29.6399612, -579.946045),
        Deadlift = CFrame.new(-2933.47998, 29.6399612, -579.946045),
        PullUp = CFrame.new(-2933.47998, 29.6399612, -579.946045),
        Boulder = CFrame.new(-2933.47998, 29.6399612, -579.946045),
    },
    MythicalGym = {
        BenchPress = CFrame.new(2371.7356, 39.2158546, 1246.31555),
        Squat = CFrame.new(2489.21484, 3.67686629, 849.051025),
        Deadlift = CFrame.new(2489.21484, 3.67686629, 849.051025),
        PullUp = CFrame.new(2489.21484, 3.67686629, 849.051025),
        Boulder = CFrame.new(2489.21484, 3.67686629, 849.051025),
    },
    EternalGym = {
        BenchPress = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        Squat = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        Deadlift = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        PullUp = CFrame.new(-7176.19141, 45.394104, -1106.31421),
        Boulder = CFrame.new(-7176.19141, 45.394104, -1106.31421),
    },
    LegendGym = {
        BenchPress = CFrame.new(4111.91748, 1020.46674, -3799.97217),
        Squat = CFrame.new(4304.99023, 987.829956, -4124.2334),
        Deadlift = CFrame.new(4304.99023, 987.829956, -4124.2334),
        PullUp = CFrame.new(4304.99023, 987.829956, -4124.2334),
        Boulder = CFrame.new(4304.99023, 987.829956, -4124.2334),
    },
    MuscleKingGym = {
        BenchPress = CFrame.new(-8590.06152, 46.0167427, -6043.34717),
        Squat = CFrame.new(-8940.12402, 13.1642084, -5699.13477),
        Deadlift = CFrame.new(-8940.12402, 13.1642084, -5699.13477),
        PullUp = CFrame.new(-8940.12402, 13.1642084, -5699.13477),
        Boulder = CFrame.new(-8940.12402, 13.1642084, -5699.13477),
    },
    JungleGym = {
        BenchPress = CFrame.new(-8173, 64, 1898),
        Squat = CFrame.new(-8352, 34, 2878),
        Deadlift = CFrame.new(-8352, 34, 2878),
        PullUp = CFrame.new(-8666, 34, 2070),
        Boulder = CFrame.new(-8621, 34, 2684),
    },
}

function teleportLoop(p289)
    task.spawn(function()
        while getgenv()[p289] do
            local v290, v291, v292 = pairs(u288)
            local v293 = {}

            while true do
                local v294

                v292, v294 = v290(v291, v292)

                if v292 == nil then
                    break
                end

                local v295, v296, v297 = pairs(v294)
                local v298 = v292

                while true do
                    local v299

                    v297, v299 = v295(v296, v297)

                    if v297 == nil then
                        break
                    end
                    if p289 == v298 .. v297 then
                        table.insert(v293, v299)
                    end
                end
            end

            if #v293 > 0 then
                local _Character6 = game.Players.LocalPlayer.Character

                if _Character6 and _Character6:FindFirstChild('HumanoidRootPart') then
                    _Character6.HumanoidRootPart.CFrame = v293[1]
                end
            end

            task.wait(0.1)
        end
    end)
end

local _Rebirths = _FarmOP:AddFolder('Rebirths')

_Rebirths:AddTextBox('Rebirth Target', function(p302)
    local v303 = tonumber(p302)

    if v303 and 0 < v303 then
        targetRebirthValue = v303

        updateStats()
        game:GetService('StarterGui'):SetCore('SendNotification', {
            Title = 'Objetivo Actualizado',
            Text = 'Nuevo objetivo: ' .. tostring(targetRebirthValue) .. ' renacimientos',
            Duration = 0,
        })
    else
        game:GetService('StarterGui'):SetCore('SendNotification', {
            Title = 'Entrada Inv\u{c3}\u{a1}lida',
            Text = 'Por favor ingresa un n\u{c3}\u{ba}mero v\u{c3}\u{a1}lido mayor que 0',
            Duration = 0,
        })
    end
end)

local u304 = nil
local _AutoRebirthTarget = _Rebirths:AddSwitch('Auto Rebirth Target', function(p305)
    _G.targetRebirthActive = p305

    if p305 then
        if _G.infiniteRebirthActive and u304 then
            u304:Set(false)

            _G.infiniteRebirthActive = false
        end

        spawn(function()
            while _G.targetRebirthActive and wait(0.1) do
                if game.Players.LocalPlayer.leaderstats.Rebirths.Value >= targetRebirthValue then
                    targetSwitch:Set(false)

                    _G.targetRebirthActive = false

                    game:GetService('StarterGui'):SetCore('SendNotification', {
                        Title = '\u{c2}\u{a1}Objetivo Alcanzado!',
                        Text = 'Has alcanzado ' .. tostring(targetRebirthValue) .. ' renacimientos',
                        Duration = 5,
                    })

                    break
                end

                game:GetService('ReplicatedStorage').rEvents.rebirthRemote:InvokeServer('rebirthRequest')
            end
        end)
    end
end, 'Renacimiento autom\u{c3}\u{a1}tico hasta alcanzar el objetivo')

u304 = _Rebirths:AddSwitch('Auto Rebirth (Infinite)', function(p307)
    _G.infiniteRebirthActive = p307

    if p307 then
        if _G.targetRebirthActive and _AutoRebirthTarget then
            _AutoRebirthTarget:Set(false)

            _G.targetRebirthActive = false
        end

        spawn(function()
            while _G.infiniteRebirthActive and wait(0.1) do
                game:GetService('ReplicatedStorage').rEvents.rebirthRemote:InvokeServer('rebirthRequest')
            end
        end)
    end
end, 'Renacimiento continuo sin parar')

_Rebirths:AddSwitch('Auto Size 1', function(p308)
    _G.autoSizeActive = p308

    if p308 then
        spawn(function()
            while _G.autoSizeActive and wait() do
                game:GetService('ReplicatedStorage').rEvents.changeSpeedSizeRemote:InvokeServer('changeSize', 1)
            end
        end)
    end
end, 'Establece el tama\u{c3}\u{b1}o del personaje a 1 continuamente')
_Rebirths:AddSwitch('Auto Teleport to Muscle King', function(p309)
    _G.teleportActive = p309

    if p309 then
        spawn(function()
            while _G.teleportActive and wait() do
                if game.Players.LocalPlayer.Character then
                    game.Players.LocalPlayer.Character:MoveTo(Vector3.new(-8646, 17, -5738))
                end
            end
        end)
    end
end, 'Teletransporte continuo al Rey M\u{c3}\u{ba}sculo')

local _StatsFarm = v3:AddTab('Stats Farm')
local _LocalPlayer12 = game.Players.LocalPlayer
local _Strength = _LocalPlayer12:WaitForChild('leaderstats'):WaitForChild('Strength')
local _Durability = _LocalPlayer12:WaitForChild('Durability')

local function u317(p314)
    local v315 = p314 < 0
    local v316 = math.abs(p314)

    if v316 >= 1000000000000000 then
        return (v315 and '-' or '') .. string.format('%.2fQa', v316 / 1000000000000000)
    elseif v316 >= 1000000000000 then
        return (v315 and '-' or '') .. string.format('%.2fT', v316 / 1000000000000)
    elseif v316 >= 1000000000 then
        return (v315 and '-' or '') .. string.format('%.2fB', v316 / 1000000000)
    elseif v316 >= 1000000 then
        return (v315 and '-' or '') .. string.format('%.2fM', v316 / 1000000)
    elseif v316 >= 1000 then
        return (v315 and '-' or '') .. string.format('%.2fK', v316 / 1000)
    else
        return (v315 and '-' or '') .. string.format('%.2f', v316)
    end
end

local _FastRepTime0d0h0m0s = _StatsFarm:AddLabel('Fast Rep Time: 0d 0h 0m 0s')

_FastRepTime0d0h0m0s.TextSize = 20

local _StrengthRate0Hour0Day0Week0Month = _StatsFarm:AddLabel('Strength Rate: 0 /Hour | 0 /Day | 0 /Week | 0 /Month')

_StrengthRate0Hour0Day0Week0Month.TextSize = 20

local _DurabilityRate0Hour0Day0Week0Month = _StatsFarm:AddLabel('Durability Rate: 0 /Hour | 0 /Day | 0 /Week | 0 /Month')

_DurabilityRate0Hour0Day0Week0Month.TextSize = 20
_StatsFarm:AddLabel('').TextSize = 10
_StatsFarm:AddLabel('Stats:').TextSize = 24

local _Strength0Gained0 = _StatsFarm:AddLabel('Strength: 0 | Gained: 0')

_Strength0Gained0.TextSize = 20

local _Durability0Gained0 = _StatsFarm:AddLabel('Durability: 0 | Gained: 0')

_Durability0Gained0.TextSize = 20

local u323 = tick()
local _Value2 = _Strength.Value
local _Value3 = _Durability.Value
local u326 = false
local u327 = {}
local u328 = {}
local u329 = 10

task.spawn(function()
    local v330 = tick()

    while true do
        v350 = tick()

        local _Value4 = _Strength.Value
        local _Value5 = _Durability.Value

        if not u326 and _Value4 - _Value2 >= 100000000000 then
            u326 = true
            u323 = tick()
            u327 = {}
            u328 = {}
        end
        if not u326 then
            break
        end

        local v333 = v350 - u323
        local v334 = math.floor(v333 / 86400)
        local v335 = math.floor(v333 % 86400 / 3600)
        local v336 = math.floor(v333 % 3600 / 60)
        local v337 = math.floor(v333 % 60)

        _FastRepTime0d0h0m0s.Text = string.format('Fast Rep Time: %dd %dh %dm %ds', v334, v335, v336, v337)

        local v338 = _Value4 - _Value2
        local v339 = _Value5 - _Value3

        _Strength0Gained0.Text = 'Strength: ' .. u317(_Value4) .. ' | Gained: ' .. u317(v338)
        _Durability0Gained0.Text = 'Durability: ' .. u317(_Value5) .. ' | Gained: ' .. u317(v339)

        table.insert(u327, {
            time = v350,
            value = _Value4,
        })
        table.insert(u328, {
            time = v350,
            value = _Value5,
        })

        while#u327 > 0 and u329 < v350 - u327[1].time do
            table.remove(u327, 1)
        end
        while#u328 > 0 and u329 < v350 - u328[1].time do
            table.remove(u328, 1)
        end

        if u329 > v350 - v330 then
            v350 = v330
        else
            if #u327 >= 2 then
                local v340 = (u327[#u327].value - u327[1].value) / u329
                local v341 = math.floor(v340 * 3600)
                local v342 = math.floor(v340 * 86400)
                local v343 = math.floor(v340 * 604800)
                local v344 = math.floor(v340 * 2592000)

                _StrengthRate0Hour0Day0Week0Month.Text = 'Strength Rate: ' .. u317(v341) .. '/Hour | ' .. u317(v342) .. '/Day | ' .. u317(v343) .. '/Week | ' .. u317(v344) .. '/Month'
            end
            if #u328 >= 2 then
                local v345 = (u328[#u328].value - u328[1].value) / u329
                local v346 = math.floor(v345 * 3600)
                local v347 = math.floor(v345 * 86400)
                local v348 = math.floor(v345 * 604800)
                local v349 = math.floor(v345 * 2592000)

                _DurabilityRate0Hour0Day0Week0Month.Text = 'Durability Rate: ' .. u317(v346) .. '/Hour | ' .. u317(v347) .. '/Day | ' .. u317(v348) .. '/Week | ' .. u317(v349) .. '/Month'
            end
        end

        task.wait(0.05)

        v330 = v350
    end

    local v350 = v330
end)

local _Calculator = v3:AddTab('Calculator', Color3.fromRGB(200, 100, 100))
local u352 = 0
local _PackDamageCalculator = _Calculator:AddFolder('Pack Damage Calculator')

_PackDamageCalculator:AddTextBox('Base Strongth (ej: 1.27Qa, T, B)', function(p354)
    local v355 = p354:upper()
    local v356, v357, v358 = pairs({
        T = 1000000000000,
        Q = 1000000000000000,
        B = 1000000000,
    })

    while true do
        local v359

        v358, v359 = v356(v357, v358)

        if v358 == nil then
            break
        end
        if v355:find(v358) then
            local v360 = tonumber(v355:match('(%d+%.?%d*)'))

            if v360 then
                u352 = v360 * v359

                return
            end
        end
    end

    u352 = tonumber(v355:match('(%d+%.?%d*)')) or 0
end)

local u361 = _PackDamageCalculator:AddLabel('')
local u362 = u352
local u363 = {}

for v364 = 1, 8 do
    local v365 = v364

    u363[v365] = _PackDamageCalculator:AddLabel(string.format('%d pack(s): -', v365))
end

_PackDamageCalculator:AddButton('Calculate Damage', function()
    if u362 > 0 then
        u361.Text = ''

        local v366 = u362 * 0.1
        local v367 = 0.335

        for v368 = 1, 8 do
            local v369 = v366 * (1 + v368 * v367)
            local v370

            if v369 >= 1000000000000000 then
                v370 = string.format('%.3f Qa', v369 / 1000000000000000)
            elseif v369 >= 1000000000000 then
                v370 = string.format('%.2f T', v369 / 1000000000000)
            elseif v369 >= 1000000000 then
                v370 = string.format('%.2f B', v369 / 1000000000)
            else
                v370 = tostring(math.floor(v369))
            end

            u363[v368].Text = string.format('%d pack(s): %s', v368, v370)
        end
    else
        u361.Text = 'Enter a valid value.'

        for v371 = 1, 8 do
            u363[v371].Text = string.format('%d pack(s): -', v371)
        end
    end
end)

local u372 = 0
local _PackDurabilityCalculator = _Calculator:AddFolder('Pack Durability Calculator')

_PackDurabilityCalculator:AddTextBox('Base durability (ej: 1.27Qa, T, B)', function(p374)
    local v375 = p374:upper()
    local v376, v377, v378 = pairs({
        T = 1000000000000,
        Q = 1000000000000000,
        B = 1000000000,
    })

    while true do
        local v379

        v378, v379 = v376(v377, v378)

        if v378 == nil then
            break
        end
        if v375:find(v378) then
            local v380 = tonumber(v375:match('(%d+%.?%d*)'))

            if v380 then
                u372 = v380 * v379

                return
            end
        end
    end

    u372 = tonumber(v375:match('(%d+%.?%d*)')) or 0
end)

local u381 = _PackDurabilityCalculator:AddLabel('')
local u382 = u372
local u383 = {}

for v384 = 1, 8 do
    u383[v384] = _PackDurabilityCalculator:AddLabel(string.format('%d pack(s): -', v384))
end

_PackDurabilityCalculator:AddButton('Calculate Durability', function()
    if u382 > 0 then
        u381.Text = ''

        local v385 = 0.335
        local v386 = 1.5

        for v387 = 1, 8 do
            local v388 = u382 * (1 + v387 * v385) * v386
            local v389

            if v388 >= 1000000000000000 then
                v389 = string.format('%.3f Qa', v388 / 1000000000000000)
            elseif v388 >= 1000000000000 then
                v389 = string.format('%.2f T', v388 / 1000000000000)
            elseif v388 >= 1000000000 then
                v389 = string.format('%.2f B', v388 / 1000000000)
            else
                v389 = tostring(math.floor(v388))
            end

            u383[v387].Text = string.format('%d pack(s): %s', v387, v389)
        end
    else
        u381.Text = 'Enter a valid value.'

        for v390 = 1, 8 do
            u383[v390].Text = string.format('%d pack(s): -', v390)
        end
    end
end)

local _Killer = v3:AddTab('Killer')
local _Selectdamageordurabilitypet = _Killer:AddLabel('Select damage or durability pet')

_Selectdamageordurabilitypet.TextSize = 18
_Selectdamageordurabilitypet.Font = Enum.Font.Merriweather
_Selectdamageordurabilitypet.TextColor3 = Color3.fromRGB(255, 255, 255)

local _SelectPet = _Killer:AddDropdown('Select Pet', function(p393)
    local _petsFolder6 = game.Players.LocalPlayer.petsFolder
    local v395, v396, v397 = pairs(_petsFolder6:GetChildren())

    while true do
        local v398

        v397, v398 = v395(v396, v397)

        if v397 == nil then
            break
        end
        if v398:IsA('Folder') then
            local v399, v400, v401 = pairs(v398:GetChildren())

            while true do
                local v402

                v401, v402 = v399(v400, v401)

                if v401 == nil then
                    break
                end

                game:GetService('ReplicatedStorage').rEvents.equipPetEvent:FireServer('unequipPet', v402)
            end
        end
    end

    task.wait(0.2)

    local v403, v404, v405 = pairs(game.Players.LocalPlayer.petsFolder.Unique:GetChildren())
    local v406 = {}

    while true do
        local v407

        v405, v407 = v403(v404, v405)

        if v405 == nil then
            break
        end
        if v407.Name == p393 then
            table.insert(v406, v407)
        end
    end

    for v408 = 1, math.min(#v406, 8)do
        game:GetService('ReplicatedStorage').rEvents.equipPetEvent:FireServer('equipPet', v406[v408])
        task.wait(0.1)
    end
end)

_SelectPet:Add('Wild Wizard')
_SelectPet:Add('Mighty Monster')
_Killer:AddSwitch('Anti Knockback', function(p410)
    if p410 then
        local _Name = game.Players.LocalPlayer.Name
        local _HumanoidRootPart = game.Workspace:FindFirstChild(_Name):FindFirstChild('HumanoidRootPart')
        local _BodyVelocity = Instance.new('BodyVelocity')

        _BodyVelocity.MaxForce = Vector3.new(100000, 0, 100000)
        _BodyVelocity.Velocity = Vector3.new(0, 0, 0)
        _BodyVelocity.P = 1250
        _BodyVelocity.Parent = _HumanoidRootPart
    else
        local _Name2 = game.Players.LocalPlayer.Name
        local _BodyVelocity2 = game.Workspace:FindFirstChild(_Name2):FindFirstChild('HumanoidRootPart'):FindFirstChild('BodyVelocity')

        if _BodyVelocity2 and _BodyVelocity2.MaxForce == Vector3.new(100000, 0, 100000) then
            _BodyVelocity2:Destroy()
        end
    end
end)

_G.whitelistedPlayers = _G.whitelistedPlayers or {}
_G.targetPlayer = _G.targetPlayer or ''

local function u418()
    local _LocalPlayer13 = game.Players.LocalPlayer

    if not _LocalPlayer13 then
        return nil
    end
    if _LocalPlayer13.Character and _LocalPlayer13.Character:FindFirstChild('HumanoidRootPart') then
        return _LocalPlayer13.Character
    else
        local v417 = tick()

        while true do
            task.wait(0.1)

            if tick() - v417 > 5 then
                return nil
            end
            if _LocalPlayer13.Character and _LocalPlayer13.Character:FindFirstChild('HumanoidRootPart') then
            end
        end
    end
end
local function u426()
    pcall(function()
        local _LocalPlayer14 = game.Players.LocalPlayer
        local _Character7 = _LocalPlayer14.Character

        if not (_Character7 and _Character7:FindFirstChild('Humanoid')) then
            return
        end

        local v421, v422, v423 = pairs(_LocalPlayer14.Backpack:GetChildren())
        local v424 = nil

        while true do
            local v425

            v423, v425 = v421(v422, v423)

            if v423 == nil then
                v425 = v424

                break
            end
            if v425.Name == 'Punch' then
                break
            end
        end

        if v425 then
            _Character7.Humanoid:EquipTool(v425)
            v425:Activate()
        end

        _LocalPlayer14.muscleEvent:FireServer('punch', 'leftHand')
        _LocalPlayer14.muscleEvent:FireServer('punch', 'rightHand')
    end)
end
local function u429(p427)
    local u428 = u418()

    if u428 then
        if p427.Character and p427.Character:FindFirstChild('HumanoidRootPart') then
            if u428:FindFirstChild('LeftHand') then
                pcall(function()
                    firetouchinterest(p427.Character.HumanoidRootPart, u428.LeftHand, 0)
                    task.wait(0.01)
                    firetouchinterest(p427.Character.HumanoidRootPart, u428.LeftHand, 1)
                    u426()
                end)
            end
        else
            return
        end
    else
        return
    end
end
local function u445(p430)
    if p430 and p430 ~= '' then
        local v431 = p430:lower()
        local v432, v433, v434 = pairs(game.Players:GetPlayers())
        local v435 = 0
        local v436 = nil

        while true do
            local v437

            v434, v437 = v432(v433, v434)

            if v434 == nil then
                break
            end
            if v437 ~= game.Players.LocalPlayer then
                local v438 = v437.Name:lower()
                local v439 = v437.DisplayName:lower()
                local v440 = v438:find(v431, 1, true) ~= nil
                local v441 = v439:find(v431, 1, true) ~= nil
                local v442 = 0
                local v443

                if v440 then
                    v443 = #v431 / #v438 * 100

                    if v438:sub(1, #v431) == v431 then
                        v443 = v443 + 50
                    end
                else
                    v443 = 0
                end
                if v441 then
                    v442 = #v431 / #v439 * 100

                    if v439:sub(1, #v431) == v431 then
                        v442 = v442 + 50
                    end
                end

                local v444 = math.max(v443, v442)

                if v435 < v444 then
                    v436 = v437
                    v435 = v444
                end
            end
        end

        if v435 > 20 then
            return v436
        else
            return nil
        end
    else
        return nil
    end
end

local _WhitelistJugadoresNone = _Killer:AddLabel('Whitelist Jugadores: None')
local _TargetPlayerNone = _Killer:AddLabel('Target Player: None')

local function u453()
    if #_G.whitelistedPlayers ~= 0 then
        local v448, v449, v450 = ipairs(_G.whitelistedPlayers)
        local v451 = 'Players on the White List: '

        while true do
            local v452

            v450, v452 = v448(v449, v450)

            if v450 == nil then
                break
            end
            if v450 > 1 then
                v451 = v451 .. ', '
            end

            v451 = v451 .. v452
        end

        _WhitelistJugadoresNone.Text = v451
    else
        _WhitelistJugadoresNone.Text = 'Whitelisted Players: None'
    end
end
local function v454()
    if _G.targetPlayer ~= '' then
        _TargetPlayerNone.Text = 'Jugador Objetivo: ' .. _G.targetPlayer
    else
        _TargetPlayerNone.Text = 'Blacklist: Ninguno'
    end
end

_Killer:AddSwitch('Agregar Amigos a WhiteList', function(p455)
    _G.autoWhitelistFriends = p455

    if p455 then
        pcall(function()
            local v456, v457, v458 = pairs(game.Players:GetPlayers())

            while true do
                local v459

                v458, v459 = v456(v457, v458)

                if v458 == nil then
                    break
                end
                if v459:IsFriendsWith(game.Players.LocalPlayer.UserId) then
                    local v460 = v459.Name .. ' (' .. v459.DisplayName .. ')'

                    if not table.find(_G.whitelistedPlayers, v460) then
                        table.insert(_G.whitelistedPlayers, v460)
                    end
                end
            end

            u453()
        end)
    end
end)
game.Players.PlayerAdded:Connect(function(p461)
    if _G.autoWhitelistFriends then
        pcall(function()
            if p461:IsFriendsWith(game.Players.LocalPlayer.UserId) then
                local v462 = p461.Name .. ' (' .. p461.DisplayName .. ')'

                if not table.find(_G.whitelistedPlayers, v462) then
                    table.insert(_G.whitelistedPlayers, v462)
                    u453()
                end
            end
        end)
    end
end)
_Killer:AddTextBox('Agregar Jugador a Whitelist (User/Nick)', function(p463)
    local v464 = p463 and p463 ~= '' and u445(p463)

    if v464 then
        local v465 = v464.Name .. ' (' .. v464.DisplayName .. ')'
        local v466, v467, v468 = ipairs(_G.whitelistedPlayers)
        local v469 = false

        while true do
            local v470

            v468, v470 = v466(v467, v468)

            if v468 == nil then
                break
            end
            if v470:find(v464.Name, 1, true) then
                v469 = true

                break
            end
        end

        if not v469 then
            table.insert(_G.whitelistedPlayers, v465)
            u453()
        end
    end
end)
_Killer:AddTextBox('Quitar Jugador de Whitelist (Name/Nickname)', function(p471)
    if p471 and p471 ~= '' then
        local v472 = p471:lower()
        local v473, v474, v475 = ipairs(_G.whitelistedPlayers)

        while true do
            local v476

            v475, v476 = v473(v474, v475)

            if v475 == nil then
                break
            end
            if v476:lower():find(v472, 1, true) then
                table.remove(_G.whitelistedPlayers, v475)
                u453()

                return
            end
        end

        local v477 = u445(p471)

        if v477 then
            local v478, v479, v480 = ipairs(_G.whitelistedPlayers)

            while true do
                local v481

                v480, v481 = v478(v479, v480)

                if v480 == nil then
                    break
                end
                if v481:find(v477.Name, 1, true) then
                    table.remove(_G.whitelistedPlayers, v480)
                    u453()

                    break
                end
            end
        end
    end
end)
_Killer:AddButton('Limpiar Whitelist', function()
    _G.whitelistedPlayers = {}

    u453()
end)
_Killer:AddSwitch('Automatically Remove Everyone (Except Whitelist)', function(p482)
    _G.autoKillAll = p482

    if p482 then
        spawn(function()
            while _G.autoKillAll do
                pcall(function()
                    local v483 = game:GetService('Players'):GetPlayers()
                    local v484, v485, v486 = ipairs(v483)

                    while true do
                        local u487

                        v486, u487 = v484(v485, v486)

                        if v486 == nil then
                            break
                        end

                        local u488 = false

                        local function v489()
                            u488 = true
                        end

                        if u487 ~= game.Players.LocalPlayer and _G.autoKillAll then
                            local v490, v491, v492 = ipairs(_G.whitelistedPlayers)
                            local v493 = false

                            while true do
                                local v494

                                v492, v494 = v490(v491, v492)

                                if v492 == nil then
                                    break
                                end
                                if v494:find(u487.Name, 1, true) then
                                    v489()

                                    v493 = true
                                end
                            end

                            if not v493 and u487.Character and u487.Character:FindFirstChild('HumanoidRootPart') and (u487.Character:FindFirstChild('Humanoid') and 0 < u487.Character.Humanoid.Health) then
                                pcall(function()
                                    u429(u487)
                                end)
                                task.wait(0.05)
                            end
                        end
                        if u488 then
                            break
                        end
                    end
                end)
                task.wait(0.2)
            end
        end)
    end
end)

local _Players3 = game:GetService('Players')
local _LocalPlayer15 = _Players3.LocalPlayer
local u497 = {}
local u498 = nil
local _SeleccionarJugadorParaVer = _Killer:AddDropdown('Seleccionar Jugador Para Ver', function(p499)
    local v500 = _Players3
    local v501, v502, v503 = ipairs(v500:GetPlayers())

    while true do
        local v504

        v503, v504 = v501(v502, v503)

        if v503 == nil then
            break
        end
        if string.format('%s (%s)', v504.DisplayName, v504.Name) == p499 then
            u498 = v504.Name

            break
        end
    end
end)
local v506 = _Players3
local v507, v508, v509 = ipairs(_Players3.GetPlayers(v506))
local u510 = u429
local u511 = _TargetPlayerNone
local v512 = u453
local u513 = u498
local u514 = _Players3

while true do
    local v515

    v509, v515 = v507(v508, v509)

    if v509 == nil then
        break
    end
    if v515 ~= _LocalPlayer15 then
        local _ss = string.format('%s (%s)', v515.DisplayName, v515.Name)

        _SeleccionarJugadorParaVer:Add(_ss)

        u497[v515.Name] = _ss
    end
end

u514.PlayerAdded:Connect(function(p517)
    if p517 ~= _LocalPlayer15 then
        local _ss2 = string.format('%s (%s)', p517.DisplayName, p517.Name)

        _SeleccionarJugadorParaVer:Add(_ss2)

        u497[p517.Name] = _ss2
    end
end)
u514.PlayerRemoving:Connect(function(p519)
    if p519 ~= _LocalPlayer15 then
        u497[p519.Name] = nil

        _SeleccionarJugadorParaVer:Clear()

        local v520, v521, v522 = pairs(u497)

        while true do
            local v523

            v522, v523 = v520(v521, v522)

            if v522 == nil then
                break
            end

            _SeleccionarJugadorParaVer:Add(v523)
        end
    end
end)
_Killer:AddSwitch('Ver Jugador', function(p524)
    spying = p524

    if spying then
        task.spawn(function()
            while spying do
                local v525 = u514:FindFirstChild(u513)

                if v525 and v525 ~= _LocalPlayer15 then
                    local _Character8 = v525.Character

                    if _Character8 then
                        _Character8 = v525.Character:FindFirstChild('Humanoid')
                    end
                    if _Character8 then
                        workspace.CurrentCamera.CameraSubject = _Character8
                    end
                end

                task.wait(0.1)
            end
        end)
    else
        workspace.CurrentCamera.CameraSubject = _LocalPlayer15.Character and _LocalPlayer15.Character:FindFirstChild('Humanoid') or _LocalPlayer15
    end
end)

_G.blacklistPlayers = _G.blacklistPlayers or {}

local function u527()
    if #_G.blacklistPlayers ~= 0 then
        u511.Text = 'Blacklist: ' .. table.concat(_G.blacklistPlayers, ', ')
    else
        u511.Text = 'Blacklist: Ninguno'
    end
end

local _AgregaraBlackList = _Killer:AddDropdown('Agregar a BlackList', function(p528)
    local _ss3, v530 = p528:match('(.+)%s|%s(.+)')

    if v530 then
        local v531 = u514
        local v532, v533, v534 = ipairs(v531:GetPlayers())

        while true do
            local v535

            v534, v535 = v532(v533, v534)

            if v534 == nil then
                break
            end
            if v535.Name == v530 then
                local v536 = v535.Name .. ' (' .. v535.DisplayName .. ')'

                if not table.find(_G.blacklistPlayers, v536) then
                    table.insert(_G.blacklistPlayers, v536)
                    game:GetService('StarterGui'):SetCore('SendNotification', {
                        Title = '\u{26ab} Blacklist',
                        Text = _ss3 .. ' a\u{f1}adido a la lista negra',
                        Duration = 3,
                    })
                    u527()
                end
            end
        end
    end
end)
local v538, v539, v540 = ipairs(u514:GetPlayers())
local u541 = u527

while true do
    local v542

    v540, v542 = v538(v539, v540)

    if v540 == nil then
        break
    end
    if v542 ~= _LocalPlayer15 then
        _AgregaraBlackList:Add(v542.DisplayName .. ' | ' .. v542.Name)
    end
end

u514.PlayerAdded:Connect(function(p543)
    if p543 ~= _LocalPlayer15 then
        _AgregaraBlackList:Add(p543.DisplayName .. ' | ' .. p543.Name)
    end
end)
u514.PlayerRemoving:Connect(function(_)
    _AgregaraBlackList:Clear()

    local v544 = u514
    local v545, v546, v547 = ipairs(v544:GetPlayers())

    while true do
        local v548

        v547, v548 = v545(v546, v547)

        if v547 == nil then
            break
        end
        if v548 ~= _LocalPlayer15 then
            _AgregaraBlackList:Add(v548.DisplayName .. ' | ' .. v548.Name)
        end
    end
end)
_Killer:AddButton('Quitar \u{da}ltimo de BlackList', function()
    if #_G.blacklistPlayers <= 0 then
        game:GetService('StarterGui'):SetCore('SendNotification', {
            Title = '\u{26aa} Blacklist',
            Text = 'No hay jugadores en la lista.',
            Duration = 3,
        })
    else
        local v549 = table.remove(_G.blacklistPlayers)

        game:GetService('StarterGui'):SetCore('SendNotification', {
            Title = '\u{26aa} Blacklist',
            Text = 'Eliminado: ' .. v549,
            Duration = 3,
        })
        u541()
    end
end)
_Killer:AddButton('Limpiar BlackList', function()
    _G.blacklistPlayers = {}

    u541()
    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = '\u{274c} Blacklist',
        Text = 'Lista negra vaciada',
        Duration = 3,
    })
end)
_Killer:AddSwitch('Matar BlackList', function(p550)
    _G.autoKillBlacklist = p550

    if p550 then
        task.spawn(function()
            while _G.autoKillBlacklist do
                pcall(function()
                    local v551, v552, v553 = ipairs(_G.blacklistPlayers)

                    while true do
                        local v554

                        v553, v554 = v551(v552, v553)

                        if v553 == nil then
                            break
                        end

                        local v555 = u514:FindFirstChild((v554:match('^(%S+)')))

                        if v555 and v555.Character and v555.Character:FindFirstChild('HumanoidRootPart') and (v555.Character:FindFirstChild('Humanoid') and 0 < v555.Character.Humanoid.Health) then
                            u510(v555)
                        end
                    end
                end)
                task.wait(0.1)
            end
        end)
    end
end)
_Killer:AddSwitch('Fast punch', function(p556)
    _G.autoPunchActive = p556

    if p556 then
        task.spawn(function()
            while _G.autoPunchActive do
                local _Punch4 = _LocalPlayer15.Backpack:FindFirstChild('Punch')

                if _Punch4 then
                    _Punch4.Parent = _LocalPlayer15.Character

                    if _Punch4:FindFirstChild('attackTime') then
                        _Punch4.attackTime.Value = 0
                    end
                end

                task.wait()
            end
        end)
        task.spawn(function()
            while _G.autoPunchActive do
                local _Character9 = _LocalPlayer15.Character

                if _Character9 then
                    _Character9 = _LocalPlayer15.Character:FindFirstChild('Punch')
                end
                if _Character9 then
                    _Character9:Activate()
                end

                task.wait()
            end
        end)
    else
        local _Character10 = _LocalPlayer15.Character

        if _Character10 then
            _Character10 = _LocalPlayer15.Character:FindFirstChild('Punch')
        end
        if _Character10 then
            _Character10.Parent = _LocalPlayer15.Backpack
        end
    end
end)
_Killer:AddSwitch('Auto Kill A Todos', function(p560)
    autoKill = p560

    task.spawn(function()
        while autoKill do
            local v561 = _LocalPlayer15.Character or _LocalPlayer15.CharacterAdded:Wait()
            local _RightHand = v561:FindFirstChild('RightHand')
            local _LeftHand = v561:FindFirstChild('LeftHand')
            local _Punch5 = _LocalPlayer15.Backpack:FindFirstChild('Punch')

            if _Punch5 and not v561:FindFirstChild('Punch') then
                _Punch5.Parent = v561
            end
            if _RightHand and _LeftHand then
                local v565 = u514
                local v566, v567, v568 = ipairs(v565:GetPlayers())

                while true do
                    local v569

                    v568, v569 = v566(v567, v568)

                    if v568 == nil then
                        break
                    end
                    if v569 ~= _LocalPlayer15 and not playerWhitelist[v569.Name] then
                        local _Character11 = v569.Character

                        if _Character11 then
                            _Character11 = _Character11:FindFirstChild('HumanoidRootPart')
                        end
                        if _Character11 then
                            pcall(function()
                                firetouchinterest(_RightHand, _Character11, 1)
                                firetouchinterest(_LeftHand, _Character11, 1)
                                firetouchinterest(_RightHand, _Character11, 0)
                                firetouchinterest(_LeftHand, _Character11, 0)
                            end)
                        end
                    end
                end
            end

            task.wait(0.05)
        end
    end)
end)
_Killer:AddButton('Quitar la animaci\u{f3}n al pu\u{f1}o', function()
    local u571 = {
        ['rbxassetid://3638729053'] = true,
        ['rbxassetid://3638767427'] = true,
    }

    local function u584()
        local _Character12 = game.Players.LocalPlayer.Character

        if _Character12 and _Character12:FindFirstChild('Humanoid') then
            local _Humanoid = _Character12:FindFirstChild('Humanoid')
            local v574, v575, v576 = pairs(_Humanoid:GetPlayingAnimationTracks())

            while true do
                local v577

                v576, v577 = v574(v575, v576)

                if v576 == nil then
                    break
                end
                if v577.Animation then
                    local _AnimationId = v577.Animation.AnimationId
                    local v579 = v577.Name:lower()

                    if u571[_AnimationId] or v579:match('punch') or (v579:match('attack') or v579:match('right')) then
                        v577:Stop()
                    end
                end
            end

            if not _G.AnimBlockConnection then
                local v583 = _Humanoid.AnimationPlayed:Connect(function(p580)
                    if p580.Animation then
                        local _AnimationId2 = p580.Animation.AnimationId
                        local v582 = p580.Name:lower()

                        if u571[_AnimationId2] or v582:match('punch') or (v582:match('attack') or v582:match('right')) then
                            p580:Stop()
                        end
                    end
                end)

                _G.AnimBlockConnection = v583
            end
        end
    end

    u584()

    local function u606()
        local function u594(p585)
            if p585 and (p585.Name == 'Punch' or p585.Name:match('Attack') or p585.Name:match('Right')) and not p585:GetAttribute('ActivatedOverride') then
                p585:SetAttribute('ActivatedOverride', true)

                local v593 = p585.Activated:Connect(function()
                    task.wait(0.05)

                    local _Character13 = game.Players.LocalPlayer.Character

                    if _Character13 and _Character13:FindFirstChild('Humanoid') then
                        local v587, v588, v589 = pairs(_Character13.Humanoid:GetPlayingAnimationTracks())

                        while true do
                            local v590

                            v589, v590 = v587(v588, v589)

                            if v589 == nil then
                                break
                            end
                            if v590.Animation then
                                local _AnimationId3 = v590.Animation.AnimationId
                                local v592 = v590.Name:lower()

                                if u571[_AnimationId3] or v592:match('punch') or (v592:match('attack') or v592:match('right')) then
                                    v590:Stop()
                                end
                            end
                        end
                    end
                end)

                if not _G.ToolConnections then
                    _G.ToolConnections = {}
                end

                _G.ToolConnections[p585] = v593
            end
        end

        local v595, v596, v597 = pairs(game.Players.LocalPlayer.Backpack:GetChildren())

        while true do
            local v598

            v597, v598 = v595(v596, v597)

            if v597 == nil then
                break
            end

            u594(v598)
        end

        local _Character14 = game.Players.LocalPlayer.Character

        if _Character14 then
            local v600, v601, v602 = pairs(_Character14:GetChildren())

            while true do
                local v603

                v602, v603 = v600(v601, v602)

                if v602 == nil then
                    break
                end
                if v603:IsA('Tool') then
                    u594(v603)
                end
            end
        end
        if not _G.BackpackAddedConnection then
            _G.BackpackAddedConnection = game.Players.LocalPlayer.Backpack.ChildAdded:Connect(function(p604)
                if p604:IsA('Tool') then
                    task.wait(0.1)
                    u594(p604)
                end
            end)
        end
        if not _G.CharacterToolAddedConnection and _Character14 then
            _G.CharacterToolAddedConnection = _Character14.ChildAdded:Connect(function(p605)
                if p605:IsA('Tool') then
                    task.wait(0.1)
                    u594(p605)
                end
            end)
        end
    end

    u606()

    if not _G.AnimMonitorConnection then
        _G.AnimMonitorConnection = game:GetService('RunService').Heartbeat:Connect(function()
            if tick() % 0.5 < 0.01 then
                local _Character15 = game.Players.LocalPlayer.Character

                if _Character15 and _Character15:FindFirstChild('Humanoid') then
                    local v608, v609, v610 = pairs(_Character15.Humanoid:GetPlayingAnimationTracks())

                    while true do
                        local v611

                        v610, v611 = v608(v609, v610)

                        if v610 == nil then
                            break
                        end
                        if v611.Animation then
                            local _AnimationId4 = v611.Animation.AnimationId
                            local v613 = v611.Name:lower()

                            if u571[_AnimationId4] or v613:match('punch') or (v613:match('attack') or v613:match('right')) then
                                v611:Stop()
                            end
                        end
                    end
                end
            end
        end)
    end
    if not _G.CharacterAddedConnection then
        _G.CharacterAddedConnection = game.Players.LocalPlayer.CharacterAdded:Connect(function(p614)
            task.wait(1)
            u584()
            u606()

            if _G.CharacterToolAddedConnection then
                _G.CharacterToolAddedConnection:Disconnect()
            end

            _G.CharacterToolAddedConnection = p614.ChildAdded:Connect(function(p615)
                if p615:IsA('Tool') then
                    task.wait(0.1)
                    processTool(p615)
                end
            end)
        end)
    end
end)

function RecoveryPunch()
    if _G.AnimBlockConnection then
        _G.AnimBlockConnection:Disconnect()

        _G.AnimBlockConnection = nil
    end
    if _G.AnimMonitorConnection then
        _G.AnimMonitorConnection:Disconnect()

        _G.AnimMonitorConnection = nil
    end
    if _G.ToolConnections then
        local v616, v617, v618 = pairs(_G.ToolConnections)

        while true do
            local v619

            v618, v619 = v616(v617, v618)

            if v618 == nil then
                break
            end
            if v619 then
                v619:Disconnect()
            end
        end

        _G.ToolConnections = nil
    end
    if _G.BackpackAddedConnection then
        _G.BackpackAddedConnection:Disconnect()

        _G.BackpackAddedConnection = nil
    end
    if _G.CharacterToolAddedConnection then
        _G.CharacterToolAddedConnection:Disconnect()

        _G.CharacterToolAddedConnection = nil
    end
    if _G.CharacterAddedConnection then
        _G.CharacterAddedConnection:Disconnect()

        _G.CharacterAddedConnection = nil
    end
end

_Killer:AddButton('Regresar La Animacion Al Pu\u{f1}o', function()
    RecoveryPunch()
end)
v512()
v454()

local u620 = false

_Killer:AddSwitch('God mode', function(p621)
    u620 = p621

    if p621 then
        task.spawn(function()
            while u620 do
                game:GetService('ReplicatedStorage').rEvents.brawlEvent:FireServer('joinBrawl')
                task.wait()
            end
        end)
    end
end)

local _Players4 = game:GetService('Players')
local _LocalPlayer16 = _Players4.LocalPlayer
local u624 = false
local u625 = nil

local function u632(p626)
    local _Character16 = _LocalPlayer16.Character
    local _Character17 = p626.Character

    if _Character16 and _Character17 then
        local _HumanoidRootPart2 = _Character16:FindFirstChild('HumanoidRootPart')
        local _HumanoidRootPart3 = _Character17:FindFirstChild('HumanoidRootPart')

        if _HumanoidRootPart2 and _HumanoidRootPart3 then
            local v631 = _HumanoidRootPart3.Position - _HumanoidRootPart3.CFrame.LookVector * 3

            _HumanoidRootPart2.CFrame = CFrame.new(v631, _HumanoidRootPart3.Position)
        end
    end
end

local _Teleportplayer = _Killer:AddDropdown('Teleport player', function(p633)
    if p633 and p633 ~= '' then
        local v634 = _Players4
        local v635, v636, v637 = ipairs(v634:GetPlayers())
        local v638 = nil

        while true do
            local v639

            v637, v639 = v635(v636, v637)

            if v637 == nil then
                v639 = v638

                break
            end
            if v639.DisplayName == p633 then
                break
            end
        end

        if v639 then
            u625 = v639.Name
            u624 = true

            print('\u{2705} Started following:', v639.Name)
            u632(v639)
        end
    end
end)
local v641 = _Players4
local v642, v643, v644 = ipairs(_Players4.GetPlayers(v641))
local u645 = u632
local u646 = u624
local u647 = u625
local u648 = _Players4
local u649 = _LocalPlayer16

while true do
    local v650, v651 = v642(v643, v644)

    if v650 == nil then
        break
    end

    v644 = v650

    if v651 ~= u649 then
        _Teleportplayer:Add(v651.DisplayName)
    end
end

u648.PlayerAdded:Connect(function(p652)
    if p652 ~= u649 then
        _Teleportplayer:Add(p652.DisplayName)
    end
end)
u648.PlayerRemoving:Connect(function(p653)
    _Teleportplayer:Clear()

    local v654 = u648
    local v655, v656, v657 = ipairs(v654:GetPlayers())

    while true do
        local v658

        v657, v658 = v655(v656, v657)

        if v657 == nil then
            break
        end
        if v658 ~= u649 then
            _Teleportplayer:Add(v658.DisplayName)
        end
    end

    if u647 == p653.Name then
        u647 = nil
        u646 = false
    end
end)
_Killer:AddButton('Dejar de Seguir', function()
    u646 = false
    u647 = nil

    print('\u{26d4} Stopped following')
end)
task.spawn(function()
    while task.wait(0.01) do
        if u646 and u647 then
            local v659 = u648:FindFirstChild(u647)

            if v659 then
                u645(v659)
            else
                u646 = false
                u647 = nil
            end
        end
    end
end)
u649.CharacterAdded:Connect(function()
    task.wait(1)

    local v660 = u646 and u647 and u648:FindFirstChild(u647)

    if v660 then
        u645(v660)
    end
end)

local u661 = false

_Killer:AddSwitch('Da\u{f1}o con God mode', function(p662)
    u661 = p662

    if p662 then
        task.spawn(function()
            while u661 do
                local v663 = u649
                local v664 = not v663.Backpack:FindFirstChild('Ground Slam') and v663.Character

                if v664 then
                    v664 = v663.Character:FindFirstChild('Ground Slam')
                end
                if v664 then
                    if v664.Parent == v663.Backpack then
                        v664.Parent = v663.Character
                    end
                    if v664:FindFirstChild('attackTime') then
                        v664.attackTime.Value = 0
                    end

                    v663.muscleEvent:FireServer('slam')
                    v664:Activate()
                end

                task.wait(0.1)
            end
        end)
    end
end)
_Killer:AddButton('Size NaN', function()
    local v665 = {
        'changeSize',
        0 / 0,
    }

    game:GetService('ReplicatedStorage'):WaitForChild('rEvents'):WaitForChild('changeSpeedSizeRemote'):InvokeServer(unpack(v665))
end)

local u666 = {
    'https://raw.githubusercontent.com/SadOz8/Stuffs/refs/heads/main/Crack',
    'https://raw.githubusercontent.com/SadOz8/Stuffs/refs/heads/main/Crack2',
    'https://raw.githubusercontent.com/SadOz8/Stuffs/refs/heads/main/Crack4',
    'https://raw.githubusercontent.com/SadOz8/Stuffs/refs/heads/main/Crack5',
    'https://raw.githubusercontent.com/SadOz8/Stuffs/refs/heads/main/Crack6',
}

_Killer:AddSwitch('Pegar Muerto', function()
    local v667, v668, v669 = ipairs(u666)

    while true do
        local u670

        v669, u670 = v667(v668, v669)

        if v669 == nil then
            break
        end

        spawn(function()
            local v671, u672 = pcall(function()
                return game:HttpGet(u670)
            end)

            if v671 and u672 then
                local v673, v674 = pcall(function()
                    loadstring(u672)()
                end)

                if not v673 then
                    warn('[Pegar Muerto] Error ejecutando raw:', u670, v674)
                end
            else
                warn('[Pegar Muerto] No se pudo cargar:', u670)
            end
        end)
    end
end)
_Killer:AddButton('Congelar el agua', function()
    local _Part = Instance.new('Part')

    _Part.Parent = Game.Workspace
    _Part.Anchored = true
    _Part.Position = Vector3.new(11, -9, 78)
    _Part.Size = Vector3.new(10000, 0, 10000)

    local _Part2 = Instance.new('Part')

    _Part2.Parent = Game.Workspace
    _Part2.Anchored = true
    _Part2.Position = Vector3.new(-110, -9, -999)
    _Part2.Size = Vector3.new(10000, 0, 10000)

    local _Part3 = Instance.new('Part')

    _Part3.Parent = Game.Workspace
    _Part3.Anchored = true
    _Part3.Position = Vector3.new(-110, -9, 1999)
    _Part3.Size = Vector3.new(10000, 0, 10000)

    local _Part4 = Instance.new('Part')

    _Part4.Parent = Game.Workspace
    _Part4.Anchored = true
    _Part4.Position = Vector3.new(-110, -9, -1999)
    _Part4.Size = Vector3.new(10000, 0, 10000)

    local _Part5 = Instance.new('Part')

    _Part5.Parent = Game.Workspace
    _Part5.Anchored = true
    _Part5.Position = Vector3.new(-110, -9, 2999)
    _Part5.Size = Vector3.new(10000, 0, 10000)

    local _Part6 = Instance.new('Part')

    _Part6.Parent = Game.Workspace
    _Part6.Anchored = true
    _Part6.Position = Vector3.new(-110, -9, -3999)
    _Part6.Size = Vector3.new(10000, 0, 10000)

    local _Part7 = Instance.new('Part')

    _Part7.Parent = Game.Workspace
    _Part7.Anchored = true
    _Part7.Position = Vector3.new(-110, -9, 4999)
    _Part7.Size = Vector3.new(10000, 0, 10000)

    local _Part8 = Instance.new('Part')

    _Part8.Parent = Game.Workspace
    _Part8.Anchored = true
    _Part8.Position = Vector3.new(-110, -9, -5999)
    _Part8.Size = Vector3.new(10000, 0, 10000)

    local _Part9 = Instance.new('Part')

    _Part9.Parent = Game.Workspace
    _Part9.Anchored = true
    _Part9.Position = Vector3.new(-110, -9, -6999)
    _Part9.Size = Vector3.new(10000, 0, 10000)

    local _Part10 = Instance.new('Part')

    _Part10.Parent = Game.Workspace
    _Part10.Anchored = true
    _Part10.Position = Vector3.new(-110, -9, -7999)
    _Part10.Size = Vector3.new(10000, 0, 10000)

    local _Part11 = Instance.new('Part')

    _Part11.Parent = Game.Workspace
    _Part11.Anchored = true
    _Part11.Position = Vector3.new(-110, -9, -8999)
    _Part11.Size = Vector3.new(10000, 0, 10000)

    local _Part12 = Instance.new('Part')

    _Part12.Parent = Game.Workspace
    _Part12.Anchored = true
    _Part12.Position = Vector3.new(-110, -9, -9999)
    _Part12.Size = Vector3.new(10000, 0, 10000)

    local _Part13 = Instance.new('Part')

    _Part13.Parent = Game.Workspace
    _Part13.Anchored = true
    _Part13.Position = Vector3.new(-110, -9, -10999)
    _Part13.Size = Vector3.new(10000, 0, 10000)

    local _Part14 = Instance.new('Part')

    _Part14.Parent = Game.Workspace
    _Part14.Anchored = true
    _Part14.Position = Vector3.new(-110, -9, -11999)
    _Part14.Size = Vector3.new(10000, 0, 10000)

    local _Part15 = Instance.new('Part')

    _Part15.Parent = Game.Workspace
    _Part15.Anchored = true
    _Part15.Position = Vector3.new(-110, -9, -12999)
    _Part15.Size = Vector3.new(10000, 0, 10000)

    local _Part16 = Instance.new('Part')

    _Part16.Parent = Game.Workspace
    _Part16.Anchored = true
    _Part16.Position = Vector3.new(-110, -9, -13999)
    _Part16.Size = Vector3.new(10000, 0, 10000)

    local _Part17 = Instance.new('Part')

    _Part17.Parent = Game.Workspace
    _Part17.Anchored = true
    _Part17.Position = Vector3.new(-110, -9, -14999)
    _Part17.Size = Vector3.new(10000, 0, 10000)

    local _Part18 = Instance.new('Part')

    _Part18.Parent = Game.Workspace
    _Part18.Anchored = true
    _Part18.Position = Vector3.new(-110, -9, -15999)
    _Part18.Size = Vector3.new(10000, 0, 10000)

    local _Part19 = Instance.new('Part')

    _Part19.Parent = Game.Workspace
    _Part19.Anchored = true
    _Part19.Position = Vector3.new(-110, -9, -16999)
    _Part19.Size = Vector3.new(10000, 0, 10000)

    local _Part20 = Instance.new('Part')

    _Part20.Parent = Game.Workspace
    _Part20.Anchored = true
    _Part20.Position = Vector3.new(-110, -9, -17999)
    _Part20.Size = Vector3.new(10000, 0, 10000)

    local _Part21 = Instance.new('Part')

    _Part21.Parent = Game.Workspace
    _Part21.Anchored = true
    _Part21.Position = Vector3.new(-110, -9, -18999)
    _Part21.Size = Vector3.new(10000, 0, 10000)

    local _Part22 = Instance.new('Part')

    _Part22.Parent = Game.Workspace
    _Part22.Anchored = true
    _Part22.Position = Vector3.new(-110, -9, -19999)
    _Part22.Size = Vector3.new(10000, 0, 10000)

    local _Part23 = Instance.new('Part')

    _Part23.Parent = Game.Workspace
    _Part23.Anchored = true
    _Part23.Position = Vector3.new(-110, -9, -20999)
    _Part23.Size = Vector3.new(10000, 0, 10000)

    local _Part24 = Instance.new('Part')

    _Part24.Parent = Game.Workspace
    _Part24.Anchored = true
    _Part24.Position = Vector3.new(-110, -9, -21999)
    _Part24.Size = Vector3.new(10000, 0, 10000)

    local _Part25 = Instance.new('Part')

    _Part25.Parent = Game.Workspace
    _Part25.Anchored = true
    _Part25.Position = Vector3.new(-110, -9, -22999)
    _Part25.Size = Vector3.new(10000, 0, 10000)

    local _Part26 = Instance.new('Part')

    _Part26.Parent = Game.Workspace
    _Part26.Anchored = true
    _Part26.Position = Vector3.new(-110, -9, -23999)
    _Part26.Size = Vector3.new(10000, 0, 10000)

    local _Part27 = Instance.new('Part')

    _Part27.Parent = Game.Workspace
    _Part27.Anchored = true
    _Part27.Position = Vector3.new(-8922.482421875, -9.848660469055176, -6233.65380859375)
    _Part27.Size = Vector3.new(20000, 0, 20000)

    local _Part28 = Instance.new('Part')

    _Part28.Parent = Game.Workspace
    _Part28.Anchored = true
    _Part28.Position = Vector3.new(-1107.2977294921875, -9.78486156463623, -4127.03369140625)
    _Part28.Size = Vector3.new(20000, 0, 20000)

    local _Part29 = Instance.new('Part')

    _Part29.Parent = Game.Workspace
    _Part29.Anchored = true
    _Part29.Position = Vector3.new(-2054.99365234375, -9.430279284715652, -5135.06201171875)
    _Part29.Size = Vector3.new(20000, 0, 20000)

    local _Part30 = Instance.new('Part')

    _Part30.Parent = Game.Workspace
    _Part30.Anchored = true
    _Part30.Position = Vector3.new(-3072.01806640625, -9.645418643951416, -6136.02392578125)
    _Part30.Size = Vector3.new(20000, 0, 20000)

    local _Part31 = Instance.new('Part')

    _Part31.Parent = Game.Workspace
    _Part31.Anchored = true
    _Part31.Position = Vector3.new(-4088.711181640625, -9.43027925491333, -5125.28076171875)
    _Part31.Size = Vector3.new(20000, 0, 20000)

    local _Part32 = Instance.new('Part')

    _Part32.Parent = Game.Workspace
    _Part32.Anchored = true
    _Part32.Position = Vector3.new(-5100.0576171875, -9.215141296386719, -5128.03369140625)
    _Part32.Size = Vector3.new(20000, 0, 20000)

    local _Part33 = Instance.new('Part')

    _Part33.Parent = Game.Workspace
    _Part33.Anchored = true
    _Part33.Position = Vector3.new(-6103.42626953125, -9.000000953674316, -5590.5859375)
    _Part33.Size = Vector3.new(20000, 0, 20000)

    local _Part34 = Instance.new('Part')

    _Part34.Parent = Game.Workspace
    _Part34.Anchored = true
    _Part34.Position = Vector3.new(-7096.3935546875, -9.784859657287598, -6403.66015625)
    _Part34.Size = Vector3.new(20000, 0, 20000)

    local _Part35 = Instance.new('Part')

    _Part35.Parent = Game.Workspace
    _Part35.Anchored = true
    _Part35.Position = Vector3.new(-9882.7021484375, -9.633520126342773, -5228.537109375)
    _Part35.Size = Vector3.new(20000, 0, 20000)

    local _Part36 = Instance.new('Part')

    _Part36.Parent = Game.Workspace
    _Part36.Anchored = true
    _Part36.Position = Vector3.new(-8889.23046875, -9.418381690979004, -4767.06689453125)
    _Part36.Size = Vector3.new(20000, 0, 20000)

    local _Part37 = Instance.new('Part')

    _Part37.Parent = Game.Workspace
    _Part37.Anchored = true
    _Part37.Position = Vector3.new(-7888.4443359375, -9.203240394592285, -4648.048828125)
    _Part37.Size = Vector3.new(20000, 0, 20000)

    local _Part38 = Instance.new('Part')

    _Part38.Parent = Game.Workspace
    _Part38.Anchored = true
    _Part38.Position = Vector3.new(-6868.951171875, -9.9881010055542, -4124.4384765625)
    _Part38.Size = Vector3.new(20000, 0, 20000)

    local _Part39 = Instance.new('Part')

    _Part39.Parent = Game.Workspace
    _Part39.Anchored = true
    _Part39.Position = Vector3.new(-5883.841796875, -9.772962093353271, -4073.54736328125)
    _Part39.Size = Vector3.new(20000, 0, 20000)

    local _Part40 = Instance.new('Part')

    _Part40.Parent = Game.Workspace
    _Part40.Anchored = true
    _Part40.Position = Vector3.new(-9873.0966796875, -9.26787185668945, -6775.55859375)
    _Part40.Size = Vector3.new(20000, 0, 20000)

    local _Part41 = Instance.new('Part')

    _Part41.Parent = Game.Workspace
    _Part41.Anchored = true
    _Part41.Position = Vector3.new(-8453.767578125, -9.26786804199219, -7212.89013671875)
    _Part41.Size = Vector3.new(20000, 0, 20000)

    local _Part42 = Instance.new('Part')

    _Part42.Parent = Game.Workspace
    _Part42.Anchored = true
    _Part42.Position = Vector3.new(-7482.234375, -9.84866714477539, -8082.72021484375)
    _Part42.Size = Vector3.new(20000, 0, 20000)

    local _Part43 = Instance.new('Part')

    _Part43.Parent = Game.Workspace
    _Part43.Anchored = true
    _Part43.Position = Vector3.new(-6483.5791015625, -9.26786422729492, -9055.9033203125)
    _Part43.Size = Vector3.new(20000, 0, 20000)

    local _Part44 = Instance.new('Part')

    _Part44.Parent = Game.Workspace
    _Part44.Anchored = true
    _Part44.Position = Vector3.new(-6561.2578125, -9.84867858886719, -10038.5146484375)
    _Part44.Size = Vector3.new(20000, 0, 20000)

    local _Part45 = Instance.new('Part')

    _Part45.Parent = Game.Workspace
    _Part45.Anchored = true
    _Part45.Position = Vector3.new(1009.69091796875, -9.11653518676758, 1046.963134765625)
    _Part45.Size = Vector3.new(20000, 0, 20000)

    local _Part46 = Instance.new('Part')

    _Part46.Parent = Game.Workspace
    _Part46.Anchored = true
    _Part46.Position = Vector3.new(2178.023681640625, -9.09156036376953, 287.1599426269531)
    _Part46.Size = Vector3.new(20000, 0, 20000)

    local _Part47 = Instance.new('Part')

    _Part47.Parent = Game.Workspace
    _Part47.Anchored = true
    _Part47.Position = Vector3.new(1023.4569091796875, -9.784860134124756, -478.5546569824219)
    _Part47.Size = Vector3.new(20000, 0, 20000)

    local _Part48 = Instance.new('Part')

    _Part48.Parent = Game.Workspace
    _Part48.Anchored = true
    _Part48.Position = Vector3.new(1416.96533203125, -9.901395559310913, 2066.506591796875)
    _Part48.Size = Vector3.new(20000, 0, 20000)

    local _Part49 = Instance.new('Part')

    _Part49.Parent = Game.Workspace
    _Part49.Anchored = true
    _Part49.Position = Vector3.new(1514.3587646484375, -9.686256408691406, 3077.935791015625)
    _Part49.Size = Vector3.new(20000, 0, 20000)

    local _Part50 = Instance.new('Part')

    _Part50.Parent = Game.Workspace
    _Part50.Anchored = true
    _Part50.Position = Vector3.new(1624.8602294921875, -9.47111701965332, 4074.02587890625)
    _Part50.Size = Vector3.new(20000, 0, 20000)

    local _Part51 = Instance.new('Part')

    _Part51.Parent = Game.Workspace
    _Part51.Anchored = true
    _Part51.Position = Vector3.new(1687.230712890625, -9.255976676940918, 5069.81640625)
    _Part51.Size = Vector3.new(20000, 0, 20000)

    local _Part52 = Instance.new('Part')

    _Part52.Parent = Game.Workspace
    _Part52.Anchored = true
    _Part52.Position = Vector3.new(1655.4146728515625, -9.040838241577148, 6056.37451171875)
    _Part52.Size = Vector3.new(20000, 0, 20000)

    local _Part53 = Instance.new('Part')

    _Part53.Parent = Game.Workspace
    _Part53.Anchored = true
    _Part53.Position = Vector3.new(650.4426879882813, -9.825699090957642, 6532.1982421875)
    _Part53.Size = Vector3.new(20000, 0, 20000)

    local _Part54 = Instance.new('Part')

    _Part54.Parent = Game.Workspace
    _Part54.Anchored = true
    _Part54.Position = Vector3.new(-347.35028076171875, -9.61055850982666, 6912.1591796875)
    _Part54.Size = Vector3.new(20000, 0, 20000)

    local _Part55 = Instance.new('Part')

    _Part55.Parent = Game.Workspace
    _Part55.Anchored = true
    _Part55.Position = Vector3.new(218.79769897460938, -9.39542007446289, 7934.68017578125)
    _Part55.Size = Vector3.new(20000, 0, 20000)

    local _Part56 = Instance.new('Part')

    _Part56.Parent = Game.Workspace
    _Part56.Anchored = true
    _Part56.Position = Vector3.new(251.1740264892578, -9.180282592773438, 8951.5595703125)
    _Part56.Size = Vector3.new(20000, 0, 20000)

    local _Part57 = Instance.new('Part')

    _Part57.Parent = Game.Workspace
    _Part57.Anchored = true
    _Part57.Position = Vector3.new(360.1578369140625, -9.965142488479614, 9967.0107421875)
    _Part57.Size = Vector3.new(20000, 0, 20000)

    local _Part58 = Instance.new('Part')

    _Part58.Parent = Game.Workspace
    _Part58.Anchored = true
    _Part58.Position = Vector3.new(488.1932067871094, -9.750001907348633, 10972.189453125)
    _Part58.Size = Vector3.new(20000, 0, 20000)

    local _Part59 = Instance.new('Part')

    _Part59.Parent = Game.Workspace
    _Part59.Anchored = true
    _Part59.Position = Vector3.new(569.8875732421875, -9.53486156463623, 11982.7353515625)
    _Part59.Size = Vector3.new(20000, 0, 20000)

    local _Part60 = Instance.new('Part')

    _Part60.Parent = Game.Workspace
    _Part60.Anchored = true
    _Part60.Position = Vector3.new(629.5374755859375, -9.319723129272461, 12985.849609375)
    _Part60.Size = Vector3.new(20000, 0, 20000)

    local _Part61 = Instance.new('Part')

    _Part61.Parent = Game.Workspace
    _Part61.Anchored = true
    _Part61.Position = Vector3.new(1664.9771728515625, -9.610558986663818, 7491.61181640625)
    _Part61.Size = Vector3.new(20000, 0, 20000)

    local _Part62 = Instance.new('Part')

    _Part62.Parent = Game.Workspace
    _Part62.Anchored = true
    _Part62.Position = Vector3.new(1945.2432861328125, -9.395420551300049, 8507.6533203125)
    _Part62.Size = Vector3.new(20000, 0, 20000)

    local _Part63 = Instance.new('Part')

    _Part63.Parent = Game.Workspace
    _Part63.Anchored = true
    _Part63.Position = Vector3.new(904.4902954101563, -9.784861087799072, -2151.783447265625)
    _Part63.Size = Vector3.new(20000, 0, 20000)

    local _Part64 = Instance.new('Part')

    _Part64.Parent = Game.Workspace
    _Part64.Anchored = true
    _Part64.Position = Vector3.new(1890.2633056640625, -9.569720268249512, -2413.07421875)
    _Part64.Size = Vector3.new(20000, 0, 20000)

    local _Part65 = Instance.new('Part')

    _Part65.Parent = Game.Workspace
    _Part65.Anchored = true
    _Part65.Position = Vector3.new(2895.560302734375, -9.354581356048584, -2546.12109375)
    _Part65.Size = Vector3.new(20000, 0, 20000)

    local _Part66 = Instance.new('Part')

    _Part66.Parent = Game.Workspace
    _Part66.Anchored = true
    _Part66.Position = Vector3.new(3881.858642578125, -9.139442920684814, -2962.385986328125)
    _Part66.Size = Vector3.new(20000, 0, 20000)

    local _Part67 = Instance.new('Part')

    _Part67.Parent = Game.Workspace
    _Part67.Anchored = true
    _Part67.Position = Vector3.new(4874.54833984375, -9.924301624298096, -3218.8798828125)
    _Part67.Size = Vector3.new(20000, 0, 20000)

    local _Part68 = Instance.new('Part')

    _Part68.Parent = Game.Workspace
    _Part68.Anchored = true
    _Part68.Position = Vector3.new(5892.513671875, -9.709161758422852, -3338.171875)
    _Part68.Size = Vector3.new(20000, 0, 20000)

    local _Part69 = Instance.new('Part')

    _Part69.Parent = Game.Workspace
    _Part69.Anchored = true
    _Part69.Position = Vector3.new(6886.39990234375, -9.505979061126709, -3488.1533203125)
    _Part69.Size = Vector3.new(20000, 0, 20000)

    local _Part70 = Instance.new('Part')

    _Part70.Parent = Game.Workspace
    _Part70.Anchored = true
    _Part70.Position = Vector3.new(7863.33056640625, -9.290840148925781, -3597.61279296875)
    _Part70.Size = Vector3.new(20000, 0, 20000)

    local _Part71 = Instance.new('Part')

    _Part71.Parent = Game.Workspace
    _Part71.Anchored = true
    _Part71.Position = Vector3.new(8840.2548828125, -9.075700759887695, -3711.208740234375)
    _Part71.Size = Vector3.new(20000, 0, 20000)

    local _Part72 = Instance.new('Part')

    _Part72.Parent = Game.Workspace
    _Part72.Anchored = true
    _Part72.Position = Vector3.new(9838.98046875, -9.860561609268188, -3811.1064453125)
    _Part72.Size = Vector3.new(20000, 0, 20000)

    local _Part73 = Instance.new('Part')

    _Part73.Parent = Game.Workspace
    _Part73.Anchored = true
    _Part73.Position = Vector3.new(10812.9287109375, -9.645423412322998, -3987.937744140625)
    _Part73.Size = Vector3.new(20000, 0, 20000)

    local _Part74 = Instance.new('Part')

    _Part74.Parent = Game.Workspace
    _Part74.Anchored = true
    _Part74.Position = Vector3.new(11798.421875, -9.430284976959229, -4059.906494140625)
    _Part74.Size = Vector3.new(20000, 0, 20000)

    local _Part75 = Instance.new('Part')

    _Part75.Parent = Game.Workspace
    _Part75.Anchored = true
    _Part75.Position = Vector3.new(12794.8408203125, -9.215145587921143, -4103.353515625)
    _Part75.Size = Vector3.new(20000, 0, 20000)

    local _Part76 = Instance.new('Part')

    _Part76.Parent = Game.Workspace
    _Part76.Anchored = true
    _Part76.Position = Vector3.new(13782.353515625, -9.000005722045898, -4253.642578125)
    _Part76.Size = Vector3.new(20000, 0, 20000)

    local _Part77 = Instance.new('Part')

    _Part77.Parent = Game.Workspace
    _Part77.Anchored = true
    _Part77.Position = Vector3.new(14800.7666015625, -9.784866333007813, -4368.7177734375)
    _Part77.Size = Vector3.new(20000, 0, 20000)

    local _Part78 = Instance.new('Part')

    _Part78.Parent = Game.Workspace
    _Part78.Anchored = true
    _Part78.Position = Vector3.new(15813.6171875, -9.569726467132568, -4453.31591796875)
    _Part78.Size = Vector3.new(20000, 0, 20000)

    local _Part79 = Instance.new('Part')

    _Part79.Parent = Game.Workspace
    _Part79.Anchored = true
    _Part79.Position = Vector3.new(16818.390625, -9.354587078094482, -4569.81396484375)
    _Part79.Size = Vector3.new(20000, 0, 20000)

    local _Part80 = Instance.new('Part')

    _Part80.Parent = Game.Workspace
    _Part80.Anchored = true
    _Part80.Position = Vector3.new(17821.248046875, -9.139448642730713, -4737.97509765625)
    _Part80.Size = Vector3.new(20000, 0, 20000)

    local _Part81 = Instance.new('Part')

    _Part81.Parent = Game.Workspace
    _Part81.Anchored = true
    _Part81.Position = Vector3.new(18001.896484375, -9.924308776855469, -3764.74609375)
    _Part81.Size = Vector3.new(20000, 0, 20000)

    local _Part82 = Instance.new('Part')

    _Part82.Parent = Game.Workspace
    _Part82.Anchored = true
    _Part82.Position = Vector3.new(17008.357421875, -9.7091703414917, -2794.70751953125)
    _Part82.Size = Vector3.new(20000, 0, 20000)

    local _Part83 = Instance.new('Part')

    _Part83.Parent = Game.Workspace
    _Part83.Anchored = true
    _Part83.Position = Vector3.new(16014.57421875, -9.49403190612793, -2500.1083984375)
    _Part83.Size = Vector3.new(20000, 0, 20000)

    local _Part84 = Instance.new('Part')

    _Part84.Parent = Game.Workspace
    _Part84.Anchored = true
    _Part84.Position = Vector3.new(18766.751953125, -9.709169387817383, -2759.8818359375)
    _Part84.Size = Vector3.new(20000, 0, 20000)

    local _Part85 = Instance.new('Part')

    _Part85.Parent = Game.Workspace
    _Part85.Anchored = true
    _Part85.Position = Vector3.new(19039.708984375, -9.494030475616455, -3743.810546875)
    _Part85.Size = Vector3.new(20000, 0, 20000)

    local _Part86 = Instance.new('Part')

    _Part86.Parent = Game.Workspace
    _Part86.Anchored = true
    _Part86.Position = Vector3.new(18948.62890625, -9.494030475616455, -1756.02783203125)
    _Part86.Size = Vector3.new(20000, 0, 20000)

    local _Part87 = Instance.new('Part')

    _Part87.Parent = Game.Workspace
    _Part87.Anchored = true
    _Part87.Position = Vector3.new(17940.4921875, -9.278892040252686, -755.4782104492188)
    _Part87.Size = Vector3.new(20000, 0, 20000)

    local _Part88 = Instance.new('Part')

    _Part88.Parent = Game.Workspace
    _Part88.Anchored = true
    _Part88.Position = Vector3.new(18944.34375, -9.063753604888916, -543.2788696289063)
    _Part88.Size = Vector3.new(20000, 0, 20000)

    local _Part89 = Instance.new('Part')

    _Part89.Parent = Game.Workspace
    _Part89.Anchored = true
    _Part89.Position = Vector3.new(16963.00390625, -9.063752174377441, -293.4074401855469)
    _Part89.Size = Vector3.new(20000, 0, 20000)

    local _Part90 = Instance.new('Part')

    _Part90.Parent = Game.Workspace
    _Part90.Anchored = true
    _Part90.Position = Vector3.new(15998.7353515625, -9.848613023757935, -1311.015869140625)
    _Part90.Size = Vector3.new(20000, 0, 20000)

    local _Part91 = Instance.new('Part')

    _Part91.Parent = Game.Workspace
    _Part91.Anchored = true
    _Part91.Position = Vector3.new(15591.837890625, -9.633473873138428, -296.08660888671875)
    _Part91.Size = Vector3.new(20000, 0, 20000)

    local _Part92 = Instance.new('Part')

    _Part92.Parent = Game.Workspace
    _Part92.Anchored = true
    _Part92.Position = Vector3.new(14579.58203125, -9.4183349609375, -991.3533935546875)
    _Part92.Size = Vector3.new(20000, 0, 20000)

    local _Part93 = Instance.new('Part')

    _Part93.Parent = Game.Workspace
    _Part93.Anchored = true
    _Part93.Position = Vector3.new(14294.275390625, -9.203196048736572, -221.879150390625)
    _Part93.Size = Vector3.new(20000, 0, 20000)

    local _Part94 = Instance.new('Part')

    _Part94.Parent = Game.Workspace
    _Part94.Anchored = true
    _Part94.Position = Vector3.new(13372.7236328125, -9.988056659698486, 73.62410736083984)
    _Part94.Size = Vector3.new(20000, 0, 20000)

    local _Part95 = Instance.new('Part')

    _Part95.Parent = Game.Workspace
    _Part95.Anchored = true
    _Part95.Position = Vector3.new(12783.056640625, -9.093517303466797, -952.5441284179688)
    _Part95.Size = Vector3.new(20000, 0, 20000)

    local _Part96 = Instance.new('Part')

    _Part96.Parent = Game.Workspace
    _Part96.Anchored = true
    _Part96.Position = Vector3.new(2003.5675048828125, -9.569721221923828, -1103.451416015625)
    _Part96.Size = Vector3.new(20000, 0, 20000)

    local _Part97 = Instance.new('Part')

    _Part97.Parent = Game.Workspace
    _Part97.Anchored = true
    _Part97.Position = Vector3.new(2978.824951171875, -9.354583263397217, -1012.9195556640625)
    _Part97.Size = Vector3.new(20000, 0, 20000)

    local _Part98 = Instance.new('Part')

    _Part98.Parent = Game.Workspace
    _Part98.Anchored = true
    _Part98.Position = Vector3.new(3944.885986328125, -9.139444351196289, -958.9783935546875)
    _Part98.Size = Vector3.new(20000, 0, 20000)

    local _Part99 = Instance.new('Part')

    _Part99.Parent = Game.Workspace
    _Part99.Anchored = true
    _Part99.Position = Vector3.new(4917.93212890625, -9.924305438995361, -1106.58837890625)
    _Part99.Size = Vector3.new(20000, 0, 20000)

    local _Part100 = Instance.new('Part')

    _Part100.Parent = Game.Workspace
    _Part100.Anchored = true
    _Part100.Position = Vector3.new(5321.544921875, -9.709166526794434, -1851.941162109375)
    _Part100.Size = Vector3.new(20000, 0, 20000)

    local _Part101 = Instance.new('Part')

    _Part101.Parent = Game.Workspace
    _Part101.Anchored = true
    _Part101.Position = Vector3.new(2894.729736328125, -9.92430329322815, -3897.88720703125)
    _Part101.Size = Vector3.new(20000, 0, 20000)

    local _Part102 = Instance.new('Part')

    _Part102.Parent = Game.Workspace
    _Part102.Anchored = true
    _Part102.Position = Vector3.new(1880.724609375, -9.709164142608643, -4258.2509765625)
    _Part102.Size = Vector3.new(20000, 0, 20000)

    local _Part103 = Instance.new('Part')

    _Part103.Parent = Game.Workspace
    _Part103.Anchored = true
    _Part103.Position = Vector3.new(-1643.215576171875, -9.569722652435303, -3132.36572265625)
    _Part103.Size = Vector3.new(20000, 0, 20000)

    local _Part104 = Instance.new('Part')

    _Part104.Parent = Game.Workspace
    _Part104.Anchored = true
    _Part104.Position = Vector3.new(-1772.5018310546875, -9.354584217071533, -2139.459716796875)
    _Part104.Size = Vector3.new(20000, 0, 20000)

    local _Part105 = Instance.new('Part')

    _Part105.Parent = Game.Workspace
    _Part105.Anchored = true
    _Part105.Position = Vector3.new(-1665.8314208984375, -9.139444828033447, -1128.0208740234375)
    _Part105.Size = Vector3.new(20000, 0, 20000)

    local _Part106 = Instance.new('Part')

    _Part106.Parent = Game.Workspace
    _Part106.Anchored = true
    _Part106.Position = Vector3.new(-1673.664794921875, -9.924305438995361, -176.67430114746094)
    _Part106.Size = Vector3.new(20000, 0, 20000)

    local _Part107 = Instance.new('Part')

    _Part107.Parent = Game.Workspace
    _Part107.Anchored = true
    _Part107.Position = Vector3.new(-1895.2357177734375, -9.709166526794434, 789.939697265625)
    _Part107.Size = Vector3.new(20000, 0, 20000)

    local _Part108 = Instance.new('Part')

    _Part108.Parent = Game.Workspace
    _Part108.Anchored = true
    _Part108.Position = Vector3.new(-1991.6993408203125, -9.494027614593506, 1789.4617919921875)
    _Part108.Size = Vector3.new(20000, 0, 20000)

    local _Part109 = Instance.new('Part')

    _Part109.Parent = Game.Workspace
    _Part109.Anchored = true
    _Part109.Position = Vector3.new(-2148.386962890625, -9.278889179229736, 2777.50146484375)
    _Part109.Size = Vector3.new(20000, 0, 20000)

    local _Part110 = Instance.new('Part')

    _Part110.Parent = Game.Workspace
    _Part110.Anchored = true
    _Part110.Position = Vector3.new(-2334.718505859375, -9.06374979019165, 3752.33056640625)
    _Part110.Size = Vector3.new(20000, 0, 20000)

    local _Part111 = Instance.new('Part')

    _Part111.Parent = Game.Workspace
    _Part111.Anchored = true
    _Part111.Position = Vector3.new(-2430.8857421875, -9.848610401153564, 4750.89208984375)
    _Part111.Size = Vector3.new(20000, 0, 20000)

    local _Part112 = Instance.new('Part')

    _Part112.Parent = Game.Workspace
    _Part112.Anchored = true
    _Part112.Position = Vector3.new(-2477.38427734375, -9.633471488952637, 5754.65625)
    _Part112.Size = Vector3.new(20000, 0, 20000)

    local _Part113 = Instance.new('Part')

    _Part113.Parent = Game.Workspace
    _Part113.Anchored = true
    _Part113.Position = Vector3.new(-1474.8240966796875, -9.418332576751709, 4993.744140625)
    _Part113.Size = Vector3.new(20000, 0, 20000)

    local _Part114 = Instance.new('Part')

    _Part114.Parent = Game.Workspace
    _Part114.Anchored = true
    _Part114.Position = Vector3.new(-1239.12744140625, -9.203193664550781, 4206.31005859375)
    _Part114.Size = Vector3.new(20000, 0, 20000)

    local _Part115 = Instance.new('Part')

    _Part115.Parent = Game.Workspace
    _Part115.Anchored = true
    _Part115.Position = Vector3.new(-1682.2164306640625, -9.418331623077393, 6633.03125)
    _Part115.Size = Vector3.new(20000, 0, 20000)

    local _Part116 = Instance.new('Part')

    _Part116.Parent = Game.Workspace
    _Part116.Anchored = true
    _Part116.Position = Vector3.new(-1427.5582275390625, -9.203193187713623, 7548.80224609375)
    _Part116.Size = Vector3.new(20000, 0, 20000)

    local _Part117 = Instance.new('Part')

    _Part117.Parent = Game.Workspace
    _Part117.Anchored = true
    _Part117.Position = Vector3.new(-3823.29248046875, -9.063750267028809, 1757.0218505859375)
    _Part117.Size = Vector3.new(20000, 0, 20000)

    local _Part118 = Instance.new('Part')

    _Part118.Parent = Game.Workspace
    _Part118.Anchored = true
    _Part118.Position = Vector3.new(-4112.26171875, -9.84861135482788, 2757.71484375)
    _Part118.Size = Vector3.new(20000, 0, 20000)

    local _Part119 = Instance.new('Part')

    _Part119.Parent = Game.Workspace
    _Part119.Anchored = true
    _Part119.Position = Vector3.new(-4299.38330078125, -9.633472919464111, 3713.58056640625)
    _Part119.Size = Vector3.new(20000, 0, 20000)

    local _Part120 = Instance.new('Part')

    _Part120.Parent = Game.Workspace
    _Part120.Anchored = true
    _Part120.Position = Vector3.new(-4433.6865234375, -9.4183349609375, 4715.2236328125)
    _Part120.Size = Vector3.new(20000, 0, 20000)

    local _Part121 = Instance.new('Part')

    _Part121.Parent = Game.Workspace
    _Part121.Anchored = true
    _Part121.Position = Vector3.new(-4646.71728515625, -9.203195571899414, 5683.84765625)
    _Part121.Size = Vector3.new(20000, 0, 20000)

    local _Part122 = Instance.new('Part')

    _Part122.Parent = Game.Workspace
    _Part122.Anchored = true
    _Part122.Position = Vector3.new(-3911.37353515625, -9.988056182861328, 6502.482421875)
    _Part122.Size = Vector3.new(20000, 0, 20000)

    local _Part123 = Instance.new('Part')

    _Part123.Parent = Game.Workspace
    _Part123.Anchored = true
    _Part123.Position = Vector3.new(-3161.617431640625, -9.772918224334717, 6943.85986328125)
    _Part123.Size = Vector3.new(20000, 0, 20000)

    local _Part124 = Instance.new('Part')

    _Part124.Parent = Game.Workspace
    _Part124.Anchored = true
    _Part124.Position = Vector3.new(-2052.14599609375, -9.988054275512695, 8546.2861328125)
    _Part124.Size = Vector3.new(20000, 0, 20000)

    local _Part125 = Instance.new('Part')

    _Part125.Parent = Game.Workspace
    _Part125.Anchored = true
    _Part125.Position = Vector3.new(-1400.4658203125, -9.772915363311768, 9297.806640625)
    _Part125.Size = Vector3.new(20000, 0, 20000)

    local _Part126 = Instance.new('Part')

    _Part126.Parent = Game.Workspace
    _Part126.Anchored = true
    _Part126.Position = Vector3.new(-1190.1282958984375, -9.44222542643547, 10259.5078125)
    _Part126.Size = Vector3.new(20000, 0, 20000)

    local _Part127 = Instance.new('Part')

    _Part127.Parent = Game.Workspace
    _Part127.Anchored = true
    _Part127.Position = Vector3.new(2187.53564453125, -9.180280685424805, 9414.32421875)
    _Part127.Size = Vector3.new(20000, 0, 20000)

    local _Part128 = Instance.new('Part')

    _Part128.Parent = Game.Workspace
    _Part128.Anchored = true
    _Part128.Position = Vector3.new(2896.7841796875, -9.180281639099121, 7566.1171875)
    _Part128.Size = Vector3.new(20000, 0, 20000)

    local _Part129 = Instance.new('Part')

    _Part129.Parent = Game.Workspace
    _Part129.Anchored = true
    _Part129.Position = Vector3.new(3187.329345703125, -9.965142488479614, 6555.640625)
    _Part129.Size = Vector3.new(20000, 0, 20000)

    local _Part130 = Instance.new('Part')

    _Part130.Parent = Game.Workspace
    _Part130.Anchored = true
    _Part130.Position = Vector3.new(3379.880615234375, -9.750005722045898, 5558.48876953125)
    _Part130.Size = Vector3.new(20000, 0, 20000)

    local _Part131 = Instance.new('Part')

    _Part131.Parent = Game.Workspace
    _Part131.Anchored = true
    _Part131.Position = Vector3.new(3961.218017578125, -9.75402545928955, 6907.60400390625)
    _Part131.Size = Vector3.new(20000, 0, 20000)

    local _Part132 = Instance.new('Part')

    _Part132.Parent = Game.Workspace
    _Part132.Anchored = true
    _Part132.Position = Vector3.new(3976.142822265625, -9.538886547088623, 7871.6318359375)
    _Part132.Size = Vector3.new(20000, 0, 20000)

    local _Part133 = Instance.new('Part')

    _Part133.Parent = Game.Workspace
    _Part133.Anchored = true
    _Part133.Position = Vector3.new(-3126.83544921875, -9.84867262840271, 802.1668090820313)
    _Part133.Size = Vector3.new(20000, 0, 20000)

    local _Part134 = Instance.new('Part')

    _Part134.Parent = Game.Workspace
    _Part134.Anchored = true
    _Part134.Position = Vector3.new(-3662.20703125, -9.633533477783203, -80.66658020019531)
    _Part134.Size = Vector3.new(20000, 0, 20000)

    local _Part135 = Instance.new('Part')

    _Part135.Parent = Game.Workspace
    _Part135.Anchored = true
    _Part135.Position = Vector3.new(-3609.484130859375, -9.418395519256592, -1017.3029174804688)
    _Part135.Size = Vector3.new(20000, 0, 20000)

    local _Part136 = Instance.new('Part')

    _Part136.Parent = Game.Workspace
    _Part136.Anchored = true
    _Part136.Position = Vector3.new(-3753.300537109375, -9.20325517654419, -1995.7613525390625)
    _Part136.Size = Vector3.new(20000, 0, 20000)

    local _Part137 = Instance.new('Part')

    _Part137.Parent = Game.Workspace
    _Part137.Anchored = true
    _Part137.Position = Vector3.new(-4743.466796875, -9.988116025924683, -2393.3125)
    _Part137.Size = Vector3.new(20000, 0, 20000)

    local _Part138 = Instance.new('Part')

    _Part138.Parent = Game.Workspace
    _Part138.Anchored = true
    _Part138.Position = Vector3.new(-5736.89892578125, -9.772977828979492, -2010.5301513671875)
    _Part138.Size = Vector3.new(20000, 0, 20000)

    local _Part139 = Instance.new('Part')

    _Part139.Parent = Game.Workspace
    _Part139.Anchored = true
    _Part139.Position = Vector3.new(-4006.782958984375, -9.772977352142334, -3332.644287109375)
    _Part139.Size = Vector3.new(20000, 0, 20000)

    local _Part140 = Instance.new('Part')

    _Part140.Parent = Game.Workspace
    _Part140.Anchored = true
    _Part140.Position = Vector3.new(-3065.00146484375, -9.557837963104248, -3415.263916015625)
    _Part140.Size = Vector3.new(20000, 0, 20000)

    local _Part141 = Instance.new('Part')

    _Part141.Parent = Game.Workspace
    _Part141.Anchored = true
    _Part141.Position = Vector3.new(-4635.92431640625, -9.418395519256592, -103.13423919677734)
    _Part141.Size = Vector3.new(20000, 0, 20000)

    local _Part142 = Instance.new('Part')

    _Part142.Parent = Game.Workspace
    _Part142.Anchored = true
    _Part142.Position = Vector3.new(-5439.66796875, -9.203256607055664, 866.9227294921875)
    _Part142.Size = Vector3.new(20000, 0, 20000)

    local _Part143 = Instance.new('Part')

    _Part143.Parent = Game.Workspace
    _Part143.Anchored = true
    _Part143.Position = Vector3.new(-5770.49462890625, -9.988117933273315, 1845.7347412109375)
    _Part143.Size = Vector3.new(20000, 0, 20000)

    local _Part144 = Instance.new('Part')

    _Part144.Parent = Game.Workspace
    _Part144.Anchored = true
    _Part144.Position = Vector3.new(-5798.58203125, -9.772978782653809, 2813.786376953125)
    _Part144.Size = Vector3.new(20000, 0, 20000)

    local _Part145 = Instance.new('Part')

    _Part145.Parent = Game.Workspace
    _Part145.Anchored = true
    _Part145.Position = Vector3.new(-6413.759765625, -9.988118171691895, 45.978721618652344)
    _Part145.Size = Vector3.new(20000, 0, 20000)

    local _Part146 = Instance.new('Part')

    _Part146.Parent = Game.Workspace
    _Part146.Anchored = true
    _Part146.Position = Vector3.new(-7281.08837890625, -9.342700004577637, -2053.28173828125)
    _Part146.Size = Vector3.new(20000, 0, 20000)

    local _Part147 = Instance.new('Part')

    _Part147.Parent = Game.Workspace
    _Part147.Anchored = true
    _Part147.Position = Vector3.new(-5939.85009765625, -9.557838439941406, -2980.478515625)
    _Part147.Size = Vector3.new(20000, 0, 20000)

    local _Part148 = Instance.new('Part')

    _Part148.Parent = Game.Workspace
    _Part148.Anchored = true
    _Part148.Position = Vector3.new(3151.140380859375, -9.876421451568604, 645.57568359375)
    _Part148.Size = Vector3.new(20000, 0, 20000)

    local _Part149 = Instance.new('Part')

    _Part149.Parent = Game.Workspace
    _Part149.Anchored = true
    _Part149.Position = Vector3.new(4175.984375, -9.661282062530518, 688.4006958007813)
    _Part149.Size = Vector3.new(20000, 0, 20000)

    local _Part150 = Instance.new('Part')

    _Part150.Parent = Game.Workspace
    _Part150.Anchored = true
    _Part150.Position = Vector3.new(3677.688232421875, -9.44614315032959, 1697.672607421875)
    _Part150.Size = Vector3.new(20000, 0, 20000)

    local _Part151 = Instance.new('Part')

    _Part151.Parent = Game.Workspace
    _Part151.Anchored = true
    _Part151.Position = Vector3.new(2702.431640625, -9.231003761291504, 2118.501953125)
    _Part151.Size = Vector3.new(20000, 0, 20000)

    local _Part152 = Instance.new('Part')

    _Part152.Parent = Game.Workspace
    _Part152.Anchored = true
    _Part152.Position = Vector3.new(3402.743896484375, -9.015864849090576, 3122.62060546875)
    _Part152.Size = Vector3.new(20000, 0, 20000)

    local _Part153 = Instance.new('Part')

    _Part153.Parent = Game.Workspace
    _Part153.Anchored = true
    _Part153.Position = Vector3.new(3311.645263671875, -9.800727367401123, 4067.2041015625)
    _Part153.Size = Vector3.new(20000, 0, 20000)

    local _Part154 = Instance.new('Part')

    _Part154.Parent = Game.Workspace
    _Part154.Anchored = true
    _Part154.Position = Vector3.new(4368.765625, -9.800727128982544, 3317.60009765625)
    _Part154.Size = Vector3.new(20000, 0, 20000)

    local _Part155 = Instance.new('Part')

    _Part155.Parent = Game.Workspace
    _Part155.Anchored = true
    _Part155.Position = Vector3.new(4876.06005859375, -9.585587978363037, 4320.9013671875)
    _Part155.Size = Vector3.new(20000, 0, 20000)

    local _Part156 = Instance.new('Part')

    _Part156.Parent = Game.Workspace
    _Part156.Anchored = true
    _Part156.Position = Vector3.new(5258.15771484375, -9.370449542999268, 5299.39990234375)
    _Part156.Size = Vector3.new(20000, 0, 20000)

    local _Part157 = Instance.new('Part')

    _Part157.Parent = Game.Workspace
    _Part157.Anchored = true
    _Part157.Position = Vector3.new(5620.71923828125, -9.155311584472656, 6254.36181640625)
    _Part157.Size = Vector3.new(20000, 0, 20000)

    local _Part158 = Instance.new('Part')

    _Part158.Parent = Game.Workspace
    _Part158.Anchored = true
    _Part158.Position = Vector3.new(5176.56591796875, -9.585587978363037, 2400.698486328125)
    _Part158.Size = Vector3.new(20000, 0, 20000)

    local _Part159 = Instance.new('Part')

    _Part159.Parent = Game.Workspace
    _Part159.Anchored = true
    _Part159.Position = Vector3.new(6081.796875, -9.370450019836426, 3295.412109375)
    _Part159.Size = Vector3.new(20000, 0, 20000)

    local _Part160 = Instance.new('Part')

    _Part160.Parent = Game.Workspace
    _Part160.Anchored = true
    _Part160.Position = Vector3.new(5741.1318359375, -9.37044906616211, 1432.1351318359375)
    _Part160.Size = Vector3.new(20000, 0, 20000)

    local _Part161 = Instance.new('Part')

    _Part161.Parent = Game.Workspace
    _Part161.Anchored = true
    _Part161.Position = Vector3.new(5813.77685546875, -9.155310153961182, 415.4798889160156)
    _Part161.Size = Vector3.new(20000, 0, 20000)

    local _Part162 = Instance.new('Part')

    _Part162.Parent = Game.Workspace
    _Part162.Anchored = true
    _Part162.Position = Vector3.new(6412.279296875, -9.940170764923096, -563.8019409179688)
    _Part162.Size = Vector3.new(20000, 0, 20000)

    local _Part163 = Instance.new('Part')

    _Part163.Parent = Game.Workspace
    _Part163.Anchored = true
    _Part163.Position = Vector3.new(6898.72900390625, -9.725030422210693, -1527.509765625)
    _Part163.Size = Vector3.new(20000, 0, 20000)

    local _Part164 = Instance.new('Part')

    _Part164.Parent = Game.Workspace
    _Part164.Anchored = true
    _Part164.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part164.Size = Vector3.new(20000, 0, 20000)

    local _Part165 = Instance.new('Part')

    _Part165.Parent = Game.Workspace
    _Part165.Anchored = true
    _Part165.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part165.Size = Vector3.new(20000, 0, 20000)

    local _Part166 = Instance.new('Part')

    _Part166.Parent = Game.Workspace
    _Part166.Anchored = true
    _Part166.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part166.Size = Vector3.new(20000, 0, 20000)

    local _Part167 = Instance.new('Part')

    _Part167.Parent = Game.Workspace
    _Part167.Anchored = true
    _Part167.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part167.Size = Vector3.new(20000, 0, 20000)

    local _Part168 = Instance.new('Part')

    _Part168.Parent = Game.Workspace
    _Part168.Anchored = true
    _Part168.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part168.Size = Vector3.new(20000, 0, 20000)

    local _Part169 = Instance.new('Part')

    _Part169.Parent = Game.Workspace
    _Part169.Anchored = true
    _Part169.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part169.Size = Vector3.new(20000, 0, 20000)

    local _Part170 = Instance.new('Part')

    _Part170.Parent = Game.Workspace
    _Part170.Anchored = true
    _Part170.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part170.Size = Vector3.new(20000, 0, 20000)

    local _Part171 = Instance.new('Part')

    _Part171.Parent = Game.Workspace
    _Part171.Anchored = true
    _Part171.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part171.Size = Vector3.new(20000, 0, 20000)

    local _Part172 = Instance.new('Part')

    _Part172.Parent = Game.Workspace
    _Part172.Anchored = true
    _Part172.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part172.Size = Vector3.new(20000, 0, 20000)

    local _Part173 = Instance.new('Part')

    _Part173.Parent = Game.Workspace
    _Part173.Anchored = true
    _Part173.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part173.Size = Vector3.new(20000, 0, 20000)

    local _Part174 = Instance.new('Part')

    _Part174.Parent = Game.Workspace
    _Part174.Anchored = true
    _Part174.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part174.Size = Vector3.new(20000, 0, 20000)

    local _Part175 = Instance.new('Part')

    _Part175.Parent = Game.Workspace
    _Part175.Anchored = true
    _Part175.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part175.Size = Vector3.new(20000, 0, 20000)

    local _Part176 = Instance.new('Part')

    _Part176.Parent = Game.Workspace
    _Part176.Anchored = true
    _Part176.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part176.Size = Vector3.new(20000, 0, 20000)

    local _Part177 = Instance.new('Part')

    _Part177.Parent = Game.Workspace
    _Part177.Anchored = true
    _Part177.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part177.Size = Vector3.new(20000, 0, 20000)

    local _Part178 = Instance.new('Part')

    _Part178.Parent = Game.Workspace
    _Part178.Anchored = true
    _Part178.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part178.Size = Vector3.new(20000, 0, 20000)

    local _Part179 = Instance.new('Part')

    _Part179.Parent = Game.Workspace
    _Part179.Anchored = true
    _Part179.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part179.Size = Vector3.new(20000, 0, 20000)

    local _Part180 = Instance.new('Part')

    _Part180.Parent = Game.Workspace
    _Part180.Anchored = true
    _Part180.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part180.Size = Vector3.new(20000, 0, 20000)

    local _Part181 = Instance.new('Part')

    _Part181.Parent = Game.Workspace
    _Part181.Anchored = true
    _Part181.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part181.Size = Vector3.new(20000, 0, 20000)

    local _Part182 = Instance.new('Part')

    _Part182.Parent = Game.Workspace
    _Part182.Anchored = true
    _Part182.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part182.Size = Vector3.new(20000, 0, 20000)

    local _Part183 = Instance.new('Part')

    _Part183.Parent = Game.Workspace
    _Part183.Anchored = true
    _Part183.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part183.Size = Vector3.new(20000, 0, 20000)

    local _Part184 = Instance.new('Part')

    _Part184.Parent = Game.Workspace
    _Part184.Anchored = true
    _Part184.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part184.Size = Vector3.new(20000, 0, 20000)

    local _Part185 = Instance.new('Part')

    _Part185.Parent = Game.Workspace
    _Part185.Anchored = true
    _Part185.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part185.Size = Vector3.new(20000, 0, 20000)

    local _Part186 = Instance.new('Part')

    _Part186.Parent = Game.Workspace
    _Part186.Anchored = true
    _Part186.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part186.Size = Vector3.new(20000, 0, 20000)

    local _Part187 = Instance.new('Part')

    _Part187.Parent = Game.Workspace
    _Part187.Anchored = true
    _Part187.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part187.Size = Vector3.new(20000, 0, 20000)

    local _Part188 = Instance.new('Part')

    _Part188.Parent = Game.Workspace
    _Part188.Anchored = true
    _Part188.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part188.Size = Vector3.new(20000, 0, 20000)

    local _Part189 = Instance.new('Part')

    _Part189.Parent = Game.Workspace
    _Part189.Anchored = true
    _Part189.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part189.Size = Vector3.new(20000, 0, 20000)

    local _Part190 = Instance.new('Part')

    _Part190.Parent = Game.Workspace
    _Part190.Anchored = true
    _Part190.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part190.Size = Vector3.new(20000, 0, 20000)

    local _Part191 = Instance.new('Part')

    _Part191.Parent = Game.Workspace
    _Part191.Anchored = true
    _Part191.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part191.Size = Vector3.new(20000, 0, 20000)

    local _Part192 = Instance.new('Part')

    _Part192.Parent = Game.Workspace
    _Part192.Anchored = true
    _Part192.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part192.Size = Vector3.new(20000, 0, 20000)

    local _Part193 = Instance.new('Part')

    _Part193.Parent = Game.Workspace
    _Part193.Anchored = true
    _Part193.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part193.Size = Vector3.new(20000, 0, 20000)

    local _Part194 = Instance.new('Part')

    _Part194.Parent = Game.Workspace
    _Part194.Anchored = true
    _Part194.Position = Vector3.new(-6474.01318359375, -9.557838439941406, -1076.6319580078125)
    _Part194.Size = Vector3.new(20000, 0, 20000)
end)

local _Lighting4 = game:GetService('Lighting')
local _changetime = _Killer:AddDropdown('change time', function(p870)
    _Lighting4.Brightness = 2
    _Lighting4.FogEnd = 100000
    _Lighting4.Ambient = Color3.fromRGB(127, 127, 127)

    if p870 == 'Morning' then
        _Lighting4.ClockTime = 6
        _Lighting4.Brightness = 2
        _Lighting4.Ambient = Color3.fromRGB(200, 200, 255)
    elseif p870 == 'Noon' then
        _Lighting4.ClockTime = 12
        _Lighting4.Brightness = 3
        _Lighting4.Ambient = Color3.fromRGB(255, 255, 255)
    elseif p870 == 'Afternoon' then
        _Lighting4.ClockTime = 16
        _Lighting4.Brightness = 2.5
        _Lighting4.Ambient = Color3.fromRGB(255, 220, 180)
    elseif p870 == 'Sunset' then
        _Lighting4.ClockTime = 18
        _Lighting4.Brightness = 2
        _Lighting4.Ambient = Color3.fromRGB(255, 150, 100)
        _Lighting4.FogEnd = 500
    elseif p870 == 'Nigth' then
        _Lighting4.ClockTime = 20
        _Lighting4.Brightness = 1.5
        _Lighting4.Ambient = Color3.fromRGB(100, 100, 150)
        _Lighting4.FogEnd = 800
    elseif p870 == 'Midnight' then
        _Lighting4.ClockTime = 0
        _Lighting4.Brightness = 1
        _Lighting4.Ambient = Color3.fromRGB(50, 50, 100)
        _Lighting4.FogEnd = 400
    elseif p870 == 'Dawn' then
        _Lighting4.ClockTime = 4
        _Lighting4.Brightness = 1.8
        _Lighting4.Ambient = Color3.fromRGB(180, 180, 220)
    elseif p870 == 'Early Morning' then
        _Lighting4.ClockTime = 2
        _Lighting4.Brightness = 1.2
        _Lighting4.Ambient = Color3.fromRGB(100, 120, 180)
    end
end)
local v872, v873, v874 = ipairs({
    'Morning',
    'Noon',
    'Afternoon',
    'Sunset',
    'Night',
    'Midnight',
    'Dawn',
    'Early Morning',
})

while true do
    local v875

    v874, v875 = v872(v873, v874)

    if v874 == nil then
        break
    end

    _changetime:Add(v875)
end

local _StatsJugadores = v3:AddTab('Stats Jugadores')
local u877 = ''
local _SelectPlayer = _StatsJugadores:AddDropdown('Select Player', function(p878)
    u877 = p878:match('| (.+)')
    previousValues = {}
end)
local v880, v881, v882 = pairs(game:GetService('Players'):GetPlayers())
local u883 = u877
local v884 = {}

while true do
    local v885

    v882, v885 = v880(v881, v882)

    if v882 == nil then
        break
    end

    local v886 = v885.DisplayName .. ' | ' .. v885.Name

    table.insert(v884, v886)
end

local v887, v888, v889 = ipairs(v884)

while true do
    local v890

    v889, v890 = v887(v888, v889)

    if v889 == nil then
        break
    end

    _SelectPlayer:Add(v890)
end

local function u892(p891)
    return tostring(p891):reverse():gsub('(%d%d%d)', '%1,'):reverse():gsub('^,', '')
end
local function u896(p893)
    local v894 = {
        '',
        'K',
        'M',
        'B',
        'T',
        'Qa',
        'Qi',
    }
    local v895 = 1

    while p893 >= 1000 do
        p893 = p893 / 1000
        v895 = v895 + 1
    end

    return string.format('%.2f', p893) .. v894[v895]
end
local function u898(p897)
    return '[ ' .. u892(p897) .. ' | ' .. u896(p897) .. ' ]'
end

local u899 = _StatsJugadores:AddLabel('')
local u900 = _StatsJugadores:AddLabel('')
local u901 = _StatsJugadores:AddLabel('')
local u902 = _StatsJugadores:AddLabel('')
local u903 = _StatsJugadores:AddLabel('')
local u904 = _StatsJugadores:AddLabel('')
local u905 = _StatsJugadores:AddLabel('')
local u906 = _StatsJugadores:AddLabel('')
local u907 = _StatsJugadores:AddLabel('')
local u908 = _StatsJugadores:AddLabel('')
local u909 = _StatsJugadores:AddLabel('')
local u910 = _StatsJugadores:AddLabel('')

task.spawn(function()
    while task.wait(0) do
        local v911 = u883 ~= '' and game.Players:FindFirstChild(u883)

        if v911 then
            if v911:FindFirstChild('Gems') then
                u900.Text = 'Gems: ' .. u898(v911.Gems.Value)
            end
            if v911:FindFirstChild('Agility') then
                u902.Text = 'Agility: ' .. u898(v911.Agility.Value)
            end
            if v911:FindFirstChild('Durability') then
                u903.Text = 'Durability: ' .. u898(v911.Durability.Value)
            end
            if v911:FindFirstChild('muscleKingTime') then
                u905.Text = 'Muscle King Time: ' .. u898(v911.muscleKingTime.Value)
            end
            if v911:FindFirstChild('customSize') then
                u907.Text = 'Custom Size: ' .. u898(v911.customSize.Value)
            end
            if v911:FindFirstChild('customSpeed') then
                u908.Text = 'Custom Speed: ' .. u898(v911.customSpeed.Value)
            end
            if v911:FindFirstChild('evilKarma') then
                u909.Text = 'Evil Karma: ' .. u898(v911.evilKarma.Value)
            end
            if v911:FindFirstChild('goodKarma') then
                u910.Text = 'Good Karma: ' .. u898(v911.goodKarma.Value)
            end

            local _leaderstats = v911:FindFirstChild('leaderstats')

            if _leaderstats then
                if _leaderstats:FindFirstChild('Strength') then
                    u899.Text = 'Strength: ' .. u898(_leaderstats.Strength.Value)
                end
                if _leaderstats:FindFirstChild('Rebirths') then
                    u901.Text = 'Rebirth: ' .. u898(_leaderstats.Rebirths.Value)
                end
                if _leaderstats:FindFirstChild('Kills') then
                    u904.Text = 'Kills: ' .. u898(_leaderstats.Kills.Value)
                end
            end
            if v911:FindFirstChild('currentMap') then
                u906.Text = 'Current Map: ' .. tostring(v911.currentMap.Value)
            else
                u906.Text = 'Current Map: Aucune donn\u{c3}\u{83}\u{c6}\u{92}\u{c3}\u{86}\u{e2}\u{80}\u{99}\u{c3}\u{83}\u{e2}\u{80} \u{c3}\u{a2}\u{e2}\u{82}\u{ac}\u{e2}\u{84}\u{a2}\u{c3}\u{83}\u{c6}\u{92}\u{c3}\u{a2}\u{e2}\u{82}\u{ac}\u{c5}\u{a1}\u{c3}\u{83}\u{e2}\u{80}\u{9a}\u{c3}\u{82}\u{c2}\u{a9}e'
            end
        end
    end
end)
_StatsJugadores:AddLabel('\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}\u{2014}')

_StatsJugadores:AddLabel('Stats Avanzadas:').TextSize = 24

local _VidadelEnemigoNA = _StatsJugadores:AddLabel('Vida del Enemigo: N/A')

_VidadelEnemigoNA.TextSize = 20
_VidadelEnemigoNA.TextColor3 = Color3.fromRGB(255, 255, 255)

local _TuDaoNA = _StatsJugadores:AddLabel('Tu Da\u{f1}o: N/A')

_TuDaoNA.TextSize = 20
_TuDaoNA.TextColor3 = Color3.fromRGB(255, 255, 255)

local _GolpesparaMatarNA = _StatsJugadores:AddLabel('Golpes para Matar: N/A')

_GolpesparaMatarNA.TextSize = 20
_GolpesparaMatarNA.TextColor3 = Color3.fromRGB(255, 255, 255)

local function u928(p916)
    if not p916 then
        return 0
    end

    local v917 = 0
    local v918 = not p916:FindFirstChild('Durability') and p916:FindFirstChild('leaderstats')

    if v918 then
        v918 = p916.leaderstats:FindFirstChild('Durability')
    end
    if v918 then
        v917 = v918.Value
    end

    local v919 = 1
    local _ultimatesFolder2 = p916:FindFirstChild('ultimatesFolder')

    if _ultimatesFolder2 then
        local _InfernalHealth = _ultimatesFolder2:FindFirstChild('Infernal Health')

        if _InfernalHealth then
            v919 = v919 + 0.15 * (_InfernalHealth.Value or 0)
        end
    end

    local _Backpack5 = p916:FindFirstChild('Backpack')

    if _Backpack5 then
        local v923 = _Backpack5:FindFirstChild('EquippedPets') or _Backpack5:FindFirstChild('equippedPets')

        if v923 then
            local v924, v925, v926 = ipairs(v923:GetChildren())

            while true do
                local v927

                v926, v927 = v924(v925, v926)

                if v926 == nil then
                    break
                end
                if string.lower(tostring(v927)):match('mighty') and string.lower(tostring(v927)):match('monster') then
                    v919 = v919 + 0.5

                    break
                end
            end
        end
    end

    return v917 * v919
end
local function u942()
    local _LocalPlayer17 = game.Players.LocalPlayer

    if not _LocalPlayer17 then
        return 0
    end

    local _leaderstats2 = _LocalPlayer17:FindFirstChild('leaderstats')
    local v931

    if _leaderstats2 then
        v931 = _leaderstats2:FindFirstChild('Strength')
    else
        v931 = nil
    end
    if not v931 then
        return 0
    end

    local v932 = v931.Value * 0.0667
    local v933 = 1
    local _ultimatesFolder3 = _LocalPlayer17:FindFirstChild('ultimatesFolder')

    if _ultimatesFolder3 then
        local _DemonDamage = _ultimatesFolder3:FindFirstChild('Demon Damage')

        if _DemonDamage then
            v933 = v933 + math.min(0.1 * (_DemonDamage.Value or 0), 0.5)
        end
    end

    local _Backpack6 = _LocalPlayer17:FindFirstChild('Backpack')

    if _Backpack6 then
        local v937 = _Backpack6:FindFirstChild('EquippedPets') or _Backpack6:FindFirstChild('equippedPets')

        if v937 then
            local v938, v939, v940 = ipairs(v937:GetChildren())

            while true do
                local v941

                v940, v941 = v938(v939, v940)

                if v940 == nil then
                    break
                end
                if string.lower(tostring(v941)):match('wild') and string.lower(tostring(v941)):match('wizard') then
                    v933 = v933 + 0.5

                    break
                end
            end
        end
    end

    return v932 * v933
end
local function u946(p943, p944)
    if p944 <= 0 then
        return '\u{221e}'
    end

    local v945 = math.ceil(p943 / p944)

    return v945 > 50 and '\u{221e}' or (v945 < 1 and 1 or v945)
end
local function u951(p947)
    if p947 then
        local v948 = u928(p947)
        local v949 = u942()
        local v950 = u946(v948, v949)

        _VidadelEnemigoNA.Text = 'Vida del Enemigo: ' .. u898(v948)
        _TuDaoNA.Text = 'Tu Da\u{f1}o: ' .. u898(v949)
        _GolpesparaMatarNA.Text = 'Golpes para Matar: ' .. tostring(v950)
    else
        _VidadelEnemigoNA.Text = 'Vida del Enemigo: N/A'
        _TuDaoNA.Text = 'Tu Da\u{f1}o: N/A'
        _GolpesparaMatarNA.Text = 'Golpes para Matar: N/A'
    end
end

task.spawn(function()
    while task.wait(0.3) do
        u951((game.Players:FindFirstChild(u883)))
    end
end)

local _Teleport = v3:AddTab('Teleport')

_Teleport:AddButton('Spawn', function()
    local _LocalPlayer18 = game.Players.LocalPlayer;

    (_LocalPlayer18.Character or _LocalPlayer18.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(2, 8, 115)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Spawn',
        Duration = 0,
    })
end)
_Teleport:AddButton('Secret Area', function()
    local _LocalPlayer19 = game.Players.LocalPlayer;

    (_LocalPlayer19.Character or _LocalPlayer19.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(1947, 2, 6191)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Secret Area',
        Duration = 0,
    })
end)
_Teleport:AddButton('Tiny Island', function()
    local _LocalPlayer20 = game.Players.LocalPlayer;

    (_LocalPlayer20.Character or _LocalPlayer20.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(-34, 7, 1903)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Tiny Island',
        Duration = 0,
    })
end)
_Teleport:AddButton('Frozen Island', function()
    local _LocalPlayer21 = game.Players.LocalPlayer;

    (_LocalPlayer21.Character or _LocalPlayer21.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(-2600.00244, 3.67686558, -403.884369, 0.0873617008, 1.0482899e-9, 0.99617666, 3.0720425299999996e-8, 1, -3.7464023e-9, -0.99617666, 3.0930262799999997e-8, 0.0873617008)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Frozen Island',
        Duration = 0,
    })
end)
_Teleport:AddButton('Mythical Island', function()
    local _LocalPlayer22 = game.Players.LocalPlayer;

    (_LocalPlayer22.Character or _LocalPlayer22.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(2255, 7, 1071)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Mythical Island',
        Duration = 0,
    })
end)
_Teleport:AddButton('Hell Island', function()
    local _LocalPlayer23 = game.Players.LocalPlayer;

    (_LocalPlayer23.Character or _LocalPlayer23.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(-6768, 7, -1287)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Hell Island',
        Duration = 0,
    })
end)
_Teleport:AddButton('Legend Island', function()
    local _LocalPlayer24 = game.Players.LocalPlayer;

    (_LocalPlayer24.Character or _LocalPlayer24.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(4604, 991, -3887)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Legend Island',
        Duration = 0,
    })
end)
_Teleport:AddButton('Muscle King Island', function()
    local _LocalPlayer25 = game.Players.LocalPlayer;

    (_LocalPlayer25.Character or _LocalPlayer25.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(-8646, 17, -5738)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Muscle King',
        Duration = 0,
    })
end)
_Teleport:AddButton('Jungle Island', function()
    local _LocalPlayer26 = game.Players.LocalPlayer;

    (_LocalPlayer26.Character or _LocalPlayer26.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(-8659, 6, 2384)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Jungle Island',
        Duration = 0,
    })
end)
_Teleport:AddButton('Brawl Lava', function()
    local _LocalPlayer27 = game.Players.LocalPlayer;

    (_LocalPlayer27.Character or _LocalPlayer27.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(4471, 119, -8836)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Brawl Lava',
        Duration = 0,
    })
end)
_Teleport:AddButton('Brawl Desert', function()
    local _LocalPlayer28 = game.Players.LocalPlayer;

    (_LocalPlayer28.Character or _LocalPlayer28.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(960, 17, -7398)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Brawl Desert',
        Duration = 0,
    })
end)
_Teleport:AddButton('Brawl Regular', function()
    local _LocalPlayer29 = game.Players.LocalPlayer;

    (_LocalPlayer29.Character or _LocalPlayer29.CharacterAdded:Wait()):WaitForChild('HumanoidRootPart').CFrame = CFrame.new(-1849, 20, -6335)

    game:GetService('StarterGui'):SetCore('SendNotification', {
        Title = 'Teletransporte',
        Text = 'Teleported to Brawl Regular',
        Duration = 0,
    })
end)

local _Crystals = v3:AddTab('Crystals')
local _ReplicatedStorage6 = game:GetService('ReplicatedStorage')
local u967 = {
    ['Blue Crystal'] = {
        {
            name = 'Blue Birdie',
            rarity = 'Basic',
        },
        {
            name = 'Orange Hedgehog',
            rarity = 'Basic',
        },
        {
            name = 'Blue Aura',
            rarity = 'Basic',
        },
        {
            name = 'Red Kitty',
            rarity = 'Basic',
        },
        {
            name = 'Dark Vampy',
            rarity = 'Advanced',
        },
        {
            name = 'Blue Bunny',
            rarity = 'Basic',
        },
        {
            name = 'Red Aura',
            rarity = 'Basic',
        },
        {
            name = 'Blue Aura',
            rarity = 'Basic',
        },
        {
            name = 'Green Aura',
            rarity = 'Basic',
        },
        {
            name = 'Purple Aura',
            rarity = 'Basic',
        },
        {
            name = 'Red Aura',
            rarity = 'Basic',
        },
        {
            name = 'Yellow Aura',
            rarity = 'Basic',
        },
    },
    ['Green Crystal'] = {
        {
            name = 'Silver Dog',
            rarity = 'Basic',
        },
        {
            name = 'Green Aura',
            rarity = 'Advanced',
        },
        {
            name = 'Dark Golem',
            rarity = 'Advanced',
        },
        {
            name = 'Green Butterfly',
            rarity = 'Advanced',
        },
        {
            name = 'Crimson Falcon',
            rarity = 'Rare',
        },
        {
            name = 'Red Aura',
            rarity = 'Basic',
        },
        {
            name = 'Blue Aura',
            rarity = 'Basic',
        },
        {
            name = 'Green Aura',
            rarity = 'Basic',
        },
        {
            name = 'Purple Aura',
            rarity = 'Basic',
        },
        {
            name = 'Red Aura',
            rarity = 'Basic',
        },
        {
            name = 'Yellow Aura',
            rarity = 'Basic',
        },
    },
    ['Frost Crystal'] = {
        {
            name = 'Yellow Butterfly',
            rarity = 'Advanced',
        },
        {
            name = 'Purple Dragon',
            rarity = 'Rare',
        },
        {
            name = 'Blue Pheonix',
            rarity = 'Epic',
        },
        {
            name = 'Orange Pegasus',
            rarity = 'Rare',
        },
        {
            name = 'Lightning',
            rarity = 'Rare',
        },
        {
            name = 'Electro',
            rarity = 'Advanced',
        },
    },
    ['Mythical Crystal'] = {
        {
            name = 'Purple Falcon',
            rarity = 'Rare',
        },
        {
            name = 'Red Dragon',
            rarity = 'Rare',
        },
        {
            name = 'Blue Firecaster',
            rarity = 'Epic',
        },
        {
            name = 'Golden Pheonix',
            rarity = 'Epic',
        },
        {
            name = 'Power Lightning',
            rarity = 'Rare',
        },
        {
            name = 'Dark Lightning',
            rarity = 'Epic',
        },
    },
    ['Inferno Crystal'] = {
        {
            name = 'Red Firecaster',
            rarity = 'Epic',
        },
        {
            name = 'Infernal Dragon',
            rarity = 'Unique',
        },
        {
            name = 'White Pegasus',
            rarity = 'Rare',
        },
        {
            name = 'Golden Pheonix',
            rarity = 'Epic',
        },
        {
            name = 'Inferno',
            rarity = 'Epic',
        },
        {
            name = 'Dark Storm',
            rarity = 'Unique',
        },
    },
    ['Legends Crystal'] = {
        {
            name = 'Ultra Birdie',
            rarity = 'Unique',
        },
        {
            name = 'Magic Butterfly',
            rarity = 'Unique',
        },
        {
            name = 'Green Firecaster',
            rarity = 'Epic',
        },
        {
            name = 'White Pheonix',
            rarity = 'Epic',
        },
        {
            name = 'Supernova',
            rarity = 'Epic',
        },
        {
            name = 'Purple Nova',
            rarity = 'Unique',
        },
    },
    ['Muscle Elite Crystal'] = {
        {
            name = 'Frostwave Legends Penguin',
            rarity = 'Rare',
        },
        {
            name = 'Phantom Genesis Dragon',
            rarity = 'Rare',
        },
        {
            name = 'Dark Legends Manticore',
            rarity = 'Epic',
        },
        {
            name = 'Ultimate Supernova Pegasus',
            rarity = 'Epic',
        },
        {
            name = 'Aether Spirit Bunny',
            rarity = 'Unique',
        },
        {
            name = 'Cybernetic Showdown Dragon',
            rarity = 'Unique',
        },
    },
    ['Galaxy Oracle Crystal'] = {
        {
            name = 'Eternal Strike Leviathan',
            rarity = 'Rare',
        },
        {
            name = 'Lightning Strike Phantom',
            rarity = 'Epic',
        },
        {
            name = 'Darkstar Hunter',
            rarity = 'Unique',
        },
        {
            name = 'Muscle King',
            rarity = 'Unique',
        },
        {
            name = 'Azure Tundra',
            rarity = 'Epic',
        },
        {
            name = 'Ultra Inferno',
            rarity = 'Rare',
        },
    },
    ['Jungle Crystal'] = {
        {
            name = 'Entropic Blast',
            rarity = 'Unique',
        },
        {
            name = 'Muscle Sensei',
            rarity = 'Unique',
        },
        {
            name = 'Grand Supernova',
            rarity = 'Epic',
        },
        {
            name = 'Neon Guardian',
            rarity = 'Unique',
        },
        {
            name = 'Eternal Megastrike',
            rarity = 'Unique',
        },
        {
            name = 'Golden Viking',
            rarity = 'Epic',
        },
        {
            name = 'Astral Electro',
            rarity = 'Epic',
        },
        {
            name = 'Dark Electro',
            rarity = 'Epic',
        },
        {
            name = 'Enchanted Mirage',
            rarity = 'Epic',
        },
        {
            name = 'Ultra Mirage',
            rarity = 'Unique',
        },
        {
            name = 'Unstable Mirage',
            rarity = 'Unique',
        },
    },
}

local function v979()
    local v968, v969, v970 = pairs(u967)
    local v971 = {}
    local v972 = {}

    while true do
        local v973

        v970, v973 = v968(v969, v970)

        if v970 == nil then
            break
        end

        local v974, v975, v976 = ipairs(v973)
        local v977 = v970

        while true do
            local v978

            v976, v978 = v974(v975, v976)

            if v976 == nil then
                break
            end
            if string.find(v978.name, 'Aura') then
                if not v971[v978.name] then
                    v971[v978.name] = {
                        name = v978.name,
                        rarity = v978.rarity,
                        crystal = v977,
                    }
                end
            elseif not v972[v978.name] then
                v972[v978.name] = {
                    name = v978.name,
                    rarity = v978.rarity,
                    crystal = v977,
                }
            end
        end
    end

    return v972, v971
end
local function u990(p980)
    local v981, v982, v983 = pairs(u967)

    while true do
        local v984

        v983, v984 = v981(v982, v983)

        if v983 == nil then
            break
        end

        local v985, v986, v987 = ipairs(v984)
        local v988 = v983

        while true do
            local v989

            v987, v989 = v985(v986, v987)

            if v987 == nil then
                break
            end
            if v989.name == p980 then
                return v988
            end
        end
    end

    return nil
end

local u991 = ''
local u992 = ''
local _, _ = v979()

_Crystals:AddLabel('=== Buy pets and auras ===')

local _Selectpet = _Crystals:AddDropdown('Select pet', function(p993)
    u991 = p993

    local v994 = u990(p993)

    print('Pet selected: ' .. p993 .. ' (Found in: ' .. (v994 or 'Unknown') .. ')')
end)

_Selectpet:Add('Blue Birdie (Basic)')
_Selectpet:Add('Orange Hedgehog (Basic)')
_Selectpet:Add('Red Kitty (Basic)')
_Selectpet:Add('Blue Bunny (Basic)')
_Selectpet:Add('Silver Dog (Basic)')
_Selectpet:Add('Dark Vampy (Advanced)')
_Selectpet:Add('Dark Golem (Advanced)')
_Selectpet:Add('Green Butterfly (Advanced)')
_Selectpet:Add('Yellow Butterfly (Advanced)')
_Selectpet:Add('Crimson Falcon (Rare)')
_Selectpet:Add('Purple Dragon (Rare)')
_Selectpet:Add('Orange Pegasus (Rare)')
_Selectpet:Add('Purple Falcon (Rare)')
_Selectpet:Add('Red Dragon (Rare)')
_Selectpet:Add('White Pegasus (Rare)')
_Selectpet:Add('Frostwave Legends Penguin (Rare)')
_Selectpet:Add('Phantom Genesis Dragon (Rare)')
_Selectpet:Add('Eternal Strike Leviathan (Rare)')
_Selectpet:Add('Blue Pheonix (Epic)')
_Selectpet:Add('Blue Firecaster (Epic)')
_Selectpet:Add('Golden Pheonix (Epic)')
_Selectpet:Add('Red Firecaster (Epic)')
_Selectpet:Add('Green Firecaster (Epic)')
_Selectpet:Add('White Pheonix (Epic)')
_Selectpet:Add('Dark Legends Manticore (Epic)')
_Selectpet:Add('Ultimate Supernova Pegasus (Epic)')
_Selectpet:Add('Lightning Strike Phantom (Epic)')
_Selectpet:Add('Golden Viking (Epic)')
_Selectpet:Add('Infernal Dragon (Unique)')
_Selectpet:Add('Ultra Birdie (Unique)')
_Selectpet:Add('Magic Butterfly (Unique)')
_Selectpet:Add('Aether Spirit Bunny (Unique)')
_Selectpet:Add('Cybernetic Showdown Dragon (Unique)')
_Selectpet:Add('Darkstar Hunter (Unique)')
_Selectpet:Add('Muscle Sensei (Unique)')
_Selectpet:Add('Neon Guardian (Unique)')

local _SelectAura = _Crystals:AddDropdown('Select Aura', function(p996)
    u992 = p996

    local v997 = u990(p996)

    print('Aura selected: ' .. p996 .. ' (Found in: ' .. (v997 or 'Unknown') .. ')')
end)

_SelectAura:Add('Blue Aura (Basic)')
_SelectAura:Add('Green Aura (Basic)')
_SelectAura:Add('Purple Aura (Basic)')
_SelectAura:Add('Red Aura (Basic)')
_SelectAura:Add('Yellow Aura (Basic)')
_SelectAura:Add('Ultra Inferno  (Rare)')
_SelectAura:Add('Azure Tundra (Epic)')
_SelectAura:Add('Grand Supernova (Epic)')
_SelectAura:Add('Muscle King (Unique)')
_SelectAura:Add('Entropic Blast (Unique)')
_SelectAura:Add('Eternal Megastrike (Unique)')
_Crystals:AddLabel('=== System to buys===')
_Crystals:AddSwitch('Auto Buy Pet', function(p999)
    _G.AutoBuyPet = p999

    if p999 then
        if u991 == '' then
            print('Please select a pet first!')

            return
        end

        local u1000 = u991:match('^(.-)%s*%(') or u991
        local v1001 = u990(u1000)

        if not v1001 then
            print('Could not find crystal for pet: ' .. u1000)

            return
        end

        print('Auto buy pet started for: ' .. u1000 .. ' from ' .. v1001)
        spawn(function()
            while _G.AutoBuyPet and u991 ~= '' do
                local v1002 = _ReplicatedStorage6.cPetShopFolder:FindFirstChild(u1000)

                if v1002 then
                    _ReplicatedStorage6.cPetShopRemote:InvokeServer(v1002)
                    print('Bought pet: ' .. u1000)
                else
                    print('Pet not found: ' .. u1000)
                end

                task.wait(0.1)
            end
        end)
    else
        print('Auto buy pet stopped')
    end
end)
_Crystals:AddSwitch('Auto buy Aura', function(p1003)
    _G.AutoBuyAura = p1003

    if p1003 then
        if u992 == '' then
            print('Please select an aura first!')

            return
        end

        local u1004 = u992:match('^(.-)%s*%(') or u992
        local v1005 = u990(u1004)

        if not v1005 then
            print('Could not find crystal for aura: ' .. u1004)

            return
        end

        print('Auto buy aura started for: ' .. u1004 .. ' from ' .. v1005)
        spawn(function()
            while _G.AutoBuyAura and u992 ~= '' do
                local v1006 = _ReplicatedStorage6.cPetShopFolder:FindFirstChild(u1004)

                if v1006 then
                    _ReplicatedStorage6.cPetShopRemote:InvokeServer(v1006)
                    print('Bought aura: ' .. u1004)
                else
                    print('Aura not found: ' .. u1004)
                end

                task.wait(0.1)
            end
        end)
    else
        print('Auto buy aura stopped')
    end
end)

local _Gift = v3:AddTab('Gift')

_Gift:AddLabel('Gifting Protein egg:').TextSize = 22

local _ProteinEggs0 = _Gift:AddLabel('Protein Eggs: 0')

_ProteinEggs0.TextSize = 20

local u1009 = nil
local _PlayertoGiftEggs = _Gift:AddDropdown('Player to Gift Eggs', function(p1010)
    local v1011 = u648
    local v1012, v1013, v1014 = ipairs(v1011:GetPlayers())

    while true do
        local v1015

        v1014, v1015 = v1012(v1013, v1014)

        if v1014 == nil then
            break
        end
        if v1015.DisplayName == p1010 then
            u1009 = v1015

            break
        end
    end
end)
local v1017, v1018, v1019 = ipairs(u648:GetPlayers())
local u1020 = _ReplicatedStorage6
local u1021 = u1009
local u1022 = 0

while true do
    local v1023, v1024 = v1017(v1018, v1019)

    if v1023 == nil then
        break
    end

    v1019 = v1023

    if v1024 ~= u648.LocalPlayer then
        _PlayertoGiftEggs:Add(v1024.DisplayName)
    end
end

u648.PlayerAdded:Connect(function(p1025)
    if p1025 ~= u648.LocalPlayer then
        _PlayertoGiftEggs:Add(p1025.DisplayName)
    end
end)
_Gift:AddTextBox('Amount of Eggs', function(p1026)
    u1022 = tonumber(p1026) or 0
end)
_Gift:AddButton('Gift Eggs', function()
    if u1021 and u1022 > 0 then
        for _ = 1, u1022 do
            local _ProteinEgg3 = u648.LocalPlayer.consumablesFolder:FindFirstChild('Protein Egg')

            if _ProteinEgg3 then
                u1020.rEvents.giftRemote:InvokeServer('giftRequest', u1021, _ProteinEgg3)
                task.wait(0.1)
            end
        end
    end
end)

_Gift:AddLabel('Gifting Tropical Shakes:').TextSize = 22

local _TropicalShakes0 = _Gift:AddLabel('Tropical Shakes: 0')

_TropicalShakes0.TextSize = 18

local u1029 = nil
local _PlayertoGiftTropicalShakes = _Gift:AddDropdown('Player to Gift Tropical Shakes', function(p1030)
    local v1031 = u648
    local v1032, v1033, v1034 = ipairs(v1031:GetPlayers())

    while true do
        local v1035

        v1034, v1035 = v1032(v1033, v1034)

        if v1034 == nil then
            break
        end
        if v1035.DisplayName == p1030 then
            u1029 = v1035

            break
        end
    end
end)
local v1037, v1038, v1039 = ipairs(u648:GetPlayers())
local u1040 = u1029
local u1041 = 0

while true do
    local v1042, v1043 = v1037(v1038, v1039)

    if v1042 == nil then
        break
    end

    v1039 = v1042

    if v1043 ~= u648.LocalPlayer then
        _PlayertoGiftTropicalShakes:Add(v1043.DisplayName)
    end
end

u648.PlayerAdded:Connect(function(p1044)
    if p1044 ~= u648.LocalPlayer then
        _PlayertoGiftTropicalShakes:Add(p1044.DisplayName)
    end
end)
_Gift:AddTextBox('Tropical Shakes gift', function(p1045)
    u1041 = tonumber(p1045) or 0
end)
_Gift:AddButton('Gift Tropical Shakes', function()
    if u1040 and u1041 > 0 then
        for _ = 1, u1041 do
            local _TropicalShake = u648.LocalPlayer.consumablesFolder:FindFirstChild('Tropical Shake')

            if _TropicalShake then
                u1020.rEvents.giftRemote:InvokeServer('giftRequest', u1040, _TropicalShake)
                task.wait(0.1)
            end
        end
    end
end)

local function u1054()
    local v1047 = 0
    local v1048 = 0
    local _Backpack7 = u648.LocalPlayer:WaitForChild('Backpack')

    if _Backpack7 then
        local v1050, v1051, v1052 = ipairs(_Backpack7:GetChildren())

        while true do
            local v1053

            v1052, v1053 = v1050(v1051, v1052)

            if v1052 == nil then
                break
            end
            if v1053.Name ~= 'Protein Egg' then
                if v1053.Name == 'Tropical Shake' or v1053.Name == 'Pi\u{f1}as' then
                    v1048 = v1048 + 1
                end
            else
                v1047 = v1047 + 1
            end
        end
    end

    _ProteinEggs0.Text = 'Protein Eggs: ' .. v1047
    _TropicalShakes0.Text = 'Tropical Shakes: ' .. v1048
end

task.spawn(function()
    while true do
        u1054()
        task.wait(0.25)
    end
end)

local u1055 = false
local u1056 = {
    'ULTRA Shake',
    'TOUGH Bar',
    'Protein Shake',
    'Energy Shake',
    'Protein Bar',
    'Energy Bar',
    'Tropical Shake',
}

local function u1066()
    local _LocalPlayer30 = game.Players.LocalPlayer
    local _Backpack8 = _LocalPlayer30:WaitForChild('Backpack')
    local v1059 = _LocalPlayer30.Character or _LocalPlayer30.CharacterAdded:Wait()
    local v1060, v1061, v1062 = ipairs(u1056)

    while true do
        local v1063

        v1062, v1063 = v1060(v1061, v1062)

        if v1062 == nil then
            break
        end

        local u1064 = _Backpack8:FindFirstChild(v1063)

        while u1064 do
            u1064.Parent = v1059

            pcall(function()
                u1064:Activate()
            end)
            task.wait(0)

            local v1065 = _Backpack8:FindFirstChild(v1063)

            u1064 = v1065
        end
    end
end

task.spawn(function()
    while true do
        while u1055 do
            u1066()
            task.wait(2)
        end

        task.wait(1)
    end
end)
_Gift:AddSwitch('Auto Clear Inventory', function(p1067)
    u1055 = p1067
end)

local _Credits = v3:AddTab('Credits')

_Credits:AddLabel('Toxic Paid V3')
_Credits:AddLabel('Script Hecho por: Pirlo')
_Credits:AddLabel('Tiktok:pirlo._')
_Credits:AddLabel('MCL TOP')
_Credits:AddLabel('KING MI VIEJA')
