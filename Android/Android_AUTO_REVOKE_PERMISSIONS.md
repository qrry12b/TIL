# Android AUTO REVOKE PERMISSIONS (사용하지 않는 앱의 권한 자동 재설정)

만약 사용자와의 상호작용이 잘 없는 백그라운드에서 실행되는 앱일 경우 ACTION_AUTO_REVOKE_PERMISSIONS 를 호출해서   

사용자가 사용하지 않는 앱의 권한 자동 재설정에서 제외하도록 설정하면 장기간 사용되지 않는 권한에 대해 초기화 되는것을 방지할 수 있습니다.

```
public boolean isAutoRevokeWhitelisted(Context context) {
    if(Build.VERSION.SDK_INT < Build.VERSION_CODES.R) return true;
    return context.getPackageManager().isAutoRevokeWhitelisted();
}

public void requestAutoRevokeWhitelisted(Context context) {
    if(isAutoRevokeWhitelisted(context) == true) return;

    Intent intent = new Intent(Intent.ACTION_AUTO_REVOKE_PERMISSIONS);
    context.startActivity(intent);
} 
```

### REF
* [android-versions-11](https://developer.android.com/about/versions/11/privacy/permissions)