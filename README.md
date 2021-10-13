local mt = getrawmetatable(game)
setreadonly(mt,false)
local nidx = mt.__newindex

mt.__newindex = newcclosure(function(a,b,c)
    if b == 'Text' and tostring(a.Name) == 'AmmoCounter' then
        return nidx(a,b,math.huge..'/3')
    end
   
    return nidx(a,b,c)
end)

local function Ammo()
    for i, v in next, getgc(true) do
        if type(v) == "table" and rawget(v, "LoadedAmmo") then
            v.LoadedAmmo = math.huge
        end
    end
end

while wait(1) do
    if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild('Revolver') then
        spawn(function()
            pcall(function()
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("SetVisible", {[1] = false})
               
                wait(0.25)
               
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("SetVisible", {[1] = true})
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("ReloadUpdate", {[1] = "Start"})
               
                wait(0.25)
               
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("ReloadUpdate", {[1] = "AmmoAdded"})
               
                Ammo()
            end)
        end)
    end
end
