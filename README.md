-- Created By Nyok, Discord: discord.gg/zSnpS53az5

getgenv().autoFarm = true
getgenv().autoPlaneName = "Cessna Skyhawk" -- Case Sensitive // THIS IS NOT NEEDED BUT IF THE PLANE DESPAWNS THISLL HELP

local TweenService = game:GetService("TweenService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local plr = game:GetService("Players").LocalPlayer
local tInfo = TweenInfo.new(0, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut, 0, false, 0)
local function completeTasks(v, b)
    if b.Name == "Destination" then
        local tValue = Instance.new("CFrameValue")
        tValue.Value = v:GetPrimaryPartCFrame()

        tValue.Changed:Connect(function()
            v:SetPrimaryPartCFrame(tValue.Value)
        end)

        local cTween = TweenService:Create(tValue,tInfo,{Value = workspace.Destinations[b.Value].CFrame})
        cTween:Play()
        cTween.Completed:Wait()
        wait(0.5)
    elseif b.Name == "Airport" then
        local tValue = Instance.new("CFrameValue")
        tValue.Value = v:GetPrimaryPartCFrame()

        tValue.Changed:Connect(function()
            v:SetPrimaryPartCFrame(tValue.Value)
        end)
        local cTween = TweenService:Create(tValue,tInfo,{Value = workspace.AirportDestinations[b.Value].CFrame})
        cTween:Play()
        cTween.Completed:Wait()
        wait(0.5)
    elseif b.Name == "Goal" and b.Parent.Type.Value == "Fly distance" then
        local tValue = Instance.new("CFrameValue")
        tValue.Value = v:GetPrimaryPartCFrame()

        tValue.Changed:Connect(function()
            v:SetPrimaryPartCFrame(tValue.Value)
        end)

        local OnTween = TweenService:Create(tValue,tInfo,{Value = v.PrimaryPart.CFrame + Vector3.new(100000, 100000, 0)})
        OnTween:Play()
        OnTween.Completed:Wait()
        wait(0.5)
    end
end
local spPlanes = {}
local spawningPlane = false
while wait(1) do
    if autoFarm then  
        for i, v in pairs(game:GetService("Workspace").SpawnedVehicles:GetChildren()) do
            if v:GetAttribute("Owner") then
                spPlanes[plr.Name] = v
            end
        end
        if spPlanes[plr.Name] and plr:DistanceFromCharacter(spPlanes[plr.Name].PrimaryPart.Parent.MainSeat.Position) < 10 then
            local plPlane = spPlanes[plr.Name]
            for a, b in pairs(plr.Tasks:GetDescendants()) do
                completeTasks(plPlane, b)
            end
            spPlanes = {}
        else
            print("a")
            if not spawningPlane then
                spawningPlane = true
                ReplicatedStorage.SpawnVehicle:FireServer(autoPlaneName, workspace.Spawners)
                wait(2)
                spPlanes = {}
                spawningPlane = false
            end
        end
    end
end
