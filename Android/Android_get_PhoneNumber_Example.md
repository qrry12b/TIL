# Android get PhoneNumber Example (안드로이드 전화번호 얻기)

번호를 얻어 로그를 출력하는 메서드와 번호를 반환하는 메서드로 나누어져 있습니다.

```
/** 모바일 전화번호를 로그로 출력합니다 */
private void phoneNumberLog() {
    try{
        String phoneNumber = getPhoneNumber();
        Log.d(TAG, "phoneNumber : " + phoneNumber);
    }catch(SecurityException e) { //권한이 없어 예외가 발생하면 권한을 요청합니다
        ActivityCompat.requestPermissions(this, new String[] {Manifest.permission.READ_PHONE_NUMBERS, Manifest.permission.READ_PHONE_STATE}, 123123 );
        Log.e(TAG, e.getMessage());
    }
}

/** 모바일 전화번호를 반환합니다 */
private String getPhoneNumber() throws SecurityException {
    TelephonyManager telephonyManager = ContextCompat.getSystemService(this, TelephonyManager.class);
    if (telephonyManager != null) {
        if ( //권한이 없다면 SecurityException를 발생시킵니다
            ActivityCompat.checkSelfPermission(this, Manifest.permission.READ_PHONE_NUMBERS) != PackageManager.PERMISSION_GRANTED
            && ActivityCompat.checkSelfPermission(this, Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_GRANTED
        ) {
            throw new SecurityException("Permission Denial : requires android.permission.READ_PHONE_NUMBERS, android.permission.READ_PHONE_STATE");
        }else{ //권한이 있다면 getLine1Number()를 반환합니다 (null을 반환할 수 있습니다)
            return telephonyManager.getLine1Number();
        }
    }

    // SystemService 가 null이라면 null을 반환합니다
    return null;
}
```