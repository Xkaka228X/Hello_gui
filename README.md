local UserInputService = game:GetService("UserInputService")
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local Players = game:GetService("Players")

local PlaceId = game.PlaceId
local JobId = game.JobId
local FileName = "SavedServers_" .. PlaceId .. ".json"

-- === НАСТРОЙКИ И ТЕМЫ ===
local Lang = "RU"
local Theme = {
    Main = Color3.fromRGB(20, 20, 20),
    Accent = Color3.fromRGB(0, 162, 255),
    Success = Color3.fromRGB(0, 255, 127),
    Danger = Color3.fromRGB(255, 70, 70),
    Text = Color3.fromRGB(255, 255, 255),
    Secondary = Color3.fromRGB(35, 35, 35)
}

local Phrases = {
    RU = {Main = "🌐 СЕРВЕРЫ", Min = "МАЛО", Max = "МНОГО", Rand = "РАНДОМ", Refresh = "ОБНОВИТЬ", TabPub = "ОБЩИЕ", TabSave = "ИЗБРАННОЕ", Confirm = "ЗАЙТИ?", Yes = "ДА", No = "НЕТ"},
    EN = {Main = "🌐 SERVERS", Min = "MIN", Max = "MAX", Rand = "RAND", Refresh = "REFRESH", TabPub = "PUBLIC", TabSave = "SAVED", Confirm = "JOIN?", Yes = "YES", No = "NO"}
}

-- === СИСТЕМА СОХРАНЕНИЯ ===
local SavedData = {}
local function SaveToFile() writefile(FileName, HttpService:JSONEncode(SavedData)) end
local function LoadSaved() 
    if isfile(FileName) then 
        local s, d = pcall(function() return HttpService:JSONDecode(readfile(FileName)) end)
        if s then SavedData = d end
    end 
end
LoadSaved()

-- === ИНТЕРФЕЙС ===
local ScreenGui = Instance.new("ScreenGui", game:GetService("CoreGui"))
ScreenGui.Name = "Gemini_Ultimate_Hub"

local MainButton = Instance.new("TextButton", ScreenGui)
MainButton.Size = UDim2.new(0, 130, 0, 45)
MainButton.Position = UDim2.new(0.5, -65, 0.05, 0)
MainButton.BackgroundColor3 = Theme.Main
MainButton.Text = Phrases[Lang].Main
MainButton.TextColor3 = Theme.Text
MainButton.Font = Enum.Font.GothamBold
Instance.new("UICorner", MainButton)
Instance.new("UIStroke", MainButton).Color = Theme.Accent

local Holder = Instance.new("Frame", ScreenGui)
Holder.Size = UDim2.new(0, 300, 0, 450)
Holder.Position = UDim2.new(0.5, -150, 0.2, 0)
Holder.BackgroundColor3 = Theme.Main
Holder.Visible = false
Instance.new("UICorner", Holder)

-- ВКЛАДКИ
local Tabs = Instance.new("Frame", Holder)
Tabs.Size = UDim2.new(1, 0, 0, 40)
Tabs.BackgroundTransparency = 1

local PubBtn = Instance.new("TextButton", Tabs)
PubBtn.Size = UDim2.new(0.5, 0, 1, 0)
PubBtn.Text = Phrases[Lang].TabPub
PubBtn.BackgroundColor3 = Theme.Secondary
PubBtn.TextColor3 = Theme.Text
PubBtn.Font = Enum.Font.GothamBold

local SavBtn = PubBtn:Clone()
SavBtn.Parent = Tabs
SavBtn.Position = UDim2.new(0.5, 0, 0, 0)
SavBtn.Text = Phrases[Lang].TabSave
SavBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)

-- ПАНЕЛЬ УПРАВЛЕНИЯ (Refresh, Lang, Sort, Rand)
local Controls = Instance.new("Frame", Holder)
Controls.Size = UDim2.new(1, 0, 0, 90)
Controls.Position = UDim2.new(0, 0, 0, 40)
Controls.BackgroundTransparency = 1

local RefreshBtn = Instance.new("TextButton", Controls)
RefreshBtn.Size = UDim2.new(0.7, -10, 0, 30)
RefreshBtn.Position = UDim2.new(0, 10, 0, 5)
RefreshBtn.BackgroundColor3 = Theme.Accent
RefreshBtn.Text = "🔄 " .. Phrases[Lang].Refresh
RefreshBtn.Font = Enum.Font.GothamBold
RefreshBtn.TextColor3 = Theme.Text
Instance.new("UICorner", RefreshBtn)

local LangBtn = RefreshBtn:Clone()
LangBtn.Parent = Controls
LangBtn.Size = UDim2.new(0.3, -10, 0, 30)
LangBtn.Position = UDim2.new(0.7, 5, 0, 5)
LangBtn.BackgroundColor3 = Theme.Secondary
LangBtn.Text = Lang

local MinBtn = RefreshBtn:Clone()
MinBtn.Parent = Controls
MinBtn.Size = UDim2.new(0.31, 0, 0, 35)
MinBtn.Position = UDim2.new(0, 10, 0, 40)
MinBtn.Text = Phrases[Lang].Min
MinBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)

local MaxBtn = MinBtn:Clone()
MaxBtn.Parent = Controls
MaxBtn.Position = UDim2.new(0.345, 10, 0, 40)
MaxBtn.Text = Phrases[Lang].Max

local RandBtn = MinBtn:Clone()
RandBtn.Parent = Controls
RandBtn.Position = UDim2.new(0.69, 10, 0, 40)
RandBtn.Text = Phrases[Lang].Rand
RandBtn.BackgroundColor3 = Color3.fromRGB(80, 40, 120)

-- СПИСКИ
local PubScroll = Instance.new("ScrollingFrame", Holder)
PubScroll.Size = UDim2.new(1, -10, 1, -140)
PubScroll.Position = UDim2.new(0, 5, 0, 130)
PubScroll.BackgroundTransparency = 1
Instance.new("UIListLayout", PubScroll).Padding = UDim.new(0, 5)

local SavScroll = PubScroll:Clone()
SavScroll.Parent = Holder
SavScroll.Visible = false

-- ПОДТВЕРЖДЕНИЕ
local ConfFrame = Instance.new("Frame", Holder)
ConfFrame.Size = UDim2.new(1, 0, 1, 0)
ConfFrame.BackgroundColor3 = Color3.new(0,0,0)
ConfFrame.BackgroundTransparency = 0.5
ConfFrame.Visible = false
ConfFrame.ZIndex = 10
Instance.new("UICorner", ConfFrame)

local Pop = Instance.new("Frame", ConfFrame)
Pop.Size = UDim2.new(0.7, 0, 0, 100)
Pop.Position = UDim2.new(0.15, 0, 0.4, 0)
Pop.BackgroundColor3 = Theme.Main
Instance.new("UICorner", Pop)
Instance.new("UIStroke", Pop).Color = Theme.Accent

local PopTxt = Instance.new("TextLabel", Pop)
PopTxt.Size = UDim2.new(1, 0, 0.5, 0)
PopTxt.Text = Phrases[Lang].Confirm
PopTxt.TextColor3 = Theme.Text
PopTxt.BackgroundTransparency = 1
PopTxt.Font = Enum.Font.GothamBold

local Yes = Instance.new("TextButton", Pop)
Yes.Size = UDim2.new(0.4, 0, 0, 30)
Yes.Position = UDim2.new(0.08, 0, 0.6, 0)
Yes.BackgroundColor3 = Theme.Success
Yes.Text = Phrases[Lang].Yes
Instance.new("UICorner", Yes)

local No = Yes:Clone()
No.Parent = Pop
No.Position = UDim2.new(0.52, 0, 0.6, 0)
No.BackgroundColor3 = Theme.Danger
No.Text = Phrases[Lang].No

-- === ЛОГИКА ===
local allServers = {}
local currentTarget = ""

local function CreateCard(s, parent, isSaved)
    local f = Instance.new("Frame", parent)
    f.Size = UDim2.new(1, -10, 0, 45)
    f.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    Instance.new("UICorner", f)

    local txt = Instance.new("TextLabel", f)
    txt.Size = UDim2.new(0.6, 0, 1, 0)
    txt.Position = UDim2.new(0, 10, 0, 0)
    txt.Text = "👤 " .. s.playing .. "/" .. s.maxPlayers .. " | " .. (s.ping or "?") .. "ms"
    txt.TextColor3 = Theme.Text
    txt.BackgroundTransparency = 1
    txt.TextXAlignment = Enum.TextXAlignment.Left

    local j = Instance.new("TextButton", f)
    j.Size = UDim2.new(0.2, 0, 0.7, 0)
    j.Position = UDim2.new(0.6, 0, 0.15, 0)
    j.BackgroundColor3 = Theme.Success
    j.Text = "JOIN"
    Instance.new("UICorner", j)
    j.MouseButton1Click:Connect(function() currentTarget = s.id ConfFrame.Visible = true end)

    local act = Instance.new("TextButton", f)
    act.Size = UDim2.new(0.15, 0, 0.7, 0)
    act.Position = UDim2.new(0.82, 0, 0.15, 0)
    act.BackgroundColor3 = isSaved and Theme.Danger or Theme.Secondary
    act.Text = isSaved and "X" or "⭐"
    Instance.new("UICorner", act)

    act.MouseButton1Click:Connect(function()
        if isSaved then SavedData[s.id] = nil f:Destroy() else SavedData[s.id] = s act.Text = "✅" end
        SaveToFile()
    end)
end

local function Render(mode)
    PubScroll:ClearAllChildren()
    Instance.new("UIListLayout", PubScroll).Padding = UDim.new(0, 5)
    if mode == "Min" then table.sort(allServers, function(a,b) return a.playing < b.playing end)
    elseif mode == "Max" then table.sort(allServers, function(a,b) return a.playing > b.playing end) end
    for _, s in pairs(allServers) do CreateCard(s, PubScroll, false) end
    PubScroll.CanvasSize = UDim2.new(0,0,0,#allServers * 50)
end

local function Fetch()
    allServers = {}
    local success, result = pcall(function()
        return HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?limit=50")).data
    end)
    if success then for _, s in pairs(result) do if s.id ~= JobId then table.insert(allServers, s) end end end
end

local function RenderSaved()
    SavScroll:ClearAllChildren()
    Instance.new("UIListLayout", SavScroll).Padding = UDim.new(0, 5)
    local count = 0
    for _, s in pairs(SavedData) do count = count + 1 CreateCard(s, SavScroll, true) end
    SavScroll.CanvasSize = UDim2.new(0,0,0,count * 50)
end

-- КНОПКИ
RefreshBtn.MouseButton1Click:Connect(function() Fetch() Render("Min") end)
MinBtn.MouseButton1Click:Connect(function() Render("Min") end)
MaxBtn.MouseButton1Click:Connect(function() Render("Max") end)
RandBtn.MouseButton1Click:Connect(function() 
    if #allServers > 0 then currentTarget = allServers[math.random(1, #allServers)].id ConfFrame.Visible = true end
end)

PubBtn.MouseButton1Click:Connect(function() 
    PubScroll.Visible = true SavScroll.Visible = false Controls.Visible = true
    PubBtn.BackgroundColor3 = Theme.Secondary SavBtn.BackgroundColor3 = Color3.fromRGB(25,25,25)
end)
SavBtn.MouseButton1Click:Connect(function() 
    PubScroll.Visible = false SavScroll.Visible = true Controls.Visible = false RenderSaved()
    SavBtn.BackgroundColor3 = Theme.Secondary PubBtn.BackgroundColor3 = Color3.fromRGB(25,25,25)
end)

LangBtn.MouseButton1Click:Connect(function()
    Lang = (Lang == "RU") and "EN" or "RU"
    MainButton.Text = Phrases[Lang].Main
    RefreshBtn.Text = "🔄 " .. Phrases[Lang].Refresh
    MinBtn.Text = Phrases[Lang].Min
    MaxBtn.Text = Phrases[Lang].Max
    RandBtn.Text = Phrases[Lang].Rand
    PubBtn.Text = Phrases[Lang].TabPub
    SavBtn.Text = Phrases[Lang].TabSave
    PopTxt.Text = Phrases[Lang].Confirm
    Yes.Text = Phrases[Lang].Yes
    No.Text = Phrases[Lang].No
    LangBtn.Text = Lang
end)

Yes.MouseButton1Click:Connect(function() TeleportService:TeleportToPlaceInstance(PlaceId, currentTarget, Players.LocalPlayer) end)
No.MouseButton1Click:Connect(function() ConfFrame.Visible = false end)
MainButton.MouseButton1Click:Connect(function() Holder.Visible = not Holder.Visible if Holder.Visible then Fetch() Render("Min") end end)

-- ПЛАВНЫЙ DRAG
local d, s, sp
MainButton.InputBegan:Connect(function(i) if i.UserInputType == Enum.UserInputType.MouseButton1 or i.UserInputType == Enum.UserInputType.Touch then d=true s=i.Position sp=MainButton.Position end end)
UserInputService.InputChanged:Connect(function(i) if d and (i.UserInputType == Enum.UserInputType.MouseMovement or i.UserInputType == Enum.UserInputType.Touch) then
    local delta = i.Position - s
    MainButton.Position = UDim2.new(sp.X.Scale, sp.X.Offset + delta.X, sp.Y.Scale, sp.Y.Offset + delta.Y)
    Holder.Position = UDim2.new(MainButton.Position.X.Scale, MainButton.Position.X.Offset - 85, MainButton.Position.Y.Scale, MainButton.Position.Y.Offset + 50)
end end)
MainButton.InputEnded:Connect(function() d=false end)
