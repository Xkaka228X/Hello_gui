local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local TweenService = game:GetService("TweenService")
local Players = game:GetService("Players")

local PlaceId = game.PlaceId
local JobId = game.JobId

-- === ЦВЕТОВАЯ ПАЛИТРА ===
local Theme = {
    Main = Color3.fromRGB(25, 25, 25),
    Accent = Color3.fromRGB(0, 162, 255),
    Success = Color3.fromRGB(0, 255, 127),
    Danger = Color3.fromRGB(255, 70, 70),
    Text = Color3.fromRGB(255, 255, 255),
    SecondaryText = Color3.fromRGB(180, 180, 180)
}

-- === НАСТРОЙКИ ЯЗЫКА ===
local Lang = "RU"
local Phrases = {
    RU = {Main = "🌐 SERVER HOP", Min = "МАЛО", Max = "МНОГО", Rand = "РАНДОМ", Refresh = "ОБНОВИТЬ", Confirm = "ЗАЙТИ?", Yes = "ДА", No = "НЕТ", Join = "Захожу..."},
    EN = {Main = "🌐 SERVER HOP", Min = "MIN", Max = "MAX", Rand = "RANDOM", Refresh = "REFRESH", Confirm = "JOIN?", Yes = "YES", No = "NO", Join = "Joining..."}
}

-- === ИНТЕРФЕЙС ===
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "FinalCleanHop"
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.ResetOnSpawn = false

local MainButton = Instance.new("TextButton")
MainButton.Size = UDim2.new(0, 130, 0, 40)
MainButton.Position = UDim2.new(0.5, -65, 0.05, 0)
MainButton.BackgroundColor3 = Theme.Main
MainButton.Text = Phrases[Lang].Main
MainButton.TextColor3 = Theme.Text
MainButton.Font = Enum.Font.GothamBold
MainButton.Parent = ScreenGui
Instance.new("UICorner", MainButton).CornerRadius = UDim.new(0, 10)
Instance.new("UIStroke", MainButton).Color = Theme.Accent

local Holder = Instance.new("Frame")
Holder.Size = UDim2.new(0, 280, 0, 380)
Holder.Position = UDim2.new(0.5, -140, 0.2, 0)
Holder.BackgroundColor3 = Theme.Main
Holder.Visible = false
Holder.ClipsDescendants = true
Holder.Parent = ScreenGui
Instance.new("UICorner", Holder).CornerRadius = UDim.new(0, 12)

-- Управление (Верхняя панель)
local TopPanel = Instance.new("Frame")
TopPanel.Size = UDim2.new(1, 0, 0, 80)
TopPanel.BackgroundTransparency = 1
TopPanel.Parent = Holder

local LangBtn = Instance.new("TextButton")
LangBtn.Size = UDim2.new(0, 35, 0, 25)
LangBtn.Position = UDim2.new(0, 10, 0, 10)
LangBtn.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
LangBtn.Text = Lang
LangBtn.TextColor3 = Theme.Text
LangBtn.Font = Enum.Font.GothamBold
LangBtn.Parent = TopPanel
Instance.new("UICorner", LangBtn)

local RefreshBtn = Instance.new("TextButton")
RefreshBtn.Size = UDim2.new(0, 100, 0, 25)
RefreshBtn.Position = UDim2.new(1, -110, 0, 10)
RefreshBtn.BackgroundColor3 = Theme.Accent
RefreshBtn.Text = "🔄 " .. Phrases[Lang].Refresh
RefreshBtn.TextColor3 = Theme.Text
RefreshBtn.Font = Enum.Font.GothamBold
RefreshBtn.TextSize = 12
RefreshBtn.Parent = TopPanel
Instance.new("UICorner", RefreshBtn)

-- Кнопки категорий
local MinBtn = Instance.new("TextButton")
MinBtn.Size = UDim2.new(0.31, 0, 0, 30)
MinBtn.Position = UDim2.new(0, 10, 0, 45)
MinBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
MinBtn.Text = Phrases[Lang].Min
MinBtn.TextColor3 = Theme.Text
MinBtn.Font = Enum.Font.GothamBold
MinBtn.Parent = TopPanel
Instance.new("UICorner", MinBtn)

local MaxBtn = MinBtn:Clone()
MaxBtn.Position = UDim2.new(0.345, 10, 0, 45)
MaxBtn.Text = Phrases[Lang].Max
MaxBtn.Parent = TopPanel

local RandBtn = MinBtn:Clone()
RandBtn.Position = UDim2.new(0.69, 10, 0, 45)
RandBtn.Text = Phrases[Lang].Rand
RandBtn.BackgroundColor3 = Color3.fromRGB(80, 40, 120)
RandBtn.Parent = TopPanel

-- Список
local Scroll = Instance.new("ScrollingFrame")
Scroll.Size = UDim2.new(1, -10, 1, -90)
Scroll.Position = UDim2.new(0, 5, 0, 85)
Scroll.BackgroundTransparency = 1
Scroll.ScrollBarThickness = 2
Scroll.Parent = Holder
Instance.new("UIListLayout", Scroll).Padding = UDim.new(0, 5)

-- Окно подтверждения
local ConfirmFrame = Instance.new("Frame")
ConfirmFrame.Size = UDim2.new(1, 0, 1, 0)
ConfirmFrame.BackgroundColor3 = Color3.new(0,0,0)
ConfirmFrame.BackgroundTransparency = 0.5
ConfirmFrame.Visible = false
ConfirmFrame.ZIndex = 10
ConfirmFrame.Parent = Holder

local Pop = Instance.new("Frame")
Pop.Size = UDim2.new(0.8, 0, 0, 100)
Pop.Position = UDim2.new(0.1, 0, 0.35, 0)
Pop.BackgroundColor3 = Theme.Main
Pop.Parent = ConfirmFrame
Instance.new("UICorner", Pop)
Instance.new("UIStroke", Pop).Color = Theme.Accent

local PopText = Instance.new("TextLabel")
PopText.Size = UDim2.new(1, 0, 0.5, 0)
PopText.Text = Phrases[Lang].Confirm
PopText.TextColor3 = Theme.Text
PopText.Font = Enum.Font.GothamBold
PopText.BackgroundTransparency = 1
PopText.Parent = Pop

local Yes = Instance.new("TextButton")
Yes.Size = UDim2.new(0.4, 0, 0, 30)
Yes.Position = UDim2.new(0.08, 0, 0.6, 0)
Yes.BackgroundColor3 = Theme.Success
Yes.Text = Phrases[Lang].Yes
Yes.Font = Enum.Font.GothamBold
Yes.Parent = Pop
Instance.new("UICorner", Yes)

local No = Yes:Clone()
No.Position = UDim2.new(0.52, 0, 0.6, 0)
No.BackgroundColor3 = Theme.Danger
No.Text = Phrases[Lang].No
No.Parent = Pop

-- === ЛОГИКА ===
local allServers = {}
local targetId = ""

local function Fetch()
    allServers = {}
    local success, result = pcall(function()
        return HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?limit=100")).data
    end)
    if success then
        for _, s in pairs(result) do if s.id ~= JobId then table.insert(allServers, s) end end
    end
end

local function Render(mode)
    for _, v in pairs(Scroll:GetChildren()) do if v:IsA("TextButton") then v:Destroy() end end
    if mode == "Min" then table.sort(allServers, function(a,b) return a.playing < b.playing end)
    else table.sort(allServers, function(a,b) return a.playing > b.playing end) end

    for _, s in pairs(allServers) do
        local b = Instance.new("TextButton")
        b.Size = UDim2.new(1, -10, 0, 40)
        b.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
        b.Text = "👤 " .. s.playing .. " / " .. s.maxPlayers .. "   |   📶 " .. (s.ping or "?") .. "ms"
        b.TextColor3 = Theme.SecondaryText
        b.Font = Enum.Font.GothamSemibold
        b.Parent = Scroll
        Instance.new("UICorner", b)
        b.MouseButton1Click:Connect(function() targetId = s.id ConfirmFrame.Visible = true end)
    end
    Scroll.CanvasSize = UDim2.new(0,0,0,#allServers * 45)
end

MainButton.MouseButton1Click:Connect(function()
    Holder.Visible = not Holder.Visible
    if Holder.Visible then Fetch() Render("Min") end
end)

RefreshBtn.MouseButton1Click:Connect(function() Fetch() Render("Min") end)
MinBtn.MouseButton1Click:Connect(function() Render("Min") end)
MaxBtn.MouseButton1Click:Connect(function() Render("Max") end)
RandBtn.MouseButton1Click:Connect(function()
    if #allServers > 0 then targetId = allServers[math.random(1, #allServers)].id ConfirmFrame.Visible = true end
end)

Yes.MouseButton1Click:Connect(function() TeleportService:TeleportToPlaceInstance(PlaceId, targetId, Players.LocalPlayer) end)
No.MouseButton1Click:Connect(function() ConfirmFrame.Visible = false end)

LangBtn.MouseButton1Click:Connect(function()
    Lang = (Lang == "RU") and "EN" or "RU"
    LangBtn.Text = Lang
    MainButton.Text = Phrases[Lang].Main
    RefreshBtn.Text = "🔄 " .. Phrases[Lang].Refresh
    MinBtn.Text = Phrases[Lang].Min
    MaxBtn.Text = Phrases[Lang].Max
    RandBtn.Text = Phrases[Lang].Rand
    PopText.Text = Phrases[Lang].Confirm
    Yes.Text = Phrases[Lang].Yes
    No.Text = Phrases[Lang].No
end)

-- Плавный Drag
local d, s, sp
MainButton.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then d=true s=i.Position sp=MainButton.Position end end)
UserInputService.InputChanged:Connect(function(i) if d and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
    local delta = i.Position - s
    MainButton.Position = UDim2.new(sp.X.Scale, sp.X.Offset + delta.X, sp.Y.Scale, sp.Y.Offset + delta.Y)
    Holder.Position = UDim2.new(MainButton.Position.X.Scale, MainButton.Position.X.Offset - 75, MainButton.Position.Y.Scale, MainButton.Position.Y.Offset + 50)
end end)
MainButton.InputEnded:Connect(function() d=false end)
