# Java - 내 화면 캡쳐 Screen Capture

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.27)   

```
Rectangle screenRect = new Rectangle(Toolkit.getDefaultToolkit().getScreenSize());
BufferedImage capture = new Robot().createScreenCapture(screenRect);
//BufferedImage 에 스크린 화면이 캡쳐되어 들어있다

//파일로 출력
ImageIO.write(capture,"bmp", new File("capture.bmp"));

//바이트 데이터로 변환
ByteArrayOutputStream bos = new ByteArrayOutputStream();
ImageIO.write(capture,"bmp",bos);
byte[] data = bos.toByteArray();

//Rectangle 의 범위를 수정하면 일부 영역만 캡쳐하는것도 가능해 보인다.
```