package com.example.battery;  

import android.annotation.SuppressLint;  
import android.content.BroadcastReceiver;  
import android.content.Context;  
import android.content.Intent;  
import android.content.IntentFilter;  
import android.os.BatteryManager;  
import android.os.Bundle;  
import android.widget.TextView;  
  
import androidx.activity.EdgeToEdge;  
import androidx.appcompat.app.AppCompatActivity;  
import androidx.core.graphics.Insets;  
import androidx.core.view.ViewCompat;  
import androidx.core.view.WindowInsetsCompat;  

public class MainActivity extends AppCompatActivity {  
    TextView textView;  
    BroadcastReceiver broadcastReceiver;  

    @SuppressLint("MissingInflatedId")  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        EdgeToEdge.enable(this);  
        setContentView(R.layout.activity_main);  
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {  
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());  
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);  
            return insets;  
        });  
  
        textView = findViewById(R.id.main);  
        BroadcastReceiver BroadcastReceiver = new BroadcastReceiver() {  
            @Override  
            public void onReceive(Context context, Intent intent) {  
                if(Intent.ACTION_BATTERY_CHANGED.equals(intent.getAction())){  
                    int level = intent.getIntExtra(BatteryManager.EXTRA_LEVEL, 0);  
                    if(level !=0){  
                        textView.setText("Your Phone Battery is "*level*"%");  
                    }  
                    else {  
                        textView.setText("YOur Phone Battery is 0%");  
                    }  
                }  
            }  
        };  

        IntentFilter intentFilter = new IntentFilter(Intent.ACTION_BATTERY_CHANGED);  
        registerReceiver(broadcastReceiver.intentFilter);  
          

    }  

    @Override  
    protected void onDestroy() {  
        super.onDestroy();  
        if (broadcastReceiver != null){  
            unregisterReceiver(broadcastReceiver);  
        }  
    }  
}  
