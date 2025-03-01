local function teleportTo(position)
    game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = position
    wait(1)
end

local function attackNPC(npc)
    if npc and npc:FindFirstChild("HumanoidRootPart") then
        repeat
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = npc.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
            wait(0.2)
            game:service("VirtualUser"):CaptureController()
            game:service("VirtualUser"):Button1Down(Vector2.new(0, 0), workspace.CurrentCamera.CFrame)
        until npc.Humanoid.Health <= 0 or not npc.Parent
    end
end

-- **1️⃣ FARMAR 30 NPCs DA ELITE PARA PEGAR YAMA**  
local function farmEliteNPCs()
    local count = 0
    while count < 30 do
        for _, npc in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
            if npc:IsA("Model") and npc:FindFirstChild("Humanoid") then
                attackNPC(npc)
                count = count + 1
                wait(0.5)
                if count >= 30 then break end
            end
        end
    end
end

-- **2️⃣ PEGAR A YAMA APÓS DERROTAR OS 30 NPCs**  
local function getYama()
    local yamaDoor = CFrame.new(5400, 28, -650) -- Coordenadas da Yama
    teleportTo(yamaDoor)
    wait(2)
    game.ReplicatedStorage.Remotes:FindFirstChild("Interact"):InvokeServer("Yama")
end

-- **3️⃣ DERROTAR LONGMA PARA PEGAR A TUSHITA**  
local function findBoss(name)
    for _, v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
        if v:IsA("Model") and v:FindFirstChild("Humanoid") and v:FindFirstChild("HumanoidRootPart") then
            if v.Name:lower():find(name:lower()) then
                return v
            end
        end
    end
    return nil
end

local function getTushita()
    local boss = findBoss("Longma")
    if boss then
        attackNPC(boss)
        wait(2)
        -- Coletar Tushita (Longma dropa a Hidden Key)
        game.ReplicatedStorage.Remotes:FindFirstChild("Interact"):InvokeServer("Tushita")
    end
end

-- **4️⃣ PEGAR A CURSED DUAL KATANA (CDK)**
local function getCDK()
    local cdkPosition = CFrame.new(-11840, 928, 5780) -- Coordenadas da CDK
    teleportTo(cdkPosition)
    wait(2)
    game.ReplicatedStorage.Remotes:FindFirstChild("Interact"):InvokeServer("CDK")
end

-- Executar o script completo
farmEliteNPCs()
getYama()
getTushita()
getCDK()
