# Java - Swing 텍스트 Drop 처리

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

```
import java.awt.Color;
import java.awt.HeadlessException;
import java.awt.datatransfer.DataFlavor;
import java.awt.dnd.DnDConstants;
import java.awt.dnd.DropTarget;
import java.awt.dnd.DropTargetDropEvent;
import java.awt.event.WindowEvent;
import java.awt.event.WindowListener;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileOutputStream;
import java.io.OutputStreamWriter;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Collections;
import java.util.HashSet;
import java.util.List;
import java.util.Set;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.TransferHandler;

public class ToDropText extends JFrame implements WindowListener{
	public static void main(String[] args) {
		new ToDropText();
	}

	private Set<String> sear = new HashSet<String>();
    
	public ToDropText() throws HeadlessException {
		super();
		addWindowListener(this);
		setSize(300,300);
		setAlwaysOnTop(true);
		setDefaultCloseOperation(JFrame.DO_NOTHING_ON_CLOSE);
		
		JPanel panel = new JPanel();
		panel.setBackground(Color.white);
		panel.setTransferHandler(new TransferHandler("text"));
		panel.setDropTarget(new DropTarget() {
			@Override
			public synchronized void drop(DropTargetDropEvent dtde) {
				try {
					dtde.acceptDrop(DnDConstants.ACTION_COPY);
					String c = dtde.getTransferable().getTransferData(DataFlavor.stringFlavor).toString();
					sear.add(c);
				}catch(Exception e) {
					e.printStackTrace();
				}
			}
		});
		add(panel);
		setVisible(true);
	}

	@Override
	public void windowOpened(WindowEvent e) {
		
	}

	@Override
	public void windowClosing(WindowEvent e) {
		if(job()) {
			dispose();			
		}else {
			JOptionPane.showMessageDialog(this, "error");
			dispose();
		}
		
	}

	private boolean job() {
		boolean bool = false;
		try {
			bool = true;
			BufferedWriter buf = null;
			try {
				Calendar cal = Calendar.getInstance();
				String fileName = new SimpleDateFormat("yyyy-MM-dd_HH-mm-ss").format(Calendar.getInstance().getTime());
				buf = new BufferedWriter(new OutputStreamWriter(new FileOutputStream(new File("./"+fileName)),"UTF8"));
				
				buf.append(" -- "+fileName+"\n");
				
				List<String> list = new ArrayList(sear);
				Collections.sort(list);
				for(String str : list) {
					buf.append(str+"\n");
				}
				
				buf.append(" -- ");
			}catch(Exception e) {
				e.printStackTrace();
			}
			try {
				if(buf!=null) {buf.close();}
			}catch (Exception e) {
				e.printStackTrace();
			}
		}catch(Exception e) {
			e.printStackTrace();
		}
		return bool;
	}

	@Override
	public void windowClosed(WindowEvent e) { }

	@Override
	public void windowIconified(WindowEvent e) { }

	@Override
	public void windowDeiconified(WindowEvent e) { }

	@Override
	public void windowActivated(WindowEvent e) { }

	@Override
	public void windowDeactivated(WindowEvent e) { }
}
```