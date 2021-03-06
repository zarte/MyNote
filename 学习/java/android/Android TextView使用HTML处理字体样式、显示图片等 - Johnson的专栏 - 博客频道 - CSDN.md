
         学 Android 的时候突然想到一个问题：怎么用TextView控件显示带有格式的文字，可否使用Html布局？查了下Android 帮助文档，其提供了 android.text.Html类和Html.ImageGetter 、Html.TagHandler接口 。

        其实本不打算写这篇博文的，但看到网络上关于此的文章，基本是：你抄我，我抄你，大家抄来抄去，有用的也就那么一两篇文章，而且说得不明不白，网络就是如此，盗版也成为了一种文化，这就是所谓的拿来主义吧。当然不否认大牛的辛勤劳作，写出的高质量文章；其次是学以致用，个人习惯--总结一下。

先看截图：

               


        我们 平常使用 TextView的 setText()方法 传递String参数 的时候 ， 其实是调用的public final void setText (CharSequence text)方法：


[java] view plain copy
print ?
/**  
    * Sets the string value of the TextView. TextView <em>does not</em> accept  
    * HTML-like formatting, which you can do with text strings in XML resource files.  
    * To style your strings, attach android.text.style.* objects to a  
    * {@link android.text.SpannableString SpannableString}, or see the  
    * <a href="{@docRoot}guide/topics/resources/available-resources.html#stringresources">  
    * Available Resource Types</a> documentation for an example of setting   
    * formatted text in the XML resource file.  
    *  
    * @attr ref android.R.styleable#TextView_text  
    */   
    @android .view.RemotableViewMethod  
    public   final   void  setText(CharSequence text) {  
       setText(text, mBufferType);  
   }  
 /**
     * Sets the string value of the TextView. TextView <em>does not</em> accept
     * HTML-like formatting, which you can do with text strings in XML resource files.
     * To style your strings, attach android.text.style.* objects to a
     * {@link android.text.SpannableString SpannableString}, or see the
     * <a href="{@docRoot}guide/topics/resources/available-resources.html#stringresources">
     * Available Resource Types</a> documentation for an example of setting 
     * formatted text in the XML resource file.
     *
     * @attr ref android.R.styleable#TextView_text
     */
    @android.view.RemotableViewMethod
    public final void setText(CharSequence text) {
        setText(text, mBufferType);
    }
         而String类是CharSequence的子类，在CharSequence子类中有一个接口Spanned，即类似html的带标记的文本，我们可以用它来在TextView中显示html。 但 在上面Android源码注释中有提及TextView does not accept HTML-like formatting。



       android.text.Html类共提供了三个方法，可以到Android帮助文档查看。


[java] view plain copy
print ?
public   static  Spanned fromHtml (String source)  
  
public   static  Spanned fromHtml (String source, Html.ImageGetter imageGetter, Html.TagHandler tagHandler)  
  
public   static  String toHtml (Spanned text)  
public static Spanned fromHtml (String source)

public static Spanned fromHtml (String source, Html.ImageGetter imageGetter, Html.TagHandler tagHandler)

public static String toHtml (Spanned text)




       通过使用第一个方法，可以将Html显示在TextView中：




[java] view plain copy
print ?
public   void  onCreate(Bundle savedInstanceState) {  
         super .onCreate(savedInstanceState);  
        setContentView(R.layout.main);  
  
        TextView tv=(TextView)findViewById(R.id.textView1);  
        String html= "<html><head><title>TextView使用HTML</title></head><body><p><strong>强调</strong></p><p><em>斜体</em></p>"   
                + "<p><a href=\"http://www.dreamdu.com/xhtml/\">超链接HTML入门</a>学习HTML!</p><p><font color=\"#aabb00\">颜色1"   
                + "</p><p><font color=\"#00bbaa\">颜色2</p><h1>标题1</h1><h3>标题2</h3><h6>标题3</h6><p>大于>小于<</p><p>"  +  
                 "下面是网络图片</p><img src=\"http://avatar.csdn.net/0/3/8/2_zhang957411207.jpg\"/></body></html>" ;  
          
        tv.setMovementMethod(ScrollingMovementMethod.getInstance()); //滚动   
        tv.setText(Html.fromHtml(html));      
    }  
public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        TextView tv=(TextView)findViewById(R.id.textView1);
        String html="<html><head><title>TextView使用HTML</title></head><body><p><strong>强调</strong></p><p><em>斜体</em></p>"
        		+"<p><a href=\"http://www.dreamdu.com/xhtml/\">超链接HTML入门</a>学习HTML!</p><p><font color=\"#aabb00\">颜色1"
        		+"</p><p><font color=\"#00bbaa\">颜色2</p><h1>标题1</h1><h3>标题2</h3><h6>标题3</h6><p>大于>小于<</p><p>" +
        		"下面是网络图片</p><img src=\"http://avatar.csdn.net/0/3/8/2_zhang957411207.jpg\"/></body></html>";
        
        tv.setMovementMethod(ScrollingMovementMethod.getInstance());//滚动
        tv.setText(Html.fromHtml(html));    
    } 效果：



              
        可以看出，字体效果是显示出来了，但是图片却没有显示。要实现图片的显示需要使用Html.fromHtml的另外一个重构方法：public static Spanned fromHtml (String source, Html.ImageGetterimageGetter, Html.TagHandler tagHandler)其中Html.ImageGetter是一个接口，我们要实现此接口，在它的getDrawable(String source)方法中返回图片的Drawable对象才可以。
修改后的代码：


[java] view plain copy
print ?
ImageGetter imgGetter =  new  Html.ImageGetter() {  
         public  Drawable getDrawable(String source) {  
              Drawable drawable =  null ;  
              URL url;    
               try  {     
                  url =  new  URL(source);    
                  drawable = Drawable.createFromStream(url.openStream(),  "" );   //获取网路图片   
              }  catch  (Exception e) {    
                   return   null ;    
              }    
              drawable.setBounds( 0 ,  0 , drawable.getIntrinsicWidth(), drawable  
                            .getIntrinsicHeight());  
               return  drawable;   
        }  
};  
ImageGetter imgGetter = new Html.ImageGetter() {
        public Drawable getDrawable(String source) {
              Drawable drawable = null;
              URL url;  
              try {   
                  url = new URL(source);  
                  drawable = Drawable.createFromStream(url.openStream(), "");  //获取网路图片
              } catch (Exception e) {  
                  return null;  
              }  
              drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable
                            .getIntrinsicHeight());
              return drawable; 
        }
};
这里主要是实现了Html.ImageGetter接口，通过图片的URL地址获取相应的Drawable实例。
不要忘了在Mainifest文件中加入网络访问的权限：




[java] view plain copy
print ?
<uses-permission android:name= "android.permission.INTERNET"  />  
<uses-permission android:name="android.permission.INTERNET" />
 友情提示：通过网络获取图片是一个耗时的操作，最好不要放在主线程中，否则容易引起阻塞。
上面介绍的是显示网络上的图片，但 如何显示本地的图片 呢：






[java] view plain copy
print ?
   ImageGetter imgGetter =  new  Html.ImageGetter() {  
         public  Drawable getDrawable(String source) {  
              Drawable drawable =  null ;  
                 
              drawable = Drawable.createFromPath(source);  //显示本地图片   
              drawable.setBounds( 0 ,  0 , drawable.getIntrinsicWidth(), drawable  
                            .getIntrinsicHeight());  
               return  drawable;   
        }  
};  
   ImageGetter imgGetter = new Html.ImageGetter() {
        public Drawable getDrawable(String source) {
              Drawable drawable = null;
               
              drawable = Drawable.createFromPath(source); //显示本地图片
              drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable
                            .getIntrinsicHeight());
              return drawable; 
        }
}; 只需将source改为本地图片的路径便可，在这里我使用的是：





[java] view plain copy
print ?
String source;  
source=getFilesDir()+ "/ic_launcher.png" ;  
    String source;
    source=getFilesDir()+"/ic_launcher.png";





我的小站： Android TextView使用HTML处理字体样式、显示图片




THE END

