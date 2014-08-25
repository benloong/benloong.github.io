Unreal Engine 4 Notes
===============

###GameMode 制定游戏规则，包含游戏状态

游戏类型，比赛状态

###Pawn 可控制的游戏对象

可被AI或玩家控制的Actor，子类Character是专门针对人形可行走的Pawn

###Controller 控制Pawn，给Pawn发送命令

控制Pawn的行为，默认情况下，一个Controller只能控制一个Pawn， Pawn实例化的时候不会自动被Controller控制，RTS类型游戏一个Controller可以控制多个Pawn

###Camera 决定玩家角度的视觉

后期效果可以加在摄像机上
摄像机职责链
CameraComponent -> PlayerCameraManager

摄像机也可被PlayerController控制

###HUD UI 交互界面，抬头显示
UI是可交互的
HUD一般是指显示信息，不能交互的