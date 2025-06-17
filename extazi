do -- before loading
    if not game:IsLoaded() then
        repeat
            game.Loaded:Wait()
        until game:IsLoaded()
    end
    --
    if not LPH_OBFUSCATED then
        function LPH_NO_VIRTUALIZE(f)
            return f
        end
        function LPH_JIT(...)
            return ...
        end
        function LPH_JIT_MAX(...)
            return ...
        end
        function LPH_NO_UPVALUES(f)
            return function(...)
                return f(...)
            end
        end
        function LPH_ENCSTR(...)
            return ...
        end
        function LPH_ENCNUM(...)
            return ...
        end
        function LPH_CRASH()
            return print(debug.traceback())
        end
    end
    --
    local game_name = game.GetService(game, "MarketplaceService"):GetProductInfo(game.PlaceId).Name:gsub("[^a-zA-Z0-9%s]", ""):lower()
    local executor = identifyexecutor and identifyexecutor() or "Unknown"
    if not game_name:find("fallen survival", 1, true) then
        game.GetService(game, "Players")["LocalPlayer"]:Kick("Execute in Fallen only!")
        return
    end
    if executor == "Wave" or executor == "AWP" or executor == "Potassium" or executor == "Swift" or executor == "Autumn" or executor == "Seliware" or executor == "Valex" then
        -- do nothing
    else
        game.GetService(game, "Players")["LocalPlayer"]:Kick("Unsupported Executor " .. executor)
        return
    end
    --
    local Hud = Instance.new("ScreenGui")
    Hud.Parent = cloneref(gethui())
    Hud.Name = cloneref(game:GetService("HttpService")):GenerateGUID(false)
    Hud.IgnoreGuiInset = true
    --
    getgenv().gethui = newcclosure(function()
        return cloneref(Hud)
    end)
    --
    loadstring(
            game:HttpGetAsync(
                    "https://pastebin.com/raw/xsTkyCXW",
                    true
            )
    )()
end
task.wait(2) -- needed check to make sure its fully loaded
--
local default_color = Color3.fromRGB(0, 255, 255)
local Library = loadstring(
        game:HttpGet(
                "https://raw.githubusercontent.com/Alexand4r16/sigma-hook/refs/heads/main/sigma_hook_debug.lua"
        )
)({    
    cheatname = "SigmaHook",
    gamename = "SigmaHook",
    fileext = ".cfg",
    logo = "120404302634102",
    icon = "120404302634102",
    Accent = default_color,
})

local Flags, Pointers = Library.Flags, Library.Pointers
getfenv(0)["Library"], getfenv(0)["Flags"], getfenv(0)["Pointers"] = Library, Flags, Pointers


-- services
local Workspace = cloneref(game:GetService("Workspace"))
local run_service = cloneref(game:GetService("RunService"))
local rs = cloneref(game:GetService("ReplicatedStorage"))
local players = cloneref(game:GetService("Players"))
local uis = cloneref(game:GetService("UserInputService"))
local tween_service = cloneref(game:GetService("TweenService"))
local stats = cloneref(game:GetService("Stats"))
local get_team = cloneref(game:GetService("Teams"))
local lighting = cloneref(game:GetService("Lighting"))
-- variables
local teams = get_team:GetTeams()
local camera = Workspace["CurrentCamera"]
local viewport_size = camera["ViewportSize"]
local local_player = players["LocalPlayer"]
local old_tuahs = {}
local lplayer_name = local_player["Name"]
local local_char = local_player.Character or local_player.CharacterAdded:Wait()
local get_mouse = local_player:GetMouse()
local sky = lighting:FindFirstChildOfClass("Sky") or cloneref(Instance.new("Sky", lighting))
local color_correct = lighting:FindFirstChildOfClass("ColorCorrectionEffect")
        or cloneref(Instance.new("ColorCorrectionEffect", lighting))
local fov_circle, sounds, snap_lines, skys = {}, {}, {},  {};
local info_viewer= {};


-- game
local modules = rs:FindFirstChild("Modules")
local sound_module = modules:FindFirstChild("SoundModule")
local sound_table = require(sound_module)
local folder = Workspace:FindFirstChild("VFX") and Workspace.VFX:FindFirstChild("VMs")
local settings_module = modules:FindFirstChild("SettingsModule")
local settings_table = require(settings_module)
if not settings_table then
    return local_player:Kick("SettingsModule not found!")
end
if not folder then
    return local_player:Kick("VMs not found!")
end
local items_module = modules:FindFirstChild("Items")
local items_table = require(items_module)

-- lighting cache
local lighting_cache = {
    Brightness = game:GetService("Lighting").Brightness,
    ClockTime = game:GetService("Lighting").ClockTime,
    Ambient = game:GetService("Lighting").Ambient,
    OutdoorAmbient = game:GetService("Lighting").OutdoorAmbient,
    FogEnd = game:GetService("Lighting").FogEnd,
    FieldOfView = game:GetService("Workspace").CurrentCamera.FieldOfView,
    Density = game:GetService("Lighting"):FindFirstChild("Atmosphere").Density,
    Offset = game:GetService("Lighting"):FindFirstChild("Atmosphere").Offset,
    Glare = game:GetService("Lighting"):FindFirstChild("Atmosphere").Glare,
    Haze = game:GetService("Lighting"):FindFirstChild("Atmosphere").Haze,
}

-- skyboxes
local skyboxes = {
    ["Blue Sky"] = { "591058823", "591059876", "591058104", "591057861", "591057625", "591059642" },
    ["Vaporwave"] = { "1417494030", "1417494146", "1417494253", "1417494402", "1417494499", "1417494643" },
    ["Redshift"] = { "401664839", "401664862", "401664960", "401664881", "401664901", "401664936" },
    ["Blaze"] = { "150939022", "150939038", "150939047", "150939056", "150939063", "150939082" },
    ["Among Us"] = { "5752463190", "5752463190", "5752463190", "5752463190", "5752463190", "5752463190" },
    ["Dark Night"] = { "6285719338", "6285721078", "6285722964", "6285724682", "6285726335", "6285730635" },
    ["Bright Pink"] = { "271042516", "271077243", "271042556", "271042310", "271042467", "271077958" },
    ["Purple Sky"] = { "570557514", "570557775", "570557559", "570557620", "570557672", "570557727" },
    ["Galaxy"] = { "15125283003", "15125281008", "15125277539", "15125279325", "15125274388", "15125275800" },
}
local hit_sounds = {
    ["Dink"]      = "rbxassetid://988593556";
    ["TF2"]       = "rbxassetid://8255306220";
    ["Gamesense"] = "rbxassetid://4817809188";
    ["Rust"]      = "rbxassetid://1255040462";
    ["Neverlose"] = "rbxassetid://8726881116";
    ["Bubble"]    = "rbxassetid://198598793";
    ["Quake"]     = "rbxassetid://1455817260";
    ["Among-Us"]  = "rbxassetid://7227567562";
    ["Ding"]      = "rbxassetid://2868331684";
    ["Minecraft"] = "rbxassetid://6361963422";
    ["Blackout"]  = "rbxassetid://3748776946";
    ["Osu"]       = "rbxassetid://7151989073"
};
for v in pairs(skyboxes) do
    table.insert(skys, v)
end
for name in pairs(hit_sounds) do 
    table.insert(sounds, name) 
end;

-- esp library
local highlight_player = nil
local player_esp = {
    player_cache = {},
    drawing_cache = {},

    childadded_connections = {},
    childremoved_connections = {},
    functions = {},
}

-- targetting
local entry, closest_part
local target_Paradox = {
    target = {
        entry = nil,
        part = nil,
        distance = math.huge,
    },
}

local quick_stack_funcs = setmetatable({}, {
    __mode = "v",
})

local scan_gc = function()
    local stuff = getgc(false)

    for _, v in getreg() do
        if type(v) == "function" then
            table.insert(stuff, v)
        end
    end

    for _, func in stuff do
        if type(func) ~= "function" or isexecutorclosure(func) or iscclosure(func) or isfunctionhooked(func) then
            continue
        end

        local info = debug.getinfo(func)
        local func_name = info.name

        if func_name == "QuickStackItem" then
            print(func_name)
            table.insert(quick_stack_funcs, func)
        end
    end
end

scan_gc()

-- Добавляем обработку при добавлении нового персонажа
local_player.CharacterAdded:Connect(function()
    task.wait(1)
    scan_gc()
end)
--
do -- functions
    player_esp.functions.create_drawing = function(class, prop, ...)
        local inst = (typeof(class) == "string") and (Instance.new(class)) or (class)
        for property, val in pairs(prop) do
            inst[property] = val
        end
        for _, t in { ... } do
            table.insert(t, inst)
        end
        return inst
    end
    --  
    player_esp.functions.get_armor_icon = function(Character)
        local icons = {}
    
        for _, child in ipairs(Character:GetChildren()) do
            if child and child.Name then
                local armorNumber, skinName = child.Name:match("Armor_(%d+)/(.*)")
                if armorNumber then
                    local key = tonumber(armorNumber)
                    if key then
                        local item = items_table[key]
                        if item and item.Type == "Armor" and item.Image then
                            if type(item.Image) == "table" then
                                if skinName and item.Image[skinName] then
                                    table.insert(icons, item.Image[skinName])
                                elseif item.Image.Default then
                                    table.insert(icons, item.Image.Default)
                                end
                            elseif type(item.Image) == "string" then
                                table.insert(icons, item.Image)
                            end
                        end
                    end
                end
            end
        end
    
        if #icons > 0 then
            return icons
        end
    
        return nil
    end; 
    --
    player_esp.functions.get_boundings = function(model)
        local hrp = model:FindFirstChild("HumanoidRootPart")
        if hrp then
            local cframe, size = model:GetBoundingBox()
            size = Vector3.new(math.min(size.X, 2.5), math.min(size.Y, 6), math.min(size.Z, 5))
            return hrp.CFrame, size, hrp.Position
        end
        return nil
    end
    --
    player_esp.functions.is_visible = function(character)
        if not character or character == local_player.Character then
            return false
        end

        local part = character.PrimaryPart
        if not part then
            return false
        end

        local raycast_params = RaycastParams.new()
        raycast_params.FilterType = Enum.RaycastFilterType.Blacklist
        local ignore_list = { local_player.Character, camera }

        if folder then
            for _, model in ipairs(folder:GetChildren()) do
                if model:IsA("Model") then
                    table.insert(ignore_list, model)
                end
            end
        end

        raycast_params.FilterDescendantsInstances = ignore_list

        local RaycastResult =
        Workspace:Raycast(camera.CFrame.p, (part.Position - camera.CFrame.p).Unit * 10000, raycast_params)
        return RaycastResult and RaycastResult.Instance and RaycastResult.Instance:IsDescendantOf(character)
    end
    --
    player_esp.functions.is_visible2 = function(character)
        if not character or character == local_player.Character then
            return false
        end
    
        local character_model = character.Character
        if not character_model then
            return false
        end
    
        local part = character_model:FindFirstChild("HumanoidRootPart")
        if not part then
            return false
        end
    
        local raycast_params = RaycastParams.new()
        raycast_params.FilterType = Enum.RaycastFilterType.Blacklist
        local ignore_list = { local_player.Character, camera }
    
        if folder then
            for _, model in ipairs(folder:GetChildren()) do
                if model:IsA("Model") then
                    table.insert(ignore_list, model)
                end
            end
        end
    
        raycast_params.FilterDescendantsInstances = ignore_list
    
        local RaycastResult = Workspace:Raycast(camera.CFrame.p, (part.Position - camera.CFrame.p).Unit * 10000, raycast_params)
    
        return RaycastResult and RaycastResult.Instance and RaycastResult.Instance:IsDescendantOf(character_model)
    end    
    --
    local function cache_character_parts(character)
        local humanoid = character:FindFirstChildOfClass("Humanoid")
        if not humanoid then
            return {}
        end

        if humanoid.RigType == Enum.HumanoidRigType.R6 then
            return { "Head", "Torso", "LeftLeg", "RightLeg", "LeftArm", "RightArm" }
        elseif humanoid.RigType == Enum.HumanoidRigType.R15 then
            return {
                "Head",
                "UpperTorso",
                "LowerTorso",
                "LeftUpperArm",
                "RightUpperArm",
                "LeftLowerArm",
                "RightLowerArm",
                "LeftLeg",
                "RightLeg",
                "LeftFoot",
                "RightFoot",
            }
        end
        return {}
    end
    --
    player_esp.functions.get_player = function()
        target_Paradox.target.entry = nil
        target_Paradox.target.part = nil
        target_Paradox.target.distance = Flags["Enable FOV"]
                and Flags["FOV Radius"] / math.tan(math.rad(camera.FieldOfView / 2))
                or math.huge

        local mouse_position, cam_position = uis:GetMouseLocation(), camera.CFrame.Position
        local closest_entry, closest_part = nil, nil
        local part_cache = {}
        local ignore_team_check = #teams == 0 or (#teams == 1 and Flags["Friendly Check"])

        for _, player in ipairs(players:GetPlayers()) do
            if player == local_player then
                continue
            end
            local character = player.Character
            if character and character:FindFirstChild("Humanoid") then
                if
                not Flags["Friendly Check"]
                        or ignore_team_check
                        or player ~= local_player
                        and (not player.Team or not local_player.Team or player.Team ~= local_player.Team)
                then
                else
                    continue
                end

                if not part_cache[player] then
                    part_cache[player] = cache_character_parts(character)
                end

                local bones_to_check = Flags["Aimbot Bone"] == "Closest Bone" and part_cache[player]
                        or { Flags["Aimbot Bone"] }
                for _, part_name in ipairs(bones_to_check) do
                    local part = character:FindFirstChild(part_name)
                    if part then
                        local screen_pos, on_screen = camera:WorldToViewportPoint(part.Position)
                        if on_screen then
                            local mouse_distance = (Vector2.new(screen_pos.X, screen_pos.Y) - Vector2.new(
                                    mouse_position.X,
                                    mouse_position.Y
                            )).Magnitude
                            local distance_to_target = (part.Position - cam_position).Magnitude
                            local is_part_visible = not Flags["Visible Check"]
                                    or (player ~= local_player and player_esp.functions.is_visible(character))
                            if
                            distance_to_target <= (Flags["Aimbot Max Distance"] or math.huge) and is_part_visible
                            then
                                if
                                mouse_distance < target_Paradox.target.distance
                                        or (
                                        mouse_distance == target_Paradox.target.distance
                                                and distance_to_target
                                                < (target_Paradox.target.part and (
                                                target_Paradox.target.part.Position - cam_position
                                        ).Magnitude or math.huge)
                                )
                                then
                                    closest_entry = player
                                    closest_part = part
                                    target_Paradox.target.distance = mouse_distance
                                end
                            end
                        end
                    end
                end
            end
        end

        return closest_entry, closest_part
    end
    --
    player_esp.functions.get_tool = function(Character)
        for _, value in pairs(Character:GetChildren()) do
            if
            value.Name ~= "HolsterModel"
                    and value:IsA("Model")
                    and value.Name ~= "Hair"
                    and (value:FindFirstChild("Detail") or value:FindFirstChild("Main") or value:FindFirstChild("Handle") or value:FindFirstChild(
                    "Attachments"
            ) or value:FindFirstChild("ArrowAttach") or value:FindFirstChild("Attach"))
                    and value.PrimaryPart
            then
                return value.Name
            end
        end
        return "None"
    end
end
--
do -- player esp
    player_esp.update_player_ESP = function(player)
        if player.Name == lplayer_name then
            return
        end
        --
        local esp_holder =
        player_esp.functions.create_drawing("ScreenGui", { Parent = cloneref(gethui()), Name = "esp_holder" })
        local drawings = {
            -- text
            name = player_esp.functions.create_drawing("TextLabel", {
                Visible = false,
                Parent = esp_holder,
                BackgroundTransparency = 1,
                TextStrokeTransparency = 0,
                TextStrokeColor3 = Color3.fromRGB(0, 0, 0),
                FontFace = Font.new([[rbxassetid://12187365977]], Enum.FontWeight.SemiBold, Enum.FontStyle.Normal),
                TextSize = 11,
                TextXAlignment = Enum.TextXAlignment.Center,
            }),
            distance = player_esp.functions.create_drawing("TextLabel", {
                Visible = false,
                Parent = esp_holder,
                BackgroundTransparency = 1,
                TextStrokeTransparency = 0,
                TextStrokeColor3 = Color3.fromRGB(0, 0, 0),
                FontFace = Font.new([[rbxassetid://12187365977]], Enum.FontWeight.SemiBold, Enum.FontStyle.Normal),
                TextSize = 11,
                TextXAlignment = Enum.TextXAlignment.Left,
                TextYAlignment = Enum.TextYAlignment.Top,
            }),
            weapon = player_esp.functions.create_drawing("TextLabel", {
                Visible = false,
                Parent = esp_holder,
                BackgroundTransparency = 1,
                TextStrokeTransparency = 0,
                TextStrokeColor3 = Color3.fromRGB(0, 0, 0),
                FontFace = Font.new([[rbxassetid://12187365977]], Enum.FontWeight.SemiBold, Enum.FontStyle.Normal),
                TextSize = 11,
                TextXAlignment = Enum.TextXAlignment.Center,
            }),

            -- box
            box = player_esp.functions.create_drawing("Frame", {
                Visible = false,
                Parent = esp_holder,
                BackgroundTransparency = 1,
                BackgroundColor3 = Color3.new(1, 1, 1),
                ZIndex = -999,
            }),
            box2 = player_esp.functions.create_drawing("Frame", {
                Visible = false,
                Parent = esp_holder,
                BackgroundTransparency = 1,
                BackgroundColor3 = Color3.new(1, 1, 1),
                ZIndex = 999,
            }),
            Outline = player_esp.functions.create_drawing("UIStroke", {
                Color = Color3.fromRGB(0, 0, 0),
                Thickness = 3,
                Transparency = 0,
                LineJoinMode = Enum.LineJoinMode.Miter,
                Enabled = true,
            }),
            Outline2 = player_esp.functions.create_drawing("UIStroke", {
                Thickness = 1,
                Enabled = true,
                LineJoinMode = Enum.LineJoinMode.Miter,
            }),

            -- healthbar
            BehindHealthbar = player_esp.functions.create_drawing("Frame", {
                Visible = false,
                Parent = esp_holder,
                ZIndex = -1,
                BackgroundColor3 = Color3.fromRGB(0, 0, 0),
                BackgroundTransparency = 0,
                BorderSizePixel = 1,
                BorderColor3 = Color3.fromRGB(0, 0, 0),
            }),
            Healthbar = player_esp.functions.create_drawing("Frame", {
                Visible = false,
                Parent = esp_holder,
                BackgroundColor3 = Color3.fromRGB(255, 255, 255),
                BackgroundTransparency = 0,
                BorderSizePixel = 0,
            }),
            HealthText = player_esp.functions.create_drawing("TextLabel", {
                Visible = false,
                Parent = esp_holder,
                TextXAlignment = Enum.TextXAlignment.Right,
                FontFace = Font.new([[rbxassetid://12187365977]], Enum.FontWeight.SemiBold, Enum.FontStyle.Normal),
                TextSize = 11,
                BackgroundTransparency = 1,
                TextStrokeTransparency = 0,
                ZIndex = 999,
                TextStrokeColor3 = Color3.new(0, 0, 0),
            }),

            -- icon top
            icon_top = player_esp.functions.create_drawing("ImageLabel", {
                Image = "rbxassetid://14239914881",
                Parent = esp_holder,
                BackgroundTransparency = 1,
                AnchorPoint = Vector2.new(0.5, 0.5)
            }),

            -- text strokes
            name_stroke = player_esp.functions.create_drawing("UIStroke", {
                Thickness = 1,
                LineJoinMode = Enum.LineJoinMode.Round,
                ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual,
                Color = Color3.fromRGB(0, 0, 0),
            }),
            distance_stroke = player_esp.functions.create_drawing("UIStroke", {
                Thickness = 1,
                LineJoinMode = Enum.LineJoinMode.Round,
                ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual,
                Color = Color3.fromRGB(0, 0, 0),
            }),
            weapon_stroke = player_esp.functions.create_drawing("UIStroke", {
                Thickness = 1,
                LineJoinMode = Enum.LineJoinMode.Round,
                ApplyStrokeMode = Enum.ApplyStrokeMode.Contextual,
                Color = Color3.fromRGB(0, 0, 0),
            }),
        }
        --
        local chams
        do -- create chams
            chams = Instance.new("Highlight", player.Character)
            chams.Enabled = false
        end
        --
        local linepos = function(frame, from, to, thickness, plrpos)
            local c = (from + to) / 2
            local o = to - from + Vector2.new(0, 1)
            local d = (plrpos - camera.CFrame.p).Magnitude
            frame.Position = UDim2.new(0, c.X, 0, c.Y)
            frame.Rotation = math.atan2(o.Y, o.X) * 180 / math.pi

            frame.Size = UDim2.new(0, o.Magnitude, 0, thickness)
        end
        --
        local function createline()
            local frame = Instance.new("Frame", esp_holder)
            frame.AnchorPoint = Vector2.new(0.5, 0.5)
            frame.ZIndex = -999
            frame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
            frame.BorderColor3 = Color3.fromRGB(0, 0, 0)
            frame.BorderSizePixel = 0
            frame.Visible = false
            return frame
        end
        --
        local viewangle_parts = {
            Head = createline(),
        }
        local parts = {
            UpperTorso_LowerTorso = createline(),
            UpperTorso_LeftUpperArm = createline(),
            UpperTorso_RightUpperArm = createline(),
            LeftUpperArm_LeftLowerArm = createline(),
            LeftLowerArm_LeftHand = createline(),
            LeftUpperLeg_LeftLowerLeg = createline(),
            LeftLowerLeg_LeftFoot = createline(),
            RightUpperArm_RightLowerArm = createline(),
            RightLowerArm_RightHand = createline(),
            RightUpperLeg_RightLowerLeg = createline(),
            RightLowerLeg_RightFoot = createline(),
            LowerTorso_LeftUpperLeg = createline(),
            LowerTorso_RightUpperLeg = createline(),
        }
        --
        drawings.Outline.Parent = drawings.box
        drawings.Outline2.Parent = drawings.box2
        drawings.name_stroke.Parent = drawings.name
        drawings.distance_stroke.Parent = drawings.distance
        drawings.weapon_stroke.Parent = drawings.weapon
        --
        table.insert(player_esp.drawing_cache, {
            name = drawings.name,
            distance = drawings.distance,
            weapon = drawings.weapon,
            box = drawings.box,
            box2 = drawings.box2,
            icon_top = drawings.icon_top,
            healthbar = drawings.BehindHealthbar,
            healthbar2 = drawings.Healthbar,
            esp_holder = esp_holder,
            update_conn = nil,
            player = player,
        })
        --
        do -- update esp
            local died_check = false
            local tween = {
                health_start = 0,
                health_transition_start = 0,
                health_transition_old = 0,
                current_health = 0,
            }
            --
            player_esp.update_esp = LPH_NO_VIRTUALIZE(function()
                local character = player.Character
                local humanoid = character:FindFirstChild("Humanoid")
                local cframe, size, position = player_esp.functions.get_boundings(character)

                do -- ESP
                    if character and humanoid then
                        local max_distance
                        if position then
                            max_distance = (position - camera.CFrame.Position).Magnitude
                        end
                        if max_distance and max_distance < Flags["Max ESP Distance"] then
                            if
                            not Flags["Enable TeamCheck"]
                                    or (
                                    player ~= local_player
                                            and (local_player.Team ~= player.Team or not local_player.Team or not player.Team)
                            )
                            then
                                local pos, on_screen = camera:WorldToScreenPoint(position)
                                local height = math.tan(math.rad(camera.FieldOfView / 2)) * 2 * pos.Z
                                local scale = Vector2.new(
                                        (viewport_size.Y / height) * size.X,
                                        (viewport_size.Y / height) * size.Y
                                )

                                do -- names
                                    if on_screen and Flags["Enable Names"] then
                                        drawings.name.Visible = true
                                        drawings.name.Text = Flags["Display Name"] and player.DisplayName or player.Name
                                        drawings.name.TextColor3 = drawings.name.TextColor3:Lerp(
                                                (
                                                        Flags["Highlight ESP"]
                                                                and highlight_player == player
                                                                and Flags["Highlight Accent"]
                                                )
                                                        and Flags["Highlight Accent"]
                                                        or Flags["Names Accent"],
                                                0.02
                                        )
                                        drawings.name.Position = UDim2.new(0, pos.X, 0, pos.Y - scale.Y / 2 - 6)
                                    else
                                        drawings.name.Visible = false
                                    end
                                end

                                do -- chams
                                    if on_screen and Flags["Enable Chams"] then
                                        chams.Enabled = true
                                        chams.DepthMode = Flags["Chams Visible Check"] == "Always Visible"
                                                and Enum.HighlightDepthMode["AlwaysOnTop"]
                                                or Enum.HighlightDepthMode["Occluded"]
                                        chams.FillTransparency = Flags["Chams Opacity"] / 100
                                        chams.OutlineTransparency = Flags["Chams Opacity"] / 100
                                        chams.FillColor = chams.FillColor:Lerp(
                                                (
                                                        Flags["Highlight ESP"]
                                                                and highlight_player == player
                                                                and Flags["Highlight Accent"]
                                                )
                                                        and Flags["Highlight Accent"]
                                                        or Flags["Chams Accent"],
                                                0.02
                                        )
                                        chams.OutlineColor = chams.OutlineColor:Lerp(
                                                (
                                                        Flags["Highlight ESP"]
                                                                and highlight_player == player
                                                                and Flags["Highlight Accent"]
                                                )
                                                        and Flags["Highlight Accent"]
                                                        or Flags["Chams Accent"],
                                                0.02
                                        )
                                    else
                                        chams.Enabled = false
                                    end
                                end

                                do -- view angle
                                    if
                                    on_screen
                                            and Flags["Enable Viewangle"]
                                            and character
                                            and character:FindFirstChild("Head")
                                    then
                                        local head_pos = camera:WorldToScreenPoint(character.Head.Position)
                                        local look_dir = character.Head.CFrame.LookVector
                                        local line_length = Flags["Viewangle Length"]
                                        local end_pos = character.Head.Position + (look_dir * line_length)
                                        local end_point = camera:WorldToScreenPoint(end_pos)

                                        local positions = {
                                            Head = Vector2.new(head_pos.X, head_pos.Y),
                                            Look = Vector2.new(end_point.X, end_point.Y),
                                        }

                                        for _, part in pairs(viewangle_parts) do
                                            part.BackgroundColor3 = part.BackgroundColor3:Lerp(
                                                    (
                                                            Flags["Highlight ESP"]
                                                                    and highlight_player == player
                                                                    and Flags["Highlight Accent"]
                                                    )
                                                            and Flags["Highlight Accent"]
                                                            or Flags["Viewangle Accent"],
                                                    0.02
                                            )
                                        end

                                        if Flags["Outline Skeletons/ViewAngle"] then
                                            for _, part in pairs(viewangle_parts) do
                                                part.BorderSizePixel = 1
                                            end
                                        else
                                            for _, part in pairs(viewangle_parts) do
                                                part.BorderSizePixel = 0
                                            end
                                        end

                                        if viewangle_parts.Head then
                                            linepos(
                                                    viewangle_parts.Head,
                                                    positions.Head,
                                                    positions.Look,
                                                    1,
                                                    character.Head.Position
                                            )
                                            for _, part in pairs(viewangle_parts) do
                                                part.Visible = true
                                            end
                                        end
                                    else
                                        for _, part in pairs(viewangle_parts) do
                                            part.Visible = false
                                        end
                                    end
                                end

                                do -- skeletons
                                    if
                                    on_screen
                                            and Flags["Enable Skeletons"]
                                            and character
                                            and character:FindFirstChild("UpperTorso")
                                            and character:FindFirstChild("HumanoidRootPart")
                                    then
                                        local get_parts = {
                                            "UpperTorso",
                                            "LowerTorso",
                                            "LeftUpperArm",
                                            "LeftLowerArm",
                                            "LeftHand",
                                            "RightUpperArm",
                                            "RightLowerArm",
                                            "RightHand",
                                            "LeftUpperLeg",
                                            "LeftLowerLeg",
                                            "LeftFoot",
                                            "RightUpperLeg",
                                            "RightLowerLeg",
                                            "RightFoot",
                                        }

                                        for _, v in pairs(get_parts) do
                                            if not character:FindFirstChild(v) then
                                                return
                                            end
                                        end

                                        local positions = {}
                                        for _, v in pairs(get_parts) do
                                            local world_pos = character[v].Position
                                            local screenPos, onScreen = camera:WorldToScreenPoint(world_pos)

                                            if onScreen and screenPos.Z > 0 then
                                                positions[v] = Vector2.new(screenPos.X, screenPos.Y)
                                            else
                                                return
                                            end
                                        end

                                        local border_size = Flags["Outline Skeletons/ViewAngle"] and 1 or 0
                                        for _, part in pairs(parts) do
                                            part.BorderSizePixel = border_size
                                        end

                                        local root = character.HumanoidRootPart.Position
                                        linepos(
                                                parts.UpperTorso_LowerTorso,
                                                positions.UpperTorso,
                                                positions.LowerTorso,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.UpperTorso_LeftUpperArm,
                                                positions.UpperTorso,
                                                positions.LeftUpperArm,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.LeftUpperArm_LeftLowerArm,
                                                positions.LeftUpperArm,
                                                positions.LeftLowerArm,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.LeftLowerArm_LeftHand,
                                                positions.LeftLowerArm,
                                                positions.LeftHand,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.UpperTorso_RightUpperArm,
                                                positions.UpperTorso,
                                                positions.RightUpperArm,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.RightUpperArm_RightLowerArm,
                                                positions.RightUpperArm,
                                                positions.RightLowerArm,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.RightLowerArm_RightHand,
                                                positions.RightLowerArm,
                                                positions.RightHand,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.LowerTorso_LeftUpperLeg,
                                                positions.LowerTorso,
                                                positions.LeftUpperLeg,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.LeftUpperLeg_LeftLowerLeg,
                                                positions.LeftUpperLeg,
                                                positions.LeftLowerLeg,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.LeftLowerLeg_LeftFoot,
                                                positions.LeftLowerLeg,
                                                positions.LeftFoot,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.LowerTorso_RightUpperLeg,
                                                positions.LowerTorso,
                                                positions.RightUpperLeg,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.RightUpperLeg_RightLowerLeg,
                                                positions.RightUpperLeg,
                                                positions.RightLowerLeg,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )
                                        linepos(
                                                parts.RightLowerLeg_RightFoot,
                                                positions.RightLowerLeg,
                                                positions.RightFoot,
                                                Flags["Skeleton Thickness"],
                                                root
                                        )

                                        for _, part in pairs(parts) do
                                            part.Visible = true
                                        end
                                    else
                                        for _, part in pairs(parts) do
                                            part.Visible = false
                                        end
                                    end
                                end

                                do -- boxes
                                    if on_screen and Flags["Enable Boxes"] then
                                        drawings.box.Size = UDim2.new(0, scale.X - 1, 0, scale.Y - 1)
                                        drawings.box2.Size = UDim2.new(0, scale.X + 1, 0, scale.Y + 1)

                                        drawings.box.Position =
                                        UDim2.new(0, pos.X - (scale.X / 2), 0, pos.Y - (scale.Y / 2) + 3)
                                        drawings.box2.Position =
                                        UDim2.new(0, pos.X - (scale.X / 2) - 1, 0, pos.Y - (scale.Y / 2) + 2)

                                        if Flags["Boxes Filled"] then
                                            drawings.box.BackgroundTransparency = Flags["Boxes Filled Opacity"] / 100
                                            drawings.box.BackgroundColor3 = Flags["Filled Boxes Accent"]
                                        else
                                            drawings.box.BackgroundTransparency = 1
                                        end

                                        drawings.box.Visible = true
                                        drawings.box2.Visible = true
                                        drawings.Outline2.Color = drawings.Outline2.Color:Lerp(
                                                (
                                                        Flags["Highlight ESP"]
                                                                and highlight_player == player
                                                                and Flags["Highlight Accent"]
                                                )
                                                        and Flags["Highlight Accent"]
                                                        or Flags["Boxes Accent"],
                                                0.02
                                        )
                                    else
                                        drawings.box.Visible = false
                                        drawings.box2.Visible = false
                                    end
                                end

                                do -- distances
                                    if on_screen and Flags["Enable Distance"] then
                                        local box_side = pos.X + (scale.X / 2)
                                        local box_top = pos.Y - (scale.Y / 2)
                                        drawings.distance.Position = UDim2.new(0, box_side + 3, 0, box_top - 1)
                                        drawings.distance.Visible = true
                                        drawings.distance.TextColor3 = drawings.distance.TextColor3:Lerp(
                                                (
                                                        Flags["Highlight ESP"]
                                                                and highlight_player == player
                                                                and Flags["Highlight Accent"]
                                                )
                                                        and Flags["Highlight Accent"]
                                                        or Flags["Distance Accent"],
                                                0.02
                                        )
                                        drawings.distance.Text = string.format("%ds", math.floor(max_distance))
                                    else
                                        drawings.distance.Visible = false
                                    end
                                end

                                do -- weapon
                                    if on_screen and Flags["Enable Weapon"] then
                                        drawings.weapon.Visible = true
                                        drawings.weapon.Position = UDim2.new(0, pos.X - 2, 0, pos.Y + scale.Y / 2 + 11)
                                        drawings.weapon.TextColor3 = drawings.weapon.TextColor3:Lerp(
                                                (
                                                        Flags["Highlight ESP"]
                                                                and highlight_player == player
                                                                and Flags["Highlight Accent"]
                                                )
                                                        and Flags["Highlight Accent"]
                                                        or Flags["Weapon Accent"],
                                                0.02
                                        )
                                        drawings.weapon.Text = player_esp.functions.get_tool(character)
                                    else
                                        drawings.weapon.Visible = false
                                    end
                                end

                                do -- top icon
                                    if (on_screen) and Flags["Enable Visible Icons"] then
                                        if player_esp.functions.is_visible(character) then
                                            drawings.icon_top.Image = "rbxassetid://14239914881";
                                            drawings.icon_top.ImageColor3 = drawings.icon_top.ImageColor3:Lerp(Color3.fromRGB(0, 255, 0), 0.02);
                                        else
                                            drawings.icon_top.Image = "rbxassetid://14239920245";
                                            drawings.icon_top.ImageColor3 = drawings.icon_top.ImageColor3:Lerp(Flags["Visible Icon Accent"], 0.02);
                                        end;
                                        drawings.icon_top.Visible = true;
                                        drawings.icon_top.Position = UDim2.new(0, pos.X, 0, pos.Y - scale.Y / 2 - 25);
                                        drawings.icon_top.Size = UDim2.new(0, 28, 0, 28);
                                    else
                                        drawings.icon_top.Visible = false;
                                    end;
                                end;

                                do -- healthbar
                                    if on_screen and Flags["Enable Healthbar"] and humanoid and humanoid.Health then
                                        local health, max_health = math.floor(humanoid.Health), humanoid.MaxHealth
                                        local health_color =
                                        Color3.new(1, 0, 0)
                                              :lerp(Color3.new(0.7, 0.8, 0), math.clamp(health / max_health, 0, 1))
                                        health_color = health_color:lerp(
                                                Color3.new(0, 1, 0),
                                                math.clamp((health / max_health - 0.5) * 2, 0, 1)
                                        )

                                        do -- healthbar animation
                                            tween.health_start = tween.health_start or 0
                                            tween.health_transition_start = tween.health_transition_start or health
                                            tween.health_transition_old = tween.health_transition_old or health
                                            tween.current_health = tween.current_health or health

                                            if health ~= tween.health_transition_start then
                                                tween.health_transition_old, tween.health_transition_start, tween.health_start =
                                                tween.current_health, health, tick()
                                            end
                                        end

                                        local progress = math.clamp((tick() - tween.health_start) / 0.2, 0, 1)
                                        tween.current_health = tween.health_transition_old
                                                + (tween.health_transition_start - tween.health_transition_old)
                                                * progress

                                        if progress >= 1 then
                                            tween.current_health, tween.health_transition_old, tween.health_transition_start, tween.health_start =
                                            health, health, health, 0
                                        end

                                        if Flags["Enable Healthtext"] then
                                            drawings.HealthText.TextColor3 = health_color
                                            drawings.HealthText.Text, drawings.HealthText.Visible =
                                            tostring(math.floor(tween.current_health)), true
                                        else
                                            drawings.HealthText.Visible = false
                                        end

                                        drawings.Healthbar.Visible = true
                                        drawings.BehindHealthbar.Visible = true

                                        local bar_width = 1
                                        local bar_height_adjust = 3
                                        local target_height = scale.Y * (tween.current_health / max_health)
                                                + bar_height_adjust

                                        local bar_offset = 5 + math.max(0, bar_width)
                                        drawings.Healthbar.Position = UDim2.new(
                                                0,
                                                pos.X - scale.X / 2 - bar_offset,
                                                0,
                                                pos.Y - scale.Y / 2 + scale.Y * (1 - tween.current_health / max_health) + 1
                                        )
                                        drawings.BehindHealthbar.Position =
                                        UDim2.new(0, pos.X - scale.X / 2 - bar_offset, 0, pos.Y - scale.Y / 2 + 1)
                                        drawings.Healthbar.Size = UDim2.new(0, bar_width, 0, target_height)
                                        drawings.BehindHealthbar.Size =
                                        UDim2.new(0, bar_width, 0, scale.Y + bar_height_adjust)
                                        local target_text_pos = UDim2.new(
                                                0,
                                                pos.X - scale.X / 2 - 9,
                                                0,
                                                pos.Y - scale.Y / 2 + scale.Y * (1 - tween.current_health / max_health) + 5
                                        ) or UDim2.new(
                                                0,
                                                pos.X - scale.X / 2 + scale.X * (tween.current_health / max_health),
                                                0,
                                                pos.Y + scale.Y / 2 + 5
                                        )
                                        drawings.HealthText.Position =
                                        drawings.HealthText.Position:Lerp(target_text_pos, progress)
                                        drawings.Healthbar.BackgroundColor3 = health_color
                                    else
                                        drawings.Healthbar.Visible = false
                                        drawings.BehindHealthbar.Visible = false
                                        drawings.HealthText.Visible = false
                                    end
                                    --
                                end
                            else
                                for _, v in ipairs({
                                    "icon_top",
                                    "name",
                                    "box",
                                    "box2",
                                    "distance",
                                    "weapon",
                                    "BehindHealthbar",
                                    "HealthText",
                                    "Healthbar",
                                }) do
                                    drawings[v].Visible = false
                                end
                                for _, part in pairs(parts) do
                                    part.Visible = false
                                end
                                for _, part in pairs(viewangle_parts) do
                                    part.Visible = false
                                end
                                chams.Enabled = false
                            end
                        else
                            for _, v in ipairs({
                                "icon_top",
                                "name",
                                "box",
                                "box2",
                                "distance",
                                "weapon",
                                "BehindHealthbar",
                                "HealthText",
                                "Healthbar",
                            }) do
                                drawings[v].Visible = false
                            end
                            for _, part in pairs(parts) do
                                part.Visible = false
                            end
                            for _, part in pairs(viewangle_parts) do
                                part.Visible = false
                            end
                            chams.Enabled = false
                        end
                    else
                        for _, v in ipairs({
                            "icon_top",
                            "name",
                            "box",
                            "box2",
                            "distance",
                            "weapon",
                            "BehindHealthbar",
                            "HealthText",
                            "Healthbar",
                        }) do
                            drawings[v].Visible = false
                        end
                        for _, part in pairs(parts) do
                            part.Visible = false
                        end
                        for _, part in pairs(viewangle_parts) do
                            part.Visible = false
                        end
                        chams.Enabled = false
                    end
                end
            end)
        end

        local cache = player_esp.drawing_cache[#player_esp.drawing_cache]
        cache.update_conn = run_service["RenderStepped"]:Connect(player_esp.update_esp)
    end
    --
    player_esp.remove_player_ESP = function(player)
        for _, cache in ipairs(player_esp.drawing_cache) do
            if cache.player == player then
                if cache.update_conn then
                    cache.update_conn:Disconnect()
                end
                cache.esp_holder:Destroy()
                table.remove(player_esp.drawing_cache, _)
                break
            end
        end
    end
    --
    player_esp.on_player_added = function(player)
        local connections = {}
        connections.character_added = player.CharacterAdded:Connect(function()
            player_esp.update_player_ESP(player)
        end)
        connections.character_removing = player.CharacterRemoving:Connect(function()
            player_esp.remove_player_ESP(player)
        end)

        player_esp.connection_cache = player_esp.connection_cache or {}
        table.insert(player_esp.connection_cache, { player = player, connections = connections })

        if player.Character then
            player_esp.update_player_ESP(player)
        end
    end
    --
    player_esp.handle_existing_players = function()
        local player_list = players:GetPlayers()
        for i = 1, #player_list do
            local player = player_list[i]
            if player.Name ~= lplayer_name then
                player_esp.on_player_added(player)
            end
        end
    end
    --
    players.PlayerAdded:Connect(player_esp.on_player_added)
    players.PlayerRemoving:Connect(function(player)
        player_esp.remove_player_ESP(player)
        for i, cache in ipairs(player_esp.connection_cache) do
            if cache.player == player then
                for _, conn in pairs(cache.connections) do
                    conn:Disconnect()
                end
                table.remove(player_esp.connection_cache, i)
                break
            end
        end
    end)
    player_esp.handle_existing_players()
end
--
local ui = {
    window = nil,
    sections = {},
    tabs = {},
}
ui.window = Library:New({ Size = UDim2.new(0, 600, 0, 650) })
--
getgenv().silent = false
getgenv().silent_key = false
getgenv().silent_hitchance = 100
getgenv().shoot_while_jumping = false

do -- menu
    do -- combat
        ui.window:Seperator({ Name = "Combat Settings" })

        ui.tabs["General"] = ui.window:Page({
            Name = "General",
            Icon = "http://www.roblox.com/asset/?id=6023426921",
        })
        do
            ui.sections["legit_main"] = ui.tabs["General"]:Section({ Name = "Main", Side = "Left", Size = 380 })
            ui.sections["legit_main"]:Toggle({ Name = "Aimbot", Flag = "Enable Aimbot" })
            ui.sections["legit_main"]:Toggle({
                Name = "Silent Aim",
                Flag = "Enable Silent Aim",
                Callback = function(state)
                    getgenv().silent = state    
                end,
            })    
            ui.sections["legit_main"]:Toggle({ Name = "Inventory Viewer", Flag = "Armor Viewer" })
            ui.sections["legit_main"]:Toggle({ Name = "Auto Shoot", Flag = "Enable Auto Shoot" })
            ui.sections["legit_main"]:Keybind({
                Flag = "Aimbot Keybind",
                Name = "Aimbot Keybind",
                Default = Enum.UserInputType.MouseButton2,
                Mode = "Hold",
            })
            ui.sections["legit_main"]:Keybind({
                Flag = "Silent Aim Keybind",
                Name = "Silent Aim Keybind",
                Default = Enum.UserInputType.MouseButton1,
                Mode = "Hold",
                Callback = function(state)
                    getgenv().silent_key = state
                end,
            })
            ui.sections["legit_main"]:Slider({
                Name = "Aimbot Speed",
                Flag = "Aimbot Speed",
                Default = 0.5,
                Minimum = 0.05,
                Maximum = 1,
                Decimals = 0.01,
                Ending = "",
            })
            ui.sections["legit_main"]:Slider({
                Name = "Hitchance",
                Flag = "Hitchance",
                Default = 100,
                Minimum = 1,
                Maximum = 100,
                Decimals = 1,
                Ending = "%",
                Callback = function(state)
                    getgenv().silent_hitchance = state
                end,
            })
            ui.sections["legit_main"]:Slider({
                Name = "Max Distance",
                Flag = "Aimbot Max Distance",
                Default = 500,
                Minimum = 100,
                Maximum = 2500,
                Decimals = 1,
                Ending = "s",
            })
            ui.sections["legit_main"]:List({
                Name = "Aimbone",
                Flag = "Aimbot Bone",
                Options = { "Head", "UpperTorso", "LowerTorso", "Closest Bone" },
                Default = "Head",
            })
        end
        
        do
            ui.sections["select_options"] = ui.tabs["General"]:Section({ Name = "Select", Side = "Right", Size = 250 })
            ui.sections["select_options"]:Toggle({ Name = "Team Check", Flag = "Friendly Check" })
            ui.sections["select_options"]:Toggle({ Name = "Visible Check", Flag = "Visible Check" })
            ui.sections["select_options"]:Toggle({ Name = "FOV Circle", Flag = "Enable FOV" })
            ui.sections["select_options"]:Colorpicker({
                Name = "FOV Color",
                Flag = "FOV Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["select_options"]:Slider({
                Name = "FOV Radius",
                Flag = "FOV Radius",
                Default = 120,
                Minimum = 30,
                Maximum = 750,
                Decimals = 1,
                Ending = "px",
            })
            ui.sections["select_options"]:Toggle({ Name = "Snaplines", Flag = "Enable Snaplines" })
            ui.sections["select_options"]:Colorpicker({
                Name = "Snaplines Color",
                Flag = "Snaplines Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
        end
        --

    do -- visuals
        ui.window:Seperator({ Name = "Visuals Settings" })
        ui.tabs["Visuals"] = ui.window:Page({
            Name = "Player",
            Icon = "http://www.roblox.com/asset/?id=6031075931",
        })
        do
            ui.sections["esp"] = ui.tabs["Visuals"]:Section({ Name = "ESP", Side = "Left", Size = 480})
            ui.sections["esp"]:Toggle({ Name = "Names", Flag = "Enable Names" })
            ui.sections["esp"]:Toggle({ Name = "Boxes", Flag = "Enable Boxes" })
            ui.sections["esp"]:Toggle({ Name = "Distance", Flag = "Enable Distance" })
            ui.sections["esp"]:Toggle({ Name = "Weapon", Flag = "Enable Weapon" })
            ui.sections["esp"]:Toggle({ Name = "Healthbar", Flag = "Enable Healthbar" })
            ui.sections["esp"]:Toggle({ Name = "Visible Icons", Flag = "Enable Visible Icons" })
            ui.sections["esp"]:Toggle({ Name = "Healthtext", Flag = "Enable Healthtext" })
            ui.sections["esp"]:Toggle({ Name = "Viewangle", Flag = "Enable Viewangle" })
            ui.sections["esp"]:Slider({
                Name = "Viewangle Length",
                Flag = "Viewangle Length",
                Default = 5,
                Minimum = 1,
                Maximum = 15,
                Decimals = 0.1,
                Ending = "",
            })
            ui.sections["esp"]:Toggle({ Name = "Skeleton", Flag = "Enable Skeletons" })
            ui.sections["esp"]:Slider({
                Name = "Skeleton Thickness",
                Flag = "Skeleton Thickness",
                Default = 1,
                Minimum = 1,
                Maximum = 2,
                Decimals = 0.1,
                Ending = "",
            })
            ui.sections["esp"]:Toggle({ Name = "Chams", Flag = "Enable Chams" })
            ui.sections["esp"]:Slider({
                Name = "Chams Opacity",
                Flag = "Chams Opacity",
                Default = 50,
                Minimum = 0,
                Maximum = 100,
                Decimals = 1,
                Ending = "%",
            })
            ui.sections["esp"]:List({
                Name = "Chams",
                Flag = "Chams Visible Check",
                Options = { "Visible Only", "Always Visible" },
            })
        end
        do
            ui.sections["esp_colors"] = ui.tabs["Visuals"]:Section({ Name = "ESP Colors", Side = "Right", Size = 275 })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Names Color",
                Flag = "Names Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Boxes Color",
                Flag = "Boxes Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Distance Color",
                Flag = "Distance Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Weapon Color",
                Flag = "Weapon Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Visible Icon Color",
                Flag = "Visible Icon Accent",
                Default = Color3.fromRGB(255, 0, 0),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Viewangle Color",
                Flag = "Viewangle Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Skeletons Color",
                Flag = "Skeletons Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Chams Color",
                Flag = "Chams Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["esp_colors"]:Colorpicker({
                Name = "Highlight Color",
                Flag = "Highlight Accent",
                Default = Color3.fromRGB(255, 0, 0),
            })
        end
        do
            ui.sections["esp settings"] =
            ui.tabs["Visuals"]:Section({ Name = "ESP Settings", Side = "Right", Size = 187 })
            ui.sections["esp settings"]:Toggle({ Name = "Highlight Target", Flag = "Highlight ESP" })
            ui.sections["esp settings"]:Toggle({ Name = "Team Check", Flag = "Enable TeamCheck" })
            ui.sections["esp settings"]:Toggle({ Name = "Display Name", Default = true, Flag = "Display Name" })
            ui.sections["esp settings"]:Toggle({
                Name = "Outline Skeletons/ViewAngle",
                Flag = "Outline Skeletons/ViewAngle",
            })
            ui.sections["esp settings"]:Slider({
                Name = "Max ESP Distance",
                Flag = "Max ESP Distance",
                Default = 500,
                Minimum = 100,
                Maximum = 2500,
                Decimals = 1,
                Ending = "s",
            })
        end
        --
        ui.tabs["world"] = ui.window:Page({
            Name = "World",
            Icon = "http://www.roblox.com/asset/?id=6034684930",
        })
        do
            ui.sections["world esp"] = ui.tabs["world"]:Section({ Name = "ESP", Side = "Left", Size = 290 })
            ui.sections["world esp"]:Toggle({ Name = "Enable", Flag = "Enable World ESP" })
            ui.sections["world esp"]:Slider({
                Name = "Max ESP Distance",
                Flag = "World ESP Max Distance",
                Default = 500,
                Minimum = 100,
                Maximum = 2500,
                Decimals = 1,
                Ending = "s",
            })
            ui.sections["world esp"]:Toggle({ Name = "Enable Wool ESP", Flag = "Enable Wool ESP" })
            ui.sections["world esp"]:Colorpicker({
                Name = "Wool ESP Color",
                Flag = "Wool ESP Color",
                Default = Color3.fromRGB(255, 255, 255),
            })
            --
            ui.sections["world esp"]:Toggle({ Name = "Enable Stone ESP", Flag = "Enable Stone ESP" })
            ui.sections["world esp"]:Colorpicker({
                Name = "Stone ESP Color",
                Flag = "Stone ESP Color",
                Default = Color3.fromRGB(255, 255, 255),
            })
            --
            ui.sections["world esp"]:Toggle({ Name = "Enable Metal ESP", Flag = "Enable Metal ESP" })
            ui.sections["world esp"]:Colorpicker({
                Name = "Metal ESP Color",
                Flag = "Metal ESP Color",
                Default = Color3.fromRGB(255, 255, 255),
            })
            --
            ui.sections["world esp"]:Toggle({ Name = "Enable Phosphate ESP", Flag = "Enable Phosphate ESP" })
            ui.sections["world esp"]:Colorpicker({
                Name = "Phosphate ESP Color",
                Flag = "Phosphate ESP Color",
                Default = Color3.fromRGB(255, 255, 255),
            })
        end
        
        do
            ui.sections["world customisation"] =
            ui.tabs["world"]:Section({ Name = "World Customisation", Side = "Right", Size = 250 })
            ui.sections["world customisation"]:Toggle({
                Name = "No Grass",
                Callback = function(state)
                    sethiddenproperty(Workspace:FindFirstChildOfClass("Terrain"), "Decoration", not state)
                end,
            })
            ui.sections["world customisation"]:Toggle({ Name = "No Fog", Flag = "Enable No Fog" })
            ui.sections["world customisation"]:Toggle({
                Name = "Fullbright",
                Flag = "Enable Fullbright",
                Callback = function(state)
                    lighting["GlobalShadows"] = state
                end,
            })
            ui.sections["world customisation"]:Toggle({ Name = "Ambient", Flag = "Enable Ambient" })
            ui.sections["world customisation"]:Colorpicker({
                Name = "Outdoor Ambient Color",
                Flag = "Outdoor Ambient Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["world customisation"]:Colorpicker({
                Name = "Ambient Color",
                Flag = "Ambient Accent",
                Default = Color3.fromRGB(255, 255, 255),
            })
            ui.sections["world customisation"]:List({
                Name = "Skybox",
                Flag = "Skyboxes",
                Options = skys,
                Callback = function(state)
                    if skyboxes[state] then
                        sky.SkyboxBk = "rbxassetid://" .. skyboxes[state][1]
                        sky.SkyboxDn = "rbxassetid://" .. skyboxes[state][2]
                        sky.SkyboxFt = "rbxassetid://" .. skyboxes[state][3]
                        sky.SkyboxLf = "rbxassetid://" .. skyboxes[state][4]
                        sky.SkyboxRt = "rbxassetid://" .. skyboxes[state][5]
                        sky.SkyboxUp = "rbxassetid://" .. skyboxes[state][6]
                    end
                end,
            })
        end
        do
            ui.tabs["local"] = ui.window:Page({
                Name = "Local",
                Icon = "http://www.roblox.com/asset/?id=6034684930",
            })
            do
                ui.sections["viewmodel"] = ui.tabs["local"]:Section({ Name = "Viewmodel", Side = "Right", Size = 137 })
                ui.sections["viewmodel"]:Toggle({ Name = "Local Chams", Flag = "Local Chams" })
                ui.sections["viewmodel"]:Slider({
                    Name = "Local Chams Opacity",
                    Flag = "Local Chams Opacity",
                    Default = 50,
                    Minimum = 0,
                    Maximum = 100,
                    Decimals = 1,
                    Ending = "%",
                })
                ui.sections["viewmodel"]:Colorpicker({
                    Name = "Local Chams Color",
                    Flag = "Local Chams Accent",
                    Default = Color3.fromRGB(255, 255, 255),
                })
            end
            
            --
            do
                ui.sections["Freecam"] = ui.tabs["local"]:Section({ Name = "Freecam", Side = "Left", Size = 160 })
                ui.sections["Freecam"]:Toggle({ Name = "Freecam", Flag = "Enable Freecam" })
                ui.sections["Freecam"]:Keybind({
                    Flag = "Freecam Keybind",
                    Name = "Freecam Keybind",
                    Default = Enum.KeyCode.K,
                    Mode = "Toggle",
                })
                ui.sections["Freecam"]:Slider({
                    Name = "Freecam Speed",
                    Flag = "Freecam Speed",
                    Default = 2,
                    Minimum = 0.5,
                    Maximum = 10,
                    Decimals = 0.1,
                    Ending = "",
                })
                ui.sections["Freecam"]:Slider({
                    Name = "Freecam Sensitivity",
                    Flag = "Freecam Sensitivity",
                    Default = 0.3,
                    Minimum = 0.1,
                    Maximum = 1,
                    Decimals = 0.01,
                    Ending = "",
                })
                ---
                ui.sections["Third"] = ui.tabs["local"]:Section({ Name = "Third Person", Side = "Right", Size = 125 })
                ui.sections["Third"]:Toggle({ Name = "Third Person", Flag = "Third Person" })
                ui.sections["Third"]:Keybind({
                    Flag = "Third Person Key",
                    Name = "Third Person Key",
                    Default = Enum.KeyCode.J,
                    Mode = "Toggle",
                })
                ui.sections["Third"]:Slider({
                    Name = "Third Person Distance",
                    Flag = "Third Person Distance",
                    Default = 5,
                    Minimum = 1,
                    Maximum = 10,
                    Decimals = 0.1,
                    Ending = "",
                })

                ui.sections["Third"] = ui.tabs["local"]:Section({ Name = "Third Person", Side = "Right", Size = 95 })
                ui.sections["Third"]:Toggle({ Name = "Anti Aim", Flag = "Anti Aim" })
                ui.sections["Third"]:Slider({
                    Name = "Anti Aim Speed",
                    Flag = "Anti Aim Speed",
                    Default = 50,
                    Minimum = 1,
                    Maximum = 100,
                    Decimals = 1,
                    Ending = "",
                })
            end
        end
    end
-- Crosshairs

local Drawing = Drawing or {}

local settings = {
    color = Color3.fromRGB(255, 255, 255),
    size = 0.6,
    spinSpeed = 5,
    isSpinning = false,
    visible = false,
    crosshairType = "circle",
}

local crosshair = {}
local screenCenter = Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y / 2)
local rotationAngle = 0

local function updateCenter()
    screenCenter = Vector2.new(workspace.CurrentCamera.ViewportSize.X / 2, workspace.CurrentCamera.ViewportSize.Y / 2)
end

local function createCrosshair(crosshairType)
    if settings.crosshairType ~= crosshairType or not crosshair.object then
        if crosshair.object then
            if type(crosshair.object) == "table" then
                for _, part in pairs(crosshair.object) do
                    if part.Remove then part:Remove() end
                end
            elseif crosshair.object.Remove then
                crosshair.object:Remove()
            end
        end

        settings.crosshairType = crosshairType

        if crosshairType == "circle" then
            local circle = Drawing.new("Circle")
            circle.Filled = false
            circle.Thickness = 2
            crosshair.object = circle
        elseif crosshairType == "square" then
            local square = Drawing.new("Square")
            square.Filled = false
            square.Thickness = 2
            crosshair.object = square
        elseif crosshairType == "x" then
            local line1 = Drawing.new("Line")
            local line2 = Drawing.new("Line")
            line1.Thickness = 2
            line2.Thickness = 2
            crosshair.object = {line1, line2}
        end
    end

    updateDrawings()
end

function updateDrawings()
    if not crosshair.object then return end

    if settings.crosshairType == "circle" then
        local circle = crosshair.object
        circle.Position = screenCenter
        circle.Radius = settings.size * 10
        circle.Color = settings.color
        circle.Visible = settings.visible

    elseif settings.crosshairType == "square" then
        local square = crosshair.object
        square.Position = screenCenter - Vector2.new(settings.size * 5, settings.size * 5)
        square.Size = Vector2.new(settings.size * 10, settings.size * 10)
        square.Color = settings.color
        square.Visible = settings.visible

    elseif settings.crosshairType == "x" then
        local s = settings.size * 5
        local line1 = crosshair.object[1]
        local line2 = crosshair.object[2]

        line1.From = screenCenter - Vector2.new(s, s)
        line1.To = screenCenter + Vector2.new(s, s)
        line2.From = screenCenter + Vector2.new(s, -s)
        line2.To = screenCenter - Vector2.new(s, -s)

        line1.Color = settings.color
        line2.Color = settings.color
        line1.Visible = settings.visible
        line2.Visible = settings.visible
    end
end

local function updateVisibility(visible)
    settings.visible = visible
    updateDrawings()
end

local function updateCrosshairType(newType)
    createCrosshair(newType)
end

local function updateSize(size)
    settings.size = size
    updateDrawings()
end

local function updateColor(color)
    settings.color = color
    updateDrawings()
end

local function updateSpinSpeed(speed)
    settings.spinSpeed = speed
end

local function toggleSpinning(isSpinning)
    settings.isSpinning = isSpinning
end

local function rotateLine(from, to, angle)
    local sin, cos = math.sin(angle), math.cos(angle)
    local relFrom = from - screenCenter
    local relTo = to - screenCenter
    local rotFrom = Vector2.new(relFrom.X * cos - relFrom.Y * sin, relFrom.X * sin + relFrom.Y * cos) + screenCenter
    local rotTo = Vector2.new(relTo.X * cos - relTo.Y * sin, relTo.X * sin + relTo.Y * cos) + screenCenter
    return rotFrom, rotTo
end

local function updateCrosshairRotation()
    if settings.isSpinning and settings.visible and settings.crosshairType == "x" and type(crosshair.object) == "table" then
        rotationAngle = (rotationAngle + settings.spinSpeed) % 360
        local angle = math.rad(rotationAngle)
        local s = settings.size * 5
        local line1 = crosshair.object[1]
        local line2 = crosshair.object[2]
        line1.From, line1.To = rotateLine(screenCenter - Vector2.new(s, s), screenCenter + Vector2.new(s, s), angle)
        line2.From, line2.To = rotateLine(screenCenter + Vector2.new(s, -s), screenCenter - Vector2.new(s, -s), angle)
    end
end

workspace.CurrentCamera:GetPropertyChangedSignal("ViewportSize"):Connect(updateCenter)

ui.sections["Crosshair"] = ui.tabs["local"]:Section({ Name = "Crosshair", Side = "Left", Size = 240 })

ui.sections["Crosshair"]:List({
    Name = "Crosshair Type",
    Flag = "Crosshair_Type",
    Options = {"circle", "square", "x"},
    Default = "circle",
    Callback = function(selectedType)
        updateCrosshairType(selectedType)
    end,
})

ui.sections["Crosshair"]:Toggle({
    Name = "Toggle Crosshair",
    Flag = "Crosshair_Toggle",
    Default = false,
    Callback = function(visible)
        updateVisibility(visible)
    end,
})

ui.sections["Crosshair"]:Slider({
    Name = "Crosshair Size",
    Flag = "CrosshairSizeSlider",
    Default = 0.6,
    Minimum = 0.05,
    Maximum = 5,
    Decimals = 0.01,
    Callback = function(size)
        updateSize(size)
    end,
})

ui.sections["Crosshair"]:Colorpicker({
    Name = "Crosshair Color",
    Flag = "Crosshair_Color",
    Default = Color3.fromRGB(255, 255, 255),
    Callback = function(color)
        updateColor(color)
    end,
})

ui.sections["Crosshair"]:Toggle({
    Name = "Toggle Spinning",
    Flag = "Crosshair_Spinning_Toggle",
    Default = false,
    Callback = function(state)
        toggleSpinning(state)
    end,
})

ui.sections["Crosshair"]:Slider({
    Name = "Spin Speed",
    Flag = "Crosshair_SpinSpeed",
    Default = 5,
    Minimum = 1,
    Maximum = 20,
    Decimals = 0.1,
    Ending = "s",
    Callback = function(speed)
        updateSpinSpeed(speed)
    end,
})

createCrosshair(settings.crosshairType)

game:GetService("RunService").Heartbeat:Connect(updateCrosshairRotation)
    --
    do -- misc
    ui.window:Seperator({ Name = "Misc Settings" })
    ui.tabs["Misc"] = ui.window:Page({
            Name = "Misc",
            Icon = "http://www.roblox.com/asset/?id=6023426921",
        })
    do
            local gun_mods = gun_mods or {
    old_recoil1 = nil,
    old_recoil2 = nil,
    old_spread = {},
    old_firerates = {},
    old_auto = {},
    old_bullet_speeds = {},
    old_slowdowns = {},
    old_equip_anims = {},
    old_equip_anims = {},
    old_vm_mults = {},
    old_gravity = {}
}
            ui.sections["rage_weaponmods"] =
            ui.tabs["Misc"]:Section({ Name = "Weapon Mods", Side = "Right", Size = 250 })
            ui.sections["rage_weaponmods"]:Toggle({
                Name = "Force Auto",
                Flag = "Force Auto",
                Default = false,
                Callback = function(state)
                    if state then
                        for _, tbl in getgc(true) do
                            if type(tbl) ~= "table" then
                                continue
                            end

                            local weapon_auto = rawget(tbl, "Auto")
                            if weapon_auto ~= nil then
                                if not gun_mods.old_auto[tbl] then
                                    gun_mods.old_auto[tbl] = weapon_auto
                                end

                                tbl.Auto = true
                            end
                        end
                    else
                        for tbl, v in gun_mods.old_auto do
                            tbl["Auto"] = v
                        end
                    end
                end,
            })
             ui.sections["rage_weaponmods"]:Toggle({
                Name = "No Recoil",
                Flag = "No Recoil",
                Callback = function(state)
                    for _, tbl in getgc(true) do
                        if type(tbl) ~= "table" then
                            continue
                        end

                        local recoil_start = rawget(tbl, "RecoilStart")
                        local recoil_finish = rawget(tbl, "RecoilFinish")

                        if
                        recoil_start
                                and recoil_finish
                                and type(recoil_start) == "function"
                                and type(recoil_finish) == "function"
                        then
                            if state then
                                gun_mods.old_recoil1 = gun_mods.old_recoil1 or recoil_start
                                gun_mods.old_recoil2 = gun_mods.old_recoil2 or recoil_finish

                                rawset(tbl, "RecoilStart", function()
                                    return 0, 0
                                end)
                                rawset(tbl, "RecoilFinish", function()
                                    return 0, 0
                                end)
                            else
                                if gun_mods.old_recoil1 then
                                    rawset(tbl, "RecoilStart", gun_mods.old_recoil1)
                                end
                                if gun_mods.old_recoil2 then
                                    rawset(tbl, "RecoilFinish", gun_mods.old_recoil2)
                                end

                                gun_mods.old_recoil1 = nil
                                gun_mods.old_recoil2 = nil
                            end
                        end
                    end
                end,
            })
            --

            --
            ui.sections["rage_weaponmods"]:Toggle({
                Name = "No Spread",
                Flag = "No Spread",
                Callback = function(state)
                    for _, tbl in getgc(true) do
                        if type(tbl) ~= "table" then
                            continue
                        end

                        local spread_table = rawget(tbl, "Spread")
                        if spread_table and typeof(spread_table) == "table" then
                            for _, spread_stuff in spread_table do
                                for i, v in spread_stuff do
                                    if type(v) == "number" then
                                        gun_mods.old_spread[spread_stuff] = gun_mods.old_spread[spread_stuff] or {}
                                        gun_mods.old_spread[spread_stuff][i] = gun_mods.old_spread[spread_stuff][i] or v
                                        spread_stuff[i] = state and 0 or gun_mods.old_spread[spread_stuff][i]
                                    end
                                end
                            end
                        end
                    end
                end,
            })
            ui.sections["rage_weaponmods"]:Toggle({
                Name = "No Sway",
                Flag = "No Sway",
                Callback = function(state)
                    if state then
                        for _, tbl in getgc(true) do
                            if type(tbl) ~= "table" then
                                continue
                            end

                            local vm_movement_mults = rawget(tbl, "VMMovementMults")
                            if vm_movement_mults then
                                for i, v in vm_movement_mults do
                                    if not gun_mods.old_vm_mults[vm_movement_mults] then
                                        gun_mods.old_vm_mults[vm_movement_mults] = {}
                                    end

                                    if not gun_mods.old_vm_mults[vm_movement_mults][i] then
                                        gun_mods.old_vm_mults[vm_movement_mults][i] = v
                                    end

                                    vm_movement_mults[i] = 0
                                end
                            end
                        end
                    else
                        for tbl, values in gun_mods.old_vm_mults do
                            for i, v in values do
                                tbl[i] = v
                            end
                        end
                    end
                end,
            })
            --
            local fast_fire_callback = function()
                local value = Library.Flags["Enable RPM"]
                for _, tbl in getgc(true) do
                    if type(tbl) ~= "table" then
                        continue
                    end

                    local current_rpm = rawget(tbl, "RPM")
                    if current_rpm then
                        if not gun_mods.old_firerates[tbl] then
                            gun_mods.old_firerates[tbl] = current_rpm
                        end

                        if value then
                            tbl.RPM = gun_mods.old_firerates[tbl] * Library.Flags["RPM Boost"]
                        else
                            tbl.RPM = gun_mods.old_firerates[tbl]
                        end
                    end
                end
            end
            ui.sections["rage_weaponmods"]:Toggle({
                Name = "RPM",
                Flag = "Enable RPM",
                Default = false,
                Callback = fast_fire_callback,
            })
            ui.sections["rage_weaponmods"]:Slider({
                Name = "RPM Boost",
                Flag = "RPM Boost",
                Minimum = 1,
                Maximum = 3,
                Default = 0,
                Decimals = 0.1,
                Ending = "%",
                Callback = fast_fire_callback,
            })
            --ui.sections['rage_weaponmods']:Toggle({Name = 'Always Headshot', Flag = 'Always Headshot'})
            ui.sections["rage_weaponmods"]:Toggle({
                Name = "Instant Hit",
                Flag = "Instant Hit",
                Callback = function(state)
                    if state then
                        for _, tbl in getgc(true) do
                            if type(tbl) ~= "table" then
                                continue
                            end

                            local bullet_table = rawget(tbl, "Bullet")
                            if bullet_table and type(bullet_table) == "table" then
                                if not gun_mods.old_bullet_speeds[bullet_table] then
                                    gun_mods.old_bullet_speeds[bullet_table] = bullet_table.Speed
                                end
                                bullet_table.Speed = 10000
                            end
                        end
                    else
                        for tbl, speed_value in gun_mods.old_bullet_speeds do
                            tbl["Speed"] = speed_value
                        end
                    end
                end,
            })
            --ui.sections['rage_weaponmods']:Toggle({Name = 'Instant Aim', Flag = 'Instant Aim'})
            --ui.sections['rage_weaponmods']:Toggle({Name = 'Instant Equip', Flag = 'Instant Equip'})
        end
        end
        do
            local bases_folder = {
                workspace.Bases.Loners:FindFirstChild("External Wooden Gate") or nil,
                workspace.Bases.Loners:FindFirstChild("External Stone Gate") or nil,
            }

            for _, v in pairs(workspace.Bases:GetChildren()) do
                if v:IsA("Folder") and v.Name == "Base" then
                    for _, sub in ipairs({"Doorway", "Floor", "Triangle Floor", "Foundation", "Triangle Foundation", "Wall", "Wall Frame", "Window"}) do
                        local sub_folder = v:FindFirstChild(sub)
                        if sub_folder then
                            table.insert(bases_folder, sub_folder)
                        end
                    end
                end
            end

            getgenv().xray_toggle = false
            getgenv().xray_enabled = false
            getgenv().xray_opacity = 0.5

            local function upd_xray(state)
                if not getgenv().xray_toggle then
                    return
                end
                for _, v in pairs(bases_folder) do
                    for _, obj in ipairs(v:GetDescendants()) do
                        if obj:IsA("MeshPart") then
                            obj.Transparency = state and getgenv().xray_opacity or 0
                        end
                    end
                end
            end
             ui.sections["xray"] = ui.tabs["Misc"]:Section({ Name = "Xray", Side = "Left", Size = 133 })
            ui.sections["xray"]:Toggle({
                Name = "Enable",
                Flag = "Enable Xray",
                Callback = function(state)
                    getgenv().xray_toggle = state
                end,
            })
            ui.sections["xray"]:Keybind({
                Flag = "Xray Keybind",
                Name = "Xray Keybind",
                Default = Enum.KeyCode.T,
                Mode = "Toggle",
                Callback = function()
                    if getgenv().xray_toggle then
                        getgenv().xray_enabled = not getgenv().xray_enabled
                        upd_xray(getgenv().xray_enabled)
                    end
                end,
            })
            ui.sections["xray"]:Slider({
                Name = "Opacity",
                Flag = "Xray Opacity",
                Default = 50,
                Minimum = 0,
                Maximum = 100,
                Decimals = 1,
                Ending = "%",
                Callback = function(state)
                    getgenv().xray_opacity = state / 100
                    if getgenv().xray_toggle and getgenv().xray_enabled then
                        upd_xray(true)
                    end
                end,
            })
        end

        do
                ui.sections["traps"] = ui.tabs["Misc"]:Section({ Name = "Misc Fun", Side = "Left", Size = 360 })
                ui.sections["traps"]:Toggle({ Name = "Bullet Tracers", Flag = "Bullet Tracers" })
                ui.sections["traps"]:Toggle({ Name = "Hitmarkers", Flag = "Hitmarkers" })
                ui.sections["traps"]:Toggle({ Name = "Hitsounds", Flag = "Hitsounds" })
                ui.sections["traps"]:List({ Name = "Sound type", Flag = "Hitsound Type", Options = sounds })
                ui.sections["traps"]:Toggle({
    Name = "No Bullet Drop",
    Flag = "no bullet drop",
    Default = false,
    Callback = function(value)
        if value then
            for _, tbl in getgc(true) do
                if type(tbl) ~= "table" then
                    continue
                end
                local bullet_table = rawget(tbl, "Bullet")
                if bullet_table and type(bullet_table) == "table" then
                    if not old_tuahs[bullet_table] then
                        old_tuahs[bullet_table] = bullet_table.Gravity
                    end
                    bullet_table.Gravity = 0
                end
            end
        else
            for tbl, v in old_tuahs do
                tbl["Gravity"] = v
            end
        end
    end,
})
               ui.sections["traps"]:Toggle({
    Name = "Always Perfect Farm",
    Flag = "always perfect farm",
    Default = false,
})
                ui.sections["traps"]:Toggle({
    Name = "Instant Loot",
    Flag = "instant loot",
    Default = false,
})

                ui.sections["traps"]:Toggle({ Name = "Silent Walk", Flag = "Silent Walk", Default = false })
                --
                ui.sections["MISCESP"] = ui.tabs["Misc"]:Section({ Name = "Misc ESP", Side = "Right", Size = 200 })
                ui.sections["MISCESP"]:Toggle({ Name = "Soldier ESP", Flag = "soldier_Name", Default = false })
                ui.sections["MISCESP"]:Toggle({ Name = "Show Health", Flag = "soldier_Health", Default = false })
                ui.sections["MISCESP"]:Toggle({ Name = "Show Distance", Flag = "soldier_Distance", Default = false })
                ui.sections["MISCESP"]:Colorpicker({ Name = "Soldier ESP Color", Flag = "soldier_Color", Default = Color3.new(255, 255, 255) }) 
                ui.sections["MISCESP"]:Toggle({ Name = "Boss ESP", Flag = "boss_Name", Default = false })
                ui.sections["MISCESP"]:Colorpicker({ Name = "Boss ESP Color", Flag = "boss_Color", Default = Color3.new(255, 255, 255) })
                
                local function esp_for_character(character, nameFlag, healthFlag, distanceFlag, colorFlag)
                    local player = game:GetService("Players").LocalPlayer
                    local runService = game:GetService("RunService")
                    local camera = workspace.CurrentCamera
                    local hrp = character:FindFirstChild("HumanoidRootPart")
                    local hum = character:FindFirstChildOfClass("Humanoid")
                
                    if hrp and hum then
                        local text = Drawing.new("Text")
                        text.Color = Flags[colorFlag]
                        text.Size = 14
                        text.Center = true
                        text.Outline = true
                        text.Visible = true
                
                        local conn
                        conn = runService.RenderStepped:Connect(function()
                            if not character:IsDescendantOf(workspace) or hum.Health <= 0 then
                                text:Remove()
                                conn:Disconnect()
                                return
                            end
                
                            if (character.Name:lower() == "soldier" and not Flags["soldier_Name"]) or ((character.Name == "Bruno" or character.Name == "Boris") and not Flags["boss_Name"]) then
                                text.Visible = false
                            else
                                local head_pos = hrp.Position + Vector3.new(0, 3, 0)
                                local text_screen, on_screen = camera:WorldToViewportPoint(head_pos)
                
                                local displayText = ""
                                if Flags[nameFlag] then displayText = displayText .. character.Name end
                                if healthFlag and Flags[healthFlag] then
                                    if character.Name:lower() == "soldier" then
                                        displayText = string.format("Soldier - %d HP", math.floor(hum.Health))
                                    else
                                        displayText = string.format("%s - %d HP", character.Name, math.floor(hum.Health))
                                    end
                                end
                                if distanceFlag and Flags[distanceFlag] then
                                    local distance = math.floor((hrp.Position - camera.CFrame.Position).Magnitude)
                                    displayText = displayText .. string.format(" - %d s", distance)
                                end
                
                                text.Position = Vector2.new(text_screen.X, text_screen.Y)
                                text.Text = displayText
                                text.Visible = on_screen
                                text.Color = Flags[colorFlag]
                            end
                        end)
                    end
                end
                
                local function initialize_esp()
                    local function on_character_added(character)
                        if character:IsA("Model") and (character.Name:lower() == "soldier" or character.Name == "Bruno" or character.Name == "Boris") then
                            if character.Name:lower() == "soldier" then
                                esp_for_character(character, "soldier_Name", "soldier_Health", "soldier_Distance", "soldier_Color")
                            elseif character.Name == "Bruno" or character.Name == "Boris" then
                                esp_for_character(character, "boss_Name", "soldier_Health", "soldier_Distance", "boss_Color")
                            end
                        end
                    end
                    
                    for _, obj in ipairs(workspace:GetDescendants()) do
                        on_character_added(obj)
                    end
                
                    workspace.DescendantAdded:Connect(on_character_added)
                end
                
                initialize_esp()
                
                do -- silent walk
                    local old_toggle_footstep = sound_table.ToggleFootstep;
                    local old_play_sound = sound_table.PlaySound;
                    sound_table.ToggleFootstep = function(_, character, sound, playback_speed)
                        if Flags["Silent Walk"] and sound then
                            return sound_table:StopSound(sound);
                        end;
                        return old_toggle_footstep(_, character, sound, playback_speed);
                    end;
                    sound_table.PlaySound = function(_, sound, is_duplicate, playback_speed)
                        if sound and sound.Name == "WalkWater" and Flags["Silent Walk"] then
                            return sound_table:StopSound(sound);
                        end;
                        return old_play_sound(_, sound, is_duplicate, playback_speed);
                    end;
                end;


                local activeConnections = {}
                local activeNotifications = 0

                function mod_check(player)
                    if not player:IsDescendantOf(game) then
                        return false
                    end

                    local role = player:GetRoleInGroup(1154360)
                    if table.find({ "Game Moderator", "Developer", "Lead Developer", "Co-Founder", "Founder" }, role) then
                        return true, role
                    end

                    return false
                end

                function notify(messages)
                    activeNotifications += 1

                    local g = Instance.new("ScreenGui")
                    g.Parent = gethui()

                    local frame = Instance.new("Frame")
                    frame.Size = UDim2.new(0, 270, 0, #messages > 1 and (50 + (#messages - 1) * 20) or 50)
                    frame.Position = UDim2.new(-0.3, 0, 0, 10 + (activeNotifications - 1) * 60)
                    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
                    frame.BackgroundTransparency = 0.2
                    frame.BorderSizePixel = 0
                    frame.Parent = g

                    local n = Instance.new("TextLabel")
                    n.Size = UDim2.new(1, -20, 1, -20)
                    n.Position = UDim2.new(0, 10, 0, 10)
                    n.BackgroundTransparency = 1
                    n.TextColor3 = Color3.fromRGB(128, 0, 128)
                    n.Font = Enum.Font.GothamBold
                    n.TextWrapped = true
                    n.TextScaled = true
                    n.Text = table.concat(messages, "\n")
                    n.TextXAlignment = Enum.TextXAlignment.Left
                    n.TextStrokeTransparency = 0.8
                    n.TextStrokeColor3 = Color3.fromRGB(255, 255, 255)
                    n.Parent = frame

                    local t = game:GetService("TweenService")
                    local slideIn = t:Create(frame, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), { Position = UDim2.new(0, 10, 0, 10 + (activeNotifications - 1) * 60) })
                    slideIn:Play()
                    task.delay(3, function()
                    local slideOut = t:Create(frame, TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.In), { Position = UDim2.new(-0.3, 0, 0, 10 + (activeNotifications - 1) * 60) })
                    slideOut:Play()
                    slideOut.Completed:Connect(function()
                    activeNotifications -= 1
                    g:Destroy()
                    end)
                    end)
                end


                function mod_check(player)
                    if not player:IsDescendantOf(game) then
                        return false
                    end

                    local role = player:GetRoleInGroup(1154360)
                    if table.find({ "Game Moderator", "Developer", "Lead Developer", "Co-Founder", "Founder" }, role) then
                        return true, role
                    end

                    return false
                end

                ui.sections = ui.sections or {}
                ui.sections["traps"] = ui.sections["traps"] or {}

                ui.sections["traps"]:Toggle({
                    Name = "Mod check",
                    Flag = "Mod Check",
                    Default = false,
                    Callback = function(enabled)
                        if enabled then
                            local messages = {}
                            for _, player in ipairs(game:GetService("Players"):GetPlayers()) do
                                local isMod, rank = mod_check(player)
                                if isMod then
                                    table.insert(messages, rank .. " already in game! - " .. player.Name)
                                end
                            end

                            if #messages > 0 then
                                notify(messages)
                            end

                            table.insert(activeConnections, game:GetService("Players").PlayerAdded:Connect(function(player)
                                local isMod, rank = mod_check(player)
                                if isMod and Flags["Mod Check"] then
                                    notify({ rank .. " joined - " .. player.Name })
                                end
                            end))

                            table.insert(activeConnections, game:GetService("Players").PlayerRemoving:Connect(function(player)
                                local isMod, rank = mod_check(player)
                                if isMod and Flags["Mod Check"] then
                                    notify({ rank .. " left - " .. player.Name })
                                end
                            end))
                        else
                            -- Disconnect all previous connections
                            for _, connection in ipairs(activeConnections) do
                                connection:Disconnect()
                            end
                            activeConnections = {}
                        end
                    end
                })

                local stored_parts = { DamagePart = {}, TouchCollision = {}, Collision = {} }

                local function new_toggle(name, partName)
                    ui.sections["traps"]:Toggle({
                        Name = name,
                        Flag = name,
                        Default = false,
                        Callback = function(state)
                            if state then
                                for _, v in workspace:GetDescendants() do
                                    if v:IsA("BasePart") and v.Name == partName then
                                        stored_parts[partName][v] = v.Parent
                                        v.Parent = cloneref(game:GetService("CoreGui"))
                                        v.Destroying:Connect(function()
                                            stored_parts[partName][v] = nil
                                        end)
                                    end
                                end
                            else
                                for part, old_parent in pairs(stored_parts[partName]) do
                                    part.Parent = old_parent
                                end
                                table.clear(stored_parts[partName])
                            end
                        end,
                    })
                end

                new_toggle("No Spike Damage", "DamagePart")
                new_toggle("No Trap Damage", "TouchCollision")

                workspace.DescendantAdded:Connect(function(descendant)
                    if descendant:IsA("BasePart") then
                        for partName, flag in pairs({
                            DamagePart = Flags["No Spike Damage"],
                            TouchCollision = Flags["No Trap Damage"],
                        }) do
                            if descendant.Name == partName and flag then
                                stored_parts[partName][descendant] = descendant.Parent
                                descendant.Parent = cloneref(game:GetService("CoreGui"))
                                descendant.Destroying:Connect(function()
                                    stored_parts[partName][descendant] = nil
                                end)
                            end
                        end
                    end
                end)
            end
            

    ui.tabs["rage"] = ui.window:Page({
            Name = "Rage",
            "http://www.roblox.com/asset/?id=6023426921",
        })

        do
            ui.sections["rage_main"] = ui.tabs["rage"]:Section({ Name = "Player", Side = "Left", Size = 275 })
            ui.sections["rage_main"]:Toggle({ Name = "Always Grounded", Flag = "Always Grounded" })
ui.sections["rage_main"]:Toggle({
    Name = "Sprint While Attacking",
    Flag = "sprint while attacking",
    Default = false,
})
ui.sections["rage_main"]:Toggle({
    Name = "Shoot While Jumping",
    Flag = "Shoot While Jumping",
    Default = false,
    Callback = function(state)
        -- Здесь можно добавить дополнительную логику, если нужно
        getgenv().shoot_while_jumping = state
    end,
})
ui.sections["rage_main"]:Toggle({
    Name = "No Gun Slowdowns",
    Flag = "no gun slowdowns",
    Default = false,
    Callback = function(value)
        -- Проверка инициализации
        if not gun_mods then
            gun_mods = {
                old_recoil1 = nil,
                old_recoil2 = nil,
                old_spread = {},
                old_firerates = {},
                old_auto = {},
                old_bullet_speeds = {},
                old_slowdowns = {},
                old_equip_anims = {},
                old_vm_mults = {},
                old_gravity = {}
            }
        end
        if not gun_mods.old_slowdowns then
            gun_mods.old_slowdowns = {}
        end

        if value then
            for _, tbl in getgc(true) do
                if type(tbl) ~= "table" then
                    continue
                end
                local weapon_table = rawget(tbl, "Weapon")
                if weapon_table and type(weapon_table) == "table" and weapon_table.SlowdownFactor ~= nil then
                    if not gun_mods.old_slowdowns[weapon_table] then
                        gun_mods.old_slowdowns[weapon_table] = weapon_table.SlowdownFactor
                    end
                    weapon_table.SlowdownFactor = 0
                end
            end
        else
            for weapon_table, slowdown in pairs(gun_mods.old_slowdowns) do
                if weapon_table and type(weapon_table) == "table" then
                    weapon_table.SlowdownFactor = slowdown
                end
            end
            gun_mods.old_slowdowns = {}
        end
    end,
})

ui.sections["rage_main"]:Toggle({
    Name = "Speed Enabled",
    Flag = "speed enabled",
    Default = false,
})
ui.sections["rage_main"]:Slider({
    Name = "Speed",
    Decimals = 1,
    Sub = "x",
    Flag = "speed",
    Min = 1,
    Max = 50,
    Default = 16,
    Increment = 1,
})
ui.sections["rage_main"]:Keybind({
    Name = "Speed Key Bind",
    Flag = "speed key bind",
    Default = Enum.KeyCode.LeftShift,
})

 ui.sections["rage_main"]:Toggle({ Name = "Spiderclimb", Flag = "Spiderclimb" })

            ui.sections["rage_main"]:Keybind({
                Flag = "Spiderclimb Keybind",
                Name = "Spiderclimb Keybind",
                Default = Enum.KeyCode.Space,
                Mode = "Hold",
            })
            ui.sections["rage_main"]:Slider({
                Name = "Spiderclimb Speed",
                Flag = "Spiderclimb Speed",
                Default = 0.75,
                Minimum = 0.5,
                Maximum = 2.5,
                Decimals = 0.01,
                Ending = "",
            })
        
        end

        local uis        = game:GetService("UserInputService")
local players    = game:GetService("Players")
local runService = game:GetService("RunService")
local local_player = players.LocalPlayer

-- Configuration
local WALK_SPEED         = 20
local FORCEFIELD_COLOR   = BrickColor.new("Bright violet")
local SHOW_DISTANCE_TEXT = true
local MAX_DISTANCE       = 8

local remake = {
    enabled        = false,
    startReplica   = nil,
    startPosition  = nil,
    remakePosition = nil,
    distanceText   = Drawing.new("Text"),
    character      = nil,
    diedConnection = nil,
}

remake.distanceText.Size     = 18
remake.distanceText.Center   = true
remake.distanceText.Outline  = true
remake.distanceText.Visible  = false
remake.distanceText.Position = Vector2.new(
    workspace.CurrentCamera.ViewportSize.X/2,
    workspace.CurrentCamera.ViewportSize.Y/2 + 50
)

local function createClone(character, color)
    local replica = Instance.new("Model")
    replica.Name = "ForcefieldClone"
    for _, part in ipairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            local clone = Instance.new("Part")
            clone.Size       = part.Size
            clone.CFrame     = part.CFrame
            clone.Anchored   = true
            clone.CanCollide = false
            clone.Material   = Enum.Material.ForceField
            clone.BrickColor = color

            local mesh = part:FindFirstChildWhichIsA("SpecialMesh")
            if mesh then
                local meshClone = Instance.new("SpecialMesh")
                pcall(function()
                    meshClone.MeshType  = mesh.MeshType
                    meshClone.MeshId    = mesh.MeshId
                    meshClone.TextureId = mesh.TextureId
                    meshClone.Scale     = mesh.Scale
                end)
                meshClone.Parent = clone
            end

            clone.Parent = replica
        end
    end
    replica.Parent = workspace
    return replica
end

local function ManipulationReset()
    remake.enabled = false
    if remake.startReplica then
        remake.startReplica:Destroy()
        remake.startReplica = nil
    end
    remake.distanceText.Visible = false
    if remake.character and remake.character:FindFirstChild("HumanoidRootPart") then
        remake.character.HumanoidRootPart.Anchored = false
    end
end

local function updateCharacterReferences()
    remake.character = local_player.Character or local_player.CharacterAdded:Wait()
    remake.character:WaitForChild("HumanoidRootPart")
    remake.character:WaitForChild("Humanoid")

    if remake.diedConnection then
        remake.diedConnection:Disconnect()
    end
    remake.diedConnection = remake.character.Humanoid.Died:Connect(ManipulationReset)
end

updateCharacterReferences()
local_player.CharacterAdded:Connect(function()
    ManipulationReset()
    updateCharacterReferences()
end)

if ui and ui.sections and ui.sections["rage_main"] then
    ui.sections["rage_main"]:Toggle({
        Name     = "Manipulation",
        Flag     = "zins_Enabled",
        Callback = function(val)
            Flags["zins_Enabled"] = val
            if not val then
                ManipulationReset()
            end
        end,
    })

ui.sections["rage_main"]:Keybind({
    Flag = "zins_keybind",
    Name = "Keybind",
    Default = Enum.KeyCode.F,
    Mode = "Toggle",
    })
end
runService.RenderStepped:Connect(function(deltaTime)

    if Flags["zins_Enabled"] and Flags["zins_keybind"] then
        if not remake.enabled then
            remake.enabled        = true
            local root            = remake.character:FindFirstChild("HumanoidRootPart")
            if root then
                root.Anchored        = true
                remake.startPosition = root.Position
                remake.remakePosition= root.Position
                remake.startReplica  = createClone(remake.character, FORCEFIELD_COLOR)
                if SHOW_DISTANCE_TEXT then
                    remake.distanceText.Visible = true
                end
            end
        end
    else
        if remake.enabled then
            ManipulationReset()
        end
    end

    if remake.enabled then
        local root = remake.character and remake.character:FindFirstChild("HumanoidRootPart")
        if root then
            local dir = Vector3.new()
            if uis:IsKeyDown(Enum.KeyCode.W) then dir += Vector3.new(0,0,-1) end
            if uis:IsKeyDown(Enum.KeyCode.A) then dir += Vector3.new(-1,0,0) end
            if uis:IsKeyDown(Enum.KeyCode.S) then dir += Vector3.new(0,0,1) end
            if uis:IsKeyDown(Enum.KeyCode.D) then dir += Vector3.new(1,0,0) end

            if dir.Magnitude > 0 then
                dir = dir.Unit * WALK_SPEED * deltaTime
            end

            local newPos = root.Position + root.CFrame:VectorToWorldSpace(dir)
            local dist   = (newPos - remake.startPosition).Magnitude
            if dist > MAX_DISTANCE then
                newPos = remake.startPosition + (newPos - remake.startPosition).Unit * MAX_DISTANCE
            end

            root.CFrame = CFrame.new(newPos)
            remake.remakePosition = newPos

            if SHOW_DISTANCE_TEXT then
                local d = (remake.remakePosition - remake.startPosition).Magnitude
                remake.distanceText.Text  = string.format("Manipulating: %.2f", d)
                remake.distanceText.Color = Color3.new(1,1,1)
            end
        end
    end
end)

        
         do
            ui.sections["camera"] = ui.tabs["rage"]:Section({ Name = "Camera", Side = "Right", Size = 187 })
            ui.sections["camera"]:Toggle({ Name = "FOV", Flag = "FOV" })
            ui.sections["camera"]:Slider({
                Name = "FOV Ammount",
                Flag = "FOV Ammount",
                Default = 70,
                Minimum = 30,
                Maximum = 120,
                Decimals = 1,
                Ending = "x",
            })
            ui.sections["camera"]:Toggle({ Name = "Zoom", Flag = "Zoom" })
            ui.sections["camera"]:Keybind({ Flag = "Zoom Keybind", Name = "Zoom Keybind", Mode = "Hold" })
            ui.sections["camera"]:Slider({
                Name = "Zoom Ammount",
                Flag = "Zoom Ammount",
                Default = 30,
                Minimum = 5,
                Maximum = 70,
                Decimals = 1,
                Ending = "x",
            })
        end
        end

    do -- settings
        ui.window:Seperator({ Name = "Settings" })
        ui.tabs["settings"] = ui.window:Page({
            Name = "Settings",
            Icon = "http://www.roblox.com/asset/?id=6031280882",
        })
        Library:Configs(ui.tabs["settings"])
    end
    end
--
do -- fov circle
    local FieldOfView =
    player_esp.functions.create_drawing("ScreenGui", { Parent = cloneref(gethui()), IgnoreGuiInset = true })
    local Frame = player_esp.functions.create_drawing("Frame", {
        Visible = false,
        BackgroundTransparency = 1,
        Size = UDim2.new(0, Flags["FOV Radius"] * 2, 0, Flags["FOV Radius"] * 2),
        Position = UDim2.new(0, 0, 0, 0),
        AnchorPoint = Vector2.new(0.5, 0.5),
        Parent = FieldOfView,
        ZIndex = 2,
    })
    local UICorner = player_esp.functions.create_drawing("UICorner", { CornerRadius = UDim.new(1, 0), Parent = Frame })
    local Stroke = player_esp.functions.create_drawing(
            "UIStroke",
            { Color = Color3.new(1, 1, 1), Thickness = 2, Transparency = 0, Parent = Frame }
    )
    -- returns
    fov_circle.FieldOfView = FieldOfView
    fov_circle.Frame = Frame
    fov_circle.Stroke = Stroke
end
--
do -- snaplines
    local snaplines =
    player_esp.functions.create_drawing("ScreenGui", { Parent = cloneref(gethui()), IgnoreGuiInset = true })
    local line = player_esp.functions.create_drawing("Frame", {
        BackgroundTransparency = 0,
        AnchorPoint = Vector2.new(0.5, 0.5),
        BorderSizePixel = 0,
        ZIndex = 2,
        Parent = snaplines,
    })
    -- returns
    snap_lines.snaplines = snaplines
    snap_lines.line = line
end
do -- inventory viewer
    local inventory_view = player_esp.functions.create_drawing("ScreenGui", { Parent = cloneref(gethui()), IgnoreGuiInset = true })
    local background = player_esp.functions.create_drawing("Frame", { BackgroundTransparency = 0, Size = UDim2.new(0, 400, 0, 15), BackgroundColor3 = Color3.fromRGB(9, 9, 9), AnchorPoint = Vector2.new(0.5, 0.5), BorderSizePixel = 0, ZIndex = 99, Parent = inventory_view })
    local icon_holder = player_esp.functions.create_drawing("Frame", { BackgroundTransparency = 1, Size = UDim2.new(0, 900, 0, 75), BackgroundColor3 = Color3.fromRGB(9, 9, 9), AnchorPoint = Vector2.new(0.5, 0.5), BorderSizePixel = 0, ZIndex = 99, Parent = inventory_view })
    local name = player_esp.functions.create_drawing("TextLabel", { BackgroundTransparency = 0, AnchorPoint = Vector2.new(0.5, 0.5), BorderSizePixel = 0, ZIndex = 999, TextColor3 = Color3.fromRGB(255, 255, 255), TextStrokeTransparency = 0, TextStrokeColor3 = Color3.fromRGB(0, 0, 0), TextSize = 11, FontFace = Font.new([[rbxassetid://12187365977]], Enum.FontWeight.SemiBold, Enum.FontStyle.Normal), Parent = background, Position = UDim2.new(0.5, 0, 0.5, 0), TextXAlignment = Enum.TextXAlignment.Center, TextYAlignment = Enum.TextYAlignment.Center })

    player_esp.functions.create_drawing("UIListLayout", {
        Parent = icon_holder,
        SortOrder = Enum.SortOrder.LayoutOrder,
        FillDirection = Enum.FillDirection.Horizontal,
        HorizontalAlignment = Enum.HorizontalAlignment.Center,
    });

    -- returns
    info_viewer.inventory_view = inventory_view
    info_viewer.background = background
    info_viewer.icon_holder = icon_holder
    info_viewer.name = name
end;
--
do -- other functions
    local old_setting = settings_table.GetSetting
    do -- funcs
        settings_table.GetSetting = function(cat, set, ...)
            if set == "Field Of View" then
                if Flags["FOV"] and not (Flags["Zoom"] and Flags["Zoom Keybind"]) then
                    return Flags["FOV Ammount"]
                elseif Flags["Zoom"] and Flags["Zoom Keybind"] then
                    return Flags["Zoom Ammount"]
                end
            end
            return old_setting(cat, set, ...)
        end
    end
end

local Desktop = {}

function Desktop:Instance(ClassName, Properties)
    local Instance = Instance.new(ClassName)

    for Index, Value in next, Properties do
        Instance[Index] = Value
    end

    return Instance
end

function Desktop:CreateBeam(Origin, Direction)
    local Tracer = Desktop:Instance("Model", { Parent = workspace })
    local Beam = Desktop:Instance("Beam", {
        FaceCamera = true,
        Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0, Color3.new(1, 1, 1)),
            ColorSequenceKeypoint.new(1, Color3.new(1, 1, 1)),
        }),
        LightEmission = 0,
        LightInfluence = 0,
        Width0 = 0.75,
        Width1 = 0.75,
        Texture = "rbxassetid://446111271",
        TextureLength = 30,
        TextureMode = Enum.TextureMode.Wrap,
        TextureSpeed = 1,
        Parent = Tracer,
        Brightness = 150,
        Attachment0 = Desktop:Instance("Attachment", {
            Parent = Desktop:Instance("Part", {
                Position = Origin,
                Anchored = true,
                Transparency = 1,
                CanCollide = false,
                Parent = Tracer,
                Size = Vector3.new(0.001, 0.001, 0.001),
            }),
        }),
        Attachment1 = Desktop:Instance("Attachment", {
            Parent = Desktop:Instance("Part", {
                Position = Direction,
                Anchored = true,
                Transparency = 1,
                CanCollide = false,
                Parent = Tracer,
                Size = Vector3.new(0.001, 0.001, 0.001),
            }),
        }),
    })

    task.delay(5, function()
        Tracer:Destroy()
    end)

    return Tracer
end

function Desktop:CreateHitmarker(Direction)
    local ProtectedModel = Desktop:Instance("Model", { Parent = workspace })

    local HitmarkerPart = Desktop:Instance("Part", {
        Position = Direction,
        Size = Vector3.new(0.1, 0.1, 0.1),
        Transparency = 1,
        CanCollide = false,
        Anchored = true,
        Parent = ProtectedModel,
    })

    local HitmarkerGui = Desktop:Instance("BillboardGui", {
        Size = UDim2.new(0, 20, 0, 20),
        AlwaysOnTop = true,
        ZIndexBehavior = Enum.ZIndexBehavior.Sibling,
        Parent = HitmarkerPart,
    })

    local HitmarkerData = {
        { UDim2.new(0, 7, 0, 1), UDim2.new(0, 0, 0.35, -6), 45 },
        { UDim2.new(0, 7, 0, 1), UDim2.new(0, 0, 0.35, 4), -45 },
        { UDim2.new(0, 7, 0, 1), UDim2.new(1, -10, 0.35, -6), -45 },
        { UDim2.new(0, 7, 0, 1), UDim2.new(1, -10, 0.35, 4), 45 },
    }

    for _, Data in next, HitmarkerData do
        local Line = Desktop:Instance("Frame", {
            Size = Data[1],
            BackgroundColor3 = Color3.new(1, 1, 1),
            BackgroundTransparency = 0,
            BorderSizePixel = 1,
            BorderColor3 = Color3.fromRGB(0, 0, 0),
            Position = Data[2],
            Rotation = Data[3],
            Parent = HitmarkerGui,
        })
    end

    task.delay(5, function()
        for Index = 1, 100 do
            for _, Object in pairs(HitmarkerGui:GetChildren()) do
                if Object:IsA("Frame") then
                    Object.BackgroundTransparency = Index / 100
                end
            end

            task.wait()
        end

        HitmarkerGui:Destroy()
        HitmarkerPart:Destroy()
        ProtectedModel:Destroy()
    end)
end

function Desktop:CreateSound()
    local selected_sound = Flags["Hitsound Type"];
    local sound_id = hit_sounds[selected_sound];

    local Sound = Desktop:Instance("Sound", {
        Parent = workspace,
        SoundId = sound_id,
        Volume = 5,
        PlayOnRemove = true,
    })

    Sound:Destroy()
end
Desktop.Bodyparts = {
    "HumanoidRootPart",
    "LowerTorso",
    "UpperTorso",
    "LeftLeg",
    "RightLeg",
    "LeftArm",
    "RightArm",
    "Head",
    "Neck",
    "LeftHand",
    "RightHand",
    "LeftFoot",
    "RightFoot",
    "LeftLowerArm",
    "RightLowerArm",
    "LeftUpperArm",
    "RightUpperArm",
    "LeftLowerLeg",
    "RightLowerLeg",
    "LeftUpperLeg",
    "RightUpperLeg",
}
-- hooks
LPH_NO_VIRTUALIZE(function()
    local bullet_stuff, raycast_stuff, shoot_check_func
    do
        local RefreshGC = function()
            for i, v in pairs(getgc(true)) do
                if typeof(v) == "table" and rawget(v, "Raycast") then
                    raycast_stuff = v
                elseif typeof(v) == "table" and rawget(v, "CreateProjectile") then
                    bullet_stuff = v
                elseif typeof(v) == "function" and debug.getinfo(v).name == "CanShoot" then
                    shoot_check_func = v
                end
                --
                if typeof(v) == "function" then
                    local name = debug.getinfo(v).name

                    if name == "LocalHitFunction" then
                        local hitfunc; hitfunc = hookfunction(v, function(data, ...)
                            if data then
                                local hitobj = data[1]
                                local hitpos = data[2]

                                if hitobj and hitpos then
                                    if Flags["Bullet Tracers"] then
                                        local origin = workspace.CurrentCamera.CFrame.Position
                                        Desktop:CreateBeam(origin, hitpos)
                                    end

                                    if table.find(Desktop.Bodyparts, hitobj.Name) then
                                        if Flags["Hitmarkers"] then
                                            Desktop:CreateHitmarker(hitpos)
                                        end

                                        if Flags["Hitsounds"] then
                                            Desktop:CreateSound()
                                        end
                                    end
                                end
                            end

                            return hitfunc(data, ...)
                        end)
                    end
                end
            end
        end
        --
        do -- handling
            local_player.CharacterAdded:Connect(function(character)
                local_char = character
                --
                task.wait(1)
                RefreshGC()
            end)
        end
        --
        RefreshGC()

        -- Хук для CanShoot
        if shoot_check_func then
            local old_shoot_check = shoot_check_func
            hookfunction(shoot_check_func, function(...)
                if Flags["Shoot While Jumping"] then
                    return true -- Игнорируем проверки на прыжок
                end
                return old_shoot_check(...)
            end)
        end

        -- Хук для Raycast
        local success, result = pcall(function()
            local rayc = raycast_stuff.Raycast
            raycast_stuff.Raycast = function(...)
                local args = { ... }
                local traceback = debug.traceback()
                local is_viewmodel_controller = traceback:find("ViewmodelController")

                local output = {
                    rayc(unpack(args)),
                }

                -- Silent Aim
                if getgenv().silent and getgenv().silent_key and math.random(1, 100) <= getgenv().silent_hitchance and is_viewmodel_controller then
                    local entry, closest_part = nil, nil

                    local fetchSuccess, fetchError = pcall(function()
                        entry, closest_part = player_esp.functions.get_player()
                    end)

                    if fetchSuccess and closest_part and entry and closest_part and entry ~= local_player then
                        args[3] = (closest_part.Position - args[2]).Unit * 2500
                        output = { rayc(unpack(args)) }
                    end
                end

                -- Always Perfect Farm
                local hit_instance = output[1]
                local hit_pos = output[2]

                if not hit_instance or typeof(hit_instance) ~= "Instance" then
                    return unpack(output)
                end

                if not hit_pos or typeof(hit_pos) ~= "Vector3" then
                    return unpack(output)
                end

                local model = hit_instance.Parent

                if not model or not model:IsA("Model") then
                    return unpack(output)
                end

                local folder = model.Parent

                if
                    folder
                    and (folder.Name == "Trees" or folder.Name == "Nodes")
                    and folder:IsA("Folder")
                    and (Library.Flags["always perfect farm"] and Library.Flags["always perfect farm"]:Get() or Flags["always perfect farm"])
                then
                    local perfect = model:FindFirstChild("NodeSpark") or model:FindFirstChild("TreeX")
                    if perfect and typeof(perfect) == "Instance" and perfect:IsA("Model") and perfect.PrimaryPart then
                        output[1] = perfect.PrimaryPart
                    end
                end

                return unpack(output)
            end
        end)

        if not success then
            warn("Не удалось установить хук для raycast: " .. tostring(result))
        end
    end
end)()
--
do -- loops
    local freecam = {
        enabled = false,
        pitch = 0,
        yaw = 0,
        keys = {},
        old_type = Enum.CameraType.Custom,
        old_behavior = Enum.MouseBehavior.Default,
    }
    local third_person = {
        enabled = false,
        old_type = Enum.CameraType.Custom,
        old_behavior = Enum.MouseBehavior.Default,
        old_cframe = CFrame.new(),
    }
    --
    local platform = Instance.new("Part")
    platform.Size = Vector3.new(2, 0.1, 2)
    platform.Anchored = true
    platform.BrickColor = BrickColor.new("Black")
    platform.CanCollide = true
    platform.Name = "Platform"
    platform.Parent = Workspace
    platform.Transparency = 1
    --
    local shooting = false
    run_service.RenderStepped:Connect(LPH_NO_VIRTUALIZE(function()
        entry, closest_part = player_esp.functions.get_player()

        do -- update target
            if closest_part and (closest_part ~= target_Paradox.target.part) then
                target_Paradox.target.part = closest_part
                target_Paradox.target.entry = entry
                target_Paradox.target.distance = (closest_part.Position - camera.CFrame.Position).Magnitude
            end
        end

        highlight_player = target_Paradox.target.entry -- highlight

        do -- auto shoot
            if local_player.Character then
                if Flags["Enable Auto Shoot"] and target_Paradox and target_Paradox.target and target_Paradox.target.entry and target_Paradox.target.part and player_esp.functions.is_visible2(entry) then
                    mouse1press()
                    mouse1release()
                end
            end            
        end;
        --
        do -- shoot while jumping and always grounded
            local isAlwaysGrounded = Flags["Always Grounded"]
            local isShootWhileJumping = Flags["Shoot While Jumping"] or isAlwaysGrounded -- Shoot While Jumping активируется, если включен Always Grounded

            -- Логика Shoot While Jumping
            if isShootWhileJumping and local_player.Character then
                local humanoid = local_player.Character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    -- Игнорируем состояние прыжка
                    humanoid:SetStateEnabled(Enum.HumanoidStateType.Jumping, true)
                    humanoid:SetStateEnabled(Enum.HumanoidStateType.Freefall, true)
                    -- Устанавливаем состояние "на земле" для обхода проверок
                    if humanoid:GetState() == Enum.HumanoidStateType.Jumping or humanoid:GetState() == Enum.HumanoidStateType.Freefall then
                        humanoid:ChangeState(Enum.HumanoidStateType.Running)
                    end
                end
            end

            -- Логика Always Grounded (платформа)
            if isAlwaysGrounded then
                local plr_model = local_char
                if plr_model and plr_model:FindFirstChild("HumanoidRootPart") then
                    local root = plr_model:FindFirstChild("HumanoidRootPart")
                    if root then
                        local targetPosition = root.Position - Vector3.new(0, 3.2, 0)
                        platform.Position = targetPosition
                    else
                        platform.Position = Vector3.new(0, 0, 0)
                    end
                else
                    platform.Position = Vector3.new(0, 0, 0)
                end
            else
                platform.Position = Vector3.new(0, 0, 0)
            end
        end

        -- ... (остальной код, например, lighting, fov circle, snaplines и т.д.)
   
        --
        do -- aimbot
            if (target_Paradox and target_Paradox.target and target_Paradox.target.entry and target_Paradox.target.part and target_Paradox.target.entry ~= local_player) then
                if (Flags["Enable Aimbot"] and Flags["Aimbot Keybind"]) then
                    local enemy_pos = target_Paradox.target.part.Position
                    camera.CFrame = Flags["Aimbot Speed"] ~= 1 and camera.CFrame:Lerp(CFrame.lookAt(camera.CFrame.Position, enemy_pos), Flags["Aimbot Speed"]) or CFrame.lookAt(camera.CFrame.Position, enemy_pos)
                end
            end
        end
        
        do -- anti aim
            if (Flags["Anti Aim"]) then
                local root = local_char:FindFirstChild("HumanoidRootPart");
                local human = local_char:FindFirstChild("Humanoid");
                local spin_speed = math.rad(Flags["Anti Aim Speed"] / 10);

                if (local_char and local_char:FindFirstChild("HumanoidRootPart")) then
                    local cameraLook = camera.CFrame.LookVector;
                    root.CFrame = root.CFrame * CFrame.Angles(0, spin_speed, 0);
                end;

                if (root and human) then
                    human.AutoRotate = false;
                    root.CFrame = root.CFrame * CFrame.Angles(0, spin_speed, 0);
                end;
            else
                local_char:FindFirstChild("Humanoid").AutoRotate = true;
            end;
        end;

        do -- local chams
            for _, model in ipairs(folder:GetChildren()) do
                if model:IsA("Model") then
                    local highlight = model:FindFirstChildOfClass("Highlight")

                    if Flags["Local Chams"] then
                        if not highlight then
                            highlight = Instance.new("Highlight")
                            highlight.Parent = model
                        end
                        highlight.FillColor = Flags["Local Chams Accent"]
                        highlight.OutlineColor = Flags["Local Chams Accent"]
                        highlight.OutlineTransparency = Flags["Local Chams Opacity"] / 100
                        highlight.FillTransparency = Flags["Local Chams Opacity"] / 100
                        highlight.Enabled = true
                    elseif highlight then
                        highlight.Enabled = false
                    end
                end
            end
        end
        --
        do -- plr mods
            if Flags["Enable Aimbot"] or getgenv().silent then
                entry, closest_part = player_esp.functions.get_player()
            end
            --
            if quick_stack_funcs and #quick_stack_funcs > 0 then
    for _, func in pairs(quick_stack_funcs) do
        local instant_loot = Flags["instant loot"]
        debug.setconstant(func, 18, instant_loot and 0 or 0.3)
        debug.setconstant(func, 19, instant_loot and 0 or 0.1)
    end
end
            --
            if Library.Flags["sprint while attacking"] then
        for _, child in ipairs(local_char:GetChildren()) do
            if child:IsA("LocalScript") and child.Name == "ViewmodelController" then
                child:SetAttribute("CanUse", true)
            end
        end
    end
    --
    if Flags["speed enabled"] and Flags["speed key bind"] then
    local character = local_player.Character
    if character then
        local humanoid = character:FindFirstChild("Humanoid")
        local root = character:FindFirstChild("HumanoidRootPart")
        if humanoid and root then
            local dir, vel = humanoid.MoveDirection.Unit, root.Velocity
            local spd = Flags["speed"]
            if dir.Magnitude > 0 then
                root.Velocity = Vector3.new(dir.X * spd, vel.Y, dir.Z * spd)
            end
        end
    end
end
            -- Freecam toggle check
            if Flags["Enable Freecam"] and Flags["Freecam Keybind"] then
                if not freecam.enabled then
                    freecam.enabled = true
                    freecam.old_type = camera.CameraType
                    freecam.old_behavior = uis.MouseBehavior
                    camera.CameraType = Enum.CameraType.Scriptable
                    uis.MouseBehavior = Enum.MouseBehavior.LockCenter
                    if local_char and local_char:FindFirstChild("HumanoidRootPart") then
                        local_char.HumanoidRootPart.Anchored = true
                    end
                end
            else
                if freecam.enabled then
                    freecam.enabled = false
                    camera.CameraType = freecam.old_type
                    uis.MouseBehavior = freecam.old_behavior
                    if local_char and local_char:FindFirstChild("HumanoidRootPart") then
                        local_char.HumanoidRootPart.Anchored = false
                    end
                end
            end

            if freecam.enabled then
                local delta = uis:GetMouseDelta()
                freecam.pitch = math.clamp(freecam.pitch - delta.Y * Flags["Freecam Sensitivity"], -80, 80)
                freecam.yaw = freecam.yaw - delta.X * Flags["Freecam Sensitivity"]

                camera.CFrame = CFrame.new(camera.CFrame.Position)
                        * CFrame.Angles(0, math.rad(freecam.yaw), 0)
                        * CFrame.Angles(math.rad(freecam.pitch), 0, 0)

                local move = Vector3.zero
                if uis:IsKeyDown(Enum.KeyCode.W) then
                    move += Vector3.new(0, 0, -1)
                end
                if uis:IsKeyDown(Enum.KeyCode.A) then
                    move += Vector3.new(-1, 0, 0)
                end
                if uis:IsKeyDown(Enum.KeyCode.S) then
                    move += Vector3.new(0, 0, 1)
                end
                if uis:IsKeyDown(Enum.KeyCode.D) then
                    move += Vector3.new(1, 0, 0)
                end
                if uis:IsKeyDown(Enum.KeyCode.Space) then
                    move += Vector3.new(0, 1, 0)
                end
                if uis:IsKeyDown(Enum.KeyCode.LeftShift) then
                    move += Vector3.new(0, -1, 0)
                end

                if move.Magnitude > 0 then
                    camera.CFrame += (camera.CFrame - camera.CFrame.Position):VectorToWorldSpace(
                move.Unit * Flags["Freecam Speed"]
                    )
                end

                if local_char and local_char:FindFirstChild("HumanoidRootPart") then
                    local_char.HumanoidRootPart.CFrame = camera.CFrame
                end
            end
            -- spiderclimb
            if Flags["Spiderclimb"] and Flags["Spiderclimb Keybind"] then
                local plr_model = local_char or local_player.CharacterAdded:Wait()

                if plr_model and plr_model:FindFirstChild("HumanoidRootPart") then
                    local hrp = plr_model.HumanoidRootPart
                    local climb_speed = Flags["Spiderclimb Speed"]

                    local current_velocity = hrp.AssemblyLinearVelocity
                    local vertical_velocity = Vector3.new(0, climb_speed, 0)

                    hrp.CFrame = hrp.CFrame * CFrame.new(0, climb_speed * 0.1, 0)
                    hrp.AssemblyLinearVelocity =
                    Vector3.new(current_velocity.X, vertical_velocity.Y, current_velocity.Z)
                end
            end
            -- platform
            if Flags["Always Grounded"] then
                local plr_model = local_char
                --
                if plr_model and plr_model:FindFirstChild("HumanoidRootPart") then
                    local root = plr_model:FindFirstChild("HumanoidRootPart")
                    if root then
                        local targetPosition = root.Position - Vector3.new(0, 3.2, 0)
                        platform.Position = targetPosition
                    else
                        platform.Position = Vector3.new(0, 0, 0)
                    end
                else
                    platform.Position = Vector3.new(0, 0, 0)
                end
            else
                platform.Position = Vector3.new(0, 0, 0)
            end
        end
        --
        do -- lighting
            if Flags["Enable Fullbright"] then
                if lighting.ClockTime ~= (Flags["Enable Fullbright"] and 12 or lighting_cache.ClockTime) then
                    lighting.ClockTime = Flags["Enable Fullbright"] and 12 or lighting_cache.ClockTime
                end
            else
                lighting.ClockTime = lighting_cache.ClockTime
            end
            --
            if Flags["Enable No Fog"] then
                lighting.FogEnd = 100000
                local atmosphere = lighting:FindFirstChild("Atmosphere")
                if atmosphere then
                    atmosphere.Density, atmosphere.Offset, atmosphere.Glare, atmosphere.Haze = 0, 0, 0, 0
                end
            else
                lighting.FogEnd = lighting_cache.FogEnd
                local atmosphere = lighting:FindFirstChild("Atmosphere")
                if atmosphere then
                    atmosphere.Density, atmosphere.Offset, atmosphere.Glare, atmosphere.Haze =
                    lighting_cache.Density, lighting_cache.Offset, lighting_cache.Glare, lighting_cache.Haze
                end
            end
            --
            if Flags["Enable Ambient"] then
                if
                lighting.OutdoorAmbient
                        ~= (Flags["Enable Ambient"] and Flags["Outdoor Ambient Accent"] or lighting_cache.OutdoorAmbient)
                then
                    lighting.OutdoorAmbient = Flags["Enable Ambient"] and Flags["Outdoor Ambient Accent"]
                            or lighting_cache.OutdoorAmbient
                end
                if
                lighting.Ambient ~= (Flags["Enable Ambient"] and Flags["Ambient Accent"] or lighting_cache.Ambient)
                then
                    lighting.Ambient = Flags["Enable Ambient"] and Flags["Ambient Accent"] or lighting_cache.Ambient
                end
            else
                lighting.Ambient = lighting_cache.Ambient
                lighting.OutdoorAmbient = lighting_cache.OutdoorAmbient
            end
        end
        --
        do -- fov circle
            if Flags["Enable FOV"] then
                fov_circle.Frame.Visible = true
                fov_circle.Stroke.Color = Flags["FOV Accent"]

                local size = Flags["FOV Radius"] * 2 / math.tan(math.rad(camera.FieldOfView / 2))
                fov_circle.Frame.Size = UDim2.new(0, size, 0, size)
                fov_circle.Frame.Position = UDim2.new(0, uis:GetMouseLocation().X, 0, uis:GetMouseLocation().Y)
            else
                fov_circle.Frame.Visible = false
            end
        end
        -- 
        if Flags["Armor Viewer"] then
            if closest_part then
                info_viewer.background.Visible = true
                info_viewer.background.Position = UDim2.new(50 / 100, 0, 10 / 100, 0)
                info_viewer.icon_holder.Visible = true
                info_viewer.icon_holder.Position = UDim2.new(50 / 100, 0, 10 / 100, 45)
                info_viewer.name.Position = UDim2.new(0.5, 0, 0.5, 0)
                info_viewer.name.Visible = true
                info_viewer.name.Text = entry.DisplayName.. "'s Inventory"
        
                if last_targeted_player_name ~= entry.Name then
                    for _, child in ipairs(info_viewer.icon_holder:GetChildren()) do
                        if child:IsA("ImageLabel") then
                            child:Destroy();
                        end;
                    end;
                    last_targeted_player_name = entry.Name;
                end;
        
                local armorIcons = player_esp.functions.get_armor_icon(entry.Character)
                if armorIcons then
                    if #armorIcons > 0 then
                        for _, armorIcon in ipairs(armorIcons) do
                            local exists = false
                            for _, child in ipairs(info_viewer.icon_holder:GetChildren()) do
                                if child:IsA("ImageLabel") and child.Image == armorIcon then
                                    exists = true
                                    break
                                end
                            end
                            if not exists then
                                local newIcon = Instance.new("ImageLabel")
                                newIcon.Parent = info_viewer.icon_holder
                                newIcon.Size = UDim2.new(0, 75, 0, 75)
                                newIcon.Image = armorIcon
                                newIcon.Visible = true
                                newIcon.BackgroundTransparency = 1
                            end
                        end
                    end
                end
            else
                info_viewer.icon_holder.Visible = false;
                info_viewer.background.Visible = false;
                info_viewer.name.Visible = false;
            end;
        else
            for _, child in ipairs(info_viewer.icon_holder:GetChildren()) do
                if child:IsA("ImageLabel") then
                    child:Destroy();
                end;
            end;
            info_viewer.icon_holder.Visible = false;
            info_viewer.background.Visible = false;
            info_viewer.name.Visible = false;
        end;                     
        --
        do -- snaplines
            if (Flags["Enable Aimbot"] or getgenv().silent) and Flags["Enable Snaplines"] then
                local line = snap_lines.line
                if closest_part then
                    local screen_pos = camera:WorldToViewportPoint(closest_part.Position)
                    local from = Vector2.new(screen_pos.X, screen_pos.Y)
                    local to = uis:GetMouseLocation()
                    local offset = to - from
                    local middle = from + offset * 0.5
                    local distance = offset.Magnitude

                    local line = snap_lines.line
                    line.Position = UDim2.new(0, middle.X, 0, middle.Y)
                    line.Rotation = math.deg(math.atan2(offset.Y, offset.X))
                    line.Size = UDim2.new(0, math.floor(distance + 0.5), 0, 1)
                    line.BackgroundColor3 = Flags["Snaplines Accent"]
                    line.Visible = true
                else
                    line.Visible = false
                end
            else
                snap_lines.line.Visible = false
            end
        end
    end))
end
--
do -- object esp (this is lazy work but it works)
    local create_esp_object = LPH_NO_VIRTUALIZE(function(Name, Object, Icon, Override)
        local Box = player_esp.functions.create_drawing("Frame", { Parent = gethui(), Visible = true })
        local Label = player_esp.functions.create_drawing("TextLabel", {
            Text = "",
            Visible = false,
            Parent = Box,
            Position = UDim2.new(0.5, 0, 0, -10),
            Size = UDim2.new(0, 100, 0, -20),
            AnchorPoint = Vector2.new(0.5, 0.5),
            BackgroundTransparency = 1,
            TextColor3 = Color3.fromRGB(255, 255, 255),
            TextSize = 13,
            TextStrokeTransparency = 0,
            TextStrokeColor3 = Color3.fromRGB(0, 0, 0),
            RichText = true,
        })
        
        --
        do
            local Updater
            Updater = run_service.RenderStepped:Connect(function()
                if Object and Object.Parent ~= nil and Object ~= nil then
                    local Position, Onscreen = camera:WorldToViewportPoint(Object.Position)
                    local Distance = math.floor((Object.Position - camera.CFrame.p).magnitude / 3.5714285714)

                    -- Text
                    if
                    Onscreen
                            and Flags[string.format("Enable %s ESP", Name)]
                            and Distance <= Flags["World ESP Max Distance"]
                    then
                        Label.Visible = true
                        Label.Text = string.format("%s\n[%sm]", Name, Distance)
                        Label.TextColor3 = Flags[string.format("%s ESP Color", Name)]
                        Label.TextSize = 11
                        Label.FontFace =
                        Font.new([[rbxassetid://12187365977]], Enum.FontWeight.SemiBold, Enum.FontStyle.Normal)
                        Box.Position = UDim2.new(0, Position.X, 0, Position.Y)
                        Box.Position = UDim2.new(0, Position.X, 0, Position.Y)
                        Box.Size = UDim2.new(0, 0, 0, 0)
                        Box.Transparency = 1
                    else
                        Label.Visible = false
                    end
                else
                    Label:Destroy()
                    Box:Destroy()
                    Updater:Disconnect()
                end
            end)
        end
    end)
    --
    local Plants = workspace:FindFirstChild("Plants")
    if Plants then
        for _, Plant in pairs(Plants:GetChildren()) do
            if Plant.Name == "Wool Plant" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Wool", Main, "")
            end
        end
        --
        Plants.ChildAdded:Connect(function(Plant)
            if Plant.Name == "Wool Plant" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Wool", Main, "")
            end
        end)
    end
    --
    local Nodes = workspace:FindFirstChild("Nodes")
    if Nodes then
        for _, Plant in pairs(Nodes:GetChildren()) do
            if Plant.Name == "Stone_Node" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Stone", Main, "")
            elseif Plant.Name == "Metal_Node" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Metal", Main, "")
            elseif Plant.Name == "Phosphate_Node" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Phosphate", Main, "")
            end
        end
        --
        Nodes.ChildAdded:Connect(function(Plant)
            if Plant.Name == "Stone_Node" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Stone", Main, "")
            elseif Plant.Name == "Metal_Node" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Metal", Main, "")
            elseif Plant.Name == "Phosphate_Node" then
                local Main = game.FindFirstChild(Plant, "Main")
                create_esp_object("Phosphate", Main, "")
            end
        end)
    end
end
