<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <RelativeLayout
        android:id="@+id/relativeLayout1"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:background="@android:color/darker_gray"
        android:layout_marginTop="100dp"
        android:layout_marginStart="50dp">

        <!-- Pierwszy RelativeLayout -->
        <ImageView
            android:id="@+id/imageView"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:src="@drawable/placeholder_image"
            android:scaleType="fitCenter"
            android:contentDescription="@string/image_description" />

    </RelativeLayout>

    <RelativeLayout
        android:id="@+id/relativeLayout2"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:background="@android:color/darker_gray"
        android:layout_marginTop="100dp"
        android:layout_marginEnd="50dp"
        android:layout_alignParentEnd="true">

        <!-- Drugi RelativeLayout -->
    </RelativeLayout>

</RelativeLayout>

.
import android.os.Bundle;
import android.view.DragEvent;
import android.view.MotionEvent;
import android.view.View;
import android.widget.ImageView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    private ImageView imageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView = findViewById(R.id.imageView);

        imageView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                if (event.getAction() == MotionEvent.ACTION_DOWN) {
                    ClipData clipData = ClipData.newPlainText("", "");
                    View.DragShadowBuilder shadowBuilder = new View.DragShadowBuilder(v);
                    v.startDragAndDrop(clipData, shadowBuilder, v, 0);
                    return true;
                } else {
                    return false;
                }
            }
        });

        findViewById(R.id.relativeLayout2).setOnDragListener(new View.OnDragListener() {
            @Override
            public boolean onDrag(View v, DragEvent event) {
                switch (event.getAction()) {
                    case DragEvent.ACTION_DROP:
                        // Uzyskaj referencję do obrazka przeciąganego
                        ImageView draggedImageView = (ImageView) event.getLocalState();

                        // Usuń obrazek z pierwotnego RelativeLayout
                        ((RelativeLayout) draggedImageView.getParent()).removeView(draggedImageView);

                        // Dodaj obrazek do docelowego RelativeLayout
                        RelativeLayout.LayoutParams layoutParams = new RelativeLayout.LayoutParams(
                                RelativeLayout.LayoutParams.WRAP_CONTENT,
                                RelativeLayout.LayoutParams.WRAP_CONTENT
                        );
                        layoutParams.addRule(RelativeLayout.CENTER_IN_PARENT, RelativeLayout.TRUE);
                        ((RelativeLayout) v).addView(draggedImageView, layoutParams);

                        draggedImageView.setVisibility(View.VISIBLE);

                        break;
                }
                return true;
            }
        });
    }
}
