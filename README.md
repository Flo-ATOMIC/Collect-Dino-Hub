# Collect-Dino-Hub
local Players=game:GetService("Players")
local RunService=game:GetService("RunService")
local vim=game:GetService("VirtualInputManager")
local gs=game:GetService("GuiService")
local player=Players.LocalPlayer
local playerGui=player.PlayerGui
if playerGui:FindFirstChild("FarmGUI") then playerGui.FarmGUI:Destroy() end

local screenGui=Instance.new("ScreenGui")
screenGui.Name="FarmGUI"
screenGui.ResetOnSpawn=false
screenGui.Parent=playerGui

local frame=Instance.new("Frame")
frame.Name="Main"
frame.Size=UDim2.new(0,280,0,312)
frame.Position=UDim2.new(0.5,-140,0,20)
frame.BackgroundColor3=Color3.fromRGB(25,25,25)
frame.BorderSizePixel=0;frame.Active=true;frame.Draggable=true;frame.Parent=screenGui
local function mc(p,r) Instance.new("UICorner",p).CornerRadius=UDim.new(0,r) end
mc(frame,10)

local function makeLine(y)
    local d=Instance.new("Frame",frame)
    d.Size=UDim2.new(1,-16,0,1);d.Position=UDim2.new(0,8,0,y)
    d.BackgroundColor3=Color3.fromRGB(60,60,60);d.BorderSizePixel=0
end

local titleLabel=Instance.new("TextLabel",frame)
titleLabel.Size=UDim2.new(1,-150,0,28);titleLabel.Position=UDim2.new(0,10,0,0)
titleLabel.BackgroundTransparency=1;titleLabel.Text="Collect Dinos Script"
titleLabel.TextColor3=Color3.fromRGB(200,200,200);titleLabel.TextSize=12
titleLabel.Font=Enum.Font.GothamBold;titleLabel.TextXAlignment=Enum.TextXAlignment.Left

local muteBtn=Instance.new("TextButton",frame)
muteBtn.Size=UDim2.new(0,26,0,20);muteBtn.Position=UDim2.new(1,-124,0,4)
muteBtn.BackgroundColor3=Color3.fromRGB(45,160,45);muteBtn.Text="$"
muteBtn.TextColor3=Color3.fromRGB(255,255,255);muteBtn.TextSize=13
muteBtn.Font=Enum.Font.GothamBold;muteBtn.BorderSizePixel=0;mc(muteBtn,5)

local fpsLabel=Instance.new("TextLabel",frame)
fpsLabel.Size=UDim2.new(0,60,0,20);fpsLabel.Position=UDim2.new(1,-96,0,4)
fpsLabel.BackgroundTransparency=1;fpsLabel.Text="Fps: 0"
fpsLabel.TextColor3=Color3.fromRGB(255,200,80);fpsLabel.TextSize=12
fpsLabel.Font=Enum.Font.GothamBold;fpsLabel.TextXAlignment=Enum.TextXAlignment.Left

local closeBtn=Instance.new("TextButton",frame)
closeBtn.Size=UDim2.new(0,22,0,22);closeBtn.Position=UDim2.new(1,-26,0,3)
closeBtn.BackgroundColor3=Color3.fromRGB(160,45,45);closeBtn.BorderSizePixel=0;closeBtn.Text=""
mc(closeBtn,6)
local x1=Instance.new("Frame",closeBtn);x1.Size=UDim2.new(0,12,0,2);x1.Position=UDim2.new(0.5,-6,0.5,-1)
x1.Rotation=45;x1.BackgroundColor3=Color3.fromRGB(255,255,255);x1.BorderSizePixel=0
local x2=Instance.new("Frame",closeBtn);x2.Size=UDim2.new(0,12,0,2);x2.Position=UDim2.new(0.5,-6,0.5,-1)
x2.Rotation=-45;x2.BackgroundColor3=Color3.fromRGB(255,255,255);x2.BorderSizePixel=0

makeLine(29)

local function makeBtn(y,text)
    local b=Instance.new("TextButton",frame)
    b.Size=UDim2.new(1,-16,0,34);b.Position=UDim2.new(0,8,0,y)
    b.BackgroundColor3=Color3.fromRGB(190,45,45);b.Text=text
    b.TextColor3=Color3.fromRGB(255,255,255);b.TextSize=13
    b.Font=Enum.Font.GothamBold;b.BorderSizePixel=0;mc(b,8)
    return b
end

local farmCont=Instance.new("Frame",frame)
farmCont.Size=UDim2.new(1,-16,0,34);farmCont.Position=UDim2.new(0,8,0,38)
farmCont.BackgroundTransparency=1;farmCont.BorderSizePixel=0
local farmBtn=Instance.new("TextButton",farmCont)
farmBtn.Size=UDim2.new(0.5,-2,1,0);farmBtn.BackgroundColor3=Color3.fromRGB(190,45,45)
farmBtn.Text="FARM";farmBtn.TextColor3=Color3.fromRGB(255,255,255)
farmBtn.TextSize=13;farmBtn.Font=Enum.Font.GothamBold;farmBtn.BorderSizePixel=0;mc(farmBtn,8)
local fd=Instance.new("Frame",farmCont);fd.Size=UDim2.new(0,2,1,0);fd.Position=UDim2.new(0.5,-1,0,0)
fd.BackgroundColor3=Color3.fromRGB(25,25,25);fd.BorderSizePixel=0
local silentBtn=Instance.new("TextButton",farmCont)
silentBtn.Size=UDim2.new(0.5,-2,1,0);silentBtn.Position=UDim2.new(0.5,2,0,0)
silentBtn.BackgroundColor3=Color3.fromRGB(190,45,45);silentBtn.Text="SILENT FARM"
silentBtn.TextColor3=Color3.fromRGB(255,255,255);silentBtn.TextSize=13
silentBtn.Font=Enum.Font.GothamBold;silentBtn.BorderSizePixel=0;mc(silentBtn,8)

makeLine(80)
local hatchBtn=makeBtn(88,"AUTO HATCH EGG")
makeLine(130)
local digBtn=makeBtn(138,"AUTO DIG")
makeLine(180)

local skipLbl=Instance.new("TextLabel",frame)
skipLbl.Size=UDim2.new(1,-16,0,14);skipLbl.Position=UDim2.new(0,8,0,186)
skipLbl.BackgroundTransparency=1;skipLbl.Text="skip eggs to dig:"
skipLbl.TextColor3=Color3.fromRGB(120,120,120);skipLbl.TextSize=11
skipLbl.Font=Enum.Font.Gotham;skipLbl.TextXAlignment=Enum.TextXAlignment.Left

local EGG_TYPES={"Small","Large","Giant","Colossal","Titanic","Legendary","Mythic"}
local EGG_COLORS={
    Color3.fromRGB(160,160,160),
    Color3.fromRGB(100,210,100),
    Color3.fromRGB(80,140,255),
    Color3.fromRGB(160,80,220),
    Color3.fromRGB(220,180,40),
    Color3.fromRGB(220,60,60),
    Color3.fromRGB(124,244,200)
}
local skipState={}
local checksRow=Instance.new("Frame",frame)
checksRow.Size=UDim2.new(1,-16,0,26);checksRow.Position=UDim2.new(0,8,0,202)
checksRow.BackgroundTransparency=1;checksRow.BorderSizePixel=0
local btnW=math.floor(264/#EGG_TYPES)
for i,name in ipairs(EGG_TYPES) do
    skipState[name]=false
    local btn=Instance.new("TextButton",checksRow)
    btn.Size=UDim2.new(0,btnW-2,1,0);btn.Position=UDim2.new(0,(i-1)*btnW,0,0)
    btn.BackgroundColor3=Color3.fromRGB(45,45,45);btn.Text=name:sub(1,3)
    btn.TextColor3=EGG_COLORS[i];btn.TextSize=10
    btn.Font=Enum.Font.GothamBold;btn.BorderSizePixel=0;mc(btn,6)
    local col=EGG_COLORS[i]
    btn.MouseButton1Click:Connect(function()
        skipState[name]=not skipState[name]
        btn.BackgroundColor3=skipState[name] and col or Color3.fromRGB(45,45,45)
        btn.TextColor3=skipState[name] and Color3.fromRGB(255,255,255) or col
    end)
end

makeLine(234)

local speedRow=Instance.new("Frame",frame)
speedRow.Size=UDim2.new(1,-16,0,28);speedRow.Position=UDim2.new(0,8,0,242)
speedRow.BackgroundTransparency=1;speedRow.BorderSizePixel=0
local speedLbl=Instance.new("TextLabel",speedRow)
speedLbl.Size=UDim2.new(0,50,1,0);speedLbl.BackgroundTransparency=1
speedLbl.Text="Speed:";speedLbl.TextColor3=Color3.fromRGB(160,160,160)
speedLbl.TextSize=12;speedLbl.Font=Enum.Font.Gotham;speedLbl.TextXAlignment=Enum.TextXAlignment.Left
local speedInput=Instance.new("TextBox",speedRow)
speedInput.Size=UDim2.new(0,55,1,-4);speedInput.Position=UDim2.new(0,52,0,2)
speedInput.BackgroundColor3=Color3.fromRGB(45,45,45);speedInput.Text="32"
speedInput.TextColor3=Color3.fromRGB(255,255,255);speedInput.TextSize=13
speedInput.Font=Enum.Font.Gotham;speedInput.BorderSizePixel=0
speedInput.ClearTextOnFocus=false;mc(speedInput,6)
local afkBtn=Instance.new("TextButton",speedRow)
afkBtn.Size=UDim2.new(1,-114,1,-4);afkBtn.Position=UDim2.new(0,112,0,2)
afkBtn.BackgroundColor3=Color3.fromRGB(50,90,160);afkBtn.Text="ANTI-AFK"
afkBtn.TextColor3=Color3.fromRGB(255,255,255);afkBtn.TextSize=12
afkBtn.Font=Enum.Font.GothamBold;afkBtn.BorderSizePixel=0;mc(afkBtn,6)

makeLine(276)

local statusLabel=Instance.new("TextLabel",frame)
statusLabel.Size=UDim2.new(1,-16,0,14);statusLabel.Position=UDim2.new(0,8,1,-16)
statusLabel.BackgroundTransparency=1;statusLabel.Text="ready"
statusLabel.TextColor3=Color3.fromRGB(120,120,120);statusLabel.TextSize=11
statusLabel.Font=Enum.Font.Gotham;statusLabel.TextXAlignment=Enum.TextXAlignment.Left

local GREEN=Color3.fromRGB(45,170,45)
local RED=Color3.fromRGB(190,45,45)
local BLUE=Color3.fromRGB(50,90,160)
local BLUE_ON=Color3.fromRGB(60,140,60)
local TeleportHome=game.ReplicatedStorage.RemoteEvents.TeleportHome
local coinSound=game.ReplicatedStorage.Audio.AddCurrency

local farming=false
local silentFarming=false
local autoHatch=false
local autoDig=false
local antiAfk=false
local soundMuted=false
local myPlot=nil
local farmThread=nil
local hatchThread=nil
local digThread=nil
local speedConn=nil
local afkConn=nil
local loopSpeedConn=nil
local origCFrames={}
local currentSpeed=32
local MIN_CD=0.82
local STEP=0.04
local MAX_MISSES=3

pcall(function() workspace.CurrentCamera.MaxZoomDistance=9999 end)

local function goHome()
    pcall(function() TeleportHome:FireServer() end)
    task.wait(1.5)
end

local function parseCash(str)
    if not str then return 0 end
    str=tostring(str):gsub("%$",""):gsub(",",""):gsub(" ","")
    local num=tonumber(str:match("[%d%.]+"))
    if not num then return 0 end
    local s=(str:match("[KMBTkmbt]+$") or ""):upper()
    if s=="K" then return num*1e3
    elseif s=="M" then return num*1e6
    elseif s=="B" then return num*1e9
    elseif s=="T" then return num*1e12 end
    return num
end

local function getRemoteHatchLimit()
    local limit=0
    pcall(function()
        if not myPlot then return end
        local t=myPlot.ParkTierModel.Screen1.Screen:FindFirstChild("RemoteHatchText",true)
        if t then
            local s=t.Text:match("%$[%d%.]+[KMBTkmbt]*")
            if s then limit=parseCash(s) end
        end
    end)
    return limit
end

local function getMyPlot()
    local hrp=player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    if not hrp then return nil end
    local closest,minDist=nil,math.huge
    for _,plot in ipairs(game.Workspace.Plots:GetChildren()) do
        local center=plot:FindFirstChild("Center")
        if center then
            local dist=(hrp.Position-center.Position).Magnitude
            if dist<minDist then minDist=dist;closest=plot end
        end
    end
    return closest
end

local function ensurePlot()
    if not myPlot then
        myPlot=getMyPlot()
        statusLabel.Text="plot: "..(myPlot and myPlot.Name or "?")
        task.wait(0.3)
    end
end

local function getSortedButtons()
    if not myPlot then return {} end
    local left,right={},{}
    for _,unit in ipairs(myPlot.UnitSigns:GetChildren()) do
        local btn=unit:FindFirstChild("Button")
        if btn and btn.Transparency==0 then
            if btn.Position.X<0 then table.insert(left,btn)
            else table.insert(right,btn) end
        end
    end
    table.sort(left,function(a,b) return a.Position.Z>b.Position.Z end)
    table.sort(right,function(a,b) return a.Position.Z>b.Position.Z end)
    local res={}
    local max=math.max(#left,#right)
    for i=1,max do
        if left[i] then table.insert(res,left[i]) end
        if right[i] then table.insert(res,right[i]) end
    end
    return res
end

local function restoreButtons()
    for btn,cf in pairs(origCFrames) do pcall(function() btn.CFrame=cf end) end
    origCFrames={}
end

local function guiClick(btn)
    if not btn then return false end
    for _=1,10 do
        gs.SelectedObject=btn
        task.wait(0.15)
        if gs.SelectedObject==btn then
            vim:SendKeyEvent(true,Enum.KeyCode.Return,false,game)
            task.wait(0.05)
            vim:SendKeyEvent(false,Enum.KeyCode.Return,false,game)
            task.wait(0.1)
            gs.SelectedObject=nil
            return true
        end
        task.wait(0.1)
    end
    gs.SelectedObject=nil
    return false
end

-- Ждём пока диалог ПОЛНОСТЬЮ готов:
-- либо HatchButton видна (можно брать на месте)
-- либо NotInZoneFrame видна (нужно идти домой)
-- возвращает "hatch", "home" или "" если таймаут
local function waitForDialogReady()
    for _=1,100 do
        local ok1,hbVis=pcall(function()
            return playerGui.EggGui.EggInfo.HatchMenu.HatchButton.Visible
        end)
        local ok2,nizVis=pcall(function()
            return playerGui.EggGui.EggInfo.HatchMenu.NotInZoneFrame.Visible
        end)
        local ok3,name=pcall(function()
            return playerGui.EggGui.EggInfo.ItemName.Text
        end)
        local realName=ok3 and name~="Item Name" and name~="" and name
        if realName then
            if ok1 and hbVis then return "hatch" end
            if ok2 and nizVis then return "home" end
        end
        task.wait(0.1)
    end
    return ""
end

local function getEggDialogInfo()
    local eggName,eggPrice,hasUnknown="",0,false
    local ok1,n=pcall(function() return playerGui.EggGui.EggInfo.ItemName.Text end)
    local ok2,p=pcall(function() return playerGui.EggGui.EggInfo.HatchMenu.PriceFrame.ItemPrice.Text end)
    eggName=(ok1 and n~="Item Name" and n~="" and n) or ""
    eggPrice=(ok2 and parseCash(p)) or 0
    local ok3,sf=pcall(function() return playerGui.EggGui.EggInfo.ScrollingFrame end)
    if ok3 and sf then
        for _,v in ipairs(sf:GetChildren()) do
            local nt=v:FindFirstChild("NameText")
            if nt and nt.Text=="???" then hasUnknown=true;break end
        end
    end
    return eggName,eggPrice,hasUnknown
end

local function anySkipped()
    for _,v in pairs(skipState) do if v then return true end end
    return false
end

local function handleEggDialog()
    statusLabel.Text="waiting for egg..."
    local dialogState=waitForDialogReady()
    if dialogState=="" then return end

    local eggName,eggPrice,hasUnknown=getEggDialogInfo()
    if eggName=="" then return end

    if dialogState=="home" then
        local remoteLimit=getRemoteHatchLimit()
        if remoteLimit>0 and eggPrice<=remoteLimit then
        else
            statusLabel.Text="going home: "..eggName
            goHome()
            task.wait(0.5)
            myPlot=getMyPlot()
            for _=1,15 do
                local ok,vis=pcall(function()
                    return playerGui.EggGui.EggInfo.HatchMenu.NotInZoneFrame.Visible
                end)
                if ok and not vis then break end
                task.wait(0.3)
            end
            eggName,eggPrice,hasUnknown=getEggDialogInfo()
        end
    end

    local ok,cashTxt=pcall(function() return playerGui.CurrencyGui.Frame.CashText.Text end)
    local myCash=(ok and parseCash(cashTxt)) or 0
    local shouldTake=false

    if not anySkipped() then
        shouldTake=hasUnknown and (eggPrice==0 or myCash>=eggPrice)
    else
        shouldTake=true
        for shortName,isSkipped in pairs(skipState) do
            if isSkipped and string.find(eggName,shortName,1,true) then
                shouldTake=false;break
            end
        end
        if eggPrice>0 and myCash<eggPrice then shouldTake=false end
    end

    local clicked=false
    if shouldTake then
        statusLabel.Text="hatching: "..eggName
        local ok2,hb=pcall(function() return playerGui.EggGui.EggInfo.HatchMenu.HatchButton end)
        if ok2 then
            while not clicked and autoDig do
                clicked=guiClick(hb)
                if not clicked then task.wait(0.2) end
            end
        end
    else
        statusLabel.Text="discarding: "..eggName
        local ok3,db=pcall(function() return playerGui.EggGui.EggInfo.DiscardMenu.DiscardButton end)
        if ok3 then
            while not clicked and autoDig do
                clicked=guiClick(db)
                if not clicked then task.wait(0.2) end
            end
        end
    end
    task.wait(0.5)
end

local function farmLoop()
    local stepWait=STEP
    local zeroCycles=0
    origCFrames={}
    while farming or silentFarming do
        local char=player.Character
        local hrp=char and char:FindFirstChild("HumanoidRootPart")
        if not hrp then task.wait(1);continue end
        local buttons=getSortedButtons()
        if #buttons==0 then task.wait(1);continue end
        statusLabel.Text="dinos:"..#buttons.." cd:"..string.format("%.2f",stepWait)
        local cycleStart=tick()
        local collected=0
        local conn=game.ReplicatedStorage.RemoteEvents.UpdateCurrencyEvent.OnClientEvent:Connect(function()
            collected=collected+1
        end)
        for _,btn in ipairs(buttons) do
            if not farming and not silentFarming then break end
            if silentFarming then
                if not origCFrames[btn] then origCFrames[btn]=btn.CFrame end
                btn.CFrame=hrp.CFrame
                task.wait(stepWait)
                btn.CFrame=origCFrames[btn]
                origCFrames[btn]=nil
                task.wait(stepWait)
            else
                local rot=hrp.CFrame-hrp.CFrame.p
                hrp.CFrame=CFrame.new(btn.Position+Vector3.new(0,2,0))*rot
                task.wait(stepWait)
            end
        end
        conn:Disconnect()
        if collected==0 then
            zeroCycles=zeroCycles+1
            if zeroCycles>=10 then
                statusLabel.Text="wrong plot? going home..."
                goHome()
                task.wait(0.5)
                myPlot=getMyPlot()
                zeroCycles=0
            end
        else
            zeroCycles=0
        end
        local missed=#buttons-collected
        if missed>=MAX_MISSES then stepWait=math.min(stepWait+0.05,0.5)
        else stepWait=math.max(stepWait-0.01,STEP) end
        local elapsed=tick()-cycleStart
        if elapsed<MIN_CD then task.wait(MIN_CD-elapsed) end
    end
    restoreButtons()
end

local function hatchLoop()
    while autoHatch do
        myPlot=getMyPlot()
        if myPlot then
            for _,slot in ipairs(myPlot.EggSlots:GetChildren()) do
                if not autoHatch then break end
                local pp=slot:FindFirstChildWhichIsA("ProximityPrompt")
                local timerPart=slot:FindFirstChild("TimerPart")
                if pp and timerPart and pp.ActionText=="Hatch" then
                    local sg=timerPart:FindFirstChildWhichIsA("SurfaceGui",true)
                    local tf=sg and sg:FindFirstChild("TimerFrame",true)
                    local tt=tf and tf:FindFirstChild("TimerText")
                    if tt and tt.Text=="Ready" then
                        local hrp=player.Character and player.Character:FindFirstChild("HumanoidRootPart")
                        if hrp then
                            local rot=hrp.CFrame-hrp.CFrame.p
                            hrp.CFrame=CFrame.new(timerPart.Position+Vector3.new(0,4,0))*rot
                            task.wait(0.5)
                            fireproximityprompt(pp)
                            task.wait(0.5)
                            goHome()
                            task.wait(0.3)
                            myPlot=getMyPlot()
                        end
                    end
                end
            end
        end
        task.wait(1)
    end
end

local function digLoop()
    while autoDig do
        local hrp=player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        if not hrp then task.wait(1);continue end
        local found=false
        for _,v in ipairs(game.Workspace.EggSpawnZones:GetDescendants()) do
            if not autoDig then break end
            if v and v.Name=="TakeEggPrompt" and v:IsA("ProximityPrompt") then
                local part=v.Parent
                if part and part:IsA("BasePart") then
                    hrp=player.Character and player.Character:FindFirstChild("HumanoidRootPart")
                    if not hrp then break end
                    local rot=hrp.CFrame-hrp.CFrame.p
                    hrp.CFrame=CFrame.new(part.Position+Vector3.new(0,3,0))*rot
                    task.wait(0.5)
                    for _=1,3 do
                        pcall(function() fireproximityprompt(v) end)
                        task.wait(0.3)
                    end
                    handleEggDialog()
                    found=true
                    task.wait(0.3)
                end
            end
        end
        if not found then task.wait(2) end
    end
end

local function startLoopSpeed(val)
    currentSpeed=val
    if loopSpeedConn then loopSpeedConn:Disconnect() end
    if speedConn then speedConn:Disconnect() end
    local char=player.Character
    if char then
        local hum=char:FindFirstChildWhichIsA("Humanoid")
        if hum then hum.WalkSpeed=val end
    end
    loopSpeedConn=RunService.Heartbeat:Connect(function()
        local c=player.Character
        if c then
            local h=c:FindFirstChildWhichIsA("Humanoid")
            if h and h.WalkSpeed~=currentSpeed then h.WalkSpeed=currentSpeed end
        end
    end)
    speedConn=player.CharacterAdded:Connect(function(c)
        c:WaitForChild("Humanoid").WalkSpeed=currentSpeed
    end)
end

local function stopLoopSpeed()
    if loopSpeedConn then loopSpeedConn:Disconnect();loopSpeedConn=nil end
    if speedConn then speedConn:Disconnect();speedConn=nil end
    currentSpeed=32
    local char=player.Character
    if char then
        local hum=char:FindFirstChildWhichIsA("Humanoid")
        if hum then hum.WalkSpeed=32 end
    end
end

local function startAntiAfk()
    if afkConn then afkConn:Disconnect() end
    afkConn=RunService.Heartbeat:Connect(function()
        if antiAfk then
            pcall(function()
                vim:SendKeyEvent(true,Enum.KeyCode.W,false,game)
                vim:SendKeyEvent(false,Enum.KeyCode.W,false,game)
            end)
        end
    end)
end

local function stopAntiAfk()
    if afkConn then afkConn:Disconnect();afkConn=nil end
end

local fpsCount=0
local fpsLast=tick()
RunService.RenderStepped:Connect(function()
    fpsCount=fpsCount+1
    local now=tick()
    if now-fpsLast>=0.5 then
        fpsLabel.Text="Fps: "..math.floor(fpsCount/(now-fpsLast))
        fpsCount=0;fpsLast=now
    end
end)

local function stopFarm()
    farming=false;silentFarming=false
    restoreButtons()
    if farmThread then task.cancel(farmThread);farmThread=nil end
    farmBtn.BackgroundColor3=RED;farmBtn.Text="FARM"
    silentBtn.BackgroundColor3=RED;silentBtn.Text="SILENT FARM"
    statusLabel.Text="stopped"
end

muteBtn.MouseButton1Click:Connect(function()
    soundMuted=not soundMuted
    pcall(function() coinSound.Volume=soundMuted and 0 or 1 end)
    muteBtn.BackgroundColor3=soundMuted and Color3.fromRGB(160,45,45) or Color3.fromRGB(45,160,45)
end)

closeBtn.MouseButton1Click:Connect(function()
    farming=false;silentFarming=false;autoHatch=false;autoDig=false;antiAfk=false
    restoreButtons()
    if farmThread then task.cancel(farmThread) end
    if hatchThread then task.cancel(hatchThread) end
    if digThread then task.cancel(digThread) end
    pcall(function() coinSound.Volume=1 end)
    stopLoopSpeed();stopAntiAfk();screenGui:Destroy()
end)

farmBtn.MouseButton1Click:Connect(function()
    if silentFarming then stopFarm();return end
    farming=not farming
    if farming then
        silentFarming=false
        farmBtn.BackgroundColor3=GREEN;farmBtn.Text="STOP"
        silentBtn.BackgroundColor3=RED;silentBtn.Text="SILENT FARM"
        ensurePlot()
        farmThread=task.spawn(farmLoop)
    else stopFarm() end
end)

silentBtn.MouseButton1Click:Connect(function()
    if farming then stopFarm();return end
    silentFarming=not silentFarming
    if silentFarming then
        farming=false
        silentBtn.BackgroundColor3=GREEN;silentBtn.Text="STOP"
        farmBtn.BackgroundColor3=RED;farmBtn.Text="FARM"
        ensurePlot()
        farmThread=task.spawn(farmLoop)
    else stopFarm() end
end)

hatchBtn.MouseButton1Click:Connect(function()
    autoHatch=not autoHatch
    if autoHatch then
        hatchBtn.BackgroundColor3=GREEN;hatchBtn.Text="STOP HATCH"
        ensurePlot()
        hatchThread=task.spawn(hatchLoop)
    else
        hatchBtn.BackgroundColor3=RED;hatchBtn.Text="AUTO HATCH EGG"
        if hatchThread then task.cancel(hatchThread);hatchThread=nil end
    end
end)

digBtn.MouseButton1Click:Connect(function()
    autoDig=not autoDig
    if autoDig then
        digBtn.BackgroundColor3=GREEN;digBtn.Text="STOP DIG"
        ensurePlot()
        digThread=task.spawn(digLoop)
    else
        digBtn.BackgroundColor3=RED;digBtn.Text="AUTO DIG"
        if digThread then task.cancel(digThread);digThread=nil end
        gs.SelectedObject=nil
    end
end)

speedInput.FocusLost:Connect(function()
    local val=tonumber(speedInput.Text)
    if val then
        val=math.clamp(val,1,500)
        speedInput.Text=tostring(val)
        startLoopSpeed(val)
    else
        speedInput.Text="32"
        stopLoopSpeed()
    end
end)

afkBtn.MouseButton1Click:Connect(function()
    antiAfk=not antiAfk
    if antiAfk then
        afkBtn.BackgroundColor3=BLUE_ON;afkBtn.Text="AFK: ON"
        startAntiAfk()
    else
        afkBtn.BackgroundColor3=BLUE;afkBtn.Text="ANTI-AFK"
        stopAntiAfk()
    end
end)
