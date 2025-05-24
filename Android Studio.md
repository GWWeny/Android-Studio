## 实验作业1：

编制一个"我的故乡"图册，下方配有说明文字，单击"上一张""下一张"按钮，图片切换时说明文字也随之切换，且"上一张""下一张"按钮的下方显示前一张及后一张的图片缩略图，如下图，图1所示。



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">
    <!-- RelativeLayout允许子视图相对于其他视图或父布局进行定位。整个布局的宽度和高度都设置为match_parent，表示他会填满父布局的整个区域，并设置了16dp的内边距，xmlns:android="http://schemas.android.com/apk/res/android"是一个 XML 命名空间声明-->
    
<!-- 子视图区域 -->
    
    <!-- 图片区域 -->
    <ImageView
        android:id="@+id/imageView"
        android:layout_width="match_parent"
        android:layout_height="300dp"
        android:scaleType="centerCrop"
        android:src="@drawable/sample_food" />
<!-- 使用ImageView显示一张图片，其宽度为match_parent（填满父布局宽度），高度为300dp,设置了scaleType="centerCrop"，表示图片会按比例缩放并裁剪以适应ImageView的大小,图片资源通过android:src="@drawable/sample_food"指定，表示从drawable资源文件夹中加载sample_food图片 -->
   
    <!-- 图片文字说明 -->
    <TextView
        android:id="@+id/textViewDescription"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/imageView"
        android:layout_marginTop="16dp"
        android:text="图片文字说明"
        android:textSize="18dp" />
<!-- 使用TextView显示一段文本，其宽度为match_parent，高度为wrap_content（包裹内容）,通过android:layout_below="@id/imageView"属性，将该文本定位在图片下方，并设置了16dp的上边距,文本内容为“图片文字说明”，字体大小为18dp（这里应该是sp，dp用于尺寸，sp用于字体大小） -->
    
    <!-- 上一张和下一张按钮区域 -->
    <LinearLayout
        android:id="@+id/buttonViewDescription"
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/textViewDescription"
        android:layout_marginTop="16dp"
        android:gravity="center">
<!-- 使用LinearLayout水平排列两个按钮，其宽度为match_parent，高度为wrap_content,通过android:layout_below="@id/textViewDescription"属性，将该按钮区域定位在文本说明下方，并设置了16dp的上边距。 -->
        
        <Button
            android:id="@+id/buttonPrevious"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="上一张" />

        <Button
            android:id="@+id/buttonNext"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="16dp"
            android:text="下一张" />
<!-- ,两个按钮分别显示“上一张”和“下一张”文字，用于实现图片切换功能。第二个按钮通过android:layout_marginStart="16dp"设置了与第一个按钮的水平间距。-->        
    </LinearLayout>

    <!-- 缩略图区域 -->

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/buttonViewDescription"
        android:layout_marginTop="16dp"
        android:gravity="center"
        android:orientation="horizontal">
<!-- 使用LinearLayout水平排列两个缩略图ImageView，其宽度为match_parent，高度为wrap_content,通过android:layout_below="@+id/buttonViewDescription"属性，将该缩略图区域定位在按钮区域下方，并设置了16dp的上边距。 -->
        
        
        <ImageView
            android:id="@+id/imageViewThumbnailPrevious"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:src="@drawable/sample_thumbnail" />

        <ImageView
            android:id="@+id/imageViewThumbnailNext"
            android:layout_width="100dp"
            android:layout_height="100dp"
            android:layout_marginStart="16dp"
            android:src="@drawable/sample_manager" />
<!-- 两个缩略图的宽度和高度均为100dp，分别显示了sample_thumbnail和sample_manager图片资源。 -->
    </LinearLayout>

</RelativeLayout>
```



### MainActivity.java

```java
package com.example.first; //表示这个应用的唯一标识。

import android.os.Bundle; //用于保存和恢复活动的状态。
import android.view.View; //用于操作视图。
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;
//用于操作按钮、图片视图和文本视图。
import androidx.core.graphics.Insets; //用于处理系统栏的内边距。

import androidx.activity.EdgeToEdge; //用于实现边缘到边缘的布局。
import androidx.appcompat.app.AppCompatActivity; //继承自AppCompatActivity，提供对旧版本Android的兼容性。
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
//用于处理窗口内边距的兼容性。

//MainActivity是一个继承自AppCompatActivity的类，表示这是一个Android活动。
public class MainActivity extends AppCompatActivity {

    private int currentIndex = 0; // 当前图片索引
    private int[] imageResources = {
            R.drawable.sample_food, // 替换为实际的图片资源 ID
            R.drawable.sample_thumbnail,
            R.drawable.sample_manager
    };
    private String[] descriptions = {
            "这是我某个同学疯狂星期四的结果",
            "这是我某个同学吃完KFC去商场买零食的图片",
            "这是与此同时我正在做的Android Studio"
    };

    //onCreate方法是活动生命周期的一部分，用于初始化活动。
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this); //启用边缘到边缘的布局，使应用内容可以延伸到屏幕边缘。
        setContentView(R.layout.activity_main); //设置当前活动的布局文件为activity_main.xml

        //使用findViewById方法获取布局文件中定义的视图组件（ImageView、TextView、Button）。
        final ImageView imageView = findViewById(R.id.imageView);
        final TextView textViewDescription = findViewById(R.id.textViewDescription);
        final Button buttonPrevious = findViewById(R.id.buttonPrevious);
        final Button buttonNext = findViewById(R.id.buttonNext);

        // 设置按钮点击事件,为“上一张”按钮（buttonPrevious）
        buttonPrevious.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //点击“上一张”按钮时，currentIndex减1，如果小于0，则通过+ imageResources.length实现循环。
                currentIndex = (currentIndex - 1 + imageResources.length) % imageResources.length;
                //每次点击按钮后，调用updateImageViewAndDescription方法更新图片和描述。
                updateImageViewAndDescription();
            }
        });

         // 设置按钮点击事件,“下一张”按钮（buttonNext）设置点击事件。
        buttonNext.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //点击“下一张”按钮时，currentIndex加1，如果超出数组范围，则通过% imageResources.length实现循环。
                currentIndex = (currentIndex + 1) % imageResources.length;
                ////每次点击按钮后，调用updateImageViewAndDescription方法更新图片和描述。
                updateImageViewAndDescription();
            }
        });

        // 设置窗口插入监听器,用于处理系统栏（如状态栏和导航栏）的内边距。
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            //获取系统栏的内边距，并将其设置为根布局的内边距，确保内容不会被系统栏遮挡。
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        // 初始化显示第一张图片和描述,在活动启动时，调用updateImageViewAndDescription方法，初始化显示第一张图片及其描述。
        updateImageViewAndDescription();
    }

    private void updateImageViewAndDescription() {
        //根据当前索引currentIndex，更新ImageView的图片资源和TextView的描述文本。
        ImageView imageView = findViewById(R.id.imageView);
        TextView textViewDescription = findViewById(R.id.textViewDescription);
        //使用setImageResource方法设置图片，使用setText方法设置文本。
        imageView.setImageResource(imageResources[currentIndex]);
        textViewDescription.setText(descriptions[currentIndex]);
    }
} 
```





## 实验作业2：

设计一个具有两个页面的程序，**第**1**个页面**显示一张封面的图片、文字(TextView)、文字编辑框(EditText)，并设计一个按钮能够跳转第2个页面。

再者，在**第**2**个页面**除了显示”欢迎进入第二页面”之外，还要能够显示第一个页面所输入的文字编辑框的内容，而且还要设计一个按钮，能够跳回第一个页面，以达到这两个页面之间能够相互切换。

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容与java编程。此外执行结果也须提供。



### activity_first.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">
<!-- 根布局：使用了 LinearLayout，这是一个线性布局容器，会按照指定的方向（水平或垂直）依次排列子视图。 android:layout_width="match_parent" 和 android:layout_height="match_parent"：表示这个布局会填满整个父布局的宽度和高度。
android:orientation="vertical"：指定子视图的排列方向为垂直方向，即子视图会从上到下依次排列。
android:padding="16dp"：为整个布局设置了 16dp 的内边距，使子视图与布局边缘有一定的间距。-->
    
    <!--ImageView：用于显示图片。-->
    <ImageView
        android:id="@+id/imageViewCover"
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:scaleType="centerCrop"
        android:src="@drawable/sample_food" />
<!-- android:id="@+id/imageViewCover"：为该图片视图定义了一个 ID，方便在代码中引用。android:layout_width="match_parent" 和 android:layout_height="200dp"：图片的宽度填满父布局，高度固定为 200dp。android:scaleType="centerCrop"：设置图片的缩放类型为“居中裁剪”，即图片会按比例缩放并裁剪以填满整个 ImageView，同时保持图片的中心部分可见。 -->
    
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="请输入内容"
        android:textSize="18sp"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="16dp"/>
<!-- android:layout_width="wrap_content" 和 android:layout_height="wrap_content"：文本视图的宽度和高度会根据内容自动调整。android:textSize="18sp"：设置文本的字体大小为 18sp（sp 是用于字体大小的单位，可以根据用户设备的字体大小设置进行缩放）。android:layout_gravity="center_horizontal"：将文本水平居中显示。android:layout_marginTop="16dp"：设置文本视图与上一个视图（图片）的上边距为 16dp。 -->
    
    <EditText
        android:id="@+id/editTextContent"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="在这里输入文字"
        android:layout_marginTop="8dp"/>
<!-- android:hint="在这里输入文字"：设置输入框的提示文本，当输入框为空时会显示这个提示。
-->
    
    <Button
        android:id="@+id/buttonNext"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="进入第二页面"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="16dp"/>
</LinearLayout>
```



### activity_second.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="欢迎进入第二页面"
        android:textSize="24sp"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="16dp"/>
<!-- 欢迎文本 -->
    
    <TextView
        android:id="@+id/textViewContent"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:layout_marginTop="8dp"/>
<!-- 动态文本内容 -->
    
    <Button
        android:id="@+id/buttonBack"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="返回第一页面"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="16dp"/>
</LinearLayout>
```



### MainActivity.java

```java
package com.example.first;

import android.content.Intent;//用于在不同组件（如Activity）之间传递消息。
import android.os.Bundle;//用于保存和恢复活动的状态。
import android.view.View;//用于操作视图。
import android.widget.Button;//用于操作按钮.
import android.widget.EditText;//用于操作输入框。
import androidx.appcompat.app.AppCompatActivity;//继承自AppCompatActivity，提供对旧版本Android的兼容性。

public class MainActivity extends AppCompatActivity {

    private EditText editTextContent;//一个EditText对象，用于获取用户输入的内容。

    //onCreate方法是活动生命周期的一部分，用于初始化活动。
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //setContentView(R.layout.activity_first)：设置当前活动的布局文件为activity_first.xml
        setContentView(R.layout.activity_first);

        //使用findViewById方法获取布局文件中定义的EditText和Button组件
        editTextContent = findViewById(R.id.editTextContent);
        Button buttonNext = findViewById(R.id.buttonNext);
        
         // 设置按钮点击事件,“下一张”按钮（buttonNext）设置点击事件。
        buttonNext.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //获取EditText中的文本内容，通过editTextContent.getText().toString()
                String content = editTextContent.getText().toString();
                //创建一个Intent对象，用于从MainActivity跳转到SecondActivity
                Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                //使用intent.putExtra("CONTENT", content)将用户输入的内容作为额外数据传递给SecondActivity。
                intent.putExtra("CONTENT", content);
                //调用startActivity(intent)启动SecondActivity。
                startActivity(intent);
            }
        });
    }
}
```



### SecondActivity.java

```java
package com.example.first;

import android.os.Bundle;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;
import android.content.Intent;
import android.widget.Button;
import android.view.View;

public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        TextView textViewContent = findViewById(R.id.textViewContent);
        Button buttonBack = findViewById(R.id.buttonBack);

        //getIntent()：获取启动当前活动的 Intent 对象。
        Intent intent = getIntent();
        //intent.getStringExtra("CONTENT")：从 Intent 中提取键为 "CONTENT" 的字符串数据，这是从 MainActivity 传递过来的用户输入内容。
        String content = intent.getStringExtra("CONTENT");
        //textViewContent.setText(content)：将提取到的内容设置到 TextView 中，显示给用户。
        textViewContent.setText(content);

        buttonBack.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                finish();//调用 finish() 方法，结束当前活动（SecondActivity），返回到上一个活动（MainActivity）。
            }
        });
    }
}

```



### AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round" 
        android:supportsRtl="true"
        android:theme="@style/Theme.First"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <!--确保  SecondActivity 都在 AndroidManifest.xml 中正确注册。-->
        <activity android:name="SecondActivity" />
    </application>

</manifest>
```



## 实验作业3：

设计一个可以移动的小球(球的图形中间是一个更小的四方形)，当小球被拖到一个小矩形块中时，则退出程序。

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容与java编程。此外执行结果也须提供。



### ic_ball.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <!-- 这段代码定义了一个 layer-list 资源，它是一个可绘制的资源（drawable resource），用于创建一个图层列表。图层列表可以包含多个图层，每个图层可以是一个形状（shape）、图片（bitmap）或其他可绘制对象。这些图层会按照定义的顺序堆叠在一起。 -->
    <item><!-- 定义了一个图层。-->
        <shape android:shape="oval">
            <solid android:color="#FF0000"/>
        </shape>
    </item>
    <!-- shape定义了一个形状。android:shape="oval"：指定形状为圆形（oval）。solid：定义了形状的填充颜色。红色（#FF0000）-->
    
    <item android:top="20dp" android:bottom="20dp" android:left="20dp" android:right="20dp">
        <shape android:shape="rectangle">
            <solid android:color="#FFFFFF"/>
        </shape>
    </item>
    <!--设置了该图层相对于上一个图层的内边距，即在上下左右各留出 20dp 的空间。android:shape="rectangle"：指定形状为矩形（rectangle）,android:color="#FFFFFF"：设置填充颜色为白色（#FFFFFF）。-->
</layer-list>
```



### ic_goal.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="#00FF00"/><!-- 绿色#00FF00 -->
</shape>
```



### strings.xml

```xml
<resources>
    <string name="app_name">First</string>

    <string name="ball_description">A movable ball in the app</string>
    <string name="goal_description">The goal area to drop the ball</string>
</resources>
<!-- 这段代码是一个 Android 资源文件（通常位于 res/values/strings.xml），用于定义应用程序中使用的字符串资源。通过将字符串定义在资源文件中，可以方便地管理和本地化应用程序的文本内容，而无需直接硬编码在代码中。

resources 是根元素，表示这是一个资源文件，用于定义各种资源（如字符串、颜色、尺寸等）。
string：定义了一个字符串资源。name="app_name"：指定该字符串资源的名称，用于在代码中引用。First：字符串的内容，表示应用程序的名称。-->
```



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ImageView
        android:id="@+id/ball"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@drawable/ic_ball"
        android:layout_centerInParent="true"
        android:contentDescription="@string/ball_description" />
<!-- android:layout_centerInParent="true"：将图片定位在父布局的中心。android:contentDescription="@string/ball_description"：设置图片的内容描述，用于辅助功能（如屏幕阅读器）。内容描述引用了 strings.xml 中定义的 ball_description。-->
    
    <ImageView
        android:id="@+id/goal"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:src="@drawable/ic_goal"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true"
        android:contentDescription="@string/goal_description" />
<!-- android:layout_alignParentBottom="true"：将图片定位在父布局的底部。android:layout_centerHorizontal="true"：将图片水平居中对齐。
 android:contentDescription="@string/goal_description"：设置图片的内容描述，用于辅助功能。内容描述引用了 strings.xml 中定义的 goal_description。-->
</RelativeLayout>
```



### MainActivity.java

```java
package com.example.first;

import android.graphics.Rect;//用于表示矩形区域，方便检测碰撞。
import android.os.Bundle;
import android.view.MotionEvent;//用于处理触摸事件。
import android.widget.ImageView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private ImageView ball;//ball：一个 ImageView 对象，表示球的图片。
    private float dX, dY;

    @Override
    //onCreate 方法是活动生命周期的一部分，用于初始化活动。
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //setContentView(R.layout.activity_main)：设置当前活动的布局文件为 activity_main.xml。
        setContentView(R.layout.activity_main);

        ball = findViewById(R.id.ball);//获取布局文件中定义的 ImageView（球）。
        //设置触摸事件监听器：
        ball.setOnTouchListener((v, event) -> {
            switch (event.getAction()) {
                //ACTION_DOWN：当用户第一次触摸屏幕时，计算触摸点相对于图片的偏移量（dX 和 dY）。
                case MotionEvent.ACTION_DOWN:
                    dX = v.getX() - event.getRawX();
                    dY = v.getY() - event.getRawY();
                    break;
                //ACTION_MOVE：当用户移动手指时，根据触摸点的位置和偏移量更新图片的位置。使用 animate().x().y().setDuration(0).start() 实现平滑移动。
                case MotionEvent.ACTION_MOVE:
                    v.animate()
                            .x(event.getRawX() + dX)
                            .y(event.getRawY() + dY)
                            .setDuration(0)
                            .start();
                    break;
            }
            return true;
        });

        //目标区域的触摸事件
        //获取布局文件中定义的 ImageView（目标区域）。
        ImageView goal = findViewById(R.id.goal);
        //设置触摸事件监听器：
        goal.setOnTouchListener((v, event) -> {
            //当用户移动手指时，检测球是否移动到了目标区域内。
            if (event.getAction() == MotionEvent.ACTION_MOVE) {
                //使用 Rect 对象表示目标区域和球的矩形区域。
                Rect goalRect = new Rect(goal.getLeft(), goal.getTop(), goal.getRight(), goal.getBottom());
                //使用 Rect 对象表示目标区域和球的矩形区域。
                Rect ballRect = new Rect((int) ball.getX(), (int) ball.getY(), (int) (ball.getX() + ball.getWidth()), (int) (ball.getY() + ball.getHeight()));
                //如果目标区域包含触摸点，并且球的矩形区域与目标区域有交集，则调用 finish() 方法退出程序。
                if (goalRect.contains((int) event.getRawX(), (int) event.getRawY()) && ballRect.intersect(goalRect)) {
                    finish(); // 退出程序
                }
            }
            return true;
        });
    }
}
```





## 实验作业4:

设计一个手绘图形的画板。此画板提供的画笔可以选择几个简单颜色(红、蓝、绿)，并且提供清除按钮。

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容与java编程。此外执行结果也须提供。



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">
<!-- android:orientation="vertical"：指定子视图的排列方向为垂直方向，即子视图会从上到下依次排列。-->
    <!-- 顶部控制栏 -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:gravity="center"
        android:background="#800080"> <!-- 紫色背景 -->
<!-- android:orientation="horizontal"：子视图水平排列。android:gravity="center"：子视图在水平方向上居中对齐。 -->
        
        <!-- 颜色按钮（增大尺寸并添加文本标签） -->
        <Button
            android:id="@+id/btnRed"
            android:layout_width="80dp"
            android:layout_height="80dp"
            android:background="#FF0000"
            android:text="红"
            android:textColor="#FFFFFF"
            android:layout_marginEnd="8dp"/>

        <Button
            android:id="@+id/btnBlue"
            android:layout_width="80dp"
            android:layout_height="80dp"
            android:background="#0000FF"
            android:text="蓝"
            android:textColor="#FFFFFF"
            android:layout_marginEnd="8dp"/>

        <Button
            android:id="@+id/btnGreen"
            android:layout_width="80dp"
            android:layout_height="80dp"
            android:background="#00FF00"
            android:text="绿"
            android:textColor="#FFFFFF"
            android:layout_marginEnd="16dp"/>

        <!-- 清除按钮 -->
        <Button
            android:id="@+id/btnClear"
            android:layout_width="120dp"
            android:layout_height="80dp"
            android:text="消除"
            android:background="#FFA500"
            android:textColor="#FFFFFF"/>
    </LinearLayout>

    <!-- 画布区域 -->
    <!-- com.example.first.DrawView：这是一个自定义视图类，用于绘制内容。android:layout_weight="1"：通过权重分配剩余空间，使该视图占据剩余的全部空间。
 android:background="#FFFFFF"：设置背景颜色为白色。-->
    <com.example.first.DrawView
        android:id="@+id/drawView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:background="#FFFFFF"/>

</LinearLayout>
```



### DrawView.java

```java
package com.example.first;

import android.content.Context;//用于访问应用程序的全局信息。
import android.graphics.*;//用于绘图相关的类，如 Paint、Path、Canvas 等。
import android.util.AttributeSet;//用于从 XML 布局文件中解析属性。
import android.view.MotionEvent;
import android.view.View;
import java.util.ArrayList;//用于存储绘制的路径和颜色。

public class DrawView extends View {

    private Paint paint;//一个 Paint 对象，用于定义绘制的样式（如颜色、线条宽度等）。
    private ArrayList<PathWithColor> paths = new ArrayList<>();//一个 ArrayList，用于存储用户绘制的路径和对应的颜色。
    private int currentColor = Color.BLUE; // 默认蓝色

    //DrawView 的构造函数，调用 init 方法初始化绘图工具。
    public DrawView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    private void init() {
        paint = new Paint();
        paint.setAntiAlias(true);//启用抗锯齿，使线条更平滑。
        paint.setStrokeWidth(8f); // 更细的线条
        paint.setStyle(Paint.Style.STROKE);//设置绘制模式为描边（不填充）。
        paint.setStrokeJoin(Paint.Join.ROUND);//设置线条连接处的样式为圆角。
        paint.setStrokeCap(Paint.Cap.ROUND);//设置线条端点的样式为圆角。
        paint.setColor(currentColor);//设置当前颜色。
    }

    //setCurrentColor：更新当前颜色，并设置 Paint 对象的颜色。
    public void setCurrentColor(int color) {
        currentColor = color;
        paint.setColor(color);
    }

    //清空路径列表，并调用 invalidate() 重新绘制视图，从而使画布变为空白。
    public void clearCanvas() {
        paths.clear();
        invalidate();
    }

    //重写 onDraw 方法，用于在 Canvas 上绘制路径。
    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        //遍历 paths 列表，为每个路径设置对应的颜色，并绘制到 Canvas 上。
        for (PathWithColor pwc : paths) {
            paint.setColor(pwc.color);
            canvas.drawPath(pwc.path, paint);
        }
    }

    //onTouchEvent：重写 onTouchEvent 方法，处理触摸事件。
    @Override
    public boolean onTouchEvent(MotionEvent event) {
        float x = event.getX();
        float y = event.getY();

        switch (event.getAction()) {
            //ACTION_DOWN：当用户第一次触摸屏幕时，创建一个新的 Path，并将其添加到 paths 列表中。
            case MotionEvent.ACTION_DOWN:
                Path path = new Path();
                path.moveTo(x, y);
                paths.add(new PathWithColor(path, currentColor));
                break;
            //ACTION_MOVE：当用户移动手指时，将新的点添加到当前路径中。调用 invalidate() 使视图重新绘制。    
            case MotionEvent.ACTION_MOVE:
                paths.get(paths.size() - 1).path.lineTo(x, y);
                break;
        }
        //调用 invalidate() 使视图重新绘制。
        invalidate();
        return true;
    }

    //内部类 PathWithColor,一个内部类，用于存储路径和对应的颜色。
    private static class PathWithColor {
        Path path;
        int color;

        PathWithColor(Path path, int color) {
            this.path = path;
            this.color = color;
        }
    }
}
```



### MainActivity.java

```java
package com.example.first;  // 必须与XML中隐含的包名一致


import android.graphics.Color;//用于处理颜色。
import android.os.Bundle;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private DrawView drawView;//drawView：一个 DrawView 对象，表示自定义的绘图视图。
    private int currentColor = Color.RED;//currentColor：当前选择的颜色，默认为红色（Color.RED）。

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        drawView = findViewById(R.id.drawView);

        // 初始化颜色按钮,调用 setupColorButton 方法，为每个颜色按钮设置点击事件。当按钮被点击时，更新 currentColor 并调用 drawView.setCurrentColor(color) 设置绘图视图的当前颜色。
        setupColorButton(R.id.btnRed, Color.RED);
        setupColorButton(R.id.btnBlue, Color.BLUE);
        setupColorButton(R.id.btnGreen, Color.GREEN);

        // 清除按钮
        Button btnClear = findViewById(R.id.btnClear);
        //设置点击事件，调用 drawView.clearCanvas() 清空画布。
        btnClear.setOnClickListener(v -> drawView.clearCanvas());
    }

    private void setupColorButton(int buttonId, final int color) {
        Button button = findViewById(buttonId);
        button.setOnClickListener(v -> {
            currentColor = color;
            //调用 drawView.setCurrentColor(color) 设置绘图视图的当前颜色。
            drawView.setCurrentColor(color);
        });
    }
}
```





```java
package com.example.first;

import android.net.Uri;
import android.os.Bundle;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private VideoView videoView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        videoView = findViewById(R.id.videoView);

        // 初始化视频路径（在 onCreate 中执行）
        String videoPath = "android.resource://" + getPackageName() + "/" + R.raw.video1;
        videoView.setVideoURI(Uri.parse(videoPath));

        // 开始播放
        videoView.start();
    }
}
```



```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</LinearLayout>
```





## 实验作业5：

设计一个具有选择影片拨放的视频播放器，并可以暂停或停止。(例如: 可在页面上，提供3-5个的已定义好的影片，使其选择拨放，影片档案可放在sd card或res文件内皆可)

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容与java编程。此外执行结果也须提供。



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="视频播放器"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_gravity="center"
        android:background="#EEE"/> <!-- 设置背景颜色为浅灰色。 -->

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:layout_weight="1">

        <LinearLayout
            android:id="@+id/videoList"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">

            <Button
                android:id="@+id/btn_video1"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="视频1 (video1.mp4)" />

            <Button
                android:id="@+id/btn_video2"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="视频2 (video2.mp4)" />

            <Button
                android:id="@+id/btn_video3"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="视频3 (video3.mp4)" />
        </LinearLayout>
    </ScrollView>
<!-- ScrollView：一个滚动视图，允许用户滚动浏览内容。 android:layout_weight="1"：通过权重分配剩余空间，使滚动视图占据剩余的全部空间。-->
    
    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="450dp"
        android:layout_marginTop="16dp"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:layout_marginTop="16dp"
        android:gravity="center">

        <Button
            android:id="@+id/btn_play"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="播放"/>

        <Button
            android:id="@+id/btn_pause"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="暂停"
            android:layout_marginStart="16dp"/>

        <Button
            android:id="@+id/btn_stop"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="停止"
            android:layout_marginStart="16dp"/>
    </LinearLayout>
    <!-- LinearLayout：定义了一个水平排列的子布局，用于放置控制按钮。 android:orientation="horizontal"：子视图水平排列。 android:layout_marginTop="16dp"：设置该布局与上一个视图的上边距为 16dp。 android:gravity="center"：子视图在水平方向上居中对齐。-->
</LinearLayout>
```



### MainActivity.java

```java
package com.example.first;

import android.net.Uri;//用于处理 URI，例如视频资源的路径。
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.VideoView;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private VideoView videoView;
    private int currentVideoResId = R.raw.video1;/currentVideoResId：//当前选择的视频资源 ID，默认为 R.raw.video1。

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //videoView = findViewById(R.id.videoView)：获取布局文件中定义的 VideoView 组件。
        videoView = findViewById(R.id.videoView);

        setupVideoButtons();//初始化视频选择按钮。
        setupControlButtons();//初始化视频控制按钮。
    }

    private void setupVideoButtons() {
        //获取布局文件中定义的三个视频选择按钮。
        Button btn1 = findViewById(R.id.btn_video1);
        Button btn2 = findViewById(R.id.btn_video2);
        Button btn3 = findViewById(R.id.btn_video3);

        //为每个按钮设置点击事件，调用 switchVideo 方法切换视频资源。
        btn1.setOnClickListener(v -> switchVideo(R.raw.video1));
        btn2.setOnClickListener(v -> switchVideo(R.raw.video2));
        btn3.setOnClickListener(v -> switchVideo(R.raw.video3));
    }

    //如果当前选择的视频资源 ID 与新选择的资源 ID 不同，则更新 currentVideoResId。
    private void switchVideo(int videoResId) {
        if (currentVideoResId != videoResId) {
            currentVideoResId = videoResId;
            //如果视频正在播放，则停止播放。
            if (videoView.isPlaying()) {
                videoView.stopPlayback();
            }
        }
    }

    private void setupControlButtons() {
        //获取布局文件中定义的播放、暂停和停止按钮。
        Button playBtn = findViewById(R.id.btn_play);
        Button pauseBtn = findViewById(R.id.btn_pause);
        Button stopBtn = findViewById(R.id.btn_stop);

        //播放按钮：调用 playVideo 方法开始播放视频。
        playBtn.setOnClickListener(v -> playVideo());
        //暂停按钮：如果视频正在播放，则暂停；否则，继续播放。
        pauseBtn.setOnClickListener(v -> {
            if (videoView.isPlaying()) {
                videoView.pause();
            } else {
                videoView.start();
            }
        });
        //停止按钮：停止播放，并重新加载当前视频资源。
        stopBtn.setOnClickListener(v -> {
            videoView.stopPlayback();
            videoView.setVideoURI(Uri.parse("android.resource://" + getPackageName() + "/" + currentVideoResId));
        });
    }

    private void playVideo() {
        //构造视频资源的 URI。
        Uri videoUri = Uri.parse("android.resource://" + getPackageName() + "/" + currentVideoResId);
        //调用 videoView.setVideoURI 设置视频资源。
        videoView.setVideoURI(videoUri);
        //调用 videoView.start 开始播放视频。
        videoView.start();
    }
}
```





## 实验作业6：

请结合教科书课本的范例5-1和5-3 ，编写一个具有较完善功能的后台音乐播放器。(若有能力，可加上自行创意内容或功能。若无想法，简单完成基本功能即可。)

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容与java编程。此外执行结果也须提供。



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <!-- 歌曲信息显示 -->
    <TextView
        android:id="@+id/tvSongTitle"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Die for you"
        android:textSize="18sp"
        android:gravity="center"
        android:padding="8dp"/>

    <!-- 进度条 -->
    <SeekBar
        android:id="@+id/sbProgress"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:max="100"
        android:progress="0"/>

    <!-- 时间显示 -->
    <TextView
        android:id="@+id/tvTime"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="00:00 / 03:30"
        android:gravity="center"
        android:textSize="14sp"/>

    <!-- 控制按钮 -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:gravity="center">

        <ImageButton
            android:id="@+id/btnPrevious"
            android:layout_width="50dp"
            android:layout_height="50dp"
            android:src="@android:drawable/ic_media_previous"
            android:background="?attr/selectableItemBackground"/>

        <ImageButton
            android:id="@+id/btnPlay"
            android:layout_width="60dp"
            android:layout_height="60dp"
            android:src="@android:drawable/ic_media_play"
            android:background="?attr/selectableItemBackground"/>

        <ImageButton
            android:id="@+id/btnNext"
            android:layout_width="50dp"
            android:layout_height="50dp"
            android:src="@android:drawable/ic_media_next"
            android:background="?attr/selectableItemBackground"/>
    </LinearLayout>

</LinearLayout>
```



### MainActivity.java

```java
package com.example.first;


import android.media.MediaPlayer;
import android.os.Bundle;
import android.os.Handler;
import android.widget.ImageButton;
import android.widget.SeekBar;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    private MediaPlayer mediaPlayer;
    private SeekBar seekBar;
    private TextView tvTime;
    private TextView tvSongTitle; // 添加声明
    private ImageButton btnPlay;
    private Handler handler; // 添加Handler声明

    // 模拟播放列表
    private int[] songResources = {R.raw.die_for_you};
    private String[] songTitles = {"Die for You"};
    private int currentSongIndex = 0;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initViews();
        setupMediaPlayer();
        setupProgressUpdater();
        setupButtonListeners();
    }

    private void initViews() {
        // 初始化所有视图组件
        seekBar = findViewById(R.id.sbProgress);
        tvTime = findViewById(R.id.tvTime);
        tvSongTitle = findViewById(R.id.tvSongTitle); // 添加tvSongTitle初始化
        btnPlay = findViewById(R.id.btnPlay);

        handler = new Handler(); // 初始化Handler
    }

    private void setupMediaPlayer() {
        mediaPlayer = MediaPlayer.create(this, songResources[currentSongIndex]);
        updateSongInfo();

        mediaPlayer.setOnCompletionListener(mp -> {
            nextSong();
            playCurrentSong();
        });
    }

    private void setupProgressUpdater() {
        handler.postDelayed(new Runnable() {
            @Override
            public void run() {
                if (mediaPlayer != null && mediaPlayer.isPlaying()) {
                    int progress = (int) (mediaPlayer.getCurrentPosition() * 100 / mediaPlayer.getDuration());
                    seekBar.setProgress(progress);
                    tvTime.setText(formatTime(mediaPlayer.getCurrentPosition()) + " / " + formatTime(mediaPlayer.getDuration()));
                }
                handler.postDelayed(this, 1000);
            }
        }, 1000);
    }

    private void setupButtonListeners() {
        // 播放/暂停按钮
        btnPlay.setOnClickListener(v -> {
            if (mediaPlayer.isPlaying()) {
                mediaPlayer.pause();
                btnPlay.setImageResource(android.R.drawable.ic_media_play);
            } else {
                mediaPlayer.start();
                btnPlay.setImageResource(android.R.drawable.ic_media_pause);
            }
        });

        // 进度条拖动
        seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
                if (fromUser) {
                    mediaPlayer.seekTo((progress * mediaPlayer.getDuration()) / 100);
                }
            }
            @Override public void onStartTrackingTouch(SeekBar seekBar) {}
            @Override public void onStopTrackingTouch(SeekBar seekBar) {}
        });

        // 上一首/下一首（示例）
        findViewById(R.id.btnPrevious).setOnClickListener(v -> previousSong());
        findViewById(R.id.btnNext).setOnClickListener(v -> nextSong());
    }

    private void playCurrentSong() {
        mediaPlayer.start();
        btnPlay.setImageResource(android.R.drawable.ic_media_pause);
    }

    private void nextSong() {
        currentSongIndex = (currentSongIndex + 1) % songResources.length;
        releaseMediaPlayer();
        setupMediaPlayer();
        updateSongInfo();
    }

    private void previousSong() {
        currentSongIndex = (currentSongIndex - 1 + songResources.length) % songResources.length;
        releaseMediaPlayer();
        setupMediaPlayer();
        updateSongInfo();
    }

    private void updateSongInfo() {
        tvSongTitle.setText(songTitles[currentSongIndex]); // 使用初始化的tvSongTitle
        tvTime.setText("00:00 / " + formatTime(songResources[currentSongIndex]));
        seekBar.setMax(100);
        seekBar.setProgress(0);
    }

    private String formatTime(int milliseconds) {
        int seconds = (milliseconds / 1000) % 60;
        int minutes = (milliseconds / 1000) / 60;
        return String.format("%02d:%02d", minutes, seconds);
    }

    private void releaseMediaPlayer() {
        if (mediaPlayer != null) {
            mediaPlayer.release();
            mediaPlayer = null;
        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        releaseMediaPlayer();
        handler.removeCallbacksAndMessages(null); // 清理Handler
    }
}
```



## 实验作业7：

请参考教科书课本的范例6-2与6-3 ，编写一个能够调用JavaScript脚本的程序，其脚本能够计算身体BMI数值(数学公式可自己设计或参考网络上信息)，也就是输入身高与体重信息后能够显示BMI数值。

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容、java编程、html档案内容。此外执行结果也须提供。



### **Activity_main.xml**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp"
    android:background="#f0f0f0"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="BMI Calculator"
        android:textSize="24sp"
        android:textColor="#333333"
        android:layout_gravity="center_horizontal"
        android:paddingBottom="20dp"/>

    <EditText
        android:id="@+id/height_input"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter your height (cm)"
        android:inputType="numberDecimal"
        android:padding="10dp"
        android:background="@drawable/edittext_bg"/>

    <EditText
        android:id="@+id/weight_input"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Enter your weight (kg)"
        android:inputType="numberDecimal"
        android:padding="10dp"
        android:background="@drawable/edittext_bg"
        android:layout_marginTop="10dp"/>

    <Button
        android:id="@+id/calculate_button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Calculate BMI"
        android:textSize="18sp"
        android:background="@drawable/button_bg"
        android:textColor="#ffffff"
        android:padding="12dp"
        android:layout_marginTop="20dp"/>

    <TextView
        android:id="@+id/result_text"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="18sp"
        android:textColor="#333333"
        android:gravity="center_horizontal"
        android:padding="20dp"
        android:layout_marginTop="20dp"
        android:visibility="gone"/>

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="250dp"
        android:layout_marginTop="20dp"/>

</LinearLayout>
```



### MainActivity.java

```java
package com.example.first;

import androidx.appcompat.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;

public class MainActivity extends AppCompatActivity {

    private EditText heightInput;
    private EditText weightInput;
    private Button calculateButton;
    private TextView resultText;
    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 初始化视图
        heightInput = findViewById(R.id.height_input);
        weightInput = findViewById(R.id.weight_input);
        calculateButton = findViewById(R.id.calculate_button);
        resultText = findViewById(R.id.result_text);
        webView = findViewById(R.id.webview);

        // 配置WebView
        WebSettings webSettings = webView.getSettings();
        webSettings.setJavaScriptEnabled(true);

        // 加载本地的HTML文件
        webView.loadUrl("file:///android_asset/bmi_calculator.html");
        webView.setWebViewClient(new WebViewClient());

        // 设置按钮点击事件
        calculateButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String heightStr = heightInput.getText().toString();
                String weightStr = weightInput.getText().toString();

                if (!heightStr.isEmpty() && !weightStr.isEmpty()) {
                    double height = Double.parseDouble(heightStr);
                    double weight = Double.parseDouble(weightStr);

                    // 将值传递给JavaScript函数
                    webView.loadUrl("javascript:calculateBMI(" + height + ", " + weight + ")");

                    // 显示结果区域
                    resultText.setVisibility(View.VISIBLE);
                } else {
                    resultText.setText("Please enter both height and weight!");
                }
            }
        });
    }
}
```



### Bml_calculator.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BMI Calculator</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
<div class="calculator-container">
    <h2>BMI Calculation Result</h2>
    <div id="result"></div>
    <div id="bmi-category"></div>
    <div id="health-advice"></div>
</div>

<script src="script.js"></script>
</body>
</html>
```



### Index.html

```html
<!DOCTYPE html>
<html>
<head>
    <title>BMI Calculator</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
<div class="container">
    <div id="result-container">
        <h2 id="bmi-value"></h2>
        <p id="bmi-status"></p>
    </div>
</div>
<script src="script.js"></script>
</body>
</html>
```



### Script.js

```js
function calculateBMI(height, weight) {
    // 将厘米转换为米
    var heightMeters = height / 100;
    
    // 计算BMI
    var bmi = weight / (heightMeters * heightMeters);
    
    // 四舍五入到两位小数
    bmi = bmi.toFixed(2);
    
    // 显示结果
    document.getElementById('result').innerHTML = 'Your BMI is: ' + bmi;
    
    // 判断BMI类别
    var category;
    if (bmi < 18.5) {
        category = 'Underweight';
        document.getElementById('bmi-category').style.color = '#2ecc71';
        document.getElementById('health-advice').innerHTML = 
            'Advice: You may need to increase your calorie intake. Eat a balanced diet with plenty of fruits, vegetables, and whole grains.';
    } else if (bmi >= 18.5 && bmi < 25) {
        category = 'Normal weight';
        document.getElementById('bmi-category').style.color = '#3498db';
        document.getElementById('health-advice').innerHTML = 
            'Advice: Congratulations! You have a healthy weight. Maintain your current diet and exercise routine.';
    } else if (bmi >= 25 && bmi < 30) {
        category = 'Overweight';
        document.getElementById('bmi-category').style.color = '#f1c40f';
        document.getElementById('health-advice').innerHTML = 
            'Advice: You may need to lose some weight. Consider reducing calorie intake and increasing physical activity.';
    } else {
        category = 'Obese';
        document.getElementById('bmi-category').style.color = '#e74c3c';
        document.getElementById('health-advice').innerHTML = 
            'Advice: You should consult with a healthcare professional to develop a weight loss plan. Making lifestyle changes can significantly improve your health.';
    }
    
    document.getElementById('bmi-category').innerHTML = 'Category: ' + category;
}
```



### Styles.css

```css
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
}

.calculator-container {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 90%;
    max-width: 400px;
    text-align: center;
}

h2 {
    color: #333;
    margin-bottom: 20px;
}

#result {
    font-size: 24px;
    color: #333;
    margin: 20px 0;
}

#bmi-category {
    font-size: 18px;
    color: #666;
    margin-bottom: 20px;
}

#health-advice {
    font-size: 16px;
    color: #888;
    line-height: 1.6;
}
```



### Button_bg.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">

    <solid android:color="#4CAF50" />
    <corners android:radius="8dp" />
    <padding
        android:left="20dp"
        android:top="12dp"
        android:right="20dp"
        android:bottom="12dp" />
</shape>
```



### Edittext_bg.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">

    <solid android:color="#FFFFFF" />
    <corners android:radius="8dp" />
    <stroke
        android:width="1dp"
        android:color="#CCCCCC" />
    <padding
        android:left="10dp"
        android:top="8dp"
        android:right="10dp"
        android:bottom="8dp" />
</shape>
```



## 实验作业8:

编请参考课本第六章内容所学，写一个用户登录程序(具有密码验证功能)，向远程服务器登录。其要求如下:

界面布局设计中，分别设置输入用户名和密码的编辑框，再设置一个“提交”按钮和显示服务器端程序返回的“登录成功”的文本标签与图案，若失败也需响应显示“登录失败”的文本标签与图案。注意其登录验证的程序须写在服务器端程序中。(服务器IP使用本机IP即可)。

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容与相关java编程(手机端与服务器端程序)。此外执行结果也须提供。



### Activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/username"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="用户名" />

    <EditText
        android:id="@+id/password"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="密码"
        android:inputType="textPassword" />

    <Button
        android:id="@+id/submit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="提交" />

    <TextView
        android:id="@+id/result_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text=""
        android:textSize="18sp"
        android:layout_gravity="center_horizontal" />

    <ImageView
        android:id="@+id/result_image"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:src="@drawable/fail" />

</LinearLayout>
```



### MainActivity.java

```java
package com.example.first;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ImageView;
import android.widget.TextView;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;

public class MainActivity extends AppCompatActivity {

    private EditText usernameEditText;
    private EditText passwordEditText;
    private Button submitButton;
    private TextView resultTextView;
    private ImageView resultImageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        usernameEditText = findViewById(R.id.username);
        passwordEditText = findViewById(R.id.password);
        submitButton = findViewById(R.id.submit);
        resultTextView = findViewById(R.id.result_text);
        resultImageView = findViewById(R.id.result_image);

        submitButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String username = usernameEditText.getText().toString();
                String password = passwordEditText.getText().toString();
                new Thread(new LoginTask(username, password)).start();
            }
        });
    }

    private class LoginTask implements Runnable {
        private String username;
        private String password;

        public LoginTask(String username, String password) {
            this.username = username;
            this.password = password;
        }

        @Override
        public void run() {
            try {
                Socket socket = new Socket("127.0.0.1", 8080); // 替换为本机IP
                PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
                BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

                out.println(username + "," + password);

                String response = in.readLine();
                if (response.equals("success")) {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            resultTextView.setText("登录成功");
                            resultImageView.setImageResource(R.drawable.success);
                        }
                    });
                } else {
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            resultTextView.setText("登录失败");
                            resultImageView.setImageResource(R.drawable.fail);
                        }
                    });
                }

                socket.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}


```



### Sever.java

```java
package com.example.first;

import java.io.*;
import java.net.*;

public class Server {
    public static void main(String[] args) {
        try {
            ServerSocket serverSocket = new ServerSocket(8080);
            System.out.println("服务器已启动，监听端口 8080...");
            while (true) {
                Socket clientSocket = serverSocket.accept();
                System.out.println("客户端已连接: " + clientSocket.getInetAddress());
                new Thread(new ClientHandler(clientSocket)).start();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    private static class ClientHandler implements Runnable {
        private Socket clientSocket;

        public ClientHandler(Socket clientSocket) {
            this.clientSocket = clientSocket;
        }

        @Override
        public void run() {
            try {
                BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
                PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);

                String input = in.readLine();
                if (input != null) {
                    System.out.println("接收到客户端数据: " + input);
                    String[] credentials = input.split(",");
                    String username = credentials[0];
                    String password = credentials[1];

                    // 验证用户名和密码
                    if ("admin".equals(username) && "123456".equals(password)) {
                        out.println("success");
                    } else {
                        out.println("failure");
                    }
                }

                clientSocket.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}

```



### AndroidManifest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <!-- 后台服务权限 -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <!-- 媒体播放专用权限（Android 13+ 强制要求） -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_MEDIA_PLAYBACK" />
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.First"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>



    </application>

</manifest>




```



## 实验作业9:

编请参考课本第七章内容所学，编写一个通过列表组件ListView显示Json数组数据的程序。其数据可用JSON数组表达：

 [{"sid":1001, "name":"张大山"}, {"sid":1002, "name":"李小丽"} ];



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="解析数据"
        android:textSize="18sp"
        android:background="#CCCCCC"
        android:padding="16dp" />

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />
</LinearLayout>
```



### MainActivity.java

```java
package com.example.first;

import android.os.Bundle;
import android.widget.ListView;
import androidx.appcompat.app.AppCompatActivity;
import org.json.JSONArray;
import org.json.JSONObject;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        String jsonData = "[{\"sid\":1001, \"name\":\"张大山\", \"age\":21}, {\"sid\":1002, \"name\":\"李小丽\", \"age\":22}]";

        ListView listView = findViewById(R.id.listView);
        List<String> dataList = new ArrayList<>();

        try {
            JSONArray jsonArray = new JSONArray(jsonData);
            for (int i = 0; i < jsonArray.length(); i++) {
                JSONObject jsonObject = jsonArray.getJSONObject(i);
                String name = jsonObject.getString("name");
                int age = jsonObject.getInt("age");
                dataList.add(name + ", " + age);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        CustomAdapter adapter = new CustomAdapter(this, dataList);
        listView.setAdapter(adapter);
    }
}
```



### CustomAdapter.java

```java
package com.example.first;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.TextView;
import java.util.List;

public class CustomAdapter extends ArrayAdapter<String> {
    public CustomAdapter(Context context, List<String> data) {
        super(context, 0, data);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = LayoutInflater.from(getContext()).inflate(R.layout.list_item, parent, false);
        }

        TextView textView = convertView.findViewById(R.id.textView);
        textView.setText(getItem(position));
        return convertView;
    }
}

```



### list.item.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/textView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:textSize="18sp"
    android:padding="16dp" />
```





## 实验作业10：

编请参考课本第八章SQLite数据库内容所学，写一个用户个人通讯簿APP，其要求如下:

需要能记录: 用户的姓名、电话、住址、微信号、Email。并能浏览所有已记入数据。

请将设计好之用户使用之GUI页面截图，并提供用户GUI页面的xml档案内容与相关java编程。此外执行结果也须提供。



### activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <ListView
        android:id="@+id/contactList"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1" />

    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="end|bottom"
        android:layout_margin="16dp"
        android:src="@android:drawable/ic_input_add" />
</LinearLayout>
```



### activity_add_contact.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Name" />

    <EditText
        android:id="@+id/phone"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Phone" />

    <EditText
        android:id="@+id/address"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Address" />

    <EditText
        android:id="@+id/wechat"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="WeChat" />

    <EditText
        android:id="@+id/email"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Email" />

    <Button
        android:id="@+id/save"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Save" />
</LinearLayout>
```



### list_item.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/nameTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        android:textStyle="bold" />

    <TextView
        android:id="@+id/phoneTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="14sp" />

    <TextView
        android:id="@+id/addressTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="14sp" />

    <TextView
        android:id="@+id/wechatTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="14sp" />

    <TextView
        android:id="@+id/emailTextView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="14sp" />
</LinearLayout>
```



### DatabaseHelper.java

```java
package com.example.first;

import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class DatabaseHelper extends SQLiteOpenHelper {
    private static final String DATABASE_NAME = "contact.db";
    private static final int DATABASE_VERSION = 1;

    private static final String TABLE_NAME = "contacts";
    private static final String COLUMN_ID = "_id";
    private static final String COLUMN_NAME = "name";
    private static final String COLUMN_PHONE = "phone";
    private static final String COLUMN_ADDRESS = "address";
    private static final String COLUMN_WECHAT = "wechat";
    private static final String COLUMN_EMAIL = "email";

    private static final String TABLE_CREATE =
            "CREATE TABLE " + TABLE_NAME + " (" +
                    COLUMN_ID + " INTEGER PRIMARY KEY AUTOINCREMENT, " +
                    COLUMN_NAME + " TEXT, " +
                    COLUMN_PHONE + " TEXT, " +
                    COLUMN_ADDRESS + " TEXT, " +
                    COLUMN_WECHAT + " TEXT, " +
                    COLUMN_EMAIL + " TEXT);";

    public DatabaseHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(TABLE_CREATE);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
        onCreate(db);
    }
}

```



### CustomAdapter.java

```java
package com.example.first;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.TextView;
import java.util.List;

public class CustomAdapter extends ArrayAdapter<String[]> {
    public CustomAdapter(Context context, List<String[]> data) {
        super(context, 0, data);
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        if (convertView == null) {
            convertView = LayoutInflater.from(getContext()).inflate(R.layout.list_item, parent, false);
        }

        String[] item = getItem(position);
        TextView nameTextView = convertView.findViewById(R.id.nameTextView);
        TextView phoneTextView = convertView.findViewById(R.id.phoneTextView);
        TextView addressTextView = convertView.findViewById(R.id.addressTextView);
        TextView wechatTextView = convertView.findViewById(R.id.wechatTextView);
        TextView emailTextView = convertView.findViewById(R.id.emailTextView);

        nameTextView.setText(item[0]);
        phoneTextView.setText(item[1]);
        addressTextView.setText(item[2]);
        wechatTextView.setText(item[3]);
        emailTextView.setText(item[4]);

        return convertView;
    }
}

```



### AddContactActivity.java

```java
package com.example.first;

import android.content.ContentValues;
import android.content.Intent;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import androidx.appcompat.app.AppCompatActivity;

public class AddContactActivity extends AppCompatActivity {
    private EditText name, phone, address, wechat, email;
    private DatabaseHelper dbHelper;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_add_contact);

        dbHelper = new DatabaseHelper(this);
        name = findViewById(R.id.name);
        phone = findViewById(R.id.phone);
        address = findViewById(R.id.address);
        wechat = findViewById(R.id.wechat);
        email = findViewById(R.id.email);
        Button save = findViewById(R.id.save);

        save.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String nameText = name.getText().toString();
                String phoneText = phone.getText().toString();
                String addressText = address.getText().toString();
                String wechatText = wechat.getText().toString();
                String emailText = email.getText().toString();

                SQLiteDatabase db = dbHelper.getWritableDatabase();
                ContentValues values = new ContentValues();
                values.put("name", nameText);
                values.put("phone", phoneText);
                values.put("address", addressText);
                values.put("wechat", wechatText);
                values.put("email", emailText);
                db.insert("contacts", null, values);
                db.close();

                Intent intent = new Intent(AddContactActivity.this, MainActivity.class);
                startActivity(intent);
                finish();
            }
        });
    }
}

```



### MainActivity.java

```java
package com.example.first;

import android.content.ContentValues;
import android.content.DialogInterface;
import android.content.Intent;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import com.google.android.material.floatingactionbutton.FloatingActionButton;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    private DatabaseHelper dbHelper;
    private ListView contactList;
    private CustomAdapter adapter;
    private List<String[]> dataList;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        dbHelper = new DatabaseHelper(this);
        contactList = findViewById(R.id.contactList);
        dataList = new ArrayList<>();
        adapter = new CustomAdapter(this, dataList);
        contactList.setAdapter(adapter);

        FloatingActionButton fab = findViewById(R.id.fab);
        fab.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(MainActivity.this, AddContactActivity.class);
                startActivity(intent);
            }
        });

        contactList.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                showDeleteDialog(position);
            }
        });

        loadContacts();
    }

    private void loadContacts() {
        dataList.clear();
        SQLiteDatabase db = dbHelper.getReadableDatabase();
        Cursor cursor = db.query("contacts", null, null, null, null, null, null);

        if (cursor.moveToFirst()) {
            do {
                int nameIndex = cursor.getColumnIndex("name");
                int phoneIndex = cursor.getColumnIndex("phone");
                int addressIndex = cursor.getColumnIndex("address");
                int wechatIndex = cursor.getColumnIndex("wechat");
                int emailIndex = cursor.getColumnIndex("email");

                if (nameIndex != -1 && phoneIndex != -1 && addressIndex != -1 && wechatIndex != -1 && emailIndex != -1) {
                    String name = cursor.getString(nameIndex);
                    String phone = cursor.getString(phoneIndex);
                    String address = cursor.getString(addressIndex);
                    String wechat = cursor.getString(wechatIndex);
                    String email = cursor.getString(emailIndex);
                    dataList.add(new String[]{name, phone, address, wechat, email});
                } else {
                    // 如果列索引是 -1，说明列名不正确或列不存在
                    cursor.close();
                    db.close();
                    return;
                }
            } while (cursor.moveToNext());
        }
        cursor.close();
        db.close();
        adapter.notifyDataSetChanged();
    }

    private void showDeleteDialog(final int position) {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("Delete Contact")
                .setMessage("Are you sure you want to delete this contact?")
                .setPositiveButton("Delete", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        deleteContact(position);
                    }
                })
                .setNegativeButton("Cancel", null)
                .show();
    }

    private void deleteContact(int position) {
        String[] item = dataList.get(position);
        String name = item[0];
        SQLiteDatabase db = dbHelper.getWritableDatabase();
        db.delete("contacts", "name=?", new String[]{name});
        db.close();
        loadContacts();
    }
}
```



### AndroidManifest.xml

```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <!-- 后台服务权限 -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
    <!-- 媒体播放专用权限（Android 13+ 强制要求） -->
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_MEDIA_PLAYBACK" />
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.First"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <activity android:name=".AddContactActivity" />

    </application>

</manifest>




```

