_G.FastAttack = true
spawn(function()
    game:GetService("RunService").RenderStepped:Connect(function()
        if _G.FastAttack or Fastattack then
             pcall(function()
                game:GetService'VirtualUser':CaptureController()
                game:GetService'VirtualUser':Button1Down(Vector2.new(0,1,0,1))
            end)
        end
    end)
end)
spawn(function()
    while _G.FastAttack do task.wait()
    
    require(game.ReplicatedStorage.Util.CameraShaker):Stop()
    xShadowFastAttackx = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework)
    xShadowx = debug.getupvalues(xShadowFastAttackx)[2]
    task.spawn(function()
        while true do task.wait()
            if _G.FastAttack then
                if typeof(xShadowx) == "table" then
                    pcall(function()
                        xShadowx.activeController.timeToNextAttack = -(math.huge^math.huge^math.huge)
                        xShadowx.activeController.timeToNextAttack = 0
                        xShadowx.activeController.hitboxMagnitude = 200
                        xShadowx.activeController.active = false
                        xShadowx.activeController.timeToNextBlock = 0
                        xShadowx.activeController.focusStart = 0
                        xShadowx.activeController.increment = 4
                        xShadowx.activeController.blocking = false
                        xShadowx.activeController.attacking = false
                        xShadowx.activeController.humanoid.AutoRotate = true
                    end)
                end
            end
        end
    end)
    
    local Module = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework)
    local CombatFramework = debug.getupvalues(Module)[2]
    local CameraShakerR = require(game.ReplicatedStorage.Util.CameraShaker)
    
    task.spawn(function()
        while true do task.wait()
            if _G.FastAttack then
                pcall(function()
                    CameraShakerR:Stop()
                    CombatFramework.activeController.attacking = false
                    CombatFramework.activeController.timeToNextAttack = 0
                    CombatFramework.activeController.increment = 4
                    CombatFramework.activeController.hitboxMagnitude = 100
                    CombatFramework.activeController.blocking = false
                    CombatFramework.activeController.timeToNextBlock = 0
                    CombatFramework.activeController.focusStart = 0
                    CombatFramework.activeController.humanoid.AutoRotate = true
                end)
            end
        end
    end)
    
    local SeraphFrame = debug.getupvalues(require(game:GetService("Players").LocalPlayer.PlayerScripts:WaitForChild("CombatFramework")))[2]
    local VirtualUser = game:GetService('VirtualUser')
    local RigControllerR = debug.getupvalues(require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework.RigController))[2]
    local Client = game:GetService("Players").LocalPlayer
    local DMG = require(Client.PlayerScripts.CombatFramework.Particle.Damage)
    
    function SeraphFuckWeapon() 
        local p13 = SeraphFrame.activeController
        local wea = p13.blades[1]
        if not wea then return end
        while wea.Parent~=game.Players.LocalPlayer.Character do wea=wea.Parent end
        return wea
    end
    
    function getHits(Size)
        local Hits = {}
        local Enemies = workspace.Enemies:GetChildren()
        local Characters = workspace.Characters:GetChildren()
        for i=1,#Enemies do local v = Enemies[i]
            local Human = v:FindFirstChildOfClass("Humanoid")
            if Human and Human.RootPart and Human.Health > 0 and game.Players.LocalPlayer:DistanceFromCharacter(Human.RootPart.Position) < Size+55 then
                table.insert(Hits,Human.RootPart)
            end
        end
        for i=1,#Characters do local v = Characters[i]
            if v ~= game.Players.LocalPlayer.Character then
                local Human = v:FindFirstChildOfClass("Humanoid")
                if Human and Human.RootPart and Human.Health > 0 and game.Players.LocalPlayer:DistanceFromCharacter(Human.RootPart.Position) < Size+55 then
                    table.insert(Hits,Human.RootPart)
                end
            end
        end
        return Hits
    end
    
    task.spawn(
        function()
        while true do task.wait()
            if  _G.FastAttack then
                if SeraphFrame.activeController then
                    if v.Humanoid.Health > 0 then
                        SeraphFrame.activeController.timeToNextAttack = -(math.huge^math.huge^math.huge)
                        SeraphFrame.activeController.timeToNextAttack = 0
                        SeraphFrame.activeController.focusStart = 0
                        SeraphFrame.activeController.hitboxMagnitude = 110
                        SeraphFrame.activeController.humanoid.AutoRotate = true
                        SeraphFrame.activeController.increment = 4
                    end
                end
            end
        end
    end)
    
    function Boost()
        spawn(function()
            if SeraphFrame.activeController then
                game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("weaponChange",tostring(SeraphFuckWeapon()))
            end
        end)
    end
    
    function Unboost()
        spawn(function()
            --print("Unboost ของจริง555")
            --game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("unequipWeapon",tostring(SeraphFuckWeapon()))
        end)
    end
    
    local cdnormal = 0
    local Animation = Instance.new("Animation")
    local CooldownFastAttack = 0.000000
    Attack = function()
        local ac = SeraphFrame.activeController
        if ac and ac.equipped then
            task.spawn(
                function()
                if tick() - cdnormal > 0 then
                    ac:attack()
                    cdnormal = tick()
                else
                    Animation.AnimationId = ac.anims.basic[2]
                    ac.humanoid:LoadAnimation(Animation):Play(0.01, 0.01) --ท่าไม่ทำงานแก้เป็น (1,1)
                    game:GetService("ReplicatedStorage").RigControllerEvent:FireServer("hit", getHits(60), 1, "")
                end
                wait(0.12)
            end)
        end
    end
    
    b = tick()
    spawn(function()
        while _G.FastAttack do task.wait()
            if _G.FastAttack then
                if b - tick() > 9e9 then
                    b = tick()
                end
                pcall(function()
                    for i, v in pairs(game.Workspace.Enemies:GetChildren()) do
                        if v.Humanoid.Health > 0 then
                            if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 90 then
                                Attack()
                                wait()
                                Boost()
                            end
                        end
                    end
                end)
                wait(0.05)
            end
        end
    end)
    
    k = tick()
    spawn(function()
        while wait() do
            if  _G.FastAttack then
                if k - tick() > 9e9 then
                    k = tick()
                end
                pcall(function()
                    for i, v in pairs(game.Workspace.Enemies:GetChildren()) do
                        if v.Humanoid.Health > 0 then
                            if (v.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude <= 90 then
                                Unboost()
                            end
                        end
                    end
                end)
            end
        end
    end)
    
    tjw1 = true
    task.spawn(
        function()
            local a = game.Players.LocalPlayer
            local b = require(a.PlayerScripts.CombatFramework.Particle)
            local c = require(game:GetService("ReplicatedStorage").CombatFramework.RigLib)
            if not shared.orl then
                shared.orl = c.wrapAttackAnimationAsync
            end
            if not shared.cpc then
                shared.cpc = b.play
            end
            if tjw1 then
                pcall(
                    function()
                        c.wrapAttackAnimationAsync = function(d, e, f, g, h)
                            local i = c.getBladeHits(e, f, g)
                            if i then
                                b.play = function()
                                end
                                d:Play(0.01,0.01,0.01)
                                h(i)
                                b.play = shared.cpc
                                wait(0.1)
                                d:Stop()
                            end
                        end
                    end
                )
            end
        end
    )
    
    local CameRa = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework.CameraShaker)
    CameRa.CameraShakeInstance.CameraShakeState = {FadingIn = 3,FadingOut = 2,Sustained = 0,Inactive =1}
    
    local Client = game.Players.LocalPlayer
    local STOP = require(Client.PlayerScripts.CombatFramework.Particle)
    local STOPRL = require(game:GetService("ReplicatedStorage").CombatFramework.RigLib)
    task.spawn(function()
        pcall(function()
            if not shared.orl then
                shared.orl = STOPRL.wrapAttackAnimationAsync
            end
                if not shared.cpc then
                    shared.cpc = STOP.play 
                end
                spawn(function()
                    require(game.ReplicatedStorage.Util.CameraShaker):Stop()
                    game:GetService("RunService").Stepped:Connect(function()
                        STOPRL.wrapAttackAnimationAsync = function(a,b,c,d,func)
                            local Hits = STOPRL.getBladeHits(b,c,d)
                            if Hits then
                                if  _G.FastAttack then
                                    STOP.play = function() end
                                    a:Play(21,29,30)
                                    func(Hits)
                                    STOP.play = shared.cpc
                                    wait(a.length * 9e9)
                                    a:Stop()
                                else
                                    func(Hits)
                                    STOP.play = shared.cpc
                                    wait(a.length * 9e9)
                                    a:Stop()
                                end
                            end
                        end
                    end)
                end)
            end)
        end)
    end
end)
