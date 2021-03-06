### TextView 的 lineSpacingExtra 是否作用最后一行？

```
if (sdk >= 21) {
	yes;
} else {
	no;
}
```
<https://android.googlesource.com/platform/frameworks/base/+/936df68%5E!/>

一些国产 Rom 不适用上面的情况，魅族手机 android 6.0 和 vivo 手机的 android 5.0 机型效果都和 sdk21 以前的版本一样：lineSpacingExtra 会作用最后一行。所以不能根据 sdk 版本判断，要通过代码判断是否会作用最后一行。

判断的具体代码如下：

```
    @Override
    protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
        super.onLayout(changed, left, top, right, bottom);
        Paint.FontMetricsInt fontMetricsInt = getPaint().getFontMetricsInt();
        Layout layout = getLayout();
        if (layout == null) {
            return;
        }
        // affect case: fontMetricsInt.descent + layout.getSpacingAdd() + layout.getBottomPadding() * 2
        // == layout.getLineDescent(layout.getLineCount() - 1)
        boolean notAffect = fontMetricsInt.descent + layout.getBottomPadding() == layout.getLineDescent(layout.getLineCount() - 1);
    }
```