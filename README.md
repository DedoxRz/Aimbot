lokale Camera = werkruimte. HuidigeCamera
lokale spelers = spel: GetService ( " Spelers " )
lokale RunService = spel: GetService ( " RunService " )
lokale UserInputService = spel: GetService ( " UserInputService " )
lokale TweenService = spel: GetService ( " TweenService " )
local LocalPlayer = Spelers. Lokale Speler
local Holding =  false

_G . AimbotEnabled  =  waar
_G . TeamCheck  =  false  -- Indien ingesteld op waar, zou het script je doel alleen op vijandige teamleden vergrendelen.
_G . AimPart  =  " Head "  -- Waar het aimbot-script op zou vergrendelen.
_G . Gevoeligheid  =  0  -- Hoeveel seconden duurt het voordat het aimbot-script officieel op het aimpart van het doelwit wordt vergrendeld.

_G . CircleSides  =  64  -- Hoeveel zijden zou de FOV-cirkel hebben.
_G . CircleColor  = Color3. fromRGB ( 255 , 255 , 255 ) -- (RGB) Kleur zoals de FOV-cirkel eruit zou zien.
_G . CircleTransparency  =  0.7  -- Transparantie van de cirkel.
_G . CircleRadius  =  80  -- De straal van de cirkel / FOV.
_G . CircleFilled  =  false  -- Bepaalt of de cirkel al dan niet gevuld is.
_G . CircleVisible  =  true  -- Bepaalt of de cirkel al dan niet zichtbaar is.
_G . CircleThickness  =  0  -- De dikte van de cirkel.

lokale FOVCircle = Tekening. nieuw ( " Cirkel " )
FOVCirkel. Positie  = Vector2. nieuw (Camera. ViewportSize . X  /  2 , Camera. ViewportSize . Y  /  2 )
FOVCirkel. Straal  =  _G . Cirkelstraal
FOVCirkel. Gevuld  =  _G . Cirkel gevuld
FOVCirkel. Kleur  =  _G . CirkelKleur
FOVCirkel. Zichtbaar  =  _G . Cirkel Zichtbaar
FOVCirkel. Straal  =  _G . Cirkelstraal
FOVCirkel. Transparantie  =  _G . CirkelTransparantie
FOVCirkel. NumSides  =  _G . Cirkelzijden
FOVCirkel. Dikte  =  _G . CirkelDikte

lokale  functie  GetClosestPlayer ()
	local MaximumDistance =  _G . Cirkelstraal
	lokaal doel =  nul

	voor _, v in volgende, Spelers: GetPlayers () do
		if v. Naam  ~= LocalPlayer. Noem  dan
			als  _G . TeamCheck  ==  waar  dan
				if v. Team  ~= LocalPlayer. Team  dan
					if v. Karakter  ~=  nul  dan
						if v. Karakter : FindFirstChild ( " HumanoidRootPart " ) ~=  nul  dan
							if v. Karakter : FindFirstChild ( " Humanoid " ) ~=  nul  en v. Karakter : FindFirstChild ( " Humanoid " ). Gezondheid  ~=  0  dan
								local ScreenPoint = Camera: WorldToScreenPoint (v. Karakter : WaitForChild ( " HumanoidRootPart " , math.huge ). Positie )
								local VectorDistance = (Vector2. new (UserInputService: GetMouseLocation (). X , UserInputService: GetMouseLocation (). Y ) - Vector2. new (ScreenPoint. X , ScreenPoint. Y )). Grootte
								
								if VectorDistance < MaximumDistance dan
									Doel = v
								einde
							einde
						einde
					einde
				einde
			anders
				if v. Karakter  ~=  nul  dan
					if v. Karakter : FindFirstChild ( " HumanoidRootPart " ) ~=  nul  dan
						if v. Karakter : FindFirstChild ( " Humanoid " ) ~=  nul  en v. Karakter : FindFirstChild ( " Humanoid " ). Gezondheid  ~=  0  dan
							local ScreenPoint = Camera: WorldToScreenPoint (v. Karakter : WaitForChild ( " HumanoidRootPart " , math.huge ). Positie )
							local VectorDistance = (Vector2. new (UserInputService: GetMouseLocation (). X , UserInputService: GetMouseLocation (). Y ) - Vector2. new (ScreenPoint. X , ScreenPoint. Y )). Grootte
							
							if VectorDistance < MaximumDistance dan
								Doel = v
							einde
						einde
					einde
				einde
			einde
		einde
	einde

	retour doel
einde

Gebruikersinvoerservice. InputBegan : Connect ( functie ( Input )
    als Invoer. UserInputType  == Enum. Gebruikersinvoertype . Muisknop2  dan
        Vasthouden =  waar
    einde
einde )

Gebruikersinvoerservice. InputEnded : Connect ( functie ( Input )
    als Invoer. UserInputType  == Enum. Gebruikersinvoertype . Muisknop2  dan
        Vasthouden =  false
    einde
einde )

RunService. RenderStepped : Verbinden ( functie ()
    FOVCirkel. Positie  = Vector2. nieuw (UserInputService: GetMouseLocation (). X , UserInputService: GetMouseLocation (). Y )
    FOVCirkel. Straal  =  _G . Cirkelstraal
    FOVCirkel. Gevuld  =  _G . Cirkel gevuld
    FOVCirkel. Kleur  =  _G . CirkelKleur
    FOVCirkel. Zichtbaar  =  _G . Cirkel Zichtbaar
    FOVCirkel. Straal  =  _G . Cirkelstraal
    FOVCirkel. Transparantie  =  _G . CirkelTransparantie
    FOVCirkel. NumSides  =  _G . Cirkelzijden
    FOVCirkel. Dikte  =  _G . CirkelDikte

    if Holding ==  true  en  _G . AimbotEnabled  ==  waar  dan
        TweenService: Create (Camera, TweenInfo. new ( _G . Sensitivity , Enum. EasingStyle . Sine , Enum. EasingDirection . Out ), {CFrame = CFrame. new (Camera. CFrame . Position , GetClosestPlayer (). Character [ _G . AimPart ] . Positie )}): Afspelen ()
    einde
einde )

