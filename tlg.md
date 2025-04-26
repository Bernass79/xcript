-- Carrega a biblioteca Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Criando a Janela da Interface com Discord e Key System
local Window = Rayfield:CreateWindow({
    Name = "🔥 v1Black Menu Mini City 🔥",
    LoadingTitle = "Black Menu",
    LoadingSubtitle = "by Bernardo",
    ConfigurationSaving = {
        Enabled = false
    },
    Theme = "Light", -- Movido para o nível correto, conforme https://docs.sirius.menu/rayfield/configuration/themes
    -- Configurações do Discord
    Discord = {
        Enabled = true,
        Invite = "9bzhsc6nDQ", -- Substitua por um invite válido, ex.: "discord.gg/hzmenu"
        RememberJoins = true
    },
    -- Configurações do Key System
    KeySystem = true,
    KeySettings = {
        Title = "Black Menu Key System",
        Subtitle = "Insira a key para entrar",
        Note = "Você precisa de uma key válida para acessar este menu",
        SaveKey = true,
        GrabKeyFromSite = false,
        Key = "HZ1234" -- Substitua por uma chave real
    }
})

local function cycleRGB()
    spawn(function()
        while rgbCycleEnabled do
            for i = 0, 360, 10 do
                if not rgbCycleEnabled then break end
                local hue = i / 360
                local color = Color3.fromHSV(hue, 1, 1)
                updateBeamColor(color)
                wait(0.1) -- Ajuste para controlar a velocidade do ciclo
            end
        end
    end)
end

local Tab = Window:CreateTab("Infos", 9405926389)
Rayfield:Notify({
   Title = "Black menu Injected 💸",
   Content = "Obrigado por Adquirir nosso menu!",
   Duration = 36.0,
   Image = 118582375849783,
})

local Paragraph = Tab:CreateParagraph({Title = "Black Menu ℹ️", Content = "+Novas Opções +Novas Farms +Menu Reeconstruido "})
local Button = Tab:CreateButton({ Name = "Ver Atualizações 📜", Callback = function() 
    print("Abrindo atualizações...")
end })

local CombatTab = Window:CreateTab("Combat ⚔️", 4483362458) -- Categoria Combat

----------------------
-- Noclip
----------------------
local noclipEnabled = false
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local localPlayer = Players.LocalPlayer
local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()

local function toggleNoclip(state)
    noclipEnabled = state
    if noclipEnabled then
        Rayfield:Notify({Title = "Noclip Ativado", Content = "Agora você pode atravessar objetos!", Duration = 2})
    else
        Rayfield:Notify({Title = "Noclip Desativado", Content = "Agora você colide normalmente.", Duration = 2})
    end
end

RunService.Stepped:Connect(function()
    if noclipEnabled then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    end
end)

CombatTab:CreateToggle({
    Name = "Noclip 🚀",
    CurrentValue = false,
    Callback = function(state)
        toggleNoclip(state)
    end
})

-- Speed
----------------------
local speedValue = 16
local speedEnabled = false
local function getHumanoid()
    if localPlayer.Character then
        return localPlayer.Character:FindFirstChildOfClass("Humanoid")
    end
    return nil
end

local function setSpeed(value)
    local humanoid = getHumanoid()
    if humanoid then
        humanoid.WalkSpeed = value
    end
end

task.spawn(function()
    while task.wait(0.05) do
        if speedEnabled then
            local humanoid = getHumanoid()
            if humanoid and humanoid.WalkSpeed ~= speedValue then
                humanoid.WalkSpeed = speedValue
            end
        end
    end
end)

localPlayer.CharacterAdded:Connect(function(character)
    repeat wait() until character:FindFirstChildOfClass("Humanoid")
    if speedEnabled then
        setSpeed(speedValue)
    end
end)

CombatTab:CreateToggle({
    Name = "Ativar Velocidade Personalizada",
    CurrentValue = false,
    Callback = function(value)
        speedEnabled = value
        if speedEnabled then
            setSpeed(speedValue)
        else
            setSpeed(16)
        end
    end
})

CombatTab:CreateSlider({
    Name = "Ajustar Velocidade",
    Range = {16, 100},
    Increment = 1,
    Suffix = "Studs/s",
    CurrentValue = 16,
    Callback = function(value)
        speedValue = value
    end
})

-- Anti-Fall Damage
----------------------
local antiFallEnabled = false

local function antiFallDamage()
    while antiFallEnabled do
        task.wait(0.1)

        if localPlayer.Character and localPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local humanoid = localPlayer.Character:FindFirstChildOfClass("Humanoid")
            local rootPart = localPlayer.Character:FindFirstChild("HumanoidRootPart")

            if humanoid and rootPart then
                if rootPart.Velocity.Y < -50 then
                    rootPart.Velocity = Vector3.new(rootPart.Velocity.X, 0, rootPart.Velocity.Z)
                end
            end
        end
    end
end

CombatTab:CreateToggle({
    Name = "Anti-Dano de Queda",
    CurrentValue = false,
    Callback = function(value)
        antiFallEnabled = value
        if antiFallEnabled then
            task.spawn(antiFallDamage)
        end
    end
})



local Tab = Window:CreateTab("AUTO-FARMS🌾", 484395794)
local Button = Tab:CreateButton({ Name = "Injetar Farms NESSESARIO ⚙️", Callback = function() 
    print("INJETADOOO")
	for _, prompt in ipairs(workspace:GetDescendants()) do
    if prompt:IsA("ProximityPrompt") then
        prompt.HoldDuration = 0
    end
end

workspace.DescendantAdded:Connect(function(descendant)
    if descendant:IsA("ProximityPrompt") then
        descendant.HoldDuration = 0
    end
end)

end })
local Button = Tab:CreateButton({
    Name = "Auto-Farm Gari 🗑️",
    Callback = function()
        print("Auto-Farm Gari ativado!")
        
        -- Função para encontrar o LEXOS mais próximo com colisão ativada
        local function findNearestLEXOS()
            local player = game:GetService("Players").LocalPlayer
            local character = player.Character or player.CharacterAdded:Wait()
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
            
            local lexosObjects = {}
            for _, obj in ipairs(workspace:GetDescendants()) do
                if obj.Name == "LEXOS" and obj:IsA("BasePart") and obj.CanCollide then
                    table.insert(lexosObjects, obj)
                end
            end
            
            if #lexosObjects == 0 then
                print("Nenhum LEXOS com colisão ativada encontrado!")
                return nil
            end
            
            local nearestLEXOS = nil
            local minDistance = math.huge
            for _, lexos in ipairs(lexosObjects) do
                local distance = (humanoidRootPart.Position - lexos.Position).Magnitude
                if distance < minDistance then
                    minDistance = distance
                    nearestLEXOS = lexos
                end
            end
            return nearestLEXOS
        end
        
        -- Função para teleportar para dentro do LEXOS mais próximo com colisão ativada
        local function teleportToNearestLEXOS()
            local nearestLEXOS = findNearestLEXOS()
            if nearestLEXOS then
                local player = game:GetService("Players").LocalPlayer
                local character = player.Character or player.CharacterAdded:Wait()
                local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
                
                -- Calcular o centro do LEXOS
                local centerPosition = nearestLEXOS.Position
                
                -- Teleportar para o centro do LEXOS
                humanoidRootPart.CFrame = CFrame.new(centerPosition)
            end
        end
        
        -- Modificar todos os ProximityPrompts para HoldDuration = 0
        for _, prompt in ipairs(workspace:GetDescendants()) do
            if prompt:IsA("ProximityPrompt") then
                prompt.HoldDuration = 0
            end
        end
        workspace.DescendantAdded:Connect(function(descendant)
            if descendant:IsA("ProximityPrompt") then
                descendant.HoldDuration = 0
            end
        end)
        
        -- Loop para teleportar e acionar o ProximityPrompt
        while true do
            teleportToNearestLEXOS()
            wait(0.3) -- Intervalo entre teleportes
            
            -- Acionar o ProximityPrompt mais próximo no LEXOS
            local nearestLEXOS = findNearestLEXOS()
            if nearestLEXOS then
                for _, child in ipairs(nearestLEXOS:GetChildren()) do
                    if child:IsA("ProximityPrompt") then
                        fireproximityprompt(child) -- Aciona o prompt diretamente
                        break
                    end
                end
            end
            wait(0.3) -- Intervalo após acionar o prompt
        end
    end
})
local Button = Tab:CreateButton({ Name = "Auto Farm Gás", Callback = function() 
    print("Auto-Farm Gás ativado!")
loadstring(game:HttpGet('https://raw.githubusercontent.com/Rafaasxs/Nexus-Menu-/refs/heads/main/tesao'))()
end })

local Button = Tab:CreateButton({
    Name = "Auto-Farm Rock 🪨",
    Callback = function()
        print("Auto-Farm Rock ativado!")
        
        -- Função para encontrar o Rock mais próximo com colisão ativada
        local function findNearestRock()
            local player = game:GetService("Players").LocalPlayer
            local character = player.Character or player.CharacterAdded:Wait()
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
            
            local rockObjects = {}
            for _, obj in ipairs(workspace:GetDescendants()) do
                if obj.Name == "Rock" and obj:IsA("BasePart") and obj.CanCollide then
                    table.insert(rockObjects, obj)
                end
            end
            
            if #rockObjects == 0 then
                print("Nenhum Rock com colisão ativada encontrado!")
                return nil
            end
            
            local nearestRock = nil
            local minDistance = math.huge
            for _, rock in ipairs(rockObjects) do
                local distance = (humanoidRootPart.Position - rock.Position).Magnitude
                if distance < minDistance then
                    minDistance = distance
                    nearestRock = rock
                end
            end
            return nearestRock
        end
        
        -- Função para teleportar para dentro do Rock mais próximo com colisão ativada
        local function teleportToNearestRock()
            local nearestRock = findNearestRock()
            if nearestRock then
                local player = game:GetService("Players").LocalPlayer
                local character = player.Character or player.CharacterAdded:Wait()
                local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
                
                -- Calcular o centro do Rock
                local centerPosition = nearestRock.Position
                
                -- Teleportar para o centro do Rock
                humanoidRootPart.CFrame = CFrame.new(centerPosition)
            end
        end
        
        -- Modificar todos os ProximityPrompts para HoldDuration = 0
        for _, prompt in ipairs(workspace:GetDescendants()) do
            if prompt:IsA("ProximityPrompt") then
                prompt.HoldDuration = 0
            end
        end
        workspace.DescendantAdded:Connect(function(descendant)
            if descendant:IsA("ProximityPrompt") then
                descendant.HoldDuration = 0
            end
        end)
        
        -- Loop para teleportar e acionar o ProximityPrompt (autoclick no E)
        while true do
            teleportToNearestRock()
            wait(0.3) -- Intervalo entre teleportes
            
            -- Acionar o ProximityPrompt mais próximo no Rock
            local nearestRock = findNearestRock()
            if nearestRock then
                for _, child in ipairs(nearestRock:GetChildren()) do
                    if child:IsA("ProximityPrompt") then
                        fireproximityprompt(child) -- Aciona o prompt diretamente
                        break
                    end
                end
            end
            wait(0.3) -- Intervalo após acionar o prompt
        end
    end
})


local Tab = Window:CreateTab("PVP", 15990136399)
local Button = Tab:CreateButton({ Name = "Aimbot 🎯", Callback = function() 
    print("Aimbot ativado!")
	--// Cache

local select = select
local pcall, getgenv, next, Vector2, mathclamp, type, mousemoverel = select(1, pcall, getgenv, next, Vector2.new, math.clamp, type, mousemoverel or (Input and Input.MouseMove))

--// Preventing Multiple Processes

pcall(function()
	getgenv().Aimbot.Functions:Exit()
end)

--// Environment

getgenv().Aimbot = {}
local Environment = getgenv().Aimbot

--// Services

local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

--// Variables

local RequiredDistance, Typing, Running, Animation, ServiceConnections = 2000, false, false, nil, {}

--// Script Settings

Environment.Settings = {
	Enabled = true,
	TeamCheck = false,
	AliveCheck = true,
	WallCheck = false, -- Laggy
	Sensitivity = 0, -- Animation length (in seconds) before fully locking onto target
	ThirdPerson = false, -- Uses mousemoverel instead of CFrame to support locking in third person (could be choppy)
	ThirdPersonSensitivity = 3, -- Boundary: 0.1 - 5
	TriggerKey = "MouseButton2",
	Toggle = false,
	LockPart = "Head" -- Body part to lock on
}

Environment.FOVSettings = {
	Enabled = true,
	Visible = true,
	Amount = 90,
	Color = Color3.fromRGB(0, 0, 0),
	LockedColor = Color3.fromRGB(255, 70, 70),
	Transparency = 0.5,
	Sides = 60,
	Thickness = 1,
	Filled = false
}

Environment.FOVCircle = Drawing.new("Circle")

--// Functions

local function CancelLock()
	Environment.Locked = nil
	if Animation then Animation:Cancel() end
	Environment.FOVCircle.Color = Environment.FOVSettings.Color
end

local function GetClosestPlayer()
	if not Environment.Locked then
		RequiredDistance = (Environment.FOVSettings.Enabled and Environment.FOVSettings.Amount or 2000)

		for _, v in next, Players:GetPlayers() do
			if v ~= LocalPlayer then
				if v.Character and v.Character:FindFirstChild(Environment.Settings.LockPart) and v.Character:FindFirstChildOfClass("Humanoid") then
					if Environment.Settings.TeamCheck and v.Team == LocalPlayer.Team then continue end
					if Environment.Settings.AliveCheck and v.Character:FindFirstChildOfClass("Humanoid").Health <= 0 then continue end
					if Environment.Settings.WallCheck and #(Camera:GetPartsObscuringTarget({v.Character[Environment.Settings.LockPart].Position}, v.Character:GetDescendants())) > 0 then continue end

					local Vector, OnScreen = Camera:WorldToViewportPoint(v.Character[Environment.Settings.LockPart].Position)
					local Distance = (Vector2(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2(Vector.X, Vector.Y)).Magnitude

					if Distance < RequiredDistance and OnScreen then
						RequiredDistance = Distance
						Environment.Locked = v
					end
				end
			end
		end
	elseif (Vector2(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2(Camera:WorldToViewportPoint(Environment.Locked.Character[Environment.Settings.LockPart].Position).X, Camera:WorldToViewportPoint(Environment.Locked.Character[Environment.Settings.LockPart].Position).Y)).Magnitude > RequiredDistance then
		CancelLock()
	end
end

--// Typing Check

ServiceConnections.TypingStartedConnection = UserInputService.TextBoxFocused:Connect(function()
	Typing = true
end)

ServiceConnections.TypingEndedConnection = UserInputService.TextBoxFocusReleased:Connect(function()
	Typing = false
end)

--// Main

local function Load()
	ServiceConnections.RenderSteppedConnection = RunService.RenderStepped:Connect(function()
		if Environment.FOVSettings.Enabled and Environment.Settings.Enabled then
			Environment.FOVCircle.Radius = Environment.FOVSettings.Amount
			Environment.FOVCircle.Thickness = Environment.FOVSettings.Thickness
			Environment.FOVCircle.Filled = Environment.FOVSettings.Filled
			Environment.FOVCircle.NumSides = Environment.FOVSettings.Sides
			Environment.FOVCircle.Color = Environment.FOVSettings.Color
			Environment.FOVCircle.Transparency = Environment.FOVSettings.Transparency
			Environment.FOVCircle.Visible = Environment.FOVSettings.Visible
			Environment.FOVCircle.Position = Vector2(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
		else
			Environment.FOVCircle.Visible = false
		end

		if Running and Environment.Settings.Enabled then
			GetClosestPlayer()

			if Environment.Locked then
				if Environment.Settings.ThirdPerson then
					Environment.Settings.ThirdPersonSensitivity = mathclamp(Environment.Settings.ThirdPersonSensitivity, 0.1, 5)

					local Vector = Camera:WorldToViewportPoint(Environment.Locked.Character[Environment.Settings.LockPart].Position)
					mousemoverel((Vector.X - UserInputService:GetMouseLocation().X) * Environment.Settings.ThirdPersonSensitivity, (Vector.Y - UserInputService:GetMouseLocation().Y) * Environment.Settings.ThirdPersonSensitivity)
				else
					if Environment.Settings.Sensitivity > 0 then
						Animation = TweenService:Create(Camera, TweenInfo.new(Environment.Settings.Sensitivity, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {CFrame = CFrame.new(Camera.CFrame.Position, Environment.Locked.Character[Environment.Settings.LockPart].Position)})
						Animation:Play()
					else
						Camera.CFrame = CFrame.new(Camera.CFrame.Position, Environment.Locked.Character[Environment.Settings.LockPart].Position)
					end
				end

			Environment.FOVCircle.Color = Environment.FOVSettings.LockedColor

			end
		end
	end)

	ServiceConnections.InputBeganConnection = UserInputService.InputBegan:Connect(function(Input)
		if not Typing then
			pcall(function()
				if Input.KeyCode == Enum.KeyCode[Environment.Settings.TriggerKey] then
					if Environment.Settings.Toggle then
						Running = not Running

						if not Running then
							CancelLock()
						end
					else
						Running = true
					end
				end
			end)

			pcall(function()
				if Input.UserInputType == Enum.UserInputType[Environment.Settings.TriggerKey] then
					if Environment.Settings.Toggle then
						Running = not Running

						if not Running then
							CancelLock()
						end
					else
						Running = true
					end
				end
			end)
		end
	end)

	ServiceConnections.InputEndedConnection = UserInputService.InputEnded:Connect(function(Input)
		if not Typing then
			if not Environment.Settings.Toggle then
				pcall(function()
					if Input.KeyCode == Enum.KeyCode[Environment.Settings.TriggerKey] then
						Running = false; CancelLock()
					end
				end)

				pcall(function()
					if Input.UserInputType == Enum.UserInputType[Environment.Settings.TriggerKey] then
						Running = false; CancelLock()
					end
				end)
			end
		end
	end)
end

--// Functions

Environment.Functions = {}

function Environment.Functions:Exit()
	for _, v in next, ServiceConnections do
		v:Disconnect()
	end

	if Environment.FOVCircle.Remove then Environment.FOVCircle:Remove() end

	getgenv().Aimbot.Functions = nil
	getgenv().Aimbot = nil
	
	Load = nil; GetClosestPlayer = nil; CancelLock = nil
end

function Environment.Functions:Restart()
	for _, v in next, ServiceConnections do
		v:Disconnect()
	end

	Load()
end

function Environment.Functions:ResetSettings()
	Environment.Settings = {
		Enabled = true,
		TeamCheck = false,
		AliveCheck = true,
		WallCheck = false,
		Sensitivity = 0, -- Animation length (in seconds) before fully locking onto target
		ThirdPerson = false, -- Uses mousemoverel instead of CFrame to support locking in third person (could be choppy)
		ThirdPersonSensitivity = 3, -- Boundary: 0.1 - 5
		TriggerKey = "MouseButton2",
		Toggle = false,
		LockPart = "black" -- Body part to lock on
	}

	Environment.FOVSettings = {
		Enabled = true,
		Visible = true,
		Amount = 90,
		Color = Color3.fromRGB(0, 0, 0), --Preto
		LockedColor = Color3.fromRGB(255, 70, 70),
		Transparency = 0.5,
		Sides = 60,
		Thickness = 1,
		Filled = false
	}
end

--// Load

Load()
end })





















-- Aba para ESP
local ESPTab = Window:CreateTab("Visual 👁️", 4483362458)

-- Tabela para armazenar os dados do ESP
local espData = {}

-- Função para criar ESP para um jogador
local function createESP(targetPlayer)
    if targetPlayer == game.Players.LocalPlayer or not targetPlayer.Character then
        return
    end

    local targetCharacter = targetPlayer.Character
    local targetRootPart = targetCharacter:WaitForChild("HumanoidRootPart", 5)
    local targetHumanoid = targetCharacter:WaitForChild("Humanoid", 5)
    if not targetRootPart or not targetHumanoid then
        return
    end

    -- Cria a linha (Beam)
    local attachment0 = Instance.new("Attachment")
    local attachment1 = Instance.new("Attachment")
    local beam = Instance.new("Beam")
    attachment0.Parent = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    attachment1.Parent = targetRootPart
    beam.Attachment0 = attachment0
    beam.Attachment1 = attachment1
    beam.Color = ColorSequence.new(Color3.fromRGB(255, 0, 0))
    beam.Width0 = 0.2
    beam.Width1 = 0.2
    beam.Transparency = NumberSequence.new(0)
    beam.Enabled = false
    beam.Parent = targetCharacter

    -- Cria o Highlight
    local highlight = Instance.new("Highlight")
    highlight.Adornee = targetCharacter
    highlight.FillColor = Color3.fromRGB(0, 255, 0)
    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
    highlight.FillTransparency = 0.5
    highlight.OutlineTransparency = 0
    highlight.Parent = targetCharacter

    -- Cria o texto (BillboardGui)
    local billboard = Instance.new("BillboardGui")
    billboard.Adornee = targetRootPart
    billboard.Size = UDim2.new(0, 100, 0, 50)
    billboard.StudsOffset = Vector3.new(0, 3, 0)
    billboard.AlwaysOnTop = true
    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
    textLabel.TextStrokeTransparency = 0
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.TextScaled = true
    textLabel.Parent = billboard
    billboard.Parent = targetCharacter

    return {beam = beam, attachment0 = attachment0, attachment1 = attachment1, highlight = highlight, billboard = billboard, textLabel = textLabel}
end

-- Função para atualizar o ESP
local function updateESP()
    spawn(function()
        while getgenv().ESPEnabled do
            local localCharacter = game.Players.LocalPlayer.Character
            local localRootPart = localCharacter and localCharacter:FindFirstChild("HumanoidRootPart")
            if not localRootPart then
                for _, data in pairs(espData) do
                    data.beam.Enabled = false
                    data.highlight.Enabled = false
                    data.billboard.Enabled = false
                end
                wait()
                continue
            end

            for targetPlayer, data in pairs(espData) do
                local targetCharacter = targetPlayer.Character
                local targetRootPart = targetCharacter and targetCharacter:FindFirstChild("HumanoidRootPart")
                local targetHumanoid = targetCharacter and targetCharacter:FindFirstChild("Humanoid")

                if targetRootPart and targetHumanoid then
                    data.beam.Enabled = getgenv().ESPLinesEnabled or false
                    data.highlight.Enabled = getgenv().ESPHighlightsEnabled or false
                    local distance = (localRootPart.Position - targetRootPart.Position).Magnitude
                    data.textLabel.Text = string.format("%s\nHP: %d/%d\nDist: %.1f", targetPlayer.Name, targetHumanoid.Health, targetHumanoid.MaxHealth, distance)
                    data.billboard.Enabled = getgenv().ESPTextEnabled or false
                else
                    data.beam.Enabled = false
                    data.highlight.Enabled = false
                    data.billboard.Enabled = false
                end
            end
            wait()
        end
        for _, data in pairs(espData) do
            data.beam.Enabled = false
            data.highlight.Enabled = false
            data.billboard.Enabled = false
        end
    end)
end

-- Função para ativar/desativar o ESP
local function toggleESP(state)
    getgenv().ESPEnabled = state
    if state then
        for _, player in pairs(game.Players:GetPlayers()) do
            espData[player] = createESP(player)
        end
        game.Players.PlayerAdded:Connect(function(player)
            espData[player] = createESP(player)
        end)
        updateESP()
    else
        for _, data in pairs(espData) do
            if data.beam then data.beam:Destroy() end
            if data.attachment0 then data.attachment0:Destroy() end
            if data.attachment1 then data.attachment1:Destroy() end
            if data.highlight then data.highlight:Destroy() end
            if data.billboard then data.billboard:Destroy() end
        end
        espData = {}
    end
end

-- Toggles para o ESP
ESPTab:CreateToggle({
    Name = "ESP Jogadores 👁️",
    CurrentValue = false,
    Callback = function(state)
        toggleESP(state)
        getgenv().ESPLinesEnabled = state
        getgenv().ESPHighlightsEnabled = state
        getgenv().ESPTextEnabled = state
    end
})
ESPTab:CreateToggle({
    Name = "Piscar Cores RGB 🌈",
    CurrentValue = false,
    Callback = function(state)
        rgbCycleEnabled = state
        if state then
            cycleRGB()
        else
            updateBeamColor(currentColor) -- Volta para a cor estática
        end
    end
})

ESPTab:CreateToggle({
    Name = "Texto ESP 📝",
    CurrentValue = false,
    Callback = function(state)
        getgenv().ESPTextEnabled = state
    end
})

-- ColorPicker para selecionar a cor das linhas
ESPTab:CreateColorPicker({
    Name = "Color Picker",
    Color = Color3.fromRGB(255, 0, 0), -- Cor inicial vermelha para combinar com o padrão do Beam
    Flag = "ColorPicker1",
    Callback = function(Value)
        for _, data in pairs(espData) do
            data.beam.Color = ColorSequence.new(Value)
        end
    end
})

local TeleportTab = Window:CreateTab("Teleports")
local Paragraph = TeleportTab:CreateParagraph({Title = "H2 Teleports", Content = "feito por Bernardo "})
local function addTeleportButton(name, cframe)
    TeleportTab:CreateButton({
        Name = name,
        Callback = function()
            local player = game.Players.LocalPlayer
            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                player.Character:MoveTo(cframe.Position + Vector3.new(0, 3, 0))
            end
        end,
    })
end
-- Adicionando os botões de teletransporte para locais fixos
addTeleportButton("Teleport Praça", CFrame.new(-291.579559, 3.26299787, 342.192535))
addTeleportButton("Teleport Gás", CFrame.new(-469.959015, 3.25349784, -54.3936005))
addTeleportButton("Teleport HP", CFrame.new(-543.439941, 3.26299858, 645.16864))
addTeleportButton("Teleport Tabacaria", CFrame.new(-83.1141129, 13.1430578, 74.7073364))
addTeleportButton("Teleport Garagem", CFrame.new(-466.870148, 7.64567232, 350.242737))
addTeleportButton("Teleport Concessionaria", CFrame.new(-91.3902893, 8.07136822, 520.355347))
addTeleportButton("Teleport Gari", CFrame.new(-518.672852, 3.16749811, -1.16962147, 0, 0, -1, 0, 1, 0, 1, 0, 0))
addTeleportButton("Teleport Imobiliaria", CFrame.new(-284.904785, 8.26088619, -72.2896194, 0, 0, -1, 0, 1, 0, 1, 0, 0))
addTeleportButton("Teleport PM", CFrame.new(-980.181458, 2.27553082, 467.080536, 1, 0, 0, 0, 1, 0, 0, 0, 1))
addTeleportButton("Teleport PRF", CFrame.new(6662.24512, 36.6637421, 5047.83838, 0.707134247, 0, 0.707079291, 0, 1, 0, -0.707079291, 0, 0.707134247))
addTeleportButton("Teleport Mineração", CFrame.new(201.932144, 2.76136589, 145.50531, 0, 0, 1, 0, 1, -0, -1, 0, 0))
addTeleportButton("Teleport Mecânica", CFrame.new(-180.608261, 3.29813337, -532.4151, 0.422592998, -0, -0.906319618, 0, 1, -0, 0.906319618, 0, 0.422592998))
addTeleportButton("Teleport Fazenda", CFrame.new(817.243225, 3.26249814, -87.316864, 0, 0, 1, 0, 1, 0, -1, 0, 0))
addTeleportButton("Teleport Prefeitura", CFrame.new(-284.388458, 15.1148872, 88.0397873, 0, 0, -1, 0, 1, 0, 1, 0, 0))
addTeleportButton("Teleport Banco", CFrame.new(-27.2709007, 11.5685892, 418.200653, 1, 0, 0, 0, 1, 0, 0, 0, 1))
addTeleportButton("Teleport Ilegal", CFrame.new(12037.2705, 27.5305443, 12794.0635, 0.965929627, -0, -0.258804798, 0, 1, -0, 0.258804798, 0, 0.965929627))
addTeleportButton("Teleport predio 1", CFrame.new(-1595.23328, 204.074341, 555.895386, 0.939687431, -0.34203434, 1.81794167e-06, 1.81794167e-06, 1.02519989e-05, 1, -0.34203434, -0.93968749, 1.02519989e-05))
addTeleportButton("Teleport Devs Mini City", CFrame.new(2555.44263, 303.167755, -1004.13763, -0.422592998, 0, 0.906319618, 0, 1, 0, -0.906319618, 0, -0.422592998))


local rev = Window:CreateTab("Revistar 🔍")
local Paragraph = rev:CreateParagraph({Title = "Berninhas", Content = "Berninhas 👨‍💻"})
local Section = rev:CreateSection("NECESSARIO")
local Button = rev:CreateButton({
   Name = "Puxar Itens 🎒",
   Callback = function()
   -- Função para deletar todas as NotifyGui
local function deletarNotifyGui()
    local playerGui = game.Players.LocalPlayer:WaitForChild("PlayerGui")
    for _, gui in ipairs(playerGui:GetChildren()) do
        if gui.Name == "NotifyGui" and gui:IsA("ScreenGui") then
            gui:Destroy() -- Deleta a NotifyGui
        end
    end
end

-- Lista de itens para pegar
local itens = {"AK47", "Uzi", "Parafal", "Faca", "IA2", "G3", "IPhone 14", "Agua", "Hamburguer", "Hi Power", "Natalina"}

-- Argumentos para a requisição
local args = {
    [1] = "mudaInv",
    [2] = "2",
    [4] = "1"
}

-- Loop principal
while true do
    -- Deletar todas as NotifyGui antes de pegar os itens
    deletarNotifyGui()

    -- Pegar itens
    for i, item in ipairs(itens) do
        if i <= 16 then  -- Só tenta alocar até 16 slots
            args[3] = item  -- Atualiza o item para o valor da vez
            args[2] = tostring(i)  -- Atribui o slot dinamicamente (1, 2, 3, ..., 16)

            -- Usar task.spawn() para execução sem delay
            task.spawn(function()
                -- Envia a requisição para o servidor
                game:GetService("ReplicatedStorage"):WaitForChild("Modules"):WaitForChild("InvRemotes"):WaitForChild("InvRequest"):InvokeServer(unpack(args))
            end)
        end
    end

    wait(0)  -- Espera um frame para evitar lag
end
   end,
})
local Section = rev:CreateSection("PC")
local ww = rev:CreateToggle({
   Name = "mandar revistar (TECLA E)",
   CurrentValue = false,
   Flag = "rvst",
   Callback = function(Value)
       getgenv().Enabled = Value
   end,
})
local Section = rev:CreateSection("MOBILE")
local mob = rev:CreateButton({
   Name = "mandar revistar UI",
   Callback = function()
   local TextChatService = game:GetService("TextChatService")

-- Função para enviar a mensagem /revistar morto
local function sendRevistarMessage()
    local channel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
    channel:SendAsync("/revistar morto")
end

-- Cria a interface
local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local CloseButton = Instance.new("TextButton")
local RevistarButton = Instance.new("TextButton")
local Title = Instance.new("TextLabel")

ScreenGui.Name = "RevistarUI"
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

-- Estilo do Frame
Frame.Size = UDim2.new(0, 300, 0, 150)
Frame.Position = UDim2.new(0.5, -150, 0.5, -75)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BorderSizePixel = 0
Frame.BackgroundTransparency = 0.1
Frame.Active = true
Frame.Draggable = true

-- Título
Title.Size = UDim2.new(1, 0, 0, 30)
Title.Position = UDim2.new(0, 0, 0, 0)
Title.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Title.Text = "Arthur"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 18

-- Botão Fechar
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -30, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(255, 50, 50)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 18
CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)


-- Botão Revistar
RevistarButton.Size = UDim2.new(0.8, 0, 0.4, 0)
RevistarButton.Position = UDim2.new(0.1, 0, 0.5, -30)
RevistarButton.BackgroundColor3 = Color3.fromRGB(50, 150, 250)
RevistarButton.Text = "manda /revistar morto"
RevistarButton.TextColor3 = Color3.fromRGB(255, 255, 255)
RevistarButton.Font = Enum.Font.SourceSansBold
RevistarButton.TextSize = 20
RevistarButton.AutoButtonColor = true
RevistarButton.MouseButton1Click:Connect(function()
    sendRevistarMessage()
end)

-- Adiciona os elementos ao frame
Frame.Parent = ScreenGui
Title.Parent = Frame
CloseButton.Parent = Frame
RevistarButton.Parent = Frame
   end,
})
local Section = rev:CreateSection("Info")
local Paragraph = rev:CreateParagraph({Title = "Como usar?", Content = "Ensinamos a usar corretamente no nosso discord."})
local otoTab = Window:CreateTab("Outros")
local Paragraph = otoTab:CreateParagraph({Title = "Berninhas top1", Content = "feito por Bernardop"})
local Toggle = otoTab:CreateToggle({
    Name = "anti staff V2",
    CurrentValue = false,
    Flag = "ToggleKickCheck",
    Callback = function(Value)
        if Value then
            getgenv().KickCheck = true
            local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- Lista de nomes a serem monitorados
local targetUsernames = {
    "jake56839ad", "CleitinDoGrau_Eb", "21peteca", "Lucalarte", "SPTmatheus123",
    "GuilhermeDRTgg", "Briessxz", "hardstyless", "Mundaka", "Isabelaaaaafofs",
    "HANRLLEY25", "kaleb_iaee", "brunizoraa", "rip_propleyfran", "pepezicador",
    "Jjhgul", "Dariosantos21048", "JEKER_2009", "tttonas", "MZPlug14k",
    "Dudubeterotv5", "Sargento_admOficial", "Cassiopia84un", "Hakplays", "Cleo_ptr"
}

-- Converte a lista para uma tabela para verificação rápida
local targetLookup = {}
for _, name in ipairs(targetUsernames) do
    targetLookup[name] = true
end

-- Inicializa a configuração global
getgenv().KickCheck = getgenv().KickCheck or true

-- Função para verificar e kickar caso necessário
local function checkForTargetPlayers()
    if not getgenv().KickCheck then return end

    for _, player in pairs(Players:GetPlayers()) do
        if targetLookup[player.Name] then
            warn("Usuário indesejado detectado:", player.Name)
            LocalPlayer:Kick("Um staff foi detectado no servidor.")
            break
        end
    end
end

-- Verifica inicialmente
checkForTargetPlayers()

-- Verifica sempre que um novo jogador entrar
Players.PlayerAdded:Connect(function(player)
    if getgenv().KickCheck and targetLookup[player.Name] then
        warn("Usuário indesejado detectado:", player.Name)
        LocalPlayer:Kick("Um staff foi detectado no servidor.")
    end
end)
print("check")
        else
            getgenv().KickCheck = false
        end
    end
})


-- Loop para checar a cada 5 segundos, apenas se ativado
task.spawn(function()
    while true do
        if checkActive then
            checkStaffTeam()
        end
        task.wait(5)
    end
end)



local Section = otoTab:CreateSection("AUTO CL CONFIG")
-- Verifica se getgenv está disponível
if getgenv == nil then
    error("getgenv não está definido neste ambiente.")
end

-- Configuração inicial do getgenv
getgenv().KickOnLowHealth = false -- Ativar/desativar o auto kick
getgenv().HealthThreshold = 10    -- Vida mínima antes de ser expulso

-- Função para monitorar a vida do jogador
local function monitorHealth()
    local player = game.Players.LocalPlayer
    local character = player.Character
    if character and character:FindFirstChild("Humanoid") then
        local humanoid = character.Humanoid
        humanoid.HealthChanged:Connect(function(health)
            if health <= getgenv().HealthThreshold and getgenv().KickOnLowHealth then
                player:Kick("Auto cl pq tua vida tava abaixo de " .. getgenv().HealthThreshold)
            end
        end)
    end
end

game.Players.LocalPlayer.CharacterAdded:Connect(function()
    monitorHealth()
end)

if game.Players.LocalPlayer.Character then
    monitorHealth()
end

-- Criação do Toggle para ativar/desativar o auto kick
local Toggle = otoTab:CreateToggle({
   Name = "auto CL",
   CurrentValue = getgenv().KickOnLowHealth,
   Flag = "AutoKickToggle",
   Callback = function(Value)
       getgenv().KickOnLowHealth = Value
   end,
})

-- Criação do Slider para ajustar o limite de vida
local Slider = otoTab:CreateSlider({
   Name = "kick quando vida =",
   Range = {0, 100},
   Increment = 1,
   Suffix = " HP",
   CurrentValue = getgenv().HealthThreshold,
   Flag = "HealthThresholdSlider",
   Callback = function(Value)
       getgenv().HealthThreshold = Value
   end,
})

getgenv().Key = Enum.KeyCode.E
getgenv().Enabled = getgenv().Enabled or false
local UserInputService = game:GetService("UserInputService")

-- Configurações via getgenv
getgenv().Key = getgenv().Key or Enum.KeyCode.R -- Tecla padrão: R
getgenv().Enabled = getgenv().Enabled or false -- Estado inicial: desativado

-- Função para enviar a mensagem /revistar
local function sendRevistarMessage()
    local TextChatService = game:GetService("TextChatService")
    local channel = TextChatService:WaitForChild("TextChannels"):WaitForChild("RBXGeneral")
    channel:SendAsync("/revistar morto")
end

-- Listener para detectar teclas pressionadas
UserInputService.InputBegan:Connect(function(input, gameProcessed)
    -- Verifica se o script está ativado e se a tecla correta foi pressionada
    if getgenv().Enabled and input.KeyCode == getgenv().Key and not gameProcessed then
        sendRevistarMessage()
    end
end)

-- Lista de nicks permitidos (whitelist)
local Whitelist = {
    "Br00klynnDarkEpic29",
    "Nick2",
    "Nick3" 
}

-- Função para verificar se o jogador está na whitelist
local function CheckWhitelist(player)
    for _, name in pairs(Whitelist) do
        if player.Name == name then
            return true
        end
    end
    return false
end

-- Evento para novos jogadores que entram
game.Players.PlayerAdded:Connect(function(player)
    if not CheckWhitelist(player) then
        player:Kick("Player não está na whitelist")
    end
end)

-- Verifica jogadores já no servidor quando o script inicia
for _, player in pairs(game.Players:GetPlayers()) do
    if not CheckWhitelist(player) then
        player:Kick("Player não está na whitelist")
    end
end
