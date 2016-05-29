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
          //--设置适配器一个下拉样式的布局资源，用到安卓内部的布局资源
          adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);  
          //--将适配器与Spinner链接起来，完成
          spinner.setAdapter(adapter);
      }
  }

android 54-55 lesson
1.在ArrayAdapter.createFromResource中android的系统布局：在C:\Users\mackerlee\AppData\Local\Android\sdk\platforms\android-23\da               ta\res中就是该SDK下android的原生自带系统资源,适配器中用到的布局就在该目录的layout下.如果想用自己设计的下拉列表样               式，则按照其格式自己创造一个xml的TextView配置文件，id必须和simple_spinner_dropdown_item.xml一样都是：
              android:id="@android:id/text1"，然后用adapter.setDropDownViewResource设置连接自定义的下拉样式即可.
2.Spinner下拉列表样式自定义实例：在res->layout中创建自定义布局spinner_dropdown_item.xml:
            <?xml version="1.0" encoding="utf-8"?>
            <TextView xmlns:android="http://schemas.android.com/apk/res/android"
                android:id="@android:id/text1"
                android:layout_width="match_parent"
                android:layout_height="wrap_parent"
                android:textColor="#00ff00"
                android:textSize="20sp"/>
  在MainActivity.java中使用自定义样式与适配器连接：
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
          //方式一：用new的方式创建，用new的方法可以使获取数组不只局限于xml文件，还可以从数据库/网络等方式获取
          // ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,android.R.layout.simple_spinner_item,citys);
          //--方式二：用API推荐的静态方法创建.用CharSequence接口定义泛型，String就是继承类CharSequence接口
          //--参数(上下文,数据,布局:android系统内部的布局)
          ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,R.array.citys,android.R.layout.simple_spinner_item);
          //--如果不设置dropdown下拉样式，则下拉列表默认任是上面的android.R.layout.simple_spinner_item布局 
          //--使用自定义下拉列表样式spinner_dropdown_item.xml
          adapter.setDropDownViewResource(R.layout.spinner_dropdown_item);
          //--将适配器与Spinner链接起来
          spinner.setAdapter(adapter);
      }
  }

3.spinner组件下拉选择响应事件：在app->java->包名->MainActivity.java中实现响应接口
  package com.example.mackerlee.android_52;

  import android.support.v7.app.AppCompatActivity;
  import android.os.Bundle;
  import android.view.View;
  import android.widget.AdapterView;
  import android.widget.ArrayAdapter;
  import android.widget.Spinner;
  import android.widget.Toast;
  
  //--implements AdapterView.OnItemSelectedListener:实现spinner组件的响应接口
  public class MainActivity extends AppCompatActivity implements AdapterView.OnItemSelectedListener{
  
      private Spinner spinner;
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_main);
          spinner = (Spinner)findViewById(R.id.spinner02);
  
          //--通过代码获取strings.xml资源文件中的string数组citys
          String[] citys = getResources().getStringArray(R.array.citys);
          //--数组适配器:将适配器与数组链接起来
          //方式一：用new的方式创建
          // ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,android.R.layout.simple_spinner_item,citys);
          //--方式二：用API推荐的静态方法创建.用CharSequence接口定义泛型，String就是继承类CharSequence接口
          //--参数(上下文,数据,布局:android系统内部的布局)
          ArrayAdapter<CharSequence> adapter = ArrayAdapter.createFromResource(this,R.array.citys,android.R.layout.simple_spinner_item);
          //--如果不设置dropdown下拉样式，则下拉列表默认任是上面的android.R.layout.simple_spinner_item布局
          //adapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);  //设置适配器一个下拉的布局资源，用到安卓内部的布局资源
          //--使用自定义下拉列表样式spinner_dropdown_item.xml
          adapter.setDropDownViewResource(R.layout.spinner_dropdown_item);
          //--将适配器与Spinner链接起来
          spinner.setAdapter(adapter);
  
          //--注册事件
          spinner.setOnItemSelectedListener(this);
      }
  
      //--选中spinner下拉列表的响应事件，参数(spinner组件，下拉textview,选中项所处列表中位置，id)
      @Override
      public void onItemSelected(AdapterView<?> parent, View view, int position, long id) {
          System.out.println(parent.getClass());
          System.out.println(view.getClass());
          System.out.println("pos"+position); //从0开始
          System.out.println("id"+id);        //从0开始
          //--spinner.getSelectedItem().toString()下拉列表所选中的值
          Toast.makeText(this,spinner.getSelectedItem().toString(),Toast.LENGTH_LONG).show();
      }
  
      //--未选中事件处理
      @Override
      public void onNothingSelected(AdapterView<?> parent) {
  
      }
  }

  
