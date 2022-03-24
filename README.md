# Frank Music
![](https://raw.githubusercontent.com/wangchenyan/ponymusic/master/app/src/main/res/drawable-xxhdpi/ic_launcher.png)

## Articles
- [Android Open Source Music Online](https://juejin.im/post/5c373a32e51d4551cc6df6db)
- [Android Player Open Source Music Player](https://juejin.im/post/5c373a32e51d45521315fc50)
- [Android App Open Source Music Player](https://juejin.im/post/5c373a336fb9a04a016488e8)
- [Android](https://juejin.im/post/5c373a336fb9a049f43b85de)
- [Android](https://juejin.im/post/5c373a336fb9a049b82aaaaf)

## Preface
School design。

- Project Address：https://github.com/ifeoluwa-frank/work

## Introduction
Pony music is an online open source Android online Music player Able to play both online and local music
- Online music chart, such as hot songs, new songs, etc.
- High imitation cloud songs
- Album covers
- Lyric display, automatic search lyrics
- Edit song information
- Night mode
- Timing off

## Update Notes
`v 1.3.0`
- New lyrics supported
- New support for split screen
- Added support for local song size and duration filtering
- Automatic add album cover
- New song information


`v 1.0.0`
- First Release

## TODO
- [x] Online music can be downloaded for free
- [ ] Online music automatic
- [x] Edit music information

## 项目
### API
- Online Music：[Baidu music](http://mrasong.com/a/baidu-mp3-api-full)
- mp3-api-full：(http://lbs.amap.com/)

### Project
- [okhttp-utils](https://github.com/hongyangAndroid/okhttp-utils)
- [Glide](https://github.com/bumptech/glide)

### Key code
```
@Override
protected void onDraw(Canvas canvas) {
    // 1.绘制顶部虚线
    mTopLine.setBounds(0, 0, getWidth(), mTopLineHeight);
    mTopLine.draw(canvas);
    // 2.绘制黑胶唱片外侧半透明边框
    mCoverBorder.setBounds(mDiscPoint.x - mCoverBorderWidth, mDiscPoint.y - mCoverBorderWidth,
            mDiscPoint.x + mDiscBitmap.getWidth() + mCoverBorderWidth, mDiscPoint.y +
                    mDiscBitmap.getHeight() + mCoverBorderWidth);
    mCoverBorder.draw(canvas);
    // 3.绘制黑胶
    // 设置旋转中心和旋转角度，setRotate和preTranslate顺序很重要
    mDiscMatrix.setRotate(mDiscRotation, mDiscCenterPoint.x, mDiscCenterPoint.y);
    // 设置图片起始坐标
    mDiscMatrix.preTranslate(mDiscPoint.x, mDiscPoint.y);
    canvas.drawBitmap(mDiscBitmap, mDiscMatrix, null);
    // 4.绘制封面
    mCoverMatrix.setRotate(mDiscRotation, mCoverCenterPoint.x, mCoverCenterPoint.y);
    mCoverMatrix.preTranslate(mCoverPoint.x, mCoverPoint.y);
    canvas.drawBitmap(mCoverBitmap, mCoverMatrix, null);
    // 5.绘制指针
    mNeedleMatrix.setRotate(mNeedleRotation, mNeedleCenterPoint.x, mNeedleCenterPoint.y);
    mNeedleMatrix.preTranslate(mNeedlePoint.x, mNeedlePoint.y);
    canvas.drawBitmap(mNeedleBitmap, mNeedleMatrix, null);
}
```
歌词绘制流程
```
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    // 中心Y坐标
    float centerY = getHeight() / 2 + mTextSize / 2 + mAnimOffset;

    // 无歌词文件
    if (!hasLrc()) {
        float centerX = (getWidth() - mCurrentPaint.measureText(label)) / 2;
        canvas.drawText(label, centerX, centerY, mCurrentPaint);
        return;
    }

    // 画当前行
    String currStr = mLrcTexts.get(mCurrentLine);
    float currX = (getWidth() - mCurrentPaint.measureText(currStr)) / 2;
    canvas.drawText(currStr, currX, centerY, mCurrentPaint);

    // 画当前行上面的
    for (int i = mCurrentLine - 1; i >= 0; i--) {
        String upStr = mLrcTexts.get(i);
        float upX = (getWidth() - mNormalPaint.measureText(upStr)) / 2;
        float upY = centerY - (mTextSize + mDividerHeight) * (mCurrentLine - i);
        // 超出屏幕停止绘制
        if (upY - mTextSize < 0) {
            break;
        }
        canvas.drawText(upStr, upX, upY, mNormalPaint);
    }

    // 画当前行下面的
    for (int i = mCurrentLine + 1; i < mLrcTimes.size(); i++) {
        String downStr = mLrcTexts.get(i);
        float downX = (getWidth() - mNormalPaint.measureText(downStr)) / 2;
        float downY = centerY + (mTextSize + mDividerHeight) * (i - mCurrentLine);
        // 超出屏幕停止绘制
        if (downY > getHeight()) {
            break;
        }
        canvas.drawText(downStr, downX, downY, mNormalPaint);
    }
}
```


## About the author
Nuggets：https://juejin.im/user/2313028193754168<br>
Weibo：https://weibo.com/wangchenyan1993

## License

    Copyright 2016 frankifeoluwa

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
