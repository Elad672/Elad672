  

local Library = loadstring(game:HttpGet("https://pastebin.com/raw/vff1bQ9F"))()
local Window = Library.CreateLib("BedWars Hub V1 Made By Elad", "Ocean")


-- MAIN
local Main = Window:NewTab("Main")
local MainSection = Main:NewSection("Main")


MainSection:NewToggle("BedNuker", "Auto break bed and covers", function(state) 
         if state then 
                 BindToStepped("BedNuker", 1, function() 
                         nuker() 
                 end) 
         else 
                 UnbindFromStepped("BedNuker") 
         end 
 end) 
 
 MainSection:NewToggle("KillAura", "Autoswing the sword if someone is near you", function(state)

	if state then

		BindToStepped("Killaura", 1, function()

			if entity.isAlive then

				KillauraRemote()

			end

		end)

	else

		UnbindFromStepped("Killaura")

	end

end)

MainSection:NewSlider("Distance 1-20", "Increase killaura distance", 20, 1, function(val)

	DistVal["Value"] = val

end)

MainSection:NewToggle("No Swing", "Disable killaura swing", function(state)

	if state then

		if killauraswing["Enabled"] == true then

			killauraswing["Enabled"] = false

		end

	else

		if killauraswing["Enabled"] == false then

			killauraswing["Enabled"] = true

		end

	end

end)

MainSection:NewSlider("Sound 1-0", "Adjust killaura sound", 1, 0, function(val)

	killaurasoundval["Value"] = val

end)

MainSection:NewButton("Keyboard", "Opens Keyboard", function()

loadstring(game:HttpGet("https://pastebin.com/raw/kC3dAMvt"))()
end)

MainSection:NewToggle("No Fall Damage", "Opens No Fall Damage", function(callback)

    local nofall = true

    if callback then

        if nofall then

            spawn(function()

                repeat

                    wait()

                    if nofall == false then

                        return end

                        game:GetService("ReplicatedStorage").rbxts_include.node_modules.net.out._NetManaged.GroundHit:FireServer()

                    until nofall == false

                end)

            end

    else

        local nofall = false

    end

end)

MainSection:NewButton("Inf Jumps", "Loads My Old Script", function()
local InfiniteJumpEnabled = true
game:GetService("UserInputService").JumpRequest:connect(function()
	if InfiniteJumpEnabled then
		game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")
	end
end)
end)

--velocity

MainSection:NewToggle("Velocity", "Prevents taking a knockback", function(state) 
         if state then 
                 KnockbackTable["kbDirectionStrength"] = 0 
                 KnockbackTable["kbUpwardStrength"] = 0 
         else 
                 KnockbackTable["kbDirectionStrength"] = 100 
                 KnockbackTable["kbUpwardStrength"] = 100 
         end 
 end)
 
 MainSection:NewToggle("Reach", "Extend your attack range", function(state) 
         if state then 
                 CombatConstant.RAYCAST_SWORD_CHARACTER_DISTANCE = (reachval["Value"] - 0.0001) 
         else 
                 CombatConstant.RAYCAST_SWORD_CHARACTER_DISTANCE = 14.4 
         end 
 end) 
  
MainSection:NewSlider("Range 18-1", "", 18, 1, function(val) -- 500 (MaxValue) | 0 (MinValue) 
         reachval["Value"] = val 
 end)
 
 MainSection:NewToggle("ESP", "ToggleInfo", function(state)
    if state then
        local lplr = game.Players.LocalPlayer
local camera = game:GetService("Workspace").CurrentCamera
local CurrentCamera = workspace.CurrentCamera
local worldToViewportPoint = CurrentCamera.worldToViewportPoint

local HeadOff = Vector3.new(0, 0.5, 0)
local LegOff = Vector3.new(0,3,0)

for i,v in pairs(game.Players:GetChildren()) do
    local BoxOutline = Drawing.new("Square")
    BoxOutline.Visible = false
    BoxOutline.Color = Color3.new(0,0,0)
    BoxOutline.Thickness = 3
    BoxOutline.Transparency = 1
    BoxOutline.Filled = false

    local Box = Drawing.new("Square")
    Box.Visible = false
    Box.Color = Color3.new(1,1,1)
    Box.Thickness = 1
    Box.Transparency = 1
    Box.Filled = false

    local HealthBarOutline = Drawing.new("Square")
    HealthBarOutline.Thickness = 3
    HealthBarOutline.Filled = false
    HealthBarOutline.Color = Color3.new(0,0,0)
    HealthBarOutline.Transparency = 1
    HealthBarOutline.Visible = false

    local HealthBar = Drawing.new("Square")
    HealthBar.Thickness = 1
    HealthBar.Filled = false
    HealthBar.Transparency = 1
    HealthBar.Visible = false

    function boxesp()
        game:GetService("RunService").RenderStepped:Connect(function()
            if v.Character ~= nil and v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("HumanoidRootPart") ~= nil and v ~= lplr and v.Character.Humanoid.Health > 0 then
                local Vector, onScreen = camera:worldToViewportPoint(v.Character.HumanoidRootPart.Position)

                local RootPart = v.Character.HumanoidRootPart
                local Head = v.Character.Head
                local RootPosition, RootVis = worldToViewportPoint(CurrentCamera, RootPart.Position)
                local HeadPosition = worldToViewportPoint(CurrentCamera, Head.Position + HeadOff)
                local LegPosition = worldToViewportPoint(CurrentCamera, RootPart.Position - LegOff)

                if onScreen then
                    BoxOutline.Size = Vector2.new(1000 / RootPosition.Z, HeadPosition.Y - LegPosition.Y)
                    BoxOutline.Position = Vector2.new(RootPosition.X - BoxOutline.Size.X / 2, RootPosition.Y - BoxOutline.Size.Y / 2)
                    BoxOutline.Visible = true

                    Box.Size = Vector2.new(1000 / RootPosition.Z, HeadPosition.Y - LegPosition.Y)
                    Box.Position = Vector2.new(RootPosition.X - Box.Size.X / 2, RootPosition.Y - Box.Size.Y / 2)
                    Box.Visible = true

                    HealthBarOutline.Size = Vector2.new(2, HeadPosition.Y - LegPosition.Y)
                    HealthBarOutline.Position = BoxOutline.Position - Vector2.new(6,0)
                    HealthBarOutline.Visible = true

                    HealthBar.Size = Vector2.new(2, (HeadPosition.Y - LegPosition.Y) / (game:GetService("Players")[v.Character.Name].NRPBS["MaxHealth"].Value / math.clamp(game:GetService("Players")[v.Character.Name].NRPBS["Health"].Value, 0, game:GetService("Players")[v.Character.Name].NRPBS:WaitForChild("MaxHealth").Value)))
                    HealthBar.Position = Vector2.new(Box.Position.X - 6, Box.Position.Y + (1 / HealthBar.Size.Y))
                    HealthBar.Color = Color3.fromRGB(255 - 255 / (game:GetService("Players")[v.Character.Name].NRPBS["MaxHealth"].Value / game:GetService("Players")[v.Character.Name].NRPBS["Health"].Value), 255 / (game:GetService("Players")[v.Character.Name].NRPBS["MaxHealth"].Value / game:GetService("Players")[v.Character.Name].NRPBS["Health"].Value), 0)
                    HealthBar.Visible = true

                    if v.TeamColor == lplr.TeamColor then
                        --- Our Team
                        BoxOutline.Visible = false
                        Box.Visible = false
                        HealthBarOutline.Visible = false
                        HealthBar.Visible = false
                    else
                        ---Enemy Team
                        BoxOutline.Visible = true
                        Box.Visible = true
                        HealthBarOutline.Visible = true
                        HealthBar.Visible = true
                    end

                else
                    BoxOutline.Visible = false
                    Box.Visible = false
                    HealthBarOutline.Visible = false
                    HealthBar.Visible = false
                end
            else
                BoxOutline.Visible = false
                Box.Visible = false
                HealthBarOutline.Visible = false
                HealthBar.Visible = false
            end
        end)
    end
    coroutine.wrap(boxesp)()
end

game.Players.PlayerAdded:Connect(function(v)
    local BoxOutline = Drawing.new("Square")
    BoxOutline.Visible = false
    BoxOutline.Color = Color3.new(0,0,0)
    BoxOutline.Thickness = 3
    BoxOutline.Transparency = 1
    BoxOutline.Filled = false

    local Box = Drawing.new("Square")
    Box.Visible = false
    Box.Color = Color3.new(1,1,1)
    Box.Thickness = 1
    Box.Transparency = 1
    Box.Filled = false

    local HealthBarOutline = Drawing.new("Square")
    HealthBarOutline.Thickness = 3
    HealthBarOutline.Filled = false
    HealthBarOutline.Color = Color3.new(0,0,0)
    HealthBarOutline.Transparency = 1
    HealthBarOutline.Visible = false

    local HealthBar = Drawing.new("Square")
    HealthBar.Thickness = 1
    HealthBar.Filled = false
    HealthBar.Transparency = 1
    HealthBar.Visible = false

    function boxesp()
        game:GetService("RunService").RenderStepped:Connect(function()
            if v.Character ~= nil and v.Character:FindFirstChild("Humanoid") ~= nil and v.Character:FindFirstChild("HumanoidRootPart") ~= nil and v ~= lplr and v.Character.Humanoid.Health > 0 then
                local Vector, onScreen = camera:worldToViewportPoint(v.Character.HumanoidRootPart.Position)

                local RootPart = v.Character.HumanoidRootPart
                local Head = v.Character.Head
                local RootPosition, RootVis = worldToViewportPoint(CurrentCamera, RootPart.Position)
                local HeadPosition = worldToViewportPoint(CurrentCamera, Head.Position + HeadOff)
                local LegPosition = worldToViewportPoint(CurrentCamera, RootPart.Position - LegOff)

                if onScreen then
                    BoxOutline.Size = Vector2.new(1000 / RootPosition.Z, HeadPosition.Y - LegPosition.Y)
                    BoxOutline.Position = Vector2.new(RootPosition.X - BoxOutline.Size.X / 2, RootPosition.Y - BoxOutline.Size.Y / 2)
                    BoxOutline.Visible = true

                    Box.Size = Vector2.new(1000 / RootPosition.Z, HeadPosition.Y - LegPosition.Y)
                    Box.Position = Vector2.new(RootPosition.X - Box.Size.X / 2, RootPosition.Y - Box.Size.Y / 2)
                    Box.Visible = true

                    HealthBarOutline.Size = Vector2.new(2, HeadPosition.Y - LegPosition.Y)
                    HealthBarOutline.Position = BoxOutline.Position - Vector2.new(6,0)
                    HealthBarOutline.Visible = true

                    HealthBar.Size = Vector2.new(2, (HeadPosition.Y - LegPosition.Y) / (game:GetService("Players")[v.Character.Name].NRPBS["MaxHealth"].Value / math.clamp(game:GetService("Players")[v.Character.Name].NRPBS["Health"].Value, 0, game:GetService("Players")[v.Character.Name].NRPBS:WaitForChild("MaxHealth").Value)))
                    HealthBar.Position = Vector2.new(Box.Position.X - 6, Box.Position.Y + (1/HealthBar.Size.Y))
		    HealthBar.Color = Color3.fromRGB(255 - 255 / (game:GetService("Players")[v.Character.Name].NRPBS["MaxHealth"].Value / game:GetService("Players")[v.Character.Name].NRPBS["Health"].Value), 255 / (game:GetService("Players")[v.Character.Name].NRPBS["MaxHealth"].Value / game:GetService("Players")[v.Character.Name].NRPBS["Health"].Value), 0)                    
		    HealthBar.Visible = true

                    if v.TeamColor == lplr.TeamColor then
                        --- Our Team
                        BoxOutline.Visible = false
                        Box.Visible = false
                        HealthBarOutline.Visible = false
                        HealthBar.Visible = false
                    else
                        ---Enemy Team
                        BoxOutline.Visible = true
                        Box.Visible = true
                        HealthBarOutline.Visible = true
                        HealthBar.Visible = true
                    end

                else
                    BoxOutline.Visible = false
                    Box.Visible = false
                    HealthBarOutline.Visible = false
                    HealthBar.Visible = false
                end
            else
                BoxOutline.Visible = false
                Box.Visible = false
                HealthBarOutline.Visible = false
                HealthBar.Visible = false
            end
        end)
    end
    coroutine.wrap(boxesp)()
end)
    else
        print("Toggle Off")
    end
end)

MainSection:NewLabel("speed")


MainSection:NewTextBox("TextboxText", "TextboxInfo", function(txt)
	game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = txt
end)

-- COOL
local Cool = Window:NewTab("Cool")
local CoolSection = Cool:NewSection("the animation is Fe")



CoolSection:NewButton("Zombie Animation" , "Plays Zombie Animation",function()

loadstring(game:HttpGet("https://pastebin.com/raw/t3yTSPRn",true))()

end)

CoolSection:NewButton("Ninja Animation","Plays Ninja Animation",function()

loadstring(game:HttpGet("https://pastebin.com/raw/bwGLPVV7",true))()

end)

CoolSection:NewButton("Robot Animation","Plays Robot Animation",function()

local Animate =

game.Players.LocalPlayer.Character.Animate

Animate.idle.Animation1.AnimationId = "http://www.roblox.com/asset/?id=616088211"

Animate.idle.Animation2.AnimationId = "http://www.roblox.com/asset/?id=616089559"

Animate.walk.WalkAnim.AnimationId = "http://www.roblox.com/asset/?id=616095330"

Animate.run.RunAnim.AnimationId = "http://www.roblox.com/asset/?id=616091570"

Animate.jump.JumpAnim.AnimationId = "http://www.roblox.com/asset/?id=616090535"

Animate.climb.ClimbAnim.AnimationId = "http://www.roblox.com/asset/?id=616086039"

Animate.fall.FallAnim.AnimationId = "http://www.roblox.com/asset/?id=616087089"

end)

CoolSection:NewButton("Toy Animation","Opens Toy Animation",function()

local Animate =

game.Players.LocalPlayer.Character.Animate

Animate.idle.Animation1.AnimationId = "http://www.roblox.com/asset/?id=782841498"

Animate.idle.Animation2.AnimationId = "http://www.roblox.com/asset/?id=782845736"

Animate.walk.WalkAnim.AnimationId = "http://www.roblox.com/asset/?id=782843345"

Animate.run.RunAnim.AnimationId = "http://www.roblox.com/asset/?id=782842708"

Animate.jump.JumpAnim.AnimationId = "http://www.roblox.com/asset/?id=782847020"

Animate.climb.ClimbAnim.AnimationId = "http://www.roblox.com/asset/?id=782843869"

Animate.fall.FallAnim.AnimationId = "http://www.roblox.com/asset/?id=782846423"

end)

CoolSection:NewLabel("capes")

CoolSection:NewButton("FerestZ cape", "", function()
    local player = game:GetService("Players") local lplr = player.LocalPlayer if lplr.Character.Humanoid.RigType == Enum.HumanoidRigType.R15 then if lplr.Character:FindFirstChild("Torso") then torso = lplr.Character.Torso else torso = lplr.Character.UpperTorso end local CapeP = Instance.new("Part", torso.Parent) CapeP.Name = "Cape" CapeP.Anchored = false CapeP.CanCollide = false CapeP.TopSurface = 0 CapeP.BottomSurface = 0 CapeP.Color = Color3.fromRGB(0,0,0) CapeP.FormFactor = "Custom" CapeP.Size = Vector3.new(0.2,0.2,0.2) local decal = Instance.new("Decal", CapeP) decal.Texture = "http://www.roblox.com/asset/?id=9695902096" decal.Face = "Back" local msh = Instance.new("BlockMesh", CapeP) msh.Scale = Vector3.new(9,17.5,0.5) local motor = Instance.new("Motor", CapeP) motor.Part0 = CapeP motor.Part1 = torso motor.MaxVelocity = 0.01 motor.C0 = CFrame.new(0,1.75,0) * CFrame.Angles(0,math.rad(90),0) motor.C1 = CFrame.new(0,1,0.45) * CFrame.Angles(0,math.rad(90),0) local wave = false repeat wait(1/44) decal.Transparency = torso.Transparency local ang = 0.1 local oldmag = torso.Velocity.magnitude local mv = 0.002 if wave then ang = ang + ((torso.Velocity.magnitude/10) * 0.05) + 0.05 wave = false else wave = true end ang = ang + math.min(torso.Velocity.magnitude/11, 0.5) motor.MaxVelocity = math.min((torso.Velocity.magnitude/111), 0.04) + mv motor.DesiredAngle = -ang if motor.CurrentAngle < -0.2 and motor.DesiredAngle > -0.2 then motor.MaxVelocity = 0.04 end repeat wait() until motor.CurrentAngle == motor.DesiredAngle or math.abs(torso.Velocity.magnitude - oldmag) >= (torso.Velocity.magnitude/10) + 1 if torso.Velocity.magnitude < 0.1 then wait(0.1) end until not CapeP or CapeP.Parent ~= torso.Parent end
end)

CoolSection:NewButton("Rainbow cape", "", function()
    local verlet = {} verlet.step_time = 1 / 50 verlet.gravity = Vector3.new(0, -10, 0) local char = game.Players.LocalPlayer.Character local torso = char:WaitForChild("Torso") local parts = {} local render = game:GetService("RunService").RenderStepped wait(2) local point = {} local link = {} local rope = {} local function ccw(A,B,C) return (C.y-A.y) * (B.x-A.x) > (B.y-A.y) * (C.x-A.x) end local function intersect(A,B,C,D) return ccw(A,C,D) ~= ccw(B,C,D) and ccw(A,B,C) ~= ccw(A,B,D) end local function vec2(v) 	return Vector2.new(v.x, v.z) end function point:step() 	if not self.fixed then 		local derivative = (self.position - self.last_position) * 0.95 		self.last_position = self.position 		self.position = self.position + derivative + (self.velocity * verlet.step_time ^ 2) 		--[[local torsoP = torso.CFrame * CFrame.new(-1, 0, 0.5) 		local torsoE = torso.CFrame * CFrame.new(1, 0, 0.5) 		local pointE = self.position + torso.CFrame.lookVector * 100 		local doIntersect = intersect(vec2(torsoP.p), vec2(torsoE.p), vec2(self.position), vec2(pointE)) 		if not doIntersect then 			self.postition = self.position - torso.CFrame.lookVector * 10 		end]] 	end end function link:step() 	for i = 1, 1 do 		local distance = self.point1.position - self.point2.position 		local magnitude = distance.magnitude 		local differance = (self.length - magnitude) / magnitude 		local translation = ((self.point1.fixed or self.point2.fixed) and 1 or 0.6) * distance * differance 		if not self.point1.fixed then 			self.point1.position = self.point1.position + translation 		end 		if not self.point2.fixed then 			self.point2.position = self.point2.position - translation 		end 	end end function verlet.new(class, a, b, c) 	if class == "Point" then 		local new = {} 		setmetatable(new, {__index = point}) 		new.class = class 		new.position = a or Vector3.new() 		new.last_position = new.position 		new.velocity = verlet.gravity 		new.fixed = false 		return new 	elseif class == "Link" then 		local new = {} 		setmetatable(new, {__index = link}) 		new.class = class 		new.point1 = a 		new.point2 = b 		new.length = c or (a.position - b.position).magnitude 		return new 	elseif class == "Rope" then 		local new = {} 		setmetatable(new, {__index = link}) 		new.class = class 		new.start_point = a 		new.finish_point = b 		new.points = {} 		new.links = {} 		local inc = (b - a) / 10 		for i = 0, 10 do 			table.insert(new.points, verlet.new("Point", a + (i * inc))) 		end 		for i = 2, #new.points do 			table.insert(new.links, verlet.new("Link", new.points[i - 1], new.points[i])) 		end 		return new 	end end local tris = {} local triParts = {} local function GetDiscoColor(hue) local section = hue % 1 * 3 local secondary = 0.5 * math.pi * (section % 1) if section < 1 then return Color3.new(1, 1 - math.cos(secondary), 1 - math.sin(secondary)) elseif section < 2 then return Color3.new(1 - math.sin(secondary), 1, 1 - math.cos(secondary)) else return Color3.new(1 - math.cos(secondary), 1 - math.sin(secondary), 1) end end local function setupPart(part) 	part.Anchored = true 	part.FormFactor = 3 	part.CanCollide = false 	part.TopSurface = 10 	part.BottomSurface = 10 	part.LeftSurface = 10 	part.RightSurface = 10 	part.FrontSurface = 10 	part.BackSurface = 10 	part.Material = "Neon" 	local m = Instance.new("SpecialMesh", part) 	m.MeshType = "Wedge" 	m.Scale = Vector3.new(0.2, 1, 1) 	return part end local function CFrameFromTopBack(at, top, back) 	local right = top:Cross(back) 	return CFrame.new(at.x, at.y, at.z, right.x, top.x, back.x, right.y, top.y, back.y, right.z, top.z, back.z) end local function drawTri(parent, a, b, c) 	local this = {} 	local mPart1 = table.remove(triParts, 1) or setupPart(Instance.new("Part")) 	local mPart2 = table.remove(triParts, 1) or setupPart(Instance.new("Part")) 	function this:Set(a, b, c) 		local ab, bc, ca = b-a, c-b, a-c 		local abm, bcm, cam = ab.magnitude, bc.magnitude, ca.magnitude 		local edg1 = math.abs(0.5 + ca:Dot(ab)/(abm*abm)) 		local edg2 = math.abs(0.5 + ab:Dot(bc)/(bcm*bcm)) 		local edg3 = math.abs(0.5 + bc:Dot(ca)/(cam*cam)) 		if edg1 < edg2 then 			if edg1 >= edg3 then		 				a, b, c = c, a, b 				ab, bc, ca = ca, ab, bc 				abm = cam 			end 		else 			if edg2 < edg3 then 				a, b, c = b, c, a 				ab, bc, ca = bc, ca, ab 				abm = bcm 			else 				a, b, c = c, a, b 				ab, bc, ca = ca, ab, bc 				abm = cam 			end 		end 	 		local len1 = -ca:Dot(ab)/abm 		local len2 = abm - len1 		local width = (ca + ab.unit*len1).magnitude 	 		local maincf = CFrameFromTopBack(a, ab:Cross(bc).unit, -ab.unit) 	 		if len1 > 0.2 then 			mPart1.Parent = parent 			mPart1.Size = Vector3.new(0.2, width, len1) 			mPart1.CFrame = maincf*CFrame.Angles(math.pi,0,math.pi/2)*CFrame.new(0,width/2,len1/2) 		else 			mPart1.Parent = nil 		end 		 		if len2 > 0.2 then 			mPart2.Parent = parent 			mPart2.Size = Vector3.new(0.2, width, len2) 			mPart2.CFrame = maincf*CFrame.Angles(math.pi,math.pi,-math.pi/2)*CFrame.new(0,width/2,-len1 - len2/2) 		else 			mPart2.Parent = nil 		end	 	end 	function this:SetProperty(prop, value) 		mPart1[prop] = value 		mPart2[prop] = value 	end 	this:Set(a, b, c) 	function this:Destroy() 		mPart1:Destroy() 		mPart2:Destroy() 	end 	this.p1 = mPart1 	this.p2 = mPart2 	this.p1.BrickColor = BrickColor.new(GetDiscoColor(math.noise(0.5, 0.5, this.p1.CFrame.Y * 0.5 + time()))) 	this.p2.BrickColor = BrickColor.new(GetDiscoColor(math.noise(0.5, 0.5, this.p2.CFrame.Y * 0.5 + time()))) 	return this end function verlet.draw(object, id) 	if object.class == "Point" then 		local part = parts[id] 		part.BrickColor = BrickColor.new(1, 1, 1) 		part.Transparency = 0 		part.formFactor = 3 		part.Anchored = true 		part.CanCollide = false 		part.TopSurface = 0 		part.BottomSurface = 0 		part.Size = Vector3.new(0.35, 0.35, 0.35) 		part.Material = "Neon" 		part.CFrame = CFrame.new(object.position) 		part.Parent = torso 		return part 	elseif object.class == "Link" then 		local part = parts[id] 		local dist = (object.point1.position - object.point2.position).magnitude 		part.Size = Vector3.new(0.2, 0.2, dist) 		part.CFrame = CFrame.new(object.point1.position, object.point2.position) * CFrame.new(0, 0, dist * -0.5) 		part.Parent = torso 		return part 	end end function verlet.clear() 	for _, v in pairs(workspace:GetChildren()) do 		if v.Name == "Part" then 			v:Destroy() 		end 	end end local points = {} local links = {} for x = 0, 2 do 	points[x] = {} 	for y = 0, 3 do 		points[x][y] = verlet.new("Point", torso.Position + Vector3.new(x * 0.8 - 2, 2 - y * 0.8, 5 + y * 0.4)) 		points[x][y].fixed = y == 0 	end end for x = 1, 2 do 	for y = 0, 3 do 		links[#links + 1] = verlet.new("Link", points[x][y], points[x - 1][y], 1 + y * 0.08) 	end end for x = 0, 2 do 	for y = 1, 3 do 		links[#links + 1] = verlet.new("Link", points[x][y], points[x][y - 1], 1.2 + y * 0.03) 	end end render:connect(function() 	for x = 0, 2 do 		for y = 0, 3 do 			if y == 0 then 				points[x][y].position = (torso.CFrame * CFrame.new(x * 1 - 1, 1, 0.5)).p 			else 				points[x][y]:step() 			end 		end 	end 	for i = 1, #links do 		links[i]:step() 	end 	for i = 1, #tris do 		triParts[#triParts + 1] = tris[i].p1 		triParts[#triParts + 1] = tris[i].p2 	end 	tris = {} 	for x = 1, 2 do 		for y = 1, 3 do 			tris[#tris + 1] = drawTri(torso, points[x - 1][y - 1].position, points[x - 1][y].position, points[x][y - 1].position) 			tris[#tris + 1] = drawTri(torso, points[x][y].position, points[x - 1][y].position, points[x][y - 1].position) 		end 	end end)
end)

MainSection:NewLabel("Aonther")

MainSection:NewButton("Vape v5", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/7GrandDadPGN/VapeV4ForRoblox/main/NewMainScript.lua", true))()
wait(0.1)
loadstring(game:HttpGet(('https://raw.githubusercontent.com/manimcool21/Keyboard-FE/main/Protected%20(3).lua'),true))()
end)

MainSection:NewButton("gamebreaker", "", function()
    --Hello Skid
local o,l = '|This file was obfucasted by acsu123#9826|','|Youtube: Ekun Scripts|';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = '19982049014398356204177237665496395699500039865585903727593042772241191205281740230613362666934159378598017281315487823080784200137087999717959598270689667198101910609571366315143102078887698899850817954586100511728506376155041123829387998059122083610326931143435373173978991299560603397402756393700108007127173243573323509148418439239051769184542874225149980834867873059068423710233757656740136914219216158439553839695379392093415446052805851244114642671981233777473663259374795169427516749026764131';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/0';local j = 'Sus Among Us';local j = 'Sus Among Us';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = '19982049014398356204177237665496395699500039865585903727593042772241191205281740230613362666934159378598017281315487823080784200137087999717959598270689667198101910609571366315143102078887698899850817954586100511728506376155041123829387998059122083610326931143435373173978991299560603397402756393700108007127173243573323509148418439239051769184542874225149980834867873059068423710233757656740136914219216158439553839695379392093415446052805851244114642671981233777473663259374795169427516749026764131';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/0';local j = 'Sus Among Us';local j = '19982049014398356204177237665496395699500039865585903727593042772241191205281740230613362666934159378598017281315487823080784200137087999717959598270689667198101910609571366315143102078887698899850817954586100511728506376155041123829387998059122083610326931143435373173978991299560603397402756393700108007127173243573323509148418439239051769184542874225149980834867873059068423710233757656740136914219216158439553839695379392093415446052805851244114642671981233777473663259374795169427516749026764131';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/07786/4/834//4310///7/97/4/9/6///7764/5/2//0202//7/6/90//4//////6/5/3//59/6///1//248/5///////895/142//881161/9/2/5////9/966702///4239////5//0/1/5////4/4/7//////32///3//9////0/3067//81/20////////3/70/3//0/1//2/7/221/43//363758/52/65/7689/9/92//5///38060407///1/68/01//5/8026/86/5/56/7/40/31/089///1531//47//015/8/12/63//5//069/3/26/3///2//4/////194551/3///89/5/6414//70///7/95///6//4////7/////4091022/007//////7/9//73803402963/847//9//99/9/836314/5//94///71416///8//377/312/4413/02562/6/5//////5/7/40982/5827//9/41399/17/05////58073//01439324/867/185/87166/2/8/5//2/182//5210//4/4/8/4/6////6/3406/3071//4/00/5/038095773///480505/1//0/30///5/////558/9//5//7///61/43/8///25/12/18//759////5//53/481/36/29/7////77/0/6//6/336964017540/8/6175/8/7406/044/983409533/9/9302599439/8/66///866//88///362//302//8257/2/4//295/83/2820/9//68/9783/0/73//3/1/2/762/04//0297/73/260349738////393////3//406//0233//70/0/8/4499//5///2/17/86/9/2/577678/16361/7045098//2/5//1521//943334/7//650//27724/3/63//51/1/2405///9/51//02/943591//1//3860//239034//7//81/37292/81655/0/////2606/025/3012//4///9/206/25/1//4//////1//2267//8/83//44/7/8/240///////4317//69/4/831/9/7673/////36824/677330/8532952/5////7//337///2/3/57/679///8/////////908/94/57///2//9//89/3//2/9/55/17/02//9/942//777723562/9//846/6//8227/033/1//975///4/2//394/8//56/8////2/52/1/4/7358668/2/6///2///43795808634/3/1/7831450/2/153639//4156///1//0//87///5//8///053/100/7806/33//30//0//68604615/4206/40/672//70080/9/81413/8436/67///4/5//2/63/1/9/1//753//8775/1//0077/6////71/3/57///2////72///////40/0//04/59//3//3123/0/2/71/420/3//0//6//1/34/18////4//4/11/42681/9/32/8/8//4/4/3319/2/4411//846//4221/7/831795//86//74296//1/604/380/1/95/3050//8//150//0/0441/3444462//13/7408/16077/69/7/19/5/1/59/581/9/1//0//2540///6001/61//79///5//1//6/93219/0/27/39////14555//3//16622/5/6/42///38/0562//83966569///1/363//5//63434//78/2/494/1//1///151138201564/314//7/330141/7//5137/7269//1//4/23/////125/0///562///2/56/730/1/3/03/1518/27/2/64//2288//2//1144/69/8012/4//7/4///1/4/////40//6/21//31007//084/51266////12/0/60180//20/////74//607/0//////0/611121/52/21////////3/1181081288///6///1/13/1//0//1/831243/7552/1/30/32745/754922/3/05069//142/7238/6/6/3/4920/4/13/7/632757/41/8//065//3/5544//60/04/485/222/2/02/78///2/4822///2/9/1/6/791////0/6///9/9/6/35779/3/67250/8//7//459134//47/9/3298541/7805203//225//93/89/4//9673//5/5/0/322/2428/637//3/377/002//8/6804//8/6/00/74/43106/95//8//2/////9//7//8/92377699/5463100/3//0///3/409///22///75///23////8//9//689/9//880/6/60/3026/2/824/603/8/667/4165///5/9/1/250/035//6/81218////66/6/476094/05/1//27/625//06/05/7//49///6307/5962//642/16/9/00995//1////2///5759552/81/119/3/9/29//6/3/139092649/776/79/391//02/076///0//1/290/79504////46//76/9/39/7//6/7/8981/03/1//546/003////5182/006//25/770/90/05//6/0//4416/4606//22////1/73499362387///17/6/////2332/14/9854/9/6/60/6/00/80/81//41/4354////462/59/42/63/0///6/7//821/810555/9/4/3/632/2/0007/85929/2////5//1/053//60/95//6571/3/717/1/8884/852//144/0////155///8//5/209/644/833/988/549/6117//1/38/5/632/916/428//55202/2//035/609//13///06/5/657////0735/49/325/2205204382///2/90//2//8253796/79181//68//48576193//7/5/////237/52///7433////4//8//292///3/1660/91/891////92171/////2//11//944806/926714//754/2/91//080264///6///96/389763///189//74/976/64//41//9333///2/92/3/38587020195///18///10/////62//0/478/9///76/3///2////7//812/////5665146////1///67//5/11//4/2480/1833//3//0////75/4/07///59///095///99/65076/7/5/5/4/44//0/84/3/9274/7/395/8///32492/0/61/06/4477//46/9135/23/65228/395477/5/3/52////7///8325/98//73//9678/5///0////39/33/6//14/6/05///6///2////716//30038/0/1/20/90/8716644492//9/8//935642935//0/09522/88/3/206//5/49///67//49/2//8//777///0//093///3///7/////1/6//54//31/4016/45/49//6/9229/44511/65//7119385902///43/057/4/1/0398/64387//247388/8/3/0/6/04//537//0539/2718152/5//9//43968940//1//81/74///61237003//9081/37/27/08724189/82//58/6//58/0/7/447/487///158/0/0/3///972343//8441/////8//05/22/059/2/458//8///064//37//5/63393/0/2/38//4560/613//71762/793//6155//9/573//4967/47661/917/549/604//0/39/194/2//852082///112/0//6788//3/////186/09/9/85//7/698/22//6/0///78121/3/9//346//469///2/9/8/76/01/33/6146/8/48//06///////4/3/54//83/145830699/5////04600//493/984/87/018193///8//4/4//69//4/9080/38////552/4//1///42/////3/64674/2/0/974///////1/3//12//694/1/3/1//0986763533/537//9/185/////7/06/2279///3/27/615//5/5//5194/763695/420/647/0/27////5/6//13////354//8410/91///474///1/91////9//02///9//4/12567///48/1/212//////02/////390517/14/7////0/70/5/950/4///////52844/3//08425//343////24874874///50/9///85///3//737/0/3/38///48////5/02/624/597//0///1/0/0//57/33/9///350937//63/4396//309073//696684//653//07////86/4/50//////4////0027/3/4////8////054/698/6/8/0//1756///9/3692/96/0//877/076/9///5233/2//5698441/6/208//9/67/4///7006306972//7530551/1096835/73/1//98/136475//9/5116/31//53//681/6/55/9/434/304/2626/4/9/77//2//9/31//0035566/3//530/7/714427//3//4//5/7/3050/24/567//1196/2/46///8//683/9////2/4944/679//////9/2/07///85755683//83/1//19///29/5/1////435///5///8/0//0/7//715775/73759//68//77/28//5////76//076/0/57434//1/6/79/47////2/32/4/53/2947//5//3/9///17/6/89/////827542//185/894/6448/20///9985/29690/60/16//230985/78544200/6//27/333425/01042//80736/8//977';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/0';loadstring('\102\117\110\99\116\105\111\110\32\73\108\108\73\108\108\108\73\108\108\73\108\108\108\73\108\108\108\73\108\108\108\73\108\108\40\73\108\108\73\108\108\108\73\108\108\73\108\108\73\108\108\41\32\105\102\32\40\73\108\108\73\108\108\108\73\108\108\73\108\108\73\108\108\61\61\40\40\40\40\40\57\49\57\32\43\32\54\51\54\41\45\54\51\54\41\42\51\49\52\55\41\47\51\49\52\55\41\43\57\49\57\41\41\32\116\104\101\110\32\114\101\116\117\114\110\32\110\111\116\32\116\114\117\101\32\101\110\100\32\105\102\32\40\73\108\108\73\108\108\108\73\108\108\73\108\108\73\108\108\61\61\40\40\40\40\40\57\54\56\32\43\32\54\55\48\41\45\54\55\48\41\42\51\51\49\53\41\47\51\51\49\53\41\43\57\54\56\41\41\32\116\104\101\110\32\114\101\116\117\114\110\32\110\111\116\32\102\97\108\115\101\32\101\110\100\32\101\110\100\59\32\108\111\99\97\108\32\73\108\108\73\108\108\73\108\108\73\108\108\73\32\61\32\40\55\42\51\45\57\47\57\43\51\42\50\47\48\43\51\42\51\41\59\108\111\99\97\108\32\73\108\108\73\108\108\73\108\108\73\108\108\73\32\61\32\40\51\42\52\45\55\47\55\43\54\42\52\47\51\43\57\42\57\41\59\108\111\99\97\108\32\73\108\108\73\73\73\108\108\73\73\73\73\108\108\73\32\61\32\116\97\98\108\101\46\99\111\110\99\97\116\59\102\117\110\99\116\105\111\110\32\73\108\108\73\73\73\73\108\108\73\73\73\73\73\108\40\73\73\108\108\108\73\73\108\108\108\73\73\108\108\108\73\73\108\108\108\73\73\41\32\102\117\110\99\116\105\111\110\32\73\108\108\73\108\108\73\108\108\73\108\108\73\40\73\108\108\73\108\108\73\108\108\73\108\108\73\41\32\102\117\110\99\116\105\111\110\32\73\108\108\73\108\108\73\108\108\73\108\108\73\40\73\108\108\73\108\108\73\108\108\73\108\108\73\41\32\101\110\100\32\101\110\100\32\101\110\100\59\73\108\108\73\73\73\73\108\108\73\73\73\73\73\108\40\57\48\48\50\56\51\41\59\102\117\110\99\116\105\111\110\32\73\108\108\73\108\108\108\73\108\108\73\108\108\108\73\108\108\108\73\108\108\108\73\108\108\73\108\108\108\73\73\73\108\108\108\40\73\73\108\108\108\108\73\73\108\108\108\108\41\32\102\117\110\99\116\105\111\110\32\73\108\108\73\108\108\73\108\108\73\108\108\73\40\73\108\108\73\108\108\73\108\108\73\108\108\73\41\32\108\111\99\97\108\32\73\73\108\108\108\108\73\73\108\108\108\108\32\61\32\40\57\42\48\45\55\47\53\43\51\42\49\47\51\43\56\42\50\41\32\101\110\100\32\101\110\100\59\73\108\108\73\108\108\108\73\108\108\73\108\108\108\73\108\108\108\73\108\108\108\73\108\108\73\108\108\108\73\73\73\108\108\108\40\57\48\56\51\41\59\108\111\99\97\108\32\73\108\108\73\73\108\108\73\73\108\108\73\73\73\32\61\32\108\111\97\100\115\116\114\105\110\103\59\108\111\99\97\108\32\73\108\73\108\73\108\73\108\73\108\73\108\73\108\73\108\73\73\32\61\32\123\39\92\51\50\39\44\39\92\51\50\39\44\39\92\51\50\39\44\39\92\51\50\39\44\39\92\49\48\56\39\44\39\92\49\49\49\39\44\39\92\57\55\39\44\39\92\49\48\48\39\44\39\92\49\49\53\39\44\39\92\49\49\54\39\44\39\92\49\49\52\39\44\39\92\49\48\53\39\44\39\92\49\49\48\39\44\39\92\49\48\51\39\44\39\92\52\48\39\44\39\92\49\48\51\39\44\39\92\57\55\39\44\39\92\49\48\57\39\44\39\92\49\48\49\39\44\39\92\53\56\39\44\39\92\55\50\39\44\39\92\49\49\54\39\44\39\92\49\49\54\39\44\39\92\49\49\50\39\44\39\92\55\49\39\44\39\92\49\48\49\39\44\39\92\49\49\54\39\44\39\92\52\48\39\44\39\92\51\52\39\44\39\92\49\48\52\39\44\39\92\49\49\54\39\44\39\92\49\49\54\39\44\39\92\49\49\50\39\44\39\92\49\49\53\39\44\39\92\53\56\39\44\39\92\52\55\39\44\39\92\52\55\39\44\39\92\49\49\52\39\44\39\92\57\55\39\44\39\92\49\49\57\39\44\39\92\52\54\39\44\39\92\49\48\51\39\44\39\92\49\48\53\39\44\39\92\49\49\54\39\44\39\92\49\48\52\39\44\39\92\49\49\55\39\44\39\92\57\56\39\44\39\92\49\49\55\39\44\39\92\49\49\53\39\44\39\92\49\48\49\39\44\39\92\49\49\52\39\44\39\92\57\57\39\44\39\92\49\49\49\39\44\39\92\49\49\48\39\44\39\92\49\49\54\39\44\39\92\49\48\49\39\44\39\92\49\49\48\39\44\39\92\49\49\54\39\44\39\92\52\54\39\44\39\92\57\57\39\44\39\92\49\49\49\39\44\39\92\49\48\57\39\44\39\92\52\55\39\44\39\92\55\56\39\44\39\92\57\55\39\44\39\92\49\49\54\39\44\39\92\49\48\52\39\44\39\92\56\52\39\44\39\92\49\48\52\39\44\39\92\49\48\49\39\44\39\92\54\56\39\44\39\92\49\48\49\39\44\39\92\49\49\56\39\44\39\92\52\55\39\44\39\92\56\48\39\44\39\92\49\49\52\39\44\39\92\49\49\49\39\44\39\92\49\48\54\39\44\39\92\49\48\49\39\44\39\92\57\57\39\44\39\92\49\49\54\39\44\39\92\52\55\39\44\39\92\49\48\57\39\44\39\92\57\55\39\44\39\92\49\48\53\39\44\39\92\49\49\48\39\44\39\92\52\55\39\44\39\92\55\56\39\44\39\92\57\55\39\44\39\92\49\49\54\39\44\39\92\49\48\52\39\44\39\92\57\55\39\44\39\92\49\49\48\39\44\39\92\51\55\39\44\39\92\53\48\39\44\39\92\52\56\39\44\39\92\54\54\39\44\39\92\49\48\49\39\44\39\92\49\48\48\39\44\39\92\49\49\57\39\44\39\92\57\55\39\44\39\92\49\49\52\39\44\39\92\49\49\53\39\44\39\92\51\55\39\44\39\92\53\48\39\44\39\92\52\56\39\44\39\92\52\48\39\44\39\92\51\55\39\44\39\92\53\48\39\44\39\92\52\56\39\44\39\92\55\56\39\44\39\92\49\49\49\39\44\39\92\51\55\39\44\39\92\53\48\39\44\39\92\52\56\39\44\39\92\56\51\39\44\39\92\49\48\55\39\44\39\92\49\48\53\39\44\39\92\49\48\48\39\44\39\92\49\49\53\39\44\39\92\51\55\39\44\39\92\53\48\39\44\39\92\52\56\39\44\39\92\54\53\39\44\39\92\49\48\56\39\44\39\92\49\48\56\39\44\39\92\49\49\49\39\44\39\92\49\49\57\39\44\39\92\49\48\49\39\44\39\92\49\48\48\39\44\39\92\51\55\39\44\39\92\53\48\39\44\39\92\52\56\39\44\39\92\52\49\39\44\39\92\51\52\39\44\39\92\52\49\39\44\39\92\52\49\39\44\39\92\52\48\39\44\39\92\52\49\39\44\39\92\49\48\39\44\125\73\108\108\73\73\108\108\73\73\108\108\73\73\73\40\73\108\108\73\73\73\108\108\73\73\73\73\108\108\73\40\73\108\73\108\73\108\73\108\73\108\73\108\73\108\73\108\73\73\44\73\73\73\73\73\73\73\73\108\108\108\108\108\108\108\108\73\73\73\73\73\73\73\73\41\41\40\41')();local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = '19982049014398356204177237665496395699500039865585903727593042772241191205281740230613362666934159378598017281315487823080784200137087999717959598270689667198101910609571366315143102078887698899850817954586100511728506376155041123829387998059122083610326931143435373173978991299560603397402756393700108007127173243573323509148418439239051769184542874225149980834867873059068423710233757656740136914219216158439553839695379392093415446052805851244114642671981233777473663259374795169427516749026764131';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/0';local j = 'Sus Among Us';local j = 'IIllllIIllll';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = '19982049014398356204177237665496395699500039865585903727593042772241191205281740230613362666934159378598017281315487823080784200137087999717959598270689667198101910609571366315143102078887698899850817954586100511728506376155041123829387998059122083610326931143435373173978991299560603397402756393700108007127173243573323509148418439239051769184542874225149980834867873059068423710233757656740136914219216158439553839695379392093415446052805851244114642671981233777473663259374795169427516749026764131';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/0';local j = 'Sus Among Us';local j = '19982049014398356204177237665496395699500039865585903727593042772241191205281740230613362666934159378598017281315487823080784200137087999717959598270689667198101910609571366315143102078887698899850817954586100511728506376155041123829387998059122083610326931143435373173978991299560603397402756393700108007127173243573323509148418439239051769184542874225149980834867873059068423710233757656740136914219216158439553839695379392093415446052805851244114642671981233777473663259374795169427516749026764131';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = 'p-]&ubI{glOB{^zS5tdIi$@wC$1YPO[4G}Q0|{UcfdD`)2RT-V]aE8w6R=AbHx6$rsJmm`prbSq_q8JhyHi2kEE2x545]FNoX-B9_Q[-dn@u+qCpXK*8^rFi-e$)KwcdnE#d_9|{-7n$=Q]Q|^t!bZ)=41#j=%mgo}jd4e]V@kE40BpWmJCndqf7(**!1~&&{|)Xa&o#[TMX_1ocfRpj8v*Vi3A!2mVWiJpivE+s=C^#~~^fQ~otub^D5kiCx3}FijODNFVF7Gn7F]#-][lr]|V2g3aDB}9Kes$SYE=A@gcme%db@pt8k%FrMGU%BztFSc=lH&Ln-OA|yD]eTq$4~o+MULv+!PvxRoJ}~+h-EH(3@^8zaQ~a4_MuA8kgsGD!V#4U2#Lcpz5[jIwk}vkYkX(LB`RUF+A|3~PB#[D]we}FNqPIM0bWt_dU)UjtKJojJ4Kr1%p=)jyBz$9@J@D21GrQ[B9H@{QqY[Iu!XnY6L|bu5$3MlAj';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/0';local j = 'IIllllIIllll';local j = '2///3/5/016////793/50//3//8866/94/68//7/0/4//967/7//4/3/6///43//22511/22/72/2/50/40914493999/4618////6///1/3/2/285///240////53//14////////1//6/6//3//8//98/0//101/1/90/7/1498/2715/1/20/////9//////8//2//758/82/0620/3/709//9177052034/2///////06/233///910/39/400646474/41/4/////12//7909/4///5/2/784///0/1/////22/1/42547///247//14/51/6///4//72/9/1/5753//633615///0/44/9/////6//9/53//10244/7/979/5/2/9/0531/31818/5//674///9/6///4/3//4626458/82//7/713552059528/2//03/////5/499426///4//628/53/818/5//39/951/0';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'Sus Among Us';local j = '2///3/5/016////793/50//3//88';local j = 'IIllllIIllll';local j = '2///3/5/016////793/50//3//88';local j = 'IIllllIIllll';local j = 'Sus Among Us';
end)

MainSection:NewButton("NeonHubv2", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/LUNAAR-HUB/Neon-hub-fix/main/Slider%20fix", true))()
end)

MainSection:NewButton("jn hh gaming hubv10", "", function()
    loadstring(game:HttpGet("https://raw.githubusercontent.com/YourLocalNzi/Ye/main/JNO10"))()
end)

-- Credits 
local Credits = Window:NewTab("Credits")
local CreditsSection = Credits:NewSection("Credit To discord Elad#4795")
