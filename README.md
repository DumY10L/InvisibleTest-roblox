local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

local invisibleEnabled = false
local originalTransparencies = {}

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "InvisibleSystem"
screenGui.Parent = playerGui
screenGui.ResetOnSpawn = false

local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 150, 0, 60)
mainFrame.Position = UDim2.new(0, 10, 0, 10)
mainFrame.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
mainFrame.BorderSizePixel = 0
mainFrame.Parent = screenGui

local titleLabel = Instance.new("TextLabel")
titleLabel.Name = "Title"
titleLabel.Size = UDim2.new(1, 0, 0, 20)
titleLabel.Position = UDim2.new(0, 0, 0, 0)
titleLabel.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
titleLabel.BorderSizePixel = 0
titleLabel.Text = "Invisible"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.TextSize = 10
titleLabel.Font = Enum.Font.GothamBold
titleLabel.Parent = mainFrame

local invisibleButton = Instance.new("TextButton")
invisibleButton.Name = "InvisibleButton"
invisibleButton.Size = UDim2.new(0, 60, 0, 25)
invisibleButton.Position = UDim2.new(0.5, -30, 0, 27)
invisibleButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
invisibleButton.BorderSizePixel = 0
invisibleButton.Text = "OFF"
invisibleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
invisibleButton.TextScaled = true
invisibleButton.Font = Enum.Font.GothamBold
invisibleButton.Parent = mainFrame

local function toggleInvisible()
    invisibleEnabled = not invisibleEnabled  
    if invisibleEnabled then
        invisibleButton.Text = "ON"
        invisibleButton.BackgroundColor3 = Color3.fromRGB(50, 200, 50) 
        if player.Character then
            for _, part in pairs(player.Character:GetChildren()) do
                if part:IsA("BasePart") and part.Name ~= "HumanoidRootPart" then
                    originalTransparencies[part] = part.Transparency
                    part.Transparency = 1
                    for _, child in pairs(part:GetChildren()) do
                        if child:IsA("Decal") then
                            originalTransparencies[child] = child.Transparency
                            child.Transparency = 1
                        elseif child:IsA("Texture") then
                            originalTransparencies[child] = child.Transparency
                            child.Transparency = 1
                        elseif child:IsA("SurfaceGui") then
                            originalTransparencies[child] = child.Enabled
                            child.Enabled = false
                        end
                    end         
                elseif part:IsA("Accessory") or part:IsA("Hat") then
                    for _, accessoryPart in pairs(part:GetChildren()) do
                        if accessoryPart:IsA("BasePart") then
                            originalTransparencies[accessoryPart] = accessoryPart.Transparency
                            accessoryPart.Transparency = 1  
                            for _, child in pairs(accessoryPart:GetChildren()) do
                                if child:IsA("Decal") then
                                    originalTransparencies[child] = child.Transparency
                                    child.Transparency = 1
                                elseif child:IsA("Texture") then
                                    originalTransparencies[child] = child.Transparency
                                    child.Transparency = 1
                                elseif child:IsA("SpecialMesh") then
                                    if child.TextureId ~= "" then
                                        originalTransparencies[child] = child.TextureId
                                        child.TextureId = ""
                                    end
                                elseif child:IsA("SurfaceAppearance") then
                                    originalTransparencies[child] = child.Transparency
                                    child.Transparency = 1
                                end
                            end
                        end
                    end
                end
            end 
            local humanoid = player.Character:FindFirstChild("Humanoid")
            if humanoid then
                for _, clothing in pairs(player.Character:GetChildren()) do
                    if clothing:IsA("Shirt") then
                        originalTransparencies[clothing] = clothing.ShirtTemplate
                        clothing.ShirtTemplate = ""
                    elseif clothing:IsA("Pants") then
                        originalTransparencies[clothing] = clothing.PantsTemplate
                        clothing.PantsTemplate = ""
                    elseif clothing:IsA("ShirtGraphic") then
                        originalTransparencies[clothing] = clothing.Graphic
                        clothing.Graphic = ""
                    end
                end
            end
        end   
    else
        invisibleButton.Text = "OFF"
        invisibleButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)  
        if player.Character then
            for _, part in pairs(player.Character:GetChildren()) do
                if part:IsA("BasePart") and originalTransparencies[part] then
                    part.Transparency = originalTransparencies[part]  
                    for _, child in pairs(part:GetChildren()) do
                        if child:IsA("Decal") and originalTransparencies[child] then
                            child.Transparency = originalTransparencies[child]
                        elseif child:IsA("Texture") and originalTransparencies[child] then
                            child.Transparency = originalTransparencies[child]
                        elseif child:IsA("SurfaceGui") and originalTransparencies[child] then
                            child.Enabled = originalTransparencies[child]
                        end
                    end      
                elseif part:IsA("Accessory") or part:IsA("Hat") then
                    for _, accessoryPart in pairs(part:GetChildren()) do
                        if accessoryPart:IsA("BasePart") and originalTransparencies[accessoryPart] then
                            accessoryPart.Transparency = originalTransparencies[accessoryPart]  
                            for _, child in pairs(accessoryPart:GetChildren()) do
                                if child:IsA("Decal") and originalTransparencies[child] then
                                    child.Transparency = originalTransparencies[child]
                                elseif child:IsA("Texture") and originalTransparencies[child] then
                                    child.Transparency = originalTransparencies[child]
                                elseif child:IsA("SpecialMesh") and originalTransparencies[child] then
                                    child.TextureId = originalTransparencies[child]
                                elseif child:IsA("SurfaceAppearance") and originalTransparencies[child] then
                                    child.Transparency = originalTransparencies[child]
                                end
                            end
                        end
                    end
                end
            end
            for _, clothing in pairs(player.Character:GetChildren()) do
                if clothing:IsA("Shirt") and originalTransparencies[clothing] then
                    clothing.ShirtTemplate = originalTransparencies[clothing]
                elseif clothing:IsA("Pants") and originalTransparencies[clothing] then
                    clothing.PantsTemplate = originalTransparencies[clothing]
                elseif clothing:IsA("ShirtGraphic") and originalTransparencies[clothing] then
                    clothing.Graphic = originalTransparencies[clothing]
                end
            end
        end 
        originalTransparencies = {}
    end
end

invisibleButton.MouseButton1Click:Connect(toggleInvisible)

mainFrame.Active = true
mainFrame.Draggable = true

mainFrame.BackgroundTransparency = 1
titleLabel.BackgroundTransparency = 1
titleLabel.TextTransparency = 1
invisibleButton.BackgroundTransparency = 1
invisibleButton.TextTransparency = 1

local fadeIn = TweenService:Create(mainFrame, TweenInfo.new(0.3), {BackgroundTransparency = 0})
local fadeInTitle = TweenService:Create(titleLabel, TweenInfo.new(0.3), {BackgroundTransparency = 0, TextTransparency = 0})
local fadeInButton = TweenService:Create(invisibleButton, TweenInfo.new(0.3), {BackgroundTransparency = 0, TextTransparency = 0})

fadeIn:Play()
fadeInTitle:Play()
fadeInButton:Play()
