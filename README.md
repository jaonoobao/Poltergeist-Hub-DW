local Poltergeist = loadstring(game:HttpGet("https://raw.githubusercontent.com/berhddb/Library/refs/heads/main/Main", true))()

-- Create main window
local Window = Poltergeist:CreateWindow({
    Name = "Poltergeist Hub",
    Subtitle = "Universal",
    LogoID = "101636243364181",
    LoadingEnabled = true,
    LoadingTitle = "Poltergeist Hub", 
    LoadingSubtitle = "V1.0 (BETA)",
    ConfigSettings = {
        RootFolder = "PoltergeistHubDW",
        ConfigFolder = "PoltergeistHubUniversal"
    },
    KeySystem = false
})

-- Create main tab
local Tab = Window:CreateHomeTab({
    Icon = 1,
    SupportedExecutors = {"Xeno", "Sonar", "Solara", "Swift", "Argon"},
    DiscordInvite = "noinvitelink"
})

-- Create General tab
local GeneralTab = Window:CreateTab({
    Name = "General",
    Icon = "laptop",
    ImageSource = "Material",
    ShowTitle = true
})

GeneralTab:CreateSection("ESP Systems")

-- Generator ESP System
local GeneratorESP = false
local GeneratorESPToggle = GeneralTab:CreateToggle({
    Name = "🔧 Generator ESP",
    Description = "Highlights incomplete generators",
    CurrentValue = false,
    Callback = function(v)
        GeneratorESP = v
        if GeneratorESP then
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Generator ESP has been activated"
		})
            coroutine.wrap(function()
                while GeneratorESP do
                    UpdateGeneratorESP()
                    wait(1) -- Update every 1 second
                end
            end)()
        else
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Generator ESP has been deactivated"
		})
            CleanUpGeneratorESP()
        end
    end
})

function UpdateGeneratorESP()
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("Model") and obj.Name == "Generator" then
            pcall(function()
                local stats = obj:FindFirstChild("Stats")
                if stats and stats:FindFirstChild("Completed") then
                    if not stats.Completed.Value then
                        if not obj:FindFirstChild("GeneratorHighlight") then
                            local highlight = Instance.new("Highlight")
                            highlight.Name = "GeneratorHighlight"
                            highlight.FillColor = Color3.fromRGB(0, 255, 0)
                            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                            highlight.Parent = obj
                            
                            -- Add distance label
                            local billboard = Instance.new("BillboardGui", obj)
                            billboard.Name = "GeneratorESP"
                            billboard.Size = UDim2.new(1, 200, 1, 30)
                            billboard.AlwaysOnTop = true
                            billboard.Adornee = obj:FindFirstChild("Main") or obj.PrimaryPart
                            billboard.ExtentsOffset = Vector3.new(0, 2, 0)
                            
                            local label = Instance.new("TextLabel", billboard)
                            label.Text = "Generator"
                            label.Size = UDim2.new(1, 0, 1, 0)
                            label.Font = Enum.Font.SourceSansBold
                            label.TextSize = 12
                            label.TextColor3 = Color3.fromRGB(255, 255, 255)
                            label.BackgroundTransparency = 1
                        end
                    else
                        CleanUpGeneratorESP(obj)
                    end
                end
            end)
        end
    end
    
    -- Update distances
    if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Head") then
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Model") and obj.Name == "Generator" and obj:FindFirstChild("GeneratorESP") then
                pcall(function()
                    local root = obj:FindFirstChild("Main") or obj.PrimaryPart
                    if root then
                        local dist = (game.Players.LocalPlayer.Character.Head.Position - root.Position).Magnitude
                        obj.GeneratorESP.TextLabel.Text = string.format("Generator [%.1fm]", dist)
                    end
                end)
            end
        end
    end
end

function CleanUpGeneratorESP(specificObj)
    if specificObj then
        local highlight = specificObj:FindFirstChild("GeneratorHighlight")
        if highlight then highlight:Destroy() end
        local esp = specificObj:FindFirstChild("GeneratorESP")
        if esp then esp:Destroy() end
    else
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Model") and obj.Name == "Generator" then
                local highlight = obj:FindFirstChild("GeneratorHighlight")
                if highlight then highlight:Destroy() end
                local esp = obj:FindFirstChild("GeneratorESP")
                if esp then esp:Destroy() end
            end
        end
    end
end

-- Twisted ESP System
local TwistedESP = false
local TwistedESPToggle = GeneralTab:CreateToggle({
    Name = "🩸 Twisted ESP",
    Description = "Highlights Twisteds with a red glow",
    CurrentValue = false,
    Callback = function(v)
        TwistedESP = v
        if TwistedESP then
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Twisters ESP has been activated"
		})
            coroutine.wrap(function()
                while TwistedESP do
                    UpdateTwistedESP()
                    wait(1) -- More frequent updates for enemies
                end
            end)()
        else
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Twisters ESP has been deactivated"
		})
            CleanUpTwistedESP()
        end
    end
})

function UpdateTwistedESP()
    for _, obj in ipairs(workspace:GetDescendants()) do
        if obj:IsA("Model") and obj.Parent.Name == "Monsters" then
            pcall(function()
                if not obj:FindFirstChild("TwistedHighlight") then
                    -- Create highlight
                    local highlight = Instance.new("Highlight")
                    highlight.Name = "TwistedHighlight"
                    highlight.FillColor = Color3.fromRGB(255, 0, 0)
                    highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                    highlight.FillTransparency = 0.4
                    highlight.Parent = obj
                    
                    -- Create ESP label
                    local billboard = Instance.new("BillboardGui", obj)
                    billboard.Name = "TwistedESP"
                    billboard.Size = UDim2.new(1, 200, 1, 30)
                    billboard.AlwaysOnTop = true
                    billboard.Adornee = obj:FindFirstChild("HumanoidRootPart") or obj.PrimaryPart
                    billboard.ExtentsOffset = Vector3.new(0, 3, 0)
                    
                    local label = Instance.new("TextLabel", billboard)
                    label.Text = obj.Name
                    label.Size = UDim2.new(1, 0, 1, 0)
                    label.Font = Enum.Font.SourceSansBold
                    label.TextSize = 14
                    label.TextColor3 = Color3.fromRGB(255, 50, 50)
                    label.BackgroundTransparency = 1
                end
            end)
        end
    end
    
    -- Update distances
    if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Head") then
        for _, obj in ipairs(workspace:GetDescendants()) do
            if obj:IsA("Model") and obj:FindFirstChild("TwistedESP") then
                pcall(function()
                    local root = obj:FindFirstChild("HumanoidRootPart") or obj.PrimaryPart
                    if root then
                        local dist = (game.Players.LocalPlayer.Character.Head.Position - root.Position).Magnitude
                        obj.TwistedESP.TextLabel.Text = string.format(obj.name.." [%.1fm]", dist)
                    end
                end)
            end
        end
    end
end

function CleanUpTwistedESP()
    for _, obj in ipairs(workspace:GetDescendants()) do
        local highlight = obj:FindFirstChild("TwistedHighlight")
        if highlight then highlight:Destroy() end
        local esp = obj:FindFirstChild("TwistedESP")
        if esp then esp:Destroy() end
    end
end

-- Item ESP System
local ItemESP = false
local itemCache = {}

local function FindItemsFolder()
    if workspace:FindFirstChild("CurrentRoom") then
        for _, child in ipairs(workspace.CurrentRoom:GetDescendants()) do
            if child.Name == "Items" and child:IsA("Folder") then
                return child
            end
        end
    end
    return nil
end

local ItemEspToggle = GeneralTab:CreateToggle({
    Name = "🔍 Item ESP",
    Description = "Highlights items",
    CurrentValue = false,
    Callback = function(v)
        ItemESP = v
        if ItemESP then
			-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Item ESP has been activated"
		})
            coroutine.wrap(function()
                while ItemESP do
                    UpdateItemESP()
                    wait(1) -- Balanced update rate
                end
            end)()
        else
            CleanUpItemESP()
			-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Item ESP has been deactivated"
		})
        end
    end
})

function UpdateItemESP()
    local itemsFolder = FindItemsFolder()
    if not itemsFolder then return end

    -- Clean up removed items
    for item, _ in pairs(itemCache) do
        if not item.Parent then
            itemCache[item] = nil
        end
    end

    -- Find new items
    for _, obj in ipairs(itemsFolder:GetChildren()) do
        if obj:IsA("Model") and obj:FindFirstChild("Prompt") and not itemCache[obj] then
            pcall(function()
                -- Create highlight
                local highlight = Instance.new("Highlight")
                highlight.Name = "ItemHighlight"
                highlight.FillColor = Color3.fromRGB(255, 215, 0) -- Gold
                highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
                highlight.FillTransparency = 0.3
                highlight.Parent = obj
                
                -- Create ESP label
                local billboard = Instance.new("BillboardGui", obj)
                billboard.Name = "ItemESP"
                billboard.Size = UDim2.new(1, 200, 1, 30)
                billboard.AlwaysOnTop = true
                billboard.Adornee = obj:FindFirstChild("Main") or obj.PrimaryPart
                billboard.ExtentsOffset = Vector3.new(0, 2, 0)
                
                local label = Instance.new("TextLabel", billboard)
                label.Text = obj.Name
                label.Size = UDim2.new(1, 0, 1, 0)
                label.Font = Enum.Font.SourceSansBold
                label.TextSize = 12
                label.TextColor3 = Color3.fromRGB(255, 215, 0)
                label.BackgroundTransparency = 1
                
                itemCache[obj] = true
            end)
        end
    end
    
    -- Update distances
    if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Head") then
        for obj, _ in pairs(itemCache) do
            if obj.Parent and obj:FindFirstChild("ItemESP") then
                pcall(function()
                    local root = obj:FindFirstChild("Main") or obj.PrimaryPart
                    if root then
                        local dist = (game.Players.LocalPlayer.Character.Head.Position - root.Position).Magnitude
                        obj.ItemESP.TextLabel.Text = string.format("%s [%.1fm]", obj.Name, dist)
                    end
                end)
            end
        end
    end
end

function CleanUpItemESP()
    for obj, _ in pairs(itemCache) do
        if obj.Parent then
            local highlight = obj:FindFirstChild("ItemHighlight")
            if highlight then highlight:Destroy() end
            local esp = obj:FindFirstChild("ItemESP")
            if esp then esp:Destroy() end
        end
    end
    itemCache = {}
end

-- Auto Skill Check Button
GeneralTab:CreateSection("Others")

local ASCValue = false

local AutoSkillCheck = GeneralTab:CreateButton({
	Name = "🤹‍♀️ Auto Skill Check",
	Description = "Automatically completes skill checks (the skill check dont appear)",
    	Callback = function()
		if ASCValue == false then
		ASCValue = true
         game.ReplicatedStorage.Events.SkillcheckUpdate.OnClientInvoke = function() return "supercomplete" end
		 -- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Auto Skill Check has been activated"
		})
		else
		 -- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Auto Skill Check is aleardy activated"
		})
		end
    	end
})

local FBValue = false

local FullBright = GeneralTab:CreateButton({
	Name = "💡 Fullbright",
	Description = "Remove game darkness",
    	Callback = function()
		if FBValue == false then
		FBValue = true
		local Lighting = game:GetService("Lighting")
        Lighting.Brightness = 2
    	Lighting.ClockTime = 14
    	Lighting.FogEnd = 100000
    	Lighting.GlobalShadows = false
    	Lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Fullbright has been activated"
		})
		else
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Fullbright is aleardy activated"
		})
		end
    	end
})

local AutoGTE = false
local AutoGTEToggle = GeneralTab:CreateToggle({
    Name = "Auto GTE",
    Description = "Automatically teleports you to elevator when panic mode",
    CurrentValue = false,
    Callback = function(v)
        AutoGTE = v
        if AutoGTE then
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Auto GTE has been activated"
		})
            coroutine.wrap(function()
                while AutoGTE do
                    UpdateAutoGTE()
                    wait(1) -- Update every 1 second
                end
            end)()
        else
		-- Notification
		Poltergeist:Notification({ 
		Title = "Notification",
		Icon = "notifications_active",
		ImageSource = "Material",
		Content = "Auto GTE has been deactivated"
		})
        end
    end
})

function UpdateAutoGTE()
if workspace.Info.Panic.Value == true then
	local char = workspace.InGamePlayers:WaitForChild(game.Players.LocalPlayer.Name)
	char.HumanoidRootPart.Position = game.workspace.Elevators.Elevator.Base.Position
end



-- Add this section to your existing script (place it with the other ESP systems)
GeneralTab:CreateSection("Movement")

-- Teleport Walk Variables (must be declared BEFORE the toggle/slider)
local tpWalking = false
local localPlayer = game:GetService("Players").LocalPlayer

-- Teleport Walk Functions (must be declared BEFORE the toggle/slider)
local function StartTeleportWalk(speedMultiplier)
    tpWalking = true
    
    coroutine.wrap(function()
        local character = localPlayer.Character or localPlayer.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        
        while tpWalking and character and humanoid and humanoid.Parent do
            local delta = game:GetService("RunService").Heartbeat:Wait()
            if humanoid.MoveDirection.Magnitude > 0 then
                character:TranslateBy(humanoid.MoveDirection * speedMultiplier * delta * 10)
            end
        end
    end)()
end

local function StopTeleportWalk()
    tpWalking = false
end

-- Now create the slider (needs to be before the toggle since toggle references it)
local TpWalkSpeedSlider = GeneralTab:CreateSlider({
    Name = "Teleport Walk Speed",
    Description = "Adjust movement speed multiplier",
    Range = {0.1, 1.5},
    Increment = 0.1,
    CurrentValue = 0.5,
    Callback = function(Value)
        if TpWalkToggle and TpWalkToggle.CurrentValue then
            StopTeleportWalk()
            StartTeleportWalk(Value)
        end
    end
}, "TeleportWalkSpeed")

-- Finally create the toggle
local TpWalkToggle = GeneralTab:CreateToggle({
    Name = "🚀 Teleport Walk",
    Description = "Enhanced movement speed",
    CurrentValue = false,
    Callback = function(Value)
        if Value then
            -- Notification
            Poltergeist:Notification({
                Title = "Notification",
                Icon = "notifications_active",
                ImageSource = "Material",
                Content = "Teleport Walk activated"
            })
            StartTeleportWalk(TpWalkSpeedSlider.CurrentValue)
        else
            -- Notification
            Poltergeist:Notification({
                Title = "Notification",
                Icon = "notifications_active",
                ImageSource = "Material",
                Content = "Teleport Walk deactivated"
            })
            StopTeleportWalk()
        end
    end
}, "TeleportWalkToggle")

-- Handle character respawns
localPlayer.CharacterAdded:Connect(function()
    if TpWalkToggle and TpWalkToggle.CurrentValue then
        StopTeleportWalk()
        wait(0.5) -- Small delay to ensure character is fully loaded
        StartTeleportWalk(TpWalkSpeedSlider.CurrentValue)
    end
end)
end
