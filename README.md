#Use androidannotations to MVP
###Declare a Interface of View
```
public interface MyView{
  public void showName(String name);
}
```
###Declare bean interface
```
public interface IMyBean{
  public void getUserName();
}
```
###Implement bean
```
@EBean
public MyBean implements IMyBean{
  @ViewInterface //New Annotation
  MyView myView;
  @Override
  @Background //Use AA to run background
  public void getUserName(){
     //read user from db or Http server or other
     myView.showName("hello");
  }
}
```
###In Activity,Fragment,Service or only a common Class
```
@EActivity(R.layout.layout_main)
public MyActivity{
  @Bean(MyBean.class)
  IMyBean myBean;
  
  @Click(R.id.btn)
  public void click(){
    //Call myBean, excuted in background
    myBean.getUserName();
  }
  //MyBean will invoke this in method getUserName, so need to be UIThread safe
  @UIThread
  public void showName(String name){
    showToast(name);
  }
}
```
###Use EViewInterfaceMethod an ViewInterfaceMethod
In MyView.java,you can add EViewInterfaceMethod

```
@EViewInterfaceMethod
public void showName(String name);
```
to mark this is a ViewInterfaceMethod declare.
in Activity,Add ViewInterfaceMethod to showName method

```
@ViewInterfaceMethod
public void showName(String name)
```
So, when any of the method is changed, compiler will give a compile error, so you know something is wrong, and you don't need to implement MyView interface
#EIntent,IntentExtra
```
@EIntent("BroadcastAction-XX")
public class MyIntent{
  @IntentExtra
  public String name;
  @IntentExtra(IntentObjectType.PARCELABLE)
  public UserInfo userInfo;
  @IntentExtra
  public SerializableObject obj;
}
```
###sendBroadCast
```
sendBroadCast(new MyIntent_().setName("name").setUserInfo(userInfo).getIntent());
```
###receive broadcast
```
MyIntent_ myIntent=MyIntent_.build(intent);
myIntent.getName()
```
#ContextEvent
###Write a event interface
```
public interface IMyEvent{
	public void onEvent1(String action);
}
```
###In Fragment,Adapter,View..
```
@ContextEvent
IMyEvent myEvent;
.............
myEvent.onEvent1("action");

```
###In activity
```
//Not necessary,but recommend implements IMyEvent
implements IMyEvent
public void onEvent1(String action){
	//do something
}
```
[AndroidAnnotations](http://androidannotations.org/)
