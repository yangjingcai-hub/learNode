package test;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
public class keybord extends JFrame implements ActionListener{
	public String content=" ";           //文本框内容
	public String[][] arrs= {
			{"`","1","2","3","4","5","6","7","8","9","0","-","=","delete"},
			{"tab","q","w","e","r","t","y","u","i","o","p","[","]","\\"},
			{"CapsLock","a","s","d","f","g","h","j","k","i",";","'","return"},
			{"shift","z","x","c","v","b","n","m",",",".","/","shift"}
	};
	public String[] diffbtn= {"delete","tab","CapsLock","shift"};
	JTextField t1;
	public static void main(String[] args) {
		keybord a=new keybord();
	}
	 keybord(){
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
	}
	public void draw() {  //通过循环来绘制键盘
		int avagerw=55;
		int avagerh=60;
		int y=80;
		for(int i=0;i<arrs.length;i++) {
			int x=0;
			System.out.println("");
			for(int j=0;j<arrs[i].length;j++) {
				JButton btn1=new JButton(arrs[i][j]);
				btn1.addMouseListener(new MouseListener() {
					public void mouseClicked(MouseEvent e) {}
					public void mousePressed(MouseEvent e) {}
					public void mouseReleased(MouseEvent e) {}
					public void mouseEntered(MouseEvent e) {btn1.setBackground(Color.red);}
					public void mouseExited(MouseEvent e) {btn1.setBackground(Color.white);}
				});
				btn1.addActionListener(this);
				btn1.setActionCommand(arrs[i][j]);
				btn1.setBackground(Color.white);
				if(arrs[i][j]=="delete" || arrs[i][j]=="tab") {
					btn1.setBounds(x, y, 80, avagerh);
					x+=80;
					this.add(btn1);
				}else if(arrs[i][j]=="CapsLock" || arrs[i][j]=="return"){
					btn1.setBounds(x, y, 95, avagerh);
					x+=95;
					this.add(btn1);
				}else if(arrs[i][j]=="shift") {
					btn1.setBounds(x, y, 125, avagerh);
					x+=125;
					this.add(btn1);
				}else {
					btn1.setBounds(x, y, avagerw, avagerh);
					x+=avagerw;
					this.add(btn1);
				}	
			}
			y+=avagerh;
		}
		JButton btn1=new JButton("空格");
		btn1.addActionListener(this);
		btn1.setActionCommand("kg");
		btn1.setBackground(Color.white);
		btn1.setBounds(0, 320, 800, 70);
		this.add(btn1);
	}
	public void actionPerformed(ActionEvent e) {
		if(e.getActionCommand().equals("delete")) {
			content = content.substring(0,content.length()-1);
			t1.setText(content);
			return;
		}
		for(int i=0;i<arrs.length;i++) {
			for(int j=0;j<arrs[i].length;j++) {
				if(e.getActionCommand().equals(arrs[i][j])){
					content+=arrs[i][j];
					t1.setText(content);
				}
			}
		}
		if(e.getActionCommand().equals("kg")) {
			content=" ";
			t1.setText(content);
		}
	}
}
		
