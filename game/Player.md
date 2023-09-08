# Create
```
depth = 0
direction = 0
soundJump = soundJump1;
grav = 0.35
jumpspeed = 12
movespeed = 7
vsp = 15
hsp = 0
```
# Step
```
Keyup = keyboard_check(vk_up)
Keyreleaseup = keyboard_check_released(vk_up)
Keypressedup = keyboard_check_pressed(vk_up)
Keyleft = keyboard_check(vk_left)
Keyright = keyboard_check(vk_right)
scr_Camera();
scr_Parallax();
{
    var move = ((-Keyleft) + Keyright)
    if Keyleft
        direction = 180
    if Keyright
        direction = 0
    if (move != 0)
        hsp = (move * movespeed)
    else
        hsp *= 0.82
    if (vsp < 15)
        vsp += grav
    if place_meeting(x, (y + 1), obj_Block)
        vsp = (Keyup * (-jumpspeed))	
    if (Keyup && place_meeting(x, (y + 1), obj_Block))
        audio_play_sound(soundJump, 1, false)
		
	if place_meeting(x, (y + 1), obj_WallTrap)
       hsp = 50;
	if place_meeting(x, (y + 1), obj_WallTrap2)
       hsp = -50;
	if place_meeting(x, (y + 1), obj_WallTrap3)
       hsp = -50;
		
    if Keyreleaseup
    {
        if (vsp < 0)
            vsp /= 3
    }
    if place_meeting((x + hsp), y, obj_Block)
    {
        while (!(place_meeting((x + sign(hsp)), y, obj_Block)))
            x += sign(hsp)
        hsp = 0
    }
    x += hsp
    if place_meeting(x, (y + vsp), obj_Block)
    {
        while (!(place_meeting(x, (y + sign(vsp)), obj_Block)))
            y += sign(vsp)
        vsp = 0
    }
    y += vsp
	
	if Keyright {

       image_xscale = 1;
	}
	else if Keyleft {

       image_xscale = -1;
	}
	else sprite_index = spr_CattoStand

	if (hsp <= -1)
	{
		sprite_index = spr_CattoRun
        image_speed = (hsp / -8)
	}
	else if (hsp >= 1)
	{
		sprite_index = spr_CattoRun
        image_speed = (hsp / 8)
	}
	else if (hsp <= 0.1 || hsp >= -0.1)
        {
            if (direction == 0)
            {
                sprite_index = spr_CattoStand
                image_xscale = 1
                image_speed = 1
            }
            else if (direction == 180)
            {
                sprite_index = spr_CattoStand
                image_xscale = -1
                image_speed = -1
            }
        }
	if Keyup
    {
		sprite_index = spr_CattoJump2
    }
	if (vsp >= 0.05)
	{
		sprite_index = spr_CattoJump2
	}
	else if (vsp <= -0.05)
	{
		sprite_index = spr_CattoJump2
	}
}
```
# Camera
```
function scr_Camera() //gml_Script_scr_Camera
{
    instance_deactivate_object(obj_Block)
    var _vx = camera_get_view_x(view_camera[0])
    var _vy = camera_get_view_y(view_camera[0])
    var _vw = camera_get_view_width(view_camera[0])
    var _vh = camera_get_view_height(view_camera[0])
    instance_activate_region((_vx - 64), (_vy - 64), (_vw + 128), (_vh + 128), true)
    camx = camera_get_view_x(view_camera[0])
    camy = camera_get_view_y(view_camera[0])
    camh = camera_get_view_height(view_camera[0])
    camw = camera_get_view_width(view_camera[0])
    camx += (((x - (camw / 2)) - camx) * 0.1)
    camy += (((y - (camh / 2)) - camy) * 0.1)
    camera_set_view_pos(view_camera[0], camx, camy)
}
```
# Parallax
```
function scr_Parallax() //gml_Script_scr_Parallax
{
    sky = "sky"
    water = "water"
    water_1 = "water_1"
    background_1 = "backgrounds_1"
    background_2 = "backgrounds_2"
    background_3 = "backgrounds_3"
    background_4 = "backgrounds_4"
    background_5 = "backgrounds_5"
    layer_x(sky, (camera_get_view_x(view_camera[0]) * 0.95))
    layer_x(background_1, (camera_get_view_x(view_camera[0]) * 0.8))
    layer_x(background_2, (camera_get_view_x(view_camera[0]) * 0.75))
    layer_x(background_3, (camera_get_view_x(view_camera[0]) * 0.7))
    layer_x(background_4, (camera_get_view_x(view_camera[0]) * 0.65))
    layer_x(background_5, (camera_get_view_x(view_camera[0]) * 0.6))
    layer_x(water, (camera_get_view_x(view_camera[0]) * 0.55))
    layer_x(water_1, (camera_get_view_x(view_camera[0]) * 0.55))
    layer_y(sky, (camera_get_view_y(view_camera[0]) * 0.95))
    layer_y(background_1, ((camera_get_view_y(view_camera[0]) * 0.8) + 150))
    layer_y(background_2, ((camera_get_view_y(view_camera[0]) * 0.75) + 250))
    layer_y(background_3, ((camera_get_view_y(view_camera[0]) * 0.7) + 350))
    layer_y(background_4, ((camera_get_view_y(view_camera[0]) * 0.65) + 450))
    layer_y(background_5, ((camera_get_view_y(view_camera[0]) * 0.6) + 550))
    layer_y(water, ((camera_get_view_y(view_camera[0]) * 0.55) + 1050))
    layer_y(water_1, ((camera_get_view_y(view_camera[0]) * 0.55) + 1450))
}
```