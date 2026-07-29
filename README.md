--// OHM-EXD: GOD OF WAR (THE ABSOLUTE FINAL VERSION)
--// FULL BOT BEHAVIOR | NO VEHICLE | ANTI-36/0 | FULL RGB MASTER
--// AUDIO ID: 76975391456984 (START 20S)

local Players = game:GetService("Players")
local player = Players.LocalPlayer
local Camera = workspace.CurrentCamera
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

-- [Global Settings]
local Tracer_Color = Color3.fromRGB(0, 255, 0)
local ID_Label_Color = Color3.fromRGB(0, 255, 0)
local RGB_Tracer, RGB_ID = false, false
local botGuardianActive, aiDetectActive, deepBypassActive = false, false, false

-- ================== 🎬 INTRO DIVINE (ห้ามหาย ห้ามลืม!) ==================
local introGui = Instance.new("ScreenGui", player.PlayerGui); introGui.IgnoreGuiInset = true
local bg = Instance.new("Frame", introGui); bg.Size = UDim2.new(1.5,0,1.5,0); bg.Position = UDim2.new(-0.25,0,-0.25,0); bg.BackgroundColor3 = Color3.new(0,0,0)

local introSound = Instance.new("Sound", bg)
introSound.SoundId = "rbxassetid://76975391456984"
introSound.Volume = 2
introSound.TimePosition = 20
introSound:Play()

local koText = Instance.new("TextLabel", bg); koText.Size = UDim2.new(0,600,0,200); koText.Position = UDim2.new(0.5,-300,0.5,-150); koText.BackgroundTransparency = 1; koText.Text = "K.O"; koText.TextColor3 = Color3.new(1,0,0); koText.TextScaled = true; koText.Font = Enum.Font.LuckiestGuy
local exmText = Instance.new("TextLabel", bg); exmText.Size = UDim2.new(0,600,0,50); exmText.Position = UDim2.new(0.5,-300,0.5,50); exmText.BackgroundTransparency = 1; exmText.Text = ""; exmText.TextColor3 = Color3.new(0,1,0); exmText.TextSize = 30; exmText.Font = Enum.Font.Code
local playBtn = Instance.new("TextButton", bg); playBtn.Size = UDim2.new(0,350,0,80); playBtn.Position = UDim2.new(0.5,-175,0.75,0); playBtn.Text = "▶ ACTIVATE SUPREME BOT"; playBtn.BackgroundColor3 = Color3.fromRGB(40,0,0); playBtn.TextColor3 = Color3.new(1,1,1); playBtn.TextSize = 25; playBtn.Visible = false; Instance.new("UICorner", playBtn)

task.spawn(function()
    TweenService:Create(koText, TweenInfo.new(1.5), {TextTransparency = 0}):Play()
    task.wait(1.5)
    local fullText = "THE ABSOLUTE GOD VERSION: ONLINE..."
    for i = 1, #fullText do exmText.Text = string.sub(fullText, 1, i) task.wait(0.04) end
    task.wait(0.5); playBtn.Visible = true
end)

-- ================== 🔥 DIVINE SYSTEM ACTIVE ==================
playBtn.MouseButton1Click:Connect(function()
    introSound:Stop(); introGui:Destroy()
    
    local scanning, espEnabled = false, false
    local saved, seen, espList, tracers = {}, {}, {}, {}
    local behaviorLog = {} -- บอทเก็บพฤติกรรม
    local gui = Instance.new("ScreenGui", player.PlayerGui)

    local function notifySide(msg, title, col)
        local nF = Instance.new("Frame", gui); nF.Size = UDim2.new(0,320,0,90); nF.Position = UDim2.new(1,50,0.25,0); nF.BackgroundColor3 = Color3.fromRGB(10,10,10); nF.BorderColor3 = col or Color3.new(0,1,0); nF.BorderSizePixel = 2; Instance.new("UICorner", nF)
        local tL = Instance.new("TextLabel", nF); tL.Text = "🚨 "..title; tL.Size = UDim2.new(1,0,0,30); tL.TextColor3 = col or Color3.new(0,1,0); tL.BackgroundTransparency = 1; tL.TextScaled = true; tL.Font = Enum.Font.SourceSansBold
        local mL = Instance.new("TextLabel", nF); mL.Text = msg; mL.Position = UDim2.new(0,0,0.4,0); mL.Size = UDim2.new(1,0,0,50); mL.TextColor3 = Color3.new(1,1,1); mL.BackgroundTransparency = 1; mL.TextScaled = true
        nF:TweenPosition(UDim2.new(1,-340,0.25,0), "Out", "Back", 0.5, true); task.delay(4, function() if nF then nF:TweenPosition(UDim2.new(1,50,0.25,0), "In", "Quart", 0.5, true); task.wait(0.6); nF:Destroy() end end)
    end

    local frame = Instance.new("Frame", gui); frame.Size = UDim2.new(0,800,0,450); frame.Position = UDim2.new(0.5,-400,0.5,-225); frame.BackgroundColor3 = Color3.fromRGB(5,5,5); frame.BorderColor3 = Color3.new(0,1,0); frame.Visible = false
    local scroll = Instance.new("ScrollingFrame", frame); scroll.Size = UDim2.new(0,210,0,410); scroll.Position = UDim2.new(0,15,0,20); scroll.BackgroundTransparency = 1; scroll.CanvasSize = UDim2.new(0,0,0,1300); scroll.ScrollBarThickness = 4
    local logBox = Instance.new("TextBox", frame); logBox.Size = UDim2.new(0,540,0,410); logBox.Position = UDim2.new(0,240,0,20); logBox.MultiLine = true; logBox.Text = "--- OHM-EXD ABSOLUTE LOG ---"; logBox.TextColor3 = Color3.new(0,1,0); logBox.BackgroundColor3 = Color3.new(0,0,0); logBox.TextYAlignment = Enum.TextYAlignment.Top; logBox.TextEditable = false

    local function createBtn(txt, y, cb)
        local b = Instance.new("TextButton", scroll); b.Size = UDim2.new(0,190,0,45); b.Position = UDim2.new(0,0,0,y); b.Text = txt; b.BackgroundColor3 = Color3.fromRGB(20,20,20); b.TextColor3 = Color3.new(0,1,0); b.Font = Enum.Font.SourceSansBold; b.MouseButton1Click:Connect(cb); Instance.new("UICorner", b); return b
    end

    createBtn("🎧 พระเจ้าดูดเพลง (No Veh)", 0, function() scanning = not scanning; notifySide(scanning and "On" or "Off", "SCANNER") end)
    createBtn("⚡ ทะลวงไอดีจริง (Bypass)", 50, function() deepBypassActive = not deepBypassActive; notifySide("Deep Bypass Active", "SYSTEM") end)
    createBtn("📡 ESP เขียวมหาเทพ", 100, function() espEnabled = not espEnabled; notifySide("ESP On", "VISUAL") end)
    createBtn("🤖 บอทมหาเทพตรวจคน", 150, function() aiDetectActive = not aiDetectActive; notifySide("Behavior Bot Active", "AI BOT") end)
    createBtn("🎭 ปลอม ID (Fake 36,0)", 200, function() botGuardianActive = not botGuardianActive; notifySide("Fake Active", "PHANTOM") end)
    createBtn("📋 คัดลอกไอดี", 250, function() if setclipboard then setclipboard(logBox.Text) notifySide("Copied", "CLIPBOARD") end end)
    createBtn("🗑 ล้างค่า Log", 300, function() seen = {}; saved = {}; logBox.Text = "--- LOG CLEARED ---" end)

    -- [⚙️ RGB SETTINGS ห้ามหาย!]
    local setBtn = createBtn("⚙️ ตั้งค่าสี / RGB", 350, function() end)
    local sFrame = Instance.new("ScrollingFrame", gui); sFrame.Size = UDim2.new(0,240,0,250); sFrame.Position = UDim2.new(0.7,0,0.3,0); sFrame.Visible = false; sFrame.BackgroundColor3 = Color3.fromRGB(15,15,15); sFrame.BorderColor3 = Color3.new(0,1,0); sFrame.CanvasSize = UDim2.new(0,0,0,400)
    setBtn.MouseButton1Click:Connect(function() sFrame.Visible = not sFrame.Visible end)
    
    local function makeSetBtn(txt, y, cb)
        local b = Instance.new("TextButton", sFrame); b.Size = UDim2.new(0,200,0,40); b.Position = UDim2.new(0,20,0,y); b.Text = txt; b.BackgroundColor3 = Color3.fromRGB(40,40,40); b.TextColor3 = Color3.new(1,1,1); b.MouseButton1Click:Connect(cb); Instance.new("UICorner", b)
    end
    makeSetBtn("🌈 Tracer RGB (On/Off)", 10, function() RGB_Tracer = not RGB_Tracer end)
    makeSetBtn("🌈 ID Label RGB (On/Off)", 60, function() RGB_ID = not RGB_ID end)
    makeSetBtn("🟢 Reset to Green", 110, function() Tracer_Color = Color3.new(0,1,0); ID_Label_Color = Color3.new(0,1,0); RGB_Tracer, RGB_ID = false, false end)

    -- [📡 CORE ENGINE: DIVINE FILTER & BEHAVIOR BOT]
    local function getDivineID(s)
        local raw = s.SoundId:match("%d+")
        local n = s.Name:lower()
        -- 🚫 กรองเสียงรถแบบเนียนๆ (ห้ามลืม!)
        if n:find("car") or n:find("engine") or n:find("drive") or n:find("wheel") or n:find("tire") or n:find("exhaust") or n:find("seat") or n:find("skate") then return nil end
        
        local function checkValid(id) return id and #id >= 10 and id ~= "36" and id ~= "0" end

        if deepBypassActive then
            for name, val in pairs(s:GetAttributes()) do
                local id = tostring(val):match("%d+")
                if checkValid(id) then return id end
            end
        end
        if checkValid(raw) then return raw end
        return nil
    end

    RunService.RenderStepped:Connect(function()
        if RGB_Tracer then Tracer_Color = Color3.fromHSV(tick()%5/5, 1, 1) end
        if RGB_ID then ID_Label_Color = Color3.fromHSV(tick()%5/5, 1, 1) end
        for i,v in pairs(espList) do v:Destroy() end espList = {}
        for i,v in pairs(tracers) do v:Remove() end tracers = {}

        for _, p in pairs(Players:GetPlayers()) do
            pcall(function()
                if p.Character and p.Character:FindFirstChild("Head") then
                    local s = nil
                    local isHacker = false
                    for _, v in pairs(p.Character:GetDescendants()) do
                        if v:IsA("Sound") and v.IsPlaying then
                            local rid = getDivineID(v)
                            if rid then 
                                s = v
                                -- บอทเช็คพฤติกรรม: ถ้าเพลงกระโดดหรือชื่อแปลกๆ
                                if v:GetAttribute("IsFromScript") or v.Name == "Sound" or v.PlaybackLoudness > 500 then isHacker = true end
                                break 
                            end
                        end
                    end

                    if s then
                        local rid = getDivineID(s)
                        local did = rid or "BLOCK:ขยะ/รถ"
                        if p == player and botGuardianActive then
                            did = ({"36","0","1",tostring(math.random(1111,9999))})[math.random(1,4)]
                        end

                        -- [🚨 บอทตรวจคนรันสคริปต์ - ป้ายแดงด่าหัว]
                        if aiDetectActive and p ~= player and isHacker then
                            local hb = Instance.new("BillboardGui", p.Character.Head); hb.Size = UDim2.new(0,250,0,50); hb.AlwaysOnTop = true; hb.ExtentsOffset = Vector3.new(0,5,0)
                            local ht = Instance.new("TextLabel", hb); ht.Size = UDim2.new(1,0,1,0); ht.BackgroundTransparency = 1; ht.Text = "🚨 มหาเทพตรวจพบคนรันเพลง 🚨"; ht.TextColor3 = Color3.new(1,0,0); ht.TextScaled = true; ht.Font = Enum.Font.SourceSansBold; table.insert(espList, hb)
                        end

                        if scanning and rid and not seen[rid] then
                            seen[rid] = true; table.insert(saved, "🎵 "..s.Name.." | ID: "..rid); logBox.Text = table.concat(saved, "\n")
                        end

                        if espEnabled then
                            local b = Instance.new("BillboardGui", p.Character.Head); b.Size = UDim2.new(0,200,0,70); b.AlwaysOnTop = true; b.ExtentsOffset = Vector3.new(0,3,0)
                            local t = Instance.new("TextLabel", b); t.Size = UDim2.new(1,0,1,0); t.BackgroundTransparency = 1; t.Text = "👤 "..p.Name.."\n🆔 "..did; t.TextColor3 = ID_Label_Color; t.TextScaled = true; table.insert(espList, b)
                            local pos, vis = Camera:WorldToViewportPoint(p.Character.Head.Position)
                            if vis and p ~= player then
                                local l = Drawing.new("Line"); l.Visible = true; l.From = Vector2.new(Camera.ViewportSize.X/2, Camera.ViewportSize.Y); l.To = Vector2.new(pos.X, pos.Y); l.Color = Tracer_Color; l.Thickness = 1.5; table.insert(tracers, l)
                            end
                        end
                    end
                end
            end)
        end
    end)

    -- [🟢 UI BUTTON: OHJ09LY]
    local openBtn = Instance.new("TextButton", gui); openBtn.Size = UDim2.new(0,120,0,40); openBtn.Position = UDim2.new(0,20,0,200); openBtn.Text = "OHJ09LY"; openBtn.BackgroundColor3 = Color3.fromRGB(0,60,0); openBtn.TextColor3 = Color3.new(1,1,1); openBtn.Font = Enum.Font.SourceSansBold; Instance.new("UICorner", openBtn)
    openBtn.MouseButton1Click:Connect(function() frame.Visible = true; openBtn.Visible = false end)
    local close = Instance.new("TextButton", frame); close.Size = UDim2.new(0,30,0,30); close.Position = UDim2.new(1,-35,0,5); close.Text = "X"; close.TextColor3 = Color3.new(1,0,0); close.MouseButton1Click:Connect(function() frame.Visible = false; openBtn.Visible = true end)
end)# -
