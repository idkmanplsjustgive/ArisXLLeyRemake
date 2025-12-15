# ArisXLLeyRemake



FREE SOURCE:


IG FREE SOURCE TO USE, ENJOY SKIDS!:

-------------------------------------------------
-- SERVICES
-------------------------------------------------
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")
local Lighting = game:GetService("Lighting")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer

-------------------------------------------------
-- NOTIFICATION HELPER
-------------------------------------------------
local function Notify(title, text)
    StarterGui:SetCore("SendNotification",{Title=title, Text=text})
end

-------------------------------------------------
-- EMBEDDED OBSIDIAN LIBRARY
-------------------------------------------------
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/deividcomsono/Obsidian/refs/heads/main/Library.lua"))()

-------------------------------------------------
-- FEATURES SETUP
-------------------------------------------------
local function SetupAllFeatures(Lib)
    -- MAIN TAB
    local Tab = Lib:CreateTab("Imposters Main", Color3.fromRGB(128,0,128))

    -- OP SECTION
    Tab:CreateSection("OP")

    local NoclipEnabled = false
    Tab:CreateButton("Noclip ON/OFF", function()
        NoclipEnabled = not NoclipEnabled
        Notify("Noclip","Noclip is now "..tostring(NoclipEnabled))
    end)

    local TrollEnabled = false
    local TrollWindow
    Tab:CreateButton("Troll ON/OFF", function()
        TrollEnabled = not TrollEnabled
        Notify("Troll","WARNING! dont use with NOCLIP ENABLED!")
        if TrollEnabled then
            TrollWindow = Lib:CreateWindow("Troll",Color3.fromRGB(128,0,128))
            TrollWindow:CreateButton("TROLL", function()
                game.ReplicatedStorage.RemoteEvents.DebugUseWeapon:InvokeServer(workspace.Debug.Dummy)
            end)
        else
            if TrollWindow then TrollWindow:Destroy() end
        end
    end)

    -- MOVEMENTS SECTION
    Tab:CreateSection("MOVEMENTS")

    local WalkSpeedEnabled=false
    local WalkSpeedValue=100
    local WalkWindow
    Tab:CreateButton("WalkSpeed ON/OFF",function()
        WalkSpeedEnabled = not WalkSpeedEnabled
        if WalkSpeedEnabled then
            WalkWindow=Lib:CreateWindow("WalkSpeed",Color3.fromRGB(128,0,128))
            WalkWindow:CreateTextBox("Speed",function(val) WalkSpeedValue=tonumber(val) or 16 end)
            WalkWindow:CreateButton("Apply",function()
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                    LocalPlayer.Character.Humanoid.WalkSpeed = WalkSpeedValue
                end
            end)
        else
            if WalkWindow then WalkWindow:Destroy() end
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                LocalPlayer.Character.Humanoid.WalkSpeed = 16
            end
        end
    end)

    local JumpEnabled=false
    local JumpValue=100
    local JumpWindow
    Tab:CreateButton("JumpPower ON/OFF",function()
        JumpEnabled = not JumpEnabled
        if JumpEnabled then
            JumpWindow=Lib:CreateWindow("JumpPower",Color3.fromRGB(128,0,128))
            JumpWindow:CreateTextBox("Jump",function(val) JumpValue=tonumber(val) or 50 end)
            JumpWindow:CreateButton("Apply",function()
                if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                    LocalPlayer.Character.Humanoid.JumpPower = JumpValue
                end
            end)
        else
            if JumpWindow then JumpWindow:Destroy() end
            if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
                LocalPlayer.Character.Humanoid.JumpPower = 50
            end
        end
    end)

    -- GAME ENHANCER SECTION
    Tab:CreateSection("GAME ENHANCER")

    local FullZoom=false
    Tab:CreateButton("Full Zoom ON/OFF",function()
        FullZoom = not FullZoom
        Notify("Full Zoom", tostring(FullZoom))
        LocalPlayer.CameraMaxZoomDistance = FullZoom and 100 or 12
    end)

    local FullBright=false
    local DefaultLighting={Brightness=Lighting.Brightness, ClockTime=Lighting.ClockTime, FogEnd=Lighting.FogEnd, GlobalShadows=Lighting.GlobalShadows, Ambient=Lighting.Ambient}
    Tab:CreateButton("Full Bright ON/OFF",function()
        FullBright = not FullBright
        Notify("Full Bright", tostring(FullBright))
        if FullBright then
            Lighting.Brightness=1
            Lighting.ClockTime=12
            Lighting.FogEnd=786543
            Lighting.GlobalShadows=false
            Lighting.Ambient=Color3.fromRGB(178,178,178)
        else
            for i,v in pairs(DefaultLighting) do
                Lighting[i]=v
            end
        end
    end)

    -- ROLE ESP
    local RoleESPEnabled=false
    local RoleESPObjects={}
    local ROLE_COLORS={Crewmate=Color3.fromRGB(0,255,0), Neutral=Color3.fromRGB(255,165,0), Imposter=Color3.fromRGB(255,0,0)}

    Tab:CreateButton("Role View ESP ON/OFF",function()
        RoleESPEnabled = not RoleESPEnabled
        Notify("Role ESP", tostring(RoleESPEnabled))
    end)

    -- DEAD BODY ESP
    local DeadBodyESPEnabled=false
    local DeadBodies={}
    Tab:CreateButton("Dead Body ESP ON/OFF",function()
        DeadBodyESPEnabled = not DeadBodyESPEnabled
        Notify("Dead Body ESP", tostring(DeadBodyESPEnabled))
    end)

    -- RENDER LOOP
    RunService.RenderStepped:Connect(function()
        -- Noclip
        if NoclipEnabled and LocalPlayer.Character then
            for _,p in pairs(LocalPlayer.Character:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide=false end
            end
        end

        -- Animated Role ESP
        if RoleESPEnabled then
            for _,plr in pairs(Players:GetPlayers()) do
                if plr~=LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
                    local hrp=plr.Character.HumanoidRootPart
                    if not RoleESPObjects[plr] then
                        local bb=Instance.new("BillboardGui")
                        bb.Size=UDim2.new(5,0,1,0)
                        bb.StudsOffset=Vector3.new(0,3,0)
                        bb.AlwaysOnTop=true
                        bb.Parent=hrp

                        local txt=Instance.new("TextLabel")
                        txt.Size=UDim2.new(1,0,1,0)
                        txt.BackgroundTransparency=1
                        txt.TextScaled=true
                        txt.Font=Enum.Font.GothamBold
                        txt.TextStrokeTransparency=0.25
                        txt.Parent=bb

                        local tracer=Drawing.new("Line")
                        tracer.Thickness=1.5

                        RoleESPObjects[plr]={Billboard=bb,Label=txt,Tracer=tracer}
                    end

                    local esp=RoleESPObjects[plr]
                    local pulse=(math.sin(tick()*3)+1)/2
                    esp.Label.Text = plr.Name.." | ROLE | ROLE"
                    local color=Color3.fromHSV(pulse*0.33,1,1)
                    esp.Label.TextColor3=color
                    esp.Tracer.Color=color
                    local pos=Camera:WorldToViewportPoint(hrp.Position)
                    esp.Tracer.From=Vector2.new(Camera.ViewportSize.X/2,Camera.ViewportSize.Y)
                    esp.Tracer.To=Vector2.new(pos.X,pos.Y)
                    esp.Tracer.Visible=true
                end
            end
        end
    end)
end

-------------------------------------------------
-- INITIALIZE FEATURES
-------------------------------------------------
SetupAllFeatures(Library)
