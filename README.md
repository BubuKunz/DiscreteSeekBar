# DiscreteSeekBar
Example of discrete seekbar implemented by native tools

![](https://github.com/BubuKunz/DiscreteSeekBar/blob/master/discrete_seek_bar_step_0.jpg)
![](https://github.com/BubuKunz/DiscreteSeekBar/blob/master/disrete_seek_bar_step_3.jpg)

###TextThumbSeekBar.class
```java
public class TextThumbSeekBar extends AppCompatSeekBar {

    private int mThumbSize;
    private TextPaint mTextPaint;

    public TextThumbSeekBar(Context context) {
        this(context, null);
    }

    public TextThumbSeekBar(Context context, AttributeSet attrs) {
        this(context, attrs, android.R.attr.seekBarStyle);
    }

    public TextThumbSeekBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);

        mThumbSize = getResources().getDimensionPixelSize(R.dimen.thumb_size);

        mTextPaint = new TextPaint();
        mTextPaint.setColor(getResources().getColor(R.color.blue_bayoux));
        mTextPaint.setTextSize(getResources().getDimensionPixelSize(R.dimen.thumb_text_size));
        mTextPaint.setTypeface(Typeface.DEFAULT_BOLD);
        mTextPaint.setTextAlign(Paint.Align.CENTER);
    }

    @Override
    protected synchronized void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        String progressText = String.valueOf(getProgress());
        Rect bounds = new Rect();
        mTextPaint.getTextBounds(progressText, 0, progressText.length(), bounds);
        int leftPadding = getPaddingLeft() - getThumbOffset();
        int rightPadding = getPaddingRight() - getThumbOffset();
        int width = getWidth() - leftPadding - rightPadding;
        float progressRatio = (float) getProgress() / getMax();
        float thumbOffset = mThumbSize * (.5f - progressRatio);
        float thumbX = progressRatio * width + leftPadding + thumbOffset;
        float thumbY = getHeight() / 2f + bounds.height() / 2f;
        canvas.drawText(progressText, thumbX, thumbY, mTextPaint);
    }
}
```

**custom_seekbar_progress.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@android:id/background"
        android:gravity="center_vertical|fill_horizontal">
        <shape android:shape="rectangle"
            android:tint="@color/color_gray">
            <size android:height="4dp" />
            <solid android:color="@color/color_gray" />
        </shape>
    </item>
    <item android:id="@android:id/progress"
        android:gravity="center_vertical|fill_horizontal">
        <scale android:scaleWidth="100%">
            <selector>
                <item android:state_enabled="false"
                    android:drawable="@android:color/transparent" />
                <item>
                    <shape android:shape="rectangle"
                        android:tint="@color/white">
                        <size android:height="4dp" />
                        <solid android:color="@color/white" />
                    </shape>
                </item>
            </selector>
        </scale>
    </item>
</layer-list>
```

**seekbar_custom_tick.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">
    <solid android:color="@android:color/white"/>
    <size
        android:width="10dp"
        android:height="10dp"/>
    <stroke
        android:width="1dp"
        android:color="@color/color_gray"/>
</shape>
```

**shape_seek_bar_text_thumb.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">
    <solid android:color="@android:color/white"/>
    <size
        android:width="18dp"
        android:height="18dp"/>
    <stroke
        android:width="1dp"
        android:color="@color/color_gray"/>
</shape>
```

###In style.xml
```xml
<style name="InvestSeekBarTheme" parent="Widget.AppCompat.SeekBar">
    <item name="android:progressBackgroundTint">@color/color_gray</item>
    <item name="android:progressTint">@color/white</item>
</style>
```

###And finaly in your layour
```xml
<com.yourpackage.TextThumbSeekBar
        android:layout_width="220dp"
        android:layout_height="wrap_content"
        android:max="5"
        android:progress="0"
        style="@style/InvestSeekBarTheme"
        android:tickMark="@drawable/seekbar_custom_tick"
        android:progressDrawable="@drawable/custom_seekbar_progress"
        android:thumb="@drawable/shape_seek_bar_text_thumb" />
```
