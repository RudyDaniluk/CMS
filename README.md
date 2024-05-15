<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/your_image"
        android:layout_centerInParent="true"
        android:adjustViewBounds="true"
        android:tag="draggable" />

</RelativeLayout>
import android.graphics.Rect;
import android.os.Bundle;
import android.view.MotionEvent;
import android.view.View;
import android.widget.ImageView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity implements View.OnTouchListener {

    private ImageView imageView;
    private int xDelta;
    private int yDelta;
    private Rect hitRect = new Rect();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView = findViewById(R.id.imageView);
        imageView.setOnTouchListener(this);
    }

    @Override
    public boolean onTouch(View view, MotionEvent event) {
        final int x = (int) event.getRawX();
        final int y = (int) event.getRawY();

        switch (event.getAction() & MotionEvent.ACTION_MASK) {
            case MotionEvent.ACTION_DOWN:
                RelativeLayout.LayoutParams lParams = (RelativeLayout.LayoutParams) view.getLayoutParams();
                xDelta = x - lParams.leftMargin;
                yDelta = y - lParams.topMargin;
                break;
            case MotionEvent.ACTION_MOVE:
                RelativeLayout.LayoutParams layoutParams = (RelativeLayout.LayoutParams) view.getLayoutParams();
                layoutParams.leftMargin = x - xDelta;
                layoutParams.topMargin = y - yDelta;
                layoutParams.rightMargin = 0;
                layoutParams.bottomMargin = 0;
                view.setLayoutParams(layoutParams);
                break;
            case MotionEvent.ACTION_UP:
                view.getHitRect(hitRect);
                if (hitRect.contains(x, y)) {
                    // Tutaj możesz dodać obsługę upuszczenia w określonym miejscu
                    // Na przykład wywołaj metodę do obsługi upuszczenia
                }
                break;
        }
        return true;
    }
}
