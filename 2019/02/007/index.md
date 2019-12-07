# Android中的Service


Service 是一个可以在后台执行长时间操作而不使用用户界面的应用组件。

那么问题来了，既然它不使用用户界面，那么它怎么知道应该什么时候开始执行什么操作呢？答案是——它可以与其他的引用组件形成一些联系，从而可以根据其传来的信息在合适的时候执行合适的操作。

<!--more-->

## 1.什么是 Service

Service 是一个可以在后台执行长时间操作而不使用用户界面的应用组件。那么问题来了，既然它不使用用户界面，那么它怎么知道应该什么时候开始执行什么操作呢？答案是——它可以与其他的引用组件形成一些联系，从而可以根据其传来的信息在合适的时候执行合适的操作。

一般来讲，这种联系分为两种：startService()以及 bindService()。这两种联系都可以使得一个 service 开始运行，但是在其他方面有着诸多不同。

|              | 启动 service 的方式                                       | 停止 service 的方式                                                                           | service 与启动它的组件之间的通信方式                                                                                    | service 的生命周期                                                                                       |
| ------------ | --------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| startService | 在其他组件中调用 startService()方法后，服务即处于启动状态 | service 中调用 stopSelf()方法，或者其他组件调用 stopService()方法后，service 将停止运行       | 没有提供默认的通信方式，启动 service 后该 service 就处于独立运行状态                                                    | 一旦启动，service 即可在后台无限期运行，即使启动 service 的组件已被销毁也不受其影响，直到其被停止        |
| bindService  | 在其他组件中调用 bindService()方法后，服务即处于启动状态  | 所有与 service 绑定的组件都被销毁，或者它们都调用了 unbindService()方法后，service 将停止运行 | 可以通过 ServiceConnection 进行通信，组件可以与 service 进行交互、发送请求、获取结果，甚至是利用 IPC 跨进程执行这些操作 | 当所有与其绑定的组件都取消绑定(可能是组件被销毁也有可能是其调用了 unbindService()方法)后，service 将停止 |

注：

1.表格中的“其他组件”不包括 Broadcast receiver，其无法直接启动或绑定 service。

2.startService()与 bindService()并不冲突，同一个 service 可能既有组件调用了 startService()启动它，又有组件与它进行了绑定。当同一个 service 与其他组件同时存在这两种联系时，其生命周期会发生变化，必须从两种方法的角度看 service 均停止才能真正停止。

ok，通过这个表格我们可以知道，这两种启动 service 的方式各有自己的特点，我们可以根据自己的项目需要选择合适的方式。

## 2.如何创建一个 service？

关于这个，每本 Android 入门的书籍基本上都会有所提及，基本上也正如它们所述：

- 创建一个类继承自 Service(或它的子类，如 IntentService)，重写里面的一些关键的回调方法，如 onStartCommand()，onBind()等
- 在 Manifests 文件里面为其声明，并根据需要配置一些其他属性。

讲道理，这一切跟新建一个 Activity 灰常的像，跟孪生兄弟一样——包括其里面的一些关键方法，都充斥着一种浓浓的基情。

- onCreate()
  在每个 service 的生命周期中这个方法会且仅会调用一次，并且它的调用在 onStartCommand()以及 onBond()之前，我们可以在这个方法中进行一些一次性的初始化工作。

- onStartCommand()
  当其他组件通过 startService()方法启动 service 时，此方法将会被调用。

- onBind()
  当其他组件通过 bindService()方法与 service 相绑定之后，此方法将会被调用。这个方法有一个 IBinder 的返回值，这意味着在重写它的时候必须返回一个 IBinder 对象，它是用来支撑其他组件与 service 之间的通信的——另外，如果你不想让这个 service 被其他组件所绑定，可以通过在这个方法返回一个 null 值来实现。

- onDestroy()
  这是 service 一生中调用的最后一个方法，当这个方法被调用之后，service 就会被销毁。所以我们应当在这个方法里面进行一些资源的清理，比如注册的一些监听器什么的。

在 Manifests 文件里进行声明的时候，只有 android:name 属性是必须要有的，其他的属性都可以没有。但是有的时候适当的配置可以让我们的开发进行地更加顺利，所以了解一下注册一个 service 可以声明哪些属性也是很有必要的。

```xml
<service
    android:enabled=["true" | "false"]
    android:exported=["true" | "false"]
    android:icon="drawable resource"
    android:isolatedProcess=["true" | "false"]
    android:label="string resource"
    android:name="string"
    android:permission="string"
    android:process="string" >
</service>
```

- android:enabled : 如果为 true，则这个 service 可以被系统实例化，如果为 false，则不行。默认为 true

- android:exported : 如果为 true，则其他应用的组件也可以调用这个 service 并且可以与它进行互动，如果为 false，则只有与 service 同一个应用或者相同 user ID 的应用可以开启或绑定此 service。它的默认值取决于 service 是否有 intent filters。如果一个 filter 都没有，就意味着只有指定了 service 的准确的类名才能调用，也就是说这个 service 只能应用内部使用——其他的应用不知道它的类名。这种情况下 exported 的默认值就为 false。反之，只要有了一个 filter，就意味着 service 是考虑到外界使用的情况的，这时 exported 的默认值就为 true

- android:icon : 一个象征着这个 service 的 icon

- android:isolatedProcess : 如果设置为 true，这个 service 将运行在一个从系统中其他部分分离出来的特殊进程中，我们只能通过 Service API 来与它进行交流。默认为 false。

- android:label : 显示给用户的这个 service 的名字。如果不设置，将会默认使用`<application>`的 label 属性。

- android:name : 这个 service 的路径名，例如“com.lypeer.demo.MyService”。这个属性是唯一一个必须填的属性。

- android:permission : 其他组件必须具有所填的权限才能启动这个 service。

- android:process : service 运行的进程的 name。默认启动的 service 是运行在主进程中的。

example:

```android
public class ServiceDemo extends Service {

    private static final String TAG = "ServiceDome";

    @Override
    public void onCreate() {
        super.onCreate();
        Log.d(TAG, "onCreate");
        //只在service创建的时候调用一次，可以在此进行一些一次性的初始化操作
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d(TAG, "onStartCommand");
        //当其他组件调用startService()方法时，此方法将会被调用
        //在这里进行这个service主要的操作
        return super.onStartCommand(intent, flags, startId);
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        Log.d(TAG, "onBind");
        //当其他组件调用bindService()方法时，此方法将会被调用
        //如果不想让这个service被绑定，在此返回null即可
        return null;
    }

    @Override
    public void onDestroy() {
        Log.d(TAG, "onDestroy");
        //service调用的最后一个方法
        //在此进行资源的回收
        super.onDestroy();
    }
}
```

```xml
<!--in manifests -->
<service android:name=".ServiceDemo"/>
```

## 3，如何启动 service？

前面讲了两种启动 service 的方式，但是都讲的比较概括，这里就详细的讲一下这两种方式。

### 3.1，startService()

另一个组件通过调用 startService()方法，就可以启动一个特定的 service，并且这将导致 service 中的 onStartCommand()方法被调用。在调用 startService()方法的时候，其他组件需要在方法中传递一个 intent 参数，然后 service 将会在 onStartCommand()中接收这个 intent，并获取一些数据。比如此时某个 Activity 要将一些数据存入数据库中，我就可以通过 intent 把数据传入 service，然后让 service 去进行连接数据库，存储数据等操作，而此时用户可以执行其他的任何操作——甚至包括销毁那个 Activity——这并不会影响 service 存储数据这件事。

当一个 service 通过这种方式启动之后，它的生命周期就已经不受启动它的组件影响了，它可以在后台无限期的运行下去，只要 service 自身没有调用 stopSelf()并且其他的组件没有调用针对它的 stopService()。

另外，如果确定了使用这种方式启动 service 并且不希望这个 service 被绑定的话，那么也许除了传统的创建一个类继承 service 之外我们有一个更好的选择——继承 IntentService。

如果是扩建 Service 类的话，通常情况下我们需要新建一个用于执行工作的新线程，因为默认情况下 service 将工作于应用的主线程，而这将会降低所有正在运行的 Activity 的性能。而 IntentService 就不同了。它是 Service 的子类，它使用工作线程来注意的处理所有的 startService 请求。如果你不要求这个 service 要同时处理多个请求，那么继承这个类显然要比直接继承 Service 好到不知道哪里去了——IntentService 已经做了这些事：

- 创建默认的工作线程，用于在应用的主线程外执行传递给 onStartCommand() 的所有 Intent
- 创建工作队列，用于将一个 Intent 逐一传递给 onHandleIntent() 实现，这样的话就永远不必担心多线程问题了
- 在处理完所有启动请求后停止服务，从此妈妈再也不用担心我忘记调用 stopSelf() 了
- 提供 onBind() 的默认实现（返回 null）
- 提供 onStartCommand() 的默认实现，可将 Intent 依次发送到工作队列和 onHandleIntent() 实现

因此我们只需要实现 onHandleIntent()方法来完成具体的功能逻辑就可以了。

举个栗子：

```Android
//现在在一个 Activity 里面
Intent intent = new Intent(MainActivity.this , IntentServiceDemo.class);
startService(intent);
```

```Android
public class IntentServiceDemo extends IntentService {

    public IntentServiceDemo(String name) {
        super(name);
        //构造方法
    }

    @Override
    protected void onHandleIntent(Intent intent) {
        //在这里根据intent进行操作
    }
}
```

相比上面的继承 service 实现，这个确实要简单许多。但是要注意的是，如果需要重写其他的方法，比如 onDestroy()方法，一定不要删掉它的超类实现！因为它的超类实现里面也许包括了对工作线程还有工作队列的初始化以及销毁等操作，如果没有了的话很容易出问题。

但是，如果你有让 service 同时处理多个请求的需求，这个时候就只能去继承 Service 了。这个时候就要自己去处理工作线程那些事。下面是一个官方的栗子：

```Android
public class HelloService extends Service {
  private Looper mServiceLooper;
  private ServiceHandler mServiceHandler;

  private final class ServiceHandler extends Handler {
      public ServiceHandler(Looper looper) {
          super(looper);
      }
      @Override
      public void handleMessage(Message msg) {
          long endTime = System.currentTimeMillis() + 5*1000;
          while (System.currentTimeMillis() < endTime) {
              synchronized (this) {
                  try {
                      wait(endTime - System.currentTimeMillis());
                  } catch (Exception e) {
                  }
              }
          }
          stopSelf(msg.arg1);
      }
  }

  @Override
  public void onCreate() {
    HandlerThread thread = new HandlerThread("ServiceStartArguments",
            Process.THREAD_PRIORITY_BACKGROUND);
    thread.start();
    mServiceLooper = thread.getLooper();
    mServiceHandler = new ServiceHandler(mServiceLooper);
  }

  @Override
  public int onStartCommand(Intent intent, int flags, int startId) {
      Toast.makeText(this, "service starting", Toast.LENGTH_SHORT).show();

      Message msg = mServiceHandler.obtainMessage();
      msg.arg1 = startId;
      mServiceHandler.sendMessage(msg);

      return START_STICKY;
  }

  @Override
  public IBinder onBind(Intent intent) {
      return null;
  }

  @Override
  public void onDestroy() {
    Toast.makeText(this, "service done", Toast.LENGTH_SHORT).show();
  }
}
```

比起 IntentService 来，显然做的工作要多的多的多。但是，由于是自己处理的对 onStartCommand 的调用，它可以同时执行多个请求——虽然官方的栗子里没有这样做。但是如果你想这样做，就可以为每一个请求创建一个线程，然后立即运行这些请求。

另外，我们注意到 onStartCommand()的返回值是一个很奇怪的值 START_STICKY，这是个什么呢？或者说这个方法的返回值是用来干嘛的呢？事实上，它的返回值是用来指定系统对当前线程的行为的。它的返回值必须是以下常量之一：

- START_NOT_STICKY : 如果系统在 onStartCommand() 返回后终止服务，则除非有挂起 Intent 要传递，否则系统不会重建服务。这是最安全的选项，可以避免在不必要时以及应用能够轻松重启所有未完成的作业时运行服务。

- START_STICKY : 如果系统在 onStartCommand() 返回后终止服务，则会重建服务并调用 onStartCommand()，但绝对不会重新传递最后一个 Intent。相反，除非有挂起 Intent 要启动服务（在这种情况下，将传递这些 Intent ），否则系统会通过空 Intent 调用 onStartCommand()。这适用于不执行命令、但无限期运行并等待作业的媒体播放器（或类似服务）。

- START_REDELIVER_INTENT : 如果系统在 onStartCommand() 返回后终止服务，则会重建服务，并通过传递给服务的最后一个 Intent 调用 onStartCommand()。任何挂起 Intent 均依次传递。这适用于主动执行应该立即恢复的作业（例如下载文件）的服务。

除此之外，继承 service 来实现的 service，一定要记得，停止这个 service。因为除非系统必须回收内存资源，否则系统不会停止或销毁 service。事实上，如果这个 service 是与可见组件绑定，其几乎永远不会被停止或销毁。在这种情况下，如果你忘记了在其工作完成之后将其停止，势必会造成系统资源的浪费以及电池电量的消耗，而这应当是我们要极力避免的。

### 3.2，bindService()

这是一种比 startService 更复杂的启动方式，同时使用这种方式启动的 service 也能完成更多的事情，比如其他组件可向其发送请求，接受来自它的响应，甚至通过它来进行 IPC 等等。我们通常将绑定它的组件成为客户端，而称它为服务器。

如果要创建一个支持绑定的 service，我们必须要重写它的 onBind()方法。这个方法会返回一个 IBinder 对象，它是客户端用来和服务器进行交互的接口。而要得到 IBinder 接口，我们通常有三种方式：继承 Binder 类，使用 Messenger 类，使用 AIDL。
