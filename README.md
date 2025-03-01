local function checkLine(s)
	local a = 0
	for i in string.gmatch(s, "[^\n]+") do  
		a = a+1
	end
	return a
end
local start = 1
local function addspace(a)
	local st = ""
	for i = 0,a*2 do
		st = st.." "
	end
	return st
end
local function usd(v,i)
	local turn

	if typeof(v) == "Axes" then
		turn = "Axes.new("..tostring(v)..")"
	elseif typeof(v) == "BrickColor" then
		turn = "BrickColor.new("..tostring(v)..")"
	elseif typeof(v) == "CFrame" then
		turn = "CFrame.new("..tostring(v)..")"
	elseif typeof(v) == "Vector3" then
		turn = "Vector3.new("..tostring(v)..")"
	elseif typeof(v) == "Color3" then
		if v.r<=1 and v.g<= 1 and v.b<= 1 and v.r >= 0 and v.g >= 0 and v.b >= 0 then
			local R = v.r * 255
			local G = v.g * 255
			local B = v.b * 255
			turn = 'Color3.fromRGB('..R..", "..G..", "..B..")"..'    --Color3.new('..tostring(v)..")"
		else
			turn = "Color Error"
		end
	elseif typeof(v) == "Instance" then
		turn = "game."..tostring(v:GetFullName())
	elseif typeof(v) == "Enum" then
		turn = tostring(v).."   -- Enum"
	else
		turn = tostring(typeof(v))..".new("..tostring(v)..")"
	end
	if i then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(turn)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(turn)
	end
	return tostring(turn)
end

local convert
local startSpace = 1
function convert(v,i,a)
	if not a then
		a = startSpace    
	end
	if type(v) == "string" then
		if checkLine(v) >= 2 then
			if type(i) == "number" then 
				return '['..tostring(i)..']'.. " = "..'[['..tostring(v)..']]'
			end
			return '["'..tostring(i)..'"]'.. " = "..'[['..tostring(v)..']]'
		else
			if type(i) == "number" then 
				return '['..tostring(i)..']'.. " = "..'"'..tostring(v)..'"'
			end
			return '["'..tostring(i)..'"]'.. " = "..'"'..tostring(v)..'"'
		end
	elseif type(v) == "number" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	elseif type(v) == "boolean" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	elseif type(v) == "nil" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = nil"
	elseif type(v) == "function" then
		local Name = ""
		if debug.getinfo(v).name == "" then
			Name = tostring(v)
		else
			Name = debug.getinfo(v).name
		end
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..'function()end  '..'--'.."[["..tostring(Name).."]]"
		end
		return '["'..tostring(i)..'"]'.. " = "..'function()end  '..'--'.."[["..tostring(Name).."]]"
	elseif type(v) == "userdata" or type(v) == "vector" then
		return usd(v,i)
	elseif type(v) == "table" then
		if i~= nil then
			if type(i) == "number" then
				stt = '['..tostring(i)..']'.." = "..'{'
			else
				stt = '["'..tostring(i)..'"]'.." = "..'{'
			end

		else
			stt = '{'
		end
		local count_table = 0
		for i,v in pairs(v) do
			count_table = count_table+1
			if count_table == 1 then
				stt = stt .. "\n" .. tostring(addspace(a)) .. tostring(convert(v, i, a + 1))
			else    
				stt = stt .. ",\n" .. tostring(addspace(a)) .. tostring(convert(v, i, a + 1))
			end
		end
		return stt.."\n}"
	elseif type(v) == "thread" then
		if type(i) == "number" then 
			return '['..tostring(i)..']'.. " = "..tostring(v)
		end
		return '["'..tostring(i)..'"]'.. " = "..tostring(v)
	end
end


local Fluent = loadstring(game:HttpGet("https://raw.githubusercontent.com/discoart/FluentPlus/refs/heads/main/Beta.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/ZAZA.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/barlossxi/barlossxi/main/InterfaceManager.lua.txt"))()

local ScreenGui1 = Instance.new("ScreenGui")
local ImageButton1 = Instance.new("ImageButton")
local UICorner = Instance.new("UICorner")

ScreenGui1.Name = "ImageButton"
ScreenGui1.Parent = game.CoreGui
ScreenGui1.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

ImageButton1.Parent = ScreenGui1
ImageButton1.BackgroundTransparency = 1
ImageButton1.BorderSizePixel = 0
ImageButton1.Position = UDim2.new(0.120833337, 0, 0.0952890813, 0)
ImageButton1.Size = UDim2.new(0, 50, 0, 50)
ImageButton1.Draggable = true
ImageButton1.Image = "rbxassetid://72591373485246"
ImageButton1.MouseButton1Down:Connect(function()
	game:GetService("VirtualInputManager"):SendKeyEvent(true,305,false,game)
	game:GetService("VirtualInputManager"):SendKeyEvent(false,305,false,game)
end)
UICorner.Parent = ImageButton1

local Window = Fluent:CreateWindow({
	Title = "SpongeBob Tower Defense",
	SubTitle = "Silver Hub",
	TabWidth = 130,
	Size = UDim2.fromOffset(600, 400),
	Acrylic = false, 
	Theme = "Darker",
	MinimizeKey = Enum.KeyCode.RightControl
})

local Tabs = {
	Auto_Join = Window:AddTab({ Title = "Auto Join", Icon = "rotate-3d" }),
	Miscellaneous = Window:AddTab({ Title = "Miscellaneous", Icon = "star" }),
	Macro = Window:AddTab({ Title = "Macro", Icon = "play" }),
	Auto_Play = Window:AddTab({ Title = "Auto Play", Icon = "monitor" }),
	Games = Window:AddTab({ Title = "Game", Icon = "gamepad" }),
	Webhook = Window:AddTab({ Title = "Webhook", Icon = "gift" }),
	Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}


local aza = "Silver Hub"
local bza = tostring(game.Players.LocalPlayer.Name)

if isfolder(aza) then
	print("")
else
	makefolder(aza)
end

function saveSettings()
	local cza = game:GetService("HttpService")
	local dza = cza:JSONEncode(_G)
	if writefile then
		if isfolder(aza) then
			writefile(aza .. "/" .. bza, dza)
		end
	end
end

function loadSettings()
	local cza = game:GetService("HttpService")
	if isfile(aza .. "/" .. bza) then
		_G = cza:JSONDecode(readfile(aza .. "/" .. bza))
	end
end
loadSettings()

local Collection = {} ; Collection.__index = Collection

local Player = game:GetService("Players")
local LocalPlayer = Player.LocalPlayer
local Character = LocalPlayer.Character
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local GuiService = game:GetService("GuiService")
local playerGui = LocalPlayer:FindFirstChild("PlayerGui")
local VirtualInputManager = game:GetService("VirtualInputManager")


local Main = Tabs.Auto_Join:AddSection("[🎮] Main") 
local Merchant = Tabs.Miscellaneous:AddSection("[🛒] Merchant")
local Pass = Tabs.Miscellaneous:AddSection("[🏅] Season Pass")
local Quest = Tabs.Miscellaneous:AddSection("[🏆] Quest")
local Achievements = Tabs.Miscellaneous:AddSection("[🎖️] Achievements")
local Macro = Tabs.Macro:AddSection("[🖱️] Macro")
local Record_Macro = Tabs.Macro:AddSection("[🎥] Record Macro")
local Play_Macro = Tabs.Macro:AddSection("[▶] Play Macro") 
local Full_Auto_Play = Tabs.Auto_Play:AddSection("[🖥️] Fully Auto Play") 
local Ingame = Tabs.Games:AddSection("[👾] In Game")
local webhook = Tabs.Webhook:AddSection("[🎁] Webhook")



local create_line = function()
	xpcall(function()
		local GeneratedParts = workspace:FindFirstChild("GeneratedParts")

		if GeneratedParts then
			GeneratedParts:Destroy()
		end

		local path = workspace.Map.Paths["1"]
		local totalNodes = 10  
		local partCount = 10  
		local counter = 1  

		local folder = Instance.new("Folder")
		folder.Name = "GeneratedParts"
		folder.Parent = workspace

		local function createPartsBetween(nodeA, nodeB, partCount)
			local startPos = nodeA.Position
			local endPos = nodeB.Position

			local direction = (endPos - startPos).Unit  
			local distance = (endPos - startPos).Magnitude  
			local step = distance / (partCount + 1)  

			for i = 1, partCount do
				local newPos = startPos + direction * (step * i)

				local part = Instance.new("Part")
				part.Size = Vector3.new(1, 1, 1)  
				part.Position = newPos
				part.Anchored = true
				part.Transparency = 1
				part.Name = tostring(counter)  
				part.Parent = folder  

				counter = counter + 1
			end
		end

		for i = 0, totalNodes - 1 do
			local nodeA = path["Node" .. i]
			local nodeB = path["Node" .. (i + 1)]

			if nodeA and nodeB then
				createPartsBetween(nodeA, nodeB, partCount)
			end
		end
	end,print)
end


local gen_size = function()
	local workspace = game.Workspace

	-- ลบ CenterPart ถ้ามีอยู่แล้ว
	local centerPart = workspace:FindFirstChild("CenterPart")
	if centerPart then
		centerPart:Destroy()
	end

	-- สร้าง CenterPart ใหม่
	centerPart = Instance.new("Part")
	centerPart.Size = Vector3.new(1, 1, 1)
	centerPart.Position = Vector3.new(0, 5, 0)
	centerPart.Anchored = true
	centerPart.CanCollide = false
	centerPart.BrickColor = BrickColor.Black()
	centerPart.Transparency = 1
	centerPart.Name = "CenterPart"
	centerPart.Parent = workspace

	-- สร้าง CirclePart ใหม่
	local circlePart = Instance.new("Part")
	circlePart.Shape = Enum.PartType.Cylinder
	circlePart.Size = Vector3.new(1, 24, 15)  -- Height, Diameter, Diameter
	circlePart.CFrame = centerPart.CFrame * CFrame.new(0, -0.5, 0) * CFrame.Angles(0, 0, math.rad(90))
	circlePart.Anchored = true
	circlePart.CanCollide = false
	circlePart.BrickColor = BrickColor.new("Light blue")
	circlePart.Transparency = 1
	circlePart.Name = "CirclePart"
	circlePart.Parent = centerPart

	local spacing = 2.2
	local gridSize = _G.Size or 0
	local startX = -(gridSize * spacing) / 2
	local startZ = -(gridSize * spacing) / 2
	local otherParts = {}
	local initialOffsets = {}
	local maxParts = (gridSize + 1) * (gridSize + 1) - 1  -- จำนวนชิ้นส่วนที่ต้องสร้าง (ลบ CenterPart ออกไป)

	local allPartsCreated = false -- ตัวแปรสำหรับตรวจสอบว่าเสร็จหรือยัง

	for i = 0, gridSize do
		for j = 0, gridSize do
			if not (i == gridSize / 2 and j == gridSize / 2) then
				local otherPart = Instance.new("Part")
				otherPart.Size = Vector3.new(1, 1, 1)
				local offset = CFrame.new(startX + (i * spacing), 0, startZ + (j * spacing))
				otherPart.CFrame = centerPart.CFrame * offset
				otherPart.Anchored = true
				otherPart.CanCollide = false
				otherPart.BrickColor = BrickColor.White()
				otherPart.Transparency = 1
				otherPart.Name = "OtherPart_" .. i .. "_" .. j
				otherPart.Parent = centerPart

				table.insert(otherParts, otherPart)
				initialOffsets[otherPart] = offset

				-- ตรวจสอบว่าชิ้นส่วนครบหรือยัง
				if #otherParts >= maxParts then
					allPartsCreated = true -- ตั้งค่าให้รู้ว่าครบแล้ว
				end
			end
		end
	end

	-- เชื่อมต่อกับ Heartbeat หลังจากสร้างเสร็จแล้ว
	game:GetService("RunService").Heartbeat:Connect(function()
		for _, otherPart in ipairs(otherParts) do
			otherPart.CFrame = centerPart.CFrame * initialOffsets[otherPart]
		end
		circlePart.CFrame = centerPart.CFrame * CFrame.new(0, -0.5, 0) * CFrame.Angles(0, 0, math.rad(90))
	end)

	if allPartsCreated == true then
		return false
	else
		return true
	end
end


local interact = function(v2)
	local events = {"Activated","MouseButton1Down","MouseButton1Click","MouseButton1Up"}
	for _,v in pairs(events) do
		for _,connection in pairs(getconnections(v2[v])) do
			connection.Function()
		end
	end
end

local To_Number = function(text)
	local number = text:gsub("%D", "") 
	if number ~= "" and number ~= nil then
		return tonumber(number)
	end
	return false
end

local getqueue = function()
	local a = require(LocalPlayer.PlayerScripts.Knit.Controllers.UIController.GuiModules.QueueScreen)
	for i,v in pairs(a.queueReplica) do
		if type(v) == "number" then
			return v
		end
	end
end

local Convert_Difficulty = function(Difficulty)
	if Difficulty == "Normal" then
		return 1
	elseif Difficulty == "Hard" then
		return 2
	elseif Difficulty == "Nightmare" then
		return 3
	end
end

local Update_World = function()
	local a = {}
	for i,v in pairs(game:GetService("ReplicatedStorage").Shared.Data.Achievements_OLD.Worlds:GetChildren()) do
		table.insert(a,v.Name)
	end
	return a
end


local Update_Quest = function()
	local a = {}

	for i,v in pairs(game:GetService("ReplicatedStorage").Shared.Data.Quests:GetChildren()) do
		table.insert(a,v.Name)
	end
	return a
end


local Update_Achievement = function()
	local a = {}

	for i,v in pairs(game:GetService("ReplicatedStorage").Shared.Data.Achievements_OLD:GetChildren()) do
		table.insert(a,v.Name)
	end
	return a
end

local convert_speed = function(aa)
	if aa == 1 then
		return 1
	elseif aa == 2 then
		return 2
	elseif aa == 3 then
		return 3
	elseif aa == 5 then
		return 4
	end
end



local Select_Stage = Main:AddDropdown("Select Stage", {
	Title = "🍔 Select Stage",
	Description = "If you do not select these auto join will not work.",
	Values = Update_World(),
	Multi = false,
	Default = _G.stage,
	Callback = function(bool)
		_G.stage = bool
		saveSettings()
	end
})


local Select_Act = Main:AddDropdown("Select Act", {
	Title = "Select Act",
	Description = "If you do not select these auto join will not work.",
	Values = {"Act1" ,"Act2" ,"Act3" ,"Act4" ,"Act5" ,"Act6" ,"Act7" ,"Act8" ,"Act9" ,"Act10"},
	Multi = false,
	Default = _G.Act,
	Callback = function(ae)
		_G.Act = To_Number(ae)
		saveSettings()
	end
})


local Select_Diff = Main:AddDropdown("Select Difficulty", {
	Title = "Select Difficulty",
	Description = "If you do not select these auto join will not work.",
	Values = {"Normal" ,"Hard" ,"Nightmare"},
	Multi = false,
	Default = _G.Difficulty,
	Callback = function(bool)
		_G.Difficulty = bool
		saveSettings()
	end
})

local pass = false

Main:AddToggle("Auto Join", {
	Title = "Auto Join",
	Description = "Create your room.",
	Default = _G.AutoJoin,
	Callback = function(bool)
		_G.AutoJoin = bool
		saveSettings()
		task.spawn(function()
			while _G.AutoJoin and task.wait() do
				xpcall(function()
					if game.PlaceId == 123662243100680 then
						for i, v in pairs(workspace.Matchmakers:GetChildren()) do
							if not v:GetAttribute("Mode") then
								if not pass then
									Character.HumanoidRootPart.CFrame = v.WorldPivot
									pass = true
								else
									local args = {
										[1] = getqueue(),
										[2] = "ConfirmMap",
										[3] = {
											["Difficulty"] = Convert_Difficulty(_G.Difficulty),
											["Chapter"] = _G.Act,
											["Endless"] = false,
											["World"] = _G.stage
										}
									}

									game:GetService("ReplicatedStorage"):WaitForChild("ReplicaRemoteEvents"):WaitForChild("Replica_ReplicaSignal"):FireServer(unpack(args))

									pass = false
								end
							end
						end
					end
				end,print)
			end
		end)
	end
})


Main:AddToggle("Auto Start", {
	Title = "Auto Start",
	Description = "Start your room.",
	Default = _G.AutoStart,
	Callback = function(bool)
		_G.AutoStart = bool
		saveSettings()
		task.spawn(function()
			while _G.AutoStart and task.wait() do
				xpcall(function()
					if game.PlaceId == 123662243100680 then
						if playerGui.QueueScreen.Main.SelectionScreen.Visible == false and playerGui.QueueScreen.Main.StartScreen.Visible == true then
							interact(playerGui.QueueScreen.Main.StartScreen.Main.Options.Start)
						end
					end
				end,print)
			end
		end)
	end
})


Main:AddToggle("Friends Only", {
	Title = "Friends Only",
	Description = "Open to allow only friends",
	Default = _G.Friend,
	Callback = function(bool)
		_G.Friend = bool
		saveSettings()
		task.spawn(function()
			while _G.Friend and task.wait() do
				xpcall(function()
					if game.PlaceId == 123662243100680 then
						if playerGui.QueueScreen.Main.SelectionScreen.Visible == true then
							if playerGui.QueueScreen.Main.SelectionScreen.Main.Options.FriendsOption.Toggle.Content.IsEnabled.Visible == false then
								local args = {
									[1] = getqueue(),
									[2] = "ToggleFriendsOnly"
								}

								ReplicatedStorage:WaitForChild("ReplicaRemoteEvents"):WaitForChild("Replica_ReplicaSignal"):FireServer(unpack(args))
								task.wait(1)
							end
						end
					end
				end,print)
			end
		end)
	end
})



local merchant = Merchant:AddDropdown("Sniper Item", {
	Title = "Sniper Item",
	Values = {"GEMS_60" ,"MagicConch" ,"Pretty Patties" ,"TraitRolls" ,"Aged Patty" ,"XP_60"},
	Multi = true,
	Default = _G.Sniper or {},
	Callback = function(bool)
		_G.Sniper = bool
		saveSettings()
	end
})


local Sniper = Merchant:AddToggle("Sniper", {Title = "Auto Sniper Item", Description = "buy your select item [sniper]", Default = _G.Buyer })
Sniper:OnChanged(function(bool)
	_G.Buyer = bool
	saveSettings()
	task.spawn(function()
		while _G.Buyer and task.wait() do
			if game.PlaceId == 123662243100680 then

				repeat task.wait()
					if workspace.LobbyMenuZones.Merchant.Position ~= Character.HumanoidRootPart.Position then
						workspace.LobbyMenuZones.Merchant.Position = Character.HumanoidRootPart.Position
					end
				until playerGui.Merchant.Enabled == true

				interact(playerGui.Merchant.Main.ShopWindow.Exit)

				repeat task.wait()
					local found = false
					for i, v in pairs(playerGui.Merchant.Main.ShopWindow.Content.Bin:GetChildren()) do
						if not v:IsA("Frame") then
							v:Destroy()
							found = true
						end
					end
				until not found

				for i, v in pairs(game:GetService("Players").LocalPlayer.PlayerGui.Merchant.Main.ShopWindow.Content.Bin:GetChildren()) do
					if _G.Sniper[v.Name] then
						local args = {
							[1] = i,
							[2] = 1
						}

						ReplicatedStorage:WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("MerchantService"):WaitForChild("RF"):WaitForChild("Purchase"):InvokeServer(unpack(args))
					end
				end
			end
		end
	end)
end)



local togglecaimpass = Pass:AddToggle("togglecaimpass", {Title = "Auto Caim Season Pass", Description = "caims your season pass", Default = _G.caimpass })
togglecaimpass:OnChanged(function(bool)
	_G.caimpass = bool
	saveSettings()
	task.spawn(function()
		while _G.caimpass and task.wait() do
			if game.PlaceId == 123662243100680 then
				ReplicatedStorage:WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("SeasonPassService"):WaitForChild("RF"):WaitForChild("ClaimAll"):InvokeServer()
				task.wait(0.25)
			end
		end
	end)
end)




local questah = Quest:AddDropdown("Select Category", {
	Title = "Select Category",
	Values = Update_Quest(),
	Multi = true,
	Default = _G.Category or {},
	Callback = function(bool)
		_G.Category = bool
		saveSettings()
	end
})


local togglecaimquest = Quest:AddToggle("togglecaimquest", {Title = "Auto Caim Quest", Description = "caims your quest", Default = _G.caimquest })
togglecaimquest:OnChanged(function(bool)
	_G.caimquest = bool
	saveSettings()
	task.spawn(function()
		while _G.caimquest and task.wait() do
			xpcall(function()
				if game.PlaceId == 123662243100680 then
					for i,v in pairs(ReplicatedStorage.Shared.Data.Quests:GetChildren()) do
						if _G.Category[v.Name] then
							local a = require(ReplicatedStorage.Shared.Data.Quests:FindFirstChild(v.Name))
							for _,z in pairs(a) do
								local args = {
									[1] = "Quests",
									[2] = {
										["Name"] = z.Name,
										["Category"] = v.Name
									}
								}

								ReplicatedStorage:WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("ProgressionService"):WaitForChild("RF"):WaitForChild("Claim"):InvokeServer(unpack(args))
							end
						end
					end
				end
			end,print)
		end
	end)
end)





local Achievement = Achievements:AddDropdown("Select Achievements", {
	Title = "Select Achievements",
	Values = Update_Achievement(),
	Multi = true,
	Default = _G.Achievements or {},
	Callback = function(bool)
		_G.Achievements = bool
		saveSettings()
	end
})


local togglecaimAchievements = Achievements:AddToggle("togglecaimAchievements", {Title = "Auto Caim Achievements", Description = "caims your achievements",  Default = _G.caimAchievements })
togglecaimAchievements:OnChanged(function(bool)
	_G.caimAchievements = bool
	saveSettings()
	task.spawn(function()
		while _G.caimAchievements and task.wait() do
			xpcall(function()
				if game.PlaceId == 123662243100680 then
					for i,v in pairs(ReplicatedStorage.Shared.Data.Achievements_OLD:GetChildren()) do
						if _G.Achievements[v.Name] then
							for _,z in pairs(v:GetChildren()) do
								local b = require(ReplicatedStorage.Shared.Data.Achievements_OLD:FindFirstChild(v.Name):FindFirstChild(z.Name))
								for _,lol in pairs(b) do
									if type(lol) ~= "number" then
										local args = {
											[1] = "Achievements",
											[2] = {
												["SubCategory"] = z.Name,
												["Name"] = lol.Name,
												["Category"] = v.Name
											}
										}

										ReplicatedStorage:WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("ProgressionService"):WaitForChild("RF"):WaitForChild("Claim"):InvokeServer(unpack(args))
									end
								end
							end
						end
					end
				end
			end,print)
		end
	end)
end)


local RecordMacroTable = {}

local path = "Silver Hub/STD/Macro/"

makefolder(path)

local ye = {}

for i,v in pairs(listfiles("Silver Hub/STD/Macro/")) do 
	table.insert(ye,({v:gsub("Silver Hub/STD/Macro/","")})[1])
end

local nameconfig = nil

local Input = Macro:AddInput("Input", {
	Title = "Create Macro",
	Default = nameconfig,
	Placeholder = "Name Macro",
	Numeric = false,
	Finished = true,
	Callback = function(text)
		nameconfig = text
		writefile(path..nameconfig ..".txt","")
	end
})

local Select_Macro = Macro:AddDropdown("Select Macro", {
	Title = "Select Macro",
	Values = ye,
	Multi = false,
	Default = _G.selectconfig,
	Callback = function(tab)
		_G.selectconfig = tab
		saveSettings()
	end
})

local function Refresh()
	ye = {}
	for i,v in pairs(listfiles("Silver Hub/STD/Macro/")) do 
		table.insert(ye,({v:gsub("Silver Hub/STD/Macro/","")})[1])
		Select_Macro:SetValues(ye)
	end
end

Macro:AddButton({
	Title = "Refresh Macro",
	Description = "",
	Callback = Refresh
})

Refresh()

Macro:AddButton({
	Title = "Delete file",
	Description = "",
	Callback = function(ez)
		_G.Delete = ez
		delfile(path.._G.selectconfig)
	end})


local basetime = 0
task.spawn(function()
	while true do 
		local realtime_wait = wait()
		if playerGui.GameUI.VoteStart.Main.Button.Visible == false then 
			basetime = basetime + realtime_wait
		else
			basetime = 0
		end
	end
end)


local bubbleChat = game:GetService("CoreGui").ExperienceChat.bubbleChat:FindFirstChildOfClass("BillboardGui")
if bubbleChat then
	bubbleChat.Name = string.gsub(bubbleChat.Name, "BubbleChat_", "")
	ownerid = bubbleChat.Name
end


local xd = nil

local toggleRecord = Record_Macro:AddToggle("toggleRecord", {Title = "Start Record", Default = xd })
toggleRecord:OnChanged(function(record)
	xd = record
	if xd then 
		RecordMacroTable = {}
	end
end)

if game.PlaceId ~= 123662243100680 then

	local actionIndex

	repeat actionIndex = 1 until actionIndex == 1

	workspace.Friendlies.ChildAdded:Connect(function (Unit)
		if not xd then return end

		if tonumber(Unit:GetAttribute("OwnerId")) ~= tonumber(ownerid) then return end

		local unitCost = tonumber(game:GetService("ReplicatedStorage").LiveSheets.UnitValues[Unit.Name].Cost.Value)
		table.insert(RecordMacroTable,{
			["Index"] = actionIndex,
			["Type"] = "Place",
			["Time"] = basetime,
			["Data"] = {
				["Name"] = Unit.Name,
				["CFrame"] = Unit.RootPart.CFrame,
				["Cost"] = unitCost
			}
		})
		writefile(path .. _G.selectconfig, convert(RecordMacroTable))
		actionIndex = actionIndex + 1

		Unit:GetAttributeChangedSignal("Upgrade"):Connect(function()
			if not xd then return end

			if tonumber(Unit:GetAttribute("OwnerId")) ~= tonumber(ownerid) then return end

			local upgradeLevel = Unit:GetAttribute("Upgrade")
			local upgradeCost = tonumber(game.ReplicatedStorage.LiveSheets.UnitUpgrades[Unit.Name][tostring(upgradeLevel)].Cost.Value)
			table.insert(RecordMacroTable, {
				["Index"] = actionIndex,
				["Type"] = "Upgrade",
				["Time"] = basetime,
				["Data"] = {
					["Name"] = Unit.Name,
					["CFrame"] = Unit.WorldPivot,
					["UpgradeLevel"] = upgradeLevel,
					["Cost"] = upgradeCost
				}
			})
			writefile(path .. _G.selectconfig, convert(RecordMacroTable))
			actionIndex = actionIndex + 1
		end)
	end)

	workspace.Friendlies.ChildRemoved:Connect(function (Unit)
		if not xd then return end

		if tonumber(Unit:GetAttribute("OwnerId")) ~= tonumber(ownerid) then return end

		table.insert(RecordMacroTable,{
			["Index"] = actionIndex,
			["Type"] = "Sell",
			["Time"] = basetime,
			["Data"] = {
				["CFrame"] = Unit.WorldPivot,
				["Cost"] = 0
			}
		})
		writefile(path .. _G.selectconfig, convert(RecordMacroTable))
		actionIndex = actionIndex + 1
	end)
end




local ezeeeeee = Play_Macro:AddDropdown("Select Mode", {
	Title = "Select Mode",
	Values = {"Time","Money"},
	Multi = false,
	Default = _G.modee,
	Callback = function(ez)
		_G.modee = ez
		saveSettings()
	end
})


local togglePlay = Play_Macro:AddToggle("togglePlay", {Title = "Play Macro", Default = _G.Play })
togglePlay:OnChanged(function(play)
	_G.Play = play
	saveSettings()
	if game.PlaceId == 123662243100680 then return end
	print("pass")
	if _G.modee == "Time" then
		repeat task.wait()
			if _G.Play then 
				local datamacro = readfile(path.._G.selectconfig)
				local real = loadstring("return "..datamacro)()
				if #real > 0 and real[#real].Time > basetime then
					for i,v in pairs(real) do 
						if _G.Play then
							repeat wait() until basetime >= v.Time
							if v["Type"] == "Place" then
								for e,z in pairs(playerGui.HUD.Bottom.Hotbar:GetChildren()) do
									if z:IsA("TextButton") then
										local viewport = z.Content.TowerInfo:FindFirstChildOfClass("ViewportFrame")
										if viewport then
											local worldMoyel = viewport:FindFirstChildOfClass("WorldModel")
											local kuy = worldMoyel:FindFirstChildOfClass("Model").Name
											if kuy == v.Data.Name then
												local args = {
													[1] = v.Data.CFrame,
													[2] = tonumber(z.Name)
												}

												game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("TowerService"):WaitForChild("RF"):WaitForChild("PlaceTower"):InvokeServer(unpack(args))
											end
										end
									end
								end
							elseif v["Type"] == "Upgrade" then

								local num = 0

								local current = 1

								local current_instance = nil

								for _,unit in pairs(workspace.Friendlies:GetChildren()) do
									local dis = (v.Data.CFrame.Position-unit.RootPart.Position).Magnitude
									if dis < current then
										current = dis
										current_instance = unit
									end
								end
								if current_instance then
									if num < 1 then
										local args = {
											[1] = current_instance:GetAttribute("Id")
										}

										game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GameService"):WaitForChild("RF"):WaitForChild("UpgradeTower"):InvokeServer(unpack(args))
										num = num + 1
									end
								end
							elseif v["Type"] == "Sell" then

								local num = 0

								local current = 1

								local current_instance = nil

								for _,unit in pairs(workspace.Friendlies:GetChildren()) do
									local dis = (v.Data.CFrame.Position-unit.RootPart.Position).Magnitude
									if dis < current then
										current = dis
										current_instance = unit
									end
								end
								if current_instance then
									if num < 1 then
										local args = {
											[1] = current_instance:GetAttribute("Id")
										}

										game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("TowerService"):WaitForChild("RF"):WaitForChild("SellTower"):InvokeServer(unpack(args))
										num = num + 1
									end
								end
							end
						end
					end
				end
			end
		until not _G.Play
	elseif _G.modee == "Money" then
		repeat
			task.wait()
			if _G.Play then
				local datamacro = readfile(path.._G.selectconfig)
				local real = loadstring("return "..datamacro)()
				local actionIndex = 1
				if #real > 0 and real[#real].Index >= actionIndex then
					for i,v in pairs(real) do
						if _G.Play then
							if game.PlaceId ~= 123662243100680 then
								if v["Index"] >= actionIndex then
									local a	
									repeat task.wait()
										a = string.gsub(game:GetService("Players").LocalPlayer.PlayerGui.HUD.Bottom.GameCurrency.Coins.Title.Text, "%D", "")
									until tonumber(a) >= tonumber(v.Data.Cost)
									if v["Type"] == "Place" then
										for e,z in pairs(playerGui.HUD.Bottom.Hotbar:GetChildren()) do
											if z:IsA("TextButton") then
												local viewport = z.Content.TowerInfo:FindFirstChildOfClass("ViewportFrame")
												if viewport then
													local worldMoyel = viewport:FindFirstChildOfClass("WorldModel")
													local kuy = worldMoyel:FindFirstChildOfClass("Model").Name
													if kuy == v.Data.Name then
														local args = {
															[1] = v.Data.CFrame,
															[2] = tonumber(z.Name)
														}
														game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("TowerService"):WaitForChild("RF"):WaitForChild("PlaceTower"):InvokeServer(unpack(args))
														task.wait(0.5)
													end
												end
											end
										end
									elseif v["Type"] == "Upgrade" then
										local num = 0
										local current = 1
										local current_instance = nil
										for _,unit in pairs(workspace.Friendlies:GetChildren()) do
											local dis = (v.Data.CFrame.Position-unit.RootPart.Position).Magnitude
											if dis < current then
												current = dis
												current_instance = unit
											end
										end
										if current_instance then
											if num < 1 then
												local args = {
													[1] = current_instance:GetAttribute("Id")
												}
												game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GameService"):WaitForChild("RF"):WaitForChild("UpgradeTower"):InvokeServer(unpack(args))
												num = num + 1
												task.wait(0.5)
											end
										end
									elseif v["Type"] == "Sell" then
										local num = 0
										local current = 1
										local current_instance = nil
										for _,unit in pairs(workspace.Friendlies:GetChildren()) do
											local dis = (v.Data.CFrame.Position-unit.RootPart.Position).Magnitude
											if dis < current then
												current = dis
												current_instance = unit
											end
										end
										if current_instance then
											if num < 1 then
												local args = {
													[1] = current_instance:GetAttribute("Id")
												}
												game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("TowerService"):WaitForChild("RF"):WaitForChild("SellTower"):InvokeServer(unpack(args))
												num = num + 1
												task.wait(0.5)
											end
										end
									end
									actionIndex = v.Index + 1
									print(actionIndex)
								end
							end
						end
					end
				end
				task.wait(0.1)
				repeat task.wait() until basetime < real[#real].Time
			end
		until not _G.Play
	end
end)




local Slider = Full_Auto_Play:AddSlider("Placement Size", {
	Title = "Placement Size",
	Description = "Select Placement Size For PlaceTower",
	Default = _G.Size or 0,
	Min = 0,
	Max = 15,
	Rounding = 0,
	Callback = function(Value)
		_G.Size = Value
		saveSettings()
		gen_size()
	end
})


local Sliderr = Full_Auto_Play:AddSlider("Placement Distance", {
	Title = "Placement Distance",
	Description = "Select Placement Distance For PlaceTower",
	Default = _G.Distance or 0,
	Min = 0,
	Max = 100,
	Rounding = 0,
	Callback = function(Value)
		_G.Distance = Value
		saveSettings()
		create_line()
	end
})

local mamung = Full_Auto_Play:AddDropdown("Select Slot", {
	Title = "Select Slot",
	Description = "",
	Values = {1,2,3,4,5},
	Multi = true,
	Default = _G.Selectslot or {},
	Callback = function(ezs)
		_G.Selectslot = ezs
		saveSettings()
	end
})


Full_Auto_Play:AddToggle("Show Hologram", {
	Title = "Show Hologram",
	Default = _G.Hologram,
	Callback = function(bool)
		_G.Hologram = bool
		saveSettings()
		if _G.Hologram then
			if game.PlaceId ~= 123662243100680 then
				task.spawn(function()
					while _G.Hologram and task.wait() do
						xpcall(function()
							workspace.CenterPart:FindFirstChild("CirclePart").Transparency = 0.5
							repeat task.wait() until not _G.Hologram
							workspace.CenterPart:FindFirstChild("CirclePart").Transparency = 1
						end,print)
					end
				end)
			end
		end
	end
})


Full_Auto_Play:AddToggle("Fully Auto Play", {
	Title = "Fully Auto Play",
	Default = _G.autoplay,
	Callback = function(bool)
		_G.autoplay = bool
		saveSettings()
		if _G.autoplay then
			if game.PlaceId ~= 123662243100680 then
				task.spawn(function()
					while _G.autoplay and task.wait() do
						xpcall(function()
							local enemy = workspace.Enemies:FindFirstChildOfClass("Model").RootPart.Position
							for i, v in pairs(workspace.GeneratedParts:GetChildren()) do
								for _,slot in pairs({1,2,3,4,5}) do
									if _G.Selectslot[slot] then
										if tonumber(v.Name) == tonumber(_G.Distance) then
											workspace.CenterPart.CFrame = v.CFrame
											for e,unit in pairs(workspace.CenterPart:GetChildren()) do
												if unit.Name ~= "CirclePart" then
													local args = {
														[1] = CFrame.new(unit.Position.X, enemy.Y-0.3, unit.Position.Z),
														[2] = slot
													}

													game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("TowerService"):WaitForChild("RF"):WaitForChild("PlaceTower"):InvokeServer(unpack(args))
												end
											end
										end
									end
								end
							end

							for _,unit in pairs(workspace.Friendlies:GetChildren()) do
								local args = {
									[1] = unit:GetAttribute("Id")
								}

								game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GameService"):WaitForChild("RF"):WaitForChild("UpgradeTower"):InvokeServer(unpack(args))
							end
						end,print)
					end
				end)
			end
		end
	end
})

local Select_Speed = Ingame:AddDropdown("Select Speed", {
	Title = "Select Speed",
	Values = {1 ,2 ,3 ,5},
	Multi = false,
	Default = _G.selectedSpeed,
	Callback = function(selected)
		_G.selectedSpeed = selected
		saveSettings()
	end
})

Ingame:AddToggle("Auto Speed Vote", {
	Title = "Auto Speed Vote",
	Description = "Automatic speed up",
	Default = _G.Speed, 
	Callback = function(bool)
		_G.Speed = bool
		saveSettings()
		local check = false

		task.spawn(function()
			while _G.Speed and task.wait() do
				xpcall(function()
					if game.PlaceId ~= 123662243100680 then
						if not check then
							local args = {
								[1] = convert_speed(_G.selectedSpeed)
							}

							game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GameService"):WaitForChild("RF"):WaitForChild("ChangeGameSpeed"):InvokeServer(unpack(args))
							task.wait(1)
							check = true
						end
					end
				end,print)
			end
		end)
	end
})


Ingame:AddToggle("Auto Replay", {
	Title = "Auto Replay",
	Description = "Start game again / Replay game",
	Default = _G.AutoReplay,
	Callback = function(bool)
		_G.AutoReplay = bool
		saveSettings()
		task.spawn(function()
			while _G.AutoReplay and task.wait() do
				xpcall(function()
					if game.PlaceId ~= 123662243100680 then
						if playerGui.RoundSummary.Enabled == true then
							game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GameService"):WaitForChild("RF"):WaitForChild("EndGameVote"):InvokeServer("Replay")
							task.wait(0.5)
						end
					end
				end,print)
			end
		end)
	end
})


Ingame:AddToggle("Auto Next", {
	Title = "Auto Next",
	Description = "Go to next stage",
	Default = _G.AutoNext,
	Callback = function(bool)
		_G.AutoNext = bool
		saveSettings()
		task.spawn(function()
			while _G.AutoNext and task.wait() do
				xpcall(function()
					if game.PlaceId ~= 123662243100680 then
						if playerGui.RoundSummary.Enabled == true then
							game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GameService"):WaitForChild("RF"):WaitForChild("EndGameVote"):InvokeServer("Next")
							task.wait(0.5)
						end
					end
				end,print)
			end
		end)
	end
})

Ingame:AddToggle("Auto Start Game", {
	Title = "Auto Start Game",
	Description = "Start your game",
	Default = _G.AutoStart,
	Callback = function(bool)
		_G.AutoStart = bool
		saveSettings()
		task.spawn(function()
			while _G.AutoStart and task.wait() do
				xpcall(function()
					if game.PlaceId ~= 123662243100680 then
						if playerGui.GameUI.VoteStart.Main.Button.Visible == true then
							game:GetService("ReplicatedStorage"):WaitForChild("Packages"):WaitForChild("_Index"):WaitForChild("sleitnick_knit@1.7.0"):WaitForChild("knit"):WaitForChild("Services"):WaitForChild("GameService"):WaitForChild("RF"):WaitForChild("VoteStartRound"):InvokeServer()
							task.wait(0.5)
						end
					end
				end,print)
			end
		end)
	end
})


local Input = webhook:AddInput("Url Webhook", {
	Title = "Url Webhook",
	Default = _G.Url,
	Placeholder = "Here",
	Numeric = false, -- Only allows numbers
	Finished = false, -- Only calls callback when you press enter
	Callback = function(Value)
		_G.Url = Value
		saveSettings()
	end
})


webhook:AddToggle("Auto Send Webhook", {
	Title = "Auto Send Webhook",
	Description = "Send Webhook After Finished Game",
	Default = _G.Send, 
	Callback = function(bool)
		_G.Send = bool
		saveSettings()
		local data = game:GetService("Players").LocalPlayer.PlayerGui.HUD.Bottom.Currency

		if game.PlaceId ~= 123662243100680 then
			playerGui.RoundSummary:GetPropertyChangedSignal("Enabled"):Connect(function()
				if playerGui.RoundSummary.Enabled == true then
					if _G.Send then
						request({
							Url = _G.Url,
							Method = "POST",
							Headers = {
								["Content-Type"] = "application/json"
							},
							Body = ([[{
        					"content": null,
        					"embeds": [
           						{
                					"title": "**Silver Hub Webhook**",
               						"description": "**PlayerStats**",
                					"color": 15272777,
                					"fields": [
                    					{
                        					"name": "**Gems** 💎",
                        					"value": "```\nGems:  %s [💎]\n```"
                    					},
                    					{
                        					"name": "**Coins** 💸",
                        					"value": "```\nCoins:  %s [💸]\n```"
                   						},
                    					{
                        					"name": "**Bubble** 🧼",
                       						"value": "```\nBubble:  %s [🧼]\n```"
                    					}
                					],
                					"image": {
                    					"url": "https://media1.tenor.com/m/NKyVYAH9AZUAAAAC/kusuriya-no-hitorigoto-trailer.gif"
            			    		}
            					}
        					],
        					"attachments": []
   						}]]):format(data.Gems.Title.Text,data.Coins.Title.Text,data.EventCurrency.Title.Text)
						})
						task.wait(2.5)
					end
				end
			end)
		end 
	end
})

SaveManager:SetLibrary(Fluent)
InterfaceManager:SetLibrary(Fluent)

InterfaceManager:BuildInterfaceSection(Tabs.Settings)
SaveManager:BuildConfigSection(Tabs.Settings)
