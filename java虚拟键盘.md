# 使用java创建一个虚拟键盘
![输入图片说明](https://images.gitee.com/uploads/images/2019/1017/042321_f8314c6d_5138370.png "clipboard1.png")



#### 功能说明
1. 当鼠标经过按键的时候颜色变为红色,离开按键变为默认颜色
2. 当点击按键的时候,按键的内容输入的顶部的文本框
3. 点击delete按键和空格按键能对文本框内容进行删除和清空


#### 流程

1. 绘制界面
2. 使用循环动态创建按键
3. 给组件添加事件



### 绘制界面
先创建一个窗口,窗口宽高为800,420,窗体的宽高会影响按键的宽高


```
public class demo extends JFrame{
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		demo one=new demo();
	}
	demo(){
		this.setLayout(null);
		this.setSize(800,420);
		this.setLocationRelativeTo(this);
		this.setVisible(true);
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		this.validate();
		this.setResizable(false);
	}

}

```

![输入图片说明](https://images.gitee.com/uploads/images/2019/1017/081942_8ab172d2_5138370.png "one1.png")


#### 绘制按键
如果一个个的创建按键将是一个巨大的工程,不仅会造成大量代码冗余,也浪费时间,所以使用数组来创建按键,然后厉遍数组动态的生成按键

```
    public class demo{
      public String[] one= {"`","1","2","3","4","5","6","7","8","9","0","-","=","delete"};
      public String[] two= {"tab","q","w","e","r","t","y","u","i","o","p","[","]","\\"};
      public String[] three= {"CapsLock","a","s","d","f","g","h","j","k","i",";","'","return"};
      public String[] four= {"shift","z","x","c","v","b","n","m",",",".","/","shift"};
	public static void main(String[] args) {
		demo one=new demo();
	}
	demo(){
		..............
	}

}

```
- 数组会在多个方法里调用所以把数组声明为一个公共数组
- 第一行14个按键的长度需要刚好填充JFrame窗口,所以对于按键的长度有着精确的要求,并且第一行有13个普通按键(宽度相同),1个异按键(长度大于普通按键),异按键的宽度是普通按键的1.5倍所以只需要算出普通按键的宽度就行了
> 13x+1.5x=800      14.5x=800        x=55    
> 普通按钮=55        异按键=55*1.5
- 从第一行来看,每一行的异按键的宽度都是不同和按键的数量都是不相同的.所以需要灵活的设置异按键的宽度

	

```
public void draw() {
                int avagerw=55;  //普通按键的宽度
		int avagerh=60;  //普通按键的高度
		int x1=0;        //第一行按键的x坐标
		int y1=80;      //第一行按键的y坐标
		for(int i=0;i<one.length;i++) {       
			JButton btn1=new JButton(one[i]);
			btn1.setBackground(Color.white);
			if(one[i]!="delete") {   //判断所有长度正常的按键
				btn1.setBounds(x1, y1, avagerw, avagerh);
				x1+=avagerw;
                                //当创建一个按键的时候x坐标进行累加,不然所有按键都会重叠
				this.add(btn1); 
			}else {                
                    //当数组厉遍为delete的时候,另外给delete异按钮设置宽度
				btn1.setBounds(x1, y1, 82, avagerwh);
				this.add(btn1); 
			}
		}
	}
```


![输入图片说明](https://images.gitee.com/uploads/images/2019/1017/084313_12fe358e_5138370.png "one2.png")

- 这样就把第一行按键创建完毕了,另外三排按键也是使用同样的方法,记住每一行异按钮的宽度是不同,并且x1和y1坐标也是不同,需要另外创建坐标变量

#### 添加事件

- 当单击按键的时候,把单击按键对应数组的值输出到文本框,在动态创建按键的同时也同时给所有按键添加事件
- 然后在事件的抽象方法里对当前点击事件进行判断,拿到当前点击的command和数组里的按键值进行对比,然后拿到当前的数组的索引并通过当前的索引值输出第一行按键数组的值到文本框
- 文本框的值是储存在content公共变量里的,我们删除和清空字符的时候只用清空centent变量


```

public String content=" ";
public void draw() {  //通过循环来绘制键盘
		int avagerw=55;
		int avagerh=60;
		int x1=0;
		int y1=80;
		for(int i=0;i<one.length;i++) {       
			JButton btn1=new JButton(one[i]);
			btn1.setBackground(Color.white);
			if(one[i]!="delete") {   //判断所有长度正常的按键
				btn1.setBounds(x1, y1, avagerw, avagerh);
				x1+=avagerw;
				btn1.addActionListener(this);  
                                //给按键添加一个事件
				btn1.setActionCommand(one[i]);
                                //给按键设置一个Commad类似于标记,用来区分事件
				this.add(btn1); 
			}else {                //按键长度大于平均按键长度
				btn1.addActionListener(this);
				btn1.setActionCommand("delete");
				btn1.setBounds(x1, y1, 80, avagerh);
				this.add(btn1); 
			}
		}
}
public void actionPerformed(ActionEvent e) {
//当事件被触发的时候,判断按钮是否被单击,并且输出,按钮当前被单击的数组索引,再通过索引去获取该数组里面的值,并且输出到content函数
		for(int i=0;i<one.length-1;i++) {
			if(e.getActionCommand().equals(one[i])) {
				content+=one[i];
				t1.setText(content);
			}
		}
}

```

- 这样我们就完成了对一行按键的事件添加,其他三行也是同样的方法

#### 设置 delete 键和 空格键 的功能
当delete按键被单击的时候就删除一个字符,使用subString函数可以实现(记得给删除按键设置事件和标识(ActionCommand)),对content函数进行截取

```
if(e.getActionCommand().equals("delete")) {
			//当delete按键被单击,content变量的最后一个字符
			content = content.substring(0,content.length()-1);
			t1.setText(content);
		}
```

空格键也是同理,把 **content** 变量清空就行了


#### 鼠标滑动
当鼠标经过按键的,当前按键的背景颜色变为红色,在创建按键的时候添加鼠标事件

	
```
for(int i=0;i<one.length;i++) {       
			JButton btn1=new JButton(one[i]);
			btn1.setBackground(Color.white);
			btn1.addMouseListener(new MouseListener() {       //对所有进行监听
				public void mouseClicked(MouseEvent e) {}
				public void mousePressed(MouseEvent e) {}
				public void mouseReleased(MouseEvent e) {}
                                //当鼠标进入按键的时候触发Entered函数
				public void mouseEntered(MouseEvent e) {btn1.setBackground(Color.red);}
                                //当鼠标离开按键的时候触发Exited函数
				public void mouseExited(MouseEvent e) {btn1.setBackground(Color.white);}
			});
			if(one[i]!="delete") {  
				btn1.setBounds(x1, y1, avagerw, avagerh);
				x1+=avagerw;
				btn1.addActionListener(this);
				btn1.setActionCommand(one[i]);
				this.add(btn1); 
			}else {               
				btn1.addActionListener(this);
				btn1.setActionCommand("delete");
				btn1.setBounds(x1, y1, 80, avagerh);
				this.add(btn1); 
			}
		}
```
这样就把鼠标滑动效果实现了,是不是很简单呀

#### 全部代码演示

```
package keyborder;
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
public class test extends JFrame implements ActionListener{
	public String content=" ";           //文本框内容为空
        //键盘按键
	public String[] one= {"`","1","2","3","4","5","6","7","8","9","0","-","=","delete"};   
	public String[] two= {"tab","q","w","e","r","t","y","u","i","o","p","[","]","\\"};
	public String[] three= {"CapsLock","a","s","d","f","g","h","j","k","i",";","'","return"};
	public String[] four= {"shift","z","x","c","v","b","n","m",",",".","/","shift"};
	JTextField t1;
	public static void main(String[] args) {
		test a=new test();
	}
	test(){
		draw();
		t1=new JTextField();
		t1.setText(content);
		t1.setBounds(0, 0, 800, 80);
		this.add(t1);
		this.setLayout(null);
		this.setSize(800,420);
		this.setLocationRelativeTo(this);
		this.setVisible(true);
		this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		this.validate();
		this.setResizable(false);
	}
	public void draw() {      //通过循环动态绘制按键
		int avagerw=55;      //普通按键的宽度
		int avagerh=60;      //异按键的宽度
		int x1=0;
		int y1=80;
		for(int i=0;i<one.length;i++) {       
			JButton btn1=new JButton(one[i]);
			btn1.setBackground(Color.white);
                        //给按键添加鼠标监听事件,当鼠标经过或离开按键的时候改变按键的背景颜色
			btn1.addMouseListener(new MouseListener() {     
				public void mouseClicked(MouseEvent e) {}
				public void mousePressed(MouseEvent e) {}
				public void mouseReleased(MouseEvent e) {}
				public void mouseEntered(MouseEvent e) {btn1.setBackground(Color.red);}
				public void mouseExited(MouseEvent e) {btn1.setBackground(Color.white);}
			});
			if(one[i]!="delete") {   //判断所有普通按键
				btn1.setBounds(x1, y1, avagerw, avagerh);
				x1+=avagerw;
				btn1.addActionListener(this);
				btn1.setActionCommand(one[i]);
				this.add(btn1); 
			}else {                //判断异按键
				btn1.addActionListener(this);
				btn1.setActionCommand("delete");
				btn1.setBounds(x1, y1, 80, avagerh);
                                            //给异按键设置80宽度
				this.add(btn1); 
			}
		}
		int x2=0;
		int y2=140;
		for(int i=0;i<two.length;i++) {
			JButton btn2=new JButton(two[i]);
			btn2.setBackground(Color.white);
			btn2.addMouseListener(new MouseListener() {
				public void mouseClicked(MouseEvent e) {}
				public void mousePressed(MouseEvent e) {}
				public void mouseReleased(MouseEvent e) {}
				public void mouseEntered(MouseEvent e) {btn2.setBackground(Color.red);}
				public void mouseExited(MouseEvent e) {btn2.setBackground(Color.white);}
			});
			if(two[i]!="tab") {
				btn2.setBounds(x2, y2, avagerw, avagerh);
				x2+=avagerw;
				btn2.addActionListener(this);
				btn2.setActionCommand(two[i]);
				this.add(btn2); 
			}else {
				btn2.setBounds(x2, y2, 80, avagerh);
				x2+=80;
				this.add(btn2); 
			}
		}
		int x3=0;
		int y3=140+60;
		for(int i=0;i<three.length;i++) {
			JButton btn3=new JButton(three[i]);
			btn3.setBackground(Color.white);
			btn3.addMouseListener(new MouseListener() {
				public void mouseClicked(MouseEvent e) {}
				public void mousePressed(MouseEvent e) {}
				public void mouseReleased(MouseEvent e) {}
				public void mouseEntered(MouseEvent e) {btn3.setBackground(Color.red);}
				public void mouseExited(MouseEvent e) {btn3.setBackground(Color.white);}
			});
			if(three[i]!="CapsLock") {        
				if(i==three.length-1) { //当for循环到最后一个按键的时候,给最后一个异类按键设置另外的宽度
				btn3.setBounds(x3, y3, 95, avagerh);
				this.add(btn3);
				continue;
				}
				btn3.setBounds(x3, y3, avagerw, avagerh);
				x3+=avagerw;
				btn3.addActionListener(this);
				btn3.setActionCommand(three[i]);
				this.add(btn3); 
			}else { //第一个按键设置宽度
				btn3.setBounds(x3, y3, 100, avagerh);
				x3+=97;
				this.add(btn3); 
			}
		}
		int x4=0;
		int y4=140+60+60;
		for(int i=0;i<four.length;i++) {
			JButton btn4=new JButton(four[i]);
			btn4.setBackground(Color.white);
			btn4.addMouseListener(new MouseListener() {
				public void mouseClicked(MouseEvent e) {}
				public void mousePressed(MouseEvent e) {}
				public void mouseReleased(MouseEvent e) {}
				public void mouseEntered(MouseEvent e) {btn4.setBackground(Color.red);}
				public void mouseExited(MouseEvent e) {btn4.setBackground(Color.white);}
			});
			if(four[i]!="shift") {
				btn4.setBounds(x4, y4, avagerw, avagerh);
				x4+=avagerw;
				btn4.addActionListener(this);
				btn4.setActionCommand(four[i]);
				this.add(btn4); 
				System.out.println("true1");
				
			}else {
				btn4.setBounds(x4, y4, 125, avagerh);
				x4+=125;
				this.add(btn4); 
				System.out.println("false1");
			}
		}
		//添加空格按键并且设置事件
		JButton btn1=new JButton("空格");       
		btn1.addActionListener(this);
		btn1.setBackground(Color.white);
		btn1.setActionCommand("kg");
		btn1.setBounds(0, y4+60, 800, 70);
		this.add(btn1);
	}
	@Override
	public void actionPerformed(ActionEvent e) {
		//当事件被触发的时候,判断按钮是否被单击,并且输出,按钮当前被单击的数组索引,再通过索引去获取该数组里面的值,并且输出到        
                //content函数
		for(int i=0;i<one.length-1;i++) {
			if(e.getActionCommand().equals(one[i])) {
				content+=one[i];
				t1.setText(content);
			}
		}
		for(int i=0;i<two.length;i++) {
			if(e.getActionCommand().equals(two[i])) {
				content+=two[i];
				t1.setText(content);
			}
		}
		for(int i=0;i<three.length;i++) {
			if(e.getActionCommand().equals(three[i])) {
				content+=three[i];
				t1.setText(content);
			}
		}
		for(int i=0;i<four.length;i++) {
			if(e.getActionCommand().equals(four[i])) {
				content+=four[i];
				t1.setText(content);
			}
		}
		if(e.getActionCommand().equals("kg")) {
			//当空格被单击清空,content变量里的值
			content=" ";
			t1.setText(content);
		}
		if(e.getActionCommand().equals("delete")) {
			//当delete按键被单击,content变量的最后一个字符
			content = content.substring(0,content.length()-1);
			t1.setText(content);
		}
	}
}
```
@黑何纵
 _如有错误的地方请在评论区及时指正,以免误人子弟._ 
