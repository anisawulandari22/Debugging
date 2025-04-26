#activity_main#
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/website_edit_text"
            android:text="@string/edit_website"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/open_website_button"
            android:layout_marginBottom="24dp"
            android:text="@string/button_website"
            android:onClick="openWebsite"/>
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/location_edit_text"
            android:text="@string/edit_location"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/location_button"
            android:layout_marginBottom="24dp"
            android:text="@string/button_location"
            android:onClick="openLocation"/>
        <EditText
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:id="@+id/share_edit_text"
            android:text="@string/edit_location"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:id="@+id/share_text_button"
            android:layout_marginBottom="24dp"
            android:text="@string/button_share"
            android:onClick="ShareText"/>
    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>

#MainActivity.kt#

package com.example.implicit

import android.content.Intent
import android.net.Uri
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import android.widget.Button
import android.widget.EditText
import android.widget.Toast

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val websiteEdit: EditText = findViewById(R.id.website_edit_text)
        val openWebsiteButton: Button = findViewById(R.id.open_website_button)
        openWebsiteButton.setOnClickListener {
            val websiteUrl = websiteEdit.text.toString()
            openWebsite(websiteUrl)
        }

        val locationEditText: EditText = findViewById(R.id.location_edit_text)
        val locationButton: Button = findViewById(R.id.location_button)
        locationButton.setOnClickListener {
            val locationName = locationEditText.text.toString()
            openLocation(locationName)
        }

        val shareEdit: EditText = findViewById(R.id.share_edit_text)
        val shareTextButton: Button = findViewById(R.id.share_text_button)
        shareTextButton.setOnClickListener {
            val text = shareEdit.text.toString()
            Log.v("cek string", text)
            Toast.makeText(applicationContext, text, Toast.LENGTH_SHORT).show()
            shareText(text)
        }
    }

    private fun openWebsite(url: String) {
        val intent = Intent(Intent.ACTION_VIEW, Uri.parse(url))
        try {
            startActivity(intent)
        } catch (e: Exception) {
            Log.e("ImplicitIntent", "Gagal membuka website: ${e.message}")
            Toast.makeText(this, "Tidak dapat membuka website.", Toast.LENGTH_SHORT).show()
        }
    }

    private fun openLocation(location: String) {
        val uri = Uri.parse("geo:0,0?q=$location")
        val intent = Intent(Intent.ACTION_VIEW, uri)
        intent.setPackage("com.google.android.apps.maps")
        try {
            startActivity(intent)
        } catch (e: Exception) {
            Log.e("ImplicitIntent", "Gagal membuka lokasi: ${e.message}")
            Toast.makeText(this, "Google Maps tidak terinstal atau tidak dapat membuka lokasi.", Toast.LENGTH_SHORT).show()
        }
    }

    private fun shareText(text: String) {
        val intent = Intent(Intent.ACTION_SEND)
        intent.type = "text/plain"
        intent.putExtra(Intent.EXTRA_TEXT, text)
        try {
            startActivity(Intent.createChooser(intent, "Share Text"))
        } catch (e: Exception) {
            Log.e("ImplicitIntent", "Gagal berbagi teks: ${e.message}")
            Toast.makeText(this, "Tidak ada aplikasi yang tersedia untuk berbagi teks.", Toast.LENGTH_SHORT).show()
        }
    }
}

#strings.xml#
<resources>
    <string name="app_name">Implicit</string>
    <string name="button_website">Open Website</string>
    <string name="button_location">Open Map</string>
    <string name="button_share">Share Text</string>
    <string name="edit_website">http://developer.android.com</string>
    <string name="edit_location">Universitas Teknologi Bandung</string>
    <string name="edit_share">Ini text yang akan dishare</string>
</resources>
