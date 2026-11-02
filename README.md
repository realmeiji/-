-- ============ PRECISION HUB UI ============
local repo = "https://raw.githubusercontent.com/majeskooooo/Obsidian/main/"
local Library = loadstring(game:HttpGet(repo .. "Library.lua"))()
local ThemeManager = loadstring(game:HttpGet(repo .. "addons/ThemeManager.lua"))()
local SaveManager = loadstring(game:HttpGet(repo .. "addons/SaveManager.lua"))()

local Options = Library.Options
local Toggles = Library.Toggles

Library.Scheme.AccentColor = Color3.fromRGB(155, 196, 226)
Library.Scheme.BackgroundColor = Color3.fromRGB(10, 13, 18)
Library.Scheme.MainColor = Color3.fromRGB(18, 22, 30)
Library.Scheme.OutlineColor = Color3.fromRGB(28, 34, 44)
Library.Scheme.DarkColor = Color3.fromRGB(4, 5, 8)

local Window = Library:CreateWindow({
    Title = "",
    Footer = "Spent Hub",
    FooterLeft = "v16",
    FooterRight = "Enhanced UI",
    Icon = 74169651953690,
    NotifySide = "Right",
    ShowCustomCursor = true,
    Size = UDim2.fromOffset(700, 750),
    Center = true,
    Resizable = true,
    SidebarCompacted = true,
    ShowMobileButtons = false,
    CornerRadius = 10,
    Font = Enum.Font.Gotham,
})

local Tabs = {
    Info     = Window:AddTab("Info",     "info",            "Script info"),
    Main     = Window:AddTab("Main",     "crosshair",       "Silent Aim & Aimbot"),
    ESP      = Window:AddTab("ESP",      "eye",             "ESP & Visuals"),
    Config   = Window:AddTab("Config",   "settings",        "Save & Load configs"),
}

Library:SetDPIScale(95)

-- ============ NOTIFY HELPER ============
local function notify(title, text, t)
    Library:Notify({ Title = title, Text = text, Time = t or 3 })
end

-- ============ INFO TAB ============
local scriptStartTime = tick()

local function getGreeting()
    local h = tonumber(os.date("%H"))
    if h < 12 then return "Good Morning"
    elseif h < 18 then return "Good Afternoon"
    else return "Good Evening" end
end

local function formatUptime(s)
    return string.format("%dh %dm %ds", math.floor(s/3600), math.floor((s%3600)/60), math.floor(s%60))
end

local InfoLeft  = Tabs.Info.Sides[1]
local InfoRight = Tabs.Info.Sides[2]
local InfoContainer = InfoLeft.Parent

local PROFILE_HEIGHT = 110
local ProfileHolder = Instance.new("Frame")
ProfileHolder.Name = "ProfilePanel"
ProfileHolder.BackgroundColor3 = Library.Scheme.BackgroundColor
ProfileHolder.Position = UDim2.fromOffset(1, 2)
ProfileHolder.Size = UDim2.new(1, -2, 0, PROFILE_HEIGHT)
ProfileHolder.Parent = InfoContainer
Library:AddToRegistry(ProfileHolder, { BackgroundColor3 = "BackgroundColor" })
local _pc = Instance.new("UICorner"); _pc.CornerRadius = UDim.new(0, Library.CornerRadius); _pc.Parent = ProfileHolder
table.insert(Library.Corners, _pc)
Library:AddOutline(ProfileHolder)

local ProfileFrame = Instance.new("Frame")
ProfileFrame.BackgroundTransparency = 1
ProfileFrame.Size = UDim2.new(1,-16,1,-16)
ProfileFrame.Position = UDim2.fromOffset(8,8)
ProfileFrame.Parent = ProfileHolder

local Headshot = Instance.new("ImageLabel")
Headshot.BackgroundColor3 = Library:GetBetterColor(Library.Scheme.BackgroundColor, 8)
Headshot.Size = UDim2.fromOffset(80,80)
Headshot.Image = ""
Headshot.Parent = ProfileFrame
Instance.new("UICorner", Headshot).CornerRadius = UDim.new(0,12)
Library:AddToRegistry(Headshot, { BackgroundColor3 = function() return Library:GetBetterColor(Library.Scheme.BackgroundColor, 8) end })
task.spawn(function()
    local ok, img = pcall(function() return Players:GetUserThumbnailAsync(player.UserId, Enum.ThumbnailType.HeadShot, Enum.ThumbnailSize.Size420x420) end)
    if ok then Headshot.Image = img end
end)

local GreetLabel = Instance.new("TextLabel")
GreetLabel.BackgroundTransparency = 1
GreetLabel.Position = UDim2.fromOffset(92,2)
GreetLabel.Size = UDim2.new(1,-92,0,26)
GreetLabel.Font = Enum.Font.GothamBold
GreetLabel.TextSize = 18
GreetLabel.TextXAlignment = Enum.TextXAlignment.Left
GreetLabel.TextColor3 = Library.Scheme.FontColor
GreetLabel.Text = getGreeting() .. ", " .. player.Name .. "!"
GreetLabel.Parent = ProfileFrame
Library:AddToRegistry(GreetLabel, { TextColor3 = "FontColor" })

local ExecutorName = "Unknown"
pcall(function() if identifyexecutor then ExecutorName = identifyexecutor() end end)

local InfoDetail = Instance.new("TextLabel")
InfoDetail.BackgroundTransparency = 1
InfoDetail.Position = UDim2.fromOffset(92,30)
InfoDetail.Size = UDim2.new(1,-92,0,60)
InfoDetail.Font = Enum.Font.Gotham
InfoDetail.TextSize = 13
InfoDetail.TextXAlignment = Enum.TextXAlignment.Left
InfoDetail.TextYAlignment = Enum.TextYAlignment.Top
InfoDetail.TextColor3 = Color3.fromRGB(180,180,180)
InfoDetail.RichText = true
InfoDetail.Text = "• Executor: "..ExecutorName.."\n• Map: Prison Life\n• Time: "..os.date("%I:%M %p")
InfoDetail.Parent = ProfileFrame

local function repositionInfoSides()
    local off = PROFILE_HEIGHT + 10
    InfoLeft.Position  = UDim2.new(0,0,0,off); InfoLeft.Size  = UDim2.new(0.5,-3,1,-off)
    InfoRight.Position = UDim2.new(1,0,0,off); InfoRight.Size = UDim2.new(0.5,-3,1,-off)
end
repositionInfoSides()

local UpdatesBox = Tabs.Info:AddLeftGroupbox("Updates", "bell")
UpdatesBox:AddLabel("Spent Hub v16 Enhanced", true)
UpdatesBox:AddLabel(
    "- Silent Aim (7zz method)\n"..
    "- Autoshoot\n"..
    "- Hitbox Expander\n"..
    "- ESP (Highlight + Names + Tracer)\n"..
    "- Noclip / Infinite Jump\n"..
    "- Invisible (A2Flacko)\n"..
    "- Anti-Invisible\n"..
    "- Anti Sit / Anti Tase\n"..
    "- Gun Mod / Fast Switch\n"..
    "- Vehicle Fly & Speed\n"..
    "- Weapon Skins & Textures\n"..
    "- Custom Kill & Hit Sounds\n"..
    "- Kill/Hit Logs\n"..
    "- Skybox Changer\n"..
    "- Spinbot\n"..
    "- Teleports", true)
UpdatesBox:AddDivider()
UpdatesBox:AddButton({ Text = "Copy Discord", Func = function() pcall(function() setclipboard("discord.gg/unxmjPC4x7") end) end })

local FeedbackBox = Tabs.Info:AddRightGroupbox("Feedback", "mail")
FeedbackBox:SetDescription("Send feedback to the developer")
FeedbackBox:AddInput("FeedbackText", { Text = "", Placeholder = "Type your feedback here...", ClearTextOnFocus = false, Finished = false })
FeedbackBox:AddButton({ Text = "Send Feedback", Func = function() getgenv().SendFeedback(Options.FeedbackText.Value) end })

local StatsBox = Tabs.Info:AddRightGroupbox("Server Stats", "info")
local regionCode = "Unknown"
pcall(function() regionCode = game:GetService("LocalizationService"):GetCountryRegionForPlayerAsync(player) end)
local StatsLabel = StatsBox:AddLabel("Region: "..regionCode.."\nUptime: "..formatUptime(tick()-scriptStartTime), true)
task.spawn(function()
    while task.wait(1) do
        pcall(function() StatsLabel:SetText("Region: "..regionCode.."\nUptime: "..formatUptime(tick()-scriptStartTime)) end)
    end
end)

-- ============ MAIN TAB — SILENT AIM ============
local AimLeftBox = Tabs.Main:AddLeftGroupbox("Silent Aim", "crosshair")

AimLeftBox:AddCheckbox("SilentAim", {
    Text = "Silent Aim",
    Default = true,
    Callback = function(v)
        Settings.Enabled = v
        if not v then currentTarget = nil; smoothedTargetPosition = nil end
    end,
})

AimLeftBox:AddCheckbox("Autoshoot", {
    Text = "Autoshoot",
    Tooltip = "Auto-fires when a target is locked",
    Default = false,
    Callback = function(v) Settings.Autoshoot = v end,
})

AimLeftBox:AddCheckbox("WallCheck", {
    Text = "Wall Check",
    Default = true,
    Callback = function(v) Settings.VisibleCheck = v end,
})

AimLeftBox:AddCheckbox("TeamCheck", {
    Text = "Team Check",
    Default = true,
    Callback = function(v) Settings.TeamCheck = v end,
})

AimLeftBox:AddCheckbox("FriendCheck", {
    Text = "Friend Check",
    Default = true,
    Callback = function(v) Settings.FriendCheck = v end,
})

AimLeftBox:AddCheckbox("ForceFieldCheck", {
    Text = "ForceField Check",
    Default = true,
    Callback = function(v) Settings.ForceFieldCheck = v end,
})

AimLeftBox:AddDivider()

AimLeftBox:AddDropdown("TargetTeams", {
    Values = {"Inmates","Guards","Criminals"},
    Multi = true,
    Text = "Target Teams",
    Callback = function(v) Settings.TargetTeams = v or {} end,
})
Options.TargetTeams:SetValue({ Inmates=true, Guards=true, Criminals=true })

AimLeftBox:AddDropdown("AimMode", {
    Values = {"FOV","Distance"},
    Default = "FOV",
    Text = "Targeting Mode",
    Callback = function(v) Settings.AimMode = v end,
})

AimLeftBox:AddDropdown("TargetPart", {
    Values = {"Head","Torso","Left Arm","Right Arm","Left Leg","Right Leg","HumanoidRootPart","Random"},
    Default = "HumanoidRootPart",
    Text = "Target Part",
    Callback = function(v) Settings.TargetPart = v end,
})

-- ============ HITBOX EXPANDER ============
local HitboxBox = Tabs.Main:AddLeftGroupbox("Hitbox Expander", "maximize-2")
HitboxBox:SetDescription("Enlarges player hitboxes")

HitboxBox:AddCheckbox("HitboxExpander", {
    Text = "Enable Hitbox Expander",
    Default = false,
    Callback = function(v) Settings.HitboxExpanderEnabled = v end,
})

HitboxBox:AddCheckbox("HitboxTeamCheck", {
    Text = "Team Check",
    Default = true,
    Callback = function(v) Settings.HitboxTeamCheck = v end,
})

HitboxBox:AddSlider("HitboxSize", {
    Text = "Hitbox Size",
    Default = 1, Min = 1, Max = 15, Rounding = 0,
    Callback = function(v) Settings.HitboxSize = v end,
})

HitboxBox:AddSlider("HitboxTransparency", {
    Text = "Hitbox Transparency",
    Default = 0.5, Min = 0, Max = 1, Rounding = 1,
    Callback = function(v) Settings.HitboxTransparency = v end,
})

HitboxBox:AddDropdown("HitboxShape", {
    Values = {"Box","Ball"},
    Default = "Box",
    Text = "Hitbox Shape",
    Callback = function(v) Settings.HitboxShape = v end,
})

-- ============ SMOOTHING & ACCURACY ============
local AccBox = Tabs.Main:AddLeftGroupbox("Smoothing & Accuracy", "target")

AccBox:AddSlider("SilentAimRange", {
    Text = "Silent Aim Range",
    Default = 500, Min = 30, Max = 2000, Rounding = 0,
    Callback = function(v) Settings.SilentAimRange = v end,
})

AccBox:AddSlider("Smoothing", {
    Text = "Smoothing",
    Default = 0, Min = 0, Max = 1, Rounding = 1,
    Callback = function(v) Settings.Smoothing = v end,
})

AccBox:AddSlider("HitChance", {
    Text = "Hit Chance",
    Default = 100, Min = 0, Max = 100, Rounding = 1,
    Callback = function(v) Settings.HitChance = v end,
})

-- ============ FOV SETTINGS ============
local FOVBox = Tabs.Main:AddRightGroupbox("FOV Settings", "scan-eye")

FOVBox:AddCheckbox("ShowFOV", {
    Text = "Show FOV Circle",
    Default = true,
    Callback = function(v)
        toggleFOVCircle(not v)
    end,
}):AddColorPicker("FOVColor", {
    Default = Color3.fromRGB(255,255,255),
    Title = "FOV Color",
    Callback = function(v) setFOVColorRGB(v) end,
})

FOVBox:AddSlider("FOVRadius", {
    Text = "FOV Radius",
    Default = 100, Min = 30, Max = 500, Rounding = 0,
    Callback = function(v) Settings.FOVRadius = v end,
})

-- ============ PLAYER MISC ============
local MiscBox = Tabs.Main:AddRightGroupbox("Player", "sliders")

MiscBox:AddCheckbox("Noclip", {
    Text = "Noclip",
    Default = false,
    Callback = function(v) getgenv().SetNoclipEnabled(v) end,
})

MiscBox:AddCheckbox("InfiniteJump", {
    Text = "Infinite Jump",
    Default = false,
    Callback = function(v) getgenv().SetInfiniteJumpEnabled(v) end,
})

MiscBox:AddCheckbox("InfiniteStamina", {
    Text = "Infinite Stamina",
    Default = false,
    Callback = function(v) Settings.InfiniteStamina = v end,
})

MiscBox:AddCheckbox("Invisible", {
    Text = "Invisible",
    Default = false,
    Callback = function(v) getgenv().SetInvisibleEnabled(v) end,
})

MiscBox:AddCheckbox("AntiInvisible", {
    Text = "Anti-Invisible",
    Default = false,
    Callback = function(v) getgenv().SetAntiInvisibleEnabled(v) end,
})

MiscBox:AddCheckbox("AntiSit", {
    Text = "Anti Sit",
    Default = false,
    Callback = function(v) getgenv().SetAntiSitEnabled(v) end,
})

MiscBox:AddCheckbox("Korblox", {
    Text = "Korblox",
    Default = false,
    Callback = function(v) getgenv().SetKorbloxEnabled(v) end,
})

MiscBox:AddCheckbox("Headless", {
    Text = "Headless",
    Default = false,
    Callback = function(v) getgenv().SetHeadlessEnabled(v) end,
})

MiscBox:AddCheckbox("CameraFOVEnabled", {
    Text = "Custom Camera FOV",
    Default = false,
    Callback = function(v) Settings.CameraFOVEnabled = v end,
})

MiscBox:AddSlider("CameraFOV", {
    Text = "Camera FOV",
    Default = 70, Min = 30, Max = 120, Rounding = 0,
    Callback = function(v) Settings.CameraFOV = v end,
})

MiscBox:AddSlider("SpinbotSpeed", {
    Text = "Spinbot Speed",
    Default = 0, Min = 0, Max = 250, Rounding = 1,
    Callback = function(v) Settings.SpinbotSpeed = v end,
})

MiscBox:AddDivider()

MiscBox:AddButton({ Text = "Activate Anti-Tase", Func = function()
    local RS = game:GetService("ReplicatedStorage")
    local remote = nil
    pcall(function() remote = RS:WaitForChild("GunRemotes",5):WaitForChild("PlayerTased",5) end)
    task.spawn(function()
        while true do
            if remote then
                local ok, conns = pcall(getconnections, remote.OnClientEvent)
                if ok then for _, c in conns do c:Disable() end end
            end
            task.wait(0.5)
        end
    end)
    notify("Anti-Tase", "Anti-Tase enabled")
end })

-- ============ GUN SWITCHER ============
local GunSwitchBox = Tabs.Main:AddRightGroupbox("Gun Switcher", "repeat")

GunSwitchBox:AddDropdown("GunSwitchList", {
    Values = {"M9","Revolver","MP5","AK-47","M4A1","FAL","Remington 870","M700"},
    Multi = true,
    Text = "Select Guns",
    Callback = function(v)
        local list = {}
        for name, sel in pairs(v or {}) do if sel then table.insert(list, name) end end
        Settings.GunSwitchList = list
    end,
})

GunSwitchBox:AddSlider("SwitchDelay", {
    Text = "Switch Delay",
    Default = 0.001, Min = 0.001, Max = 1, Rounding = 2,
    Callback = function(v) Settings.GunSwitchDelay = v end,
})

GunSwitchBox:AddCheckbox("CycleAllWeapons", {
    Text = "Cycle All Weapons",
    Default = false,
    Callback = function(v) Settings.GunSwitchCycleAll = v end,
})

GunSwitchBox:AddButton({ Text = "Toggle Fast Switch", Func = function()
    local newState = not Settings.GunSwitchEnabled
    getgenv().ToggleGunSwitch(newState)
    notify("Gun Switcher", "Fast Switch: "..(newState and "ON" or "OFF"))
end })

GunSwitchBox:AddLabel("Fast Switch Keybind"):AddKeyPicker("FastSwitchKeybind", {
    Default = "None", NoUI = false, Text = "Fast Switch Keybind",
    Callback = function()
        local ns = not Settings.GunSwitchEnabled
        getgenv().ToggleGunSwitch(ns)
    end,
})
task.spawn(function()
    while true do task.wait(0.1) Settings.FastSwitchKeyCode = Options.FastSwitchKeybind.Value end
end)

-- ============ WEAPON MOD ============
local WeaponModBox = Tabs.Main:AddRightGroupbox("Weapon Mod", "sword")
local WMTabbox = WeaponModBox:AddTabbox()

local GunTab = WMTabbox:AddTab(nil, "wrench")
GunTab:AddCheckbox("WeaponMod",    { Text = "Weapon Mod",   Default = false, Callback = function(v) getgenv().SetGunModEnabled(v) end })
GunTab:AddDivider()
GunTab:AddCheckbox("AutoFire",     { Text = "Auto Fire",    Default = false, Callback = function(v) getgenv().SetGunModAutoFire(v) end })
GunTab:AddCheckbox("FastFirerate", { Text = "Fast Firerate",Default = false, Callback = function(v) getgenv().SetGunModFastFirerate(v) end })
GunTab:AddCheckbox("AutoReload",   { Text = "Auto Reload",  Default = false, Callback = function(v) getgenv().SetAutoReloadEnabled(v) end })
GunTab:AddSlider("FireRateValue",  { Text = "Fire Rate", Default = 0.001, Min = 0.001, Max = 0.1, Rounding = 2, Callback = function(v) getgenv().SetGunModFireRate(v) end })

local BulletTab = WMTabbox:AddTab(nil, "cable")
BulletTab:AddCheckbox("CustomBulletColor", {
    Text = "Custom Bullet Color", Default = false,
    Callback = function(v) setCustomBulletColorEnabled(v) end,
})
BulletTab:AddLabel("Bullet Color"):AddColorPicker("BulletColor", {
    Default = Color3.fromRGB(255,255,0), Title = "Bullet Color",
    Callback = function(v) setBulletColorRGB(v) end,
})

local SkinTab = WMTabbox:AddTab(nil, "paintbrush")
SkinTab:AddCheckbox("EnableWeaponColor", {
    Text = "Weapon Color", Default = false,
    Callback = function(v) getgenv().SetWeaponColorEnabled(v) end,
}):AddColorPicker("WeaponColor", {
    Default = Color3.fromRGB(255,255,255), Title = "Weapon Color",
    Callback = function(v) getgenv().SetWeaponColorRGB(v) end,
})
SkinTab:AddCheckbox("EnableWeaponTexture", { Text = "Enable Texture", Default = false, Callback = function(v) getgenv().SetWeaponTextureEnabled(v) end })
SkinTab:AddSlider("WeaponTransparency", { Text = "Transparency", Default = 0, Min = 0, Max = 1, Rounding = 1, Callback = function(v) getgenv().SetWeaponTransparency(v) end })
SkinTab:AddDropdown("WeaponMaterial", { Values = {"ForceField","SmoothPlastic","DiamondPlate","Metal","Marble","Neon"}, Default = "SmoothPlastic", Text = "Material", Callback = function(v) getgenv().SetWeaponMaterial(v) end })
SkinTab:AddDropdown("WeaponTexture", {
    Values = {"Matrix","Blue Void","Classic blue White","Green and Black","White","Blue sky's with White Guns","Water Skin","Aurora","Epic White","Pink White","Blue Stars"},
    Default = "Matrix", Text = "Texture",
    Callback = function(v) getgenv().SetWeaponTexture(v) end,
})

local SoundTab = WMTabbox:AddTab(nil, "volume-2")
SoundTab:AddDropdown("WeaponSoundPack", { Values = {"None","Silenced Shot","Bubble Shot","Laser Shot","Dart Gun","Energy Shot","Neverlose"}, Default = "None", Text = "Shot Sound", Callback = function(v) getgenv().SetWeaponSoundPack(v) end })
SoundTab:AddDivider()
SoundTab:AddCheckbox("CustomKillSound", { Text = "Kill Sound", Default = false, Callback = function(v) getgenv().SetCustomKillSoundEnabled(v) end })
SoundTab:AddCheckbox("CustomHitSound",  { Text = "Hit Sound",  Default = false, Callback = function(v) getgenv().SetCustomHitSoundEnabled(v) end })
SoundTab:AddDivider()
SoundTab:AddDropdown("KillSound", { Values = {"Fahhh","Epic Kill 1","Epic Kill 2","Laugh","Cat Laughing Meme","Goofy Laugh","PVZ Laugh","CoD Kaboom","You are a Idiot","Creepy Bell","Vine Boom","RDR2 Low Honor","Get Out","Diahrrea Sound","Goku Prowler","iPhone Ding","Bark Fart","Enrique","Fatality 2"}, Default = 1, Text = "Kill Sound", Callback = function(v) getgenv().SetKillSound(v) end })
SoundTab:AddDropdown("HitSound",  { Values = {"TF2 Hit","Metal Bat Hit","Neverlose","Minecraft Hit 1","Minecraft Hit 2","MLG Hit","Fahhh Hit","Xbox Live Notification","Rust","Fatality","Skeet","Pick","Creepy Bell"}, Default = 1, Text = "Hit Sound", Callback = function(v) getgenv().SetHitSound(v) end })

local NotifTab = WMTabbox:AddTab(nil, "bell")
NotifTab:AddCheckbox("KillEffect", {
    Text = "Kill Screen Effect", Default = false,
    Callback = function(v) getgenv().SetKillEffectEnabled(v) end,
}):AddColorPicker("KillEffectColor", {
    Default = Color3.fromRGB(255,255,255), Title = "Kill Effect Color",
    Callback = function(v) getgenv().SetKillEffectColor(v) end,
})
NotifTab:AddSlider("KillEffectFade", { Text = "Fade Duration", Default = 0.25, Min = 0.05, Max = 1, Rounding = 2, Callback = function(v) getgenv().SetKillEffectFadeDuration(v) end })
NotifTab:AddDivider("Logs")
NotifTab:AddCheckbox("KillLogs", { Text = "Kill Logs", Default = false, Callback = function(v) getgenv().SetKillLogsEnabled(v) end })
NotifTab:AddCheckbox("HitLogs",  { Text = "Hit Logs",  Default = false, Callback = function(v) getgenv().SetHitLogsEnabled(v) end })

-- ============ GET WEAPONS ============
local GetWeaponsBox = Tabs.Main:AddRightGroupbox("Get Weapons", "shopping-basket")

GetWeaponsBox:AddDropdown("GetWeaponsList", {
    Values = {"MP5","AK-47","M4A1","FAL","Remington 870","M700"},
    Multi = true,
    Text = "Select Weapons",
    Callback = function(v)
        local list = {}
        for name, sel in pairs(v or {}) do if sel then table.insert(list, name) end end
        Settings.GetWeaponsList = list
    end,
})
GetWeaponsBox:AddCheckbox("AutoGetWeapons", { Text = "Auto Get on Spawn", Default = false, Callback = function(v) Settings.AutoGetWeapons = v end })
GetWeaponsBox:AddButton({ Text = "Get Weapons", Func = function() if getgenv().GetWeapons then getgenv().GetWeapons() end end })

-- ============ VEHICLE MOD ============
local VehicleBox = Tabs.Main:AddRightGroupbox("Vehicle Mod", "car")
local VTabbox = VehicleBox:AddTabbox()

local VFlyTab = VTabbox:AddTab(nil, "wind")
VFlyTab:AddCheckbox("EnableVehicleFly", { Text = "Vehicle Fly", Default = false, Callback = function(v) Settings.VehicleFlyEnabled = v end })
VFlyTab:AddSlider("VehicleFlyHeight", { Text = "Height", Default = 0, Min = 0, Max = 65, Rounding = 0, Callback = function(v) Settings.VehicleFlyHeight = v end })
VFlyTab:AddLabel("Vehicle Fly Keybind"):AddKeyPicker("VehicleFlyKeybind", {
    Default = "None", NoUI = false, Text = "Vehicle Fly Keybind",
    Callback = function() Settings.VehicleFlyEnabled = not Settings.VehicleFlyEnabled end,
})
task.spawn(function() while true do task.wait(0.1) Settings.VehicleFlyKeyCode = Options.VehicleFlyKeybind.Value end end)
VFlyTab:AddButton({ Text = "Flip Vehicle", Func = function() if getgenv().FlipVehicle then getgenv().FlipVehicle() end end })

local VSpeedTab = VTabbox:AddTab(nil, "zap")
VSpeedTab:AddCheckbox("VehicleMaxSpeed", { Text = "Max Speed Changer", Default = false, Callback = function(v) Settings.VehicleMaxSpeedEnabled = v end })
VSpeedTab:AddSlider("VehicleMaxSpeedVal", { Text = "Max Speed", Default = 70, Min = 70, Max = 300, Rounding = 0, Callback = function(v) Settings.VehicleMaxSpeed = v end })

local VCarTab = VTabbox:AddTab(nil, "car")
VCarTab:AddDropdown("CarList", { Values = {}, Default = nil, AllowNull = true, Text = "Car List", Callback = function(v) Settings.SelectedCar = v end })
VCarTab:AddButton({ Text = "Refresh List", Func = function() if getgenv().RefreshCarList then local cars = getgenv().RefreshCarList(); Options.CarList:SetValues(cars) end end })
VCarTab:AddButton({ Text = "Bring Car",     Func = function() if getgenv().BringCar       and Options.CarList.Value then getgenv().BringCar(Options.CarList.Value)       end end })
VCarTab:AddButton({ Text = "Teleport to Car",Func = function() if getgenv().TeleportToCar and Options.CarList.Value then getgenv().TeleportToCar(Options.CarList.Value)  end end })
local warnLabel = VCarTab:AddLabel("Bannable — use at your own risk", true)
do local lbl = warnLabel.Holder and warnLabel.Holder:FindFirstChildOfClass("TextLabel"); if lbl then lbl.TextColor3 = Color3.fromRGB(235,70,70) end end

-- ============ ESP TAB ============
local ESPBox = Tabs.ESP:AddLeftGroupbox("Highlight ESP", "eye")

ESPBox:AddCheckbox("ESP", {
    Text = "Highlight ESP",
    Default = false,
    Callback = function(v) Settings.ESPEnabled = v; updateESP() end,
})
ESPBox:AddLabel("Target Highlight Color"):AddColorPicker("ESPTargetColor", {
    Default = Color3.fromRGB(255,182,255), Title = "ESP Target Color",
    Callback = function(v) setESPTargetColorRGB(v) end,
})

local NameESPBox = Tabs.ESP:AddLeftGroupbox("Name ESP", "type-outline")
NameESPBox:AddCheckbox("NameESP",        { Text = "Name ESP",         Default = true,  Callback = function(v) Settings.ESPNamesEnabled = v; updateESP() end })
NameESPBox:AddCheckbox("TargetNameESP",  { Text = "Target Name Label",Default = false, Callback = function(v) Settings.ESPTargetNameEnabled = v end })
NameESPBox:AddLabel("Text Color"):AddColorPicker("ESPTextColor", { Default = Color3.fromRGB(255,255,255), Title = "Text Color", Callback = function(v) setESPTextColor(v) end })
NameESPBox:AddLabel("Target Text Color"):AddColorPicker("ESPTargetTextColor", { Default = Color3.fromRGB(255,182,255), Title = "Target Text Color", Callback = function(v) setESPTargetTextColor(v) end })
NameESPBox:AddSlider("NameTextSize",   { Text = "Name Text Size",   Default = 12, Min = 0, Max = 24, Rounding = 0, Callback = function(v) Settings.ESPNameTextSize = v end })
NameESPBox:AddSlider("TargetTextSize", { Text = "Target Text Size", Default = 12, Min = 0, Max = 24, Rounding = 0, Callback = function(v) Settings.ESPTargetNameTextSize = v end })
NameESPBox:AddDivider()
NameESPBox:AddInput("TargetCustomText", { Text = "", Placeholder = "Target", ClearTextOnFocus = false, Finished = false })
NameESPBox:AddButton({ Text = "Apply", Func = function() local t = Options.TargetCustomText.Value; getgenv().SetTargetCustomText(t ~= "" and t or "Target") end })
NameESPBox:AddButton({ Text = "Reset", Func = function() Options.TargetCustomText:SetValue(""); getgenv().SetTargetCustomText("Target") end })

local TracerBox = Tabs.ESP:AddRightGroupbox("Tracer ESP", "zap")
TracerBox:AddCheckbox("TracerESP",       { Text = "Tracer ESP",  Default = false, Callback = function(v) Settings.TracerESPEnabled = v end })
TracerBox:AddCheckbox("TracerTargetOnly",{ Text = "Target Only", Default = false, Callback = function(v) Settings.TracerTargetOnly = v end })
TracerBox:AddSlider("TracerThickness", { Text = "Thickness", Default = 0.2, Min = 0.1, Max = 1.5, Rounding = 1, Callback = function(v) Settings.TracerThickness = v end })
TracerBox:AddLabel("Tracer Color"):AddColorPicker("TracerColor", { Default = Color3.fromRGB(255,182,255), Title = "Tracer Color", Callback = function(v) setTracerColorRGB(v) end })

local EnvBox = Tabs.ESP:AddRightGroupbox("Environment", "trees")

local WorldGradientEnabled = false
local DefaultAmbient = game:GetService("Lighting").Ambient
local DefaultOutdoorAmbient = game:GetService("Lighting").OutdoorAmbient
local CurrentGradientColor = Color3.fromRGB(255,255,255)
local CurrentOutdoorGradient = Color3.fromRGB(255,255,255)

EnvBox:AddCheckbox("WorldGradient", {
    Text = "World Gradient", Default = false,
    Callback = function(v)
        WorldGradientEnabled = v
        local Lighting = game:GetService("Lighting")
        if v then Lighting.Ambient = CurrentGradientColor; Lighting.OutdoorAmbient = CurrentOutdoorGradient
        else Lighting.Ambient = DefaultAmbient; Lighting.OutdoorAmbient = DefaultOutdoorAmbient end
    end,
})
EnvBox:AddLabel("Gradient Color"):AddColorPicker("GradientColor", { Default = Color3.fromRGB(255,255,255), Title = "Gradient Color", Callback = function(v) CurrentGradientColor = v; if WorldGradientEnabled then game:GetService("Lighting").Ambient = v end end })
EnvBox:AddLabel("Outdoor Color"):AddColorPicker("OutdoorGradient", { Default = Color3.fromRGB(255,255,255), Title = "Outdoor Gradient", Callback = function(v) CurrentOutdoorGradient = v; if WorldGradientEnabled then game:GetService("Lighting").OutdoorAmbient = v end end })
EnvBox:AddDivider()
EnvBox:AddCheckbox("RemoveFog", {
    Text = "Remove Fog", Default = false,
    Callback = function(v)
        local L = game:GetService("Lighting")
        if v then L.FogEnd = 100000; L.FogStart = 100000 else L.FogEnd = 5000; L.FogStart = 500 end
    end,
}):AddColorPicker("FogColor", { Default = Color3.fromRGB(100,100,100), Title = "Fog Color", Callback = function(v) setFogColorRGB(v) end })
EnvBox:AddDivider()
EnvBox:AddCheckbox("CustomSkybox", {
    Text = "Custom Skybox", Default = false,
    Callback = function(v) if v then setSkybox(Options.Skyboxes.Value or "None") else setSkybox("None") end end,
})
EnvBox:AddDropdown("Skyboxes", {
    Values = {"None","Gray Skybox","2knw & Donk666 Skybox","Snow Skybox","Cartoon Skybox","Lince Skybox","Red Skybox","Slam Skybox","Green Eyeball Skybox","Morioh Skybox","Rainy Skybox","Rusk1ddski Skybox"},
    Default = "None", Text = "Skybox",
    Callback = function(v) if Toggles.CustomSkybox and Toggles.CustomSkybox.Value then setSkybox(v) end end,
})

-- ============ TELEPORTS ============
local TeleportBox = Tabs.ESP:AddLeftGroupbox("Teleports", "map-pin")
local teleports = {
    {"Cafeteria",        Vector3.new(868,100,2263)},
    {"Yard",             Vector3.new(714,98,2519)},
    {"Entrance",         Vector3.new(663,100,2314)},
    {"Secret Room",      Vector3.new(728,100,2316)},
    {"Inside Prison",    Vector3.new(864,100,2406)},
    {"Guardtower",       Vector3.new(840,122,2593)},
    {"Guardtower 2",     Vector3.new(840,122,2067)},
    {"Police Room",      Vector3.new(850,100,2274)},
    {"Police Room 2",    Vector3.new(793,101,2246)},
    {"Frontwall",        Vector3.new(510,122,2328)},
    {"Armory",           Vector3.new(804,108,2257)},
    {"Containers",       Vector3.new(254,73,2359)},
    {"Sniper Building",  Vector3.new(-354,114,1991)},
    {"Criminal Base",    Vector3.new(-984,96,2079)},
}
for _, tp in ipairs(teleports) do
    TeleportBox:AddButton({ Text = tp[1], Func = function()
        local c = player.Character
        if c and c:FindFirstChild("HumanoidRootPart") then c.HumanoidRootPart.CFrame = CFrame.new(tp[2]) end
    end })
end

-- ============ CONFIG TAB ============
local MenuGroup = Tabs.Config:AddLeftGroupbox("Menu", "settings")

MenuGroup:AddCheckbox("KeybindMenu", {
    Text = "Keybind Menu",
    Default = Library.KeybindFrame.Visible,
    Callback = function(v) Library.KeybindFrame.Visible = v end,
})

MenuGroup:AddCheckbox("CustomCursor", {
    Text = "Custom Cursor",
    Default = Library.ShowCustomCursor,
    Callback = function(v) Library.ShowCustomCursor = v end,
})

MenuGroup:AddCheckbox("AlwaysOnTop", {
    Text = "Always On Top",
    Default = false,
    Callback = function(v) Window:SetAlwaysOnTop(v) end,
})

MenuGroup:AddDropdown("NotifSide", {
    Values = {"Left","Right"},
    Default = "Right",
    Text = "Notification Side",
    Callback = function(v) Library:SetNotifySide(v) end,
})

MenuGroup:AddDivider()
MenuGroup:AddLabel("Menu Keybind"):AddKeyPicker("MenuKeybind", { Default = "RightShift", NoUI = true, Text = "Menu keybind" })
MenuGroup:AddButton("Unload", function() Library:Unload() end)

Library.ToggleKeybind = Options.MenuKeybind

-- ============ ADDON SETUP ============
ThemeManager:SetLibrary(Library)
SaveManager:SetLibrary(Library)

SaveManager:IgnoreThemeSettings()
SaveManager:SetIgnoreIndexes({ "MenuKeybind" })

ThemeManager:SetFolder("SpentHub")
SaveManager:SetFolder("SpentHub/PrisonLife")

SaveManager:BuildConfigSection(Tabs.Config)
ThemeManager:ApplyToTab(Tabs.Config)

SaveManager:LoadAutoloadConfig()

notify("Spent Hub", "v16 Enhanced loaded · RightShift to toggle")
