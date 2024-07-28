# WAE 纯英文

## Actions.ini

```ini
[SIAddMoney]
Name=Add Money
Description=Add money.
IDOverride=50000
P3Name=Money
P3Type=Number

[SITakeMoney]
Name=Take Money
Description=Take Money.
IDOverride=50001
P3Name=Money
P3Type=Number

[SIOperateHouseVariable_Add]
Name=Operate House Variable, Add
Description=Operate House Variable, Add.
IDOverride=50200
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Mult]
Name=Operate House Variable, Mult
Description=Operate House Variable, Mult.
IDOverride=50201
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Pow]
Name=Operate House Variable, Pow
Description=Operate House Variable, Pow.
IDOverride=50202
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Log]
Name=Operate House Variable, Log
Description=Operate House Variable, Log.
IDOverride=50203
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Mod]
Name=Operate House Variable, Mod
Description=Operate House Variable, Mod.
IDOverride=50204
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_EQ]
Name=Operate House Variable, Equal
Description=Operate House Variable, Equal.
IDOverride=50210
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_NE]
Name=Operate House Variable, NotEqual
Description=Operate House Variable, NotEqual.
IDOverride=50211
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_GT]
Name=Operate House Variable, Greater
Description=Operate House Variable, Greater.
IDOverride=50212
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_GE]
Name=Operate House Variable, GreaterEqual
Description=Operate House Variable, GreaterEqual.
IDOverride=50213
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_LT]
Name=Operate House Variable, Less
Description=Operate House Variable, Less.
IDOverride=50214
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_LE]
Name=Operate House Variable, LessEqual
Description=Operate House Variable, LessEqual.
IDOverride=50215
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Not]
Name=Operate House Variable, Not
Description=Operate House Variable, Not.
IDOverride=50220
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Abs]
Name=Operate House Variable, Abs
Description=Operate House Variable, Abs.
IDOverride=50221
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Ceil]
Name=Operate House Variable, Ceil
Description=Operate House Variable, Ceil.
IDOverride=50222
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Floor]
Name=Operate House Variable, Floor
Description=Operate House Variable, Floor.
IDOverride=50223
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Round]
Name=Operate House Variable, Round
Description=Operate House Variable, Round.
IDOverride=50224
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Set]
Name=Operate House Variable, Set
Description=Operate House Variable, Set.
IDOverride=50230
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Max]
Name=Operate House Variable, Max
Description=Operate House Variable, Max.
IDOverride=50231
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Min]
Name=Operate House Variable, Min
Description=Operate House Variable, Min.
IDOverride=50232
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Random]
Name=Operate House Variable, Random
Description=Operate House Variable, Random.
IDOverride=50233
P3Name=IDCode
P3Type=Number

[SIOperateHouseVariable_Delete]
Name=Operate House Variable, Delete
Description=Operate House Variable, Delete.
IDOverride=50234
P3Name=IDCode
P3Type=Number

[SICompleteCurrentLevel]
Name=Complete Current Level
Description=Complete current level.
IDOverride=50500
P3Name=LevelID
P3Type=Number

[SISpecificNextLevel]
Name=Specific Next Level
Description=Specific next level.
IDOverride=50501
P3Name=LevelID
P3Type=Number

[SIEnterPrepareLevel]
Name=Enter Prepare Level
Description=Enter prepare level.
IDOverride=50502
P3Name=LevelID
P3Type=Number
```



## Events.ini

```ini
[SIIfHouseOwnsOriginTechno]
Name=If House Owns Origin Techno
Description=If house owns specific type of techno, this will check techno origin type and ignore type conversion.
IDOverride=50000
P2Type=Number

[SIIfNotHouseOwnsOriginTechno]
Name=If Not House Owns Origin Techno
Description=If not house owns specific type of techno, this will check techno origin type and ignore type conversion.
IDOverride=50001
P2Type=Number

[SIIfHouseOwnsBuidlingAtWaypoint]
Name=If House Owns Buidling At Waypoint
Description=If house owns specific type of building at specific waypoint.
IDOverride=50010
P2Type=Number

[SIIfHouseOwnsBuidlingAtWaypoint]
Name=If House Owns Buidling And Not At Waypoint
Description=If house owns specific type of building and not at specific waypoint.
IDOverride=50011
P2Type=Number

[SICheckNumber_EQ]
Name=Check Number, Equal
Description=Check Number, Equal.
IDOverride=50400
P2Type=Number

[SICheckNumber_NE]
Name=Check Number, NotEqual
Description=Check Number, NotEqual.
IDOverride=50401
P2Type=Number

[SICheckNumber_GT]
Name=Check Number, Greater
Description=Check Number, Greater.
IDOverride=50402
P2Type=Number

[SICheckNumber_GE]
Name=Check Number, GreaterEqual
Description=Check Number, GreaterEqual.
IDOverride=50403
P2Type=Number

[SICheckNumber_LT]
Name=Check Number, Less
Description=Check Number, Less.
IDOverride=50404
P2Type=Number

[SICheckNumber_LE]
Name=Check Number, LessEqual
Description=Check Number, LessEqual.
IDOverride=50405
P2Type=Number
```



## ScriptActions.ini

```ini
[SIJumpToRandomScript]
Name=Jump To Random Script
Description=Replace the script for the current team according to the settings and weights.
IDOverride=50000
ParamDescription=IDCode
ParamType=Number

[SIScriptMaxDuration]
Name=Script Max Duration
Description=Set the maximum duration for the next script actions, and automatically end those script actions after timeout. The team will reset timer when switch the script actions.
IDOverride=50001
ParamDescription=IDCode
ParamType=Number

[SIPrintsToLog]
Name=Prints To debug.log
Description=Prints a string to debug.log according to the settings.
IDOverride=50010
ParamDescription=IDCode
ParamType=Number

[SIPrintsCSF]
Name=Prints CSF
Description=Prints a text to screen according to the settings.
IDOverride=50011
ParamDescription=IDCode
ParamType=Number

[SIFireRandomSuperWeapon]
Name=Fire Random Super Weapon
Description=Fire random super weapon on team leader floor according to the settings and weights.
IDOverride=50020
ParamDescription=IDCode
ParamType=Number

[SIPlaceRandomTechno]
Name=Place Random Techno
Description=Place random techno on team leader floor according to the settings and weights.
IDOverride=50030
ParamDescription=IDCode
ParamType=Number

[SIMoveToOffsetLocation]
Name=Move To Offset Location
Description=Command all team members to move to the designated location according to the settings and weights.
IDOverride=50040
ParamDescription=IDCode
ParamType=Number

[SIMoveToRandomLocation]
Name=Move To Random Location
Description=Command all team members to move to the designated location according to the settings and weights.
IDOverride=50041
ParamDescription=IDCode
ParamType=Number

[SISplitTeam]
Name=Split Team Members
Description=Split the eligible team members into a new team according to the settings and execute a new script.
IDOverride=50050
ParamDescription=IDCode
ParamType=Number

[SIVirtualKey]
Name=Virtual Key
Description=Like selecting all team members and pressing the specified 'Blank Key' (without actually selecting the team members).
IDOverride=50060
ParamDescription=KeyCode
Option0=1,Blank Key A
Option1=2,Blank Key B
Option2=3,Blank Key C
Option3=4,Blank Key D
Option4=5,Blank Key E
Option5=6,Blank Key F
Option6=7,Blank Key G
Option7=8,Blank Key H
Option8=9,Blank Key I
Option9=10,Blank Key J
Option10=11,Blank Key K
Option11=12,Blank Key L

[SIMarkBuff]
Name=Mark Buff
Description=Mark Buffs for all team members according to the settings.
IDOverride=50100
ParamDescription=IDCode
ParamType=Number

[SIActiveBuff]
Name=Active Buff
Description=Active Buffs for all team members according to the settings.
IDOverride=50101
ParamDescription=IDCode
ParamType=Number

[SIAfterBuff]
Name=After Buff
Description=After Buffs for all team members according to the settings.
IDOverride=50102
ParamDescription=IDCode
ParamType=Number

[SIRemoveBuff]
Name=Remove Buff
Description=Remove Buffs for all team members according to the settings.
IDOverride=50103
ParamDescription=IDCode
ParamType=Number

[SIModifyBuff]
Name=Modify Buff
Description=Modify Buffs for all team members according to the settings.
IDOverride=50104
ParamDescription=IDCode
ParamType=Number

[SIChangeBuff]
Name=Change Buff
Description=Change Buffs for all team members according to the settings.
IDOverride=50105
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Add]
Name=Operate House Variable, Add
Description=Operate House Variable, Add.
IDOverride=50200
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Mult]
Name=Operate House Variable, Mult
Description=Operate House Variable, Mult.
IDOverride=50201
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Pow]
Name=Operate House Variable, Pow
Description=Operate House Variable, Pow.
IDOverride=50202
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Log]
Name=Operate House Variable, Log
Description=Operate House Variable, Log.
IDOverride=50203
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Mod]
Name=Operate House Variable, Mod
Description=Operate House Variable, Mod.
IDOverride=50204
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_EQ]
Name=Operate House Variable, Equal
Description=Operate House Variable, Equal.
IDOverride=50210
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_NE]
Name=Operate House Variable, NotEqual
Description=Operate House Variable, NotEqual.
IDOverride=50211
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_GT]
Name=Operate House Variable, Greater
Description=Operate House Variable, Greater.
IDOverride=50212
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_GE]
Name=Operate House Variable, GreaterEqual
Description=Operate House Variable, GreaterEqual.
IDOverride=50213
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_LT]
Name=Operate House Variable, Less
Description=Operate House Variable, Less.
IDOverride=50214
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_LE]
Name=Operate House Variable, LessEqual
Description=Operate House Variable, LessEqual.
IDOverride=50215
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Not]
Name=Operate House Variable, Not
Description=Operate House Variable, Not.
IDOverride=50220
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Abs]
Name=Operate House Variable, Abs
Description=Operate House Variable, Abs.
IDOverride=50221
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Ceil]
Name=Operate House Variable, Ceil
Description=Operate House Variable, Ceil.
IDOverride=50222
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Floor]
Name=Operate House Variable, Floor
Description=Operate House Variable, Floor.
IDOverride=50223
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Round]
Name=Operate House Variable, Round
Description=Operate House Variable, Round.
IDOverride=50224
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Set]
Name=Operate House Variable, Set
Description=Operate House Variable, Set.
IDOverride=50230
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Max]
Name=Operate House Variable, Max
Description=Operate House Variable, Max.
IDOverride=50231
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Min]
Name=Operate House Variable, Min
Description=Operate House Variable, Min.
IDOverride=50232
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Random]
Name=Operate House Variable, Random
Description=Operate House Variable, Random.
IDOverride=50233
ParamDescription=IDCode
ParamType=Number

[SIOperateHouseVariable_Delete]
Name=Operate House Variable, Delete
Description=Operate House Variable, Delete.
IDOverride=50234
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_EQ]
Name=Check Techno Count And Switch Script, Equal
Description=Check Techno Count And Switch Script, Equal.
IDOverride=50300
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_NE]
Name=Check Techno Count And Switch Script, NotEqual
Description=Check Techno Count And Switch Script, NotEqual.
IDOverride=50301
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_GT]
Name=Check Techno Count And Switch Script, Greater
Description=Check Techno Count And Switch Script, Greater.
IDOverride=50302
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_GE]
Name=Check Techno Count And Switch Script, GreaterEqual
Description=Check Techno Count And Switch Script, GreaterEqual.
IDOverride=50303
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_LT]
Name=Check Techno Count And Switch Script, Less
Description=Check Techno Count And Switch Script, Less.
IDOverride=50304
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_LE]
Name=Check Techno Count And Switch Script, LessEqual
Description=Check Techno Count And Switch Script, LessEqual.
IDOverride=50305
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_EQ]
Name=Check Two Techno Counts And Switch Script, Equal
Description=Check Two Techno Counts And Switch Script, Equal.
IDOverride=50310
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_NE]
Name=Check Two Techno Counts And Switch Script, NotEqual
Description=Check Two Techno Counts And Switch Script, NotEqual.
IDOverride=50311
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_GT]
Name=Check Two Techno Counts And Switch Script, Greater
Description=Check Two Techno Counts And Switch Script, Greater.
IDOverride=50312
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_GE]
Name=Check Two Techno Counts And Switch Script, GreaterEqual
Description=Check Two Techno Counts And Switch Script, GreaterEqual.
IDOverride=50313
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_LT]
Name=Check Two Techno Counts And Switch Script, Less
Description=Check Two Techno Counts And Switch Script, Less.
IDOverride=50314
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_LE]
Name=Check Two Techno Counts And Switch Script, LessEqual
Description=Check Two Techno Counts And Switch Script, LessEqual.
IDOverride=50315
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_Waiting_EQ]
Name=Check Techno Count And Switch Script, Waiting, Equal
Description=Check Techno Count And Switch Script, Waiting, Equal.
IDOverride=50320
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_Waiting_NE]
Name=Check Techno Count And Switch Script, Waiting, NotEqual
Description=Check Techno Count And Switch Script, Waiting, NotEqual.
IDOverride=50321
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_Waiting_GT]
Name=Check Techno Count And Switch Script, Waiting, Greater
Description=Check Techno Count And Switch Script, Waiting, Greater.
IDOverride=50322
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_Waiting_GE]
Name=Check Techno Count And Switch Script, Waiting, GreaterEqual
Description=Check Techno Count And Switch Script, Waiting, GreaterEqual.
IDOverride=50323
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_Waiting_LT]
Name=Check Techno Count And Switch Script, Waiting, Less
Description=Check Techno Count And Switch Script, Waiting, Less.
IDOverride=50324
ParamDescription=IDCode
ParamType=Number

[SICheckTechnoCountAndSwitchScript_Waiting_LE]
Name=Check Techno Count And Switch Script, Waiting, LessEqual
Description=Check Techno Count And Switch Script, Waiting, LessEqual.
IDOverride=50325
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_Waiting_EQ]
Name=Check Two Techno Counts And Switch Script, Waiting, Equal
Description=Check Two Techno Counts And Switch Script, Waiting, Equal.
IDOverride=50330
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_Waiting_NE]
Name=Check Two Techno Counts And Switch Script, Waiting, NotEqual
Description=Check Two Techno Counts And Switch Script, Waiting, NotEqual.
IDOverride=50331
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_Waiting_GT]
Name=Check Two Techno Counts And Switch Script, Waiting, Greater
Description=Check Two Techno Counts And Switch Script, Waiting, Greater.
IDOverride=50332
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_Waiting_GE]
Name=Check Two Techno Counts And Switch Script, Waiting, GreaterEqual
Description=Check Two Techno Counts And Switch Script, Waiting, GreaterEqual.
IDOverride=50333
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_Waiting_LT]
Name=Check Two Techno Counts And Switch Script, Waiting, Less
Description=Check Two Techno Counts And Switch Script, Waiting, Less.
IDOverride=50334
ParamDescription=IDCode
ParamType=Number

[SICheckTwoTechnoCountsAndSwitchScript_Waiting_LE]
Name=Check Two Techno Counts And Switch Script, Waiting, LessEqual
Description=Check Two Techno Counts And Switch Script, Waiting, LessEqual.
IDOverride=50335
ParamDescription=IDCode
ParamType=Number

[SICheckNumber_EQ]
Name=Check Number And Switch Script, Equal
Description=Check number and switch script, Equal.
IDOverride=50400
ParamDescription=IDCode
ParamType=Number

[SICheckNumber_NE]
Name=Check Number And Switch Script, NotEqual
Description=Check number and switch script, NotEqual.
IDOverride=50401
ParamDescription=IDCode
ParamType=Number

[SICheckNumber_GT]
Name=Check Number And Switch Script, Greater
Description=Check number and switch script, Greater.
IDOverride=50402
ParamDescription=IDCode
ParamType=Number

[SICheckNumber_GE]
Name=Check Number And Switch Script, GreaterEqual
Description=Check number and switch script, GreaterEqual.
IDOverride=50403
ParamDescription=IDCode
ParamType=Number

[SICheckNumber_LT]
Name=Check Number And Switch Script, Less
Description=Check number and switch script, Less.
IDOverride=50404
ParamDescription=IDCode
ParamType=Number

[SICheckNumber_LE]
Name=Check Number And Switch Script, LessEqual
Description=Check number and switch script, LessEqual.
IDOverride=50405
ParamDescription=IDCode
ParamType=Number
```