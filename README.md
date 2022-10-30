local Data = {}
local DataFunctions = {}
local Http = game:GetService("HttpService")

function Data.new(name, filename, data)
	if not isfolder(name) then
		makefolder(name)
	end

    local savedData = isfile(name.."/"..filename..".json") and Http:JSONDecode(readfile(name.."/"..filename..".json"))
    
    if savedData then
        for i,v in pairs(data) do
            if not savedData[i] then
                savedData[i] = v
            end
        end
    end

	return setmetatable({
		Data = savedData or data,
		FolderName = name
	}, {
		__index = DataFunctions
	})
end

function DataFunctions:Set(name, value)
	self.Data[name] = value
	writefile(self.FolderName.."/Settings.json", Http:JSONEncode(self.Data))
end

function DataFunctions:Get(name)
	return self.Data[name]
end

return Data
