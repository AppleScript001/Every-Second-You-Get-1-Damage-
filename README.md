local Mobs_Table = {}
for _, v in ipairs(game:GetService("Workspace").Npc:GetDescendants()) do
    if v:IsA("Model") and v:FindFirstChild("HumanoidRootPart") and not table.find(Mobs_Table, v.Name) then
        table.insert(Mobs_Table, v.Name)
        table.sort(Mobs_Table)
    end
end

local PlayerTP1
local TweenService = game:GetService("TweenService")


local Material = loadstring(game:HttpGet("https://raw.githubusercontent.com/Kinlei/MaterialLua/master/Module.lua"))()

local X = Material.Load({
    Title = "Every Second You Get 1+ Damage",
    Style = 1,
    SizeX = 350,
    SizeY = 350,
    Theme = "Dark",
    ColorOverrides = {
        MainFrame = Color3.fromRGB(0,0,0),
        Toggle = Color3.fromRGB(124,37,255),
        ToggleAccent = Color3.fromRGB(255,255,255), 
        Dropdown = Color3.fromRGB(124,37,255),
		DropdownAccent = Color3.fromRGB(255,255,255),
        Slider = Color3.fromRGB(124,37,255),
		SliderAccent = Color3.fromRGB(255,255,255),
        NavBarAccent = Color3.fromRGB(0,0,0),
        Content = Color3.fromRGB(0,0,0),
    }
})

local Y = X.New({
    Title = "Main"
})

local Z = X.New({
    Title = "LocalPlayer"
})

local Cred = X.New({
    Title = "Credits"
})
    Cred.Button({
    Text = "Free|Script",
    Callback = function()
        setclipboard("Good")
        toclipboard("Nice|Scripts")
    end,
})

local ii = Y.Dropdown({
    Text = "Select Mobs!",
    Callback = function(Value)
        getgenv().mobname = Value
end,
    Options = Mobs_Table
})
Y.Button(
    {
        Text = "Refresh Mobs",
        Callback = function()
            table.clear(Mobs_Table)
            for i, v in pairs(game:GetService("Workspace").Npc:GetDescendants()) do
                if v:IsA "Model" and v:FindFirstChild("HumanoidRootPart") then
                    if not table.find(Mobs_Table, tostring(v)) then
                        table.insert(Mobs_Table, tostring(v))
                        ii:SetOptions(Mobs_Table)
                    end
                end
            end
        end
    }
)
Y.Toggle({
Text = "Auto Selected [Hit]",
Callback = function(Value)
b = Value
while b do task.wait()
pcall(function()
for i,v in pairs(game:GetService("Workspace").Npc:GetDescendants()) do
if v.Name == PlayerTP1 then
if v.Humanoid.Health > 0 then
repeat
local toolName = "RainbowSword" -- Replace "YourToolNameHere" with the name of your tool
                
local LocalPlayer = game:GetService("Players").LocalPlayer
for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
if v:IsA('Tool') and v.Name == toolName then
v.Parent = LocalPlayer.Character
break -- Stop the loop after picking up the tool
end
end 
LocalPlayer.Character:FindFirstChild(toolName):Activate()    
game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * CFrame.new(0,0,5)
wait()  -- Wait a short time before checking again
until not _G.Attack or v.Humanoid.Health <= 0
end
end
end
end)
end
end,
Enabled = false
})
Z.Slider({
	Text = "WalkSpeed",
	Callback = function(Value)
		getgenv().bool = Value
	end,
	Min = 50,
	Max = 500,
	Def = 50
})

    Z.Toggle({
    Text = "Toggle WalkSpeed",
    Callback = function(Value)
        g = Value
        while g do task.wait()
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = bool
        end
	end,
    Enabled = false
})
for i,v in pairs(getconnections(game.Players.LocalPlayer.Idled)) do
    v:Disable()
end
