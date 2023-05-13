8.AIM: Create a user registration application that stores the user details in a database table.

XML CODE:

activity_main.xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:orientation="vertical"
    android:layout_height="match_parent"
    android:gravity="center"
    tools:context=".MainActivity">

    <EditText
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:id="@+id/edt_name"
        android:hint="Name"/>

    <EditText
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:id="@+id/edt_email"
        android:hint="Email"
        />
    <EditText
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:id="@+id/edt_pass"
        android:hint="Password"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/btn_register"
        android:layout_marginTop="20dp"
        android:text="Register"/>
</LinearLayout>

JAVA CODE:

MainActivity.java

package com.example.program8;

import androidx.appcompat.app.AppCompatActivity;
import android.content.ContentValues;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {
    EditText nameEditText, emailEditText, passwordEditText;
    Button registerButton;
    MyDB Mydb;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        nameEditText = findViewById(R.id.edt_name);
        emailEditText = findViewById(R.id.edt_email);
        passwordEditText = findViewById(R.id.edt_pass);
        registerButton = findViewById(R.id.btn_register);

        Mydb = new MyDB(this);

        registerButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String name = nameEditText.getText().toString();
                String email = emailEditText.getText().toString();
                String password = passwordEditText.getText().toString();

                SQLiteDatabase db = Mydb.getWritableDatabase();
                ContentValues values = new ContentValues();
                values.put("name", name);
                values.put("email", email);
                values.put("password", password);
                db.insert("users", null, values);
                Toast.makeText(MainActivity.this, "Registration successful!", Toast.LENGTH_SHORT).show();
            }
        });
    }
}

MyDB.java

package com.example.program8;

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class MyDB extends SQLiteOpenHelper {
    private static final int DATABASE_VERSION = 1;
    private static final String DATABASE_NAME = "users.db";

    public MyDB(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        String createTable = "CREATE TABLE users (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, email TEXT, password TEXT)";
        db.execSQL(createTable);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS users");
        onCreate(db);
    }
}
