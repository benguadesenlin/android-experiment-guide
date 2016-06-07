# 游戏开发

##1. 黑白块


完整代码参见：https://github.com/hzuapps/android-labs/tree/master/app/src/main/java/edu/hzuapps/androidworks/homeworks/net1314080903206 


##1.小块实例
public Net1314080903206Blocks(Context context) {
		super(context);
		
		label = new TextView(getContext());
		label.setTextSize(1);
		label.setTextColor(Color.RED);
		label.setBackgroundColor(Color.BLACK);
		label.setTextSize(10);
		//label.setText("w");
		
		LayoutParams lp = new LayoutParams(-1, -1);
		lp.setMargins(5, 5, 0, 0);
		addView(label, lp);
		
	}
	
  先写出小块的实例。然后在游戏界面直接调用生成该小块就行。
  
##2.开始游戏
private void startGame()
	{
		
		Net1314080903206StepOnWhiteActivity sowa = Net1314080903206StepOnWhiteActivity.getSOWActivity();
		sowa.showBestScore(sowa.getBestScore());
		sowa.clearScore();
	
		for (int x = 0; x < 5; x++) {
			for (int y = 0; y < 5; y++) {
				blocksMap[x][y].setColor(Color.BLACK);
				blocksMap[x][y].label.setTextSize(10);
			}
		}
		complete=false;
		Net1314080903206StepOnWhiteActivity.getSOWActivity().setTime();
		if(frist)		
			Net1314080903206StepOnWhiteActivity.getSOWActivity().TimeStart();								
		frist=true;
		
		addRandomColor();
		Log.d("SowGameView",Net1314080903206StepOnWhiteActivity.getSOWActivity().t+"");
	}
  
##3.添加小块（黑块）
private void addBlocks(int blockWidth,int blockHeight)
	{
		Net1314080903206Blocks b;
		
		for (int x = 0; x < 5; x++) {
			for (int y = 0; y < 5; y++) {
				
				b = new Net1314080903206Blocks(getContext());
				addView(b, blockWidth, blockHeight);
				
				blocksMap[x][y] = b;
			}
		}

	}
	
##4.添加白块
private void addRandomColor() 
	{
	
		emptyPoints.clear();
		
		if(complete==false)
		{
			for (int x = 0; x < 5; x++) {
				for (int y = 0; y < 5; y++) {					
					emptyPoints.add(new Point(x, y));								
				}
			}
		
			p = emptyPoints.remove((int)(Math.random()*emptyPoints.size()));
			if(blocksMap[p.x][p.y].label.getTextSize()>=100)
				addRandomColor();
			blocksMap[p.x][p.y].setColor(Color.WHITE);	
			blocksMap[p.x][p.y].label.setTextSize(200);
		}

	}
	
##5.实时更新游戏的界面
private void changeColor()
	{
		
		
		for (int x = 0; x < 5; x++) 
		{
			for (int y = 0; y < 5; y++) 
			{
				
					final Net1314080903206Blocks bl;
					bl=blocksMap[x][y];
					bl.setOnClickListener(new View.OnClickListener() {
						
						@Override
						public void onClick(View v) {
							checkComplete();
							if(complete)
								gameOver();
							else
							{
								if(bl.label.getTextSize()>=100)
								{
							
									bl.setColor(Color.BLACK);
									bl.label.setTextSize(1);
									Net1314080903206StepOnWhiteActivity.getSOWActivity().addScore(1);
								
									n=0;
								
									int k;
									k=(int)(Math.random()*2+1);
									for (int x = 0; x < 5; x++) 
									{
										for (int y = 0; y < 5; y++) 
										{
											if(blocksMap[x][y].label.getTextSize()<=100)
												n++;
											else
												m++;
										}
									}
									if(k>n)
										k=n;
									if(m>0)
										check=false;
									else check=true;
									if(check)
									{
										while(k>=0)
										{
											addRandomColor();
											k--;
										}
									}
									while(m>0)
									{
										m--;
									}
									i=1;
								}
								else Net1314080903206StepOnWhiteActivity.getSOWActivity().addScore(-5);											
							}
						}
					});	
				
				if(i==1)
					break;
			}
			if(i==1)
				break;
		}
					
	}
	
##6.结束游戏
private void gameOver()
	{
		String s="你的分数是："+Net1314080903206StepOnWhiteActivity.getSOWActivity().getScore();
		new AlertDialog.Builder(getContext()).setTitle("gameover").setMessage(s).setCancelable(false).setPositiveButton("重来", new DialogInterface.OnClickListener() {
				
				@Override
				public void onClick(DialogInterface dialog, int which) {
					startGame();
				}
			}).setNegativeButton("不玩了",new DialogInterface.OnClickListener()
			{

				@Override
				public void onClick(DialogInterface dialog, int which) {
					Net1314080903206StepOnWhiteActivity.getSOWActivity().exitGame();
					
				}
				
			}).show();
	}
	
##7.保存或获取最高分
public void saveBestScore(int s){
		Editor e = getPreferences(MODE_PRIVATE).edit();
		e.putInt(SP_KEY_BEST_SCORE, s);
		e.commit();
	}

	public int getBestScore(){
		return getPreferences(MODE_PRIVATE).getInt(SP_KEY_BEST_SCORE, 0);
	}
