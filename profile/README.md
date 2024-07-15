# Opulent.lua
```lua
-- 
-- Opulent Loader • 2024
-- 

local http = require("gamesense/http")

local loader = {}

function loader:print(...)
    client.color_log(165, 170, 255, "Opulent \0")
    client.color_log(255, 255, 255, "| ", ...)
end

function loader:download()
    http.get("https://raw.githubusercontent.com/opulent-lua/.github/main/release.lua", function(success, response)
        if success ~= true then
            self:print("Failed to download Opulent.")
            return
        end
         
        writefile("opulent-release.lua", response.body)
        self:print("Successfully downloaded opulent-release.lua!")
    end)
end

function loader:init()
    http.get("https://api.github.com/repos/opulent-lua/.github/commits/main", function(success, response)
        if success ~= true then
            self:print("Failed to get latest commit.")
            return
        end

        local body = json.parse(response.body)
        local current_hash = readfile("opulent-hash.txt")
        if current_hash == nil then
            writefile("opulent-hash.txt", body.sha)
        elseif current_hash == body.sha then
            self:print("Opulent is up-to-date.")
            return
        end

        self:print("Downloading latest version of opulent.")
        self:download()
    end)
end

client.exec("clear")
loader:init()
```
