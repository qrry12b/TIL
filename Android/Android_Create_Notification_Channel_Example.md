# Android Create Notification Channel Example (알림채널 만들기)

안드로이드 Notification 채널 만들기
```
NotificationManagerCompat managerCompat = NotificationManagerCompat.from(this);

String description = "notify description";

managerCompat.createNotificationChannelGroup(
   new NotificationChannelGroupCompat.Builder("Group1")
       .setName("그룹1")
       .setDescription(description)
       .build()
);

managerCompat.createNotificationChannel(
   new NotificationChannelCompat.Builder("chA", NotificationManagerCompat.IMPORTANCE_DEFAULT)
       .setName("channelA")
       .setDescription(description)
       .build()
);

managerCompat.createNotificationChannel(
   new NotificationChannelCompat.Builder("chB", NotificationManagerCompat.IMPORTANCE_DEFAULT)
               .setName("channelB")
               .setDescription(description)
               .build()
);

managerCompat.createNotificationChannel(
   new NotificationChannelCompat.Builder("chC", NotificationManagerCompat.IMPORTANCE_DEFAULT)
               .setName("channelC")
               .setDescription(description)
               .setGroup("Group1")
               .build()
);

managerCompat.createNotificationChannel(
   new NotificationChannelCompat.Builder("chD", NotificationManagerCompat.IMPORTANCE_DEFAULT)
               .setName("channelD")
               .setDescription(description)
               .setGroup("Group1")
               .build()
);

```