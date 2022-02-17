# Java - 클립보드에 저장된 내용 가져오기 (awt.Toolkit)

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

```
import java.awt.Toolkit;
import java.awt.datatransfer.Clipboard;
import java.awt.datatransfer.DataFlavor;
import java.awt.datatransfer.Transferable;
import java.awt.datatransfer.UnsupportedFlavorException;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.List;
public class MTest2 {
	public static void main(String[] args) throws UnsupportedFlavorException, IOException {
		Clipboard clipboard = Toolkit.getDefaultToolkit().getSystemClipboard();
		Transferable contents = clipboard.getContents(clipboard);
		if(contents.isDataFlavorSupported(DataFlavor.javaFileListFlavor)) {
			List<File> files = (List) contents.getTransferData(DataFlavor.javaFileListFlavor);
			for(File obj : files) {
				System.out.println(obj);
			}
		}
		if(contents.isDataFlavorSupported(DataFlavor.imageFlavor)) {
			BufferedImage image = (BufferedImage) contents.getTransferData(DataFlavor.imageFlavor);
			System.out.println(image);
		}
		if(contents.isDataFlavorSupported(DataFlavor.allHtmlFlavor)) {
			String html = (String) contents.getTransferData(DataFlavor.allHtmlFlavor);
			System.out.println(html);
		}
		if(contents.isDataFlavorSupported(DataFlavor.stringFlavor)) {
			String str = (String) contents.getTransferData(DataFlavor.stringFlavor);
			System.out.println(str);
		}
	}
}
```