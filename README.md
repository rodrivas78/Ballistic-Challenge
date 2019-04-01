

BALLISTIC CHALLENGE by Rodrigo Gonzales 


GAMEPLAY

  You have a "cannon" (actually a state-of-the-art photonic ray slingshot!) that shoots bouncy projectiles. Your goal is to hit the cannonball on the two opposing walls (to the right and left of the screen). Each time you achieve this you will advance a level and you will not lose the projectile.  
  Avoid hitting obstacles. If this occurs you will remain at the same level and you will 
lose a projectile. If you run out of ammunition and a collision occurs, it's "GAME OVER"!       

It will be victorious who can get through all 10 levels before the ammo ends. 

Note: If the ball has a low vertical velocity, it will be swallowed underground, which will also result in the loss of a projectile.
  Good luck!
  
  
CONTROLS:

Arrow keys: controls the angle and shooting power
Space bar: shoot

------------------------------------------------------------------------------

You can play instantly with the following online emulator: 
http://webmsx.org/?MACHINE=MSX1&DISK=https://sites.google.com/site/taivas01/msxspirit/Ballistic_Challenge.dsk

-----------------------------------------------------------------------------

SHORT DESCRIPTION OF THE PROGRAM

  
Line 10:  Starts the variables and sets the width of the sprites.
 
Line 20:  Dimension and load the matrices. In the DATA lines we have the coordinates of each barrier / obstacle (sprites2 and sprite3) for each phase, sequentially. Note: For each level are four values (sprite2 (x, y) / sprite3 (x, y)), which will be stored in the matrices.

Lines 30 and 40:  Scenario design, positioning of obstacles and active text elements (LEVEL and AMMO).
  Here also are defined: the "acceleration of gravity" (AY = 1), the number of levels (C = 10) and the victory screen of the game.
  
Line 50:  Implementing controls (default for keyboard)
 By pressing the space bar a shot is fired and the program goes to line 70
 Note:  To play with the joystick, change the values in parentheses of STICK (0) and STRIG (0) from 0 to 1.
 
Line 60:  Draw the cannon and show the angle and power that the projectile will fire.

Lines 70 and 80:  Here we have the shooting routine. Where is the control of touches on walls and obstacles (ON SPRITE GOSUB). It is also where the "physics engine" of the game is located (read more about it below**), with the calculations involved in updating the position and velocity of the projectile.
If there is a collision with an obstacle the program is directed to line 100.

Line 90: Creates the projectile trail and closes the firing routine loop (initiated by FOR on line 70).
Addendum: to make the projectile leave a trail (as seen in the screenshots), change this line to:
 90 PSET(PX+6,PY+2),4:NEXT     
 
Line 100: Explosion! Sound and routine of projectile collision with obstacles. Controls the end of the game, when G (number of projectiles is equal to zero). Print "GAME OVER" and ask if there will be any new action. If yes ("Y"), the program is restarted (directed to line 10). Otherwise, END (end of program).

-------------------------------------------------------------------------------------
PURPOSE OF SOME VARIABLES:

G:  Holds the number of projectiles.
B:  Stores the amount of touches on the walls. If it is equal to 2 there is a level advance.
C:  Holds the number of levels.
Q:  Does not allow the arrays to be resized when the game restarts. This would generate a "Redimensioned array" error.
X2 and Y2:  Retains (if there is a collision with obstacles), the positioning values of the cannon configured by the player in the previous shot.

--------------------------------------------------------------------------------------
**PHYSICS ENGINE

The game contains a (partial / simplified) physics engine. It consists of an integrator (of position and velocity) that calculates and updates the position of the projectile based on the velocity, acceleration (stored respectively in vectors VX, VY, AX and AY) and time (T).

 The integrator can be divided into two sections: one part calculates the position and the other updates the speed (taking into account the acceleration), both using a time interval, defined in T.
 
Position calculation:

Px = Px + Vx * T
Py = Py + Vy * T

Velocity calculation:

Vy = Vy + Ay * T
Vx = Vx + Ax * T

Description of the variables:

 Px = Position (X axis)
 Py = Position (Y axis)
 Vx = velocity vector on the x-axis.
 Vy = velocity vector on the y-axis.
Ax and Ay = Acceleration vectors.

The "force" that simulates gravity (a constant "negative" acceleration on the Y-axis) can be found in line 30:
  AY = 1. (Here the value set is positive because the origin of the Y coordinate (y = 0) in MSX is "inverted", ie at the top of the screen).
  
----------------------------------------------------------------------------------------------------

EXPERIENCES -  Turn the game into a ballistic simulator.

⦁ Ballistics on the Moon or Mars:
Change (in line 30) the AY value to something around 0.2 to 0.4 and see how the projectile now climbs like a rocket with minimal force application. You can also simulate a stronger gravity (or a heavier projectile) by increasing the value (to above the pre-set value: 1).

⦁ Fireball shot (anti-gravitational):
Change in line 30 the value from AY to AY = -.4.

⦁ Increasing the limiting power of the cannon:
To do this, increase (at line 50) the values that limit the velocity of the vectors at the moment of firing. As in this example (in bold):
50 D=STICK(0):IFVX>=20THENVX=19ELSEIFVX<=-1THENVX=0ELSEIFABS(VY)>=20THENVY=-19ELSEIFVY=0THENVY=-1ELSEIFD=1THENVY=VY-1ELSEIFD=5THENVY=VY+1ELSEIFD=3THENVX=VX+1ELSEIFD=7THENVX=VX-1ELSEIFSTRIG(0)THEN70

⦁ Slow Motion 
To leave the trajectory in slow motion, modify (in line 10) the value of the variable "T" for T = .5 or even T = .5.

Tip: If you want to make room on the screen remove obstacles by deleting the following commands on line 30: [PUTSPRITE2, (I (C), J (C)), 4.2] and [PUTSPRITE3, (K (C), M C)), 11.3]

FINAL CONSIDERATIONS

It is possible to use this physics engine to elaborate other games, as well as several other interesting experiences.
Example:

Try to implement a Lunar Lander by adding accelerations on the AX and AY vectors during the main loop (lines 70 through 90).

----------------------------------------------------------------------------------------------

