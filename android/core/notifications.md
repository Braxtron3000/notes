[guide](https://developer.android.com/training/notify-user/build-notification)


# create a notification

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

        NotificationCompat.Builder builder = new NotificationCompat.Builder(Mainactivity.this, getString(R.string.channel_name))
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


# adding actions

## adding a tap action

```
 //set the notification's tap action
        Intent intent = new Intent(this,MainActivity.class);
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
        PendingIntent pendingIntent = PendingIntent.getActivity(this,0,intent,0);
```

then add this to the notificationCompat.builder constructor

```
//Set intent that fires when notification is tapped
                .setContentIntent(pendingIntent)
                .setAutoCancel(true)
```

## creating action buttons

```
//set an action button
        Intent actionIntent = new Intent(this, MainActivity.class); //MainActivity.class needs to be set to a broadcast receiver class
        actionIntent.setAction("big action");
        actionIntent.putExtra("extraID",0);
        //getBroadcast method does some weird background stuff
        PendingIntent actionIntentPending = PendingIntent.getBroadcast(this,0,actionIntent,0);
```
see this [link](https://developer.android.com/guide/components/broadcasts) for details on a broadcast receiver class. (app functioning in the background)
then add this to the NotificationCompat.Builder constructor
```
.addAction(R.drawable.image2,"action BOI",actionIntentPending);
```