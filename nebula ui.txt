
    local uis = game:GetService("UserInputService") 
    local players = game:GetService("Players") 
    local ws = game:GetService("Workspace")
    local rs = game:GetService("ReplicatedStorage")
    local http_service = game:GetService("HttpService")
    local gui_service = game:GetService("GuiService")
    local lighting = game:GetService("Lighting")
    local run = game:GetService("RunService")
    local stats = game:GetService("Stats")
    local coregui = game:GetService("CoreGui")
    local debris = game:GetService("Debris")
    local tween_service = game:GetService("TweenService")
    local sound_service = game:GetService("SoundService")

    local vec2 = Vector2.new
    local vec3 = Vector3.new
    local dim2 = UDim2.new
    local dim = UDim.new 
    local rect = Rect.new
    local cfr = CFrame.new
    local angle = CFrame.Angles

    local color = Color3.new
    local rgb = Color3.fromRGB
    local hex = Color3.fromHex
    local rgbseq = ColorSequence.new
    local rgbkey = ColorSequenceKeypoint.new
    local numseq = NumberSequence.new
    local numkey = NumberSequenceKeypoint.new

    local camera = ws.CurrentCamera
    local lp = players.LocalPlayer 
    local mouse = lp:GetMouse() 
    local gui_offset = gui_service:GetGuiInset().Y

    local max = math.max 
    local floor = math.floor 
    local min = math.min 
    local abs = math.abs 
    local noise = math.noise
    local rad = math.rad 
    local random = math.random 
    local pow = math.pow 

    local main_event = rs:FindFirstChild("MainEvent")
-- 

local milkyboy = {
    target_selection = {
        player = nil, 
        time = os.clock(),
        pass = false, 
    },
    misc = {
        auto_shoot_clock = os.clock(), 
        buying_armor = false 
    }, 
    config = {
        saving = {current = {}}, 
        luas = {}, 
    },

    drawings = {}, 
    instances = {}, 

    connections = {},
    typing_check = false, 

    delta = nil, 
    radians = 0, 

    directory = "MilkyBoy Skibidi",
}

if not LPH_OBFUSCATED then
    getfenv().LPH_NO_VIRTUALIZE = function(...) return (...) end
end

-- library init
    local library = {
        directory = "MilkyBoy Skibidi",
        folders = {
            "/fonts",
            "/configs"
        },
        flags = {},
        config_flags = {},
        guis = {}, 
        connections = {},   

        current_tab, 
        current_element_open, 
        dock_button_holder,  
        font, 
        keybind_list
    }

    local flags = library.flags
    local config_flags = library.config_flags

    local themes = {
        preset = {
            ["outline"] = rgb(32, 32, 38), -- 
            ["inline"] = rgb(60, 55, 75), --
            ["accent"] = hex("#7D98AF"), --
            ["high_contrast"] = rgb(41, 41, 55),
            ["low_contrast"] = rgb(35, 35, 47),
            ["text"] = rgb(180, 180, 180),
            ["text_outline"] = rgb(0, 0, 0),
            ["glow"] = hex("#7D98AF"), 
        }, 	

        utility = {
            ["outline"] = {
                ["BackgroundColor3"] = {}, 	
                ["Color"] = {}, 
            },
            ["inline"] = {
                ["BackgroundColor3"] = {}, 	
            },
            ["accent"] = {
                ["BackgroundColor3"] = {}, 	
                ["TextColor3"] = {}, 
                ["ImageColor3"] = {}, 
                ["ScrollBarImageColor3"] = {} 
            },
            ["contrast"] = {
                ["Color"] = {}, 	
            },
            ["text"] = {
                ["TextColor3"] = {}, 	
            },
            ["text_outline"] = {
                ["Color"] = {}, 	
            },
            ["glow"] = {
                ["ImageColor3"] = {}, 	
            }, 
        }, 

        find = {
            ["Frame"] = "BackgroundColor3", 
            ["TextLabel"] = "TextColor3", 
            ["UIGradient"] = "Color",
            ["UIStroke"] = "Color",
            ["ImageLabel"] = "ImageColor3",
            ["TextButton"] = "BackgroundColor3", 
            ["ScrollingFrame"] = "ScrollBarImageColor3"
        }
    }

    local keys = {
        [Enum.KeyCode.LeftShift] = "LS",
        [Enum.KeyCode.RightShift] = "RS",
        [Enum.KeyCode.LeftControl] = "LC",
        [Enum.KeyCode.RightControl] = "RC",
        [Enum.KeyCode.Insert] = "INS",
        [Enum.KeyCode.Backspace] = "BS",
        [Enum.KeyCode.Return] = "Ent",
        [Enum.KeyCode.LeftAlt] = "LA",
        [Enum.KeyCode.RightAlt] = "RA",
        [Enum.KeyCode.CapsLock] = "CAPS",
        [Enum.KeyCode.One] = "1",
        [Enum.KeyCode.Two] = "2",
        [Enum.KeyCode.Three] = "3",
        [Enum.KeyCode.Four] = "4",
        [Enum.KeyCode.Five] = "5",
        [Enum.KeyCode.Six] = "6",
        [Enum.KeyCode.Seven] = "7",
        [Enum.KeyCode.Eight] = "8",
        [Enum.KeyCode.Nine] = "9",
        [Enum.KeyCode.Zero] = "0",
        [Enum.KeyCode.KeypadOne] = "Num1",
        [Enum.KeyCode.KeypadTwo] = "Num2",
        [Enum.KeyCode.KeypadThree] = "Num3",
        [Enum.KeyCode.KeypadFour] = "Num4",
        [Enum.KeyCode.KeypadFive] = "Num5",
        [Enum.KeyCode.KeypadSix] = "Num6",
        [Enum.KeyCode.KeypadSeven] = "Num7",
        [Enum.KeyCode.KeypadEight] = "Num8",
        [Enum.KeyCode.KeypadNine] = "Num9",
        [Enum.KeyCode.KeypadZero] = "Num0",
        [Enum.KeyCode.Minus] = "-",
        [Enum.KeyCode.Equals] = "=",
        [Enum.KeyCode.Tilde] = "~",
        [Enum.KeyCode.LeftBracket] = "[",
        [Enum.KeyCode.RightBracket] = "]",
        [Enum.KeyCode.RightParenthesis] = ")",
        [Enum.KeyCode.LeftParenthesis] = "(",
        [Enum.KeyCode.Semicolon] = ",",
        [Enum.KeyCode.Quote] = "'",
        [Enum.KeyCode.BackSlash] = "\\",
        [Enum.KeyCode.Comma] = ",",
        [Enum.KeyCode.Period] = ".",
        [Enum.KeyCode.Slash] = "/",
        [Enum.KeyCode.Asterisk] = "*",
        [Enum.KeyCode.Plus] = "+",
        [Enum.KeyCode.Period] = ".",
        [Enum.KeyCode.Backquote] = "`",
        [Enum.UserInputType.MouseButton1] = "MB1",
        [Enum.UserInputType.MouseButton2] = "MB2",
        [Enum.UserInputType.MouseButton3] = "MB3",
        [Enum.KeyCode.Escape] = "ESC",
        [Enum.KeyCode.Space] = "SPC",
    }
        
    library.__index = library

    for _, path in next, library.folders do 
        makefolder(library.directory .. path)
    end 

    if setreadonly then 
        if not isfile(library.directory .. "/fonts/main.ttf") then 
            writefile(library.directory .. "/fonts/main.ttf", game:HttpGet("https://github.com/f1nobe7650/other/raw/main/uis/font.ttf"))
        end 
        
        local tahoma = {
            name = "SmallestPixel7",
            faces = {
                {
                    name = "Regular",
                    weight = 400,
                    style = "normal",
                    assetId = getcustomasset(library.directory .. "/fonts/main.ttf")
                }
            }
        }
        
        if not isfile(library.directory .. "/fonts/main_encoded.ttf") then 
            writefile(library.directory .. "/fonts/main_encoded.ttf", http_service:JSONEncode(tahoma))
        end 
        
        library.font = Font.new(getcustomasset(library.directory .. "/fonts/main_encoded.ttf"), Enum.FontWeight.Regular)
    else 
        library.font = Font.new("rbxasset://fonts/families/Zekton.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal)
    end 

    local config_holder 
-- 

-- library functions 
    -- misc functions
        function library:make_resizable(frame) 
            local Frame = Instance.new("TextButton")
            Frame.Position = dim2(1, -10, 1, -10)
            Frame.BorderColor3 = rgb(0, 0, 0)
            Frame.Size = dim2(0, 10, 0, 10)
            Frame.BorderSizePixel = 0
            Frame.BackgroundColor3 = rgb(255, 255, 255)
            Frame.Parent = frame
            Frame.BackgroundTransparency = 1 
            Frame.Text = ""

            local resizing = false 
            local start_size 
            local start 
            local og_size = frame.Size  

            Frame.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    resizing = true
                    start = input.Position
                    start_size = frame.Size
                end
            end)

            Frame.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    resizing = false
                end
            end)

            library:connection(uis.InputChanged, function(input, game_event) 
                if resizing and input.UserInputType == Enum.UserInputType.MouseMovement then
                    local viewport_x = camera.ViewportSize.X
                    local viewport_y = camera.ViewportSize.Y

                    local current_size = dim2(
                        start_size.X.Scale,
                        math.clamp(
                            start_size.X.Offset + (input.Position.X - start.X),
                            og_size.X.Offset,
                            viewport_x
                        ),
                        start_size.Y.Scale,
                        math.clamp(
                            start_size.Y.Offset + (input.Position.Y - start.Y),
                            og_size.Y.Offset,
                            viewport_y
                        )
                    )
                    frame.Size = current_size
                end
            end)
        end

        function library:new_drawing(class, properties)
            local ins = Drawing.new(class)

            for _, v in next, properties do 
                ins[_] = v
            end 

            table.insert(milkyboy.drawings, ins)

            return ins 
        end 

        function library:new_item(class, properties) 
            local ins = Instance.new(class)

            for _, v in next, properties do 
                ins[_] = v
            end 

            table.insert(milkyboy.instances, ins)

            return ins 
        end 

        function library:convert_enum(enum)
            local enum_parts = {}
        
            for part in string.gmatch(enum, "[%w_]+") do
                table.insert(enum_parts, part)
            end
        
            local enum_table = Enum
            for i = 2, #enum_parts do
                local enum_item = enum_table[enum_parts[i]]
        
                enum_table = enum_item
            end
        
            return enum_table
        end

        function library:config_list_update() 
            if not config_holder then return end 
        
            local list = {}
        
            for idx, file in next, listfiles(library.directory .. "/configs") do
                local name = file:gsub(library.directory .. "/configs\\", ""):gsub(".cfg", "")
                list[#list + 1] = name
            end
            
            config_holder.refresh_options(list)
        end 

        function library:get_config()
            local Config = {}
        
            for _, v in flags do
                if type(v) == "table" and v.key then
                    Config[_] = {active = v.active, mode = v.mode, key = tostring(v.key)}
                elseif type(v) == "table" and v["Transparency"] and v["Color"] then
                    Config[_] = {Transparency = v["Transparency"], Color = v["Color"]:ToHex()}
                else
                    Config[_] = v
                end
            end 
            
            return http_service:JSONEncode(Config)
        end

        function library:load_config(config_json) 
            local config = http_service:JSONDecode(config_json)
        
            for _, v in next, config do 
                local function_set = library.config_flags[_]
        
                if function_set then 
                    if type(v) == "table" and v["Transparency"] and v["Color"] then
                        function_set(hex(v["Color"]), v["Transparency"])
                    elseif type(v) == "table" and v["active"] then 
                        function_set(v)
                    else 
                        function_set(v)
                    end
                end 
            end 
        end 
        
        function library:round(number, float) 
            local multiplier = 1 / (float or 1)
            return math.floor(number * multiplier + 0.5) / multiplier
        end 

        function library:apply_theme(instance, theme, property) 
            table.insert(themes.utility[theme][property], instance)
        end

        function library:update_theme(theme, color)
            for _, property in next, themes.utility[theme] do 

                for m, object in next, property do 
                    if object[_] == themes.preset[theme] or object.ClassName == "UIGradient" then 
                        object[_] = color 
                    end 
                end 
            end 

            themes.preset[theme] = color 
        end 

        function library:connection(signal, callback)
            local connection = signal:Connect(callback)
            
            table.insert(library.connections, connection)

            return connection 
        end

        function library:apply_stroke(parent) 
            local stroke = library:create("UIStroke", {
                Parent = parent,
                Color = themes.preset.text_outline, 
                LineJoinMode = Enum.LineJoinMode.Miter
            }) 
            
            library:apply_theme(stroke, "text_outline", "Color")
        end

        function library:create(instance, options)
            local ins = Instance.new(instance) 
            
            for prop, value in next, options do 
                ins[prop] = value
            end
            
            if instance == "TextLabel" or instance == "TextButton" or instance == "TextBox" then 	
                library:apply_theme(ins, "text", "TextColor3")
                library:apply_stroke(ins)
            elseif instance == "ScreenGui" then 
                table.insert(library.guis, ins)
            end 
            
            return ins 
        end
    -- 

    -- elements 
        function library:panel(options) 
            local cfg = {
                name = options.text or options.name or "Window", 
                size = options.size or dim2(0, 530, 0, 590),
                position = options.position or dim2(0, 500, 0, 500),
                anchor_point = options.anchor_point or vec2(0, 0)
            }

            local sgui = library:create("ScreenGui", {
                Enabled = true,
                Parent = coregui,
                Name = "" 
            })

            local main_holder = library:create("Frame", {
                Parent = sgui,
                Name = "",
                AnchorPoint = vec2(cfg.anchor_point.X, cfg.anchor_point.Y),
                Position = cfg.position,
                Active = true, 
                Draggable = true,
                BorderColor3 = rgb(0, 0, 0),
                Size = cfg.size,
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38)
            })
            
            library:apply_theme(main_holder, "outline", "BackgroundColor3") 
            
            local window_inline = library:create("Frame", {
                Parent = main_holder,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })
            
            library:apply_theme(window_inline, "accent", "BackgroundColor3") 
            
            local window_holder = library:create("Frame", {
                Parent = window_inline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(32, 32, 38),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            library:apply_theme(window_holder, "outline", "BackgroundColor3") 
            
            local UIGradient = library:create("UIGradient", {
                Parent = window_holder,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(41, 41, 55)),
                rgbkey(1, rgb(35, 35, 47))
            }
            })
            
            library:apply_theme(UIGradient, "contrast", "Color") 
            
            local text = library:create("TextLabel", {
                Parent = window_holder,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = cfg.name,
                BackgroundTransparency = 1,
                Position = dim2(0, 2, 0, 4),
                BorderSizePixel = 0,
                AutomaticSize = Enum.AutomaticSize.XY,
                TextSize = 12,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIStroke = library:create("UIStroke", {
                Parent = text,
                Name = "",
                LineJoinMode = Enum.LineJoinMode.Miter
            })
            
            local UIPadding = library:create("UIPadding", {
                Parent = window_holder,
                Name = "",
                PaddingBottom = dim(0, 4),
                PaddingRight = dim(0, 4),
                PaddingLeft = dim(0, 4)
            })
            
            local outline = library:create("Frame", {
                Parent = window_holder,
                Name = "",
                Position = dim2(0, 0, 0, 18),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 1, -18),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })
            
            library:apply_theme(outline, "inline", "BackgroundColor3") 
            
            local inline = library:create("Frame", {
                Parent = outline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38)
            })
            
            library:apply_theme(inline, "outline", "BackgroundColor3") 
            
            local holder = library:create("Frame", {
                Parent = inline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = holder,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
            })
            
            library:apply_theme(UIGradient, "contrast", "Color") 
            
            local UIPadding = library:create("UIPadding", {
                Parent = holder,
                Name = "",
                PaddingTop = dim(0, 5),
                PaddingBottom = dim(0, 5),
                PaddingRight = dim(0, 5),
                PaddingLeft = dim(0, 5)
            })
            
            local glow = library:create("ImageLabel", {
                Parent = main_holder,
                Name = "",
                ImageColor3 = themes.preset.accent,
                ScaleType = Enum.ScaleType.Slice,
                BorderColor3 = rgb(0, 0, 0),
                BackgroundColor3 = rgb(255, 255, 255),
                Visible = true,
                Image = "http://www.roblox.com/asset/?id=18245826428",
                BackgroundTransparency = 1,
                ImageTransparency = 0.8, 
                Position = dim2(0, -20, 0, -20),
                Size = dim2(1, 40, 1, 40),
                ZIndex = 2,
                BorderSizePixel = 0,
                SliceCenter = rect(vec2(21, 21), vec2(79, 79))
            })library:apply_theme(glow, "glow", "ImageColor3") 
            
            return holder, sgui      
        end 

        local sgui = library:create("ScreenGui", {
            Enabled = true,
            Parent = game.CoreGui,
            Name = "",
            DisplayOrder = 2, 
        })

        function library:window(properties)
            local window = {}
            -- local button_holder  
            
            --[[local function create_dock_button(options) 
                local cfg = {
                    image = options.image or "rbxassetid://79856374238119",
                    open = options.open or true 
                    callback = options.callback or function() end
                }
        
                local button = library:create("TextButton", {
                    Parent = button_holder,
                    Name = "",
                    FontFace = Font.new("rbxasset://fonts/families/SourceSansPro.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal),
                    TextColor3 = rgb(0, 0, 0),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "",
                    Size = dim2(0, 25, 0, 25),
                    BorderSizePixel = 0,
                    TextSize = 14,
                    BackgroundColor3 = rgb(60, 55, 75)
                })
                
                local button_inline = library:create("Frame", {
                    Parent = button,
                    Name = "",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(button_inline, "outline", "BackgroundColor3") 
                
                local button_inline = library:create("Frame", {
                    Parent = button_inline,
                    Name = "",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })library:apply_theme(button_inline, "inline", "BackgroundColor3")
                
                local UIGradient = library:create("UIGradient", {
                    Parent = button_inline,
                    Name = "",
                    Rotation = 90,
                    Color = rgbseq{
                    rgbkey(0, rgb(35, 35, 47)),
                    rgbkey(1, rgb(41, 41, 55))
                }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                local ImageLabel = library:create("ImageLabel", {
                    Parent = button_inline,
                    Name = "",
                    ImageColor3 = themes.preset.accent,
                    Image = "rbxassetid://79856374238119",
                    BackgroundTransparency = 1,
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, 0),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })library:apply_theme(ImageLabel, "accent", "ImageColor3") 
                
                local UIPadding = library:create("UIPadding", {
                    Parent = button_inline,
                    Name = "",
                    PaddingTop = dim(0, 4),
                    PaddingBottom = dim(0, 4),
                    PaddingRight = dim(0, 4),
                    PaddingLeft = dim(0, 4)
                })
        
                button.MouseButton1Click:Connect(function()
                    cfg.open = not cfg.open 
        
                    cfg.callback(cfg.open) 
                end)
            end ]]

            -- main window
                local holder, path = library:panel({
                    name = properties and properties.name or "milkyboy.tech", 
                    size = dim2(0, 500, 0, 500),
                    position = dim2(0, (camera.ViewportSize.X / 2) - 250, 0, (camera.ViewportSize.Y / 2) - 250),
                }) 

                library.frame = path

                -- create_dock_button({
                --     image = "rbxassetid://100959383267514",
                --     callback = function(open)
                --         path.Enabled = open 
                --     end 
                -- })

                window["tab_holder"] = library:create("Frame", {
                    Parent = holder,
                    Name = " ",
                    BackgroundTransparency = 1,
                    Size = dim2(1, 0, 0, 22),
                    BorderColor3 = rgb(0, 0, 0),
                    ZIndex = 5,
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })

                library:create("UIListLayout", {
                    Parent = window["tab_holder"],
                    FillDirection = Enum.FillDirection.Horizontal,
                    HorizontalFlex = Enum.UIFlexAlignment.Fill,
                    Padding = dim(0, 2),
                    SortOrder = Enum.SortOrder.LayoutOrder
                })

                local section_holder = library:create("Frame", {
                    Parent = holder,
                    Name = " ",
                    BackgroundTransparency = 1,
                    Position = dim2(0, -1, 0, 19),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, -22),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                window["section_holder"] = section_holder

                local outline = library:create("Frame", {
                    Parent = section_holder,
                    Name = "\0",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, 2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })
                
                library:apply_theme(outline, "outline", "BackgroundColor3") 

                local inline = library:create("Frame", {
                    Parent = outline,
                    Name = "\0",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })
                
                library:apply_theme(inline, "inline", "BackgroundColor3") 

                local background = library:create("Frame", {
                    Parent = inline,
                    Name = "\0",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })

                library.section_holder = background

                library:create("UIPadding", {
                    Parent = background,
                    PaddingTop = dim(0, 4),
                    PaddingBottom = dim(0, 4),
                    PaddingRight = dim(0, 4),
                    PaddingLeft = dim(0, 4)
                })

                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Rotation = 90,
                    Color = rgbseq{
                        rgbkey(0, rgb(41, 41, 55)),
                        rgbkey(1, rgb(35, 35, 47))
                    }
                })
                
                library:apply_theme(UIGradient, "contrast", "Color") 

                library:make_resizable(path:FindFirstChildOfClass("Frame")) 
            -- 

            -- keybind list       
                local outline = library:create("Frame", {
                    Parent = sgui,
                    Name = "",
                    Visible = false, 
                    Position = UDim2.new(0.04290718212723732, 0, 0.4762658178806305, 0),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Size = UDim2.new(0, 182, 0, 25),
                    BorderSizePixel = 0,
                    BackgroundColor3 = themes.preset.outline
                })
                library:apply_theme(outline, "outline", "BackgroundColor3") 
                library.keybind_list_frame = outline 

                local inline = library:create("Frame", {
                    Parent = outline,
                    Name = "",
                    Position = UDim2.new(0, 1, 0, 1),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Size = UDim2.new(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = themes.preset.inline
                })
                library:apply_theme(inline, "inline", "BackgroundColor3")

                local background = library:create("Frame", {
                    Parent = inline,
                    Name = "",
                    Position = UDim2.new(0, 1, 0, 1),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Size = UDim2.new(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Name = "",
                    Rotation = 90,
                    Color = ColorSequence.new{
                        ColorSequenceKeypoint.new(0, themes.preset.high_contrast),
                        ColorSequenceKeypoint.new(1, themes.preset.low_contrast)
                    }
                })
                library:apply_theme(UIGradient, "contrast", "Color") 
                
                local bg = library:create("Frame", {
                    Parent = background,
                    Name = "a",
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Size = UDim2.new(1, 0, 0, 2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = themes.preset.accent
                }); library:apply_theme(bg, "accent", "BackgroundColor3")
                
                
                library:create("UIGradient", {
                    Parent = bg,
                    Name = "",
                    Enabled = true, 
                    Rotation = 90,
                    Color = ColorSequence.new{
                        ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 255, 255)),
                        ColorSequenceKeypoint.new(1, Color3.fromRGB(167, 167, 167))
                    }
                })
                
                local text = library:create("TextLabel", {
                    Parent = background,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = themes.preset.text,
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Text = "Keybinds",
                    BackgroundTransparency = 1,
                    TextTruncate = Enum.TextTruncate.AtEnd,
                    Size = UDim2.new(1, 0, 1, 0),
                    BorderSizePixel = 0,
                    TextSize = 12,
                    BackgroundColor3 = themes.preset.text
                }, "text")
                
                local UIStroke = library:create("UIStroke", {
                    Parent = text,
                    Name = "",
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
                
                local text_holder = library:create("Frame", {
                    Parent = background,
                    Name = "",
                    Size = UDim2.new(0, 182, 0, 22),
                    Position = UDim2.new(0, -2, 1, 1),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    BackgroundColor3 = themes.preset.outline
                })
                library:apply_theme(text_holder, "outline", "BackgroundColor3")

                local inline = library:create("Frame", {
                    Parent = text_holder,
                    Name = "",
                    Size = UDim2.new(1, -2, 1, -2),
                    Position = UDim2.new(0, 1, 0, 1),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    BorderSizePixel = 0,
                    --AutomaticSize = Enum.AutomaticSize.Y,
                    BackgroundColor3 = themes.preset.inline
                })
                library:apply_theme(inline, "inline", "BackgroundColor3")
                
                local background = library:create("Frame", {
                    Parent = inline,
                    Name = "",
                    Size = UDim2.new(1, -2, 1, -2),
                    Position = UDim2.new(0, 1, 0, 1),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    BorderSizePixel = 0,
                    --AutomaticSize = Enum.AutomaticSize.Y,
                    BackgroundColor3 = Color3.fromRGB(255, 255, 255)
                })
                library.keybind_list = background
                
                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Name = "",
                    Rotation = 90,
                    Color = ColorSequence.new{
                        ColorSequenceKeypoint.new(0, themes.preset.high_contrast),
                        ColorSequenceKeypoint.new(1, themes.preset.low_contrast)
                    }
                })
                library:apply_theme(UIGradient, "contrast", "Color") 
                
                library:create("UIListLayout", {
                    Parent = background,
                    Name = "",
                    Padding = UDim.new(0, -1),
                    SortOrder = Enum.SortOrder.LayoutOrder
                })
                
                library:create("UIPadding", {
                    Parent = background,
                    Name = "",
                    PaddingBottom = UDim.new(0, 4),
                    PaddingLeft = UDim.new(0, 5)
                })
                
            
            -- 

            -- -- dock innit
            --     local dock_outline = library:create("Frame", {
            --         Parent = sgui,
            --         Name = "",
            --         Visible = true,
            --         BorderColor3 = rgb(0, 0, 0),
            --         AnchorPoint = vec2(0.5, 0),
            --         Position = dim2(0.5, 0, 0, 20),
            --         Size = dim2(0, 127, 0, 39),
            --         BorderSizePixel = 0,
            --         BackgroundColor3 = rgb(32, 32, 38)
            --     })library:apply_theme(dock_outline, "outline", "BackgroundColor3") 
                
            --     local dock_inline = library:create("Frame", {
            --         Parent = dock_outline,
            --         Name = "",
            --         Position = dim2(0, 1, 0, 1),
            --         BorderColor3 = rgb(0, 0, 0),
            --         Size = dim2(1, -2, 1, -2),
            --         BorderSizePixel = 0,
            --         BackgroundColor3 = rgb(60, 55, 75)
            --     })library:apply_theme(dock_inline, "inline", "BackgroundColor3") 
                
            --     local dock_holder = library:create("Frame", {
            --         Parent = dock_inline,
            --         Name = "",
            --         Size = dim2(1, -2, 1, -2),
            --         Position = dim2(0, 1, 0, 1),
            --         BorderColor3 = rgb(32, 32, 38),
            --         BorderSizePixel = 0,
            --         BackgroundColor3 = rgb(255, 255, 255)
            --     })library:apply_theme(dock_holder, "outline", "BackgroundColor3") 
                
            --     local accent = library:create("Frame", {
            --         Parent = dock_holder,
            --         Name = "",
            --         Size = dim2(1, 0, 0, 2),
            --         BorderColor3 = rgb(0, 0, 0),
            --         BorderSizePixel = 0,
            --         BackgroundColor3 = themes.preset.accent
            --     })library:apply_theme(accent, "accent", "BackgroundColor3") 
                
            --     local UIGradient = library:create("UIGradient", {
            --         Parent = accent,
            --         Name = "",
            --         Rotation = 90,
            --         Color = rgbseq{
            --         rgbkey(0, rgb(255, 255, 255)),
            --         rgbkey(1, rgb(167, 167, 167))
            --     }
            --     })
                
            --     button_holder = library:create("Frame", {
            --         Parent = dock_holder,
            --         Name = "",
            --         BackgroundTransparency = 1,
            --         Size = dim2(1, 0, 1, 0),
            --         BorderColor3 = rgb(0, 0, 0),
            --         BorderSizePixel = 0,
            --         BackgroundColor3 = rgb(255, 255, 255)
            --     })
                
            --     local UIListLayout = library:create("UIListLayout", {
            --         Parent = button_holder,
            --         Name = "",
            --         Padding = dim(0, 5),
            --         FillDirection = Enum.FillDirection.Horizontal,
            --         SortOrder = Enum.SortOrder.LayoutOrder
            --     })
                
            --     local UIPadding = library:create("UIPadding", {
            --         Parent = button_holder,
            --         Name = "",
            --         PaddingTop = dim(0, 6),
            --         PaddingBottom = dim(0, 4),
            --         PaddingRight = dim(0, 4),
            --         PaddingLeft = dim(0, 4)
            --     })
                        
            --     local UIGradient = library:create("UIGradient", {
            --         Parent = dock_holder,
            --         Name = "",
            --         Rotation = 90,
            --         Color = rgbseq{
            --         rgbkey(0, rgb(41, 41, 55)),
            --         rgbkey(1, rgb(35, 35, 47))
            --     }
            --     })library:apply_theme(UIGradient, "contrast", "Color") 
            -- -- 

            -- -- cfg holder
                
            --     window["configs"], config_path = library:panel({
            --         name = "Configurations", 
            --         anchor_point = vec2(-1, 0),
            --         size = dim2(0, 322, 0, 391),
            --         position = dim2(0, holder.AbsolutePosition.X + holder.AbsoluteSize.X + 10, 0, holder.AbsolutePosition.Y - 22)
            --     }) 

            --     create_dock_button({
            --         image = "rbxassetid://79856374238119",
            --         callback = function(open)
            --             config_path.Enabled = open 
            --         end 
            --     })

            --     local UIGradient = library:create("UIGradient", {
            --         Parent = holder,
            --         Name = "",
            --         Rotation = 90,
            --         Color = rgbseq{
            --             rgbkey(0, rgb(41, 41, 55)),
            --             rgbkey(1, rgb(35, 35, 47))
            --         }
            --     })library:apply_theme(UIGradient, "contrast", "Color") 
                
            --     local UIPadding = library:create("UIPadding", {
            --         Parent = holder,
            --         Name = "",
            --         PaddingTop = dim(0, 5),
            --         PaddingBottom = dim(0, 5),
            --         PaddingRight = dim(0, 5),
            --         PaddingLeft = dim(0, 5)
            --     })
                
            --     local glow = library:create("ImageLabel", {
            --         Parent = config_outline,
            --         Name = "",
            --         ImageColor3 = themes.preset.accent,
            --         ScaleType = Enum.ScaleType.Slice,
            --         BorderColor3 = rgb(0, 0, 0),
            --         BackgroundColor3 = rgb(255, 255, 255),
            --         Visible = true,
            --         Image = "http://www.roblox.com/asset/?id=18245826428",
            --         BackgroundTransparency = 1,
            --         ImageTransparency = 0.8, 
            --         Position = dim2(0, -20, 0, -20),
            --         Size = dim2(1, 40, 1, 40),
            --         ZIndex = 2,
            --         BorderSizePixel = 0,
            --         SliceCenter = rect(vec2(21, 21), vec2(79, 79))
            --     })library:apply_theme(glow, "glow", "ImageColor3") 
                
            -- -- 

            -- -- playerlist 
            --     window["playerlist"], playerlist_gui = library:panel({
            --         name = "Playerlist", 
            --         anchor_point = vec2(-1, 0),
            --         size = dim2(0, 385, 0, 399),
            --         position = dim2(0, holder.AbsolutePosition.X - 395, 0, holder.AbsolutePosition.Y - 22)
            --     })    
                
            --     create_dock_button({
            --         image = "rbxassetid://115194686863276",
            --         callback = function(open)
            --             playerlist_gui.Enabled = open 
            --         end 
            --     })
            -- -- 
            
            -- -- theming 
            --     window["themes"], themes_gui = library:panel({
            --         name = "Style", 
            --         anchor_point = vec2(-1, 0),
            --         size = dim2(0, 322, 0, 391),
            --         position = dim2(0, window["configs"].AbsolutePosition.X + window["configs"].AbsoluteSize.X + 10, 0, window["configs"].AbsolutePosition.Y - 22)
            --     })   

            --     create_dock_button({
            --         image = "rbxassetid://115194686863276",
            --         callback = function(open)
            --             themes_gui.Enabled = open 
            --         end 
            --     })
            -- -- 
             

            return setmetatable(window, library)
        end

        function library:watermark(options) 
            local cfg = {
                default = options.text or options.default or os.date('drain.lol | %b %d %Y | %H:%M') -- highly skidded by finobe
            }

            local watermark_outline = library:create("Frame", {
                Parent = sgui,
                Name = "",
                BorderColor3 = rgb(0, 0, 0),
                AnchorPoint = vec2(0.001, 0.001),
                Position = dim2(0, 120, 0, -25),
                Size = dim2(0, 0, 0, 24),
                BorderSizePixel = 0,
                AutomaticSize = Enum.AutomaticSize.X,
                BackgroundColor3 = rgb(32, 32, 38)
            })library:apply_theme(watermark_outline, "outline", "BackgroundColor3") 
            
            local watermark_inline = library:create("Frame", {
                Parent = watermark_outline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })library:apply_theme(watermark_inline, "inline", "BackgroundColor3") 
            
            local watermark_background = library:create("Frame", {
                Parent = watermark_inline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = watermark_background,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
            })library:apply_theme(UIGradient, "contrast", "Color") 
            
            local text = library:create("TextLabel", {
                Parent = watermark_background,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "  drain.lol | Beta | Aug 29 2024 | 07:29:00  ",
                Size = dim2(0, 0, 1, 0),
                BackgroundTransparency = 1,
                Position = dim2(0, -1, 0, 1),
                BorderSizePixel = 0,
                AutomaticSize = Enum.AutomaticSize.X,
                TextSize = 12,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            library:create("UIStroke", {
                Parent = text,
                Name = "",
                LineJoinMode = Enum.LineJoinMode.Miter
            })
            
            local accent = library:create("Frame", {
                Parent = watermark_outline,
                Name = "",
                Position = dim2(0, 2, 0, 2),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -4, 0, 2),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })library:apply_theme(accent, "accent", "BackgroundColor3") 
            
            local UIGradient = library:create("UIGradient", {
                Parent = accent,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                    rgbkey(0, rgb(255, 255, 255)),
                    rgbkey(1, rgb(167, 167, 167))
                }
            })
            
            function cfg.change_text(input)
                text.Text = "  ".. input .."  "
            end 

            cfg.change_text(cfg.default)

            return cfg 
        end 

        function library:tab(options)	
            local cfg = {
                name = options.name or "tab", 
                enabled = false, 
            }
            
            -- button instances
                local tab_holder = library:create("TextButton", {
                    Parent = self.tab_holder,
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "",
                    Name = "\0",
                    BorderSizePixel = 0,
                    Size = dim2(0, 0, 1, -2),
                    ZIndex = 5,
                    TextSize = 12,
                    BackgroundColor3 = rgb(32, 32, 38),
                    AutoButtonColor = false
                })library:apply_theme(tab_holder, "outline", "BackgroundColor3") 

                local inline = library:create("Frame", {
                    Parent = tab_holder,
                    Size = dim2(1, -2, 1, 0),
                    Name = "\0",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    ZIndex = 5,
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(inline, "inline", "BackgroundColor3") 

                local background = library:create("Frame", {
                    Parent = inline,
                    Size = dim2(1, -2, 1, -1),
                    Name = "\0",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    ZIndex = 5,
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })

                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Rotation = 90,
                    Color = rgbseq{rgbkey(0, rgb(41, 41, 55)), rgbkey(1, rgb(35, 35, 47))}
                })library:apply_theme(UIGradient, "contrast", "Color") 

                local text = library:create("TextLabel", {
                    Parent = background,
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = cfg.name,
                    Name = "\0",
                    BackgroundTransparency = 1,
                    Size = dim2(1, 0, 1, 0),
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.X,
                    TextSize = 12,
                    ZIndex = 5,
                    BackgroundColor3 = rgb(255, 255, 255)
                }, "text")
                library:apply_theme(text, "accent", "TextColor3")
            
            -- section instances 
                local section_holder = library:create("Frame", {
                    Parent = library.section_holder,
                    BackgroundTransparency = 1,
                    Name = "\0",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, 0),
                    BorderSizePixel = 0,
                    Visible = false,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
            
                cfg["holder"] = section_holder

                library:create("UIListLayout", {
                    Parent = section_holder,
                    FillDirection = Enum.FillDirection.Horizontal,
                    HorizontalFlex = Enum.UIFlexAlignment.Fill,
                    Padding = dim(0, 4),
                    SortOrder = Enum.SortOrder.LayoutOrder
                })

            function cfg.open_tab()
                if library.current_tab and library.current_tab[1] ~= background then 
                    local button = library.current_tab[1]
                    button.Size = dim2(1, -2, 1, -1)
                    button:FindFirstChildOfClass("UIGradient").Rotation = 90
                    button:FindFirstChildOfClass("TextLabel").TextColor3 = themes.preset.text
                        
                    library.current_tab[2].Visible = false
                    
                    library.current_tab = nil
                end
                
                library.current_tab = {
                    background, section_holder
                }
                
                local button = library.current_tab[1] 
                button.Size = dim2(1, -2, 1, 0)
                button:FindFirstChildOfClass("UIGradient").Rotation = -90
                button:FindFirstChildOfClass("TextLabel").TextColor3 = themes.preset.accent 

                library.current_tab[2].Visible = true 

                if library.current_element_open and library.current_element_open ~= cfg then 
                    library.current_element_open.set_visible(false)
                    library.current_element_open.open = false 
                    library.current_element_open = nil 
                end
            end
            
            tab_holder.MouseButton1Click:Connect(cfg.open_tab)
            
            return setmetatable(cfg, library) 
        end

        function library:column(path) 
            local cfg = {}
            
            local holder = path and self[path] or self.holder
            
            local column = library:create("Frame", {
                Parent = holder,
                BackgroundTransparency = 1,
                Name = "\0",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })library:apply_theme(column, "inline", "BackgroundColor3") 
            
            library:create("UIListLayout", {
                Parent = column,
                Padding = dim(0, 4),
                SortOrder = Enum.SortOrder.LayoutOrder,
                VerticalFlex = Enum.UIFlexAlignment.Fill
            })
            
            cfg["holder"] = column

            return setmetatable(cfg, library) 
        end

        function library:section(options)
            local cfg = {
                name = options.name or "Section", 
            }
            
            local section = library:create("Frame", {
                Parent = self.holder,
                Name = "\0",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })library:apply_theme(section, "inline", "BackgroundColor3") 

            local inline = library:create("Frame", {
                Parent = section,
                Name = "\0",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38)
            })library:apply_theme(inline, "outline", "BackgroundColor3") 

            local background = library:create("Frame", {
                Parent = inline,
                Name = "\0",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })

            local text = library:create("TextLabel", {
                Parent = background,
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = cfg.name,
                Name = "\0",
                BackgroundTransparency = 1,
                Position = dim2(0, 6, 0, 4),
                BorderSizePixel = 0,
                AutomaticSize = Enum.AutomaticSize.XY,
                TextSize = 12,
                BackgroundColor3 = rgb(255, 255, 255)
            })

            library:create("UIStroke", {
                Parent = text,
                LineJoinMode = Enum.LineJoinMode.Miter
            })

            local accent = library:create("Frame", {
                Parent = background,
                Name = "\0",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 0, 2),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })library:apply_theme(accent, "accent", "BackgroundColor3") 

            local UIGradient = library:create("UIGradient", {
                Parent = accent,
                Rotation = 90,
                Color = rgbseq{
                    rgbkey(0, rgb(255, 255, 255)),
                    rgbkey(1, rgb(167, 167, 167))
                }
            })

            local UIGradient = library:create("UIGradient", {
                Parent = background,
                Rotation = 90,
                Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
            })library:apply_theme(UIGradient, "contrast", "Color") 

            local ScrollingFrame = library:create("ScrollingFrame", {
                Parent = background,
                ScrollBarImageColor3 = themes.preset.accent,
                Active = true,
                AutomaticCanvasSize = Enum.AutomaticSize.Y,
                ScrollBarThickness = 2,
                Size = dim2(1, 0, 1, -20),
                BackgroundTransparency = 1,
                Position = dim2(0, 0, 0, 20),
                BackgroundColor3 = rgb(255, 255, 255),
                BorderColor3 = rgb(0, 0, 0),
                BorderSizePixel = 0,
                CanvasSize = dim2(0, 0, 0, 0)
            })library:apply_theme(ScrollingFrame, "accent", "ScrollBarImageColor3") 

            local elements = library:create("Frame", {
                Parent = ScrollingFrame,
                Name = "\0",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 0, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            cfg.holder = elements 

            library:create("UIListLayout", {
                Parent = elements,
                Padding = dim(0, 4),
                HorizontalAlignment = Enum.HorizontalAlignment.Center,
                SortOrder = Enum.SortOrder.LayoutOrder
            })

            library:create("UIPadding", {
                Parent = ScrollingFrame,
                PaddingBottom = dim(0, 10)
            })

            return setmetatable(cfg, library)
        end

        function library:slider(options)
            local cfg = {
                name = options.name or nil,
                suffix = options.suffix or "",
                flag = options.flag or tostring(2^789),
                callback = options.callback or function() end, 

                min = options.min or options.minimum or 0,
                max = options.max or options.maximum or 100,
                intervals = options.interval or options.decimal or 1,
                default = options.default or 10,

                dragging = false,
                value = options.default or 10, 

                previous_holder = self
            } 

            local slider = library:create("TextLabel", {
                Parent = self.holder, 
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "",
                Name = "slider",
                ZIndex = 2,
                Size = dim2(1, -8, 0, 12),
                BorderSizePixel = 0,
                BackgroundTransparency = 1,
                TextXAlignment = Enum.TextXAlignment.Left,
                AutomaticSize = Enum.AutomaticSize.Y,
                TextYAlignment = Enum.TextYAlignment.Top,
                TextSize = 11,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            if cfg.name then 
                local left_components = library:create("Frame", {
                    Parent = slider,
                    Name = "left_components",
                    BackgroundTransparency = 1,
                    Position = dim2(0, 2, 0, -1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 0, 0, 14),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local text = library:create("TextLabel", {
                    Parent = left_components,
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = cfg.name,
                    Name = "text",
                    BackgroundTransparency = 1,
                    Size = dim2(0, 0, 1, -1),
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.X,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                }, "text")

                library:create("UIListLayout", {
                    Parent = left_components,
                    Padding = dim(0, 5),
                    Name = "_",
                    FillDirection = Enum.FillDirection.Horizontal
                })
            end 
            
            local bottom_components = library:create("Frame", {
                Parent = slider,
                Name = "bottom_components",
                Position = dim2(0, 0, 0, cfg.name and 15 or 0),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 26, 0, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local slider = library:create("TextButton", {
                Parent = bottom_components,
                Name = "slider",
                Position = dim2(0, 0, 0, 2),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -27, 1, 12),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38),
                Text = "",
                AutoButtonColor = false,
            })library:apply_theme(slider, "outline", "BackgroundColor3") 
            
            local inline = library:create("Frame", {
                Parent = slider,
                Name = "inline",
                Position = dim2(0, 0, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -1, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })library:apply_theme(inline, "inline", "BackgroundColor3") 
            
            local background = library:create("Frame", {
                Parent = inline,
                Name = "background",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })library:apply_theme(background, "accent", "BackgroundColor3") 
            
            local contrast = library:create("Frame", {
                Parent = background,
                Name = "contrast",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local slidertext = library:create("TextLabel", {
                Parent = contrast,
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "12.50/100.00",
                Name = "text",
                BackgroundTransparency = 1,
                Position = dim2(0, 0, 0, -1),
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                TextSize = 12,
                ZIndex = 2,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local fill = library:create("Frame", {
                Parent = contrast,
                Name = "fill",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(0.75, 0, 1, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })library:apply_theme(fill, "accent", "BackgroundColor3") 
            
            local UIGradient = library:create("UIGradient", {
                Parent = fill,
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(255, 255, 255)),
                rgbkey(1, rgb(167, 167, 167))
            }
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = contrast,
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(41, 41, 55)),
                rgbkey(1, rgb(35, 35, 47))
            }
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = background,
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(255, 255, 255)),
                rgbkey(1, rgb(167, 167, 167))
            }
            }) 
            
            library:create("UIListLayout", {
                Parent = bottom_components,
                Padding = dim(0, 10),
                Name = "_",
                SortOrder = Enum.SortOrder.LayoutOrder
            })
            
            library:create("UIPadding", {
                Parent = slider,
                PaddingLeft = dim(0, 1)
            })

            function cfg.set(value)
                cfg.value = math.clamp(library:round(value, cfg.intervals), cfg.min, cfg.max)

                fill.Size = dim2((cfg.value - cfg.min) / (cfg.max - cfg.min), 0, 1, 0)
                slidertext.Text = tostring(cfg.value) .. cfg.suffix .. "/" .. tostring(cfg.max) .. cfg.suffix
                flags[cfg.flag] = cfg.value

                cfg.callback(flags[cfg.flag])
            end

            library:connection(uis.InputChanged, function(input)
                if cfg.dragging and input.UserInputType == Enum.UserInputType.MouseMovement then 
                    local size_x = (input.Position.X - slider.AbsolutePosition.X) / slider.AbsoluteSize.X
                    local value = ((cfg.max - cfg.min) * size_x) + cfg.min
                    cfg.set(value)
                end
            end)

            library:connection(uis.InputEnded, function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    cfg.dragging = false 
                end 
            end)

            slider.MouseButton1Down:Connect(function()
                cfg.dragging = true
            end)

            cfg.set(cfg.default)
            
            config_flags[cfg.flag] = cfg.set

            library.config_flags[cfg.flag] = cfg.set

            return self 
        end 

        function library:toggle(options)
            local cfg = {
                enabled = options.enabled or nil,
                name = options.name or "Toggle",
                flag = options.flag or tostring(math.random(1,9999999)),
                callback = options.callback or function() end,
                default = options.default or false,
                colorpicker = options.color or nil,
                previous_holder = self
            }
            
            -- instances
                local toggle_holder = library:create("TextButton", {
                    Parent = self.holder,
                    FontFace = library.font,
                    TextColor3 = rgb(151, 151, 151),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "",
                    Name = "toggle",
                    ZIndex = 1,
                    Size = dim2(1, -8, 0, 12),
                    BorderSizePixel = 0,
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextYAlignment = Enum.TextYAlignment.Top,
                    TextSize = 11,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
            
                local right_components = library:create("Frame", {
                    Parent = toggle_holder,
                    Name = "right_components",
                    Position = dim2(1, -1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 0, 0, 12),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                cfg["right_holder"] = right_components
            
                local list = library:create("UIListLayout", {
                    Parent = right_components,
                    VerticalAlignment = Enum.VerticalAlignment.Center,
                    FillDirection = Enum.FillDirection.Horizontal,
                    HorizontalAlignment = Enum.HorizontalAlignment.Right,
                    Padding = dim(0, 4),
                    Name = "list",
                    SortOrder = Enum.SortOrder.LayoutOrder
                })
            
                library:create("UIPadding", {
                    Parent = toggle_holder
                })
            
                local left_components = library:create("Frame", {
                    Parent = toggle_holder,
                    Name = "left_components",
                    BackgroundTransparency = 1,
                    Position = dim2(0, 0, 0, 0),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 0, 0, 14),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
            
                local text = library:create("TextLabel", {
                    Parent = left_components,
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = cfg.name,
                    Name = "text",
                    BackgroundTransparency = 1,
                    Size = dim2(0, 0, 1, -1),
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.X,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
            
                library:create("UIStroke", {
                    Parent = text,
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
            
                library:create("UIListLayout", {
                    Parent = left_components,
                    Padding = dim(0, 5),
                    Name = "_",
                    FillDirection = Enum.FillDirection.Horizontal
                })
            
                local toggle = library:create("TextButton", {
                    Parent = left_components,
                    Name = "!toggle",
                    Text = "",
                    AutoButtonColor = false,
                    Position = dim2(0, 0, 0, 2),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 15, 0, 15),
                    BorderSizePixel = 0,
                    ZIndex = 1, 
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(toggle, "outline", "BackgroundColor3") 
            
                local inline = library:create("Frame", {
                    Parent = toggle,
                    Name = "inline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(inline, "inline", "BackgroundColor3") 
            
                local background = library:create("Frame", {
                    Parent = inline,
                    Name = "background",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = themes.preset.accent
                })library:apply_theme(background, "accent", "BackgroundColor3") 
            
                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Rotation = 90,
                    Color = rgbseq{
                        rgbkey(0, rgb(255, 255, 255)),
                        rgbkey(1, rgb(167, 167, 167))
                    }
                })
            -- 
        
            function cfg.set(bool)
                background.BackgroundColor3 = bool and themes.preset.accent or themes.preset.inline
        
                flags[cfg.flag] = bool
                
                cfg.callback(bool)
            end
        
            library:connection(toggle_holder.MouseButton1Click, function()
                cfg.enabled = not cfg.enabled
        
                cfg.set(cfg.enabled)
            end)

            library:connection(toggle.MouseButton1Click, function()
                cfg.enabled = not cfg.enabled
        
                cfg.set(cfg.enabled)
            end)

            cfg.set(cfg.default)
        
            self.previous_holder = left_components
            self.right_holder = right_components
            
            library.config_flags[cfg.flag] = cfg.set

            return self
        end
        
        function library:colorpicker(options)
            local parent = self.right_holder
            
            local cfg = {
                name = options.name or "Color", 
                flag = options.flag or tostring(2^789),
                color = options.color or Color3.new(1, 1, 1), -- Default to white color if not provided
                alpha = options.alpha or 1,
                callback = options.callback or function() end,
                holder = self.previous_holder
            }

            local dragging_sat = false 
            local dragging_hue = false 
            local dragging_alpha = false 

            local h, s, v = cfg.color:ToHSV() 
            local a = cfg.alpha 
            
            -- colorpicker button 
                local colorpicker_button = library:create("TextButton", {
                    Parent = parent,
                    Name = "outline",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 24, 0, 14),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38),
                    Text = "",
                    AutoButtonColor = false,
                })library:apply_theme(colorpicker_button, "outline", "BackgroundColor3") 
            
                local inline = library:create("Frame", {
                    Parent = colorpicker_button,
                    Name = "inline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(inline, "inline", "BackgroundColor3") 
            
                local handler = library:create("Frame", {
                    Parent = inline,
                    Name = "handler",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(250, 165, 27)
                })
            
                local UIGradient = library:create("UIGradient", {
                    Parent = handler,
                    Rotation = 90,
                    Color = rgbseq{
                        rgbkey(0, rgb(255, 255, 255)),
                        rgbkey(1, rgb(167, 167, 167))
                    }
                })
            -- 

            -- colorpicker instances
                local colorpicker_holder = library:create("Frame", {
                    Parent = sgui,
                    Name = "colorpicker",
                    Position = dim2(0, colorpicker_button.AbsolutePosition.X + 1, 0, colorpicker_button.AbsolutePosition.Y + 17),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 190, 0, 190),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38),
                    Visible = false,
                    ZIndex = 1
                })library:apply_theme(colorpicker_holder, "outline", "BackgroundColor3") 

                library:make_resizable(colorpicker_holder)
                
                local window_inline = library:create("Frame", {
                    Parent = colorpicker_holder,
                    Name = "window_inline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = themes.preset.accent
                })library:apply_theme(window_inline, "accent", "BackgroundColor3") 
                
                local window_holder = library:create("Frame", {
                    Parent = window_inline,
                    Name = "window_holder",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(32, 32, 38),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })library:apply_theme(window_holder, "outline", "BackgroundColor3") 
                
                local UIGradient = library:create("UIGradient", {
                    Parent = window_holder,
                    Rotation = 90,
                    Name = "_",
                    Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                local text = library:create("TextLabel", {
                    Parent = window_holder,
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = cfg.name,
                    Name = "text",
                    BackgroundTransparency = 1,
                    Position = dim2(0, 2, 0, 4),
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.XY,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIStroke", {
                    Parent = text,
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
                
                library:create("UIPadding", {
                    Parent = window_holder,
                    Name = "_",
                    PaddingBottom = dim(0, 4),
                    PaddingRight = dim(0, 4),
                    PaddingLeft = dim(0, 4)
                })
                
                local main_holder = library:create("Frame", {
                    Parent = window_holder,
                    Name = "main_holder",
                    Position = dim2(0, 0, 0, 20),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, -20),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(main_holder, "inline", "BackgroundColor3") 
                
                local main_holder_inline = library:create("Frame", {
                    Parent = main_holder,
                    Name = "main_holder_inline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(main_holder_inline, "outline", "BackgroundColor3") 
                
                local main_holder_background = library:create("Frame", {
                    Parent = main_holder_inline,
                    Name = "main_holder_background",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = main_holder_background,
                    Rotation = 90,
                    Name = "_",
                    Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                library:create("UIPadding", {
                    Parent = main_holder_background,
                    PaddingTop = dim(0, 4),
                    Name = "_",
                    PaddingBottom = dim(0, 4),
                    PaddingRight = dim(0, 4),
                    PaddingLeft = dim(0, 4)
                })
                
                local alpha = library:create("TextButton", {
                    Parent = main_holder_background,
                    AnchorPoint = vec2(0, 0.5),
                    Name = "alpha",
                    Position = dim2(0, 0, 1, -8),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -20, 0, 14),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75),
                    Text = "",
                    AutoButtonColor = false,
                })library:apply_theme(alpha, "inline", "BackgroundColor3") 
                
                local outline = library:create("Frame", {
                    Parent = alpha,
                    Name = "outline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(outline, "outline", "BackgroundColor3") 
                
                local alpha_drag = library:create("Frame", {
                    Parent = outline,
                    Name = "alpha_drag",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(0, 221, 255)
                })
                
                local alphaind = library:create("ImageLabel", {
                    Parent = alpha_drag,
                    ScaleType = Enum.ScaleType.Tile,
                    BorderColor3 = rgb(0, 0, 0),
                    Image = "rbxassetid://18274452449",
                    BackgroundTransparency = 1,
                    Name = "alphaind",
                    Size = dim2(1, 0, 1, 0),
                    TileSize = dim2(0, 6, 0, 6),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = alphaind,
                    Transparency = numseq{
                        numkey(0, 0),
                        numkey(1, 1)
                    }
                })
                
                local alpha_picker = library:create("Frame", {
                    Parent = alpha_drag,
                    Name = "alpha_picker",
                    BorderMode = Enum.BorderMode.Inset,
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 4, 1, 0),
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local hue = library:create("TextButton", {
                    Parent = main_holder_background,
                    AnchorPoint = vec2(1, 0),
                    Name = "hue",
                    Position = dim2(1, -1, 0, 0),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 14, 1, -20),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75),
                    Text = "",
                    AutoButtonColor = false
                })
                
                local outline = library:create("Frame", {
                    Parent = hue,
                    Name = "outline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })
                
                local Frame = library:create("Frame", {
                    Parent = outline,
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = Frame,
                    Rotation = 270,
                    Color = rgbseq{
                    rgbkey(0, rgb(255, 0, 0)),
                    rgbkey(0.17000000178813934, rgb(255, 255, 0)),
                    rgbkey(0.33000001311302185, rgb(0, 255, 0)),
                    rgbkey(0.5, rgb(0, 255, 255)),
                    rgbkey(0.6700000166893005, rgb(0, 0, 255)),
                    rgbkey(0.8299999833106995, rgb(255, 0, 255)),
                    rgbkey(1, rgb(255, 0, 0))
                }
                }) 
                
                local hue_picker = library:create("Frame", {
                    Parent = Frame,
                    Name = "hue_picker",
                    BorderMode = Enum.BorderMode.Inset,
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 0, 4),
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local visualize = library:create("Frame", {
                    Parent = main_holder_background,
                    AnchorPoint = vec2(1, 1),
                    Name = "visualize",
                    Position = dim2(1, -1, 1, -1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 14, 0, 14),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(visualize, "inline", "BackgroundColor3") 
                
                local outline = library:create("Frame", {
                    Parent = visualize,
                    Size = dim2(1, -2, 1, -2),
                    Name = "outline",
                    Active = true,
                    BorderColor3 = rgb(0, 0, 0),
                    Position = dim2(0, 1, 0, 1),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(outline, "outline", "BackgroundColor3") 
                
                local visualize = library:create("Frame", {
                    Parent = outline,
                    Size = dim2(1, -2, 1, -2),
                    Name = "visualize",
                    Active = true,
                    BorderColor3 = rgb(0, 0, 0),
                    Position = dim2(0, 1, 0, 1),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(0, 221, 255)
                })
                
                local satval_picker = library:create("Frame", {
                    Parent = main_holder_background,
                    Name = "satval_picker",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -20, 1, -20),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(satval_picker, "inline", "BackgroundColor3") 
                
                local outline = library:create("Frame", {
                    Parent = satval_picker,
                    Name = "outline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(outline, "outline", "BackgroundColor3") 
                
                local colorpicker = library:create("Frame", {
                    Parent = outline,
                    Name = "colorpicker",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(0, 221, 255)
                })
                
                local sat = library:create("TextButton", {
                    Parent = colorpicker,
                    Name = "sat",
                    Size = dim2(1, 0, 1, 0),
                    BorderColor3 = rgb(0, 0, 0),
                    ZIndex = 2,
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255),
                    Text = "",
                    AutoButtonColor = false,
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = sat,
                    Rotation = 270,
                    Transparency = numseq{
                        numkey(0, 0),
                        numkey(1, 1)
                    },
                    Color = rgbseq{
                        rgbkey(0, rgb(0, 0, 0)),
                        rgbkey(1, rgb(0, 0, 0))
                    }
                })
                
                local val = library:create("TextButton", {
                    Parent = colorpicker,
                    Name = "val",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, 0),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255),
                    Text = "",
                    AutoButtonColor = false,
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = val,
                    Transparency = numseq{
                        numkey(0, 0),
                        numkey(1, 1)
                    }
                })
                
                local satval_picker_REAL = library:create("Frame", {
                    Parent = colorpicker,
                    Name = "satval_picker_REAL",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 2, 0, 2),
                    BorderSizePixel = 1,
                    BackgroundColor3 = rgb(255, 255, 255),
                    ZIndex = 3, 
                })
            -- 
                
            function cfg.set_visible(bool)
                colorpicker_holder.Visible = bool

                if bool then 
                    if library.current_element_open and library.current_element_open ~= cfg then 
                        library.current_element_open.set_visible(false)
                        library.current_element_open.open = false 
                    end

                    library.current_element_open = cfg 
                end
            end 

            colorpicker_button.MouseButton1Click:Connect(function()		
                cfg.open = not cfg.open

                cfg.set_visible(cfg.open) 
            end)

            colorpicker_button:GetPropertyChangedSignal("AbsolutePosition"):Connect(function()
                colorpicker_holder.Position = dim2(0, colorpicker_button.AbsolutePosition.X + 1, 0, colorpicker_button.AbsolutePosition.Y + 17)
            end)

            function cfg.set(color, alpha)
                if color then 
                    h, s, v = color:ToHSV()
                end 
            
                if alpha then 
                    a = alpha
                end 
            
                local hsv_position = Color3.fromHSV(h, s, v)
                local Color = Color3.fromHSV(h, s, v)
                
                local value = 1 - h
                local offset = (value < 1) and 0 or -4
                hue_picker.Position = dim2(0, 0, value, offset)

                local offset = (a < 1) and 0 or -4
                alpha_picker.Position = dim2(a, offset, 0, 0)

                alpha_drag.BackgroundColor3 = Color3.fromHSV(h, s, v)
                
                visualize.BackgroundColor3 = Color
                handler.BackgroundColor3 = Color 

                colorpicker.BackgroundColor3 = Color3.fromHSV(h, 1, 1)
                
                cfg.color = Color
                cfg.alpha = a
                
                local s_offset = (s < 1) and 0 or -3
                local v_offset = (1 - v < 1) and 0 or -3
                satval_picker_REAL.Position = dim2(s, s_offset, 1 - v, v_offset)

                flags[cfg.flag] = {} 
                flags[cfg.flag]["Color"] = Color
                flags[cfg.flag]["Transparency"] = a
            
                cfg.callback(Color, a)
            end

            function cfg.update_color() 
                local mouse = uis:GetMouseLocation() 

                if dragging_sat then	
                    s = math.clamp((vec2(mouse.X, mouse.Y - gui_offset) - val.AbsolutePosition).X / val.AbsoluteSize.X, 0, 1)
                    v = 1 - math.clamp((vec2(mouse.X, mouse.Y - gui_offset) - sat.AbsolutePosition).Y / sat.AbsoluteSize.Y, 0, 1)
                elseif dragging_hue then 
                    h = math.clamp(1 - (vec2(mouse.X, mouse.Y - gui_offset) - hue.AbsolutePosition).Y / hue.AbsoluteSize.Y, 0, 1)
                elseif dragging_alpha then 
                    a = math.clamp((vec2(mouse.X, mouse.Y - gui_offset) - alpha.AbsolutePosition).X / alpha.AbsoluteSize.X, 0, 1)
                end

                cfg.set(nil, nil)
            end

            alpha.MouseButton1Down:Connect(function()
                dragging_alpha = true 
            end)

            hue.MouseButton1Down:Connect(function()
                dragging_hue = true 
            end)

            sat.MouseButton1Down:Connect(function()
                dragging_sat = true  
            end)

            uis.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging_sat = false
                    dragging_hue = false
                    dragging_alpha = false 
                end
            end)

            uis.InputChanged:Connect(function(input)
                if (dragging_sat or dragging_hue or dragging_alpha) and input.UserInputType == Enum.UserInputType.MouseMovement then
                    cfg.update_color() 
                end
            end)	

            cfg.set(cfg.color, cfg.alpha)

            self.previous_holder = parent

            library.config_flags[cfg.flag] = cfg.set
            
            return self 
        end

        function library:keybind(options)
            local parent = self.right_holder

            local cfg = {
                flag = options.flag or "SET ME A FLAG NOWWW!!!!",
                callback = options.callback or function() end,
                open = false,
                binding = nil, 
                name = options.name or nil, 
                ignore_key = options.ignore or false, 

                key = options.key or nil, 
                mode = options.mode or "toggle",
                active = options.default or false, 

                hold_instances = {},
            }

            flags[cfg.flag] = {} 
            
            local KEYBIND_ELEMENT;
            if cfg.name then 
                KEYBIND_ELEMENT = library:create("TextLabel", {
                    Parent = library.keybind_list,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = Color3.fromRGB(180, 180, 180),
                    BorderColor3 = Color3.fromRGB(0, 0, 0),
                    Text = "[ Hold ]  Fly - X",
                    Size = UDim2.new(1, -5, 0, 18),
                    Visible = false, 
                    Position = UDim2.new(0, 5, 0, -1),
                    BorderSizePixel = 0,
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    TextTruncate = Enum.TextTruncate.AtEnd,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextSize = 12,
                    BackgroundColor3 = themes.preset.text
                }, "text")
            end 

            local element_outline = library:create("TextButton", {
                Parent = parent,
                Name = "",
                BorderColor3 = rgb(0, 0, 0),
                Text = "", 
                Size = dim2(0, 24, 0, 14),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38)
            })library:apply_theme(element_outline, "outline", "BackgroundColor3") 
            
            local inline = library:create("Frame", {
                Parent = element_outline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })library:apply_theme(inline, "inline", "BackgroundColor3") 
            
            local handler = library:create("Frame", {
                Parent = inline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = handler,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
            })library:apply_theme(UIGradient, "contrast", "Color") 
            
            local key_text = library:create("TextLabel", {
                Parent = handler,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "b",
                Size = dim2(1, 0, 1, 0),
                BackgroundTransparency = 1,
                Position = dim2(0, 0, 0, -2),
                BorderSizePixel = 0,
                AutomaticSize = Enum.AutomaticSize.XY,
                TextSize = 12,
                BackgroundColor3 = rgb(255, 255, 255)
            })
                
            -- mode selector
                local keybind_selector = library:create("Frame", {
                    Parent = sgui,
                    Name = "",
                    Position = dim2(0, element_outline.AbsolutePosition.X + 1, 0, element_outline.AbsolutePosition.Y + 17),
                    BorderColor3 = rgb(255, 255, 255),
                    BorderSizePixel = 2,
                    Visible = false, 
                    AutomaticSize = Enum.AutomaticSize.XY,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIListLayout", {
                    Parent = keybind_selector,
                    Name = "",
                    SortOrder = Enum.SortOrder.LayoutOrder,
                    HorizontalFlex = Enum.UIFlexAlignment.Fill,
                    Padding = dim(0, 2)
                })
                
                local hold_button = library:create("TextButton", {
                    Parent = keybind_selector,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "hold",
                    BackgroundTransparency = 1,
                    AutomaticSize = Enum.AutomaticSize.XY,
                    BorderSizePixel = 0,
                    ZIndex = 2,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIStroke", {
                    Parent = hold_button,
                    Name = "",
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
                
                library:create("UIPadding", {
                    Parent = keybind_selector,
                    Name = "",
                    PaddingTop = dim(0, 3),
                    PaddingBottom = dim(0, 5),
                    PaddingRight = dim(0, 5),
                    PaddingLeft = dim(0, 5)
                })
                
                local toggle_button = library:create("TextButton", {
                    Parent = keybind_selector,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "toggle",
                    BackgroundTransparency = 1,
                    AutomaticSize = Enum.AutomaticSize.XY,
                    BorderSizePixel = 0,
                    ZIndex = 2,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIStroke", {
                    Parent = toggle_button,
                    Name = "",
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
                
                local always_button = library:create("TextButton", {
                    Parent = keybind_selector,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "always",
                    BackgroundTransparency = 1,
                    AutomaticSize = Enum.AutomaticSize.XY,
                    BorderSizePixel = 0,
                    ZIndex = 2,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIStroke", {
                    Parent = always_button,
                    Name = "",
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = keybind_selector,
                    Name = "",
                    Rotation = 90,
                    Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
                })
                
                local UIStroke = library:create("UIStroke", {
                    Parent = keybind_selector,
                    Name = "",
                    Color = rgb(60, 55, 75),
                    LineJoinMode = Enum.LineJoinMode.Miter,
                    ApplyStrokeMode = Enum.ApplyStrokeMode.Border
                })
            -- 

            -- init 
                element_outline:GetPropertyChangedSignal("AbsolutePosition"):Connect(function()
                    keybind_selector.Position = dim2(0, element_outline.AbsolutePosition.X + 1, 0, element_outline.AbsolutePosition.Y + 17)
                end)

                function cfg.set_visible(bool)
                    keybind_selector.Visible = bool

                    if bool then 
                        if library.current_element_open and library.current_element_open ~= cfg then 
                            library.current_element_open.set_visible(false)
                            library.current_element_open.open = false 
                        end

                        library.current_element_open = cfg 
                    end
                end 

                function cfg.set_mode(mode) 
                    cfg.mode = mode 

                    if mode == "always" then
                        cfg.set(true)
                    elseif mode == "hold" then
                        cfg.set(false)
                    end

                    flags[cfg.flag]["mode"] = mode
                end 

                function cfg.set(input)
                    if type(input) == "boolean" then 
                        local __cached = input 

                        if cfg.mode == "always" then 
                            __cached = true 
                        end 

                        cfg.active = __cached 
                        flags[cfg.flag]["active"] = __cached 
                        cfg.callback(__cached)
                    elseif tostring(input):find("Enum") then 
                        input = input.Name == "Escape" and "..." or input

                        cfg.key = input or "..."	

                        local _text = keys[cfg.key] or tostring(cfg.key):gsub("Enum.", "")
                        local _text2 = (tostring(_text):gsub("KeyCode.", ""):gsub("UserInputType.", "")) or "..."
                        cfg.key_name = _text2

                        flags[cfg.flag]["mode"] = cfg.mode 
                        flags[cfg.flag]["key"] = cfg.key 

                        key_text.Text = string.lower(_text2)

                        cfg.callback(cfg.active or false)
                    elseif table.find({"toggle", "hold", "always"}, input) then 
                        cfg.set_mode(input)

                        if input == "always" then 
                            cfg.active = true 
                        end 

                        cfg.callback(cfg.active or false)
                    elseif type(input) == "table" then 
                        input.key = type(input.key) == "string" and input.key ~= "..." and library:convert_enum(input.key) or input.key

                        input.key = input.key == Enum.KeyCode.Escape and "..." or input.key
                        cfg.key = input.key or "..."
                        
                        cfg.mode = input.mode or "toggle"

                        if input.active then
                            cfg.active = input.active
                        end

                        local text = tostring(cfg.key) ~= "Enums" and (keys[cfg.key] or tostring(cfg.key):gsub("Enum.", "")) or nil
                        local __text = text and (tostring(text):gsub("KeyCode.", ""):gsub("UserInputType.", ""))
                        
                        key_text.Text = string.lower(__text) or "..."
                        cfg.key_name = __text
                    end 

                    flags[cfg.flag] = {
                        mode = cfg.mode,
                        key = cfg.key, 
                        active = cfg.active
                    }
                    
                    if cfg.name then 
                        KEYBIND_ELEMENT.Visible = cfg.active
                    end 

                    local text = tostring(cfg.key) ~= "Enums" and (keys[cfg.key] or tostring(cfg.key):gsub("Enum.", "")) or nil
                    local __text = text and (tostring(text):gsub("KeyCode.", ""):gsub("UserInputType.", ""))

                    if cfg.name then 
                        KEYBIND_ELEMENT.Text = "[ " .. string.upper(string.sub(cfg.mode, 1, 1)) .. string.sub(cfg.mode, 2) .. " ] " .. cfg.name .. " - " .. __text
                    end 
                end

                hold_button.MouseButton1Click:Connect(function()
                    cfg.set_mode("hold") 
                    cfg.set_visible(false)
                    cfg.open = false 
                end) 

                toggle_button.MouseButton1Click:Connect(function()
                    cfg.set_mode("toggle") 
                    cfg.set_visible(false)
                    cfg.open = false 
                end) 

                always_button.MouseButton1Click:Connect(function()
                    cfg.set_mode("always") 
                    cfg.set_visible(false)
                    cfg.open = false 
                end) 

                element_outline.MouseButton2Click:Connect(function()
                    cfg.open = not cfg.open 

                    cfg.set_visible(cfg.open)
                end)

                element_outline.MouseButton1Down:Connect(function()
                    task.wait()
                    key_text.Text = "..."	

                    cfg.binding = library:connection(uis.InputBegan, function(keycode, game_event)  
                        cfg.set(keycode.KeyCode)
                        
                        cfg.binding:Disconnect() 
                        cfg.binding = nil
                    end)
                end)

                library:connection(uis.InputBegan, function(input, game_event) 
                    if not game_event then 
                        if input.KeyCode == cfg.key then 
                            if cfg.mode == "toggle" then 
                                cfg.active = not cfg.active
                                cfg.set(cfg.active)
                            elseif cfg.mode == "hold" then 
                                cfg.set(true)
                            end
                        end
                    end
                end)

                library:connection(uis.InputEnded, function(input, game_event) 
                    if game_event then 
                        return 
                    end 

                    local selected_key = input.UserInputType == Enum.UserInputType.Keyboard and input.KeyCode or input.UserInputType
        
                    if selected_key == cfg.key then
                        if cfg.mode == "hold" then 
                            cfg.set(false)
                        end
                    end
                end)
        
                cfg.set({mode = cfg.mode, active = cfg.active, key = cfg.key})
        
                library.config_flags[cfg.flag] = cfg.set
            -- 
            
            self.previous_holder = parent

            library.config_flags[cfg.flag] = cfg.set

            return self 
        end 

        function library:dropdown(options) 
            local parent = self.holder 

            local cfg = {
                name = options.name or nil,
                flag = options.flag or tostring(math.random(1,9999999)),

                items = options.items or {"1", "2", "3"},
                callback = options.callback or function() end,
                multi = options.multi or false, 

                previous_holder = self, 
                open = false, 
                option_instances = {}, 
                multi_items = {}, 
                scrolling = options.scrolling or false, 

                previous_holder = self
            }
            cfg.default = options.default or (cfg.multi and {cfg.items[1]}) or cfg.items[1] or nil

            -- dropdown elements
                local dropdown = library:create("TextLabel", {
                    Parent = parent,
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "",
                    Name = "dropdown",
                    ZIndex = 2,
                    Size = dim2(1, -8, 0, 12),
                    BorderSizePixel = 0,
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextYAlignment = Enum.TextYAlignment.Top,
                    TextSize = 11,
                    BackgroundColor3 = rgb(255, 255, 255)
                })

                local main_text      
                if cfg.name then 
                    local left_components = library:create("Frame", {
                        Parent = dropdown,
                        Name = "left_components",
                        BackgroundTransparency = 1,
                        Position = dim2(0, 2, 0, -1),
                        BorderColor3 = rgb(0, 0, 0),
                        Size = dim2(0, 0, 0, 14),
                        BorderSizePixel = 0,
                        BackgroundColor3 = rgb(255, 255, 255)
                    })

                    main_text = library:create("TextLabel", {
                        Parent = left_components,
                        FontFace = library.font,
                        TextColor3 = rgb(180, 180, 180),
                        BorderColor3 = rgb(0, 0, 0),
                        Text = cfg.name,
                        Name = "text",
                        BackgroundTransparency = 1,
                        Size = dim2(0, 0, 1, -1),
                        BorderSizePixel = 0,
                        AutomaticSize = Enum.AutomaticSize.X,
                        TextSize = 12,
                        BackgroundColor3 = rgb(255, 255, 255)
                    })
                    
                    library:create("UIStroke", {
                        Parent = main_text,
                        LineJoinMode = Enum.LineJoinMode.Miter
                    })
                    
                    library:create("UIListLayout", {
                        Parent = left_components,
                        Padding = dim(0, 5),
                        Name = "_",
                        FillDirection = Enum.FillDirection.Horizontal
                    })

                    local right_components = library:create("Frame", {
                        Parent = dropdown,
                        Name = "right_components",
                        Position = dim2(1, -1, 0, 1),
                        BorderColor3 = rgb(0, 0, 0),
                        Size = dim2(0, 0, 0, 12),
                        BorderSizePixel = 0,
                        BackgroundColor3 = rgb(255, 255, 255)
                    })
                    cfg["right_holder"] = right_components
        
                    local list = library:create("UIListLayout", {
                        Parent = right_components,
                        VerticalAlignment = Enum.VerticalAlignment.Center,
                        FillDirection = Enum.FillDirection.Horizontal,
                        HorizontalAlignment = Enum.HorizontalAlignment.Right,
                        Padding = dim(0, 4),
                        Name = "list",
                        SortOrder = Enum.SortOrder.LayoutOrder
                    })

                    self.previous_holder = left_components
                    self.right_holder = right_components
                end 

                local bottom_components = library:create("Frame", {
                    Parent = dropdown,
                    Name = "bottom_components",
                    Position = dim2(0, 0, 0, cfg.name and 15 or 0),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 26, 0, 0),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local dropdown = library:create("TextButton", {
                    Parent = bottom_components,
                    Name = "dropdown",
                    Position = dim2(0, 0, 0, 2),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -27, 1, 18),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38),
                    Text = "",
                    AutoButtonColor = false, 
                })library:apply_theme(dropdown, "outline", "BackgroundColor3") 
                
                local inline = library:create("Frame", {
                    Parent = dropdown,
                    Name = "inline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(inline, "inline", "BackgroundColor3") 
                
                local background = library:create("Frame", {
                    Parent = inline,
                    Name = "background",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = themes.preset.accent
                })library:apply_theme(background, "accent", "BackgroundColor3") 
                
                local contrast = library:create("Frame", {
                    Parent = background,
                    Name = "contrast",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, 0),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })

                local plus = library:create("TextLabel", {
                    Parent = contrast,
                    TextWrapped = true,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "+",
                    Name = "plus",
                    Size = dim2(1, -4, 1, 0),
                    Position = dim2(0, 0, 0, -1),
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Right,
                    FontFace = library.font,
                    TextTruncate = Enum.TextTruncate.AtEnd,
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIStroke", {
                    Parent = plus,
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
                
                local text = library:create("TextLabel", {
                    Parent = contrast,
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "aa",
                    Name = "text",
                    Size = dim2(1, -4, 1, 0),
                    Position = dim2(0, 4, 0, -1),
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    BorderSizePixel = 0,
                    TextTruncate = Enum.TextTruncate.AtEnd,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIStroke", {
                    Parent = text,
                    LineJoinMode = Enum.LineJoinMode.Miter
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = contrast,
                    Rotation = 90,
                    Color = rgbseq{
                        rgbkey(0, rgb(41, 41, 55)),
                        rgbkey(1, rgb(35, 35, 47))
                    }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Rotation = 90,
                    Color = rgbseq{
                        rgbkey(0, rgb(255, 255, 255)),
                        rgbkey(1, rgb(167, 167, 167))
                    }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                library:create("UIListLayout", {
                    Parent = bottom_components,
                    Padding = dim(0, 10),
                    Name = "_",
                    SortOrder = Enum.SortOrder.LayoutOrder
                })     
            --

            -- dropdown holder
                local dropdown_holder = library:create("Frame", {
                    Parent = sgui,
                    BorderColor3 = rgb(0, 0, 0),
                    Name = "dropdown_holder",
                    BackgroundTransparency = 1,
                    Position = dim2(0, dropdown.AbsolutePosition.X + 1, 0, dropdown.AbsolutePosition.Y + 22),
                    Size = dim2(0, dropdown.AbsoluteSize.X, 0, cfg.scrolling and 180 or 0),
                    BorderSizePixel = 0,
                    AutomaticSize = cfg.scrolling and Enum.AutomaticSize.None or Enum.AutomaticSize.Y,
                    BackgroundColor3 = rgb(32, 32, 38),
                    Visible = false
                })
                
                local inline = library:create("Frame", {
                    Parent = dropdown_holder,
                    Size = dim2(1, -2, 1, 2),
                    Name = "inline",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    ZIndex = 2,
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(inline, "inline", "BackgroundColor3") 
                
                local background; 
                if not cfg.scrolling then 
                    background = library:create("Frame", {
                        Parent = inline,
                        BorderColor3 = rgb(0, 0, 0),
                        Name = "background",
                        BackgroundTransparency = 1,
                        Position = dim2(0, 1, 0, 1),
                        Size = dim2(1, -2, 1, 1),
                        ZIndex = 2,
                        BorderSizePixel = 0,
                        BackgroundColor3 = themes.preset.accent
                    })
                    library:apply_theme(background, "accent", "BackgroundColor3") 
                else 
                    background = library:create("ScrollingFrame", {
                        Parent = inline,
                        BorderColor3 = rgb(0, 0, 0),
                        Name = "background",
                        BackgroundTransparency = 1,
                        Position = dim2(0, 1, 0, 1),
                        Size = dim2(1, -2, 1, 1),
                        ZIndex = 2,
                        BorderSizePixel = 0,
                        BackgroundColor3 = themes.preset.accent,
                        CanvasSize = dim2(0, 0, 0, 0),
                        AutomaticCanvasSize = Enum.AutomaticSize.Y,
                        ScrollBarThickness = 2,
                        ScrollBarImageColor3 = themes.preset.accent
                    })
                    library:apply_theme(background, "accent", "BackgroundColor3") 
                end 
                
                local contrast = library:create("Frame", {
                    Parent = background,
                    Name = "contrast",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, -3),
                    BorderSizePixel = 0,
                    ZIndex = 2, 
                    BackgroundColor3 = rgb(255, 255, 255),
                    AutomaticSize = cfg.scrolling and Enum.AutomaticSize.Y or Enum.AutomaticSize.None,
                }); 

                library:create("UIPadding", {
                    Parent = contrast,
                    PaddingTop = dim(0, 2),
                    PaddingBottom = dim(0, 2),
                    PaddingRight = dim(0, 0),
                    PaddingLeft = dim(0, 4)
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = contrast,
                    Rotation = 90,
                    Color = rgbseq{
                        rgbkey(0, rgb(41, 41, 55)),
                        rgbkey(1, rgb(35, 35, 47))
                    }
                })library:apply_theme(UIGradient, "contrast", "Color") 
            
                library:create("UIListLayout", {
                    Parent = contrast,
                    Padding = dim(0, 5),
                    SortOrder = Enum.SortOrder.LayoutOrder
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Rotation = 90,
                    Color = rgbseq{
                        rgbkey(0, rgb(255, 255, 255)),
                        rgbkey(1, rgb(167, 167, 167))
                    }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                local stroke = library:create("UIStroke", {
                    Parent = inline,
                    Color = rgb(32, 32, 38),
                    LineJoinMode = Enum.LineJoinMode.Miter
                })library:apply_theme(stroke, "outline", "Color") 

                dropdown:GetPropertyChangedSignal("AbsoluteSize"):Connect(function()
                    dropdown_holder.Size = dim2(0, dropdown.AbsoluteSize.X, 0, 0)
                end)

                dropdown:GetPropertyChangedSignal("AbsolutePosition"):Connect(function()
                    dropdown_holder.Position = dim2(0, dropdown.AbsolutePosition.X + 1, 0, dropdown.AbsolutePosition.Y + 22)
                end)
            -- 
            
            function cfg.set_visible(bool) 
                dropdown_holder.Visible = bool

                plus.Text = bool and "-" or "+"
                plus.TextSize = bool and 12 or 8

                if bool then 
                    if library.current_element_open and library.current_element_open ~= cfg then 
                        library.current_element_open.set_visible(false)
                        library.current_element_open.open = false 
                    end

                    library.current_element_open = cfg 
                end
            end

            function cfg.set(value) 
                local selected = {}

                local is_table = type(value) == "table"

                for _,v in next, cfg.option_instances do 
                    if v.Text == value or (is_table and table.find(value, v.Text)) then 
                        table.insert(selected, v.Text)
                        cfg.multi_items = selected
                        v.TextColor3 = themes.preset.accent
                    else 
                        v.TextColor3 = themes.preset.text
                    end

                end

                text.Text = is_table and table.concat(selected, ", ") or selected[1] or "nun"
                flags[cfg.flag] = is_table and selected or selected[1]
                cfg.callback(flags[cfg.flag]) 
            end

            function cfg:refresh_options(refreshed_list) 
                for _, v in next, cfg.option_instances do 
                    v:Destroy() 
                end

                cfg.option_instances = {} 

                for i,v in next, refreshed_list do 
                    local TextButton = library:create("TextButton", {
                        Parent = contrast,
                        FontFace = library.font,
                        TextColor3 = themes.preset.text,
                        BorderColor3 = rgb(0, 0, 0),
                        Size = dim2(1, 0, 0, 0),
                        BackgroundTransparency = 1,
                        BorderSizePixel = 0,
                        TextWrapped = true,
                        AutomaticSize = Enum.AutomaticSize.Y,
                        TextSize = 12,
                        TextXAlignment = Enum.TextXAlignment.Left,
                        ZIndex = 2, 
                        Text = v,
                        BackgroundColor3 = rgb(255, 255, 255)
                    })library:apply_theme(TextButton, "accent", "TextColor3") 
                    
                    library:create("UIStroke", {
                        Parent = TextButton,
                        LineJoinMode = Enum.LineJoinMode.Miter
                    })

                    table.insert(cfg.option_instances, TextButton)

                    TextButton.MouseButton1Down:Connect(function()
                        if cfg.multi then 
                            local selected_index = table.find(cfg.multi_items, TextButton.Text)

                            if selected_index then 
                                table.remove(cfg.multi_items, selected_index)
                            else
                                table.insert(cfg.multi_items, TextButton.Text)
                            end

                            cfg.set(cfg.multi_items) 				
                        else 
                            cfg.set_visible(false)
                            cfg.open = false 

                            cfg.set(TextButton.Text)
                        end
                    end)
                end
            end

            dropdown.MouseButton1Click:Connect(function()
                cfg.open = not cfg.open 

                cfg.set_visible(cfg.open)
            end)

            cfg:refresh_options(cfg.items) 

            cfg.set(cfg.default)

            library.config_flags[cfg.flag] = cfg.set

            return self
        end 

        function library:list(options)
            local cfg = {
                callback = options and options.callback or function() end, 

                scale = options.size or 100, 
                items = options.items or {"1", "2", "3"}, 
                -- order = options.order or 1, 
                placeholdertext = options.placeholder or options.placeholdertext or "search here...",

                option_instances = {}, 
                current_instance = nil, 
                flag = options.flag or "SET A FLAG U NIGGER", 

                previous_holder = self
            } 

            local list_holder = library:create("TextLabel", {
                Parent = self.holder,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "",
                ZIndex = 2,
                Size = dim2(1, -8, 0, 12),
                BorderSizePixel = 0,
                BackgroundTransparency = 1,
                TextXAlignment = Enum.TextXAlignment.Left,
                AutomaticSize = Enum.AutomaticSize.Y,
                TextYAlignment = Enum.TextYAlignment.Top,
                TextSize = 11,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIPadding = library:create("UIPadding", {
                Parent = list_holder,
                Name = "",
                PaddingLeft = dim(0, 1)
            })
            
            local UIStroke = library:create("UIStroke", {
                Parent = list_holder,
                Name = ""
            })
            
            local bottom_components = library:create("Frame", {
                Parent = list_holder,
                Name = "",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 26, 0, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            library:create("UIListLayout", {
                Parent = bottom_components,
                Name = "",
                Padding = dim(0, 10),
                SortOrder = Enum.SortOrder.LayoutOrder
            })
            
            local list = library:create("Frame", {
                Parent = bottom_components,
                Name = "",
                Position = dim2(0, 0, 0, 2),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -27, 1, 232),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38)
            })library:apply_theme(main_holder, "outline", "BackgroundColor3") 
            
            local inline = library:create("Frame", {
                Parent = list,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })library:apply_theme(inline, "inline", "BackgroundColor3") 
            
            local background = library:create("Frame", {
                Parent = inline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })library:apply_theme(background, "accent", "BackgroundColor3") 
            
            local UIGradient = library:create("UIGradient", {
                Parent = background,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(255, 255, 255)),
                rgbkey(1, rgb(167, 167, 167))
            }
            })library:apply_theme(UIGradient, "contrast", "Color") 
            
            local contrast = library:create("Frame", {
                Parent = background,
                Name = "",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = contrast,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(41, 41, 55)),
                rgbkey(1, rgb(35, 35, 47))
            }
            })library:apply_theme(UIGradient, "contrast", "Color") 
            
            local ScrollingFrame = library:create("ScrollingFrame", {
                Parent = contrast,
                Name = "",
                ScrollBarImageColor3 = themes.preset.accent,
                Active = true,
                AutomaticCanvasSize = Enum.AutomaticSize.Y,
                ScrollBarThickness = 3,
                BackgroundTransparency = 1,
                Size = dim2(1, 0, 1, 0),
                BackgroundColor3 = rgb(255, 255, 255),
                BorderColor3 = rgb(0, 0, 0),
                BorderSizePixel = 0,
                CanvasSize = dim2(0, 0, 0, 0)
            })library:apply_theme(ScrollingFrame, "accent", "ScrollBarImageColor3") 
            
            local UIPadding = library:create("UIPadding", {
                Parent = ScrollingFrame,
                Name = "",
                PaddingBottom = dim(0, 4),
                PaddingTop = dim(0, 4)
            })
            
            local UIListLayout = library:create("UIListLayout", {
                Parent = ScrollingFrame,
                Name = "",
                Padding = dim(0, 4),
                SortOrder = Enum.SortOrder.LayoutOrder
            })

            function cfg.render_option(text) 
                local TextButton = library:create("TextButton", {
                    Parent = ScrollingFrame,
                    Name = "",
                    Text = tostring(text),
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    BackgroundTransparency = 1,
                    Size = dim2(1, 0, 0, 0),
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local UIStroke = library:create("UIStroke", {
                    Parent = TextButton,
                    Name = ""
                })

                return TextButton 
            end 

            function cfg.refresh_options(options) 
                if type(options) == "function" then return end 
                for _, v in next, cfg.option_instances do 
                    v:Destroy() 
                end 

                for _, option in next, options do 
                    local button = cfg.render_option(option) 

                    table.insert(cfg.option_instances, button)

                    button.MouseButton1Click:Connect(function()
                        if cfg.current_instance and cfg.current_instance ~= button then 
                            cfg.current_instance.TextColor3 = themes.preset.text 
                        end 

                        cfg.current_instance = button 
                        button.TextColor3 = themes.preset.accent 

                        flags[cfg.flag] = button.text
                        
                        cfg.callback(button.text)
                    end)
                end 
            end     

            function cfg.filter_options(text)
                for _, v in next, cfg.option_instances do 
                    if string.find(v.Text, text) then 
                        v.Visible = true 
                    else 
                        v.Visible = false
                    end
                end
            end 

            cfg.refresh_options(cfg.items) 
            
            return self, cfg
        end 

        function library:textbox(options)
            local cfg = {
                placeholder = options.placeholder or options.placeholdertext or options.holder or options.holdertext or "type here...",
                default = options.default,
                flag = options.flag or "SET ME NIGGA",
                callback = options.callback or function() end 
            }

            local textbox_holder = library:create("TextLabel", {
                Parent = self.holder,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "",
                ZIndex = 2,
                Size = dim2(1, -8, 0, 12),
                BorderSizePixel = 0,
                BackgroundTransparency = 1,
                TextXAlignment = Enum.TextXAlignment.Left,
                AutomaticSize = Enum.AutomaticSize.Y,
                TextYAlignment = Enum.TextYAlignment.Top,
                TextSize = 11,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            library:create("UIPadding", {
                Parent = textbox_holder,
                Name = "",
                PaddingLeft = dim(0, 1)
            })
            
            library:create("UIStroke", {
                Parent = textbox_holder,
                Name = ""
            })
            
            local button = library:create("Frame", {
                Parent = textbox_holder,
                Name = "",
                Position = dim2(0, 0, 0, 2),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -27, 0, 18),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38)
            })
            
            library:apply_theme(button, "outline", "BackgroundColor3") 
            
            local inline = library:create("Frame", {
                Parent = button,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })
            
            library:apply_theme(inline, "inline", "BackgroundColor3") 
            
            local background = library:create("Frame", {
                Parent = inline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })
            
            library:apply_theme(background, "accent", "BackgroundColor3") 
            
            local TextBox = library:create("TextBox", {
                Parent = background,
                Name = "",
                CursorPosition = -1,
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                ZIndex = 2,
                Text = "", 
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                TextWrapped = true,
                BackgroundTransparency = 1,
                TextTruncate = Enum.TextTruncate.SplitWord,
                PlaceholderText = "Type here...",
                ClearTextOnFocus = false,
                TextSize = 12,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            library:create("UIStroke", {
                Parent = TextBox,
                Name = ""
            })
            
            local TextButton = library:create("TextButton", {
                Parent = background,
                Name = "",
                FontFace = Font.new("rbxasset://fonts/families/SourceSansPro.json", Enum.FontWeight.Regular, Enum.FontStyle.Normal),
                TextColor3 = rgb(0, 0, 0),
                BorderColor3 = rgb(0, 0, 0),
                Text = "",
                AutoButtonColor = false,
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                TextSize = 14,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = TextButton,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(41, 41, 55)),
                rgbkey(1, rgb(35, 35, 47))
            }
            })
            
            library:apply_theme(UIGradient, "contrast", "Color") 
            
            library:create("UIListLayout", {
                Parent = textbox_holder,
                Name = "",
                FillDirection = Enum.FillDirection.Horizontal,
                HorizontalFlex = Enum.UIFlexAlignment.Fill,
                Padding = dim(0, 4),
                SortOrder = Enum.SortOrder.LayoutOrder
            })
            
            TextBox:GetPropertyChangedSignal("Text"):Connect(function()
                flags[cfg.flag] = TextBox.text
                cfg.callback(TextBox.text)
            end)

            function cfg.set(text) 
                flags[cfg.flag] = text
                TextBox.Text = text
                cfg.callback(text)
            end 

            if cfg.default then 
                cfg.set(cfg.default) 
            end 

            library.config_flags[cfg.flag] = cfg.set

            return self 
        end 

        function library:button_holder(options) 
            local button_holder = library:create("TextLabel", {
                Parent = self.holder,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "",
                ZIndex = 2,
                Size = dim2(1, -8, 0, 12),
                BorderSizePixel = 0,
                BackgroundTransparency = 1,
                TextXAlignment = Enum.TextXAlignment.Left,
                AutomaticSize = Enum.AutomaticSize.Y,
                TextYAlignment = Enum.TextYAlignment.Top,
                TextSize = 11,
                BackgroundColor3 = rgb(255, 255, 255), 
            })

            self.current_holder = button_holder
            
            library:create("UIStroke", {
                Parent = button_holder,
                Name = ""
            })
            
            library:create("UIListLayout", {
                Parent = button_holder,
                Name = "",
                FillDirection = Enum.FillDirection.Horizontal,
                HorizontalFlex = Enum.UIFlexAlignment.Fill,
                Padding = dim(0, 4),
                SortOrder = Enum.SortOrder.LayoutOrder
            })
        
            return self
        end 

        function library:button(options)
            local cfg = {
                callback = options.callback or function() end, 
                name = options.text or options.name or "Button",
            }   

            local button = library:create("TextButton", {
                Parent = self.current_holder,
                Name = "",
                Position = dim2(0, 0, 0, 2),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -27, 0, 18),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(32, 32, 38),
                Text = ""
            })
            
            library:apply_theme(button, "outline", "BackgroundColor3") 
            
            local inline = library:create("Frame", {
                Parent = button,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(60, 55, 75)
            })
            
            library:apply_theme(inline, "inline", "BackgroundColor3") 
            
            local background = library:create("Frame", {
                Parent = inline,
                Name = "",
                Position = dim2(0, 1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, -2, 1, -2),
                BorderSizePixel = 0,
                BackgroundColor3 = themes.preset.accent
            })
            
            library:apply_theme(background, "accent", "BackgroundColor3") 
            
            local _UIGradient = library:create("UIGradient", {
                Parent = background,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(255, 255, 255)),
                rgbkey(1, rgb(167, 167, 167))
            }
            })
            
            library:apply_theme(_UIGradient, "contrast", "Color") 
            
            local contrast = library:create("Frame", {
                Parent = background,
                Name = "",
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(1, 0, 1, 0),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIGradient = library:create("UIGradient", {
                Parent = contrast,
                Name = "",
                Rotation = 90,
                Color = rgbseq{
                rgbkey(0, rgb(41, 41, 55)),
                rgbkey(1, rgb(35, 35, 47))
            }
            })
            
            library:apply_theme(UIGradient, "contrast", "Color") 
            
            local text = library:create("TextLabel", {
                Parent = contrast,
                Name = "",
                TextWrapped = true,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = cfg.name,
                Size = dim2(1, -4, 1, 0),
                Position = dim2(0, 4, 0, -1),
                BackgroundTransparency = 1,
                TextTruncate = Enum.TextTruncate.AtEnd,
                BorderSizePixel = 0,
                FontFace = library.font,
                TextSize = 12,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIStroke = library:create("UIStroke", {
                Parent = text,
                Name = "",
                LineJoinMode = Enum.LineJoinMode.Miter
            })

            button.MouseButton1Click:Connect(function()
                cfg.callback() 
            end)

            return self
        end 

        function library:label(options)
            local cfg = {name = options.text or options.name or "Label", previous_holder = self}

            local dropdown = library:create("TextLabel", {
                Parent = self.holder,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = "",
                ZIndex = 2,
                Size = dim2(1, -8, 0, 12),
                BorderSizePixel = 0,
                BackgroundTransparency = 1,
                TextXAlignment = Enum.TextXAlignment.Left,
                AutomaticSize = Enum.AutomaticSize.Y,
                TextYAlignment = Enum.TextYAlignment.Top,
                TextSize = 11,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local UIStroke = library:create("UIStroke", {
                Parent = dropdown,
                Name = ""
            })
            
            local left_components = library:create("Frame", {
                Parent = dropdown,
                Name = "",
                BackgroundTransparency = 1,
                Position = dim2(0, 2, 0, -1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(0, 0, 0, 14),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })
            
            local TextLabel = library:create("TextLabel", {
                Parent = left_components,
                Name = "",
                FontFace = library.font,
                TextColor3 = rgb(180, 180, 180),
                BorderColor3 = rgb(0, 0, 0),
                Text = cfg.name,
                BackgroundTransparency = 1,
                TextXAlignment = Enum.TextXAlignment.Left,
                BorderSizePixel = 0,
                AutomaticSize = Enum.AutomaticSize.Y,
                TextSize = 12,
                BackgroundColor3 = rgb(255, 255, 255)
            })

            local right_components = library:create("Frame", {
                Parent = dropdown,
                Name = "right_components",
                Position = dim2(1, -1, 0, 1),
                BorderColor3 = rgb(0, 0, 0),
                Size = dim2(0, 0, 0, 12),
                BorderSizePixel = 0,
                BackgroundColor3 = rgb(255, 255, 255)
            })

            local list = library:create("UIListLayout", {
                Parent = right_components,
                VerticalAlignment = Enum.VerticalAlignment.Center,
                FillDirection = Enum.FillDirection.Horizontal,
                HorizontalAlignment = Enum.HorizontalAlignment.Right,
                Padding = dim(0, 4),
                Name = "list",
                SortOrder = Enum.SortOrder.LayoutOrder
            })
            
            local UIStroke = library:create("UIStroke", {
                Parent = TextLabel,
                Name = ""
            })

            function cfg.change_text(text) 
                TextLabel.Text = text 
            end 

            self.previous_holder = left_components
            self.right_holder = right_components
            
            return self, cfg       
        end 

        function library:playerlist(options) 
            local cfg = {
                players = {}, 
                callback = options.callback or function() end, 
                selected_player, 

                labels = {
                    name,
                    display, 
                    uid, 
                }
            }

            local patterns = {
                ["Priority"] = Color3.fromRGB(255, 255, 0),
                ["Enemy"] = Color3.fromRGB(255, 0, 0),
                ["Neutral"] = themes.preset.text,
                ["Friendly"] = Color3.fromRGB(0, 255, 255)
            }

            -- elements 
                local playerlist_holder = library:create("TextLabel", {
                    Parent = self.holder,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "",
                    ZIndex = 2,
                    Size = dim2(1, -8, 0, 12),
                    BorderSizePixel = 0,
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextYAlignment = Enum.TextYAlignment.Top,
                    TextSize = 11,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local UIPadding = library:create("UIPadding", {
                    Parent = playerlist_holder,
                    Name = "",
                    PaddingBottom = dim(0, -2),
                    PaddingLeft = dim(0, 1)
                })
                
                local UIStroke = library:create("UIStroke", {
                    Parent = playerlist_holder,
                    Name = ""
                })
                
                local bottom_components = library:create("Frame", {
                    Parent = playerlist_holder,
                    Name = "",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 26, 0, 0),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                library:create("UIListLayout", {
                    Parent = bottom_components,
                    Name = "",
                    Padding = dim(0, 10),
                    SortOrder = Enum.SortOrder.LayoutOrder
                })
                
                local list = library:create("Frame", {
                    Parent = bottom_components,
                    Name = "",
                    Position = dim2(0, 0, 0, 2),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -27, 1, 232),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(list, "outline", "BackgroundColor3") 
                
                local inline = library:create("Frame", {
                    Parent = list,
                    Name = "",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(60, 55, 75)
                })library:apply_theme(inline, "inline", "BackgroundColor3") 
                
                local background = library:create("Frame", {
                    Parent = inline,
                    Name = "",
                    Position = dim2(0, 1, 0, 1),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, -2, 1, -2),
                    BorderSizePixel = 0,
                    BackgroundColor3 = themes.preset.accent
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = background,
                    Name = "",
                    Rotation = 90,
                    Color = rgbseq{
                    rgbkey(0, rgb(255, 255, 255)),
                    rgbkey(1, rgb(167, 167, 167))
                }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                local contrast = library:create("Frame", {
                    Parent = background,
                    Name = "",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 1, 0),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local UIGradient = library:create("UIGradient", {
                    Parent = contrast,
                    Name = "",
                    Rotation = 90,
                    Color = rgbseq{
                    rgbkey(0, rgb(41, 41, 55)),
                    rgbkey(1, rgb(35, 35, 47))
                }
                })library:apply_theme(UIGradient, "contrast", "Color") 
                
                local ScrollingFrame = library:create("ScrollingFrame", {
                    Parent = contrast,
                    Name = "",
                    ScrollBarImageColor3 = themes.preset.accent,
                    Active = true,
                    AutomaticCanvasSize = Enum.AutomaticSize.Y,
                    ScrollBarThickness = 3,
                    BackgroundTransparency = 1,
                    Size = dim2(1, 0, 1, 0),
                    BackgroundColor3 = rgb(255, 255, 255),
                    BorderColor3 = rgb(0, 0, 0),
                    BorderSizePixel = 0,
                    CanvasSize = dim2(0, 0, 0, 0)
                })library:apply_theme(ScrollingFrame, "accent", "ScrollBarImageColor3") 
                
                local UIPadding = library:create("UIPadding", {
                    Parent = ScrollingFrame,
                    Name = "",
                    PaddingTop = dim(0, 4),
                    PaddingBottom = dim(0, 4),
                    PaddingRight = dim(0, 4),
                    PaddingLeft = dim(0, 4)
                })
                
                local UIListLayout = library:create("UIListLayout", {
                    Parent = ScrollingFrame,
                    Name = "",
                    Padding = dim(0, 4),
                    SortOrder = Enum.SortOrder.LayoutOrder
                })
            -- 

            function cfg.create_player(player) 
                cfg.players[tostring(player)] = {}
                local path = cfg.players[tostring(player)]
                
                local TextButton = library:create("TextButton", {
                    Parent = ScrollingFrame,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "",
                    BackgroundTransparency = 1,
                    Size = dim2(1, 0, 0, 0),
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })

                path.instance = TextButton

                local player_name = library:create("TextLabel", {
                    Parent = TextButton,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = tostring(player),
                    BorderSizePixel = 0,
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    TextTruncate = Enum.TextTruncate.AtEnd,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                library:apply_theme(player_name, "text", "TextColor3") 
                library:apply_theme(player_name, "accent", "TextColor3") 
                                
                local TextLabel = library:create("TextLabel", {
                    Parent = TextButton,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = tostring(players[tostring(player)].Team) or "None",
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                                
                local Frame = library:create("Frame", {
                    Parent = TextLabel,
                    Name = "",
                    Position = dim2(0, -10, 0, 0),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 1, 0, 12),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(main_holder, "outline", "BackgroundColor3") 
                
                local priority_text = library:create("TextLabel", {
                    Parent = TextButton,
                    Name = "",
                    FontFace = library.font,
                    TextColor3 = rgb(180, 180, 180),
                    BorderColor3 = rgb(0, 0, 0),
                    Text = "Neutral",
                    BackgroundTransparency = 1,
                    TextXAlignment = Enum.TextXAlignment.Left,
                    BorderSizePixel = 0,
                    AutomaticSize = Enum.AutomaticSize.Y,
                    TextSize = 12,
                    BackgroundColor3 = rgb(255, 255, 255)
                })
                
                local Frame = library:create("Frame", {
                    Parent = priority_text,
                    Name = "",
                    Position = dim2(0, -10, 0, 0),
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(0, 1, 0, 12),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(main_holder, "outline", "BackgroundColor3") 
                
                local UIListLayout = library:create("UIListLayout", {
                    Parent = TextButton,
                    Name = "",
                    FillDirection = Enum.FillDirection.Horizontal,
                    HorizontalFlex = Enum.UIFlexAlignment.Fill,
                    SortOrder = Enum.SortOrder.LayoutOrder,
                    VerticalFlex = Enum.UIFlexAlignment.Fill
                })
                
                local UIPadding = library:create("UIPadding", {
                    Parent = TextButton,
                    Name = "",
                    PaddingRight = dim(0, 2),
                    PaddingLeft = dim(0, 2)
                })

                local line = library:create("Frame", {
                    Parent = ScrollingFrame,
                    Name = "",
                    BorderColor3 = rgb(0, 0, 0),
                    Size = dim2(1, 0, 0, 1),
                    BorderSizePixel = 0,
                    BackgroundColor3 = rgb(32, 32, 38)
                })library:apply_theme(main_holder, "outline", "BackgroundColor3") 

                path.line = line

                function path.set_priority(text) 
                    priority_text.Text = text
                    priority_text.TextColor3 = patterns[text]
                end 

                TextButton.MouseButton1Click:Connect(function()
                    if cfg.selected_player then 
                        cfg.selected_player.TextColor3 = themes.preset.text 
                    end     

                    cfg.selected_player = player_name 
                    player_name.TextColor3 = themes.preset.accent 

                    cfg.callback(players[tostring(player)]) 

                    if cfg.labels.name then 
                        cfg.labels.name.change_text("User: " .. tostring(player))
                        cfg.labels.display.change_text("DisplayName: " .. players[tostring(player)].DisplayName)
                        cfg.labels.uid.change_text("User Id: " .. players[tostring(player)].UserId)
                    end     
                end)

                return path 
            end 

            function cfg.remove_player(player) 
                cfg.players[tostring(player)].instance:Destroy() 
                cfg.players[tostring(player)].line:Destroy() 
                cfg.players[tostring(player)] = nil 
            end 

            players.PlayerAdded:Connect(cfg.create_player)
            players.PlayerRemoving:Connect(cfg.remove_player)
            
            for _, player in next, players:GetPlayers() do 
                local player_object = cfg.create_player(player.Name)
                table.insert(cfg.players, player_object)
            end 

            _, cfg.labels.name = self:label({name = "Name: ??"})
            _, cfg.labels.display = self:label({name = "Display Name: ??"})
            _, cfg.labels.uid = self:label({name = "User Id: ??"})

            return self
        end 
    -- 
-- 



-- documentation 
    local window = library:window({name = os.date('nigger - %b %d %Y'), size = dim2(0, 614, 0, 639)})
    local watermark = library:watermark({default = os.date('nigger - %b %d %Y - %H:%M:%S')})  

    local Aiming = window:tab({name = "Aiming"})
    local Misc = window:tab({name = "Misc"})
    local Settings = window:tab({name = "Settings"})

    -- Aiming
        local column =  Aiming:column() 
            column:section({name = "Target Selection"})
                :toggle({name = "Toggle With Keybind", flag = "e", callback = function(bool)
                    print("Toggle With Keybind On/Off") -- im not making shit for this
                end})
                :keybind({name = "Aiming", flag = "e", callback = function(bool)
                    bounding_box_fill.CFrame = cfr(9e9, 9e9, 9e9) -- keybind
                end})
                :toggle({name = "Normal Toggle", flag = "e", callback = function(bool)
                    print("Tggole On/Off")
                end})
                :slider({name = "Slide", min = 0, max = 1000, default = 40, interval = 1, suffix = "ms", flag = "e", callback = function(bool)
                    print("This Is A Slide")
                end})
                :dropdown({name = "Multi Dropdown", flag = "e", items = {"Juju", "Temple", "Eagle", "Unnamed"}, multi = true
                })
                :toggle({name = "Toggle With ColorPicker", flag = "e"})
                :colorpicker({name = "Color Picker", flag = "e", color = Color3.fromHex("#000000"), callback = function(color, alpha) 
                end})
                :dropdown({name = "Dropdown", flag = "e", items = {"Hentai", "Porn", "Milfs"}, multi = false, callback = function(option) 
                    print("Real")
                end})
                :textbox({name = "TextBox", flag = "e"})          
        local column = Aiming:column()
            column:section({name = "Section"})  
                :toggle({name = "Toggle", flag = "e"})
            column:section({name = "Section"})  
                :toggle({name = "Toggle", flag = "e"})
    -- 

    -- Misc
        local column = Misc:column()
            column:section({name = "Combat"})
                :toggle({name = "Speed", flag = "cframe_speed"})
                :keybind({name = "Speed", flag = "cframe_speed_bind", mode = "toggle"})
                :slider({name = "Amount", flag = 'speed_increments', min = 0, max = 100, default = 10, interval = 1})
                :toggle({name = "Auto Jump", flag = "auto_jump"})
                :toggle({name = "Fly", flag = "fly"})
                :keybind({name = "Fly", flag = "fly_bind", mode = "toggle"})
                :slider({name = "Amount", flag = 'fly_increments', min = 0, max = 100, default = 20, interval = 1})
                :toggle({name = "No Slow", flag = "no_slow"})
                :toggle({name = "No Jump Cooldown", flag = "no_jump_cooldown", callback = function(bool)
                    lp.Character.Humanoid.UseJumpPower = not bool
                end})
                :toggle({name = "Auto Reload", flag = "auto_reload"})
                :toggle({name = "Auto Shoot", flag = "auto_shoot", callback = function(bool)
                end})
                :slider({name = "Delay", flag = 'auto_shoot_delay', min = 0, max = 1000, default = 50, interval = 1, suffix = "ms"})
                :toggle({name = "Auto Armor", flag = "auto_armor"})
                :toggle({name = "Anti Void Kill", flag = "anti_void_kill", callback = function(bool)
                    ws.FallenPartsDestroyHeight = bool and -50000 or -500
                end})
                -- :toggle({name = "Anti Stomp", flag = "anti_stomp"})
                :toggle({name = "Infinite Zoom", flag = "infinite_zoom", callback = function(bool)
                    lp.CameraMaxZoomDistance = bool and 9e9 or 30
                end})
    --

            -- Settings
        local column = Settings:column()
            local section = column:section({name = "Options"})
                local old_config = library:get_config()
                _, config_holder = section:list({flag = "config_name_list"})
                section:textbox({flag = "config_name_text_box"})
                section:button_holder({})
                section:button({name = "Create", callback = function()
                    writefile(library.directory .. "/configs/" .. flags["config_name_text_box"] .. ".cfg", library:get_config())
                    library:config_list_update()
                end})
                section:button({name = "Delete", callback = function()
                    delfile(library.directory .. "/configs/" .. flags["config_name_list"] .. ".cfg")
                    library:config_list_update()
                end})
                section:button_holder({})
                section:button({name = "Load", callback = function()
                    library:load_config(readfile(library.directory .. "/configs/" .. flags["config_name_list"] .. ".cfg"))
                end})
                section:button({name = "Save", callback = function()
                    writefile(library.directory .. "/configs/" .. flags["config_name_text_box"] .. ".cfg", library:get_config())
                    library:config_list_update()
                end})
                section:button_holder({})
                section:button({name = "Unload Config", callback = function()
                    library:load_config(old_config)
                end})
                section:button({name = "Unload Menu", callback = function()
                    for _, gui in next, library.guis do 
                        gui:Destroy() 
                    end 

                    for _, connection in next, library.connections do 
                        connection:Disconnect() 
                    end     

                    for _, instance in next, milkyboy.instances do 
                        instance:Destroy() 
                    end 

                    for _, drawing in next, milkyboy.drawings do 
                        drawing:Remove()
                    end 
                end})
        local column = Settings:column()
            column:section({name = "Theme"})
                :label({name = "Accent"})
                :colorpicker({name = "Accent", color = themes.preset.accent, flag = "accent", callback = function(color, alpha)
                    library:update_theme("accent", color)
                end})
                :label({name = "Contrast"})
                :colorpicker({name = "Low", color = themes.preset.low_contrast, flag = "low_contrast", callback = function(color)
                    if (flags["high_contrast"] and flags["low_contrast"]) then 
                        library:update_theme("contrast", rgbseq{
                            rgbkey(0, flags["low_contrast"].Color),
                            rgbkey(1, flags["high_contrast"].Color)
                        })
                    end 
                end})
                :colorpicker({name = "High", color = themes.preset.high_contrast, flag = "high_contrast", callback = function(color)
                    library:update_theme("contrast", rgbseq{
                        rgbkey(0, flags["low_contrast"].Color),
                        rgbkey(1, flags["high_contrast"].Color)
                    })
                end})
                :label({name = "Inline"})
                :colorpicker({name = "Inline", color = themes.preset.inline, callback = function(color, alpha)
                    library:update_theme("inline", color)
                end})
                :label({name = "Outline"})
                :colorpicker({name = "Outline", color = themes.preset.outline, callback = function(color, alpha)
                    library:update_theme("outline", color)
                end})
                :label({name = "Text Color"})
                :colorpicker({name = "Main", color = themes.preset.text, callback = function(color, alpha)
                    library:update_theme("text", color)
                end})
                :colorpicker({name = "Outline", color = themes.preset.text_outline, callback = function(color, alpha)
                    library:update_theme("text_outline", color)
                end})
                :label({name = "Glow"})
                :colorpicker({name = "Glow", color = themes.preset.glow, callback = function(color, alpha)
                    library:update_theme("glow", color)
                end})
                :label({name = "UI Bind"})
                :keybind({callback = function(bool)
                    library.frame.Enabled = bool
                end})
                :toggle({name = "Keybind List", flag = "keybind_list", callback = function(bool)
                    library.keybind_list_frame.Visible = bool
                end})
                
    -- 


library:config_list_update()
