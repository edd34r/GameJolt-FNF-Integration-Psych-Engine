# GameJolt FNF Integration Psych Engine

## This project is designed to be used with **Friday Night Funkin** in **Psych Engine**. This allows you to add trohies in your GameJolt gamepage and award them to the user along with adding scores to leaderboard tables!

### This Can be used for android too because it support multiple platforms

### Included in the repo is the API to add trophies and the state (GameJoltLogin) to sign the user in.

# SETUP (GAMEJOLT):

Make sure to add `import gamejolt.*;` at the top of `main.hx`!

To add your game's keys, you will need to make a file in the source folder named GameJoltKeys.hx (filepath: ../source/gamejolt/GameJoltKeys.hx).
<br>
In this file, you will need to add the GameJoltKeys class with two public static variables, `id:Int` and `key:String`.

### `source/gamejolt/GameJoltKeys.hx` example:
```hx
package gamejolt;

class GameJoltKeys
{
    public static var id:Int = 	0; // Put your game's ID here
    public static var key:String = ""; // Put your game's private API key here
}
```
### **DO NOT SHARE YOUR GAME'S API KEY! You can add `source/gamejolt/GameJoltKeys.hx` to a `.gitignore` file to make sure no one grabs the key! If someone gets it, they can send false data!**

### You can find your game's API key and ID code within the game page's settngs under the game API tab.

# SETUP (In Game Saves):

Make sure to add `import gamejolt.GameJoltAPI;` at the top of `ClientPrefs.hx`!

Then in this class at the function `loadPrefs` add

```hx
        //GameJolt Things
        if(FlxG.save.data.gjUser != null)
        {
            FlxG.save.data.gjUser = FlxG.save.data.gjUser;
        }

        if (FlxG.save.data.gjToken != null)
        {
            FlxG.save.data.gjToken = FlxG.save.data.gjToken;
        }

        if (FlxG.save.data.lbToggle == null)
        {
            FlxG.save.data.lbToggle = false;
            FlxG.save.flush();
        }

        if (FlxG.save.data.lbToggle != null)
        {
            GameJoltAPI.leaderboardToggle = FlxG.save.data.lbToggle;
        }
```

This is for making the game to save the GameJolt user, Token and if you enable the leaderboardToggle in GameJoltLogin state in game

# SETUP (TOASTS):

## **Thank you Firubii for the code for this! Please go check them out!**
**https://twitter.com/firubiii**
**https://github.com/firubii**

To setup toasts, you will need to do a few things.

Inside the Main class (Main.hx), you need to make a new variable called toastManager.

`Main.hx`
```haxe
public static var gjToastManager:GameJoltToastManager;
```

Inside the setupGame function in the Main class, you will need to create the toastManager.
```haxe
gjToastManager = new GameJoltToastManager();
addChild(gjToastManager);
```

TYSM Firubii for your help!

# USAGE:

## Make sure to put `import gamejolt.GameJoltAPI;` at the top of the file if you want to call a command!

```hx
import gamejolt.GameJoltAPI;
```

## These commands **must** be ran before starting the API. Place these in `TitleState.hx` at function create:

put those after ClientPrefs.loadPrefs(); function to make sure the game load the game saves
```hx
GameJoltAPI.connect();
GameJoltAPI.authDaUser(FlxG.save.data.gjUser, FlxG.save.data.gjToken);
```

### Username and Token are grabbed from the default `FlxG.save` file. This file can be changed in `TitleState.hx`.

### Exiting the login menu will throw you back to Main Menu State. You can change this in the GameJoltLogin class inside gamejolt folder.

### The session will automatically start on login and will be pinged every 30 seconds. If it isn't pinged within 120 seconds, the session automatically ends from GameJolt's side.

### You can open the login state by calling the GameJoltLogin state:
```hx
MusicBeatState.switchState(new GameJoltLogin());
```

# CHANGABLE VARIABLES:

`GameJoltInfo.font:String`
> The font used in GameJoltLogin

`GameJoltInfo.fontPath:String`
> The font path used for the notifications

`GameJoltInfo.imagePath:String`
> The file path for the image in the notifications.

# COMMANDS AVAILABLE:

`GameJoltAPI.getStatus():Bool`
> Checking to see if the user has signed in. Returns a `bool` value. `true` if signed in, `false` if not signed in.

`GameJoltAPI.getuserInfo(username):String`
> Grabs the username and usertoken of the user and returns a `String`.<br>
> `username:Bool = true` -> `true` to grab username, `false` to grab usertoken.

`GameJoltAPI.getTrophy(trophyID);`
> `TrophyID:Int` -> ID of the trophy you want the player to earn.

`GameJoltAPI.checkTrophy(trophyID);`
> Returns a `bool` value of the achieved status. `True` for achieved, `false` for not achieved.<br>
> `TrophyID:Int` -> ID of the trophy you want to check.

`GameJoltAPI.pullTrophy(trophyID);`
> Returns a `Map<String,String>` of the trophy called for.<br>
> `TrophyID:Int` -> ID of the trophy you want to pull.

`GameJoltAPI.addScore(score:Int, tableID:Int, ?extraData:String);`
> Adds a score to a table on GameJolt.<br>
> `score:Int` -> The score to add. Will also count as the sorting value.<br>
> `tableID:Int` -> ID of the table.<br>
> `extraData:String` -> Exta data you want to add. Could be accuracy, who knows.

`GameJoltAPI.pullHighScore(tableID:Int)`
> Pulls the data from the highest score on the table. Will return a `Map<String,String>` value.<br>
> `tableID:Int` -> ID of the table.<br>
> Values returned -> `score, sort, user_id, user, extra_data, stored, guest, success`

# CREDITS:

- <a href = "https://github.com/jigsaw-4277821">Saw (M.A. Jigsaw) me! </a> - Redoing the code + adding multiple platforms support
- <a href = "https://github.com/TentaRJ">TentaRJ</a> - Original repo and codes

TentaRJ Credits
- <a href = "https://github.com/brightfyregit">BrightFyre</a> - Testing and UI design
- <a href ="https://github.com/haya3218">Haya</a> - Systools fork
- <a href = "https://github.com/firubii">Firubii</a> - Toast system
