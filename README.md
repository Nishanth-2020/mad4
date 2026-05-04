<XML

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:padding="16dp"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/appTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Electricity Bill Calculator"
        android:textSize="22sp"
        android:gravity="center"
        android:textStyle="bold"
        android:padding="12dp"/>

    <EditText
        android:id="@+id/unitsInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter units consumed"
        android:inputType="number"/>

    <EditText
        android:id="@+id/phoneInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter mobile number"
        android:inputType="phone"/>

    <Button
        android:id="@+id/calculateBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Calculate Bill"
        android:padding="10dp"/>

    <TextView
        android:id="@+id/resultView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Bill will appear here"
        android:textSize="18sp"
        android:padding="12dp"
        android:textColor="#333333"/>

    <Button
        android:id="@+id/sendBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Send SMS"
        android:padding="10dp"/>
</LinearLayout>
<MAIN ACTIVITY.MAIN

package com.example.electricitybill;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.telephony.SmsManager;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private EditText unitsInput, phoneInput;
    private Button calculateBtn, sendBtn;
    private TextView resultView;
    private double billAmount = 0.0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        unitsInput = findViewById(R.id.unitsInput);
        phoneInput = findViewById(R.id.phoneInput);
        calculateBtn = findViewById(R.id.calculateBtn);
        sendBtn = findViewById(R.id.sendBtn);
        resultView = findViewById(R.id.resultView);

        // Calculate bill
        calculateBtn.setOnClickListener(v -> {
            String unitsStr = unitsInput.getText().toString();
            if (!unitsStr.isEmpty()) {
                int units = Integer.parseInt(unitsStr);
                billAmount = calculateBill(units);
                String message = "Electricity Bill: ₹" + billAmount;
                resultView.setText(message);
            } else {
                Toast.makeText(this, "Enter units consumed", Toast.LENGTH_SHORT).show();
            }
        });

        // Send SMS
        sendBtn.setOnClickListener(v -> {
            String phone = phoneInput.getText().toString();
            if (!phone.isEmpty() && billAmount > 0) {
                String smsMessage = "Your electricity bill is ₹" + billAmount;
                try {
                    SmsManager smsManager = SmsManager.getDefault();
                    smsManager.sendTextMessage(phone, null, smsMessage, null, null);
                    Toast.makeText(this, "SMS Sent to " + phone, Toast.LENGTH_SHORT).show();
                } catch (Exception e) {
                    Toast.makeText(this, "Failed to send SMS", Toast.LENGTH_SHORT).show();
                }
            } else {
                Toast.makeText(this, "Enter phone number and calculate bill first", Toast.LENGTH_SHORT).show();
            }
        });
    }

    // Simple tariff calculation
    private double calculateBill(int units) {
        double amount = 0;
        if (units <= 100) {
            amount = units * 5; // ₹5 per unit
        } else if (units <= 200) {
            amount = (100 * 5) + ((units - 100) * 7); // ₹7 per unit after 100
        } else {
            amount = (100 * 5) + (100 * 7) + ((units - 200) * 10); // ₹10 per unit after 200
        }
        return amount;
    }
}
