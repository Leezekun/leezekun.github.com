# 计算机图形学课程展示 

    按照老师上课讲的ppt，此次画火柴人行走的重点在如果画分成两段的胳膊和腿，并使火柴人行走。首先设计了一个画方格的方法，火柴人的身体每一部分均为方格，不过不同部位的大小以及位置不同，可以用来实现。另外在画胳膊和腿的两部分的时候，按照ppt上所讲的步骤来画。在控制stickman行走的时候，每次使其手脚摆动一定角度，当达到最大角度时，将方向调反，反向摆动。
    
* glTranslatef(xPos,yPos,zPos);
* glScalef(x,y,z);
* glTranslatef(Tx,Ty,0);
* glRotatef(u,0,0,1);


## 以下为效果展示：

![](/images/blog/walkingman.gif)
