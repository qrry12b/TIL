# Android API 29 이상 범위 지정 저장소 대응 (다운로드 디렉터리에 파일쓰기)

Android 6.0 (API 23) 이상을 타깃 하는 경우 쓰기 권한을 요청하고 File 을 통해 저장할 경로를 지정해 직접 쓰기 작업을 했다면

Android 10 (API 29) 이상을 타깃 하는 경우 범위 지정 저장소가 적용되며 기존 저장소 읽기/쓰기 권한으로는 File 을 통해 직접 다운로드 디렉터리에 쓰기 작업을 할 수 없습니다.

android 10 (API 29)에서는 Android Manifest.xml에 **requestLegacyExternalStorage** 속성을 사용해 일시적으로 선택 해제할 수 있지만 API 30부터는 해당 속성이 무시됩니다.

android 10 (API 29)부터는 File을 통해 직접 쓰기 작업을 할 수 없지만 ContentResolver, ContentValues를 통해 별다른 권한 요청 없이 파일을 생성할 수 있습니다.

아래는 다운로드 디렉터리에 텍스트 파일을 생성하는 예시이며 실제 쓰기 작업 시에는 이미 같은 이름의 파일이 있는지 확인하거나 메인 스레드가 아닌 다른 스레드에서 호출해야 합니다.

이미 같은 이름의 파일이 존재하는 경우 파일명 끝에 자동으로 구분하기 위한 (숫자) 가 포함됩니다.

```
if(Build.VERSION.SDK_INT >= 29) {
    String fileName = "MyDownload.txt"; //저장 이름
    String subDirectory = "myAppSubDirectory"; // Downloads/ {subDirectory}
    String mimeType = "text/plane"; //mimeType
    String writeText = "Create Download Directory Text File " + System.currentTimeMillis(); //writeText

    ContentValues values = new ContentValues();
    values.put(MediaStore.Downloads.MIME_TYPE, mimeType);
    values.put(MediaStore.Downloads.IS_PENDING, 1);

    if(subDirectory != null) {
        values.put(MediaStore.Downloads.RELATIVE_PATH, Environment.DIRECTORY_DOWNLOADS + File.separator + subDirectory);
    }

    ContentResolver database = getBaseContext().getContentResolver();
    Uri uri = database.insert(MediaStore.Downloads.EXTERNAL_CONTENT_URI, values);

    try{
        OutputStream outputStream = getBaseContext().getContentResolver().openOutputStream(uri);
        outputStream.write( writeText.getBytes(StandardCharsets.UTF_8) );
        outputStream.close();
    }catch(Exception e) {
        e.printStackTrace();
    }

    values.clear();
    values.put(MediaStore.Downloads.DISPLAY_NAME, fileName);
    values.put(MediaStore.Downloads.IS_PENDING, 0);

    database.update(uri, values, null, null);
}
```