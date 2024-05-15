<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/placeholder_image"
        android:layout_centerInParent="true"
        android:contentDescription="@string/image_description" />

</RelativeLayout>
.
java 
.
.

import android.content.ClipData;
import android.os.Bundle;
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

        imageView.setOnDragListener(new View.OnDragListener() {
            @Override
            public boolean onDrag(View v, DragEvent event) {
                switch (event.getAction()) {
                    case DragEvent.ACTION_DROP:
                        ImageView draggedImageView = (ImageView) event.getLocalState();
                        ImageView targetImageView = (ImageView) v;
                        targetImageView.setImageDrawable(draggedImageView.getDrawable());
                        break;
                }
                return true;
            }
        });
    }
}
.

 .
 .
 imageView.setOnDragListener(new View.OnDragListener() {
    @Override
    public boolean onDrag(View v, DragEvent event) {
        switch (event.getAction()) {
            case DragEvent.ACTION_DROP:
                // Pobierz współrzędne przeciąganego obrazka
                float x = event.getX();
                float y = event.getY();

                // Ustaw nowe współrzędne dla ImageView
                imageView.setX(x - imageView.getWidth() / 2);
                imageView.setY(y - imageView.getHeight() / 2);
                
                // Sprawdź czy ImageView wychodzi poza granice rodzica i dostosuj je w razie potrzeby
                int parentWidth = ((View) imageView.getParent()).getWidth();
                int parentHeight = ((View) imageView.getParent()).getHeight();
                if (imageView.getX() < 0) {
                    imageView.setX(0);
                } else if (imageView.getX() + imageView.getWidth() > parentWidth) {
                    imageView.setX(parentWidth - imageView.getWidth());
                }
                if (imageView.getY() < 0) {
                    imageView.setY(0);
                } else if (imageView.getY() + imageView.getHeight() > parentHeight) {
                    imageView.setY(parentHeight - imageView.getHeight());
                }
                
                break;
            case DragEvent.ACTION_DRAG_ENDED:
                if (!event.getResult()) {
                    // Obsługa sytuacji, gdy przeciąganie jest anulowane lub nie powiedzie się
                    // Możesz tu wykonać odpowiednie czynności, np. przywrócenie oryginalnego obrazka
                }
                break;
        }
        return true;
    }
});
