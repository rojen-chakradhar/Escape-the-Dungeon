1. The Player: Create Event
In your obj_player, add these variables to the Create Event so the game knows how many lives you have.

Code snippet
hp = 3; // Starting health
max_hp = 3;
2. The Heart: Step Event
You only need one heart object (obj_heart). We will use its "Variable Definitions" or Create Event to give it a "Number" so it knows which slot it belongs to.

In the Step Event of obj_heart:

Code snippet
if (instance_exists(obj_player)) {
    // Offset based on which heart this is (1st, 2nd, or 3rd)
    // heart_number is a variable you set (0, 1, or 2)
    var _offset_x = (heart_number - 1) * 20; 
    
    x = obj_player.x + _offset_x;
    y = obj_player.y - 30; // Float above head
    
    // Make heart invisible if HP is too low
    if (obj_player.hp <= heart_number) {
        image_alpha = 0; 
    } else {
        image_alpha = 1;
    }
}
Note: In the room editor, double-click each heart instance and set heart_number to 0, 1, and 2 respectively.

3. Update your Death / Reset Logic
Replace your current DEATH / RESET section in the obj_player Step Event with this. This stops the room from restarting instantly and instead subtracts a heart.

Code snippet
// --------------------------------------
// DAMAGE & DEATH
// --------------------------------------

// If player falls or hits spikes
if (y > room_height + 100) || (place_meeting(x, y, oSpikes)) 
{
    hp -= 1; // Lose a heart
    
    if (hp > 0) {
        // Teleport back to start instead of restarting room
        x = xstart; 
        y = ystart;
    } else {
        // TRULY DEAD: Restart or Transition
        if (!instance_exists(obj_transition)) {
            instance_create_layer(0, 0, "Instances", obj_transition);
        }
        room_restart();
    }
}
4. Important: Sprite Origins
In GameMaker, for the hearts to look right:

Open your Heart Sprite.

Set the Origin to Middle Centre.

Open your Player Sprite.

Set the Origin to Bottom Centre.
