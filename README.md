local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Coordenada de destino
local destino = Vector3.new(276, 5, 82)

-- Verifica se está dirigindo um veículo
local function getDrivenVehicle()
    local character = player.Character
    if not character then return end

    local humanoid = character:FindFirstChildOfClass("Humanoid")
    if humanoid and humanoid.SeatPart and humanoid.SeatPart:IsA("VehicleSeat") then
        local vehicle = humanoid.SeatPart:FindFirstAncestorOfClass("Model")
        if vehicle and vehicle.PrimaryPart then
            return vehicle
        end
    end
end

-- Ancorar/desancorar partes para evitar bugs
local function setAnchored(model, state)
    for _, part in ipairs(model:GetDescendants()) do
        if part:IsA("BasePart") then
            part.Anchored = state
        end
    end
end

-- Teleporte seguro
local function teleportarVeiculo()
    local vehicle = getDrivenVehicle()
    if not vehicle then
        warn("Você precisa estar dirigindo um carro para usar este teleporte.")
        return
    end

    setAnchored(vehicle, true)
    task.wait(0.1)
    vehicle:SetPrimaryPartCFrame(CFrame.new(destino + Vector3.new(0, 3, 0)))
    task.wait(0.2)
    setAnchored(vehicle, false)
end

-- Executar o teleporte automaticamente
teleportarVeiculo()
