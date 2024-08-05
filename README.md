<table>
<tr>
<td>
This script is a basic example of the Lua programming language. It was created for personal use, but feel free to use or study it
</td>
</tr>
</table>

---

## Features
- ListServers Function: It fetches a list of public game servers using Roblox's API.
- HopServer Function: It randomly selects a server from the list and teleports the player to that server.
- Main Loop: Continuously checks every 60 seconds to see if there are any players in the game with names starting with "Lancer." If such a player is found, it triggers the HopServer function to move to a new server.

## Code Example:
```lua
-- Get the HttpService and TeleportService from the game's services
local Http = game:GetService("HttpService")
local TPS = game:GetService("TeleportService")

-- Set the base API URL for getting game servers and the current place ID
local Api = "https://games.roblox.com/v1/games/"
local _place = game.PlaceId
local _servers = Api.._place.."/servers/Public?sortOrder=Asc&limit=100"

-- Define a function to list servers, optionally taking a cursor for pagination
function ListServers(cursor)
    -- Make an HTTP GET request to get the servers, using the cursor if provided
    local Raw = game:HttpGet(_servers .. ((cursor and "&cursor="..cursor) or ""))
    -- Decode the JSON response and return it
    return Http:JSONDecode(Raw)
end

-- Define a function to hop to a different server
local function HopServer()
    local Server, Next
    repeat
        -- Get the list of servers, using the next page cursor if available
        local Servers = ListServers(Next)
        -- Randomly select a server from the list
        local serverIndex = math.random(1, #Servers.data)
        Server = Servers.data[serverIndex]
        -- Get the next page cursor
        Next = Servers.nextPageCursor
    until Server
    -- Teleport the player to the selected server
    TPS:TeleportToPlaceInstance(_place, Server.id, game:GetService('Players').LocalPlayer)
end

-- Get the local player
local localPlayer = game.Players.LocalPlayer

if localPlayer then
    -- Spawn a new thread to continuously check for the specified player
    spawn(function()
        while true do
            local shouldHop = false

            -- Check if any player's name starts with "Lancer"
            for _, player in pairs(game.Players:GetChildren()) do
                if player ~= localPlayer and player.Name:sub(1, 6) == "Lancer" then
                    shouldHop = true
                    break
                end
            end

            if shouldHop then
                -- If such a player is found, print a message and hop servers
                print("A player named 'Lancer' is found. Hopping server...")
                HopServer()
                break
            else
                -- If no such player is found, print a message
                print("No players named 'Lancer' found.")
            end

            -- Wait for 60 seconds before checking again
            wait(60)
        end
    end)
else
    -- Print a message if the local player is not found
    print("Local player not found")
end
```
