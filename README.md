    local Players=game:   
                                                                        GetService("Players");local LocalPlayer=Players 
                                                                    .LocalPlayer;local Camera=workspace.CurrentCamera;local       
                                                                RunService=game:GetService("RunService");local UserInputService=game:   
                                                            GetService("UserInputService");local StarterGui=game:GetService("StarterGui") 
                                                          ;_G.AimbotEnabled=false;_G.TeamCheck=true;_G.AimPart="Head";_G.Sensitivity=0.1;_G 
                                                        .FOVRadius=50;_G.FREE_FOR_ALL=false;local fovCircle=Drawing.new("Circle");fovCircle.  
                                                      Thickness=1;fovCircle.NumSides=64;fovCircle.Radius=_G.FOVRadius;fovCircle.Filled=false;   
                                                    fovCircle.Visible=false;fovCircle.Color=Color3.fromRGB(255,255,255);fovCircle.Transparency=   
                                                  0.7;local function createESP(player) if (player==LocalPlayer) then return;end if  not player.     
                                                  Character then return;end local box=Drawing.new("Square");box.Thickness=2;box.Filled=false;box.     
                                                Color=Color3.fromRGB(255,0,0);box.Transparency=1;local connection;connection=RunService.RenderStepped:  
                                                Connect(function() if ( not player.Character or  not player.Character:FindFirstChild("HumanoidRootPart")) 
                                               then box.Visible=false;return;end local rootPart=player.Character.HumanoidRootPart;local rootPos,onScreen=   
                                              Camera:WorldToViewportPoint(rootPart.Position);if onScreen then local head=player.Character:FindFirstChild(   
                                            "Head");local headPos=(head and Camera:WorldToViewportPoint(head.Position + Vector3.new(0,1,0) )) or rootPos ;    
                                            local legsPos=Camera:WorldToViewportPoint(rootPart.Position-Vector3.new(0,3,0) );local height=math.abs(headPos.Y-   
                                          legsPos.Y );local width=height * 0.5 ;box.Size=Vector2.new(width,height);box.Position=Vector2.new(rootPos.X-(width/2) , 
                                          rootPos.Y-(height/2) );box.Visible=true;if (_G.FREE_FOR_ALL or (player.TeamColor~=LocalPlayer.TeamColor)) then box.Color= 
                                          Color3.fromRGB(255,0,0);else box.Color=Color3.fromRGB(0,170,255);end else box.Visible=false;end end);player.AncestryChanged 
                                          :Connect(function() box:Remove();connection:Disconnect();end);end for _,player in pairs(Players:GetPlayers()) do createESP( 
                                        player);end Players.PlayerAdded:Connect(function(player) player.CharacterAdded:Connect(function() createESP(player);end);end);  
                                        local function getClosestPlayerToCursor() local closestPlayer=nil;    --[[==============================]]local shortestDistance= 
                                        _G.FOVRadius;for _,player in pairs(Players:GetPlayers()) do --[[============================================]] if (player==       
                                        LocalPlayer) then continue;end if ( not player.         --[[======================================================]]Character or    
                                      not player.Character:FindFirstChild(_G.AimPart)) then --[[==========================================================]] continue;end if  
                                      (player.Character.Humanoid.Health<=0) then continue --[[==============================================================]];end if (_G.    
                                      TeamCheck and  not _G.FREE_FOR_ALL and (player.     --[[================================================================]]TeamColor==     
                                      LocalPlayer.TeamColor)) then continue;end local     --[[==================================================================]]part=player.  
                                      Character[_G.AimPart];local screenPos,onScreen=     --[[==================================================================]]Camera:           
                                    WorldToViewportPoint(part.Position);local mousePos=   --[[====================================================================]]              
                    UserInputService:GetMouseLocation();local distance=(Vector2.new(      --[[====================================================================]]screenPos.X,    
              screenPos.Y) -mousePos).Magnitude;if (onScreen and (distance<               --[[======================================================================]]              
            shortestDistance)) then closestPlayer=player;shortestDistance=distance;end    --[[======================================================================]]end return    
          closestPlayer;end RunService.RenderStepped:Connect(function() if _G.            --[[======================================================================]]AimbotEnabled 
         then local target=getClosestPlayerToCursor();if (target and target.Character and --[[======================================================================]] target.      
        Character:FindFirstChild(_G.AimPart)) then local headPos=target.Character[_G.     --[[======================================================================]]AimPart].     
      Position;Camera.CFrame=Camera.CFrame:Lerp(CFrame.new(Camera.CFrame.Position,headPos --[[======================================================================]]),_G.         
      Sensitivity);end end fovCircle.Position=UserInputService:GetMouseLocation();fovCircle --[[==================================================================]].Radius=_G.     
      FOVRadius;end);local screenGui=Instance.new("ScreenGui",game.CoreGui);screenGui.Name= --[[================================================================]]"AimbotMenu";     
    local frame=Instance.new("Frame");frame.Size=UDim2.new(0,220,0,260);frame.Position=     --[[==============================================================]]UDim2.new(0.5, -  
    110,0.5, -130);frame.BackgroundColor3=Color3.fromRGB(20,20,20);frame.BorderSizePixel=0;   --[[==========================================================]]frame.Active=true;  
    frame.Draggable=true;frame.Visible=false;frame.Parent=screenGui;local title=Instance.new(   --[[====================================================]]"TextLabel");title.Size 
    =UDim2.new(1,0,0,30);title.BackgroundColor3=Color3.fromRGB(40,40,40);title.Text=              --[[==============================================]]"Aimbot Universal";title. 
    TextColor3=Color3.new(1,1,1);title.Font=Enum.Font.SourceSansBold;title.TextSize=18;title.Parent=  --[[====================================]]frame;local toggleTeam=       
    Instance.new("TextButton");toggleTeam.Size=UDim2.new(1, -20,0,30);toggleTeam.Position=UDim2.new(0,10, --[[========================]]0,40);toggleTeam.Text=                
    "Team Check: ON";toggleTeam.TextColor3=Color3.new(1,1,1);toggleTeam.BackgroundColor3=Color3.fromRGB(60,60,60);toggleTeam.Font=Enum.Font.SourceSans;toggleTeam.TextSize= 
  16;toggleTeam.Parent=frame;toggleTeam.MouseButton1Click:Connect(function() _G.TeamCheck= not _G.TeamCheck;toggleTeam.Text="Team Check: "   .. ((_G.TeamCheck and "ON")  
  or "OFF") ;end);local togglePart=Instance.new("TextButton");togglePart.Size=UDim2.new(1, -20,0,30);togglePart.Position=UDim2.new(0,10,0,80);togglePart.Text=          
  "Parte Alvo: Head";togglePart.TextColor3=Color3.new(1,1,1);togglePart.BackgroundColor3=Color3.fromRGB(60,60,60);togglePart.Font=Enum.Font.SourceSans;togglePart.        
  TextSize=16;togglePart.Parent=frame;togglePart.MouseButton1Click:Connect(function() _G.AimPart=((_G.AimPart=="Head") and "HumanoidRootPart") or "Head" ;togglePart.Text 
  ="Parte Alvo: "   .. _G.AimPart ;end);local increaseFOV=Instance.new("TextButton");increaseFOV.Size=UDim2.new(0.48, -5,0,30);increaseFOV.Position=UDim2.new(0.02,5,1, - 
  105);increaseFOV.Text="FOV +";increaseFOV.TextColor3=Color3.fromRGB(255,255,255);increaseFOV.BackgroundColor3=Color3.fromRGB(40,120,40);increaseFOV.Font=Enum.Font.     
  SourceSans;increaseFOV.TextSize=16;increaseFOV.Parent=frame;increaseFOV.MouseButton1Click:Connect(function() _G.FOVRadius=_G.FOVRadius + 10 ;end);local decreaseFOV=    
  Instance.new("TextButton");decreaseFOV.Size=UDim2.new(0.48, -5,0,30);decreaseFOV.Position=UDim2.new(0.5,5,1, -105);decreaseFOV.Text="FOV -";decreaseFOV.TextColor3=     
  Color3.fromRGB(255,255,255);decreaseFOV.BackgroundColor3=Color3.fromRGB(120,40,40);decreaseFOV.Font=Enum.Font.SourceSans;decreaseFOV.TextSize=16;decreaseFOV.Parent=    
  frame;decreaseFOV.MouseButton1Click:Connect(function() _G.FOVRadius=math.max(10,_G.FOVRadius-10 );end);local discordButton=Instance.new("TextButton");discordButton.    
  Size=UDim2.new(1, -20,0,30);discordButton.Position=UDim2.new(0,10,1, -70);discordButton.Text="Discord";discordButton.TextColor3=Color3.fromRGB(255,255,255);            
  discordButton.BackgroundColor3=Color3.fromRGB(70,70,120);discordButton.Font=Enum.Font.SourceSansBold;discordButton.TextSize=16;discordButton.Parent=frame;discordButton.  
  MouseButton1Click:Connect(function() setclipboard("https://discord.gg/jfKVrrMx");StarterGui:SetCore("SendNotification",{Title="Discord",Text=                             
  "Link copiado para a área de transferência!",Duration=3});end);local credits=Instance.new("TextLabel");credits.Size=UDim2.new(1,0,0,20);credits.Position=UDim2.new(0,0,1, 
   -20);credits.BackgroundTransparency=1;credits.Text="Created By Pedrin031";credits.TextColor3=Color3.fromRGB(255,255,255);credits.Font=Enum.Font.SourceSansItalic;credits 
  .TextSize=14;credits.TextTransparency=0.3;credits.Parent=frame;UserInputService.InputBegan:Connect(function(input) if (input.KeyCode==Enum.KeyCode.E) then _G.            
  AimbotEnabled= not _G.AimbotEnabled;fovCircle.Visible=_G.AimbotEnabled;elseif (input.KeyCode==Enum.KeyCode.P) then frame.Visible= not frame.Visible;end end);
