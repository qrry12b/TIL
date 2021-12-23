# Android Progress Notification Example (프로그레스 알림)

```
String channelId = "downloadProgress";
int notificationId = (int)(Math.random() * Integer.MAX_VALUE);

NotificationManagerCompat notificationManagerCompat = NotificationManagerCompat.from(this);

NotificationChannelCompat notificationChannelCompat =
    new NotificationChannelCompat.Builder(channelId, NotificationManagerCompat.IMPORTANCE_DEFAULT)
    .setName("다운로드 상태")
    .build();

notificationManagerCompat.createNotificationChannel(notificationChannelCompat);

NotificationCompat.Builder builder = new NotificationCompat.Builder(this, notificationChannelCompat.getId())
    .setSmallIcon(R.drawable.ic_launcher_foreground)
    .setTicker("Download...")
    .setContentTitle("Download...")
    .setPriority(NotificationManagerCompat.IMPORTANCE_DEFAULT)
    .setContentText("File 1 (1 / 25)")
    .setProgress(100, 0, false)
    .setAutoCancel(false);

notificationManagerCompat.notify(notificationId, builder.build());

new Thread(() -> {
    int max = 100, progress = 0;

    try{
        while(progress < max) {
            Thread.sleep(500);
            progress += 1;
            builder.setProgress(max, progress, false);
            builder.setContentText("File "+progress+" ("+progress+" / "+max+")");
            notificationManagerCompat.notify(notificationId, builder.build());
        }

        builder.setContentText("Complete");
        builder.setProgress(0, 0, false);
        builder.setAutoCancel(true);
        notificationManagerCompat.notify(notificationId, builder.build());

    }catch(InterruptedException e) {
        e.printStackTrace();
    }
}).start();
```