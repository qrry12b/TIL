# Get battery level and state in Android (안드로이드 배터리 퍼센트 얻기)

호출 시점에서의 배터리 퍼센트를 얻습니다.   
SDK 21 이상일 경우 registerReceiver에 Receiver 를 전달할 경우 상태가 변경될때 수신할 수 있습니다. 

registReceiver에 Receiver 대신 null을 전달해도 반환값으로 정보를 얻을 수 있습니다.

```
public int getBatteryPercentage() {
    if (Build.VERSION.SDK_INT >= 21) {

        BatteryManager bm = (BatteryManager) context.getSystemService(BATTERY_SERVICE);
        return bm.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY);

    } else {

        IntentFilter iFilter = new IntentFilter(Intent.ACTION_BATTERY_CHANGED);
        Intent batteryStatus = context.registerReceiver(null, iFilter);

        int level = -1, scale = -1;
        if(batteryStatus != null) {
            batteryStatus.getIntExtra(BatteryManager.EXTRA_LEVEL, -1);
            batteryStatus.getIntExtra(BatteryManager.EXTRA_SCALE, -1);
        }

        double batteryPct = level / (double) scale;
        return (int) (batteryPct * 100);
    }
}
```

### REF
* [Stack Overflow-a-42327441](https://stackoverflow.com/a/42327441)