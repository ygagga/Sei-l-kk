local OrionLib = loadstring(game:HttpGet("https://raw.githubusercontent.com/Sc-Rhyan57/ThisSxHub/refs/heads/main/Library/OrionLibrary.lua"))()

local Window = OrionLib:MakeWindow({
	Name = "Cartux Hub V1 | Brookhaven",
	HidePremium = false,
	SaveConfig = false,
	IntroText = "Cartux Hub V1|Brookhaven",
	IntroIcon = "rbxassetid://2005276185", -- Ícone de chapéu estiloso
	IntroEnabled = true
})

-- Serviços necessários
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RE = ReplicatedStorage:FindFirstChild("RE")
local NameEvent = RE and RE:FindFirstChild("1RPNam1eTex1t")
local ColorEvent = RE and RE:FindFirstChild("1RPNam1eColo1r")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RE = game:GetService("ReplicatedStorage"):WaitForChild("RE")
local TriggerEvent = RE:FindFirstChild("1Playe1rTrigge1rEven1t")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local PlayerName = LocalPlayer.Name

local Executor = identifyexecutor and identifyexecutor() or "Desconhecido"

local CasaID = 0

-- Texto e cores
local NameText = "🧙Cartux Hubi"
local Roxo = Color3.fromRGB(139, 0, 255)  -- Cor roxa

-- Função para atualizar o nome
local function AtualizarNome()
if NameEvent then
NameEvent:FireServer("RolePlayName", NameText)
end
end

-- Função para atualizar a cor
local function AtualizarCor(cor)
if ColorEvent then
ColorEvent:FireServer("PickingRPNameColor", cor)
end
end

-- Executa automaticamente ao iniciar o script
AtualizarNome()   -- Altera o nome para "🧙Cartux Hubi"
AtualizarCor(Roxo) -- Aplica a cor roxa ao nome

local evento = game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r")

-- Adiciona item ao avatar ao executar o script
local function AdicionarItem()
local args = {
[1] = "wear",
[2] = 14987998496
}
evento:FireServer(unpack(args))
end

AdicionarItem()

-- Atualiza a lista de jogadores
local function AtualizarPlayers()
    PlayersList = {}
    for _, v in ipairs(game:GetService("Players"):GetPlayers()) do
        if v ~= game.Players.LocalPlayer then
            table.insert(PlayersList, v.Name)
        end
    end
end

AtualizarPlayers()

local SelectedPlayer = nil

local WelcomeTab = Window:MakeTab({
    Name = "Welcome",
    Icon = "rbxassetid://7734053494",
    PremiumOnly = false
})

WelcomeTab:AddButton({
    Name = "Usuário: " .. PlayerName,
    Callback = function() end
})

-- Botão com nome do executor
WelcomeTab:AddButton({
    Name = "Executor: " .. Executor,
    Callback = function() end
})


-- Créditos da equipe
WelcomeTab:AddButton({
    Name = "Dev: " --666 Mystic
    Callback = function() end
})


-- Aba Troll
local TrollTab = Window:MakeTab({Name = "Kill Player bus", Icon = "rbxassetid://2005276185", Premium = false})

TrollTab:AddDropdown({
    Name = "Select Player",
    Options = PlayersList,
    Callback = function(Value)
        SelectedPlayer = Value
    end
})

TrollTab:AddButton({
    Name = "Atualizar Lista de Jogadores",
    Callback = function()
        AtualizarPlayers()
        OrionLib:MakeNotification({
            Name = "Lista Atualizada",
            Content = "A lista de jogadores foi atualizada!",
            Image = "rbxassetid://4483345998",
            Time = 3
        })
    end
})

-- Spectate Player
local SpectateEnabled = false

TrollTab:AddToggle({
    Name = "Spectate Player",
    Default = false,
    Callback = function(State)
        SpectateEnabled = State
        if SpectateEnabled and SelectedPlayer then
            local Target = game:GetService("Players"):FindFirstChild(SelectedPlayer)
            if Target and Target.Character and Target.Character:FindFirstChild("Humanoid") then
                workspace.CurrentCamera.CameraSubject = Target.Character:FindFirstChild("Humanoid")
            end
        else
            local LocalChar = game.Players.LocalPlayer.Character
            if LocalChar and LocalChar:FindFirstChild("Humanoid") then
                workspace.CurrentCamera.CameraSubject = LocalChar:FindFirstChild("Humanoid")
            end
        end
    end
})

-- Fling Bus (individual)

TrollTab:AddButton({
    Name = "Fling Bus",
    Callback = function()
        if not SelectedPlayer then return end
        local Players = game:GetService("Players")
        local LocalPlayer = Players.LocalPlayer
        local RE = game:GetService("ReplicatedStorage"):WaitForChild("RE")
        local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
        local PosInicial = HumanoidRootPart.Position
        local TriggerEvent = RE:FindFirstChild("1Playe1rTrigge1rEven1t")

        local function SpawnESentar()
            Character:MoveTo(Vector3.new(608.35, 5.34, 265.23))
            task.wait(1)
            RE["1Ca1r"]:FireServer("PickingCar", "SchoolBus")

            local bus
            for i = 1, 20 do
                task.wait(0.2)
                for _, v in ipairs(workspace.Vehicles:GetChildren()) do
                    if v:FindFirstChildWhichIsA("VehicleSeat", true) then
                        local dist = (v:GetPivot().Position - HumanoidRootPart.Position).Magnitude
                        if dist < 30 then
                            bus = v
                            break
                        end
                    end
                end
                if bus then break end
            end

            if not bus then return false end

            local driverSeat
            for _, v in ipairs(bus:GetDescendants()) do
                if v:IsA("VehicleSeat") then
                    driverSeat = v
                    break
                end
            end

            if driverSeat then
                for i = 1, 10 do
                    HumanoidRootPart.CFrame = driverSeat.CFrame + Vector3.new(0, 2, 0)
                    task.wait(0.2)
                    Character.Humanoid.Sit = true
                    task.wait(0.2)
                    if Character.Humanoid.SeatPart == driverSeat then
                        return true
                    end
                end
            end

            return false
        end

        local function FlingBusTarget(targetPlayer)
            if not targetPlayer or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") then return end

            local connection
            local targetHumanoid = targetPlayer.Character:WaitForChild("Humanoid")

            connection = targetHumanoid:GetPropertyChangedSignal("SeatPart"):Connect(function()
                if targetHumanoid.SeatPart and targetHumanoid.SeatPart:IsDescendantOf(workspace.Vehicles) then
                    connection:Disconnect()
                    local voidPos = Vector3.new(9999, -500, 9999)
                    HumanoidRootPart.CFrame = CFrame.new(voidPos)
                    task.wait(0.4)
                    RE["1Ca1r"]:FireServer("DeleteAllVehicles")
                    task.wait(0.4)
                    HumanoidRootPart.CFrame = CFrame.new(PosInicial)

                    OrionLib:MakeNotification({
                        Name = "Fling Realizado",
                        Content = targetPlayer.Name .. " foi pro espaço!",
                        Image = "rbxassetid://4483345998",
                        Time = 4
                    })
                end
            end)

            task.spawn(function()
                while true do
                    if not targetPlayer or not targetPlayer.Character then break end
                    if targetPlayer.Character:FindFirstChild("Humanoid").SeatPart then break end
                    HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(math.random(-3, 3), 0, math.random(-3, 3))
                    task.wait(0.1)
                end
            end)
        end

        if SpawnESentar() then
            local target = Players:FindFirstChild(SelectedPlayer)
            if target then
                FlingBusTarget(target)
            end
        end
    end
})


-- Adicionando Fling Bus ALL

TrollTab:AddButton({
    Name = "Fling Bus ALL",
    Callback = function()
        local Players = game:GetService("Players")
        local LocalPlayer = Players.LocalPlayer
        local RE = game:GetService("ReplicatedStorage"):WaitForChild("RE")
        local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
        local PosInicial = HumanoidRootPart.Position

        -- Função para spawnar e sentar no ônibus
        local function SpawnESentar()
            Character:MoveTo(Vector3.new(608.35, 5.34, 265.23))
            task.wait(1)

            RE["1Ca1r"]:FireServer("PickingCar", "SchoolBus")

            local bus
            for i = 1, 20 do
                task.wait(0.2)
                for _, v in ipairs(workspace.Vehicles:GetChildren()) do
                    if v:FindFirstChildWhichIsA("VehicleSeat", true) then
                        local dist = (v:GetPivot().Position - HumanoidRootPart.Position).Magnitude
                        if dist < 30 then
                            bus = v
                            break
                        end
                    end
                end
                if bus then break end
            end

            if not bus then return false end

            local driverSeat
            for _, v in ipairs(bus:GetDescendants()) do
                if v:IsA("VehicleSeat") then
                    driverSeat = v
                    break
                end
            end

            if driverSeat then
                for i = 1, 10 do
                    HumanoidRootPart.CFrame = driverSeat.CFrame + Vector3.new(0, 2, 0)
                    task.wait(0.2)
                    Character.Humanoid.Sit = true
                    task.wait(0.2)
                    if Character.Humanoid.SeatPart == driverSeat then
                        return true
                    end
                end
            end

            return false
        end

        -- Função para flingar
        local function FlingBusTarget(targetPlayer)
            if not targetPlayer or not targetPlayer.Character or not targetPlayer.Character:FindFirstChild("HumanoidRootPart") then return end

            local connection
            local targetHumanoid = targetPlayer.Character:WaitForChild("Humanoid")

            connection = targetHumanoid:GetPropertyChangedSignal("SeatPart"):Connect(function()
                if targetHumanoid.SeatPart and targetHumanoid.SeatPart:IsDescendantOf(workspace.Vehicles) then
                    connection:Disconnect()

                    local voidPos = Vector3.new(9999, -500, 9999)
                    HumanoidRootPart.CFrame = CFrame.new(voidPos)
                    task.wait(0.4)

                    RE["1Ca1r"]:FireServer("DeleteAllVehicles")
                    task.wait(0.4)

                    HumanoidRootPart.CFrame = CFrame.new(PosInicial)

                    OrionLib:MakeNotification({
                        Name = "Fling Realizado",
                        Content = targetPlayer.Name .. " foi pro espaço!",
                        Image = "rbxassetid://4483345998",
                        Time = 4
                    })
                end
            end)

            task.spawn(function()
                while true do
                    if not targetPlayer or not targetPlayer.Character then break end
                    if targetPlayer.Character:FindFirstChild("Humanoid").SeatPart then break end
                    HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(math.random(-3, 3), 0, math.random(-3, 3))
                    task.wait(0.1)
                end
            end)
        end

        -- Começa o processo para todos
        local allTargets = {}
        for _, player in ipairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                table.insert(allTargets, player)
            end
        end

        task.spawn(function()
            for index, player in ipairs(allTargets) do
                OrionLib:MakeNotification({
                    Name = "Fling Bus All",
                    Content = "Tentando pegar: " .. player.Name,
                    Image = "rbxassetid://4483345998",
                    Time = 2
                })

                local ok = SpawnESentar()
                if not ok then
                    OrionLib:MakeNotification({
                        Name = "Erro",
                        Content = "Não foi possível sentar no ônibus!",
                        Image = "rbxassetid://4483345998",
                        Time = 4
                    })
                    return
                end

                FlingBusTarget(player)
                repeat task.wait() until not player.Character or player.Character:FindFirstChild("Humanoid").SeatPart
                task.wait(2)
            end

            OrionLib:MakeNotification({
                Name = "Concluído",
                Content = "Todos os jogadores foram flingados!",
                Image = "rbxassetid://4483345998",
                Time = 5
            })
        end)
    end
})

TrollTab:AddParagraph("╚═ Fling bus ALL e fling bus só player não estiver sentado!═╝", "Pois como não tem como pegar um player enquanto ele estiver sentado eu fiz isso!")


-- Aba Lag
local LagTab = Window:MakeTab({Name = "Lag Server", Icon = "rbxassetid://2005276185", PremiumOnly = false})

local lagEnabled = false

LagTab:AddToggle({
Name = "Lag Server Computador",
Default = false,
Callback = function(Value)
lagEnabled = Value
if lagEnabled then
task.spawn(function()
local Player = game.Players.LocalPlayer
if Player and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
local Root = Player.Character.HumanoidRootPart
while lagEnabled do
Root.CFrame = CFrame.new(-320.57, 10.26, -108.27)

-- Clicar em itens clicáveis próximos  
                    for _, obj in ipairs(workspace:GetDescendants()) do  
                        if obj:IsA("ClickDetector") then  
                            local parent = obj.Parent  
                            if parent and parent:IsA("Model") and parent:FindFirstChild("PrimaryPart") then  
                                local distance = (parent.PrimaryPart.Position - Vector3.new(-320.57, 10.26, -108.27)).Magnitude  
                                if distance < 10 then -- Ajuste do raio de detecção  
                                    fireclickdetector(obj, 5)  
                                end  
                            elseif parent and parent:IsA("BasePart") then  
                                local distance = (parent.Position - Vector3.new(-320.57, 10.26, -108.27)).Magnitude  
                                if distance < 10 then  
                                    fireclickdetector(obj, 5)  
                                end  
                            end  
                        end  
                    end  
                    task.wait(0.1)  
                end  
            end  
        end)  
    end  
end

})

LagTab:AddToggle({
Name = "Lag Server Book",
Default = false,
Callback = function(Value)
lagEnabled = Value
if lagEnabled then
task.spawn(function()
local Player = game.Players.LocalPlayer
if Player and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
local Root = Player.Character.HumanoidRootPart
while lagEnabled do
Root.CFrame = CFrame.new(-66.74, 36.18, -181.05)

-- Clicar em itens clicáveis próximos  
                    for _, obj in ipairs(workspace:GetDescendants()) do  
                        if obj:IsA("ClickDetector") then  
                            local parent = obj.Parent  
                            if parent and parent:IsA("Model") and parent:FindFirstChild("PrimaryPart") then  
                                local distance = (parent.PrimaryPart.Position - Vector3.new(-320.57, 10.26, -108.27)).Magnitude  
                                if distance < 10 then -- Ajuste do raio de detecção  
                                    fireclickdetector(obj, 5)  
                                end  
                            elseif parent and parent:IsA("BasePart") then  
                                local distance = (parent.Position - Vector3.new(-66.74, 36.18, -181.05)).Magnitude  
                                if distance < 10 then  
                                    fireclickdetector(obj, 5)  
                                end  
                            end  
                        end  
                    end  
                    task.wait(0.1)  
                end  
            end  
        end)  
    end  
end

})

-- Aba ESP
local ESPTab = Window:MakeTab({Name = "ESP Red Black", Icon = "rbxassetid://2005276185", PremiumOnly = false})

local ESPAtivo = false
local ESPFolder = Instance.new("Folder", game.CoreGui)

local function CriarESP(Player)
if not Player.Character or not Player.Character:FindFirstChild("HumanoidRootPart") then return end
local RootPart = Player.Character.HumanoidRootPart

local Box = Instance.new("BoxHandleAdornment")  
Box.Adornee = RootPart  
Box.Size = RootPart.Size + Vector3.new(1, 1, 1)  
Box.Color3 = Color3.fromRGB(255, 0, 0)  
Box.Transparency = 0.5  
Box.AlwaysOnTop = true  
Box.ZIndex = 5  
Box.Parent = ESPFolder  

local Billboard = Instance.new("BillboardGui")  
Billboard.Adornee = RootPart  
Billboard.Size = UDim2.new(2, 0, 1, 0)  
Billboard.StudsOffset = Vector3.new(0, 3, 0)  
Billboard.AlwaysOnTop = true  
Billboard.Parent = ESPFolder  

local TextLabel = Instance.new("TextLabel")  
TextLabel.Size = UDim2.new(1, 0, 1, 0)  
TextLabel.TextColor3 = Color3.fromRGB(255, 0, 0)  
TextLabel.TextScaled = true  
TextLabel.BackgroundTransparency = 1  
TextLabel.Parent = Billboard  

task.spawn(function()  
    while ESPAtivo and Player and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") do  
        local Distancia = math.floor((RootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude)  
        TextLabel.Text = Player.Name .. "\n" .. Distancia .. "m"  
        task.wait(0.1)  
    end  
    Box:Destroy()  
    Billboard:Destroy()  
end)

end

ESPTab:AddToggle({
Name = "Esp Padrão",
Default = false,
Callback = function(Value)
ESPAtivo = Value
if ESPAtivo then
for _, Player in ipairs(game.Players:GetPlayers()) do
if Player ~= game.Players.LocalPlayer then
CriarESP(Player)
end
end
game.Players.PlayerAdded:Connect(function(Player)
if ESPAtivo then
CriarESP(Player)
end
end)
else
ESPFolder:ClearAllChildren()
end
end
})

-- Red Black ESP
local RunService = game:GetService("RunService")
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local RedBlackESP_Enabled = false
local ESPObjects = {}

-- Criar ESP para um player
local function CreateESP(Player)
    if Player == LocalPlayer then return end
    if ESPObjects[Player] then return end

    local Billboard = Instance.new("BillboardGui")
    Billboard.Name = "RedBlackESP"
    Billboard.Size = UDim2.new(0, 100, 0, 40)
    Billboard.StudsOffset = Vector3.new(0, 3, 0)
    Billboard.AlwaysOnTop = true

    local Label = Instance.new("TextLabel")
    Label.Size = UDim2.new(1, 0, 1, 0)
    Label.BackgroundTransparency = 1
    Label.TextColor3 = Color3.fromRGB(255, 0, 0)
    Label.TextStrokeTransparency = 0.5
    Label.TextScaled = true
    Label.Font = Enum.Font.SourceSans
    Label.Text = ""
    Billboard.Parent = nil
    Label.Parent = Billboard

    local Highlight = Instance.new("Highlight")
    Highlight.FillColor = Color3.fromRGB(255, 0, 0)
    Highlight.OutlineTransparency = 1
    Highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    Highlight.Enabled = true
    Highlight.Parent = nil

    ESPObjects[Player] = {
        Billboard = Billboard,
        Label = Label,
        Highlight = Highlight
    }
end

-- Atualizar ESP
local function UpdateESP()
    for _, Player in ipairs(Players:GetPlayers()) do
        if Player ~= LocalPlayer and Player.Character and Player.Character:FindFirstChild("HumanoidRootPart") then
            if not ESPObjects[Player] then
                CreateESP(Player)
            end

            local esp = ESPObjects[Player]
            esp.Billboard.Parent = Player.Character:FindFirstChild("Head") or Player.Character:FindFirstChild("HumanoidRootPart")
            esp.Highlight.Parent = Player.Character
            esp.Highlight.Adornee = Player.Character

            local Distance = math.floor((LocalPlayer.Character.HumanoidRootPart.Position - Player.Character.HumanoidRootPart.Position).Magnitude)
            esp.Label.Text = Player.Name .. " [" .. Distance .. "m]"
        end
    end
end

-- Desativar ESP
local function ClearESP()
    for _, esp in pairs(ESPObjects) do
        if esp.Billboard then esp.Billboard:Destroy() end
        if esp.Highlight then esp.Highlight:Destroy() end
    end
    ESPObjects = {}
end

-- Toggle ESP
ESPTab:AddToggle({
    Name = "ESP 2.0",
    Default = false,
    Callback = function(Value)
        RedBlackESP_Enabled = Value
        if not Value then
            ClearESP()
        end
    end
})

-- Loop de atualização
RunService.RenderStepped:Connect(function()
    if RedBlackESP_Enabled then
        UpdateESP()
    end
end)

-- Aba de Créditos Supremos
local CasaTab = Window:MakeTab({
Name = "Casa",
Icon = "rbxassetid://2005276185",
PremiumOnly = false
})

CasaTab:AddTextbox({
    Name = "Digite o Número da Casa",
    Default = "",
    TextDisappear = true,
    Callback = function(Value)
        CasaID = tonumber(Value)
    end
})

CasaTab:AddButton({
    Name = "Pegar Permissão da Casa",
    Callback = function()
        for _, v in pairs(Players:GetPlayers()) do
            local args = {
                [1] = "GivePermissionLoopToServer",
                [2] = LocalPlayer,
                [3] = CasaID
            }
            TriggerEvent:FireServer(unpack(args))
        end

        OrionLib:MakeNotification({
            Name = "Sucesso",
            Content = "Tentando pegar permissão da casa ID: " .. CasaID,
            Time = 3
        })
    end
})

CasaTab:AddButton({
    Name = "Pegar Permissão de Todas as Casas",
    Callback = function()
        for casaID = 1, 36 do
            local args = {
                [1] = "GivePermissionLoopToServer",
                [2] = LocalPlayer,
                [3] = casaID
            }
            TriggerEvent:FireServer(unpack(args))
            wait(0.1) -- Pequeno delay pra não crashar
        end

        OrionLib:MakeNotification({
            Name = "Sucesso",
            Content = "Tentando pegar permissão das casas!",
            Time = 4
        })
    end
})

CasaTab:AddParagraph("╚══『Não Clique muita vezes risco de kick』══╝", "Botão:Pegar permissão de todas as casas.")

-- Aba Avatar
local AvatarTab = Window:MakeTab({
    Name = "Avatar",
    Icon = "rbxassetid://2005276185",
    PremiumOnly = false
})

-- Lista de Avatares
local Avatares = {
    ["corpo de tartaruga sei la"] = {
        3657481497,
        3657478273,
        3657474549,
        3657479706,
        3745126840,
        1
    },
    ["Chicken"] = {
        128157408,
        128157262,
        128157213,
        128157361,
        128157317,
        1
    },
    ["Batata"] = {
        5392155773,
        5392150804,
        5392146467,
        5392152751,
        5392148570,
        1
    }
}

-- Dropdown para selecionar avatar
AvatarTab:AddDropdown({
    Name = "Selecionar Avatar",
    Default = "",
    Options = {"corpo de tartaruga sei la", "Chicken", "Batata"},
    Callback = function(avatarSelecionado)
        local args = {
            [1] = "CharacterChange",
            [2] = Avatares[avatarSelecionado],
            [3] = "by:REDz"
        }

        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Avata1rOrigina1l"):FireServer(unpack(args))

        OrionLib:MakeNotification({
            Name = "Avatar Alterado",
            Content = "Você virou: " .. avatarSelecionado,
            Time = 3
        })
    end
})

AvatarTab:AddDropdown({
    Name = "Selecionar Valkyrie",
    Default = "Escolha uma Valk",
    Options = {
        "Valkyrie Azul",
        "Valkyrie Vermelha",
        "Valkyrie Branca"
    },
    Callback = function(selected)
        local valkIds = {
            ["Valkyrie Azul"] = 1180433861,
            ["Valkyrie Vermelha"] = 2830437685,
            ["Valkyrie Branca"] = 17517206952
        }

        local id = valkIds[selected]
        if id then
            local args = {
                [1] = "wear",
                [2] = id
            }

            game:GetService("ReplicatedStorage").RE:FindFirstChild("1Updat1eAvata1r"):FireServer(unpack(args))
        end
    end
})
