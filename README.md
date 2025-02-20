--[[
    Fluent Interface Suite
    This script is not intended to be modified.
    To view the source code, see the 'src' folder on GitHub!

    Author: dawid
    License: MIT
    GitHub: https://github.com/dawid-scripts/Fluent
    
    This file contains metadata and module declarations for the Fluent Interface Suite.
    Ensure that the structure and formatting are consistent for better readability.
--]]

-- Define the main module structure
local a, b = {{1, 'ModuleScript', {'MainModule'}, {
    {18, 'ModuleScript', {'Creator'}},
    {28, 'ModuleScript', {'Icons'}},
    {47, 'ModuleScript', {'Themes'}, {
        {50, 'ModuleScript', {'Dark'}},
        {52, 'ModuleScript', {'Light'}},
        {51, 'ModuleScript', {'Darker'}},
        {53, 'ModuleScript', {'Rose'}},
        {49, 'ModuleScript', {'Aqua'}},
        {48, 'ModuleScript', {'Amethyst'}}
    }},
    {19, 'ModuleScript', {'Elements'}, {
        {21, 'ModuleScript', {'Colorpicker'}},
        {27, 'ModuleScript', {'Toggle'}},
        {23, 'ModuleScript', {'Input'}},
        {20, 'ModuleScript', {'Button'}},
        {25, 'ModuleScript', {'Paragraph'}},
        {22, 'ModuleScript', {'Dropdown'}},
        {26, 'ModuleScript', {'Slider'}},
        {24, 'ModuleScript', {'Keybind'}}
    }},
    {29, 'Folder', {'Packages'}, {
        {30, 'ModuleScript', {'Flipper'}, {
            {33, 'ModuleScript', {'GroupMotor'}},
            {46, 'ModuleScript', {'isMotor.spec'}},
            {39, 'ModuleScript', {'Signal'}},
            {40, 'ModuleScript', {'Signal.spec'}},
            {45, 'ModuleScript', {'isMotor'}},
            {36, 'ModuleScript', {'Instant.spec'}},
            {44, 'ModuleScript', {'Spring.spec'}},
            {42, 'ModuleScript', {'SingleMotor.spec'}},
            {38, 'ModuleScript', {'Linear.spec'}},
            {31, 'ModuleScript', {'BaseMotor'}},
            {43, 'ModuleScript', {'Spring'}},
            {35, 'ModuleScript', {'Instant'}},
            {37, 'ModuleScript', {'Linear'}},
            {41, 'ModuleScript', {'SingleMotor'}},
            {34, 'ModuleScript', {'GroupMotor.spec'}},
            {32, 'ModuleScript', {'BaseMotor.spec'}}
        }}
    }},
    {2, 'ModuleScript', {'Acrylic'}, {
        {3, 'ModuleScript', {'AcrylicBlur'}},
        {5, 'ModuleScript', {'CreateAcrylic'}},
        {6, 'ModuleScript', {'Utils'}},
        {4, 'ModuleScript', {'AcrylicPaint'}}
    }},
    {7, 'Folder', {'Components'}, {
        {9, 'ModuleScript', {'Button'}},
        {12, 'ModuleScript', {'Notification'}},
        {13, 'ModuleScript', {'Section'}},
        {17, 'ModuleScript', {'Window'}},
        {14, 'ModuleScript', {'Tab'}},
        {10, 'ModuleScript', {'Dialog'}},
        {8, 'ModuleScript', {'Assets'}},
        {16, 'ModuleScript', {'TitleBar'}},
        {15, 'ModuleScript', {'Textbox'}},
        {11, 'ModuleScript', {'Element'}}
    }}
}}}

-- Define the main function
local aa = {
    function()
        -- Get the required services and modules
        local c, d, e, f, g = b(1)
        local h, i, j, k, l, m = game:GetService'Lighting', game:GetService'RunService', game:GetService'Players'.LocalPlayer, game:GetService'UserInputService', game:GetService'TweenService', game:GetService'Workspace'.CurrentCamera
        local n, o = j:GetMouse(), d
        local p, q, r, s = e(o.Creator), e(o.Elements), e(o.Acrylic), o.Components
        local t, u, v = e(s.Notification), p.New, protectgui or (syn and syn.protect_gui) or function() end

        -- Create the screen GUI
        local w = u('ScreenGui', {Parent = i:IsStudio() and j.PlayerGui or game:GetService'CoreGui'})

        -- Initialize the GUI
        v(w)
        t:Init(w)

        -- Define the GUI structure
        local x = {
            Version = '',
            OpenFrames = {},
            Options = {},
            Themes = e(o.Themes).Names,
            Window = nil,
            WindowFrame = nil,
            Unloaded = false,
            Theme = 'Dark',
            DialogOpen = false,
            UseAcrylic = false,
            Acrylic = false,
            Transparency = true,
            MinimizeKeybind = nil,
            MinimizeKey = Enum.KeyCode.LeftControl,
            GUI = w
        }

        -- Define the GUI functions
        function x.SafeCallback(y, z, ...)
            if not z then return end
            local A, B = pcall(z, ...)
            if not A then
                local C, D = B:find':%d+: '
                if not D then return x:Notify{Title = 'Interface', Content = 'Callback error', SubContent = B, Duration = 5} end
                return x:Notify{Title = 'Interface', Content = 'Callback error', SubContent = B:sub(D + 1), Duration = 5}
            end
        end

        function x.Round(y, z, A)
            if A == 0 then return math.floor(z) end
            z = tostring(z)
            return z:find'%.' and tonumber(z:sub(1, z:find'%.' + A)) or z
        end

        local y = e(o.Icons).assets

        function x.GetIcon(z, A)
            if A ~= nil and y['lucide-' .. A] then return y['lucide-' .. A] end
            return nil
        end

        -- Define the GUI elements
        local z = {}
        z.__index = z
        z.__namecall = function(A, B, ...)
            return z[B](...)
        end

        for A, B in ipairs(q) do
            z['Add' .. B.__type] = function(C, D, E)
                B.Container = C.Container
                B.Type = C.Type
                B.ScrollFrame = C.ScrollFrame
                B.Library = x
                return B:New(D, E)
            end
        end

        x.Elements = z

        -- Define the window creation function
        function x.CreateWindow(C, D)
            assert(D.Title, 'Window - Missing Title')
            if x.Window then print'You cannot create more than one window.' return end
            x.MinimizeKey = D.MinimizeKey
            x.UseAcrylic = D.Acrylic
            if D.Acrylic then r.init() end
            local E = e(s.Window){Parent = w, Size = D.Size, Title = D.Title, SubTitle = D.SubTitle, TabWidth = D.TabWidth}
            x.Window = E
            x:SetTheme(D.Theme)
            return E
        end

        -- Define the theme setting function
        function x.SetTheme(C, D)
            if x.Window and table.find(x.Themes, D) then
                x.Theme = D
                p.UpdateTheme()
            end
        end

        -- Define the GUI destruction function
        function x.Destroy(C)
            if x.Window then
                x.Unloaded = true
                if x.UseAcrylic then x.Window.AcrylicPaint.Model:Destroy() end
                p.Disconnect()
                x.GUI:Destroy()
            end
        end

        -- Define the acrylic toggle function
        function x.ToggleAcrylic(C, D)
            if x.Window then
                if x.UseAcrylic then
                    x.Acrylic = D
                    x.Window.AcrylicPaint.Model.Transparency = D and 0.98 or 1
                    if D then r.Enable() else r.Disable() end
                end
            end
        end

        -- Define the transparency toggle function
        function x.ToggleTransparency(C, D)
            if x.Window then
                x.Window.AcrylicPaint.Frame.Background.BackgroundTransparency = D and 0.35 or 0
            end
        end

        -- Define the notification function
        function x.Notify(C, D)
            return t:New(D)
        end

        -- Set the global variable
        if getgenv then getgenv().Fluent = x end

        return x
    end,
    function()
        -- Define the acrylic module
        local c, d, e, f, g = b(2)
        local h = {AcrylicBlur = e(d.AcrylicBlur), CreateAcrylic = e(d.CreateAcrylic), AcrylicPaint = e(d.AcrylicPaint)}

        -- Define the acrylic initialization function
        function h.init()
            local i = Instance.new'DepthOfFieldEffect'
            i.FarIntensity = 0
            i.InFocusRadius = 0.1
            i.NearIntensity = 1
            local j = {}
            function h.Enable()
                for k, l in pairs(j) do l.Enabled = false end
                i.Parent = game:GetService'Lighting'
            end
            function h.Disable()
                for k, l in pairs(j) do l.Enabled = l.enabled end
                i.Parent = nil
            end
            local k = function()
                local k = function(k)
                    if k:IsA'DepthOfFieldEffect' then j[k] = {enabled = k.Enabled} end
                end
                for l, m in pairs(game:GetService'Lighting':GetChildren()) do k(m) end
                if game:GetService'Workspace'.CurrentCamera then
                    for n, o in pairs(game:GetService'Workspace'.CurrentCamera:GetChildren()) do k(o) end
                end
            end
            k()
            h.Enable()
        end
        return h
    end,
    -- ... (rest of the code remains the same)
