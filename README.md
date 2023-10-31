### Funkin' Port Tools - 0.2.7.1

## WARNING: 

# This is for 0.2.7.1, go to the other branch for 0.2.8! (NOT EVEN STARTED YET LOL)

# ALSO AT THE TIME OF TYPING THIS ISN'T DONE YET 

## How to port fnf mods!!

* Step 1: download this repo and extract the .github, assets, and source folders into the mod's folder that ur porting. If it asks you to replace anything go ahead and do it.

* Step 2: In project.xml after this line:
```
<window if="mobile" orientation="landscape" fullscreen="true" width="0" height="0" resizable="false"/>
```
add: 
```
<define name="mobileC" if="mobile" />
<assets path="assets/shared/images/hitbox"/>
```

Step 3: If the mod you're porting has discord rpc support make sure to comment it out or just remove it completely.

Step 4: In every **state** (NOT SUBSTATE) add: 
```haxe
        #if mobileC 
        addVirtualPad(FULL, A_B); 
        #end
```         
before:
```haxe
super.create();
```

Step 5: In "GameOverSubstate.hx" after: 
```haxe
bf.playAnim('firstDeath');
``` 
add: 
```haxe
	#if mobileC 
    addVirtualPad(NONE, A_B); 
    var camcontrol = new FlxCamera(); 
    FlxG.cameras.add(camcontrol); 
    camcontrol.bgColor.alpha = 0; 
    _virtualpad.cameras = [camcontrol];	
    #end
```    

Step 6: In "PlayState.hx" in the lines where you import stuff add:

```haxe
import ui.Mobilecontrols;
``` 
after:
```haxe
private var camGame:FlxCamera;
```
add:
```haxe
#if mobileC var mcontrols:Mobilecontrols;#end
```
after: 
```haxe
doof.cameras = [camHUD];
``` 
add: 

```haxe
		#if mobileC
			mcontrols = new Mobilecontrols();
			switch (mcontrols.mode)
			{
				case VIRTUALPAD_RIGHT | VIRTUALPAD_LEFT | VIRTUALPAD_CUSTOM:
					controls.setVirtualPad(mcontrols._virtualPad, FULL, NONE);
				case HITBOX:
					controls.setHitBox(mcontrols._hitbox);
				default:
			}
			trackedinputs = controls.trackedinputs;
			controls.trackedinputs = [];

			var camcontrol = new FlxCamera();
			FlxG.cameras.add(camcontrol);
			camcontrol.bgColor.alpha = 0;
			mcontrols.cameras = [camcontrol];

			mcontrols.visible = false;

			add(mcontrols);
		#end
```

After: 
```haxe
function startCountdown():Void
```
Add:

```haxe
		#if mobileC
		mcontrols.visible = true;
		#end
```

After: 
```haxe
function endSong():Void
```
Add:

```haxe
		#if mobileC
		mcontrols.visible = false;
		#end
```
