import android.content.ClipData;
import android.os.Bundle;
import android.view.DragEvent;
import android.view.MotionEvent;
import android.view.View;
import android.widget.ImageView;
import androidx.appcompat.app.AppCompatActivity;

public class DragDropActivity extends AppCompatActivity implements View.OnTouchListener, View.OnDragListener {

    private ImageView imageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_drag_drop);

        imageView = findViewById(R.id.imageView);
        imageView.setOnTouchListener(this);

        findViewById(R.id.container).setOnDragListener(this);
    }

    @Override
    public boolean onTouch(View v, MotionEvent event) {
        if (event.getAction() == MotionEvent.ACTION_DOWN) {
            ClipData clipData = ClipData.newPlainText("", "");
            View.DragShadowBuilder shadowBuilder = new View.DragShadowBuilder(v);
            v.startDrag(clipData, shadowBuilder, v, 0);
            return true;
        }
        return false;
    }

    @Override
    public boolean onDrag(View v, DragEvent event) {
        int action = event.getAction();
        switch (action) {
            case DragEvent.ACTION_DROP:
                View view = (View) event.getLocalState();
                // Pozycja dotknięcia na ekranie
                int x = (int) event.getX();
                int y = (int) event.getY();
                // Ustawienie nowej pozycji obrazka
                view.setX(x - view.getWidth() / 2);
                view.setY(y - view.getHeight() / 2);
                view.setVisibility(View.VISIBLE);
                break;
            case DragEvent.ACTION_DRAG_ENDED:
                // W przypadku zakończenia przeciągania, ustaw widoczność obrazka na widoczny
                if (!event.getResult()) {
                    ((View) event.getLocalState()).setVisibility(View.VISIBLE);
                }
                break;
        }
        return true;
    }
}
