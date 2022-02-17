# Java - Web 이미지 내려받기

(이 TIL은 개인 블로그에 정리했던 내용을 계정 정리로 삭제하기 위해 옮겨적은 내용입니다. 작성일 2020.9.29)   

``
 public static void getImage (String str) {
		try {
			URL url = new URL(str);
			InputStream in = new BufferedInputStream(url.openStream());
			ByteArrayOutputStream out = new ByteArrayOutputStream();
			byte[] buf = new byte[1024];
			int n = 0;
			while(-1!=(n=in.read(buf))) {
				out.write(buf,0,n);
			}
			out.close();
			in.close();
			byte[] response = out.toByteArray();
			
			FileOutputStream fos = new FileOutputStream("./img.png");
			fos.write(response);
			fos.close();
		}catch(Exception e) {
			e.printStackTrace();
		}
	}
```

파일 확장자가 명확할때는 저장할 이름을 직접 지정하지만 모를경우 바이너리 데이터 (바이트코드)의 헤더를 해석해서 추측할 수 있다.

### REF
* [stackoverflow](https://stackoverflow.com/questions/5882005/how-to-download-image-from-any-web-page-in-java)