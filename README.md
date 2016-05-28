# Spinner

android 52-53 lesson
1.Spinner组件：提供一个快速的方法来从一组选择一个值，默认状态下显示当前选择的值，触摸spinner组件可以显示一个下拉菜单供用户选择                  其实就是一个下拉列表,Spinner的下拉选项,可以在string.xml中设置一个数组，这是静态方法，还有动态方法.
2.Spinner实例：在app->res->layout->activity_main.xml中：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:paddingBottom="@dimen/activity_vertical_margin"
      android:paddingLeft="@dimen/activity_horizontal_margin"
      android:paddingRight="@dimen/activity_horizontal_margin"
      android:paddingTop="@dimen/activity_vertical_margin"
      tools:context="com.example.mackerlee.android_52.MainActivity">
  
      <Spinner
          android:id="@+id/spinner01"
          android:layout_width="match_parent"
          android:layout_height="wrap_content"
          //--设置将spinner与下面string.xml中的citys数组联系起来，如果是调用单个字符串用@string/id名称，如果调用字符数组用
              @array/id名称
          android:entries="@array/citys">
  
      </Spinner>
  </RelativeLayout>
  
  在res->values->string.xml中设置spinner数组:
  <resources>
    <string name="app_name">Android_52</string>
    //--设置下拉数组,数组名称自定义为citys
    <string-array name="citys">
        <item>北京</item>
        <item>上海</item>
        <item>广州</item>
        <item>深圳</item>
        <item>珠海</item>
    </string-array>
  </resources>
  
3.通过代码动态填充下拉列表:
  在res->layout->activity_main.xml中：
  <?xml version="1.0" encoding="utf-8"?>
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:tools="http://schemas.android.com/tools"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:paddingBottom="@dimen/activity_vertical_margin"
      android:paddingLeft="@dimen/activity_horizontal_margin"
      android:paddingRight="@dimen/activity_horizontal_margin"
      android:paddingTop="@dimen/activity_vertical_margin"
      tools:context="com.example.mackerlee.android_52.MainActivity">
  
      //--静态xml方式
      <Spinner
          android:id="@+id/spinner01"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:entries="@array/citys">
      </Spinner>
      //--代码动态方式
      <Spinner
          android:id="@+id/spinner02"
          android:layout_width="wrap_content"
          android:layout_height="wrap_content"
          android:layout_below="@id/spinner01">
      </Spinner>
  </RelativeLayout>
  
  在代码app->java->包名->MainActivity.java：
  package com.example.mackerlee.android_52;

  import android.support.v7.app.AppCompatActivity;
  import android.os.Bundle;
  import android.widget.ArrayAdapter;
  import android.widget.Spinner;
  
  public class MainActivity extends AppCompatActivity {
  
      private Spinner spinner;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          spinner = (Spinner)findViewById(R.id.spinner02);
  
          //--通过代码获取strings.xml资源文件中的string数组citys
          String[] citys = getResources().getStringArray(R.array.citys);
          //--数组适配器:将适配器与数组链接起来
          //方式一：用new的方式创建,android.R.layout.simple_spinner_item是android自带的布局
          // ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,android.R.layout.simple_spinner_item,citys);
          //方式二：用API推荐的静态方法创建.用CharSequence接口定义泛型，String就是继承类CharSequence接口
          ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,R.array.citys,android.R.layout.simple_spinner_item);
          //--设置适配器一个下拉的布局资源，用到安卓内部的布局资源
          adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);  
          //--将适配器与Spinner链接起来，完成
          spinner.setAdapter(adapter);
      }
  }

  
