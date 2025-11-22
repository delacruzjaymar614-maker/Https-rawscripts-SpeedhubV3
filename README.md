
Rayfield:LoadConfiguration({
    LoadingTitle = "99 Nights Menu",
    LoadingSubtitle = "Game Utility | Loading...",
    LoadingImage = nil, -- Optional: url to a thumbnail/logo
    ConfigurationSaving = {
        Enabled = false
    }
})

task.wait(1) -- Simulate loading effect, increase if needed

local Window = Rayfield:CreateWindow({
    Name = "99 Nights Utility",
    LoadingTitle = "99 Nights Menu",
    LoadingSubtitle = "by Copilot",
    ConfigurationSaving = {
        Enabled = false
    }
})

-- Utility features here -- just paste your main script below from 'local function bringKids()...'

-- Bring Kids
local function bringKids()
    for _, kid in ipairs(workspace:GetChildren()) do
        if kid.Name:lower():find("kid") and kid:FindFirstChild("HumanoidRootPart") then
            kid.HumanoidRootPart.CFrame = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame + Vector3.new(math.random(-3,3),0,math.random(-3,3))
        end
    end
end

-- VFX (Shader)
local function addVFX()
    local lighting = game:GetService("Lighting")
    local colorCorrection = Instance.new("ColorCorrectionEffect")
    colorCorrection.Saturation = 0.3
    colorCorrection.Contrast = 0.5
    colorCorrection.TintColor = Color3.fromRGB(170, 225, 255)
    colorCorrection.Parent = lighting
    
    local bloom = Instance.new("BloomEffect")
    bloom.Intensity = 2
    bloom.Size = 24
    bloom.Threshold = 1
    bloom.Parent = lighting
end

-- Auto Save
local function autoSave()
    while getgenv().autoSaveActive do
        pcall(function()
            game.ReplicatedStorage.Remotes.Save:FireServer()
        end)
        wait(60)
    end
end

-- Auto Open Chest
local function autoOpenChest()
    while getgenv().autoChestActive do
        for _, v in ipairs(workspace:GetChildren()) do
            if v.Name:lower():find("chest") and v:FindFirstChild("Open") then
                game.ReplicatedStorage.Remotes.OpenChest:FireServer(v)
            end
        end
        wait(3)
    end
end

-- ESP
local function espAll()
    for _, v in ipairs(workspace:GetDescendants()) do
        if v:IsA("BasePart") and v.Transparency < 1 then
            local highlight = Instance.new("Highlight")
            highlight.Adornee = v
            highlight.FillColor = Color3.fromRGB(0,255,0)
            highlight.OutlineColor = Color3.fromRGB(255,255,0)
            highlight.Parent = v
        end
    end
end

-- Kill Aura
local function killAura()
    while getgenv().killAuraActive do
        for _, obj in ipairs(workspace:GetChildren()) do
            if obj:FindFirstChild("Humanoid") and obj ~= game.Players.LocalPlayer.Character then
                if (obj.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude < 10 then
                    game.ReplicatedStorage.Remotes.Damage:FireServer(obj, 1e5)
                end
            end
        end
        wait(1)
    end
end

-- Tree Aura
local function treeAura()
    while getgenv().treeAuraActive do
        for _, obj in ipairs(workspace:GetChildren()) do
            if obj.Name:lower():find("tree") then
                game.ReplicatedStorage.Remotes.ChopTree:FireServer(obj)
            end
        end
        wait(1)
    end
end

-- Infinite Saplings
local function infiniteSaplings()
    while getgenv().infSaplingsActive do
        game.ReplicatedStorage.Remotes.GiveSapling:FireServer()
        wait(1)
    end
end

-- UI Controls
Window:CreateButton({
    Name = "Bring Kids",
    Callback = bringKids
})

Window:CreateButton({
    Name = "Add Shader/VFX",
    Callback = addVFX
})

Window:CreateToggle({
    Name = "Auto Save",
    CurrentValue = false,
    Callback = function(state)
        getgenv().autoSaveActive = state
        if state then autoSave() end
    end
})

Window:CreateToggle({
    Name = "Auto Open Chest",
    CurrentValue = false,
    Callback = function(state)
        getgenv().autoChestActive = state
        if state then autoOpenChest() end
    end
})

Window:CreateButton({
    Name = "ESP All Parts",
    Callback = espAll
})

Window:CreateToggle({
    Name = "Kill Aura",
    CurrentValue = false,
    Callback = function(state)
        getgenv().killAuraActive = state
        if state then killAura() end
    end
})

Window:CreateToggle({
    Name = "Tree Aura",
    CurrentValue = false,
    Callback = function(state)
        getgenv().treeAuraActive = state
        if state then treeAura() end
    end
})

Window:CreateToggle({
    Name = "Infinite Saplings",
    CurrentValue = false,
    Callback = function(state)
        getgenv().infSaplingsActive = state
        if state then infiniteSaplings() end
    end
})

Rayfield:Notify({
    Title = "99 Nights Utility",
    Content = "Script Loaded! Toggle what you need.",
    Duration = 5
})

