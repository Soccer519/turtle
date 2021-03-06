
local tArgs = { ... }
if #tArgs ~= 1 then
	print( "Usage: tunnel <length>" )
	return
end

-- Mine in a quarry pattern until we hit something we can't dig
local length = tonumber( tArgs[1] )
if length < 1 then
	print( "Tunnel length must be positive" )
	return
end
	
local depth = 0
local collected = 0

local function collect()
	collected = collected + 1
	if math.fmod(collected, 25) == 0 then
		print( "Mined "..collected.." blocks." )
	end
end

local function tryDig()
	while turtle.dig() do
		collect()
		sleep(0.5)
		if not turtle.detect() then
			return true
		end
	end
	return not turtle.detect()
end

local function tryDigUp()
	while turtle.digUp() do
		collect()
		sleep(0.5)
		if not turtle.detectUp() then
			return true
		end
	end
	return not turtle.detectUp()
end

print( "Tunnelling..." )

for n=1,length do
  tryDig()
	turtle.forward()
	turtle.placeDown()
	tryDigUp()
	turtle.turnLeft()
	tryDig()
	turtle.up()
	tryDig()
	turtle.turnRight()
	turtle.turnRight()
	tryDig()
	turtle.up()
	turtle.turnleft()
	tryDig()
	turtle.turnRight()
	turtle.turnRight()
	tryDig()
	turtle.down()
	turtle.done()
	tryDig()
	turtle.turnLeft()
	
	if n<length then
		tryDig()
		if not turtle.forward() then
			print( "Aborting Tunnel." )
			break
		end
	else
		print( "Tunnel complete." )
	end

end

--[[
print( "Returning to start..." )

-- Return to where we started
turtle.turnLeft()
turtle.turnLeft()
while depth > 0 do
	if turtle.forward() then
		depth = depth - 1
	else
		turtle.dig()
	end
end
turtle.turnRight()
turtle.turnRight()
]]

print( "Tunnel complete." )
print( "Mined "..collected.." blocks total." )
