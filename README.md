#Send-Offline-Notification-In-Android


    //This Method Call in On create() or app startup 
    private void createNotificationChannel(){

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            String channelId = "1";
            CharSequence channelName = "Offline Notifications";
            String description = "Channel for offline notifications";
            int importance = NotificationManager.IMPORTANCE_DEFAULT;

            NotificationChannel channel = new NotificationChannel(channelId, channelName, importance);
            channel.setDescription(description);

            NotificationManager notificationManager = getSystemService(NotificationManager.class);
            notificationManager.createNotificationChannel(channel);
    //This Method Call in On create() or app startup 
    }
    
    // Button clicked to send Notification
     private void sendOfflineNotification(){
 
        // Intent to open an activity when the notification is clicked
        Intent intent = new Intent(this, MainActivity2.class);
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);

        PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_IMMUTABLE);

        // Build the notification
        NotificationCompat.Builder builder = new NotificationCompat.Builder(this, "offline_notification_channel")
                .setSmallIcon(R.drawable.logo) // Replace with your icon
                .setContentTitle("Offline Notification")
                .setContentText("This is a notification triggered offline.")
                .setPriority(NotificationCompat.PRIORITY_DEFAULT)
                .setContentIntent(pendingIntent)
                .setAutoCancel(true); // Dismiss notification on click

        // Show the notification
        NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
        notificationManager.notify(1, builder.build());


    }


    //Notification Send By Schedule or other condition
     private void scheduleNotification(long triggerTime){
     
        Intent intent = new Intent(this, NotificationReceiver.class);
        PendingIntent pendingIntent = PendingIntent.getBroadcast(this, 0, intent, PendingIntent.FLAG_IMMUTABLE);

        AlarmManager alarmManager = (AlarmManager) getSystemService(Context.ALARM_SERVICE);
        alarmManager.setExact(AlarmManager.RTC_WAKEUP, triggerTime, pendingIntent);
        //Notification Send By Schedule or other condition
        
    }

    //Notification permission check
    private void checkNotificationPermission() {
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) { // Android 13+
            if (checkSelfPermission(android.Manifest.permission.POST_NOTIFICATIONS) != PackageManager.PERMISSION_GRANTED) {
                requestPermissionLauncher.launch(android.Manifest.permission.POST_NOTIFICATIONS);
            }
        } else {
            // Permission not required for Android versions below 13
            Toast.makeText(this, "Notification permission not required on this version.", Toast.LENGTH_SHORT).show();
        }
    }

     //Variable diclaration
     //oncreate function
        private ActivityResultLauncher<String> requestPermissionLauncher;

         //>>This is Request permission Launcher
         //>>This code write On create or other Method call by on create()
         requestPermissionLauncher = registerForActivityResult(
                new ActivityResultContracts.RequestPermission(),
                isGranted -> {
                    if (isGranted) {
                        Toast.makeText(this, "Notification permission granted!", Toast.LENGTH_SHORT).show();
                    } else {
                        Toast.makeText(this, "Notification permission denied!", Toast.LENGTH_SHORT).show();
                    }
                }
           );


      }
