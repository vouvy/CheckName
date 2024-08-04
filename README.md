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
