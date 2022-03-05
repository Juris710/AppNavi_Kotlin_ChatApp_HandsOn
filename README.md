# アプリナビ Kotlin HandsOn

## 1.3　UIの形や色を変更する

現状のUIと目標のUIを比較します。左：現状、右：目標<br>

![session1-3-compare-init-result](https://user-images.githubusercontent.com/57338033/156861276-a67d6b0c-1919-469e-9796-50135cffb4a0.png)

フォームやボタンの形・全体の色が違いますね、今回は色や形を設定していきます。<br>

## Statusbarの変更
まずは`Statusbar`・`ActionBar`の色を変更します。<br>
ちなみに`StatusBar`は`handsOnChatApp`と表示されている紫のバーで、`ActionBar`は充電や時間が記載されている少し濃い紫の部分のことを言います。<br>

![session1-3-explain-bar](https://user-images.githubusercontent.com/57338033/156746591-0c2ada74-ccf8-468b-ada1-696f42e43a34.png)

- `res/values/colors.xml`を開きましょう
- `colors.xml`では色を定義することができます。自分で定義することで使い回しやすくなります。
- 以下の行を`colors.xml`の`<resources></resources>`内に追加しましょう

```xml
  <color name="base_color_blue">#0A84FF</color>
```

- `base_color_blue`はこの色の名前でありリソースIDです。`name`で指定した文字列が`color`要素の名前とリソースIDの役割を持ちます。

![session1-3-edit-color-xml](https://user-images.githubusercontent.com/57338033/156861294-caa3091b-5833-4747-8547-8423ead13812.png)

- 次に`res/values/themes/themes.xml`を開きましょう。
- `@color/purple_500`を`@color/base_color_blue`に変更しましょう。
- `?attr/colorPrimryVariant`を`@color/base_color_blue`に変更しましょう。
- 以下に編集後の`theme.xml`を載せておきます

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.HandsOnChatApp" parent="Theme.MaterialComponents.DayNight.DarkActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/base_color_blue</item>
        <item name="colorPrimaryVariant">@color/purple_700</item>
        <item name="colorOnPrimary">@color/white</item>
        <!-- Secondary brand color. -->
        <item name="colorSecondary">@color/teal_200</item>
        <item name="colorSecondaryVariant">@color/teal_700</item>
        <item name="colorOnSecondary">@color/black</item>
        <!-- Status bar color. -->
        <item name="android:statusBarColor" tools:targetApi="l">@color/base_color_blue</item>
        <!-- Customize your theme here. -->
    </style>
</resources>
```
- 編集後の画面は以下の通りです。

![session1-3-changed_primary_color](https://user-images.githubusercontent.com/57338033/156863695-bdc3c365-e783-4626-8e4a-26c46e485ff7.png)

## 入力Formの編集
- まずは`res/drawable`を右クリックし、`New`→`Drawable Resource File`を選択します。
- `edittext_frame.xml`という名前でxmlファイルを作成します。
- 以下の内容を作成したファイルに記入しましょう。

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <shape android:shape="rectangle">
            <stroke
                android:color="@color/base_color_blue"
                android:width="2dp"
                />
            <corners android:radius="25dp"/>
            <solid android:color="@color/white"/>
        </shape>
    </item>
</selector> 
```

- 続いて`activity_register.xml`を開きましょう
- `Attributes`からユーザー名、メールアドレス、パスワードそれぞれの`background`に`@drawable/edittext_frame`を入力します。
- これでFormの見た目を変えることができました。ただ、このままだとヒントが左によりすぎて少しかなり見にくいです。そこでPaddingを設定します。
- `Attributes`からユーザー名、メールアドレス、パスワードそれぞれの`padding left`を`16dp`にします。
- 以下にここまでの画面を表示します

![session1-3-apply-background-from](https://user-images.githubusercontent.com/57338033/156864257-6ae8f3ae-8392-4049-a3cb-af679fcd38c7.png)

## 課題
- 同様の手順で`register_button`、`select_photo_button`という名前でxmlファイルを作成し、`register_button.xml`は`RegisterButton`に、`select_photo_button.xml`は`SelectPhotoButton`の`background`に適用しましょう。
- 以下に`register_button.xml`、`select_photo_button.xml`に記載する内容を載せます。

- `register_button.xml`
```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <shape android:shape="rectangle">
            <corners android:radius="25dp"/>
        </shape>
    </item>
</selector> 
```

- `select_photo_button.xml`
```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item>
        <shape android:shape="rectangle">
            <corners android:radius="75dp"/>
        </shape>
    </item>
</selector> 
```

- 以下のような画面になっていればOKです。`RegisterButton`と`SelectPhotoButton`が丸くなっていれば正解です。

![session1-3-apply-background-select-photo-button](https://user-images.githubusercontent.com/57338033/156864775-6e423c98-09cd-4a82-a2f3-dacb0a4afd8b.png)

# 画像を丸くする
- 現状画像のみが丸くなっておりません。
- 先程のようにxmlファイルで画像を丸くするようにしても画像自体は丸くなりますが、端末に保存されている画像を表示したとき四角い画像になってしまいます。
- そこで[CircleImageView](https://github.com/hdodenhof/CircleImageView)というライブラリを使います。
- まずは`build.gradle(Module)`を開きます。
- 以下の緑のハイライトを`dependencies`内に追加します。

```diff
dependencies {
    ...
+    implementation 'de.hdodenhof:circleimageview:3.1.0'
}

```

- 追加できましたら`Sync Now`を押しましょう。
- 続いて`activity_register`を開きましょう
- ViewModeをCodeモードに変更します。
- `ImageView`を`de.hdodenhof.circleimageview.CircleImageView`に書き換えましょう

```diff
- <ImageView
+ <de.hdodenhof.circleimageview.CircleImageView
      android:id="@+id/circle_view_register"
      略
```

- これで画像を丸くする対応は完了です！

<details>
<summary>前回との差分</summary>
    DiffではActionBarが紫に指定されておりませんが、気にしないでください<br>
    <a href="https://github.com/syota-kawaguchi/AppNavi_Kotlin_ChatApp_HandsOn/commit/8497e7c412a7383d3bc502e9866a00a3c34e504c">diff</a>
</details>

## Next
