public class MainActivity extends Activity {



 private Handler handler;
 private String html;
 private TextView tv;
 private ProgressBar bar;

 @Override
 protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_main);
  // 网上找的html数据
  html = "<html><head><title>TextView使用HTML</title></head><body><p><strong>强调</strong></p><p><em>斜体</em></p>"
    + "<p><a href=\"http://www.jb51.net">超链接HTML入门</a>学习HTML!</p><p><font color=\"#aabb00\">颜色1"
    + "</p><p><font color=\"#00bbaa\">颜色2</p><h1>标题1</h1><h3>标题2</h3><h6>标题3</h6><p>大于>小于<</p><p>"
    + "下面是网络图片</p><img src=\"http://www.jb51.net/1207.jpg\"/></body>"
    + "下面是网络图片</p><img src=\"http://www.jb51.net/207.jpg\"/></body></html>";

  tv = (TextView) this.findViewById(R.id.id);
  bar = (ProgressBar) this.findViewById(R.id.id_bar);
  tv.setMovementMethod(ScrollingMovementMethod.getInstance());// 滚动

  handler = new Handler() {
   @Override
   public void handleMessage(Message msg) {
    // TODO Auto-generated method stub
    if (msg.what == 0x101) {
     bar.setVisibility(View.GONE);
     tv.setText((CharSequence) msg.obj);
    }
    super.handleMessage(msg);
   }
  };

  // 因为从网上下载图片是耗时操作 所以要开启新线程
  Thread t = new Thread(new Runnable() {
   Message msg = Message.obtain();

   @Override
   public void run() {
    // TODO Auto-generated method stub
    bar.setVisibility(View.VISIBLE);
    /**
     * 要实现图片的显示需要使用Html.fromHtml的一个重构方法：public static Spanned
     * fromHtml (String source, Html.ImageGetterimageGetter,
     * Html.TagHandler
     * tagHandler)其中Html.ImageGetter是一个接口，我们要实现此接口，在它的getDrawable
     * (String source)方法中返回图片的Drawable对象才可以。
     */
    ImageGetter imageGetter = new ImageGetter() {

     @Override
     public Drawable getDrawable(String source) {
      // TODO Auto-generated method stub
      URL url;
      Drawable drawable = null;
      try {
       url = new URL(source);
       drawable = Drawable.createFromStream(
         url.openStream(), null);
       drawable.setBounds(0, 0,
         drawable.getIntrinsicWidth(),
         drawable.getIntrinsicHeight());
      } catch (MalformedURLException e) {
       // TODO Auto-generated catch block
       e.printStackTrace();
      } catch (IOException e) {
       // TODO Auto-generated catch block
       e.printStackTrace();
      }
      return drawable;
     }
    };
    CharSequence test = Html.fromHtml(html, imageGetter, null);
    msg.what = 0x101;
    msg.obj = test;
    handler.sendMessage(msg);
   }
  });
  t.start();
 }

 @Override
 public boolean onCreateOptionsMenu(Menu menu) {
  // Inflate the menu; this adds items to the action bar if it is present.
  getMenuInflater().inflate(R.menu.main, menu);
  return true;
 }

}
