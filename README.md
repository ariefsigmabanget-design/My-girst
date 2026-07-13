-- GUI utama
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 320, 0, 240)
MainFrame.Position = UDim2.new(0.5, -160, 0.5, -120)
MainFrame.BackgroundColor3 = Color3.fromRGB(0,0,0)
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0,20)
UICorner.Parent = MainFrame

-- Tombol hapus GUI
local CloseButton = Instance.new("TextButton")
CloseButton.Size = UDim2.new(0, 30, 0, 30)
CloseButton.Position = UDim2.new(1, -35, 0, 5)
CloseButton.Text = "X"
CloseButton.BackgroundColor3 = Color3.fromRGB(200,0,0)
CloseButton.TextColor3 = Color3.fromRGB(255,255,255)
CloseButton.Parent = MainFrame

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0,8)
CloseCorner.Parent = CloseButton

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

-- Tombol Server Hop manual
local HopButton = Instance.new("TextButton")
HopButton.Size = UDim2.new(0, 120, 0, 40)
HopButton.Position = UDim2.new(0.2, -60, 0.1, -20)
HopButton.Text = "Server Hop"
HopButton.BackgroundColor3 = Color3.fromRGB(0,0,0)
HopButton.TextColor3 = Color3.fromRGB(255,255,255)
HopButton.Parent = MainFrame

local BtnCorner1 = Instance.new("UICorner")
BtnCorner1.CornerRadius = UDim.new(0,15)
BtnCorner1.Parent = HopButton

-- Frame status
local StatusFrame = Instance.new("Frame")
StatusFrame.Size = UDim2.new(0, 300, 0, 40)
StatusFrame.Position = UDim2.new(0.5, -150, 0.3, -20)
StatusFrame.BackgroundColor3 = Color3.fromRGB(0,0,139)
StatusFrame.Parent = MainFrame

local StatusCorner = Instance.new("UICorner")
StatusCorner.CornerRadius = UDim.new(0,10)
StatusCorner.Parent = StatusFrame

local StatusLabel = Instance.new("TextLabel")
StatusLabel.Size = UDim2.new(1, -10, 1, -10)
StatusLabel.Position = UDim2.new(0,5,0,5)
StatusLabel.Text = "Ready..."
StatusLabel.TextScaled = true
StatusLabel.TextColor3 = Color3.fromRGB(255,255,255)
StatusLabel.Parent = StatusFrame

-- Catatan scroll
local NoteFrame = Instance.new("ScrollingFrame")
NoteFrame.Size = UDim2.new(0, 300, 0, 100)
NoteFrame.Position = UDim2.new(0.5, -150, 0.55, 0)
NoteFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
NoteFrame.ScrollBarThickness = 8
NoteFrame.CanvasSize = UDim2.new(0,0,0,0)
NoteFrame.Parent = MainFrame

local NoteCorner = Instance.new("UICorner")
NoteCorner.CornerRadius = UDim.new(0,10)
NoteCorner.Parent = NoteFrame

local NoteLabel = Instance.new("TextLabel")
NoteLabel.Size = UDim2.new(1, -10, 0, 200)
NoteLabel.Position = UDim2.new(0,5,0,5)
NoteLabel.TextWrapped = true
NoteLabel.TextScaled = true
NoteLabel.TextColor3 = Color3.fromRGB(0,0,139)
NoteLabel.Text = "First, you have to press Server hop first to process to enter the game or to someone else's server and then you activate Auto Process shift mode and it will automatically go to another server or if the server is not good, you can directly use server hop."
NoteLabel.Parent = NoteFrame

NoteFrame.CanvasSize = UDim2.new(0,0,0,NoteLabel.AbsoluteSize.Y+20)

-- Tombol hapus catatan
local DeleteNote = Instance.new("TextButton")
DeleteNote.Size = UDim2.new(0, 100, 0, 30)
DeleteNote.Position = UDim2.new(0.5, -50, 0.85, -15)
DeleteNote.Text = "Clear Text"
DeleteNote.BackgroundColor3 = Color3.fromRGB(0,0,0)
DeleteNote.TextColor3 = Color3.fromRGB(255,255,255)
DeleteNote.Parent = MainFrame

local DelCorner = Instance.new("UICorner")
DelCorner.CornerRadius = UDim.new(0,10)
DelCorner.Parent = DeleteNote

-- Credits
local Credits = Instance.new("TextLabel")
Credits.Size = UDim2.new(0, 320, 0, 25)
Credits.Position = UDim2.new(0, 0, 1, -25)
Credits.Text = "Credits: VersMandex🇮🇩"
Credits.TextColor3 = Color3.fromRGB(0,0,139)
Credits.Font = Enum.Font.Arcade
Credits.TextScaled = true
Credits.Parent = MainFrame

local Stroke = Instance.new("UIStroke")
Stroke.Color = Color3.fromRGB(0,0,0)
Stroke.Thickness = 2
Stroke.Parent = Credits

-- Label countdown timer
local TimerLabel = Instance.new("TextLabel")
TimerLabel.Size = UDim2.new(0, 200, 0, 50)
TimerLabel.Position = UDim2.new(0.5, -100, 0.75, -25)
TimerLabel.BackgroundColor3 = Color3.fromRGB(0,0,0)
TimerLabel.TextColor3 = Color3.fromRGB(255,255,255)
TimerLabel.TextScaled = true
TimerLabel.Text = "05:00"
TimerLabel.Parent = MainFrame

local TimerCorner = Instance.new("UICorner")
TimerCorner.CornerRadius = UDim.new(0,10)
TimerCorner.Parent = TimerLabel

-- Teleport logic
local HttpService = game:GetService("HttpService")
local TeleportService = game:GetService("TeleportService")
local PlaceId = game.PlaceId

local function teleportNow()
    StatusFrame.BackgroundColor3 = Color3.fromRGB(173,216,230)
    StatusLabel.Text = "Teleporting..."
    local servers = HttpService:JSONDecode(game:HttpGet(
        "https://games.roblox.com/v1/games/"..PlaceId.."/servers/Public?sortOrder=Asc&limit=100"
    ))
    for _, server in pairs(servers.data) do
        if server.playing < server.maxPlayers then
            TeleportService:TeleportToPlaceInstance(PlaceId, server.id, game.Players.LocalPlayer)
            break
        end
    end
end

-- Tombol manual
HopButton.MouseButton1Click:Connect(function()
    teleportNow()
end)

-- Countdown 5 menit otomatis
task.spawn(function()
    local totalTime = 300 -- 5 menit
    for t = totalTime, 1, -1 do
        local minutes = math.floor(t / 60)
        local seconds = t % 60
        TimerLabel.Text = string.format("%02d:%02d", minutes, seconds)
        StatusLabel.Text = "Auto hop dalam "..TimerLabel.Text
        task.wait(1)
    end
    teleportNow()
end)

-- Hapus catatan
DeleteNote.MouseButton1Click:Connect(function()
    NoteFrame:Destroy()
end)
