---
title : Unity 동작방식2 :게임 루프
author : YS_AN
date : 2023-10-17 21:12:04 +0900
categories : [Unity, UnityInfo]
tags : [Unity, 기술면접]
---

# 게임 루프 (Game Loop)
전체 게임 프로그램의 전반적인 흐름 제어로, 사용자가 종료할 때까지 게임은 일련의 작업을 계속 반복함. <br/>
게임 루프의 반복을 **프레임**이라고 한다. 대부분의 실시간 게임은 초당 여러번 업데이트되고,
이때 업데이트 되는 회수를 초당 프레임수라고 부른다. 가장 일반적인 간격은 30과 60이다. (60FPS = 1초에 60번 업데이트 됨)

```c#
bool running = true;

while (running) {
    Update();
    Render();
}
```



