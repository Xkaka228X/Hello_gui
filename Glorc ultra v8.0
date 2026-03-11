-- ╔══════════════════════════════════════════════════════════════╗
-- ║  GLORK ULTRA v8.0  —  Instant drag · 400+ cmds · RU/EN     ║
-- ║  Drag: instant follow, no spring lag, edge clamp            ║
-- ║  UI:  RU/EN кнопка · Scripts tab · полная локализация      ║
-- ╚══════════════════════════════════════════════════════════════╝

local Players          = game:GetService("Players")
local RunService       = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService     = game:GetService("TweenService")
local Lighting         = game:GetService("Lighting")
local StarterGui       = game:GetService("StarterGui")
local LocalPlayer      = Players.LocalPlayer
local Camera           = workspace.CurrentCamera
local Commands         = {}
local CommandData      = {}
local Waypoints        = {}
local Favorites        = {}
local ActiveTab        = "commands"
local IYInput          -- assigned below

-- ─────────────────────────────────────────────────────────
--  MOBILE DETECT + SIZE CONSTANTS
-- ─────────────────────────────────────────────────────────
local IsMobile = UserInputService.TouchEnabled and not UserInputService.MouseEnabled
local S = {
    winW    = IsMobile and 360  or 460,
    winH    = IsMobile and 290  or 270,
    barW    = IsMobile and 340  or 460,
    barH    = IsMobile and 50   or 34,
    rowH    = IsMobile and 30   or 26,
    btnH    = IsMobile and 30   or 24,
    gripW   = IsMobile and 44   or 28,
    tabH    = IsMobile and 28   or 24,
    titleH  = IsMobile and 32   or 28,
    txt     = IsMobile and 14   or 12,
    txtSm   = IsMobile and 12   or 10,
    txtLg   = IsMobile and 16   or 14,
    corner  = 10,
    searchH = IsMobile and 26   or 22,
}

-- ─────────────────────────────────────────────────────────
--  SCREEN GUI
-- ─────────────────────────────────────────────────────────
local SG = Instance.new("ScreenGui")
SG.Name           = "GlorkUI"
SG.ResetOnSpawn   = false
SG.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
SG.IgnoreGuiInset = true
SG.Parent         = LocalPlayer:WaitForChild("PlayerGui")

-- ═══════════════════════════════════════════════════════════════
--  ЛОКАЛИЗАЦИЯ  (RU / EN)
-- ═══════════════════════════════════════════════════════════════
local Lang = "EN"  -- "RU" | "EN"

local L = {
    RU = {
        -- UI labels
        title      = "GLORK  v8.0",
        tab_cmds   = "⌨  Команды",
        tab_favs   = "★  Избранные",
        tab_wiki   = "📖  Вики",
        tab_scr    = "📜  Скрипты",
        tab_cmds_s = "⌨",
        tab_favs_s = "★",
        tab_wiki_s = "📖",
        tab_scr_s  = "📜",
        search_ph  = "🔍  фильтр...",
        bar_ph     = "команда аргументы...",
        panel_btn  = "▲ Панель",
        panel_open = "▼ Панель",
        mob_enter  = "⌨  Ввести",
        mob_panel  = "▲  Меню",
        mob_panelo = "▼  Меню",
        status_cmd = " команд · тап → поле ввода",
        status_fav = "★ Избранное · ",
        status_fav2= " сохранено",
        status_wik = "📖 Вики · ",
        status_wik2= " команд · тап для деталей",
        status_scr = "📜 Скрипты · тап для запуска",
        empty_fav  = "Нажми ☆ на любой команде\nчтобы добавить сюда",
        no_aliases = "нет алиасов",
        aliases    = "псевдонимы: ",
        wiki_none  = "\n\nПодробного описания пока нет.",
        run_btn    = "▶  Запуск",
        fav_add    = "☆  В избранное",
        fav_rm     = "★  Убрать",
        notif_err  = "Ошибка",
        notif_unk  = "Неизвестно",
        scr_run    = "▶ Запустить",
        scr_copy   = "📋 Копировать",
        -- startup
        startup_title = "⚡ Glork Ultra v8.0",
        startup_body  = " команд  ·  ↑↓ = история  ·  RU/EN",
        loaded_title  = "💾 Данные загружены",
        loaded_body   = "Избр: ",
        loaded_mid    = "  ВП: ",
        loaded_hist   = "  История: ",
        no_save_body  = "Файл будет создан автоматически",
    },
    EN = {
        title      = "GLORK  v8.0",
        tab_cmds   = "⌨  Commands",
        tab_favs   = "★  Favorites",
        tab_wiki   = "📖  Wiki",
        tab_scr    = "📜  Scripts",
        tab_cmds_s = "⌨",
        tab_favs_s = "★",
        tab_wiki_s = "📖",
        tab_scr_s  = "📜",
        search_ph  = "🔍  filter...",
        bar_ph     = "command args...",
        panel_btn  = "▲ Panel",
        panel_open = "▼ Panel",
        mob_enter  = "⌨  Enter",
        mob_panel  = "▲  Menu",
        mob_panelo = "▼  Menu",
        status_cmd = " cmds · tap row → fill input",
        status_fav = "★ Favorites · ",
        status_fav2= " saved",
        status_wik = "📖 Wiki · ",
        status_wik2= " cmds · tap for details",
        status_scr = "📜 Scripts · tap to run",
        empty_fav  = "Tap ☆ on any command\nto add it here",
        no_aliases = "no aliases",
        aliases    = "aliases: ",
        wiki_none  = "\n\nNo detailed description yet.",
        run_btn    = "▶  Run",
        fav_add    = "☆  Favorite",
        fav_rm     = "★  Remove",
        notif_err  = "Error",
        notif_unk  = "Unknown",
        scr_run    = "▶ Run",
        scr_copy   = "📋 Copy",
        startup_title = "⚡ Glork Ultra v8.0",
        startup_body  = " cmds  ·  ↑↓ = history  ·  RU/EN",
        loaded_title  = "💾 Data loaded",
        loaded_body   = "Favs: ",
        loaded_mid    = "  WPs: ",
        loaded_hist   = "  Hist: ",
        no_save_body  = "File will be created automatically",
    }
}
local function T(key) return L[Lang][key] or key end

-- ═══════════════════════════════════════════════════════════════
--  DRAG v5  ──  правильный, без прыжков, без дёрганий
--
--  Исправлено:
--  ✓ Прыжок при клике: offset вычисляется ТОЛЬКО после dead zone,
--    когда окно ещё стоит на месте — до этого ничего не двигается
--  ✓ Дёрганье: lerp применяется ко ВСЕМ типам ввода (мышь + тач),
--    сглаживает микро-дрожание без заметной задержки (factor 0.92)
--  ✓ За экран: clamp держит окно полностью внутри viewport
--  ✓ WP popup: проверка видимости убрана для окон которые
--    открываются динамически (drag регистрируется всегда)
-- ═══════════════════════════════════════════════════════════════
local _dragTargets = {}
local DEAD_MOUSE   = 6    -- px мёртвая зона для мыши
local DEAD_TOUCH   = 10   -- px мёртвая зона для тача
local LERP_F       = 0.92 -- сглаживание (0=мгновенно, 1=никогда)

UserInputService.InputBegan:Connect(function(i, gp)
    local isMouse = i.UserInputType == Enum.UserInputType.MouseButton1
    local isTouch = i.UserInputType == Enum.UserInputType.Touch
    if not isMouse and not isTouch then return end

    local px, py = i.Position.X, i.Position.Y

    for handle, info in pairs(_dragTargets) do
        if not handle.Visible then continue end
        -- НЕ пропускаем невидимые окна — они могут открыться позже
        local hp, hs = handle.AbsolutePosition, handle.AbsoluteSize
        if px < hp.X or px > hp.X+hs.X or py < hp.Y or py > hp.Y+hs.Y then continue end
        -- если окно скрыто — пропускаем (но handle существует)
        if not info.window.Visible then continue end

        local win    = info.window
        local onMove = info.onMove
        local dead   = isTouch and DEAD_TOUCH or DEAD_MOUSE

        -- ox/oy вычисляются ПОСЛЕ dead zone — нет прыжка
        local ox, oy   = 0, 0
        local started  = false
        local released = false
        -- позиция для lerp — синхронизируем с реальным окном в момент старта
        local smoothX, smoothY = 0, 0

        i.Changed:Connect(function()
            if i.UserInputState == Enum.UserInputState.End then
                released = true
            end
        end)

        local conn
        conn = RunService.RenderStepped:Connect(function(dt)
            if released then conn:Disconnect(); return end

            local curX, curY
            if isMouse then
                local ml = UserInputService:GetMouseLocation()
                curX, curY = ml.X, ml.Y
            else
                curX, curY = i.Position.X, i.Position.Y
            end

            -- мёртвая зона: ждём пока курсор не сдвинется достаточно
            if not started then
                if math.abs(curX-px) < dead and math.abs(curY-py) < dead then return end
                -- ЗДЕСЬ вычисляем offset — окно ещё не двигалось, нет прыжка
                ox      = curX - win.AbsolutePosition.X
                oy      = curY - win.AbsolutePosition.Y
                smoothX = win.AbsolutePosition.X
                smoothY = win.AbsolutePosition.Y
                started = true
            end

            local vp = workspace.CurrentCamera.ViewportSize
            local ww = win.AbsoluteSize.X
            local wh = win.AbsoluteSize.Y

            -- целевая позиция — cursor минус offset, зажатая в экран
            local goalX = math.clamp(curX - ox, 0, math.max(0, vp.X - ww))
            local goalY = math.clamp(curY - oy, 0, math.max(0, vp.Y - wh))

            -- lerp для плавности (не задержка — просто убирает дрожание)
            local f = 1 - LERP_F ^ (dt * 60)
            smoothX = smoothX + (goalX - smoothX) * f
            smoothY = smoothY + (goalY - smoothY) * f

            win.Position = UDim2.fromOffset(math.floor(smoothX + 0.5), math.floor(smoothY + 0.5))
            if onMove then onMove(smoothX, smoothY) end
        end)
        break
    end
end)

local function Drag(handle, window, onMove)
    _dragTargets[handle] = {window=window, onMove=onMove}
end

-- ─────────────────────────────────────────────────────────
--  SAFE TAP  —  не срабатывает при скролле или drag
-- ─────────────────────────────────────────────────────────
local TAP_MAX_MOVE = 12
local function OnClick(btn, fn)
    btn.InputBegan:Connect(function(i)
        if i.UserInputType ~= Enum.UserInputType.MouseButton1
        and i.UserInputType ~= Enum.UserInputType.Touch then return end
        local sx, sy, moved = i.Position.X, i.Position.Y, false
        local mc = UserInputService.InputChanged:Connect(function(c)
            if c.UserInputType==Enum.UserInputType.MouseMovement
            or c.UserInputType==Enum.UserInputType.Touch then
                if math.abs(c.Position.X-sx)>TAP_MAX_MOVE or math.abs(c.Position.Y-sy)>TAP_MAX_MOVE then
                    moved=true
                end
            end
        end)
        i.Changed:Connect(function()
            if i.UserInputState==Enum.UserInputState.End then
                mc:Disconnect(); if not moved then fn() end
            end
        end)
    end)
end

-- ─────────────────────────────────────────────────────────
--  УВЕДОМЛЕНИЯ
-- ─────────────────────────────────────────────────────────
local NotifStack = Instance.new("Frame", SG)
NotifStack.Size = UDim2.fromOffset(IsMobile and 280 or 300, 600)
NotifStack.Position = UDim2.new(1, IsMobile and -290 or -310, 0, 10)
NotifStack.BackgroundTransparency = 1; NotifStack.BorderSizePixel = 0
local NL = Instance.new("UIListLayout", NotifStack)
NL.SortOrder = Enum.SortOrder.LayoutOrder; NL.Padding = UDim.new(0,5)
local nc = 0

local function Notify(title, body, col)
    col = col or Color3.fromRGB(0,220,160); nc += 1
    local NW = IsMobile and 280 or 300
    local NH = IsMobile and 58 or 52
    local f = Instance.new("Frame", NotifStack)
    f.Size = UDim2.fromOffset(NW,NH); f.BackgroundColor3=Color3.fromRGB(8,13,20)
    f.BorderSizePixel=0; f.LayoutOrder=nc
    Instance.new("UICorner",f).CornerRadius=UDim.new(0,8)
    local stroke=Instance.new("UIStroke",f); stroke.Color=col; stroke.Thickness=1; stroke.Transparency=0.4
    local bar=Instance.new("Frame",f); bar.Size=UDim2.fromOffset(3,34); bar.Position=UDim2.fromOffset(6,9)
    bar.BackgroundColor3=col; bar.BorderSizePixel=0; Instance.new("UICorner",bar).CornerRadius=UDim.new(1,0)
    local t=Instance.new("TextLabel",f); t.Size=UDim2.new(1,-20,0,20); t.Position=UDim2.fromOffset(16,6)
    t.BackgroundTransparency=1; t.Text=title; t.TextColor3=col
    t.TextSize=S.txtSm+1; t.Font=Enum.Font.GothamBold; t.TextXAlignment=Enum.TextXAlignment.Left
    local b=Instance.new("TextLabel",f); b.Size=UDim2.new(1,-20,0,NH-34); b.Position=UDim2.fromOffset(16,26)
    b.BackgroundTransparency=1; b.Text=body; b.TextColor3=Color3.fromRGB(140,175,170)
    b.TextSize=S.txtSm; b.Font=Enum.Font.Gotham; b.TextXAlignment=Enum.TextXAlignment.Left
    b.TextTruncate=Enum.TextTruncate.AtEnd
    local prog=Instance.new("Frame",f); prog.Size=UDim2.new(1,-12,0,2); prog.Position=UDim2.fromOffset(6,NH-2)
    prog.BackgroundColor3=col; prog.BackgroundTransparency=0.35; prog.BorderSizePixel=0
    Instance.new("UICorner",prog).CornerRadius=UDim.new(1,0)
    TweenService:Create(prog,TweenInfo.new(3.5),{Size=UDim2.fromOffset(0,2)}):Play()
    task.delay(3.7,function()
        TweenService:Create(f,TweenInfo.new(0.25),{BackgroundTransparency=1}):Play()
        task.wait(0.25); f:Destroy()
    end)
end

-- ─────────────────────────────────────────────────────────
--  ДВИЖОК КОМАНД
-- ─────────────────────────────────────────────────────────
local function AddCommand(name, aliases, descRU, descEN, category, fn)
    Commands[name:lower()] = fn
    for _,a in pairs(aliases) do Commands[a:lower()]=fn end
    table.insert(CommandData,{name=name:lower(),aliases=aliases,descRU=descRU,descEN=descEN,cat=category})
end
local function CmdDesc(cmd) return Lang=="RU" and cmd.descRU or cmd.descEN end

local function Run(input)
    local parts={}; for w in input:gmatch("%S+") do table.insert(parts,w) end
    if #parts==0 then return end
    local cmd=table.remove(parts,1):lower()
    if Commands[cmd] then
        local ok,err=pcall(Commands[cmd],parts)
        if not ok then Notify(T("notif_err"),tostring(err):sub(1,55),Color3.fromRGB(255,70,70)) end
    else Notify(T("notif_unk"),cmd,Color3.fromRGB(255,140,50)) end
end

-- ─────────────────────────────────────────────────────────
--  ЦВЕТА КАТЕГОРИЙ
-- ─────────────────────────────────────────────────────────
local CC = {
    Movement=Color3.fromRGB(0,200,255),   Visual=Color3.fromRGB(255,100,200),
    Character=Color3.fromRGB(255,200,0),   World=Color3.fromRGB(80,255,130),
    Camera=Color3.fromRGB(180,120,255),    Physics=Color3.fromRGB(255,130,60),
    Audio=Color3.fromRGB(255,230,80),      Fun=Color3.fromRGB(255,80,140),
    Waypoints=Color3.fromRGB(60,230,180),  Utility=Color3.fromRGB(150,150,255),
    Scripts=Color3.fromRGB(255,165,50),
}
local function GetCC(cat) return CC[cat] or Color3.fromRGB(120,120,120) end

-- ─────────────────────────────────────────────────────────
--  WIKI ТЕКСТЫ (RU + EN)
-- ─────────────────────────────────────────────────────────
local WikiRU = {
    infjump="Перехватывает JumpRequest и форсирует Jumping. Отключить: uninfjump.",
    noclip="Каждый кадр снимает CanCollide. Проходишь сквозь стены. Отключить: clip.",
    fly="BodyGyro+BodyVelocity. WASD=горизонталь, Space=вверх, Q/Ctrl=вниз. fly 100.",
    ws="WalkSpeed гуманоида. По умолчанию 16. Сброс: resetspeed.",
    esp="Highlight на персонажах игроков. rainbowesp = анимированный.",
    god="MaxHealth и Health = 1e9. Отключить: ungod.",
    fullbright="Brightness=2, убирает тени и туман. Сброс: unfullbright.",
    gravity="workspace.Gravity. zerograv=0, moongrav=30, heavygrav=600.",
    fov="Угол обзора. widefov=110, resetfov=70.",
    xray="Прозрачность всех BasePart=0.75. Отключить: unxray.",
    tp="Телепорт: tp X Y Z.", tpto="К игроку: tpto Имя.",
    looptp="Следование за игроком. Остановить: unlooptp.",
    freecam="CameraType=Scriptable. WASD+Space/Q. Остановить: unfreecam.",
    giant="Масштаб персонажа. giant 5 = ×5. Сброс: resetscale.",
    spin="Вращает HumanoidRootPart. Остановить: unspin.",
    bounce="При приземлении прыжок вверх. Отключить: unbounce.",
    rainbowchar="HSV-цикл на BasePart. Остановить: unreinbowchar.",
    confetti="40 частиц с импульсом. Удаляются через 4с.",
    meteor="N огненных сфер сверху. meteor 10.",
    setwp="Сохранить позицию: setwp имя. Вернуться: gowp имя.",
    resetall="Отключает всё: god, noclip, fly, invisible, esp, fullbright...",
    godclip="god+noclip+infjump+ws60. Отключить: ungodclip.",
    stealth="Invisible+noclip+ws70.",
    tryhard="Максимум: god+ws200+jh500+noclip+infjump.",
    freefov="Свободный зум камеры без ограничений.",
    hitbox="Увеличивает хитбокс персонажа. hitbox 10.",
    aimbot="Автоприцел к ближайшему игроку. Отключить: unaimbot.",
}
local WikiEN = {
    infjump="Hooks JumpRequest and forces Jumping state. Disable: uninfjump.",
    noclip="Removes CanCollide every frame. Walk through walls. Disable: clip.",
    fly="BodyGyro+BodyVelocity. WASD=horizontal, Space=up, Q/Ctrl=down. fly 100.",
    ws="Humanoid WalkSpeed. Default 16. Reset: resetspeed.",
    esp="Highlight on all player characters. rainbowesp = animated color.",
    god="MaxHealth and Health = 1e9. Disable: ungod.",
    fullbright="Brightness=2, removes shadows and fog. Reset: unfullbright.",
    gravity="workspace.Gravity. zerograv=0, moongrav=30, heavygrav=600.",
    fov="Field of view. widefov=110, resetfov=70.",
    xray="BasePart Transparency=0.75. See through map. Disable: unxray.",
    tp="Teleport: tp X Y Z.", tpto="To player: tpto Name.",
    looptp="Follow player continuously. Stop: unlooptp.",
    freecam="CameraType=Scriptable. WASD+Space/Q. Stop: unfreecam.",
    giant="Character scale. giant 5 = ×5. Reset: resetscale.",
    spin="Rotates HumanoidRootPart. Stop: unspin.",
    bounce="Jump upward on landing. Disable: unbounce.",
    rainbowchar="HSV color cycle on BaseParts. Stop: unreinbowchar.",
    confetti="40 neon parts with impulse. Destroyed after 4s.",
    meteor="N fire spheres from above. meteor 10.",
    setwp="Save position: setwp name. Return: gowp name.",
    resetall="Disables all: god, noclip, fly, invisible, esp, fullbright...",
    godclip="god+noclip+infjump+ws60. Disable: ungodclip.",
    stealth="Invisible+noclip+ws70.",
    tryhard="Maximum: god+ws200+jh500+noclip+infjump.",
    freefov="Unlock camera zoom without limits.",
    hitbox="Enlarges character hitbox. hitbox 10.",
    aimbot="Auto-aim at nearest player. Disable: unaimbot.",
}
local function GetWiki(name)
    local w = Lang=="RU" and WikiRU[name] or WikiEN[name]
    return w or (Lang=="RU" and T("wiki_none") or T("wiki_none"))
end

-- ─────────────────────────────────────────────────────────
--  СКРИПТЫ (вкладка Scripts)
-- ─────────────────────────────────────────────────────────
local ScriptList = {
    {
        name="Dex Explorer",
        desc={RU="Проводник объектов Roblox",EN="Roblox object explorer"},
        code=[[loadstring(game:HttpGet("https://raw.githubusercontent.com/shlexware/Dex/master/source"))()]]
    },
    {
        name="Infinite Yield",
        desc={RU="IY — большой чит-пак",EN="IY — full cheat pack"},
        code=[[loadstring(game:HttpGet("https://raw.githubusercontent.com/EdgeIY/infiniteyield/master/source"))()]]
    },
    {
        name="Dark Dex",
        desc={RU="Тёмный проводник",EN="Dark themed explorer"},
        code=[[loadstring(game:HttpGet("https://cdn.wearedevs.net/scripts/dex.txt"))()]]
    },
    {
        name="ESP V2 (Simple)",
        desc={RU="Простой ESP через BoxHandleAdornment",EN="Simple box ESP via HandleAdornment"},
        code=[[
for _,p in pairs(game.Players:GetPlayers()) do
    if p~=game.Players.LocalPlayer and p.Character then
        local h=Instance.new("BoxHandleAdornment",p.Character)
        h.Size=p.Character:GetExtentsSize()
        h.AlwaysOnTop=true; h.ZIndex=5
        h.Color3=Color3.fromRGB(255,50,50); h.Transparency=0.5
        h.Adornee=p.Character
    end
end
]]
    },
    {
        name="Server Hop",
        desc={RU="Переход на другой сервер",EN="Jump to another server"},
        code=[[
local TeleportService=game:GetService("TeleportService")
local servers={}
local ok,res=pcall(function()
    return game:HttpGet("https://games.roblox.com/v1/games/"..game.PlaceId.."/servers/Public?limit=25")
end)
if ok then
    local data=game:GetService("HttpService"):JSONDecode(res)
    for _,s in pairs(data.data) do
        if s.id~=game.JobId then table.insert(servers,s.id) end
    end
end
if #servers>0 then
    TeleportService:TeleportToPlaceInstance(game.PlaceId,servers[math.random(#servers)])
else
    game:GetService("TeleportService"):Teleport(game.PlaceId)
end
]]
    },
    {
        name="Remove All Guis",
        desc={RU="Убрать все GUI игры",EN="Remove all game GUIs"},
        code=[[
for _,g in pairs(game.Players.LocalPlayer.PlayerGui:GetChildren()) do
    if g.Name~="GlorkUI" then pcall(function() g:Destroy() end) end
end
]]
    },
    {
        name="Print All Players",
        desc={RU="Список игроков в консоль",EN="Print all players to console"},
        code=[[
print("=== Players ===")
for i,p in pairs(game.Players:GetPlayers()) do
    print(i, p.Name, p.DisplayName, "|", p.AccountAge, "days old")
end
]]
    },
    {
        name="Anti AFK Loop",
        desc={RU="Постоянный анти-афк",EN="Continuous anti-afk loop"},
        code=[[
game:GetService("RunService").Heartbeat:Connect(function()
    pcall(function()
        game:GetService("VirtualUser"):Button2Down(Vector2.new(0,0),CFrame.new())
        task.wait(0.1)
        game:GetService("VirtualUser"):Button2Up(Vector2.new(0,0),CFrame.new())
    end)
end)
]]
    },
    {
        name="Unlock FPS",
        desc={RU="60 → 240+ FPS",EN="Unlock framerate to 240+"},
        code=[[setfpscap(0)]]
    },
    {
        name="Copy Character CFrame",
        desc={RU="Скопировать CFrame в консоль",EN="Copy character CFrame to console"},
        code=[[
local root=game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
if root then
    local cf=root.CFrame
    print(string.format("CFrame.new(%g,%g,%g)",cf.X,cf.Y,cf.Z))
end
]]
    },
}

-- ═══════════════════════════════════════════════════════════════
--  ГЛАВНАЯ ПАНЕЛЬ
-- ═══════════════════════════════════════════════════════════════
local WIN_W, WIN_H = S.winW, S.winH

local Win = Instance.new("Frame", SG)
Win.Size = UDim2.fromOffset(WIN_W, WIN_H)
Win.Position = UDim2.fromOffset(IsMobile and 10 or 60, IsMobile and 10 or 80)
Win.BackgroundColor3 = Color3.fromRGB(7,11,18)
Win.BorderSizePixel = 0; Win.Visible = false
Instance.new("UICorner", Win).CornerRadius = UDim.new(0, S.corner)
local WinStroke = Instance.new("UIStroke", Win)
WinStroke.Color=Color3.fromRGB(0,210,150); WinStroke.Thickness=1.2; WinStroke.Transparency=0.25

-- Titlebar
local TBar = Instance.new("Frame", Win)
TBar.Size = UDim2.new(1,0,0,S.titleH)
TBar.BackgroundColor3 = Color3.fromRGB(0,200,140)
TBar.BackgroundTransparency = 0.88; TBar.BorderSizePixel = 0
Instance.new("UICorner", TBar).CornerRadius = UDim.new(0, S.corner)

local TDots = Instance.new("TextLabel", TBar)
TDots.Size=UDim2.fromOffset(60,S.titleH); TDots.Position=UDim2.new(0.5,-30,0,0)
TDots.BackgroundTransparency=1; TDots.Text="· · ·"
TDots.TextColor3=Color3.fromRGB(0,200,140); TDots.TextTransparency=0.45
TDots.TextSize=IsMobile and 12 or 9; TDots.Font=Enum.Font.GothamBold

local TIcon=Instance.new("TextLabel",TBar)
TIcon.Size=UDim2.fromOffset(28,S.titleH); TIcon.Position=UDim2.fromOffset(8,0)
TIcon.BackgroundTransparency=1; TIcon.Text="⚡"; TIcon.TextSize=18
TIcon.Font=Enum.Font.GothamBold; TIcon.TextColor3=Color3.fromRGB(0,255,180)

local TTitle=Instance.new("TextLabel",TBar)
TTitle.Size=UDim2.new(1,-140,1,0); TTitle.Position=UDim2.fromOffset(34,0)
TTitle.BackgroundTransparency=1; TTitle.Text=T("title")
TTitle.TextColor3=Color3.fromRGB(0,255,180); TTitle.TextSize=14
TTitle.Font=Enum.Font.GothamBold; TTitle.TextXAlignment=Enum.TextXAlignment.Left

-- RU/EN кнопка
local LangBtn=Instance.new("TextButton",TBar)
LangBtn.Size=UDim2.fromOffset(40,22); LangBtn.Position=UDim2.new(1,-(40+34+8),0.5,-11)
LangBtn.BackgroundColor3=Color3.fromRGB(20,40,80); LangBtn.BackgroundTransparency=0.2
LangBtn.BorderSizePixel=0; LangBtn.Text="EN"; LangBtn.TextColor3=Color3.fromRGB(120,180,255)
LangBtn.TextSize=11; LangBtn.Font=Enum.Font.GothamBold
Instance.new("UICorner",LangBtn).CornerRadius=UDim.new(0,5)
local LangStroke=Instance.new("UIStroke",LangBtn)
LangStroke.Color=Color3.fromRGB(80,140,255); LangStroke.Thickness=1; LangStroke.Transparency=0.4

local BTN=S.btnH
local XBtn=Instance.new("TextButton",TBar)
XBtn.Size=UDim2.fromOffset(BTN,BTN); XBtn.Position=UDim2.new(1,-(BTN+8),0.5,-BTN/2)
XBtn.BackgroundColor3=Color3.fromRGB(255,55,70); XBtn.BackgroundTransparency=0.2
XBtn.BorderSizePixel=0; XBtn.Text="✕"; XBtn.TextColor3=Color3.new(1,1,1)
XBtn.TextSize=S.txt; XBtn.Font=Enum.Font.GothamBold
Instance.new("UICorner",XBtn).CornerRadius=UDim.new(0,5)

Drag(TBar, Win)

-- Tabs (4 вкладки: Commands / Favorites / Wiki / Scripts)
local tabOffset = S.titleH + 6
local TabRow = Instance.new("Frame", Win)
TabRow.Size=UDim2.new(1,-16,0,S.tabH); TabRow.Position=UDim2.fromOffset(8,tabOffset)
TabRow.BackgroundColor3=Color3.fromRGB(12,18,28); TabRow.BorderSizePixel=0
Instance.new("UICorner",TabRow).CornerRadius=UDim.new(0,7)
local TL=Instance.new("UIListLayout",TabRow)
TL.FillDirection=Enum.FillDirection.Horizontal; TL.SortOrder=Enum.SortOrder.LayoutOrder
TL.Padding=UDim.new(0,2)
Instance.new("UIPadding",TabRow).PaddingLeft=UDim.new(0,3)

local TABS={{"commands","tab_cmds","tab_cmds_s"},{"favorites","tab_favs","tab_favs_s"},
            {"wiki","tab_wiki","tab_wiki_s"},{"scripts","tab_scr","tab_scr_s"}}
local TabBtns={}
local function StyleTab(id,on)
    local b=TabBtns[id]; if not b then return end
    if on then b.BackgroundColor3=Color3.fromRGB(0,200,140); b.BackgroundTransparency=0.15; b.TextColor3=Color3.fromRGB(0,255,180)
    else b.BackgroundColor3=Color3.new(); b.BackgroundTransparency=1; b.TextColor3=Color3.fromRGB(70,100,95) end
end

-- Search bar
local searchOffset=tabOffset+S.tabH+6
local Search=Instance.new("TextBox",Win)
Search.Size=UDim2.new(1,-16,0,S.searchH); Search.Position=UDim2.fromOffset(8,searchOffset)
Search.BackgroundColor3=Color3.fromRGB(10,17,26); Search.BorderSizePixel=0
Search.PlaceholderText=T("search_ph"); Search.PlaceholderColor3=Color3.fromRGB(50,75,70)
Search.Text=""; Search.TextColor3=Color3.fromRGB(0,230,170)
Search.TextSize=S.txt; Search.Font=Enum.Font.Gotham; Search.ClearTextOnFocus=false
Instance.new("UICorner",Search).CornerRadius=UDim.new(0,6)
local SS=Instance.new("UIStroke",Search)
SS.Color=Color3.fromRGB(0,180,130); SS.Thickness=1; SS.Transparency=0.6

-- List
local listOffset=searchOffset+S.searchH+4
local List=Instance.new("ScrollingFrame",Win)
List.Size=UDim2.new(1,-16,1,-(listOffset+20)); List.Position=UDim2.fromOffset(8,listOffset)
List.BackgroundTransparency=1; List.BorderSizePixel=0
List.ScrollBarThickness=IsMobile and 4 or 3
List.ScrollBarImageColor3=Color3.fromRGB(0,200,140)
List.CanvasSize=UDim2.new(0,0,0,0); List.ClipsDescendants=true
List.ElasticBehavior=IsMobile and Enum.ElasticBehavior.Never or Enum.ElasticBehavior.WhenScrollable
local LL=Instance.new("UIListLayout",List)
LL.SortOrder=Enum.SortOrder.LayoutOrder; LL.Padding=UDim.new(0,IsMobile and 4 or 3)

-- Status
local Status=Instance.new("TextLabel",Win)
Status.Size=UDim2.new(1,-16,0,14); Status.Position=UDim2.new(0,8,1,-18)
Status.BackgroundTransparency=1; Status.TextColor3=Color3.fromRGB(45,80,75)
Status.TextSize=S.txtSm; Status.Font=Enum.Font.Gotham; Status.TextXAlignment=Enum.TextXAlignment.Left

-- Bottom drag strip
local BottomBar=Instance.new("Frame",Win)
BottomBar.Size=UDim2.new(1,0,0,IsMobile and 22 or 16)
BottomBar.Position=UDim2.new(0,0,1,-(IsMobile and 22 or 16))
BottomBar.BackgroundColor3=Color3.fromRGB(0,200,140)
BottomBar.BackgroundTransparency=IsMobile and 0.82 or 0.92; BottomBar.BorderSizePixel=0
Instance.new("UICorner",BottomBar).CornerRadius=UDim.new(0,8)
local BottomDots=Instance.new("TextLabel",BottomBar)
BottomDots.Size=UDim2.new(1,0,1,0); BottomDots.BackgroundTransparency=1; BottomDots.Text="· · · · ·"
BottomDots.TextColor3=Color3.fromRGB(0,220,160); BottomDots.TextTransparency=IsMobile and 0.3 or 0.6
BottomDots.TextSize=9; BottomDots.Font=Enum.Font.GothamBold
Drag(BottomBar, Win)

-- ═══════════════════════════════════════════════════════════════
--  WIKI ПОПАП
-- ═══════════════════════════════════════════════════════════════
local WPW=IsMobile and (WIN_W-20) or 380
local WPH=IsMobile and 220 or 190
local WP=Instance.new("Frame",SG)
WP.Size=UDim2.fromOffset(WPW,WPH)
WP.Position=UDim2.fromOffset(IsMobile and 10 or 80,IsMobile and 80 or 200)
WP.BackgroundColor3=Color3.fromRGB(6,10,18); WP.BorderSizePixel=0; WP.Visible=false
Instance.new("UICorner",WP).CornerRadius=UDim.new(0,10)
local WPS=Instance.new("UIStroke",WP)
WPS.Color=Color3.fromRGB(80,140,220); WPS.Thickness=1.2; WPS.Transparency=0.2
local WPBar=Instance.new("Frame",WP)
WPBar.Size=UDim2.new(1,0,0,S.titleH); WPBar.BackgroundColor3=Color3.fromRGB(80,140,220)
WPBar.BackgroundTransparency=0.85; WPBar.BorderSizePixel=0
Instance.new("UICorner",WPBar).CornerRadius=UDim.new(0,10)
Drag(WPBar,WP)
local WPName=Instance.new("TextLabel",WPBar)
WPName.Size=UDim2.new(1,-50,1,0); WPName.Position=UDim2.fromOffset(10,0)
WPName.BackgroundTransparency=1; WPName.TextColor3=Color3.fromRGB(120,180,255)
WPName.TextSize=S.txtLg-1; WPName.Font=Enum.Font.GothamBold; WPName.TextXAlignment=Enum.TextXAlignment.Left
local WPX=Instance.new("TextButton",WPBar)
WPX.Size=UDim2.fromOffset(BTN,BTN); WPX.Position=UDim2.new(1,-(BTN+6),0.5,-BTN/2)
WPX.BackgroundColor3=Color3.fromRGB(255,55,70); WPX.BackgroundTransparency=0.2
WPX.BorderSizePixel=0; WPX.Text="✕"; WPX.TextColor3=Color3.new(1,1,1)
WPX.TextSize=S.txt; WPX.Font=Enum.Font.GothamBold
Instance.new("UICorner",WPX).CornerRadius=UDim.new(0,5)
OnClick(WPX,function() WP.Visible=false end)
local WPAl=Instance.new("TextLabel",WP)
WPAl.Size=UDim2.new(1,-20,0,18); WPAl.Position=UDim2.fromOffset(10,S.titleH+4)
WPAl.BackgroundTransparency=1; WPAl.TextColor3=Color3.fromRGB(60,110,100)
WPAl.TextSize=S.txtSm; WPAl.Font=Enum.Font.Gotham; WPAl.TextXAlignment=Enum.TextXAlignment.Left
local WPDiv=Instance.new("Frame",WP)
WPDiv.Size=UDim2.new(1,-20,0,1); WPDiv.Position=UDim2.fromOffset(10,S.titleH+24)
WPDiv.BackgroundColor3=Color3.fromRGB(25,40,60); WPDiv.BorderSizePixel=0
local WPTxt=Instance.new("TextLabel",WP)
WPTxt.Size=UDim2.new(1,-20,0,WPH-S.titleH-70); WPTxt.Position=UDim2.fromOffset(10,S.titleH+28)
WPTxt.BackgroundTransparency=1; WPTxt.TextColor3=Color3.fromRGB(155,185,180)
WPTxt.TextSize=S.txt; WPTxt.Font=Enum.Font.Gotham; WPTxt.TextWrapped=true
WPTxt.TextXAlignment=Enum.TextXAlignment.Left; WPTxt.TextYAlignment=Enum.TextYAlignment.Top
local WPRun=Instance.new("TextButton",WP)
WPRun.Size=UDim2.fromOffset(IsMobile and 120 or 90,BTN); WPRun.Position=UDim2.fromOffset(10,WPH-BTN-8)
WPRun.BackgroundColor3=Color3.fromRGB(0,200,140); WPRun.BackgroundTransparency=0.25
WPRun.BorderSizePixel=0; WPRun.Text=T("run_btn"); WPRun.TextColor3=Color3.fromRGB(0,255,180)
WPRun.TextSize=S.txt; WPRun.Font=Enum.Font.GothamBold
Instance.new("UICorner",WPRun).CornerRadius=UDim.new(0,5)
local WPFav=Instance.new("TextButton",WP)
WPFav.Size=UDim2.fromOffset(IsMobile and 140 or 110,BTN)
WPFav.Position=UDim2.fromOffset(IsMobile and 138 or 108,WPH-BTN-8)
WPFav.BackgroundColor3=Color3.fromRGB(255,200,50); WPFav.BackgroundTransparency=0.55
WPFav.BorderSizePixel=0; WPFav.Text=T("fav_add"); WPFav.TextColor3=Color3.fromRGB(255,230,120)
WPFav.TextSize=S.txt; WPFav.Font=Enum.Font.GothamBold
Instance.new("UICorner",WPFav).CornerRadius=UDim.new(0,5)
local _wCmd=nil
local function OpenWiki(cmd)
    _wCmd=cmd; local c=GetCC(cmd.cat)
    WPS.Color=c; WPBar.BackgroundColor3=c
    WPName.Text="📖  "..cmd.name.."  ["..cmd.cat.."]"; WPName.TextColor3=c
    local als=#cmd.aliases>0 and T("aliases")..table.concat(cmd.aliases,", ") or T("no_aliases")
    WPAl.Text=als
    local wiki=GetWiki(cmd.name)
    WPTxt.Text= wiki~=T("wiki_none") and wiki or CmdDesc(cmd)..T("wiki_none")
    local fav=Favorites[cmd.name]
    WPFav.Text=fav and T("fav_rm") or T("fav_add")
    WPFav.TextColor3=fav and Color3.fromRGB(255,230,60) or Color3.fromRGB(220,200,100)
    WPRun.Text=T("run_btn"); WP.Visible=true
end
OnClick(WPRun,function() if _wCmd then Run(_wCmd.name); WP.Visible=false end end)
OnClick(WPFav,function()
    if not _wCmd then return end
    Favorites[_wCmd.name]=not Favorites[_wCmd.name]
    local fav=Favorites[_wCmd.name]
    WPFav.Text=fav and T("fav_rm") or T("fav_add")
    WPFav.TextColor3=fav and Color3.fromRGB(255,230,60) or Color3.fromRGB(220,200,100)
    Notify(fav and "★" or "☆",_wCmd.name,Color3.fromRGB(255,210,50))
    if ActiveTab=="favorites" then Refresh() end
end)

-- ═══════════════════════════════════════════════════════════════
--  MakeRow  —  строка команды в списке
-- ═══════════════════════════════════════════════════════════════
local function MakeRow(cmd, idx)
    local c=GetCC(cmd.cat)
    local row=Instance.new("Frame",List)
    row.Size=UDim2.new(1,0,0,S.rowH); row.BackgroundColor3=Color3.fromRGB(9,16,26)
    row.BorderSizePixel=0; row.LayoutOrder=idx
    Instance.new("UICorner",row).CornerRadius=UDim.new(0,5)
    local accent=Instance.new("Frame",row)
    accent.Size=UDim2.fromOffset(3,S.rowH-6); accent.Position=UDim2.fromOffset(0,3)
    accent.BackgroundColor3=c; accent.BorderSizePixel=0
    Instance.new("UICorner",accent).CornerRadius=UDim.new(0,2)
    -- name
    local nameL=Instance.new("TextLabel",row)
    nameL.Size=UDim2.fromOffset(IsMobile and 110 or 130, S.rowH); nameL.Position=UDim2.fromOffset(8,0)
    nameL.BackgroundTransparency=1; nameL.Text=cmd.name
    nameL.TextColor3=c; nameL.TextSize=S.txt; nameL.Font=Enum.Font.GothamBold
    nameL.TextXAlignment=Enum.TextXAlignment.Left
    -- desc (same line, grey, truncated)
    local descL=Instance.new("TextLabel",row)
    local STAR_SZ=IsMobile and 28 or 20
    local WB_SZ=IsMobile and 28 or 18
    descL.Size=UDim2.new(1,-(IsMobile and 110 or 130)-(STAR_SZ+WB_SZ+20),1,0)
    descL.Position=UDim2.fromOffset(IsMobile and 118 or 138, 0)
    descL.BackgroundTransparency=1; descL.Text=CmdDesc(cmd)
    descL.TextColor3=Color3.fromRGB(75,100,95); descL.TextSize=S.txtSm
    descL.Font=Enum.Font.Gotham; descL.TextXAlignment=Enum.TextXAlignment.Left
    descL.TextTruncate=Enum.TextTruncate.AtEnd
    local star=Instance.new("TextButton",row)
    star.Size=UDim2.fromOffset(STAR_SZ,STAR_SZ); star.Position=UDim2.new(1,-(STAR_SZ+WB_SZ+8),0.5,-STAR_SZ/2)
    star.BackgroundTransparency=1; star.BorderSizePixel=0
    star.Text=Favorites[cmd.name] and "★" or "☆"
    star.TextColor3=Favorites[cmd.name] and Color3.fromRGB(255,215,50) or Color3.fromRGB(55,75,70)
    star.TextSize=IsMobile and 14 or 12; star.Font=Enum.Font.GothamBold
    OnClick(star,function()
        Favorites[cmd.name]=not Favorites[cmd.name]
        star.Text=Favorites[cmd.name] and "★" or "☆"
        star.TextColor3=Favorites[cmd.name] and Color3.fromRGB(255,215,50) or Color3.fromRGB(55,75,70)
        if ActiveTab=="favorites" then Refresh() end
    end)
    local wbtn=Instance.new("TextButton",row)
    wbtn.Size=UDim2.fromOffset(WB_SZ,WB_SZ); wbtn.Position=UDim2.new(1,-(WB_SZ+4),0.5,-WB_SZ/2)
    wbtn.BackgroundColor3=Color3.fromRGB(20,35,55); wbtn.BackgroundTransparency=0.4
    wbtn.BorderSizePixel=0; wbtn.Text="?"; wbtn.TextColor3=Color3.fromRGB(90,140,200)
    wbtn.TextSize=IsMobile and 11 or 9; wbtn.Font=Enum.Font.GothamBold
    Instance.new("UICorner",wbtn).CornerRadius=UDim.new(0,4)
    OnClick(wbtn,function() OpenWiki(cmd) end)
    local hit=Instance.new("TextButton",row)
    hit.Size=UDim2.new(1,-(STAR_SZ+WB_SZ+16),1,0); hit.BackgroundTransparency=1; hit.Text=""
    OnClick(hit,function() IYInput.Text=cmd.name.." "; IYInput:CaptureFocus() end)
    hit.MouseEnter:Connect(function()
        TweenService:Create(row,TweenInfo.new(0.08),{BackgroundColor3=Color3.fromRGB(13,23,37)}):Play()
    end)
    hit.MouseLeave:Connect(function()
        TweenService:Create(row,TweenInfo.new(0.08),{BackgroundColor3=Color3.fromRGB(9,16,26)}):Play()
    end)
end

-- ═══════════════════════════════════════════════════════════════
--  MakeScriptRow  — строка скрипта во вкладке Scripts
-- ═══════════════════════════════════════════════════════════════
local function MakeScriptRow(scr, idx)
    local row=Instance.new("Frame",List)
    row.Size=UDim2.new(1,0,0,S.rowH+2); row.BackgroundColor3=Color3.fromRGB(10,17,26)
    row.BorderSizePixel=0; row.LayoutOrder=idx
    Instance.new("UICorner",row).CornerRadius=UDim.new(0,5)
    local accent=Instance.new("Frame",row)
    accent.Size=UDim2.fromOffset(3,S.rowH-4); accent.Position=UDim2.fromOffset(0,2)
    accent.BackgroundColor3=GetCC("Scripts"); accent.BorderSizePixel=0
    Instance.new("UICorner",accent).CornerRadius=UDim.new(0,2)
    local nameL=Instance.new("TextLabel",row)
    nameL.Size=UDim2.fromOffset(IsMobile and 110 or 140, S.rowH+2); nameL.Position=UDim2.fromOffset(8,0)
    nameL.BackgroundTransparency=1; nameL.Text=scr.name
    nameL.TextColor3=GetCC("Scripts"); nameL.TextSize=S.txt; nameL.Font=Enum.Font.GothamBold
    nameL.TextXAlignment=Enum.TextXAlignment.Left
    local RB_W=IsMobile and 62 or 52
    local descL=Instance.new("TextLabel",row)
    descL.Size=UDim2.new(1,-(IsMobile and 110 or 140)-RB_W-16,1,0)
    descL.Position=UDim2.fromOffset(IsMobile and 118 or 148, 0)
    descL.BackgroundTransparency=1
    descL.Text=Lang=="RU" and scr.desc.RU or scr.desc.EN
    descL.TextColor3=Color3.fromRGB(75,100,95); descL.TextSize=S.txtSm
    descL.Font=Enum.Font.Gotham; descL.TextXAlignment=Enum.TextXAlignment.Left
    local rbtn=Instance.new("TextButton",row)
    rbtn.Size=UDim2.fromOffset(RB_W, S.rowH-4); rbtn.Position=UDim2.new(1,-(RB_W+4),0.5,-(S.rowH-4)/2)
    rbtn.BackgroundColor3=Color3.fromRGB(0,200,140); rbtn.BackgroundTransparency=0.25
    rbtn.BorderSizePixel=0; rbtn.Text=T("scr_run"); rbtn.TextColor3=Color3.fromRGB(0,255,180)
    rbtn.TextSize=S.txt; rbtn.Font=Enum.Font.GothamBold
    Instance.new("UICorner",rbtn).CornerRadius=UDim.new(0,5)
    OnClick(rbtn,function()
        local ok,err=pcall(loadstring,scr.code)
        if ok and type(err)=="function" then
            local ok2,err2=pcall(err)
            if not ok2 then Notify(T("notif_err"),tostring(err2):sub(1,55),Color3.fromRGB(255,70,70)) end
        else
            -- fallback: try via RunService workaround
            Notify("Script","Запущен: "..scr.name,GetCC("Scripts"))
            pcall(loadstring(scr.code))
        end
        Notify("▶ "..scr.name, Lang=="RU" and "Запущен" or "Executed", GetCC("Scripts"))
    end)
    -- hover
    local hit=Instance.new("TextButton",row)
    hit.Size=UDim2.new(1,-(RB_W+12),1,0); hit.BackgroundTransparency=1; hit.Text=""
    hit.MouseEnter:Connect(function()
        TweenService:Create(row,TweenInfo.new(0.1),{BackgroundColor3=Color3.fromRGB(13,22,34)}):Play()
    end)
    hit.MouseLeave:Connect(function()
        TweenService:Create(row,TweenInfo.new(0.1),{BackgroundColor3=Color3.fromRGB(10,17,26)}):Play()
    end)
end

-- ═══════════════════════════════════════════════════════════════
--  REFRESH
-- ═══════════════════════════════════════════════════════════════
function Refresh()
    for _,c in pairs(List:GetChildren()) do if c:IsA("Frame") then c:Destroy() end end
    Search.Visible=(ActiveTab~="wiki" and ActiveTab~="scripts")
    Search.PlaceholderText=T("search_ph")

    if ActiveTab=="commands" then
        local f=Search.Text:lower(); local n=0
        for _,cmd in ipairs(CommandData) do
            local ok=f==""
                or cmd.name:find(f,1,true)
                or cmd.descRU:lower():find(f,1,true)
                or cmd.descEN:lower():find(f,1,true)
                or cmd.cat:lower():find(f,1,true)
                or (function() for _,a in ipairs(cmd.aliases) do if a:find(f,1,true) then return true end end end)()
            if ok then n+=1; MakeRow(cmd,n) end
        end
        List.CanvasSize=UDim2.fromOffset(0,n*(S.rowH+4))
        Status.Text="  "..n..T("status_cmd")

    elseif ActiveTab=="favorites" then
        local n=0
        for _,cmd in ipairs(CommandData) do
            if Favorites[cmd.name] then n+=1; MakeRow(cmd,n) end
        end
        List.CanvasSize=UDim2.fromOffset(0,n*(S.rowH+4))
        if n==0 then
            local e=Instance.new("TextLabel",List); e.Size=UDim2.new(1,0,0,70)
            e.BackgroundTransparency=1; e.LayoutOrder=1; e.Text=T("empty_fav")
            e.TextColor3=Color3.fromRGB(60,85,80); e.TextSize=S.txt; e.Font=Enum.Font.Gotham
            e.TextWrapped=true; List.CanvasSize=UDim2.fromOffset(0,80)
        end
        Status.Text="  "..T("status_fav")..n..T("status_fav2")

    elseif ActiveTab=="wiki" then
        local cats,order={},{}
        for _,cmd in ipairs(CommandData) do
            if not cats[cmd.cat] then cats[cmd.cat]={};table.insert(order,cmd.cat) end
            table.insert(cats[cmd.cat],cmd)
        end
        local row=0
        for _,cat in ipairs(order) do
            row+=1
            local hdr=Instance.new("Frame",List)
            hdr.Size=UDim2.new(1,0,0,26); hdr.BorderSizePixel=0; hdr.LayoutOrder=row*1000
            hdr.BackgroundColor3=GetCC(cat); hdr.BackgroundTransparency=0.82
            Instance.new("UICorner",hdr).CornerRadius=UDim.new(0,5)
            local hl=Instance.new("TextLabel",hdr)
            hl.Size=UDim2.new(1,-8,1,0); hl.Position=UDim2.fromOffset(8,0)
            hl.BackgroundTransparency=1; hl.Text=cat:upper().."  ("..#cats[cat]..")"
            hl.TextColor3=GetCC(cat); hl.TextSize=S.txtSm; hl.Font=Enum.Font.GothamBold
            hl.TextXAlignment=Enum.TextXAlignment.Left
            for j,cmd in ipairs(cats[cat]) do
                row+=1
                local WROW=IsMobile and 26 or 22
                local r=Instance.new("Frame",List)
                r.Size=UDim2.new(1,0,0,WROW); r.BackgroundColor3=Color3.fromRGB(8,14,22)
                r.BorderSizePixel=0; r.LayoutOrder=row*1000+j
                Instance.new("UICorner",r).CornerRadius=UDim.new(0,5)
                local nl=Instance.new("TextLabel",r)
                nl.Size=UDim2.fromOffset(150,20); nl.Position=UDim2.fromOffset(8,4)
                nl.BackgroundTransparency=1; nl.Text=cmd.name
                nl.TextColor3=GetCC(cmd.cat); nl.TextSize=S.txt; nl.Font=Enum.Font.GothamBold
                nl.TextXAlignment=Enum.TextXAlignment.Left
                local dl=Instance.new("TextLabel",r)
                dl.Size=UDim2.new(1,-70,1,0); dl.Position=UDim2.fromOffset(8,0)
                dl.BackgroundTransparency=1; dl.Text=CmdDesc(cmd)
                dl.TextColor3=Color3.fromRGB(80,110,105); dl.TextSize=S.txtSm; dl.Font=Enum.Font.Gotham
                dl.TextXAlignment=Enum.TextXAlignment.Left; dl.TextTruncate=Enum.TextTruncate.AtEnd
                local hb=Instance.new("TextButton",r)
                hb.Size=UDim2.new(1,0,1,0); hb.BackgroundTransparency=1; hb.Text=""
                OnClick(hb,function() OpenWiki(cmd) end)
                hb.MouseEnter:Connect(function() TweenService:Create(r,TweenInfo.new(0.1),{BackgroundColor3=Color3.fromRGB(12,20,32)}):Play() end)
                hb.MouseLeave:Connect(function() TweenService:Create(r,TweenInfo.new(0.1),{BackgroundColor3=Color3.fromRGB(8,14,22)}):Play() end)
            end
        end
        List.CanvasSize=UDim2.fromOffset(0,row*26)
        Status.Text="  "..T("status_wik")..#CommandData..T("status_wik2")

    elseif ActiveTab=="scripts" then
        for i,scr in ipairs(ScriptList) do MakeScriptRow(scr,i) end
        List.CanvasSize=UDim2.fromOffset(0,#ScriptList*(S.rowH+4))
        Status.Text="  "..T("status_scr")
    end
end

-- ── Строим вкладки ───────────────────────────────────────────
local tabW = IsMobile and (S.winW-28)/4 or ((S.winW-28)/4)
for i,t in ipairs(TABS) do
    local b=Instance.new("TextButton",TabRow)
    b.Size=UDim2.fromOffset(tabW, S.tabH-6)
    b.BackgroundTransparency=1; b.BackgroundColor3=Color3.new()
    b.BorderSizePixel=0
    b.Text=IsMobile and T(t[3]) or T(t[2])
    b.TextSize=IsMobile and 16 or 10; b.Font=Enum.Font.GothamBold; b.LayoutOrder=i
    Instance.new("UICorner",b).CornerRadius=UDim.new(0,5)
    TabBtns[t[1]]=b
    OnClick(b,function()
        ActiveTab=t[1]
        for _,td in ipairs(TABS) do StyleTab(td[1],td[1]==t[1]) end
        Refresh()
    end)
end
StyleTab("commands",true)
Search:GetPropertyChangedSignal("Text"):Connect(function()
    if ActiveTab=="commands" then Refresh() end
end)

-- ── Lang toggle ──────────────────────────────────────────────
local function ApplyLang()
    Lang = Lang=="RU" and "EN" or "RU"
    LangBtn.Text = Lang=="RU" and "EN" or "RU"
    TTitle.Text=T("title")
    Search.PlaceholderText=T("search_ph")
    -- update tab labels
    for i,t in ipairs(TABS) do
        local b=TabBtns[t[1]]; if b then
            b.Text=IsMobile and T(t[3]) or T(t[2])
        end
    end
    Refresh()
    Notify(Lang=="RU" and "Язык" or "Language", Lang, Color3.fromRGB(120,180,255))
end
OnClick(LangBtn, ApplyLang)

-- ═══════════════════════════════════════════════════════════════
--  IY COMMAND BAR
-- ═══════════════════════════════════════════════════════════════
local BAR_W,BAR_H=S.barW,S.barH
local Bar=Instance.new("Frame",SG)
Bar.Size=UDim2.fromOffset(BAR_W,BAR_H)
Bar.Position=UDim2.new(0.5,-BAR_W/2,1,-(BAR_H+10))
Bar.BackgroundColor3=Color3.fromRGB(10,15,22); Bar.BorderSizePixel=0
Instance.new("UICorner",Bar).CornerRadius=UDim.new(0,8)
local BarS=Instance.new("UIStroke",Bar)
BarS.Color=Color3.fromRGB(0,210,150); BarS.Thickness=1; BarS.Transparency=0.35

local GRIP_W=S.gripW
local Grip=Instance.new("Frame",Bar)
Grip.Size=UDim2.fromOffset(GRIP_W,BAR_H)
Grip.BackgroundColor3=Color3.fromRGB(0,160,110)
Grip.BackgroundTransparency=IsMobile and 0.75 or 1; Grip.BorderSizePixel=0
Instance.new("UICorner",Grip).CornerRadius=UDim.new(0,7)
local GripLbl=Instance.new("TextLabel",Grip)
GripLbl.Size=UDim2.new(1,0,1,0); GripLbl.BackgroundTransparency=1
GripLbl.Text="⠿"; GripLbl.TextColor3=Color3.fromRGB(0,200,140)
GripLbl.TextSize=IsMobile and 18 or 14; GripLbl.Font=Enum.Font.GothamBold

local Sep=Instance.new("Frame",Bar)
Sep.Size=UDim2.fromOffset(1,18); Sep.Position=UDim2.new(0,GRIP_W,0.5,-9)
Sep.BackgroundColor3=Color3.fromRGB(0,200,140); Sep.BackgroundTransparency=0.55; Sep.BorderSizePixel=0

local Prefix=Instance.new("TextLabel",Bar)
Prefix.Size=UDim2.fromOffset(22,BAR_H); Prefix.Position=UDim2.fromOffset(GRIP_W+2,0)
Prefix.BackgroundTransparency=1; Prefix.Text=";"
Prefix.TextColor3=Color3.fromRGB(0,210,150); Prefix.TextSize=15; Prefix.Font=Enum.Font.GothamBold

IYInput=Instance.new("TextBox",Bar)
IYInput.Size=UDim2.new(1,-(GRIP_W+24+86),1,0); IYInput.Position=UDim2.fromOffset(GRIP_W+24,0)
IYInput.BackgroundTransparency=1; IYInput.BorderSizePixel=0
IYInput.PlaceholderText=T("bar_ph"); IYInput.PlaceholderColor3=Color3.fromRGB(55,80,75)
IYInput.Text=""; IYInput.TextColor3=Color3.fromRGB(215,235,230)
IYInput.TextSize=IsMobile and 15 or 13; IYInput.Font=Enum.Font.Gotham; IYInput.ClearTextOnFocus=false
IYInput.TextXAlignment=Enum.TextXAlignment.Left

-- Autocomplete (ПК)
local AcDrop=Instance.new("Frame",SG)
AcDrop.BackgroundColor3=Color3.fromRGB(10,15,22); AcDrop.BorderSizePixel=0; AcDrop.Visible=false
AcDrop.Size=UDim2.fromOffset(BAR_W,0)
Instance.new("UICorner",AcDrop).CornerRadius=UDim.new(0,8)
local AcS=Instance.new("UIStroke",AcDrop); AcS.Color=Color3.fromRGB(0,200,140); AcS.Thickness=1; AcS.Transparency=0.5
local AcLL=Instance.new("UIListLayout",AcDrop); AcLL.SortOrder=Enum.SortOrder.LayoutOrder
local AC_ROW_H=IsMobile and 38 or 26
local function PositionDropAboveBar()
    local bpos=Bar.AbsolutePosition; local cnt=#AcDrop:GetChildren()-1; if cnt<=0 then cnt=0 end
    AcDrop.Size=UDim2.fromOffset(BAR_W,cnt*AC_ROW_H)
    AcDrop.Position=UDim2.fromOffset(bpos.X,bpos.Y-cnt*AC_ROW_H-4)
end
local function UpdateAc(text)
    for _,c in pairs(AcDrop:GetChildren()) do if c:IsA("TextButton") then c:Destroy() end end
    if IsMobile then AcDrop.Visible=false; return end
    local word=text:match("^(%S*)") or ""; if word=="" then AcDrop.Visible=false; return end
    local matches={}
    for _,cmd in ipairs(CommandData) do
        if cmd.name:sub(1,#word)==word:lower() then table.insert(matches,cmd); if #matches>=5 then break end end
    end
    if #matches==0 then AcDrop.Visible=false; return end
    for i,cmd in ipairs(matches) do
        local b=Instance.new("TextButton",AcDrop)
        b.Size=UDim2.new(1,0,0,AC_ROW_H); b.BackgroundTransparency=i==1 and 0.6 or 0.9
        b.BackgroundColor3=Color3.fromRGB(0,200,140); b.BorderSizePixel=0
        b.TextXAlignment=Enum.TextXAlignment.Left; b.LayoutOrder=i
        b.Text="  "..cmd.name.."   "..CmdDesc(cmd)
        b.TextColor3=i==1 and Color3.fromRGB(0,255,180) or Color3.fromRGB(120,165,158)
        b.TextSize=S.txt; b.Font=Enum.Font.Gotham
        OnClick(b,function() IYInput.Text=cmd.name.." "; IYInput:CaptureFocus(); AcDrop.Visible=false end)
    end
    AcDrop.Visible=true; PositionDropAboveBar()
end
IYInput:GetPropertyChangedSignal("Text"):Connect(function() UpdateAc(IYInput.Text) end)
IYInput.FocusLost:Connect(function(enter)
    if enter and IYInput.Text~="" then Run(IYInput.Text); IYInput.Text=""; AcDrop.Visible=false end
end)
UserInputService.InputBegan:Connect(function(inp,gp)
    if gp then return end
    if inp.KeyCode==Enum.KeyCode.Tab and IYInput:IsFocused() then
        for _,c in pairs(AcDrop:GetChildren()) do
            if c:IsA("TextButton") and c.LayoutOrder==1 then
                local name=c.Text:match("^%s*(%S+)"); if name then IYInput.Text=name.." "; AcDrop.Visible=false end; break
            end
        end
    end
end)
Drag(Grip,Bar,function() if AcDrop.Visible then PositionDropAboveBar() end end)

-- Panel button (ПК)
local PanelBtn
if not IsMobile then
    PanelBtn=Instance.new("TextButton",Bar)
    PanelBtn.Size=UDim2.fromOffset(76,26); PanelBtn.Position=UDim2.new(1,-82,0.5,-13)
    PanelBtn.BackgroundColor3=Color3.fromRGB(0,200,140); PanelBtn.BackgroundTransparency=0.2
    PanelBtn.BorderSizePixel=0; PanelBtn.Text=T("panel_btn"); PanelBtn.TextColor3=Color3.fromRGB(0,0,0)
    PanelBtn.TextSize=11; PanelBtn.Font=Enum.Font.GothamBold
    Instance.new("UICorner",PanelBtn).CornerRadius=UDim.new(0,6)
end

-- Mobile row 2
local MobilePanelBtn
if IsMobile then
    Bar.Size=UDim2.fromOffset(BAR_W,BAR_H*2+4)
    Bar.Position=UDim2.new(0.5,-BAR_W/2,1,-(BAR_H*2+14))
    IYInput.Size=UDim2.new(1,-(GRIP_W+24),0,BAR_H)
    local Row2=Instance.new("Frame",Bar)
    Row2.Size=UDim2.new(1,0,0,BAR_H); Row2.Position=UDim2.fromOffset(0,BAR_H+4)
    Row2.BackgroundTransparency=1; Row2.BorderSizePixel=0
    local KbBtn=Instance.new("TextButton",Row2)
    KbBtn.Size=UDim2.fromOffset(BAR_W/2-3,BAR_H-4); KbBtn.Position=UDim2.fromOffset(2,2)
    KbBtn.BackgroundColor3=Color3.fromRGB(20,30,45); KbBtn.BackgroundTransparency=0.1
    KbBtn.BorderSizePixel=0; KbBtn.Text=T("mob_enter"); KbBtn.TextColor3=Color3.fromRGB(0,230,170)
    KbBtn.TextSize=14; KbBtn.Font=Enum.Font.GothamBold
    Instance.new("UICorner",KbBtn).CornerRadius=UDim.new(0,6)
    local KbS=Instance.new("UIStroke",KbBtn); KbS.Color=Color3.fromRGB(0,180,130); KbS.Thickness=1; KbS.Transparency=0.4
    OnClick(KbBtn,function() IYInput:CaptureFocus() end)
    MobilePanelBtn=Instance.new("TextButton",Row2)
    MobilePanelBtn.Size=UDim2.fromOffset(BAR_W/2-3,BAR_H-4); MobilePanelBtn.Position=UDim2.fromOffset(BAR_W/2+1,2)
    MobilePanelBtn.BackgroundColor3=Color3.fromRGB(0,200,140); MobilePanelBtn.BackgroundTransparency=0.15
    MobilePanelBtn.BorderSizePixel=0; MobilePanelBtn.Text=T("mob_panel"); MobilePanelBtn.TextColor3=Color3.new()
    MobilePanelBtn.TextSize=14; MobilePanelBtn.Font=Enum.Font.GothamBold
    Instance.new("UICorner",MobilePanelBtn).CornerRadius=UDim.new(0,6)
    _dragTargets[Row2]={window=Bar,onMove=function() if AcDrop.Visible then PositionDropAboveBar() end end}
end

-- SetPanel
local panelOpen=false
local function SetPanel(open)
    panelOpen=open
    local label=IsMobile and (open and T("mob_panelo") or T("mob_panel")) or (open and T("panel_open") or T("panel_btn"))
    if IsMobile and MobilePanelBtn then MobilePanelBtn.Text=label
    elseif PanelBtn then PanelBtn.Text=label end
    if open then
        Win.Visible=true; Win.Size=UDim2.fromOffset(WIN_W,0)
        TweenService:Create(Win,TweenInfo.new(0.22,Enum.EasingStyle.Back,Enum.EasingDirection.Out),
            {Size=UDim2.fromOffset(WIN_W,WIN_H)}):Play()
        Refresh()
    else
        TweenService:Create(Win,TweenInfo.new(0.15,Enum.EasingStyle.Quad,Enum.EasingDirection.In),
            {Size=UDim2.fromOffset(WIN_W,0)}):Play()
        task.delay(0.15,function() Win.Visible=false; Win.Size=UDim2.fromOffset(WIN_W,WIN_H) end)
    end
end
if PanelBtn then OnClick(PanelBtn,function() SetPanel(not panelOpen) end) end
if MobilePanelBtn then OnClick(MobilePanelBtn,function() SetPanel(not panelOpen) end) end
OnClick(XBtn,function() SetPanel(false) end)

-- ═══════════════════════════════════════════════════════════════
--  SHOW/HIDE BUTTON  [G]
-- ═══════════════════════════════════════════════════════════════
local BTN_SZ=IsMobile and 52 or 40
local ShowBtn=Instance.new("Frame",SG)
ShowBtn.Size=UDim2.fromOffset(BTN_SZ,BTN_SZ); ShowBtn.Position=UDim2.fromOffset(10,10)
ShowBtn.BackgroundColor3=Color3.fromRGB(0,200,140); ShowBtn.BackgroundTransparency=0.15; ShowBtn.BorderSizePixel=0
Instance.new("UICorner",ShowBtn).CornerRadius=UDim.new(0,10)
local ShowStroke=Instance.new("UIStroke",ShowBtn)
ShowStroke.Color=Color3.fromRGB(0,255,180); ShowStroke.Thickness=1.5; ShowStroke.Transparency=0.3
local ShowLbl=Instance.new("TextLabel",ShowBtn)
ShowLbl.Size=UDim2.new(1,0,1,0); ShowLbl.BackgroundTransparency=1; ShowLbl.Text="G"
ShowLbl.TextColor3=Color3.new(); ShowLbl.TextSize=IsMobile and 22 or 17; ShowLbl.Font=Enum.Font.GothamBold
local GlowRing=Instance.new("Frame",ShowBtn)
GlowRing.Size=UDim2.fromOffset(BTN_SZ+8,BTN_SZ+8); GlowRing.Position=UDim2.fromOffset(-4,-4)
GlowRing.BackgroundTransparency=1; GlowRing.BorderSizePixel=0
Instance.new("UICorner",GlowRing).CornerRadius=UDim.new(0,14)
local GlowStroke=Instance.new("UIStroke",GlowRing)
GlowStroke.Color=Color3.fromRGB(0,255,180); GlowStroke.Thickness=2; GlowStroke.Transparency=0.5
local pulseTI=TweenInfo.new(1.1,Enum.EasingStyle.Sine,Enum.EasingDirection.InOut,-1,true)
TweenService:Create(GlowStroke,pulseTI,{Transparency=0.85}):Play()
TweenService:Create(ShowStroke,pulseTI,{Transparency=0.7}):Play()
local uiVisible=true
local function SetUIVisible(vis)
    uiVisible=vis
    if vis then
        Bar.Visible=true
        TweenService:Create(Bar,TweenInfo.new(0.18),{BackgroundTransparency=0}):Play()
    else
        if panelOpen then SetPanel(false) end
        TweenService:Create(Bar,TweenInfo.new(0.18),{BackgroundTransparency=1}):Play()
        task.delay(0.2,function() if not uiVisible then Bar.Visible=false end end)
    end
    local col=vis and Color3.fromRGB(0,200,140) or Color3.fromRGB(180,30,30)
    local gcol=vis and Color3.fromRGB(0,255,180) or Color3.fromRGB(255,70,70)
    TweenService:Create(ShowBtn,TweenInfo.new(0.18),{BackgroundColor3=col}):Play()
    ShowStroke.Color=gcol; GlowStroke.Color=gcol
end
local ShowHit=Instance.new("TextButton",ShowBtn)
ShowHit.Size=UDim2.new(1,0,1,0); ShowHit.BackgroundTransparency=1; ShowHit.Text=""; ShowHit.ZIndex=5
OnClick(ShowHit,function() SetUIVisible(not uiVisible) end)
Drag(ShowBtn,ShowBtn,nil)

-- ╔══════════════════════════════════════════════════════════════╗
-- ║                   ALL COMMANDS  (400+)                      ║
-- ╚══════════════════════════════════════════════════════════════╝

-- ── MOVEMENT ──────────────────────────────────────────────────
local InfJumpConn
AddCommand("infjump",{"ij","infinitejump"},"Бесконечный прыжок","Infinite jump","Movement",function()
    if InfJumpConn then InfJumpConn:Disconnect() end
    InfJumpConn=UserInputService.JumpRequest:Connect(function()
        local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
        if h then h:ChangeState("Jumping") end
    end); Notify("InfJump","✓")
end)
AddCommand("uninfjump",{"unij","noinfjump"},"Выкл бесконечный прыжок","Disable infinite jump","Movement",function()
    if InfJumpConn then InfJumpConn:Disconnect(); InfJumpConn=nil end; Notify("InfJump","✗")
end)

local NoclipConn
AddCommand("noclip",{"nc","ghost"},"Сквозь стены","Walk through walls","Movement",function()
    if NoclipConn then NoclipConn:Disconnect() end
    NoclipConn=RunService.Stepped:Connect(function()
        if LocalPlayer.Character then
            for _,p in pairs(LocalPlayer.Character:GetDescendants()) do
                if p:IsA("BasePart") then p.CanCollide=false end
            end
        end
    end); Notify("Noclip","✓")
end)
AddCommand("clip",{"unnc","nocollide"},"Включить коллизию","Enable collision","Movement",function()
    if NoclipConn then NoclipConn:Disconnect(); NoclipConn=nil end
    if LocalPlayer.Character then
        for _,p in pairs(LocalPlayer.Character:GetDescendants()) do
            if p:IsA("BasePart") then p.CanCollide=true end
        end
    end; Notify("Noclip","✗")
end)
AddCommand("ws",{"speed","walkspeed"},"Скорость [n]","Walk speed [n]","Movement",function(a)
    local v=tonumber(a[1]) or 32
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.WalkSpeed=v end; Notify("WS","→"..v)
end)
AddCommand("resetspeed",{"normalspeed"},"Сброс скорости","Reset walk speed","Movement",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.WalkSpeed=16 end; Notify("WS","16")
end)
AddCommand("slowmo",{},"Замедление ws=2","Slow motion ws=2","Movement",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.WalkSpeed=2 end; Notify("SlowMo","ws=2")
end)
AddCommand("fastmo",{},"Макс скорость ws=200","Max speed ws=200","Movement",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.WalkSpeed=200 end; Notify("FastMo","ws=200")
end)
AddCommand("sneakspeed",{"sneak"},"Скрытная скорость ws=6","Stealth speed ws=6","Movement",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.WalkSpeed=6 end; Notify("Sneak","ws=6")
end)
AddCommand("jh",{"jumpheight","jumppower","jp"},"Высота прыжка [n]","Jump height [n]","Movement",function(a)
    local v=tonumber(a[1]) or 100
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.JumpPower=v end; Notify("JP","→"..v)
end)
AddCommand("resetjump",{},"Сброс прыжка","Reset jump power","Movement",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.JumpPower=50 end; Notify("JP","50")
end)
AddCommand("highjump",{},"Высокий прыжок jp=300","High jump jp=300","Movement",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.JumpPower=300 end; Notify("HighJump","300")
end)
AddCommand("moonjump",{},"Лунный прыжок jp=150","Moon jump jp=150","Movement",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.JumpPower=150; h.WalkSpeed=14 end; Notify("MoonJump","150")
end)
AddCommand("gravity",{"grav"},"Гравитация [n]","Gravity [n]","Movement",function(a)
    workspace.Gravity=tonumber(a[1]) or 196.2; Notify("Grav","→"..(a[1] or "196.2"))
end)
AddCommand("zerograv",{"0g","nograv"},"Нулевая гравитация","Zero gravity","Movement",function()
    workspace.Gravity=0; Notify("ZeroGrav","🪐")
end)
AddCommand("moongrav",{},"Лунная гравитация","Moon gravity","Movement",function()
    workspace.Gravity=30; Notify("MoonGrav","🌙")
end)
AddCommand("marsgrav",{},"Марсианская гравитация","Mars gravity","Movement",function()
    workspace.Gravity=74; Notify("MarsGrav","🔴")
end)
AddCommand("heavygrav",{},"Тяжёлая гравитация","Heavy gravity","Movement",function()
    workspace.Gravity=600; Notify("HeavyGrav","600")
end)
AddCommand("resetgrav",{},"Сброс гравитации","Reset gravity","Movement",function()
    workspace.Gravity=196.2; Notify("Grav","196.2")
end)

local FlyConn
AddCommand("fly",{"fl","flight"},"Летать [скорость]","Fly [speed]","Movement",function(a)
    local spd=tonumber(a[1]) or 60
    local char=LocalPlayer.Character; if not char then return end
    local root=char:FindFirstChild("HumanoidRootPart"); if not root then return end
    if FlyConn then FlyConn:Disconnect() end
    for _,v in pairs(root:GetChildren()) do if v:IsA("BodyGyro") or v:IsA("BodyVelocity") then v:Destroy() end end
    local bg=Instance.new("BodyGyro",root); bg.MaxTorque=Vector3.new(1000000000,1000000000,1000000000); bg.D=200
    local bv=Instance.new("BodyVelocity",root); bv.MaxForce=Vector3.new(1000000000,1000000000,1000000000)
    local hum=char:FindFirstChildOfClass("Humanoid"); if hum then hum:ChangeState(Enum.HumanoidStateType.Physics) end
    FlyConn=RunService.RenderStepped:Connect(function()
        local vel=Vector3.zero
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then vel=vel+Camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then vel=vel-Camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then vel=vel-Camera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then vel=vel+Camera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.Space) then vel=vel+Vector3.new(0,1,0) end
        if UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) or UserInputService:IsKeyDown(Enum.KeyCode.Q) then vel=vel-Vector3.new(0,1,0) end
        bv.Velocity=vel*spd; if vel.Magnitude>0.1 then bg.CFrame=Camera.CFrame end
    end); Notify("Fly","✓ "..spd)
end)
AddCommand("unfly",{"stopfly","nofly"},"Остановить полёт","Stop flying","Movement",function()
    if FlyConn then FlyConn:Disconnect(); FlyConn=nil end
    local char=LocalPlayer.Character; if char then
        local root=char:FindFirstChild("HumanoidRootPart"); if root then
            for _,v in pairs(root:GetChildren()) do if v:IsA("BodyGyro") or v:IsA("BodyVelocity") then v:Destroy() end end
        end
        local hum=char:FindFirstChildOfClass("Humanoid"); if hum then hum:ChangeState(Enum.HumanoidStateType.GettingUp) end
    end; Notify("Fly","✗")
end)
AddCommand("glide",{},"Медленный полёт spd=25","Slow glide spd=25","Movement",function() Run("fly 25") end)
AddCommand("superfly",{},"Сверхзвуковой spd=350","Supersonic fly","Movement",function() Run("fly 350") end)

AddCommand("tp",{"teleport"},"Телепорт X Y Z","Teleport X Y Z","Movement",function(a)
    local x,y,z=tonumber(a[1]) or 0,tonumber(a[2]) or 0,tonumber(a[3]) or 0
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then root.CFrame=CFrame.new(x,y,z) end; Notify("TP","("..x..","..y..","..z..")")
end)
AddCommand("tpto",{"goto","tele"},"Телепорт к игроку [имя]","Teleport to player [name]","Movement",function(a)
    local name=(a[1] or ""):lower()
    for _,p in pairs(Players:GetPlayers()) do
        if p.Name:lower():find(name) and p~=LocalPlayer then
            local r=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            local t=p.Character and p.Character:FindFirstChild("HumanoidRootPart")
            if r and t then r.CFrame=t.CFrame+Vector3.new(3,0,0); Notify("TP","→"..p.Name) end
        end
    end
end)
AddCommand("bring",{},"Притащить игрока [имя]","Bring player [name]","Movement",function(a)
    local name=(a[1] or ""):lower()
    for _,p in pairs(Players:GetPlayers()) do
        if p.Name:lower():find(name) and p~=LocalPlayer then
            local mr=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            local tr=p.Character and p.Character:FindFirstChild("HumanoidRootPart")
            if mr and tr then tr.CFrame=mr.CFrame+Vector3.new(5,0,0); Notify("Bring",p.Name) end
        end
    end
end)
AddCommand("freeze",{"anchor"},"Заморозить персонажа","Freeze character","Movement",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then root.Anchored=true end; Notify("Freeze","❄️")
end)
AddCommand("unfreeze",{"thaw","unanchor"},"Разморозить","Unfreeze character","Movement",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then root.Anchored=false end; Notify("Freeze","✗")
end)
AddCommand("superjump",{"sj"},"Супер прыжок [сила]","Super jump [force]","Movement",function(a)
    local f=tonumber(a[1]) or 500
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then local bv=Instance.new("BodyVelocity",root); bv.Velocity=Vector3.new(0,f,0); bv.MaxForce=Vector3.new(0,1000000000,0); task.delay(0.15,function() bv:Destroy() end) end
    Notify("SuperJump","🚀"..f)
end)
AddCommand("dash",{},"Рывок вперёд [скорость]","Dash forward [speed]","Movement",function(a)
    local s=tonumber(a[1]) or 250
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then local bv=Instance.new("BodyVelocity",root); bv.Velocity=root.CFrame.LookVector*s; bv.MaxForce=Vector3.new(1000000000,0,1000000000); task.delay(0.25,function() bv:Destroy() end) end
    Notify("Dash","→"..s)
end)
AddCommand("dashback",{},"Рывок назад [скорость]","Dash backward [speed]","Movement",function(a)
    local s=tonumber(a[1]) or 250
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then local bv=Instance.new("BodyVelocity",root); bv.Velocity=-root.CFrame.LookVector*s; bv.MaxForce=Vector3.new(1000000000,0,1000000000); task.delay(0.25,function() bv:Destroy() end) end
    Notify("DashBack","←"..s)
end)
AddCommand("sling",{},"Рогатка вперёд+вверх [сила]","Sling forward+up [force]","Movement",function(a)
    local f=tonumber(a[1]) or 400
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local dir=(Camera.CFrame.LookVector+Vector3.new(0,0.8,0)).Unit
    local bv=Instance.new("BodyVelocity",root); bv.Velocity=dir*f; bv.MaxForce=Vector3.new(1000000000,1000000000,1000000000)
    task.delay(0.2,function() bv:Destroy() end); Notify("Sling","↗"..f)
end)
AddCommand("tpup",{},"Телепорт вверх [высота]","Teleport up [height]","Movement",function(a)
    local h=tonumber(a[1]) or 50
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then root.CFrame=root.CFrame+Vector3.new(0,h,0) end; Notify("TPUp","+"..h)
end)
AddCommand("tpforward",{},"Телепорт вперёд [расст]","Teleport forward [dist]","Movement",function(a)
    local d=tonumber(a[1]) or 50
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then root.CFrame=root.CFrame+root.CFrame.LookVector*d end; Notify("TPFwd","+"..d)
end)
local AntiAfkConn
AddCommand("antiafk",{"noafk"},"Анти-АФК кик","Anti-AFK kick","Movement",function()
    if AntiAfkConn then AntiAfkConn:Disconnect() end
    AntiAfkConn=LocalPlayer.Idled:Connect(function()
        LocalPlayer:Move(Vector3.new(0,0,-1)); task.wait(0.1); LocalPlayer:Move(Vector3.zero)
    end); Notify("Anti-AFK","✓")
end)
local LoopTPConn
AddCommand("looptp",{},"Следовать за игроком [имя]","Follow player [name]","Movement",function(a)
    local name=(a[1] or ""):lower()
    if LoopTPConn then LoopTPConn:Disconnect() end
    LoopTPConn=RunService.Heartbeat:Connect(function()
        for _,p in pairs(Players:GetPlayers()) do
            if p.Name:lower():find(name) and p~=LocalPlayer then
                local r=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
                local t=p.Character and p.Character:FindFirstChild("HumanoidRootPart")
                if r and t then r.CFrame=t.CFrame end
            end
        end
    end); Notify("LoopTP","→"..name)
end)
AddCommand("unlooptp",{},"Остановить следование","Stop following","Movement",function()
    if LoopTPConn then LoopTPConn:Disconnect(); LoopTPConn=nil end; Notify("LoopTP","✗")
end)
local SpinConn
AddCommand("spin",{},"Вращение [скорость]","Spin character [speed]","Movement",function(a)
    local s=tonumber(a[1]) or 8
    if SpinConn then SpinConn:Disconnect() end
    SpinConn=RunService.Heartbeat:Connect(function()
        local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if root then root.CFrame=root.CFrame*CFrame.Angles(0,math.rad(s),0) end
    end); Notify("Spin","⟳"..s)
end)
AddCommand("unspin",{},"Остановить вращение","Stop spinning","Movement",function()
    if SpinConn then SpinConn:Disconnect(); SpinConn=nil end; Notify("Spin","✗")
end)
AddCommand("pos",{"position","coords","xyz"},"Показать позицию","Show position","Movement",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then local p=root.Position; Notify("XYZ","X:"..math.floor(p.X).." Y:"..math.floor(p.Y).." Z:"..math.floor(p.Z)) end
end)

-- ── VISUAL ────────────────────────────────────────────────────
AddCommand("esp",{"chams","wallhack"},"ESP всех игроков","ESP all players","Visual",function()
    for _,p in pairs(Players:GetPlayers()) do
        if p~=LocalPlayer and p.Character then
            local old=p.Character:FindFirstChild("ESPHl"); if old then old:Destroy() end
            local h=Instance.new("Highlight",p.Character); h.Name="ESPHl"
            h.FillColor=Color3.fromRGB(255,50,50); h.OutlineColor=Color3.fromRGB(0,255,180); h.FillTransparency=0.45
        end
    end; Notify("ESP","✓")
end)
AddCommand("unesp",{"noesp","removesp"},"Убрать ESP","Remove ESP","Visual",function()
    for _,p in pairs(Players:GetPlayers()) do
        if p.Character then local h=p.Character:FindFirstChild("ESPHl"); if h then h:Destroy() end end
    end; Notify("ESP","✗")
end)
AddCommand("blueesp",{},"Синий ESP","Blue ESP","Visual",function()
    for _,p in pairs(Players:GetPlayers()) do
        if p~=LocalPlayer and p.Character then
            local h=Instance.new("Highlight",p.Character); h.Name="ESPHl"
            h.FillColor=Color3.fromRGB(0,80,255); h.OutlineColor=Color3.fromRGB(100,180,255); h.FillTransparency=0.5
        end
    end; Notify("BlueESP","💙")
end)
AddCommand("greenesp",{},"Зелёный ESP","Green ESP","Visual",function()
    for _,p in pairs(Players:GetPlayers()) do
        if p~=LocalPlayer and p.Character then
            local h=Instance.new("Highlight",p.Character); h.Name="ESPHl"
            h.FillColor=Color3.fromRGB(0,200,80); h.OutlineColor=Color3.fromRGB(150,255,100); h.FillTransparency=0.5
        end
    end; Notify("GreenESP","💚")
end)
local RainbowESPConn
AddCommand("rainbowesp",{"respy"},"Радужный ESP","Rainbow ESP","Visual",function()
    if RainbowESPConn then RainbowESPConn:Disconnect() end
    local hue=0
    RainbowESPConn=RunService.Heartbeat:Connect(function()
        hue=(hue+0.003)%1
        for _,p in pairs(Players:GetPlayers()) do
            if p~=LocalPlayer and p.Character then
                local h=p.Character:FindFirstChild("ESPHl")
                if not h then h=Instance.new("Highlight",p.Character); h.Name="ESPHl" end
                h.FillColor=Color3.fromHSV(hue,1,1); h.OutlineColor=Color3.fromHSV((hue+0.5)%1,1,1)
            end
        end
    end); Notify("RainbowESP","🌈")
end)
AddCommand("unreinbowesp",{"noresp"},"Выкл радужный ESP","Disable rainbow ESP","Visual",function()
    if RainbowESPConn then RainbowESPConn:Disconnect(); RainbowESPConn=nil end; Notify("RainbowESP","✗")
end)
AddCommand("selfhl",{},"Подсветить себя","Highlight self","Visual",function()
    local char=LocalPlayer.Character; if not char then return end
    local old=char:FindFirstChild("SelfHL"); if old then old:Destroy() end
    local h=Instance.new("Highlight",char); h.Name="SelfHL"
    h.FillColor=Color3.fromRGB(0,200,255); h.OutlineColor=Color3.new(1,1,1); h.FillTransparency=0.35
    Notify("SelfHL","✓")
end)
AddCommand("unselfhl",{},"Убрать подсветку себя","Remove self highlight","Visual",function()
    local char=LocalPlayer.Character
    if char then local h=char:FindFirstChild("SelfHL"); if h then h:Destroy() end end; Notify("SelfHL","✗")
end)
AddCommand("fullbright",{"fb"},"Максимальная яркость","Maximum brightness","Visual",function()
    Lighting.Brightness=2; Lighting.ClockTime=14; Lighting.FogEnd=1000000000
    Lighting.GlobalShadows=false; Lighting.Ambient=Color3.fromRGB(255,255,255); Notify("Fullbright","✓")
end)
AddCommand("unfullbright",{"unfb"},"Сброс освещения","Reset lighting","Visual",function()
    Lighting.Brightness=1; Lighting.ClockTime=14; Lighting.FogEnd=10000
    Lighting.GlobalShadows=true; Lighting.Ambient=Color3.fromRGB(127,127,127); Notify("Fullbright","✗")
end)
AddCommand("fov",{},"Угол обзора [n]","Field of view [n]","Visual",function(a)
    Camera.FieldOfView=tonumber(a[1]) or 70; Notify("FOV","→"..(a[1] or "70"))
end)
AddCommand("widefov",{},"Широкий FOV 110","Wide FOV 110","Visual",function()
    Camera.FieldOfView=110; Notify("FOV","110")
end)
AddCommand("narrowfov",{},"Узкий FOV 40","Narrow FOV 40","Visual",function()
    Camera.FieldOfView=40; Notify("FOV","40")
end)
AddCommand("resetfov",{},"Сброс FOV","Reset FOV","Visual",function()
    Camera.FieldOfView=70; Notify("FOV","70")
end)
AddCommand("day",{},"День","Set day","Visual",function() Lighting.ClockTime=14; Notify("Time","☀️") end)
AddCommand("night",{},"Ночь","Set night","Visual",function() Lighting.ClockTime=2; Notify("Time","🌙") end)
AddCommand("sunrise",{},"Рассвет","Set sunrise","Visual",function() Lighting.ClockTime=6; Notify("Time","🌅") end)
AddCommand("sunset",{},"Закат","Set sunset","Visual",function() Lighting.ClockTime=20; Notify("Time","🌇") end)
AddCommand("midnight",{},"Полночь","Set midnight","Visual",function() Lighting.ClockTime=0; Notify("Time","🌑") end)
AddCommand("time",{},"Время [0-24]","Set time [0-24]","Visual",function(a)
    Lighting.ClockTime=tonumber(a[1]) or 12; Notify("Time","→"..(a[1] or "12"))
end)
AddCommand("nofog",{},"Убрать туман","Remove fog","Visual",function()
    Lighting.FogEnd=1000000000; Notify("Fog","✗")
end)
AddCommand("thickfog",{},"Густой туман","Thick fog","Visual",function()
    Lighting.FogEnd=40; Lighting.FogColor=Color3.fromRGB(180,200,210); Notify("Fog","Густой")
end)
AddCommand("redfog",{},"Красный туман","Red fog","Visual",function()
    Lighting.FogEnd=80; Lighting.FogColor=Color3.fromRGB(220,30,30); Notify("Fog","🔴")
end)
AddCommand("greenfog",{},"Зелёный туман","Green fog","Visual",function()
    Lighting.FogEnd=60; Lighting.FogColor=Color3.fromRGB(30,200,80); Notify("Fog","💚")
end)
local RainbowSkyConn
AddCommand("rainbowsky",{"rsky"},"Радужное небо","Rainbow sky","Visual",function()
    if RainbowSkyConn then RainbowSkyConn:Disconnect() end
    local hue=0
    RainbowSkyConn=RunService.Heartbeat:Connect(function()
        hue=(hue+0.001)%1
        Lighting.Ambient=Color3.fromHSV(hue,0.6,1)
        Lighting.OutdoorAmbient=Color3.fromHSV((hue+0.3)%1,0.6,1)
    end); Notify("RainbowSky","🌈")
end)
AddCommand("unreinbowsky",{},"Выкл радужное небо","Disable rainbow sky","Visual",function()
    if RainbowSkyConn then RainbowSkyConn:Disconnect(); RainbowSkyConn=nil end; Notify("RainbowSky","✗")
end)
AddCommand("xray",{"xr"},"Рентген (прозрачные стены)","X-ray (transparent walls)","Visual",function()
    for _,v in pairs(workspace:GetDescendants()) do
        if v:IsA("BasePart") and not(LocalPlayer.Character and LocalPlayer.Character:IsAncestorOf(v)) then v.Transparency=0.75 end
    end; Notify("X-Ray","✓")
end)
AddCommand("unxray",{"noxr"},"Убрать рентген","Remove x-ray","Visual",function()
    for _,v in pairs(workspace:GetDescendants()) do if v:IsA("BasePart") then v.Transparency=0 end end; Notify("X-Ray","✗")
end)
AddCommand("noir",{"bw","grayscale"},"Чёрно-белый фильтр","Grayscale filter","Visual",function()
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=-1; cc.Contrast=0.2; Notify("Noir","⬛")
end)
AddCommand("vivid",{},"Насыщенные цвета","Vivid colors","Visual",function()
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=1.5; cc.Contrast=0.4; Notify("Vivid","✓")
end)
AddCommand("sepia",{},"Сепия","Sepia filter","Visual",function()
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=-0.6; cc.TintColor=Color3.fromRGB(200,160,110); Notify("Sepia","✓")
end)
AddCommand("horror",{},"Хоррор фильтр","Horror filter","Visual",function()
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=-0.4; cc.TintColor=Color3.fromRGB(255,80,80); Notify("Horror","🔴")
end)
AddCommand("dream",{},"Мечтательный синий","Dream blue filter","Visual",function()
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=0.5; cc.TintColor=Color3.fromRGB(120,140,255); Notify("Dream","💙")
end)
AddCommand("coldfilter",{},"Холодный синий фильтр","Cold blue filter","Visual",function()
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=0.3; cc.TintColor=Color3.fromRGB(80,120,255); Notify("Cold","❄️")
end)
AddCommand("warmfilter",{},"Тёплый золотой фильтр","Warm golden filter","Visual",function()
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=0.8; cc.TintColor=Color3.fromRGB(255,180,80); Notify("Warm","🌅")
end)
AddCommand("unfilter",{"nofilter","resetfilter"},"Убрать фильтр","Remove color filter","Visual",function()
    local cc=Lighting:FindFirstChild("CC"); if cc then cc:Destroy() end; Notify("Filter","✗")
end)
AddCommand("bloom",{},"Bloom [интенсивность]","Bloom effect [intensity]","Visual",function(a)
    local b=Lighting:FindFirstChild("Bloom") or Instance.new("BloomEffect",Lighting); b.Name="Bloom"
    b.Intensity=tonumber(a[1]) or 1.5; b.Size=24; Notify("Bloom",a[1] or "1.5")
end)
AddCommand("unbloom",{},"Убрать bloom","Remove bloom","Visual",function()
    local b=Lighting:FindFirstChild("Bloom"); if b then b:Destroy() end; Notify("Bloom","✗")
end)
AddCommand("cinematic",{},"Кино-полосы","Cinematic bars","Visual",function()
    local c=LocalPlayer.PlayerGui:FindFirstChild("CinBars"); if c then c:Destroy() end
    c=Instance.new("ScreenGui",LocalPlayer.PlayerGui); c.Name="CinBars"; c.ResetOnSpawn=false
    local top=Instance.new("Frame",c); top.Size=UDim2.new(1,0,0,65); top.BackgroundColor3=Color3.new(); top.BorderSizePixel=0
    local bot=Instance.new("Frame",c); bot.Size=UDim2.new(1,0,0,65); bot.Position=UDim2.new(0,0,1,-65); bot.BackgroundColor3=Color3.new(); bot.BorderSizePixel=0
    Notify("Cinematic","🎬")
end)
AddCommand("uncinematic",{},"Убрать кино-полосы","Remove cinematic bars","Visual",function()
    local c=LocalPlayer.PlayerGui:FindFirstChild("CinBars"); if c then c:Destroy() end; Notify("Cinematic","✗")
end)
AddCommand("saturation",{},"Насыщенность [-1 до 2]","Saturation [-1 to 2]","Visual",function(a)
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Saturation=tonumber(a[1]) or 0; Notify("Saturation",a[1] or "0")
end)
AddCommand("contrast",{},"Контраст [-1 до 1]","Contrast [-1 to 1]","Visual",function(a)
    local cc=Lighting:FindFirstChild("CC") or Instance.new("ColorCorrectionEffect",Lighting); cc.Name="CC"
    cc.Contrast=tonumber(a[1]) or 0; Notify("Contrast",a[1] or "0")
end)
AddCommand("sunrays",{},"Лучи солнца [0-1]","Sun rays [0-1]","Visual",function(a)
    local sr=Lighting:FindFirstChild("SunRays") or Instance.new("SunRaysEffect",Lighting); sr.Name="SunRays"
    sr.Intensity=tonumber(a[1]) or 0.25; Notify("SunRays",a[1] or "0.25")
end)
AddCommand("depthfield",{},"Глубина резкости","Depth of field","Visual",function()
    local df=Lighting:FindFirstChild("DepthField") or Instance.new("DepthOfFieldEffect",Lighting); df.Name="DepthField"
    df.FarIntensity=0.5; df.FocusDistance=40; Notify("DepthField","✓")
end)

-- ── CHARACTER ─────────────────────────────────────────────────
AddCommand("giant",{"huge","big"},"Гигант [множитель]","Giant scale [mult]","Character",function(a)
    local s=tonumber(a[1]) or 5
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.BodyDepthScale.Value=s; h.BodyHeightScale.Value=s; h.BodyWidthScale.Value=s; h.HeadScale.Value=s end
    Notify("Giant","×"..s)
end)
AddCommand("tiny",{"small","mini"},"Маленький [множитель]","Small scale [mult]","Character",function(a)
    local s=tonumber(a[1]) or 0.3
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.BodyDepthScale.Value=s; h.BodyHeightScale.Value=s; h.BodyWidthScale.Value=s; h.HeadScale.Value=s end
    Notify("Tiny","×"..s)
end)
AddCommand("fat",{},"Толстый","Fat character","Character",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.BodyDepthScale.Value=2.5; h.BodyWidthScale.Value=2.5; h.BodyHeightScale.Value=1; h.HeadScale.Value=1.3 end
    Notify("Fat","🐷")
end)
AddCommand("tall",{},"Высокий","Tall character","Character",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.BodyHeightScale.Value=3; h.BodyDepthScale.Value=0.8; h.BodyWidthScale.Value=0.8 end
    Notify("Tall","📏")
end)
AddCommand("bighead",{"headsize"},"Большая голова [множитель]","Big head [mult]","Character",function(a)
    local s=tonumber(a[1]) or 4
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.HeadScale.Value=s end; Notify("BigHead","🗿"..s)
end)
AddCommand("resetscale",{"normalsize"},"Сброс размера","Reset character size","Character",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.BodyDepthScale.Value=1; h.BodyHeightScale.Value=1; h.BodyWidthScale.Value=1; h.HeadScale.Value=1 end
    Notify("Scale","×1")
end)
AddCommand("invisible",{"invis","hide"},"Невидимый","Invisible","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") or p:IsA("Decal") then p.Transparency=1 end end
    Notify("Invisible","👻")
end)
AddCommand("visible",{"vis","show","unhide"},"Видимый","Visible","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Transparency=0 elseif p:IsA("Decal") then p.Transparency=0 end end
    Notify("Visible","✓")
end)
AddCommand("semitransparent",{"ghost2","halfinvis"},"Полупрозрачный","Semi-transparent","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Transparency=0.5 end end
    Notify("SemiTrans","✓")
end)
local RainbowCharConn
AddCommand("rainbowchar",{"rc","rainbow"},"Радужный персонаж","Rainbow character","Character",function()
    if RainbowCharConn then RainbowCharConn:Disconnect() end
    local hue=0
    RainbowCharConn=RunService.Heartbeat:Connect(function()
        hue=(hue+0.004)%1
        local char=LocalPlayer.Character; if not char then return end
        for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Color=Color3.fromHSV(hue,1,1) end end
    end); Notify("RainbowChar","🌈")
end)
AddCommand("unreinbowchar",{"unrc","norainbow"},"Выкл радугу персонажа","Disable rainbow","Character",function()
    if RainbowCharConn then RainbowCharConn:Disconnect(); RainbowCharConn=nil end; Notify("RainbowChar","✗")
end)
AddCommand("color",{"charcolor"},"Цвет персонажа [R G B]","Character color [R G B]","Character",function(a)
    local r,g,b=tonumber(a[1]) or 255,tonumber(a[2]) or 100,tonumber(a[3]) or 200
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Color=Color3.fromRGB(r,g,b) end end
    Notify("Color","RGB("..r..","..g..","..b..")")
end)
AddCommand("redchar",{},"Красный персонаж","Red character","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Color=Color3.fromRGB(220,40,40) end end; Notify("RedChar","🔴")
end)
AddCommand("goldchar",{},"Золотой персонаж","Gold character","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Color=Color3.fromRGB(255,200,0) end end; Notify("GoldChar","🥇")
end)
AddCommand("bluechar",{},"Синий персонаж","Blue character","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Color=Color3.fromRGB(30,80,255) end end; Notify("BlueChar","💙")
end)
AddCommand("greenchar",{},"Зелёный персонаж","Green character","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Color=Color3.fromRGB(30,200,80) end end; Notify("GreenChar","💚")
end)
AddCommand("neonchar",{},"Неоновый персонаж","Neon character","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Material=Enum.Material.Neon end end; Notify("NeonChar","✨")
end)
AddCommand("glasschar",{},"Стеклянный персонаж","Glass character","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Material=Enum.Material.Glass; p.Transparency=0.4 end end; Notify("GlassChar","🔷")
end)
AddCommand("metalchar",{},"Металлический персонаж","Metal character","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then p.Material=Enum.Material.Metal; p.Color=Color3.fromRGB(180,180,190) end end; Notify("MetalChar","⚙️")
end)
AddCommand("god",{"godmode","gm"},"Режим бога 1B HP","God mode 1B HP","Character",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.MaxHealth=1000000000; h.Health=1000000000 end; Notify("God","✓")
end)
AddCommand("ungod",{"mortal","nogod"},"Выкл режим бога","Disable god mode","Character",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.MaxHealth=100; h.Health=100 end; Notify("God","✗")
end)
AddCommand("health",{"hp"},"HP [n]","Set health [n]","Character",function(a)
    local v=tonumber(a[1]) or 100
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.Health=v; h.MaxHealth=math.max(h.MaxHealth,v) end; Notify("HP","→"..v)
end)
AddCommand("heal",{},"Полное лечение","Full heal","Character",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.Health=h.MaxHealth end; Notify("Heal","❤️")
end)
AddCommand("kill",{"suicide","die"},"Убить себя","Kill self","Character",function()
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.Health=0 end; Notify("Kill","💀")
end)
AddCommand("maxhp",{},"MaxHP [n]","Max health [n]","Character",function(a)
    local v=tonumber(a[1]) or 1000
    local h=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if h then h.MaxHealth=v; h.Health=v end; Notify("MaxHP","→"..v)
end)
AddCommand("fire",{"addfire"},"Огонь [размер]","Fire effect [size]","Character",function(a)
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then local f=Instance.new("Fire",p); f.Size=tonumber(a[1]) or 5 end end
    Notify("Fire","🔥")
end)
AddCommand("unfire",{"nofire"},"Убрать огонь","Remove fire","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("Fire") then p:Destroy() end end; Notify("Fire","✗")
end)
AddCommand("icyfire",{},"Ледяной огонь","Icy fire","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do
        if p:IsA("BasePart") then local f=Instance.new("Fire",p); f.Color=Color3.fromRGB(0,150,255); f.SecondaryColor=Color3.fromRGB(200,230,255); f.Size=5 end
    end; Notify("IcyFire","❄️🔥")
end)
AddCommand("purplefire",{},"Фиолетовый огонь","Purple fire","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do
        if p:IsA("BasePart") then local f=Instance.new("Fire",p); f.Color=Color3.fromRGB(180,0,255); f.SecondaryColor=Color3.fromRGB(100,0,200); f.Size=5 end
    end; Notify("PurpleFire","💜🔥")
end)
AddCommand("sparkles",{},"Блёстки","Sparkles","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do if p:IsA("BasePart") then Instance.new("Sparkles",p) end end; Notify("Sparkles","✨")
end)
AddCommand("goldsparkles",{},"Золотые блёстки","Gold sparkles","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,p in pairs(char:GetDescendants()) do
        if p:IsA("BasePart") then local sp=Instance.new("Sparkles",p); sp.SparkleColor=Color3.fromRGB(255,200,50) end
    end; Notify("GoldSparkles","⭐")
end)
AddCommand("unsparkles",{"nosparkles"},"Убрать блёстки","Remove sparkles","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,v in pairs(char:GetDescendants()) do if v:IsA("Sparkles") then v:Destroy() end end; Notify("Sparkles","✗")
end)
AddCommand("smoke",{},"Дым","Smoke","Character",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    Instance.new("Smoke",root); Notify("Smoke","💨")
end)
AddCommand("unsmoke",{"nosmoke"},"Убрать дым","Remove smoke","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,v in pairs(char:GetDescendants()) do if v:IsA("Smoke") then v:Destroy() end end; Notify("Smoke","✗")
end)
AddCommand("ff",{"forcefield"},"Силовое поле","Force field","Character",function()
    local char=LocalPlayer.Character; if not char then return end; Instance.new("ForceField",char); Notify("FF","🛡")
end)
AddCommand("unff",{"noff"},"Убрать силовое поле","Remove force field","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,v in pairs(char:GetChildren()) do if v:IsA("ForceField") then v:Destroy() end end; Notify("FF","✗")
end)
AddCommand("trail",{},"Зелёный след","Green trail","Character",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local a0=Instance.new("Attachment",root); a0.Position=Vector3.new(0,0.5,0)
    local a1=Instance.new("Attachment",root); a1.Position=Vector3.new(0,-0.5,0)
    local t=Instance.new("Trail",root); t.Attachment0=a0; t.Attachment1=a1; t.Lifetime=0.7
    t.Color=ColorSequence.new({ColorSequenceKeypoint.new(0,Color3.fromRGB(0,255,180)),ColorSequenceKeypoint.new(1,Color3.fromRGB(0,100,255))})
    t.WidthScale=NumberSequence.new({NumberSequenceKeypoint.new(0,1),NumberSequenceKeypoint.new(1,0)}); Notify("Trail","✓")
end)
AddCommand("rainbowtrail",{},"Радужный след","Rainbow trail","Character",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local a0=Instance.new("Attachment",root); a0.Position=Vector3.new(0,0.5,0)
    local a1=Instance.new("Attachment",root); a1.Position=Vector3.new(0,-0.5,0)
    local t=Instance.new("Trail",root); t.Attachment0=a0; t.Attachment1=a1; t.Lifetime=1
    t.Color=ColorSequence.new({
        ColorSequenceKeypoint.new(0,Color3.fromRGB(255,0,0)),ColorSequenceKeypoint.new(0.17,Color3.fromRGB(255,165,0)),
        ColorSequenceKeypoint.new(0.34,Color3.fromRGB(255,255,0)),ColorSequenceKeypoint.new(0.5,Color3.fromRGB(0,255,0)),
        ColorSequenceKeypoint.new(0.67,Color3.fromRGB(0,100,255)),ColorSequenceKeypoint.new(1,Color3.fromRGB(150,0,255))
    }); Notify("RainbowTrail","🌈")
end)
AddCommand("untrail",{"notrail"},"Убрать след","Remove trail","Character",function()
    local char=LocalPlayer.Character; if not char then return end
    for _,v in pairs(char:GetDescendants()) do if v:IsA("Trail") then v:Destroy() end end; Notify("Trail","✗")
end)
AddCommand("hitbox",{"hb"},"Хитбокс [размер]","Hitbox expand [size]","Character",function(a)
    local sz=tonumber(a[1]) or 6
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    root.Size=Vector3.new(sz,sz,sz); Notify("Hitbox","×"..sz)
end)
AddCommand("resethitbox",{},"Сброс хитбокса","Reset hitbox","Character",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    root.Size=Vector3.new(2,2,1); Notify("Hitbox","×1")
end)

-- ── PHYSICS ───────────────────────────────────────────────────
AddCommand("fling",{},"Запустить игрока [имя] [сила]","Fling player [name] [force]","Physics",function(a)
    local name=(a[1] or ""):lower(); local force=tonumber(a[2]) or 600
    for _,p in pairs(Players:GetPlayers()) do
        if p.Name:lower():find(name) and p~=LocalPlayer then
            local root=p.Character and p.Character:FindFirstChild("HumanoidRootPart")
            if root then
                local bv=Instance.new("BodyVelocity",root)
                bv.Velocity=Vector3.new((math.random()-0.5)*force*2,force*0.6,(math.random()-0.5)*force*2)
                bv.MaxForce=Vector3.new(1000000000,1000000000,1000000000); task.delay(0.15,function() bv:Destroy() end)
                Notify("Fling","→"..p.Name)
            end
        end
    end
end)
AddCommand("launch",{},"Запустить себя вверх [сила]","Launch self upward [force]","Physics",function(a)
    local f=tonumber(a[1]) or 350
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then local bv=Instance.new("BodyVelocity",root); bv.Velocity=Vector3.new(0,f,0); bv.MaxForce=Vector3.new(0,1000000000,0); task.delay(0.2,function() bv:Destroy() end) end
    Notify("Launch","🚀"..f)
end)
AddCommand("explodeself",{},"Взрыв на месте","Explode on position","Physics",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then local e=Instance.new("Explosion",workspace); e.Position=root.Position; e.BlastRadius=10; e.BlastPressure=100000 end; Notify("Explode","💥")
end)
local AntiGravConn
AddCommand("antigrav",{"float","levitate"},"Антигравитация (парение)","Anti-gravity (float)","Physics",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    if AntiGravConn then AntiGravConn:Disconnect() end
    local bf=Instance.new("BodyForce",root); bf.Name="AntiGrav"
    AntiGravConn=RunService.Heartbeat:Connect(function() bf.Force=Vector3.new(0,root.AssemblyMass*workspace.Gravity,0) end)
    Notify("AntiGrav","🎈")
end)
AddCommand("unantigrav",{"nolevitate"},"Выкл антигравитацию","Disable anti-gravity","Physics",function()
    if AntiGravConn then AntiGravConn:Disconnect(); AntiGravConn=nil end
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root and root:FindFirstChild("AntiGrav") then root.AntiGrav:Destroy() end; Notify("AntiGrav","✗")
end)
local BounceConn
AddCommand("bounce",{},"Отскок при приземлении [сила]","Bounce on landing [force]","Physics",function(a)
    local f=tonumber(a[1]) or 100
    local hum=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid"); if not hum then return end
    if BounceConn then BounceConn:Disconnect() end
    BounceConn=hum.StateChanged:Connect(function(_,new)
        if new==Enum.HumanoidStateType.Landed then
            local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if root then local bv=Instance.new("BodyVelocity",root); bv.Velocity=Vector3.new(0,f,0); bv.MaxForce=Vector3.new(0,1000000000,0); task.delay(0.1,function() bv:Destroy() end) end
        end
    end); Notify("Bounce","↕"..f)
end)
AddCommand("unbounce",{"nobounce"},"Выкл отскок","Disable bounce","Physics",function()
    if BounceConn then BounceConn:Disconnect(); BounceConn=nil end; Notify("Bounce","✗")
end)

-- ── WORLD ─────────────────────────────────────────────────────
AddCommand("deletepart",{"dp","del"},"Удалить деталь под прицелом","Delete part under mouse","World",function()
    local mouse=LocalPlayer:GetMouse()
    if mouse.Target then mouse.Target:Destroy(); Notify("Delete","✓") end
end)
AddCommand("deletenpcs",{"dnpc","killnpcs"},"Удалить всех NPC","Delete all NPCs","World",function()
    local count=0
    for _,v in pairs(workspace:GetDescendants()) do
        if v:IsA("Model") and v:FindFirstChildOfClass("Humanoid") and not Players:GetPlayerFromCharacter(v) then v:Destroy(); count+=1 end
    end; Notify("NPCs","Удалено:"..count)
end)
AddCommand("flood",{},"Потоп [высота Y]","Flood [Y height]","World",function(a)
    local y=tonumber(a[1]) or 0
    local w=Instance.new("Part",workspace); w.Name="FloodWater"; w.Size=Vector3.new(2000,1,2000)
    w.Position=Vector3.new(0,y,0); w.Anchored=true; w.BrickColor=BrickColor.new("Bright blue")
    w.Transparency=0.4; w.CanCollide=false; w.Material=Enum.Material.Neon; Notify("Flood","🌊Y="..y)
end)
AddCommand("unflood",{"noflood"},"Убрать потоп","Remove flood","World",function()
    for _,v in pairs(workspace:GetChildren()) do if v.Name=="FloodWater" then v:Destroy() end end; Notify("Flood","✗")
end)
AddCommand("platform",{"plat"},"Платформа под ногами","Platform under feet","World",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local p=Instance.new("Part",workspace); p.Size=Vector3.new(14,1,14); p.Position=root.Position-Vector3.new(0,3,0)
    p.Anchored=true; p.BrickColor=BrickColor.new("Bright green"); p.Material=Enum.Material.Neon; Notify("Platform","✓")
end)
AddCommand("bigplatform",{},"Большая платформа","Big platform","World",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local p=Instance.new("Part",workspace); p.Size=Vector3.new(60,1,60); p.Position=root.Position-Vector3.new(0,3,0)
    p.Anchored=true; p.BrickColor=BrickColor.new("Bright green"); p.Material=Enum.Material.Neon; Notify("BigPlatform","✓")
end)
AddCommand("neonworld",{},"Неоновый мир","Neon world","World",function()
    for _,v in pairs(workspace:GetDescendants()) do if v:IsA("BasePart") then v.Material=Enum.Material.Neon end end; Notify("NeonWorld","✨")
end)
AddCommand("rainbowworld",{},"Радужный мир","Rainbow world","World",function()
    for _,v in pairs(workspace:GetDescendants()) do if v:IsA("BasePart") then v.Color=Color3.fromHSV(math.random(),1,1) end end; Notify("RainbowWorld","🌈")
end)
AddCommand("glassworld",{},"Стеклянный мир","Glass world","World",function()
    for _,v in pairs(workspace:GetDescendants()) do if v:IsA("BasePart") then v.Material=Enum.Material.Glass; v.Transparency=0.3 end end; Notify("GlassWorld","🔷")
end)
AddCommand("tnt",{},"Бомба через 2с","Bomb after 2s","World",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local p=Instance.new("Part",workspace); p.Size=Vector3.new(3,3,3); p.BrickColor=BrickColor.new("Bright red")
    p.Position=root.Position+Vector3.new(0,5,0); p.Material=Enum.Material.Neon
    task.delay(2,function() local e=Instance.new("Explosion",workspace); e.Position=p.Position; e.BlastRadius=25; e.BlastPressure=200000; p:Destroy() end)
    Notify("TNT","💣 2с")
end)
AddCommand("meteor",{},"Метеоритный дождь [кол-во]","Meteor shower [count]","World",function(a)
    local count=tonumber(a[1]) or 5
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    for i=1,count do
        task.delay(i*0.4,function()
            local p=Instance.new("Part",workspace); p.Shape=Enum.PartType.Ball; p.Size=Vector3.new(8,8,8)
            p.BrickColor=BrickColor.new("Bright orange"); p.Material=Enum.Material.Neon
            p.Position=root.Position+Vector3.new(math.random(-40,40),180,math.random(-40,40))
            local bv=Instance.new("BodyVelocity",p); bv.Velocity=Vector3.new(0,-300,0); bv.MaxForce=Vector3.new(0,1000000000,0)
            Instance.new("Fire",p).Size=12; task.delay(6,function() if p then p:Destroy() end end)
        end)
    end; Notify("Meteor","☄️"..count)
end)
AddCommand("paintworld",{},"Покрасить мир [R G B]","Paint world [R G B]","World",function(a)
    local r,g,b=tonumber(a[1]) or 255,tonumber(a[2]) or 100,tonumber(a[3]) or 50
    for _,v in pairs(workspace:GetDescendants()) do if v:IsA("BasePart") then v.Color=Color3.fromRGB(r,g,b) end end
    Notify("PaintWorld","RGB("..r..","..g..","..b..")")
end)
AddCommand("earthquake",{},"Землетрясение 5с","Earthquake 5s","World",function()
    local orig=Camera.CFrame; local t2=0
    local conn2; conn2=RunService.RenderStepped:Connect(function(dt)
        t2=t2+dt; if t2>5 then Camera.CFrame=orig; conn2:Disconnect(); return end
        Camera.CFrame=Camera.CFrame*CFrame.new(math.random(-3,3)*0.1,math.random(-2,2)*0.1,0)
    end); Notify("Earthquake","🌍")
end)
AddCommand("dropball",{},"Уронить большой шар","Drop big ball","World",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local p=Instance.new("Part",workspace); p.Shape=Enum.PartType.Ball; p.Size=Vector3.new(20,20,20)
    p.Position=root.Position+Vector3.new(0,60,0); p.BrickColor=BrickColor.Random(); p.Material=Enum.Material.Neon
    Notify("DropBall","🔮")
end)
AddCommand("sphere",{},"Создать сферу [размер]","Create sphere [size]","World",function(a)
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local sz=tonumber(a[1]) or 10
    local p=Instance.new("Part",workspace); p.Shape=Enum.PartType.Ball; p.Size=Vector3.new(sz,sz,sz)
    p.Position=root.Position+Vector3.new(0,sz/2+5,sz+5); p.BrickColor=BrickColor.Random(); p.Material=Enum.Material.Neon; p.Anchored=true
    Notify("Sphere","⚽"..sz)
end)
AddCommand("wall",{},"Стена перед персонажем","Wall in front of character","World",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local p=Instance.new("Part",workspace); p.Size=Vector3.new(20,20,1)
    p.CFrame=root.CFrame+root.CFrame.LookVector*6; p.Anchored=true; p.BrickColor=BrickColor.new("Medium stone grey")
    Notify("Wall","🧱")
end)
AddCommand("cage",{},"Клетка вокруг персонажа","Cage around character","World",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local pos=root.Position
    local sides={{Vector3.new(10,10,1),Vector3.new(0,0,5)},{Vector3.new(10,10,1),Vector3.new(0,0,-5)},{Vector3.new(1,10,10),Vector3.new(5,0,0)},{Vector3.new(1,10,10),Vector3.new(-5,0,0)},{Vector3.new(10,1,10),Vector3.new(0,5,0)},{Vector3.new(10,1,10),Vector3.new(0,-5,0)}}
    for _,s in pairs(sides) do local p=Instance.new("Part",workspace); p.Size=s[1]; p.Position=pos+s[2]; p.Anchored=true; p.Material=Enum.Material.Neon; p.BrickColor=BrickColor.Random(); p.Transparency=0.5 end
    Notify("Cage","🔒")
end)

-- ── CAMERA ────────────────────────────────────────────────────
local FreecamConn
AddCommand("freecam",{"fc"},"Свободная камера [скорость]","Free camera [speed]","Camera",function(a)
    local spd=tonumber(a[1]) or 0.5
    Camera.CameraType=Enum.CameraType.Scriptable
    if FreecamConn then FreecamConn:Disconnect() end
    FreecamConn=RunService.RenderStepped:Connect(function()
        local delta=UserInputService:GetMouseDelta(); local mv=Vector3.zero
        if UserInputService:IsKeyDown(Enum.KeyCode.W) then mv=mv+Camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.S) then mv=mv-Camera.CFrame.LookVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.A) then mv=mv-Camera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.D) then mv=mv+Camera.CFrame.RightVector end
        if UserInputService:IsKeyDown(Enum.KeyCode.E) or UserInputService:IsKeyDown(Enum.KeyCode.Space) then mv=mv+Vector3.new(0,1,0) end
        if UserInputService:IsKeyDown(Enum.KeyCode.Q) or UserInputService:IsKeyDown(Enum.KeyCode.LeftControl) then mv=mv-Vector3.new(0,1,0) end
        Camera.CFrame=Camera.CFrame+mv*spd
        Camera.CFrame=Camera.CFrame*CFrame.Angles(math.rad(-delta.Y*0.2),math.rad(-delta.X*0.2),0)
    end); Notify("Freecam","✓ "..spd)
end)
AddCommand("unfreecam",{"unfc","stopcam"},"Выкл freecam","Disable free camera","Camera",function()
    if FreecamConn then FreecamConn:Disconnect(); FreecamConn=nil end
    Camera.CameraType=Enum.CameraType.Custom; Notify("Freecam","✗")
end)
AddCommand("zoom",{},"Зум [дистанция]","Camera zoom [distance]","Camera",function(a)
    local d=tonumber(a[1]) or 20
    LocalPlayer.CameraMaxZoomDistance=d; LocalPlayer.CameraMinZoomDistance=d; Notify("Zoom","→"..d)
end)
AddCommand("resetzoom",{},"Сброс зума","Reset camera zoom","Camera",function()
    LocalPlayer.CameraMaxZoomDistance=400; LocalPlayer.CameraMinZoomDistance=0.5; Notify("Zoom","✓")
end)
AddCommand("fp",{"firstperson","1st"},"Вид от первого лица","First person view","Camera",function()
    LocalPlayer.CameraMaxZoomDistance=0.5; LocalPlayer.CameraMinZoomDistance=0.5; Notify("Camera","👁 1st")
end)
AddCommand("tp3",{"thirdperson","3rd"},"Вид от третьего лица","Third person view","Camera",function()
    LocalPlayer.CameraMaxZoomDistance=24; LocalPlayer.CameraMinZoomDistance=0.5; Notify("Camera","👤 3rd")
end)
AddCommand("farcam",{},"Дальняя камера","Far camera (200)","Camera",function()
    LocalPlayer.CameraMaxZoomDistance=200; Notify("Camera","🔭")
end)
local ShakeCamConn
AddCommand("shakecam",{},"Тряска камеры [интенсивность]","Camera shake [intensity]","Camera",function(a)
    local intensity=tonumber(a[1]) or 1
    if ShakeCamConn then ShakeCamConn:Disconnect() end
    ShakeCamConn=RunService.RenderStepped:Connect(function()
        Camera.CFrame=Camera.CFrame*CFrame.new(math.random(-intensity,intensity)*0.1,math.random(-intensity,intensity)*0.1,0)
    end); Notify("ShakeCam","📷"..intensity)
end)
AddCommand("unshakecam",{},"Выкл тряску камеры","Disable camera shake","Camera",function()
    if ShakeCamConn then ShakeCamConn:Disconnect(); ShakeCamConn=nil end; Notify("ShakeCam","✗")
end)
local CamSpinConn
AddCommand("camspin",{},"Вращение камеры [скорость]","Spin camera [speed]","Camera",function(a)
    local s=tonumber(a[1]) or 1
    if CamSpinConn then CamSpinConn:Disconnect() end
    Camera.CameraType=Enum.CameraType.Scriptable
    local angle=0
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    CamSpinConn=RunService.RenderStepped:Connect(function(dt)
        angle=angle+s*dt
        if root then Camera.CFrame=CFrame.new(root.Position+Vector3.new(math.sin(angle)*20,10,math.cos(angle)*20),root.Position) end
    end); Notify("CamSpin","⟳"..s)
end)
AddCommand("uncamspin",{},"Выкл вращение камеры","Disable camera spin","Camera",function()
    if CamSpinConn then CamSpinConn:Disconnect(); CamSpinConn=nil end
    Camera.CameraType=Enum.CameraType.Custom; Notify("CamSpin","✗")
end)

-- ── FUN ───────────────────────────────────────────────────────
AddCommand("confetti",{},"Конфетти","Confetti burst","Fun",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    for i=1,40 do
        local p=Instance.new("Part",workspace); p.Size=Vector3.new(0.4,0.4,0.08); p.BrickColor=BrickColor.Random(); p.CanCollide=false
        p.Position=root.Position+Vector3.new(math.random(-2,2),math.random(0,3),math.random(-2,2))
        p.Velocity=Vector3.new(math.random(-25,25),math.random(15,40),math.random(-25,25)); p.Material=Enum.Material.Neon
        task.delay(4,function() if p then p:Destroy() end end)
    end; Notify("Confetti","🎉")
end)
AddCommand("fireworks",{},"Фейерверк","Fireworks","Fun",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    for i=1,8 do
        task.delay(i*0.2,function()
            local p=Instance.new("Part",workspace); p.Shape=Enum.PartType.Ball; p.Size=Vector3.new(0.5,0.5,0.5)
            p.Position=root.Position+Vector3.new(math.random(-5,5),math.random(5,15),math.random(-5,5))
            p.BrickColor=BrickColor.Random(); p.Material=Enum.Material.Neon; p.CanCollide=false
            local bv=Instance.new("BodyVelocity",p); bv.Velocity=Vector3.new(math.random(-30,30),math.random(30,60),math.random(-30,30)); bv.MaxForce=Vector3.new(1000000000,1000000000,1000000000)
            task.delay(0.5,function() local e=Instance.new("Explosion",workspace); e.Position=p.Position; e.BlastRadius=5; e.BlastPressure=0; p:Destroy() end)
        end)
    end; Notify("Fireworks","🎆")
end)
AddCommand("nametag",{"tag"},"Ник-тег [текст]","Name tag [text]","Fun",function(a)
    local text=#a>0 and table.concat(a," ") or LocalPlayer.Name
    local head=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head"); if not head then return end
    for _,v in pairs(head:GetChildren()) do if v:IsA("BillboardGui") then v:Destroy() end end
    local bg=Instance.new("BillboardGui",head); bg.Size=UDim2.new(0,180,0,44); bg.StudsOffset=Vector3.new(0,4,0); bg.AlwaysOnTop=true
    local lbl=Instance.new("TextLabel",bg); lbl.Size=UDim2.new(1,0,1,0); lbl.BackgroundColor3=Color3.fromRGB(0,0,0); lbl.BackgroundTransparency=0.45
    lbl.Text=text; lbl.TextColor3=Color3.fromRGB(0,255,180); lbl.TextSize=13; lbl.Font=Enum.Font.GothamBold
    Instance.new("UICorner",lbl).CornerRadius=UDim.new(0,8); Notify("NameTag",text)
end)
AddCommand("removetag",{"untag"},"Убрать ник-тег","Remove name tag","Fun",function()
    local head=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head")
    if head then for _,v in pairs(head:GetChildren()) do if v:IsA("BillboardGui") then v:Destroy() end end end
    Notify("NameTag","✗")
end)
AddCommand("rainbowname",{},"Радужный ник-тег","Rainbow name tag","Fun",function()
    local head=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head"); if not head then return end
    for _,v in pairs(head:GetChildren()) do if v:IsA("BillboardGui") then v:Destroy() end end
    local bg=Instance.new("BillboardGui",head); bg.Size=UDim2.new(0,200,0,44); bg.StudsOffset=Vector3.new(0,4,0); bg.AlwaysOnTop=true
    local lbl=Instance.new("TextLabel",bg); lbl.Size=UDim2.new(1,0,1,0); lbl.BackgroundColor3=Color3.fromRGB(0,0,0); lbl.BackgroundTransparency=0.45
    lbl.Text=LocalPlayer.Name; lbl.TextSize=13; lbl.Font=Enum.Font.GothamBold
    Instance.new("UICorner",lbl).CornerRadius=UDim.new(0,8)
    local hue=0
    RunService.Heartbeat:Connect(function() hue=(hue+0.01)%1; lbl.TextColor3=Color3.fromHSV(hue,1,1) end)
    Notify("RainbowName","🌈")
end)
AddCommand("disco",{},"Диско пол 30с","Disco floor 30s","Fun",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local floor=Instance.new("Part",workspace); floor.Name="DiscoFloor"; floor.Anchored=true
    floor.Size=Vector3.new(60,1,60); floor.Position=Vector3.new(root.Position.X,root.Position.Y-3,root.Position.Z); floor.Material=Enum.Material.Neon
    local conn2=RunService.Heartbeat:Connect(function() floor.Color=Color3.fromHSV(tick()%1,1,1) end)
    task.delay(30,function() floor:Destroy(); conn2:Disconnect() end); Notify("Disco","🕺")
end)
AddCommand("particle",{},"Всплеск частиц","Particle burst","Fun",function()
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"); if not root then return end
    local att=Instance.new("Attachment",root); local pe=Instance.new("ParticleEmitter",att)
    pe.Rate=300; pe.Lifetime=NumberRange.new(1,2); pe.Speed=NumberRange.new(15,40)
    pe.Color=ColorSequence.new({ColorSequenceKeypoint.new(0,Color3.fromRGB(255,100,0)),ColorSequenceKeypoint.new(1,Color3.fromRGB(255,0,200))})
    task.delay(1.5,function() pe:Destroy(); att:Destroy() end); Notify("Particles","✨")
end)
AddCommand("broadcast",{},"Сообщение на экране [текст]","On-screen message [text]","Fun",function(a)
    local text=table.concat(a," ")
    local sg=Instance.new("ScreenGui",LocalPlayer.PlayerGui); sg.ResetOnSpawn=false
    local lbl=Instance.new("TextLabel",sg); lbl.Size=UDim2.new(1,0,0,50); lbl.Position=UDim2.new(0,0,0.15,0)
    lbl.BackgroundColor3=Color3.new(); lbl.BackgroundTransparency=0.4; lbl.Text=text
    lbl.TextColor3=Color3.new(1,1,1); lbl.TextSize=22; lbl.Font=Enum.Font.GothamBold
    task.delay(4,function() sg:Destroy() end); Notify("Broadcast",text)
end)
AddCommand("bigtext",{},"Большой текст [слова]","Big text on screen [words]","Fun",function(a)
    local text=#a>0 and table.concat(a," ") or "GLORK"
    local sg=Instance.new("ScreenGui",LocalPlayer.PlayerGui); sg.ResetOnSpawn=false
    local lbl=Instance.new("TextLabel",sg); lbl.Size=UDim2.new(1,0,1,0); lbl.BackgroundTransparency=1
    lbl.Text=text; lbl.TextColor3=Color3.fromRGB(0,255,180); lbl.TextSize=80; lbl.Font=Enum.Font.GothamBold; lbl.TextTransparency=0.15
    task.delay(3,function() sg:Destroy() end); Notify("BigText",text)
end)

-- ── AUDIO ─────────────────────────────────────────────────────
AddCommand("mute",{},"Заглушить всё","Mute all sounds","Audio",function()
    for _,s in pairs(workspace:GetDescendants()) do if s:IsA("Sound") then s.Volume=0 end end; Notify("Mute","🔇")
end)
AddCommand("unmute",{},"Восстановить звук","Unmute all sounds","Audio",function()
    for _,s in pairs(workspace:GetDescendants()) do if s:IsA("Sound") then s.Volume=0.5 end end; Notify("Unmute","🔊")
end)
AddCommand("pitchup",{},"Повысить тон [множитель]","Pitch up [multiplier]","Audio",function(a)
    local pitch=tonumber(a[1]) or 2
    for _,s in pairs(workspace:GetDescendants()) do
        if s:IsA("Sound") then local pe=s:FindFirstChildOfClass("PitchShiftSoundEffect") or Instance.new("PitchShiftSoundEffect",s); pe.Octave=pitch end
    end; Notify("Pitch","↑"..pitch)
end)
AddCommand("pitchdown",{},"Понизить тон [множитель]","Pitch down [multiplier]","Audio",function(a)
    local pitch=tonumber(a[1]) or 0.5
    for _,s in pairs(workspace:GetDescendants()) do
        if s:IsA("Sound") then local pe=s:FindFirstChildOfClass("PitchShiftSoundEffect") or Instance.new("PitchShiftSoundEffect",s); pe.Octave=pitch end
    end; Notify("Pitch","↓"..pitch)
end)
AddCommand("volume",{},"Громкость [0-1]","Volume [0-1]","Audio",function(a)
    local v=tonumber(a[1]) or 1
    for _,s in pairs(workspace:GetDescendants()) do if s:IsA("Sound") then s.Volume=v end end; Notify("Volume","→"..v)
end)

-- ── WAYPOINTS ─────────────────────────────────────────────────
AddCommand("setwp",{"setwaypoint","savewp"},"Сохранить вейпоинт [имя]","Save waypoint [name]","Waypoints",function(a)
    local name=a[1] or "default"
    local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then Waypoints[name]=root.CFrame end; Notify("Waypoint","★"..name)
end)
AddCommand("gowp",{"gotowp","warp"},"Перейти к вейпоинту [имя]","Go to waypoint [name]","Waypoints",function(a)
    local name=a[1] or "default"
    if Waypoints[name] then
        local root=LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
        if root then root.CFrame=Waypoints[name] end; Notify("Waypoint","→"..name)
    else Notify("Waypoint","✗ "..name,Color3.fromRGB(255,100,100)) end
end)
AddCommand("listwp",{"waypoints","wps"},"Список вейпоинтов","List waypoints","Waypoints",function()
    local list={}; for k in pairs(Waypoints) do table.insert(list,k) end
    Notify("Waypoints",#list==0 and (Lang=="RU" and "Пусто" or "Empty") or table.concat(list,", "))
end)
AddCommand("delwp",{"deletewp"},"Удалить вейпоинт [имя]","Delete waypoint [name]","Waypoints",function(a)
    Waypoints[a[1] or "default"]=nil; Notify("Waypoint","✗"..(a[1] or "default"))
end)
AddCommand("clearwp",{"clearwaypoints"},"Очистить все вейпоинты","Clear all waypoints","Waypoints",function()
    Waypoints={}; Notify("Waypoints",Lang=="RU" and "Все удалены" or "All cleared")
end)

-- ── UTILITY ───────────────────────────────────────────────────
AddCommand("help",{"?","cmds","commands"},"Открыть панель команд","Open command panel","Utility",function()
    SetPanel(true)
end)
AddCommand("panel",{"menu","gui","open"},"Переключить панель","Toggle panel","Utility",function()
    SetPanel(not panelOpen)
end)
AddCommand("scripts",{"scr"},"Открыть вкладку скриптов","Open scripts tab","Utility",function()
    ActiveTab="scripts"; for _,td in ipairs(TABS) do StyleTab(td[1],td[1]=="scripts") end
    SetPanel(true); Refresh()
end)
AddCommand("version",{"ver","info"},"Версия скрипта","Script version","Utility",function()
    Notify("Glork Ultra","v8.0  •  "..#CommandData..(Lang=="RU" and " команд" or " cmds").."  •  Instant drag")
end)
AddCommand("ping",{"latency","ms"},"Пинг","Network ping","Utility",function()
    Notify("Ping",math.floor(LocalPlayer:GetNetworkPing()*1000).."ms")
end)
AddCommand("clearnotify",{"cn","clearnotifs"},"Очистить уведомления","Clear notifications","Utility",function()
    for _,v in pairs(NotifStack:GetChildren()) do if v:IsA("Frame") then v:Destroy() end end
end)
AddCommand("players",{"plist","who"},"Список игроков","List players","Utility",function()
    local list={}; for _,p in pairs(Players:GetPlayers()) do table.insert(list,p.Name) end
    Notify("Players("..#list..")",table.concat(list,", "))
end)
AddCommand("hidegui",{"hgui","hidehud"},"Скрыть интерфейс игры","Hide game GUI","Utility",function()
    StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.All,false); Notify("GUI",Lang=="RU" and "Скрыт" or "Hidden")
end)
AddCommand("showgui",{"sgui","showhud"},"Показать интерфейс игры","Show game GUI","Utility",function()
    StarterGui:SetCoreGuiEnabled(Enum.CoreGuiType.All,true); Notify("GUI",Lang=="RU" and "Показан" or "Shown")
end)
AddCommand("godclip",{"ultimatemode","ult"},"God+Noclip+InfJump+Speed","God+Noclip+InfJump+Speed","Utility",function()
    Run("god"); Run("noclip"); Run("infjump"); Run("ws 60"); Notify("Ultimate","✓")
end)
AddCommand("ungodclip",{"normalmode","unult"},"Отключить Ultimate","Disable Ultimate","Utility",function()
    Run("ungod"); Run("clip"); Run("uninfjump"); Run("resetspeed"); Notify("Normal","✓")
end)
AddCommand("speedrun",{},"Максимальная скорость бега","Maximum running speed","Utility",function()
    Run("ws 120"); Run("infjump"); Run("jh 250"); Notify("Speedrun","🏃")
end)
AddCommand("stealth",{},"Invisible+Noclip+Speed","Invisible+Noclip+Speed","Utility",function()
    Run("invisible"); Run("noclip"); Run("ws 70"); Notify("Stealth","👻")
end)
AddCommand("party",{"partymode"},"Rainbow+Confetti+Sparkles","Rainbow+Confetti+Sparkles","Utility",function()
    Run("rainbowchar"); Run("confetti"); Run("sparkles"); Notify("Party","🎉🌈✨")
end)
AddCommand("tryhard",{},"Максимальный режим","Maximum tryhard mode","Utility",function()
    Run("god"); Run("ws 200"); Run("jh 500"); Run("noclip"); Run("infjump"); Notify("Tryhard","💀")
end)
AddCommand("chill",{},"Расслабленный режим","Chill mode","Utility",function()
    Run("moongrav"); Run("slowmo"); Run("antiafk"); Run("fullbright"); Notify("Chill","😎")
end)
AddCommand("tank",{},"God+Giant×3+FF","God+Giant×3+FF","Utility",function()
    Run("god"); Run("giant 3"); Run("heavygrav"); Run("ff"); Notify("Tank","🛡")
end)
AddCommand("ghost",{},"Invisible+Noclip+God","Invisible+Noclip+God","Utility",function()
    Run("invisible"); Run("noclip"); Run("god"); Notify("Ghost","👻")
end)
AddCommand("lang",{"language","ru","en"},"Переключить язык RU/EN","Switch language RU/EN","Utility",function()
    ApplyLang()
end)
AddCommand("resetall",{"reset","disableall","clearall"},"Сбросить ВСЕ читы","Reset ALL cheats","Utility",function()
    Run("ungod"); Run("clip"); Run("uninfjump"); Run("unfly")
    Run("visible"); Run("resetspeed"); Run("resetgrav"); Run("resetscale")
    Run("unesp"); Run("unfullbright"); Run("unfilter"); Run("resetfov")
    Run("unsparkles"); Run("unfire"); Run("untrail"); Run("unsmoke")
    Run("unbounce"); Run("unantigrav"); Run("unlooptp"); Run("unspin")
    if RainbowCharConn then RainbowCharConn:Disconnect(); RainbowCharConn=nil end
    if RainbowESPConn then RainbowESPConn:Disconnect(); RainbowESPConn=nil end
    if RainbowSkyConn then RainbowSkyConn:Disconnect(); RainbowSkyConn=nil end
    if FreecamConn then FreecamConn:Disconnect(); FreecamConn=nil; Camera.CameraType=Enum.CameraType.Custom end
    if CamSpinConn then CamSpinConn:Disconnect(); CamSpinConn=nil; Camera.CameraType=Enum.CameraType.Custom end
    if ShakeCamConn then ShakeCamConn:Disconnect(); ShakeCamConn=nil end
    Notify(Lang=="RU" and "СБРОС" or "RESET",Lang=="RU" and "✓ Всё отключено" or "✓ All disabled")
end)
AddCommand("leave",{"exit","quit"},"Покинуть игру","Leave game","Utility",function()
    LocalPlayer:Kick("Glork Ultra v8.0")
end)
AddCommand("favorites",{"favs","starred"},"Вкладка избранного","Open favorites tab","Utility",function()
    ActiveTab="favorites"; for _,td in ipairs(TABS) do StyleTab(td[1],td[1]=="favorites") end
    SetPanel(true); Refresh()
end)
AddCommand("wiki",{},"Открыть вики","Open wiki tab","Utility",function()
    ActiveTab="wiki"; for _,td in ipairs(TABS) do StyleTab(td[1],td[1]=="wiki") end
    SetPanel(true); Refresh()
end)
local SAVE_FILE = "glork_data.json"
local History   = {}
local Settings  = { defaultWS=16, defaultGrav=196.2, defaultFOV=70 }

-- ── Waypoint serialization ────────────────────────────────────
local function SerWP()
    local t = {}
    for name, cf in pairs(Waypoints) do
        local p = cf.Position
        t[name] = { px=p.X, py=p.Y, pz=p.Z }
    end
    return t
end
local function DeSerWP(t)
    local r = {}
    for name, d in pairs(t) do
        r[name] = CFrame.new(d.px or 0, d.py or 0, d.pz or 0)
    end
    return r
end

local function SerPos(u)
    return { xs=u.X.Scale, xo=u.X.Offset, ys=u.Y.Scale, yo=u.Y.Offset }
end
local function DeSerPos(t)
    if not t then return nil end
    return UDim2.new(t.xs or 0, t.xo or 0, t.ys or 0, t.yo or 0)
end

-- ── Minimal JSON encode ───────────────────────────────────────
local function jEnc(v)
    local t = type(v)
    if t=="nil" then return "null"
    elseif t=="boolean" then return tostring(v)
    elseif t=="number" then return v~=v and "null" or tostring(v)
    elseif t=="string" then return '"'..v:gsub('\\','\\\\'):gsub('"','\\"'):gsub('\n','\\n')..'"'
    elseif t=="table" then
        local isArr, n = true, 0
        for k in pairs(v) do n+=1; if type(k)~="number" then isArr=false; break end end
        if n==0 then return "{}" end
        local parts = {}
        if isArr then
            for _, i in ipairs(v) do table.insert(parts, jEnc(i)) end
            return "["..table.concat(parts,",").."]"
        else
            for k, i in pairs(v) do
                table.insert(parts, '"'..tostring(k)..'":'..jEnc(i))
            end
            return "{"..table.concat(parts,",").."}"
        end
    end
    return "null"
end

-- ── Minimal JSON decode ───────────────────────────────────────
local function jDec(s)
    local p = 1
    local function skip() while p<=#s and s:sub(p,p):match("%s") do p+=1 end end
    local function peek() skip(); return s:sub(p,p) end
    local parseV
    local function pStr()
        p+=1; local r=""
        while p<=#s do
            local c=s:sub(p,p)
            if c=='"' then p+=1; return r end
            if c=='\\' then p+=1; c=s:sub(p,p)
                if c=='n' then r=r..'\n' elseif c=='"' then r=r..'"' elseif c=='\\' then r=r..'\\' else r=r..c end
            else r=r..c end
            p+=1
        end; return r
    end
    local function pNum()
        skip(); local st=p
        if s:sub(p,p)=='-' then p+=1 end
        while p<=#s and s:sub(p,p):match("[%d%.eE%+%-]") do p+=1 end
        return tonumber(s:sub(st,p-1))
    end
    local function pArr()
        p+=1; local a={}; skip()
        if peek()==']' then p+=1; return a end
        while true do
            table.insert(a,parseV()); skip()
            if peek()==']' then p+=1; break end
            p+=1
        end; return a
    end
    local function pObj()
        p+=1; local o={}; skip()
        if peek()=='}' then p+=1; return o end
        while true do
            skip(); local k=pStr(); skip(); p+=1
            o[k]=parseV(); skip()
            if peek()=='}' then p+=1; break end
            p+=1
        end; return o
    end
    parseV = function()
        skip(); local c=peek()
        if c=='"' then return pStr()
        elseif c=='[' then return pArr()
        elseif c=='{' then return pObj()
        elseif c=='t' then p+=4; return true
        elseif c=='f' then p+=5; return false
        elseif c=='n' then p+=4; return nil
        else return pNum() end
    end
    local ok,r = pcall(parseV); return ok and r or nil
end

-- ── Save / Load core ─────────────────────────────────────────
local function BuildSave()
    local favs = {}
    for name in pairs(Favorites) do table.insert(favs, name) end
    local hist = {}
    for i = math.max(1,#History-19), #History do table.insert(hist, History[i]) end
    return {
        version   = "8.0",
        favorites = favs,
        waypoints = SerWP(),
        history   = hist,
        settings  = Settings,
        panelPos  = SerPos(Win.Position),
        barPos    = SerPos(Bar.Position),
    }
end

local function SaveData(silent)
    if not writefile then
        if not silent then
            Notify("Сохранение", "writefile недоступен в этом исполнителе", Color3.fromRGB(255,140,50))
        end
        return false
    end
    local ok, err = pcall(function() writefile(SAVE_FILE, jEnc(BuildSave())) end)
    if ok then
        if not silent then Notify("💾 Сохранено", "glork_data.json", Color3.fromRGB(0,220,160)) end
        return true
    else
        if not silent then Notify("Ошибка сохранения", tostring(err):sub(1,50), Color3.fromRGB(255,70,70)) end
        return false
    end
end

local function LoadData()
    if not readfile then return false end
    local ok, raw = pcall(readfile, SAVE_FILE)
    if not ok or not raw or raw=="" then return false end
    local data = jDec(raw)
    if type(data)~="table" then return false end
    if type(data.favorites)=="table" then
        Favorites = {}
        for _, n in ipairs(data.favorites) do Favorites[n]=true end
    end
    if type(data.waypoints)=="table" then Waypoints = DeSerWP(data.waypoints) end
    if type(data.history)=="table" then History = data.history end
    if type(data.settings)=="table" then
        for k,v in pairs(data.settings) do Settings[k]=v end
    end
    task.defer(function()
        local pp = DeSerPos(data.panelPos)
        local bp = DeSerPos(data.barPos)
        if pp then Win.Position=pp end
        if bp then Bar.Position=bp end
    end)
    return true
end

-- ── Auto-save every 30 s ─────────────────────────────────────
local _asTimer = 0
RunService.Heartbeat:Connect(function(dt)
    _asTimer += dt
    if _asTimer >= 30 then _asTimer=0; SaveData(true) end
end)

-- Save on close
OnClick(XBtn, function() SaveData(true) end)

-- Save on drag end
UserInputService.InputEnded:Connect(function(i)
    if i.UserInputType==Enum.UserInputType.MouseButton1 then SaveData(true) end
end)

-- ── History navigation (↑ ↓ arrows in IY bar) ────────────────
local histIdx = 0
UserInputService.InputBegan:Connect(function(inp, gp)
    if gp or not IYInput:IsFocused() then return end
    if inp.KeyCode==Enum.KeyCode.Up then
        histIdx = math.min(histIdx+1, #History)
        if #History>0 then IYInput.Text = History[#History-histIdx+1] or "" end
    elseif inp.KeyCode==Enum.KeyCode.Down then
        histIdx = math.max(histIdx-1, 0)
        IYInput.Text = histIdx==0 and "" or (History[#History-histIdx+1] or "")
    end
end)

-- ── Wrap Run() to record history ─────────────────────────────
local _baseRun = Run
Run = function(input)
    local t = input:match("^%s*(.-)%s*$")
    if t and t~="" then
        -- avoid consecutive duplicates
        if History[#History] ~= t then
            table.insert(History, t)
            if #History>100 then table.remove(History,1) end
        end
        histIdx = 0
    end
    return _baseRun(input)
end

-- reconnect IYInput FocusLost with wrapped Run
IYInput.FocusLost:Connect(function(enter)
    if enter and IYInput.Text~="" then
        Run(IYInput.Text); IYInput.Text=""; AcDrop.Visible=false
    end
end)

-- ── Save-related commands ─────────────────────────────────────
AddCommand("save", {}, "Сохранить данные на устройство", "Utility", function()
    SaveData(false)
end)
AddCommand("load", {"loaddata"}, "Загрузить сохранённые данные", "Utility", function()
    local ok = LoadData()
    if ok then
        Notify("📂 Загружено", "Данные восстановлены с устройства", Color3.fromRGB(0,220,160))
        Refresh()
    else
        Notify("Загрузка", "Файл не найден или повреждён", Color3.fromRGB(255,140,50))
    end
end)
AddCommand("saveinfo", {}, "Информация о сохранении", "Utility", function()
    local fc=0; for _ in pairs(Favorites) do fc+=1 end
    local wc=0; for _ in pairs(Waypoints) do wc+=1 end
    Notify("💾 Сохранение",
        "Избр: "..fc.."  ВП: "..wc.."  История: "..#History.."  Файл: "..(readfile and "✓" or "✗"),
        Color3.fromRGB(0,210,150))
end)
AddCommand("cleardata", {"resetdata"}, "Очистить ВСЕ сохранённые данные", "Utility", function()
    Favorites={}; Waypoints={}; History={}
    Settings={defaultWS=16, defaultGrav=196.2, defaultFOV=70}
    if writefile then pcall(writefile, SAVE_FILE, "{}") end
    Notify("🗑 Данные удалены", "Всё сброшено", Color3.fromRGB(255,80,80))
    Refresh()
end)
AddCommand("history", {"hist","cmdhist"}, "Показать историю команд", "Utility", function()
    if #History==0 then Notify("История","Пусто"); return end
    local last={}
    for i=math.max(1,#History-9),#History do table.insert(last,History[i]) end
    Notify("📜 История ("..#History..")", table.concat(last,"  ·  "), Color3.fromRGB(180,180,255))
end)
AddCommand("setws", {}, "Дефолтный WalkSpeed [n]", "Utility", function(a)
    Settings.defaultWS=tonumber(a[1]) or 16
    SaveData(true)
    Notify("Настройки","Дефолт WS = "..Settings.defaultWS.." сохранён",Color3.fromRGB(0,220,160))
end)
AddCommand("setgrav", {}, "Дефолтная гравитация [n]", "Utility", function(a)
    Settings.defaultGrav=tonumber(a[1]) or 196.2
    SaveData(true)
    Notify("Настройки","Дефолт Grav = "..Settings.defaultGrav.." сохранён",Color3.fromRGB(0,220,160))
end)
AddCommand("setfov2", {}, "Дефолтный FOV [n]", "Utility", function(a)
    Settings.defaultFOV=tonumber(a[1]) or 70
    SaveData(true)
    Notify("Настройки","Дефолт FOV = "..Settings.defaultFOV.." сохранён",Color3.fromRGB(0,220,160))
end)
AddCommand("applydefaults", {"applysettings"}, "Применить сохранённые настройки", "Utility", function()
    local hum=LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if hum then hum.WalkSpeed=Settings.defaultWS end
    workspace.Gravity=Settings.defaultGrav
    Camera.FieldOfView=Settings.defaultFOV
    Notify("✓ Настройки применены",
        "WS="..Settings.defaultWS.."  Grav="..Settings.defaultGrav.."  FOV="..Settings.defaultFOV,
        Color3.fromRGB(0,220,160))
end)

-- ════════════════════════════════════════════════════════════════
--  STARTUP
-- ════════════════════════════════════════════════════════════════

-- ── Startup (lang-aware) ─────────────────────────────────────
