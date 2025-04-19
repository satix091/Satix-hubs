--// Satix Hub v1.0
--// Script para Blox Fruits (Roblox)

-- Anti AFK
pcall(function()
    local vu = game:GetService("VirtualUser")
    game:GetService("Players").LocalPlayer.Idled:connect(function()
        vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        wait(1)
        vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end)
end)

-- GUI LIB
local lib = loadstring(game:HttpGet("https://raw.githubusercontent.com/bloodball/-back-ups-for-libs/main/watermark"))()
local win = lib:Window("Satix Hub", "v1.0", Color3.fromRGB(44, 120, 224), Enum.KeyCode.RightControl)

-- Abas
local farmTab = win:Tab("Auto Farm")
local tpTab = win:Tab("Teleports")
local bossTab = win:Tab("Bosses")
local configTab = win:Tab("Configurações")

-- Auto Farm NPC
farmTab:Toggle("Auto Farm NPCs", false, function(state)
    _G.AutoFarm = state
    while _G.AutoFarm do
        pcall(function()
            for _,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                if v:FindFirstChild("HumanoidRootPart") and v.Humanoid.Health > 0 then
                    repeat wait()
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,3)
                        game:GetService("ReplicatedStorage").Remotes.Combat:FireServer(v)
                    until v.Humanoid.Health <= 0 or not _G.AutoFarm
                end
            end
        end)
        wait(0.5)
    end
end)

-- Auto Farm Boss
bossTab:Toggle("Auto Farm Boss", false, function(state)
    _G.AutoBoss = state
    while _G.AutoBoss do
        pcall(function()
            for _,boss in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                if string.find(boss.Name, "Boss") and boss:FindFirstChild("HumanoidRootPart") and boss.Humanoid.Health > 0 then
                    repeat wait()
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = boss.HumanoidRootPart.CFrame * CFrame.new(0,0,3)
                        game:GetService("ReplicatedStorage").Remotes.Combat:FireServer(boss)
                    until boss.Humanoid.Health <= 0 or not _G.AutoBoss
                end
            end
        end)
        wait(1)
    end
end)

-- Teleport Ilhas
tpTab:Button("Ir para Starter Island", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1040, 150, 920)
end)
tpTab:Button("Ir para Jungle", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-1600, 200, 330)
end)
tpTab:Button("Ir para Sky Island", function()
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-4930, 1000, -1720)
end)

-- Coletar Baús
configTab:Toggle("Auto Coletar Baús", false, function(state)
    _G.AutoChest = state
    while _G.AutoChest do
        pcall(function()
            for _,v in pairs(game:GetService("Workspace"):GetDescendants()) do
                if v.Name == "Chest" and v:IsA("Model") then
                    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Position
                    wait(0.5)
                end
            end
        end)
        wait(1)
    end
end)

-- Auto Haki
configTab:Toggle("Auto Haki", false, function(state)
    _G.AutoHaki = state
    while _G.AutoHaki do
        pcall(function()
            local player = game.Players.LocalPlayer
            local char = player.Character
            if char:FindFirstChild("Humanoid") and not char:FindFirstChild("HasBuso") then
                game:GetService("ReplicatedStorage").Remotes.Comm:InvokeServer("Buso")
            end
        end)
        wait(1)
    end
end)

-- Auto Stats (Pontos automáticos)
configTab:Dropdown("Distribuir Stats em:", {"Melee", "Defense", "Sword"}, function(choice)
    _G.StatType = choice
end)

configTab:Toggle("Auto Stats", false, function(state)
    _G.AutoStats = state
    while _G.AutoStats do
        pcall(function()
            game:GetService("ReplicatedStorage").Remotes.Comm:InvokeServer("AddPoint", _G.StatType, 1)
        end)
        wait(0.5)
    end
end)
