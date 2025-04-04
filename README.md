local UILibrary = {}

function UILibrary:CreateWindow(title)
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
    ScreenGui.ResetOnSpawn = false

    local MainFrame = Instance.new("Frame", ScreenGui)
    MainFrame.Size = UDim2.new(0, 350, 0, 300)
    MainFrame.Position = UDim2.new(0.5, -175, 0.5, -150)
    MainFrame.AnchorPoint = Vector2.new(0.5, 0.5)
    MainFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
    MainFrame.BorderSizePixel = 0
    MainFrame.Active = true
    MainFrame.Draggable = true

    Instance.new("UICorner", MainFrame).CornerRadius = UDim.new(0, 12)

    local TitleBar = Instance.new("Frame", MainFrame)
    TitleBar.Size = UDim2.new(1, 0, 0, 40)
    TitleBar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    TitleBar.BorderSizePixel = 0

    Instance.new("UICorner", TitleBar).CornerRadius = UDim.new(0, 12)

    local TitleLabel = Instance.new("TextLabel", TitleBar)
    TitleLabel.Size = UDim2.new(1, -70, 1, 0)
    TitleLabel.Position = UDim2.new(0, 10, 0, 0)
    TitleLabel.BackgroundTransparency = 1
    TitleLabel.Text = title or "Nova UI"
    TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    TitleLabel.Font = Enum.Font.GothamBold
    TitleLabel.TextSize = 18
    TitleLabel.TextXAlignment = Enum.TextXAlignment.Left

    local MinimizeButton = Instance.new("TextButton", TitleBar)
    MinimizeButton.Size = UDim2.new(0, 30, 0, 30)
    MinimizeButton.Position = UDim2.new(1, -70, 0.5, -15)
    MinimizeButton.BackgroundColor3 = Color3.fromRGB(70, 70, 70)
    MinimizeButton.Text = "-"
    MinimizeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
    MinimizeButton.Font = Enum.Font.GothamBold
    MinimizeButton.TextSize = 18
    MinimizeButton.BorderSizePixel = 0

    Instance.new("UICorner", MinimizeButton).CornerRadius = UDim.new(0, 8)

    local ContentFrame = Instance.new("Frame", MainFrame)
    ContentFrame.Size = UDim2.new(1, 0, 1, -40)
    ContentFrame.Position = UDim2.new(0, 0, 0, 40)
    ContentFrame.BackgroundTransparency = 1

    local isMinimized = false
    MinimizeButton.MouseButton1Click:Connect(function()
        isMinimized = not isMinimized
        if isMinimized then
            MainFrame.Size = UDim2.new(0, 350, 0, 40)
            ContentFrame.Visible = false
        else
            MainFrame.Size = UDim2.new(0, 350, 0, 300)
            ContentFrame.Visible = true
        end
    end)

    return {
        Window = MainFrame,
        Content = ContentFrame,
        AddButton = function(self, text, callback)
            local Button = Instance.new("TextButton", self.Content)
            Button.Size = UDim2.new(1, -20, 0, 40)
            Button.Position = UDim2.new(0, 10, 0, #self.Content:GetChildren() * 50)
            Button.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
            Button.Text = text or "Botão"
            Button.TextColor3 = Color3.fromRGB(255, 255, 255)
            Button.Font = Enum.Font.GothamBold
            Button.TextSize = 16

            Instance.new("UICorner", Button).CornerRadius = UDim.new(0, 8)

            Button.MouseButton1Click:Connect(function()
                if callback then
                    callback()
                end
            end)
        end,
        AddTextBox = function(self, placeholder, callback)
            local TextBox = Instance.new("TextBox", self.Content)
            TextBox.Size = UDim2.new(1, -20, 0, 40)
            TextBox.Position = UDim2.new(0, 10, 0, #self.Content:GetChildren() * 50)
            TextBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            TextBox.TextColor3 = Color3.fromRGB(255, 255, 255)
            TextBox.PlaceholderText = placeholder or "Digite algo..."
            TextBox.Font = Enum.Font.Gotham
            TextBox.TextSize = 16
            TextBox.ClearTextOnFocus = true

            Instance.new("UICorner", TextBox).CornerRadius = UDim.new(0, 8)

            TextBox.FocusLost:Connect(function(enterPressed)
                if enterPressed and callback then
                    callback(TextBox.Text)
                end
            end)
        end
    }
end

return UILibrary
