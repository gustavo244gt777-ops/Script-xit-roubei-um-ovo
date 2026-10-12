
-- EGG XIT PRO MAX
-- Interface de demonstração para Roblox Studio
-- Não modifica nem explora jogos de terceiros.
-- Os ovos e as raridades são dados locais de teste.

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local old = playerGui:FindFirstChild("EggXitProMax")
if old then old:Destroy() end

local C = {
    bg = Color3.fromRGB(12, 14, 22),
    panel = Color3.fromRGB(22, 25, 38),
    green = Color3.fromRGB(0, 255, 160),
    blue = Color3.fromRGB(60, 170, 255),
    purple = Color3.fromRGB(180, 90, 255),
    white = Color3.fromRGB(240, 245, 255),
    muted = Color3.fromRGB(145, 155, 175)
}

local eggs = 0
local rareAlerts = true
local glowEnabled = true
local currentTab = "INÍCIO"

local gui = Instance.new("ScreenGui")
gui.Name = "EggXitProMax"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.DisplayOrder = 50
gui.Parent = playerGui

local function make(class, props, parent)
    local obj = Instance.new(class)
    for k, v in pairs(props) do
        obj[k] = v
    end
    obj.Parent = parent
    return obj
end

local function corner(obj, radius)
    make("UICorner", {
        CornerRadius = UDim.new(0, radius or 10)
    }, obj)
end

local function stroke(obj, color, thickness)
    return make("UIStroke", {
        Color = color,
        Thickness = thickness or 1,
        Transparency = 0.15
    }, obj)
end

local function tween(obj, props, duration)
    local t = TweenService:Create(
        obj,
        TweenInfo.new(
            duration or 0.2,
            Enum.EasingStyle.Quint,
            Enum.EasingDirection.Out
        ),
        props
    )
    t:Play()
    return t
end

local function label(parent, text, size, pos, color)
    return make("TextLabel", {
        BackgroundTransparency = 1,
        Text = text,
        TextColor3 = color or C.white,
        TextSize = size,
        Font = Enum.Font.GothamMedium,
        Position = pos,
        Size = UDim2.new(1, 0, 0, 30),
        TextXAlignment = Enum.TextXAlignment.Left
    }, parent)
end

-- Botão flutuante
local float = make("TextButton", {
    Name = "FloatingButton",
    Size = UDim2.fromOffset(56, 56),
    Position = UDim2.new(0, 18, 0.5, -28),
    BackgroundColor3 = C.bg,
    Text = "EGG",
    TextColor3 = C.green,
    TextSize = 16,
    Font = Enum.Font.GothamBlack,
    AutoButtonColor = false,
    ZIndex = 10
}, gui)
corner(float, 18)
local floatStroke = stroke(float, C.green, 2)

-- Janela principal
local main = make("Frame", {
    Name = "MainPanel",
    Size = UDim2.new(0.88, 0, 0.72, 0),
    Position = UDim2.new(0.5, 0, 0.5, 0),
    AnchorPoint = Vector2.new(0.5, 0.5),
    BackgroundColor3 = C.bg,
    Visible = false,
    ClipsDescendants = true
}, gui)
main.Size = UDim2.new(0, 0, 0, 0)
corner(main, 16)
local mainStroke = stroke(main, C.green, 1.5)

make("UISizeConstraint", {
    MinSize = Vector2.new(280, 300),
    MaxSize = Vector2.new(520, 540)
}, main)

-- Barra superior
local top = make("Frame", {
    Size = UDim2.new(1, 0, 0, 54),
    BackgroundColor3 = C.panel,
    BorderSizePixel = 0
}, main)
corner(top, 14)

label(top, "EGG XIT PRO MAX", 17,
    UDim2.new(0, 14, 0, 5), C.green)

local subtitle = make("TextLabel", {
    BackgroundTransparency = 1,
    Text = "LAB  //  TEST BUILD",
    TextColor3 = C.muted,
    TextSize = 9,
    Font = Enum.Font.Gotham,
    Position = UDim2.new(0, 15, 0, 29),
    Size = UDim2.new(0, 150, 0, 14),
    TextXAlignment = Enum.TextXAlignment.Left
}, top)

local minimize = make("TextButton", {
    Text = "—",
    TextSize = 22,
    Font = Enum.Font.GothamBold,
    TextColor3 = C.white,
    BackgroundColor3 = C.bg,
    Size = UDim2.fromOffset(36, 32),
    Position = UDim2.new(1, -78, 0, 11),
    AutoButtonColor = false
}, top)
corner(minimize, 8)

local close = make("TextButton", {
    Text = "×",
    TextSize = 23,
    Font = Enum.Font.GothamBold,
    TextColor3 = Color3.fromRGB(255, 100, 125),
    BackgroundColor3 = C.bg,
    Size = UDim2.fromOffset(32, 32),
    Position = UDim2.new(1, -38, 0, 11),
    AutoButtonColor = false
}, top)
corner(close, 8)

-- Navegação
local nav = make("Frame", {
    Position = UDim2.new(0, 10, 0, 64),
    Size = UDim2.new(1, -20, 0, 38),
    BackgroundTransparency = 1
}, main)

make("UIListLayout", {
    FillDirection = Enum.FillDirection.Horizontal,
    Padding = UDim.new(0, 5),
    SortOrder = Enum.SortOrder.LayoutOrder
}, nav)

local content = make("ScrollingFrame", {
    Position = UDim2.new(0, 10, 0, 110),
    Size = UDim2.new(1, -20, 1, -120),
    BackgroundTransparency = 1,
    BorderSizePixel = 0,
    ScrollBarThickness = 3,
    ScrollBarImageColor3 = C.green,
    CanvasSize = UDim2.new(0, 0, 0, 0),
    AutomaticCanvasSize = Enum.AutomaticSize.Y
}, main)

make("UIListLayout", {
    Padding = UDim.new(0, 9),
    SortOrder = Enum.SortOrder.LayoutOrder
}, content)

make("UIPadding", {
    PaddingBottom = UDim.new(0, 12),
    PaddingRight = UDim.new(0, 4)
}, content)

local function clearContent()
    for _, v in ipairs(content:GetChildren()) do
        if v:IsA("GuiObject") then
            v:Destroy()
        end
    end
end

local function card(height)
    local f = make("Frame", {
        Size = UDim2.new(1, -4, 0, height),
        BackgroundColor3 = C.panel,
        BorderSizePixel = 0
    }, content)
    corner(f, 12)
    stroke(f, Color3.fromRGB(48, 56, 76), 1)
    return f
end

local function actionButton(parent, text, pos, size, color, callback)
    local b = make("TextButton", {
        Text = text,
        TextSize = 12,
        Font = Enum.Font.GothamBold,
        TextColor3 = C.bg,
        BackgroundColor3 = color,
        Position = pos,
        Size = size,
        AutoButtonColor = false
    }, parent)
    corner(b, 9)

    b.Activated:Connect(function()
        tween(b, {BackgroundTransparency = 0.25}, 0.08)
        task.delay(0.09, function()
            if b.Parent then
                tween(b, {BackgroundTransparency = 0}, 0.12)
            end
        end)
        callback()
    end)

    return b
end

local function showHome()
    clearContent()

    local stats = card(100)
    label(stats, "OVOS DE TESTE", 11,
        UDim2.new(0, 13, 0, 10), C.muted)

    local countLabel = label(stats, tostring(eggs), 34,
        UDim2.new(0, 13, 0, 34), C.green)
    countLabel.Font = Enum.Font.GothamBlack

    label(stats, "CONTADOR LOCAL", 10,
        UDim2.new(0, 100, 0, 47), C.muted)

    local controls = card(105)
    label(controls, "SIMULADOR DE COLETA", 12,
        UDim2.new(0, 13, 0, 8), C.white)

    actionButton(controls, "+1 OVO",
        UDim2.new(0, 12, 0, 43),
        UDim2.new(0.48, -14, 0, 38),
        C.green, function()
            eggs += 1
            showHome()
        end)

    actionButton(controls, "ZERAR",
        UDim2.new(0.52, 0, 0, 43),
        UDim2.new(0.48, -12, 0, 38),
        C.blue, function()
            eggs = 0
            showHome()
        end)

    local info = card(76)
    label(info, "STATUS DO PAINEL", 11,
        UDim2.new(0, 13, 0, 8), C.muted)
    label(info, "● Interface ativa  •  Dados locais",
        12, UDim2.new(0, 13, 0, 35), C.green)

    local rare = card(100)
    label(rare, "ALERTA DE RARIDADE (DEMO)", 11,
        UDim2.new(0, 13, 0, 8), C.purple)

    actionButton(rare, "TESTAR ALERTA RARO",
        UDim2.new(0, 12, 0, 43),
        UDim2.new(1, -24, 0, 38),
        C.purple, function()
            if rareAlerts then
                game:GetService("StarterGui"):SetCore(
                    "SendNotification", {
                        Title = "EGG XIT PRO MAX",
                        Text = "Ovo lendário detectado! (SIMULAÇÃO)",
                        Duration = 4
                    }
                )
            end
        end)
end

local function showSettings()
    clearContent()

    local box = card(105)
    label(box, "CONFIGURAÇÕES", 13,
        UDim2.new(0, 13, 0, 8), C.white)

    local toggle = actionButton(box,
        "Alertas raros: " .. (rareAlerts and "ON" or "OFF"),
        UDim2.new(0, 12, 0, 48),
        UDim2.new(1, -24, 0, 38),
        rareAlerts and C.green or C.muted, function()
            rareAlerts = not rareAlerts
            showSettings()
        end)

    local box2 = card(105)
    label(box2, "EFEITO NEON", 13,
        UDim2.new(0, 13, 0, 8), C.white)

    actionButton(box2,
        "Brilho: " .. (glowEnabled and "ON" or "OFF"),
        UDim2.new(0, 12, 0, 48),
        UDim2.new(1, -24, 0, 38),
        C.blue, function()
            glowEnabled = not glowEnabled
            mainStroke.Transparency = glowEnabled and 0.15 or 0.8
            floatStroke.Transparency = glowEnabled and 0.15 or 0.8
            showSettings()
        end)
end

local function showAbout()
    clearContent()

    local box = card(145)
    label(box, "SOBRE O PROJETO", 13,
        UDim2.new(0, 13, 0, 10), C.green)

    local description = make("TextLabel", {
        BackgroundTransparency = 1,
        TextWrapped = true,
        TextYAlignment = Enum.TextYAlignment.Top,
        TextXAlignment = Enum.TextXAlignment.Left,
        Text = "EGG XIT PRO MAX\n\nInterface de demonstração para celular.\n\nContador: funcional com dados locais.\nAlertas: simulação manual.\nColeta real: não implementada.\nDetecção de ovos do jogo original: não implementada.",
        TextColor3 = C.white,
        TextSize = 12,
        Font = Enum.Font.Gotham,
        Position = UDim2.new(0, 13, 0, 40),
        Size = UDim2.new(1, -26, 0, 100)
    }, box)
end

local tabs = {"INÍCIO", "AJUSTES", "SOBRE"}

local function renderTab(tabName)
    currentTab = tabName

    for _, child in ipairs(nav:GetChildren()) do
        if child:IsA("TextButton") then
            local active = child.Name == tabName
            tween(child, {
                BackgroundColor3 = active and C.green or C.panel,
                TextColor3 = active and C.bg or C.white
            }, 0.15)
        end
    end

    if tabName == "INÍCIO" then
        showHome()
    elseif tabName == "AJUSTES" then
        showSettings()
    else
        showAbout()
    end
end

for _, tabName in ipairs(tabs) do
    local b = make("TextButton", {
        Name = tabName,
        Text = tabName,
        TextSize = 10,
        Font = Enum.Font.GothamBold,
        TextColor3 = C.white,
        BackgroundColor3 = C.panel,
        Size = UDim2.new(1 / 3, -4, 1, 0),
        AutoButtonColor = false
    }, nav)
    corner(b, 8)
    b.Activated:Connect(function()
        renderTab(tabName)
    end)
end

-- Abrir e fechar
local isOpen = false

local function setOpen(value)
    isOpen = value

    if value then
        main.Visible = true
        main.Size = UDim2.new(0, 0, 0, 0)
        tween(main, {
            Size = UDim2.new(0.88, 0, 0.72, 0)
        }, 0.25)
    else
        local t = tween(main, {
            Size = UDim2.new(0, 0, 0, 0)
        }, 0.18)
        t.Completed:Connect(function()
            if not isOpen then
                main.Visible = false
            end
        end)
    end
end

float.Activated:Connect(function()
    setOpen(not isOpen)
end)

close.Activated:Connect(function()
    setOpen(false)
end)

minimize.Activated:Connect(function()
    setOpen(false)
end)

-- Arrastar a janela com mouse ou toque
local dragging = false
local dragStart
local startPos
local dragInput

top.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1
        or input.UserInputType == Enum.UserInputType.Touch then

        dragging = true
        dragStart = input.Position
        startPos = main.Position
        dragInput = input

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if dragging and (
        input.UserInputType == Enum.UserInputType.MouseMovement
        or input.UserInputType == Enum.UserInputType.Touch
    ) then
        local delta = input.Position - dragStart
        main.Position = UDim2.new(
            startPos.X.Scale,
            startPos.X.Offset + delta.X,
            startPos.Y.Scale,
            startPos.Y.Offset + delta.Y
        )
    end
end)

-- Animação decorativa do botão flutuante
task.spawn(function()
    while gui.Parent do
        tween(floatStroke, {Transparency = 0.55}, 0.8)
        task.wait(0.8)
        tween(floatStroke, {Transparency = 0.05}, 0.8)
        task.wait(0.8)
    end
end)

renderTab("INÍCIO")

print("EGG XIT PRO MAX carregado!")
