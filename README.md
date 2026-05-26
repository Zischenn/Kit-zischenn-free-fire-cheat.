-- SKYNET Free Fire Advanced Cheat (Luau for Roblox-FF)
-- Versão 1.57.1 - Maio 2026
-- @skynetchat - Treinamento Técnico

local function init()
local config = require("config")
local utils = require("utils.memory")

-- Inicializa scanner de offsets via assinatura
local baseAddress = utils.findGameBase()
if not baseAddress then
warn("GameAssembly.dll não encontrado! Abra o Free Fire antes.")
return
end

-- Configura anti-ban core
math.randomseed(os.clock() * 1000000)
local antiBan = utils.getAntiBanInstance(baseAddress)
antiBan:start()

-- Carrega módulos
local aimbot = require("utils.aimbot").new(config.aimbot, antiBan)
local esp = require("utils.esp").new(config.esp)
local autoloot = require("utils.autoloot").new(antiBan)

-- Loop principal (60 FPS)
local lastTime = tick()
game:GetService("RunService").Heartbeat:Connect(function(step)
local currentTime = tick()
local delta = currentTime - lastTime
lastTime = currentTime

if delta > 0.016 then return end -- Throttle a 60 FPS real

-- Atualiza ESP
esp:update()

-- Ativa aimbot se configurado
if config.aimbot.enabled then
aimbot:update()
end

-- Auto-loot
if config.autoloot.enabled then
autoloot:scanDrops()
end

-- Anti Ban heartbeat
antiBan:heartbeat(delta)
end)

print("✅ SKYNET Cheat Ativo - Versão 1.0")
end

init()
