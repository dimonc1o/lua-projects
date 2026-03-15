SWEP.PrintName = "Bow"
SWEP.Author = "dimoncio"
SWEP.Instructions = "ЛКМ для выстрела. 0.8 сек для полного заряда снаряда"
SWEP.Category = "Custom Weapons"

SWEP.Spawnable = true
SWEP.AdminOnly = false

SWEP.Primary.ClipSize = -1
SWEP.Primary.DefaultClip = -1
SWEP.Primary.Automatic = true 
SWEP.Primary.Ammo = "none"

SWEP.Secondary.ClipSize = -1
SWEP.Secondary.DefaultClip = -1
SWEP.Secondary.Automatic = false
SWEP.Secondary.Ammo = "none"

SWEP.ViewModel = "models/weapons/c_crossbow.mdl"
SWEP.WorldModel = "models/weapons/w_crossbow.mdl"
SWEP.UseHands = true

function SWEP:Initialize()
    self:SetHoldType("crossbow")
    self.IsCharging = false
    self.StartChargeTime = 0
    self.FullPowerReached = false
end

function SWEP:PrimaryAttack()
    self:SetNextPrimaryFire(CurTime() + 0.1)

    if not self.IsCharging then
        self.IsCharging = true
        self.FullPowerReached = false
        self.StartChargeTime = CurTime()
        self:EmitSound("weapons/crossbow/pull1.wav", 65, 120)
    end
end

function SWEP:Think()
    if not IsValid(self.Owner) then return end

    if self.IsCharging then
        local timePassed = CurTime() - self.StartChargeTime

        if timePassed >= 0.8 and not self.FullPowerReached then
            self.FullPowerReached = true
            self:EmitSound("buttons/lightswitch2.wav", 60, 160)
        end

        if self.FullPowerReached and CLIENT then
            local shake = math.sin(CurTime() * 60) * 0.15
            local p = self.Owner:EyeAngles()
            self.Owner:SetEyeAngles(p + Angle(shake, shake, 0))
        end

        if not self.Owner:KeyDown(IN_ATTACK) then
            if timePassed >= 0.1 then
                self:ShootArrow(self.FullPowerReached)
            end
            
            self.IsCharging = false
            self.FullPowerReached = false
            self:SetNextPrimaryFire(CurTime() + 0.5)
        end
    end
end

function SWEP:ShootArrow(isPowered)
    if CLIENT then return end 

    local owner = self.Owner
    local arrow = ents.Create("crossbow_bolt") 
    if not IsValid(arrow) then return end

    local forward = owner:GetAimVector()
    arrow:SetPos(owner:GetShootPos() + forward * 32)
    arrow:SetAngles(owner:EyeAngles())
    arrow:SetOwner(owner)
    arrow:Spawn()

    local speed = isPowered and 4000 or 2000
    arrow:SetVelocity(forward * speed)

    self:EmitSound("weapons/crossbow/fire1.wav", 80, isPowered and 100 or 140)
    owner:ViewPunch(Angle(isPowered and -4 or -1, 0, 0))
end
