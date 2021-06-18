[guide](https://developer.android.com/training/notify-user/build-notification)

## calling the functions

```
createNotificationChannel();
        findViewById(R.id.imageButton).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                createNotification();
            }
        });
```

## creating the notification
```
public void createNotification() {

        NotificationCompat.Builder builder = new NotificationCompat.Builder(this, getString(R.string.channel_name))
                .setSmallIcon(R.drawable.image)
                .setContentTitle("HELLO WORLD")
                .setContentText("here's what I'm saying")
                .setPriority(NotificationCompat.PRIORITY_DEFAULT);

        NotificationManagerCompat notificationManager = NotificationManagerCompat.from(this);

// notificationId is a unique int for each notification that you must define
        notificationManager.notify(1234, builder.build());

    }
```

## registering it in the channel
the notification won't work without being registered.

```
public void createNotificationChannel(){
        if(Build.VERSION.SDK_INT>=Build.VERSION_CODES.O){
            CharSequence name = getString(R.string.channel_name);
            String description = "description";
            int importance = NotificationManager.IMPORTANCE_DEFAULT;
            NotificationChannel channel = new NotificationChannel(getString(R.string.channel_name),"noteChannel",NotificationManager.IMPORTANCE_DEFAULT);
            channel.setDescription(description);

            NotificationManager notificationManager = getSystemService(NotificationManager.class);
            notificationManager.createNotificationChannel(channel);
        }
    }
```

## creating action buttons

