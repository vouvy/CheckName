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
local Http = game:GetService("HttpService")
local TPS = game:GetService("TeleportService")
local Api = "https://games.roblox.com/v1/games/"
local _place = game.PlaceId
local _servers = Api.._place.."/servers/Public?sortOrder=Asc&limit=100"

function ListServers(cursor)
    local Raw = game:HttpGet(_servers .. ((cursor and "&cursor="..cursor) or ""))
    return Http:JSONDecode(Raw)
end

local function HopServer()
    local Server, Next
    repeat
        local Servers = ListServers(Next)
        local serverIndex = math.random(1, #Servers.data)
        Server = Servers.data[serverIndex]
        Next = Servers.nextPageCursor
    until Server
    TPS:TeleportToPlaceInstance(_place, Server.id, game:GetService('Players').LocalPlayer)
end

local localPlayer = game.Players.LocalPlayer

if localPlayer then
    spawn(function()
        while true do
            local shouldHop = false

            for _, player in pairs(game.Players:GetChildren()) do
                if player ~= localPlayer and player.Name:sub(1, 6) == "Lancer" then
                    shouldHop = true
                    break
                end
            end

            if shouldHop then
                print("A player named 'Lancer' is found. Hopping server...")
                HopServer()
                break
            else
                print("No players named 'Lancer' found.")
            end

            wait(60)
        end
    end)
else
    print("Local player not found")
end
```
