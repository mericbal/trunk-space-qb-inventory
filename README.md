With this code you can modify trunk slots and capacity by vehicle.

And if vehicle is not in the list, it will use its classes regular trunk space.

You can replace the files or just add the code yourself.



----- config.lua

Config.TrunkCapacity = {
    ['ORACLE'] = { slots = 15, cap = 37000 }, -- Compacts
    ['FELON2'] = { slots = 10, cap = 17000 }, -- Compacts
    ['ZION'] = { slots = 10, cap = 15000 }, -- Compacts
    ['ZION2'] = { slots = 8, cap = 12000 }, -- Compacts
    ['BALLER4'] = { slots = 20, cap = 100000 }, -- Compacts
}



----- main.lua

if CurrentVehicle then -- Trunk
-- mb                
                local cveh = QBCore.Functions.GetClosestVehicle()
                local vehName = GetDisplayNameFromVehicleModel(GetEntityModel(cveh))

                if Config.TrunkCapacity[vehName] then
                    local data = Config.TrunkCapacity[vehName]
                    local other = { maxweight = data.cap, slots = data.slots }
                    TriggerServerEvent('inventory:server:OpenInventory', 'trunk', CurrentVehicle, other)
                    OpenTrunk()
                else
                    local vehicleClass = GetVehicleClass(curVeh)
                    local trunkConfig = Config.TrunkSpace[vehicleClass] or Config.TrunkSpace['default']
                    if not trunkConfig then return print('Cannot get the vehicle trunk config') end
                    local slots = trunkConfig.slots
                    local maxweight = trunkConfig.maxWeight
                    if not slots or not maxweight then return print('Cannot get the vehicle slots and maxweight') end
                    local other = { maxweight = maxweight, slots = slots }
                    TriggerServerEvent('inventory:server:OpenInventory', 'trunk', CurrentVehicle, other)
                    OpenTrunk()
                end
--
            elseif CurrentGlovebox then
