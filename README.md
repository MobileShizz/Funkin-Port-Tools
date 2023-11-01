### Funkin' Port Tools - 0.2.8

## WARNING: 

# This is for 0.2.8, go to the other branch for 0.2.7.1!

# ALSO AT THE TIME OF TYPING THIS ISN'T DONE YET 

## How to port fnf mods!! (that use 0.2.8)

* Step 1: download this repo and extract the .github, assets, and source folders into the mod's folder that ur porting. If it asks you to replace anything go ahead and do it.

* Step 2: In project.xml replace this line:
```
<window if="mobile" orientation="landscape" fullscreen="true" width="0" height="0" resizable="false"/>
```
with: 
```
<window if="mobile" orientation="landscape" fullscreen="true" width="1280" height="720" resizable="false"/>

```

Step 3: If the mod you're porting has discord rpc support make sure to comment it out or just remove it completely.

Step 4: In every **state** (NOT SUBSTATE) add: 
```haxe
		#if android
		addVirtualPad(FULL, A_B_E); // psych shit go brr
		#end
```         
before:
```haxe
super.create();
```

Step 5: In "GameOverSubstate.hx" after: 
```haxe
add(camFollow);
``` 
add: 
```haxe
		#if android
		addVirtualPad(NONE, A_B);
		addPadCamera();
		#end
```    

Step 6: In "PlayState.hx" before:

```haxe
		if (SONG == null)
			SONG = Song.loadFromJson('tutorial');
``` 
add:
```haxe
		#if android
		addAndroidControls();
		MusicBeatState.androidc.visible = true;
		MusicBeatState.androidc.alpha = 0.000001;
		
		#end
```
in:
```haxe
if(startTimer.finished){
```
add: 
```haxe
		    #if android
    		    MusicBeatState.androidc.visible = true;
    			if (MusicBeatState.checkHitbox != true) MusicBeatState.androidc.alpha = 1;
    		    #end
``` 
overwrite this: 

```haxe
if (controls.PAUSE
```

with: 
```haxe
if (controls.PAUSE #if android || FlxG.android.justReleased.BACK #end && startedCountdown && canPause)
```
Add:

```haxe
		#if mobileC
		mcontrols.visible = true;
		#end
```

In: 
```haxe
function endSong():Void
```
Add:

```haxe
        #if android
		MusicBeatState.androidc.alpha = 0.00001;
		#end
```
