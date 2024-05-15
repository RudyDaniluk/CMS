imageView.setOnDragListener(new View.OnDragListener() {
    @Override
    public boolean onDrag(View v, DragEvent event) {
        switch (event.getAction()) {
            case DragEvent.ACTION_DROP:
                // Pobierz współrzędne przeciąganego obrazka
                float x = event.getX();
                float y = event.getY();

                // Ustaw nowe współrzędne dla ImageView
                RelativeLayout.LayoutParams layoutParams = (RelativeLayout.LayoutParams) imageView.getLayoutParams();
                layoutParams.leftMargin = (int) (x - imageView.getWidth() / 2);
                layoutParams.topMargin = (int) (y - imageView.getHeight() / 2);
                imageView.setLayoutParams(layoutParams);

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
