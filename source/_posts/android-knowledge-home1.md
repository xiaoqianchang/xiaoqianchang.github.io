title: Android弹药库
date: 2016-03-10 11:04:35
categories: Android
tags: 
  - Android
---

在工作中突然发现以前多么熟悉的知识技能现在不能愉快的玩耍了，想要再次使用以前的一些知识还得Google，这是一件多么伤心的事啊。这些天比较闲，打算把最近用到的小知识总结下，这里会持续更新。还是应了那句话，“知识在于积累”。

<!--more-->

好，开工啦！

## 设备信息的获取 ##

下面以Samsung SM-N9006（Android 4.4.2 API19）为例

### 手机的型号 ###

``` bash
/** 手机的型号 */
public static String getMobileType() {
	return android.os.Build.MODEL; // SM-N9006
}
```

### 手机的OS版本号 ###

``` bash
/** 手机的OS版本号 */
public static String getSdkVersion() {
	return android.os.Build.VERSION.RELEASE; // 指的是版本，例如4.4.2
	// android.os.Build.VERSION.SDK; // 指的是level，例如 19
	// android.os.Build.VERSION.SDK_INT; // 指的是level，例如 19
}
```

### 设备的DeviceId ###

``` bash
/**
 * @return 设备的DeviceId,如果获取不到，就将该手机的Wifi Mac地址作为唯一识别码
 */
public static String getDeviceId() {
	TelephonyManager tm = (TelephonyManager) context.getSystemService(Context.TELEPHONY_SERVICE);
	String deviceId = tm.getDeviceId();
	if (deviceId == null || deviceId.trim().length() == 0) {
		WifiManager wifi = (WifiManager) context.getSystemService(Context.WIFI_SERVICE);
		WifiInfo info = wifi.getConnectionInfo();
		deviceId = info.getMacAddress();
	}
	return deviceId;
}
```

## APP信息的获取 ##

### 获取PackageInfo ###

``` bash
public static PackageInfo getPackageInfo() {
		PackageInfo pi = null;
		try {
			pi = context.getPackageManager()
					.getPackageInfo(context.getPackageName(), PackageManager.GET_CONFIGURATIONS);
		} catch (NameNotFoundException e) {
			e.printStackTrace();
		}
		return pi;
	}
```

### 获取版本号 ###

``` bash
/** 手机版本号 */
public static int getSoftVersion() {
	return getPackageInfo().versionCode;
}
```

### 获取版本信息 ###

``` bash
/** 手机的版本名 */
public static String getVersionName() {
	return getPackageInfo().versionName;
}
```

### 检查apk文件是否为一个完整的apk文件 ###

``` bash
/** 检查下载完成的apk文件，是否为一个完整的apk文件 */
public static boolean checkApkFile(String apkFilePath) {
	boolean result = false;
	try {
		PackageManager pManager = FrameworkController.getInstance().getPackageManager();
		PackageInfo pInfo = pManager.getPackageArchiveInfo(apkFilePath, PackageManager.GET_ACTIVITIES);
		if (pInfo == null) {
			result = false;
		} else {
			result = true;
		}
	} catch (Exception e) {
		result = false;
		e.printStackTrace();
	}
	return result;
}
```

## 存储操作 ##

### 外置存储(sdcard) ###

#### 检查sdcard是否存在 ####

``` bash
/** 检查sdcard是否存在(安装好) */
public static boolean isSdcardExist() {
	if (android.os.Environment.getExternalStorageState().equals(
			android.os.Environment.MEDIA_MOUNTED))
		return true;

	return false;
}
```

可以这样配置路径
`Environment.getExternalStorageDirectory().getAbsolutePath()+ File.separator+"MobileOffice"+File.separator+"upgrade"`

#### 得到外置存储的root的绝对路径 ####

``` bash
/** 得到外置存储的root的绝对路径，一般为 /storage/emulated/0  */
public static String getStorageRootDir() { // /mnt/sdcard
	return Environment.getExternalStorageDirectory().getAbsolutePath();
}
```

#### 创建sdcard的文件夹 ####

``` bash
// dir 要创建sdcard的文件夹的绝对路径
public  static boolean createDir(String dir) {
	File file = new File(dir);
	if(!file.exists()){
		return file.mkdirs();
	}else {
		return true;
	}
}
```

### 内部存储 ###

得到cashe路径

``` bash
String cachePath = context.getCacheDir().getAbsolutePath();
```

## MD5加密 ##

### 方法一 ###

``` bash
public final static String MD5(String s) {
	char hexDigits[] = { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f' };
	try {
		byte[] btInput = s.getBytes();
		// 获得MD5摘要算法的 MessageDigest 对象
		MessageDigest mdInst = MessageDigest.getInstance("MD5");
		// 使用指定的字节更新摘要
		mdInst.update(btInput);
		// 获得密文
		byte[] md = mdInst.digest();
		// 把密文转换成十六进制的字符串形式
		int j = md.length;
		char str[] = new char[j * 2];
		int k = 0;
		for (int i = 0; i < j; i++) {
			byte byte0 = md[i];
			str[k++] = hexDigits[byte0 >>> 4 & 0xf];
			str[k++] = hexDigits[byte0 & 0xf];
		}
		return new String(str);
	} catch (Exception e) {
		e.printStackTrace();
		return null;
	}
}
```

### 方法二 ###

``` bash
/** 对原始字符串进行md5的hash计算 */
private static String md5(String string) {
	byte[] hash;
	try {
		hash = MessageDigest.getInstance("MD5").digest(string.getBytes("UTF-8"));
	} catch (NoSuchAlgorithmException e) {
		throw new RuntimeException("Huh, MD5 should be supported?", e);
	} catch (UnsupportedEncodingException e) {
		throw new RuntimeException("Huh, UTF-8 should be supported?", e);
	}
	StringBuilder hex = new StringBuilder(hash.length * 2);
	for (byte b : hash) {
		if ((b & 0xFF) < 0x10)
			hex.append("0");
		hex.append(Integer.toHexString(b & 0xFF));
	}
	return hex.toString();
}
```

### 方法三 ###

``` bash
public String Md5(String plainText) {
	String result = "";
	try {
		MessageDigest md = MessageDigest.getInstance("MD5");
		md.update(plainText.getBytes());
		byte b[] = md.digest();
		int i;
		StringBuffer buf = new StringBuffer("");
		for (int offset = 0; offset < b.length; offset++) {
			i = b[offset];
			if (i < 0)
				i += 256;
			if (i < 16)
				buf.append("0");
			buf.append(Integer.toHexString(i));
		}
		result = buf.toString().toUpperCase();// 32位的加密（转成大写）
		buf.toString().substring(8, 24);// 16位的加密
	} catch (NoSuchAlgorithmException e) {
		e.printStackTrace();
	}
	return result;
}
```

## 判断给定字符串是否空白串 ##

``` bash
/**
 * 判断给定字符串是否空白串。<br>
 * 空白串是指由空格、制表符、回车符、换行符组成的字符串<br>
 * StringUtils.isBlank(null) = true StringUtils.isBlank("") = true StringUtils.isBlank(" ") = true
 * 
 * @return boolean
 */
public static boolean isBlank(CharSequence cs) {
	int strLen;
	if (cs == null || (strLen = cs.length()) == 0) {
		return true;
	}
	for (int i = 0; i < strLen; i++) {
		if (!Character.isWhitespace(cs.charAt(i))) {
			return false;
		}
	}
	return true;
}
```

## 对指定字符串隐藏 ##

### 对字符串中间部分隐藏 ###

``` bash
/**
 * 对身份证号和就诊卡号进行部分隐藏 </br>
 * <pre>
 * StringUtils.hidePartString("abcdefghi", 3 , 4) = abc**fghi
 * </pre>
 */
public static String hideMiddleString(String str, int prefixCount, int suffixCount) {
	if (null == str || "".equals(str)) {
		return "";
	}
	int length = str.length();
	if (length <= (prefixCount + suffixCount))
		return str;

	int count = length - prefixCount - suffixCount;
	String temp1 = str.substring(0, prefixCount);
	String temp2 = "", temp3 = "";
	for (int i = 0; i < count; i++)
		temp2 += "*";
	temp3 = str.substring(length - suffixCount, length);
	return temp1 + temp2 + temp3;
}
```

### 对字符串两边部分隐藏 ###

``` bash
/**
 * 对身份证号和就诊卡号进行部分隐藏 </br>
 * <pre>
 * StringUtils.hidePartString("abcdefghi", 3 , 4) = ***de****
 * </pre>
 */
public static String hidePartString2(String str, int prefixCount, int suffixCount) {
	if (null == str || "".equals(str)) {
		return "";
	}
	int length = str.length();
	if (length <= (prefixCount + suffixCount))
		return str;

	String temp1 = str.substring(prefixCount, length - suffixCount);
	String temp2 = "", temp3 = "";
	for (int i = 0; i < prefixCount; i++)
		temp2 += "*";
	for (int i = 0; i < suffixCount; i++)
		temp3 += "*";
	return temp2 + temp1 + temp3;
}
```

## 对.properties文件的加载解析 ##

``` bash
// msgId 为键值对的key
public static String getMessage(String msgId) {
	String resourcePath = "/assets/FailureMessage.properties";
	Properties pro = new Properties();
	try {
		pro.load(CommonUtils.class.getResourceAsStream(resourcePath));
	} catch (IOException e) {
		e.printStackTrace();
	}
	return pro.getProperty(msgId);
}
```

## 得到所有进程Task最上面的Activity ##

``` bash
public static String getTopActivity(Context context) {
	ActivityManager manager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
	List<RunningTaskInfo> runningTaskInfos = manager.getRunningTasks(1);

	if (runningTaskInfos != null)
		return runningTaskInfos.get(0).topActivity.getClassName();
	else
		return "";
}
```

## 获取Html相关内容 ##

### 获取Html的title ###

``` bash
// 通过正则表达式提取
static final String REG_TITLE       = "<title>(.+)</title>";

public static String getTitle(String html) {
    String str = null;
    Matcher matcher = Pattern.compile(REG_TITLE).matcher(html);
    if (matcher.find()) {
        str = matcher.group(1);
    }
    return str;
}
```

### 获取Html的所欲IMG ###

``` bash
static final String REG_IMG         = "<img.+src=\"(.+)\".+>";

public static String[] getImgUrl(String html) {
    String[] str = null;
    Matcher matcher = Pattern.compile(REG_IMG).matcher(html);
    if (matcher.find()) {
        str = new String[matcher.groupCount()];
        for (int i = 1; i <= matcher.groupCount(); i++)
            str[i - 1] = matcher.group(i);
    }
    return str;
}
```

### 获取Html的所欲meta ###

``` bash
static final String REG_DESCRIPTION = ".*<meta name=\"description\" content=\"([^>]+)\">.*";

public static String getDescription(String html) {
    String str = null;
    Matcher matcher = Pattern.compile(REG_DESCRIPTION).matcher(html);
    if (matcher.find()) {
        str = matcher.group(1);
    }
    return str;
}
```

## 图片处理框架用法 ##

### ImageLoader(一) ###

``` bash
ImageLoader.getInstance().displayImage(
		imgUrl,
		imageView,
		new DisplayImageOptions.Builder()
				.showImageForEmptyUri(R.drawable.pic_loading_empty)
				.showImageOnFail(R.drawable.pic_loading_error)
				// .displayer(new RoundedBitmapDisplayer(10))
				// .considerExifParams(true)
				.cacheInMemory(true).build());
```

或者：

1. 先定义DisplayImageOptions
``` bash
public static DisplayImageOptions options = new DisplayImageOptions.Builder()
    	.showImageForEmptyUri(R.drawable.default_patient)
    	.showImageOnFail(R.drawable.default_patient)
    	// .displayer(new RoundedBitmapDisplayer(10))
		// .considerExifParams(true)
    	.cacheInMemory(true)
    	.build();
```
2. 在使用的地方加载并显示

``` bash
ImageLoader.getInstance().displayImage(imgUrl, imageView, options);
```

### ImageLoader(二) ###

 1. 先定义

``` bash
// 第一次显示监听器
public static class AnimateFirstDisplayListener extends SimpleImageLoadingListener {

    // 装imgUrl
	public static final List<String> displayedImages = Collections.synchronizedList(new LinkedList<String>());

	@Override
	public void onLoadingComplete(String imageUri, View view, Bitmap loadedImage) {
		if (loadedImage != null) {
			ImageView imageView = (ImageView) view;
			boolean firstDisplay = !displayedImages.contains(imageUri);
			if (firstDisplay) {
				FadeInBitmapDisplayer.animate(imageView, 500);
				displayedImages.add(imageUri);
			}
		}
	}
}
```
2. 在使用的地方加载并显示

``` bash
ImageLoader.getInstance().displayImage(imgUrl,
    		imageView, options,
    		new AnimateFirstDisplayListener());
```

## 下载网络图片 ##

``` bash
/** 图片Url保存为位图并进行缩放操作，通过传入图片url获取位图方法 */
public static Bitmap getNetImage(Context context, String url) {
	Bitmap mBitmap = null;
	URL myFileUrl = null;
	try {
		myFileUrl = new URL(url);
	} catch (MalformedURLException e) {
		e.printStackTrace();
	}
	try {
		if (myFileUrl != null) {
			HttpURLConnection conn = (HttpURLConnection) myFileUrl.openConnection();
			conn.setDoInput(true);
			conn.connect();
			InputStream is = conn.getInputStream();
			mBitmap = BitmapFactory.decodeStream(is);
			is.close();
		}
	} catch (IOException e) {
		e.printStackTrace();
	}
	if (mBitmap != null) {
//			DisplayMetrics metrics = new DisplayMetrics();
//			metrics = context.getResources().getDisplayMetrics();
//			int screenWidth = metrics.widthPixels;
//			int screenHeight = metrics.heightPixels;
		int bitmapWidth = mBitmap.getWidth();
		int bitmapHeight = mBitmap.getHeight();
//			float scale = ((float) screenWidth) / bitmapWidth;
		Matrix matrix = new Matrix();
//			matrix.postScale(scale, scale);
		mBitmap = Bitmap.createBitmap(mBitmap, 0, 0, bitmapWidth, bitmapHeight, matrix, true);
	}
	return mBitmap;
}
```

## 手机号码限制 ##

``` bash
/**
 * 判别手机是否为正确手机号码； 号码段分配如下：
 * 
 * 移动：134、135、136、137、138、139、150、151、157(TD)、158、159、187、188
 * 联通：130、131、132、152、155、156、185、186 电信：133、153、180、189、（1349卫通）
 */
public static boolean isMobileNum(String mobiles) {
	Pattern p1 = Pattern.compile("(\\+[0-9]+[\\- \\.]*)?" // +<digits><sdd>*
			+ "(\\([0-9]+\\)[\\- \\.]*)?" // (<digits>)<sdd>*
			+ "([0-9][0-9\\- \\.][0-9\\- \\.]+[0-9])");
	// Pattern p = Pattern.compile("^((13[0-9])|(15[^4,//D])|(18[0,5-9]))//d{8}$");

	Matcher m = p1.matcher(mobiles);
	return m.matches();
}

public static boolean isMobileNum2(String num) {
	String str = "^((13[0-9])|(15[0-9])|(18[0-9])|(145)|(147))\\d{8}$";
	Pattern p = Pattern.compile(str);
	Matcher m = p.matcher(num);
	return m.matches();
}
```

## 蓝牙Bluetooth ##

Android 中打开 Bluetooth：有以下三种方法：

 1. 强制打开
 2. 调用系统弹出框提示用户打开
 3. 跳转到系统设置中让用户自己打开

### 获取BluetoothAdapter ###

``` bash
public static BluetoothAdapter getBluetoothAdapter() {
	return BluetoothAdapter.getDefaultAdapter();
}
```

### 当前 Android 设备是否支持 Bluetooth ###

``` bash
// true：支持 Bluetooth false：不支持 Bluetooth
public static boolean isBluetoothSupported() {
	return BluetoothAdapter.getDefaultAdapter() != null ? true : false;
}
```

### 当前 Android 设备的 bluetooth 是否已经开启 ###

``` bash
/** 没有直接的用户的允许绝不要开启 Bluetooth。如果你想要打开 Bluetooth 创建一个无线连接，你应当使用 ACTION_REQUEST_ENABLE Intent，这样会弹出一个提示框提示用户是否开启 Bluetooth，enable() 方法仅提供给有 UI 、更改系统设置的应用来使用，例如“电源管理”应用。 */
public static boolean isBluetoothEnabled() {
	BluetoothAdapter bluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
	if (bluetoothAdapter != null) {
		return bluetoothAdapter.isEnabled();
	}
	return false;
}
```

### 强制打开 ###

``` bash
// 强制开启当前 Android 设备的 Bluetooth
public static boolean turnOnBluetooth() {
	BluetoothAdapter bluetoothAdapter = BluetoothAdapter.getDefaultAdapter();
	if (bluetoothAdapter != null) {
		return bluetoothAdapter.enable();
	}
	return false;
}
```

### 调用系统弹出框提示用户打开 ###

``` bash
// 弹出系统弹框提示用户打开 Bluetooth
public static void openBluetooth(Activity activity) {
	// 请求打开 Bluetooth
	Intent enableBtIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_ENABLE);  
	enableBtIntent.setAction(BluetoothAdapter.ACTION_REQUEST_DISCOVERABLE);
	enableBtIntent.putExtra(BluetoothAdapter.EXTRA_DISCOVERABLE_DURATION, BLUETOOTH_DISCOVERABLE_DURATION);
	// 设置 Bluetooth 设备可以被其它 Bluetooth 设备扫描到
	// 打开本机的蓝牙发现功能（默认打开120秒，可以将时间最多延长至300秒）  
    // Intent discoveryIntent = new Intent(BluetoothAdapter.ACTION_REQUEST_DISCOVERABLE); 
	// 设置 Bluetooth 设备可见时间
    // discoveryIntent.putExtra(BluetoothAdapter.EXTRA_DISCOVERABLE_DURATION, 300);//设置持续时间（最多300秒）
    activity.startActivityForResult(enableBtIntent, 2);
}
```

### 强制关闭 ###

``` bash
BluetoothAdapter.getDefaultAdapter().disable();
```

### 蓝牙是否打开 ###

``` bash
BluetoothAdapter.getDefaultAdapter().isEnabled();
```

### 获取以配对的蓝牙设备 ###

``` bash
// 首次连接某蓝牙设备需要先配对，一旦配对成功，该设备的信息会被保存，以后连接时无需再配对，但是已配对的设备不一定是能连接的。
Set<BluetoothDevice> devices = BluetoothAdapter.getDefaultAdapter().getBondedDevices();
```

### 蓝牙广播接收器 ###

``` bash
public BroadcastReceiver receiver = new BroadcastReceiver() {    
    @Override    
    public void onReceive(Context context, Intent intent) {    
    	String action = intent.getAction();
        BluetoothDevice device = intent.getParcelableExtra(BluetoothDevice.EXTRA_DEVICE);
        Log.i("XC", "---:---" + action);
        if(BluetoothDevice.ACTION_FOUND.equals(action)) {
            Log.i("XC", device.getName() + "ACTION_FOUND");
            Toast.makeText(context, device.getName() + " 发现设备", Toast.LENGTH_LONG).show();
        } else if (BluetoothDevice.ACTION_ACL_CONNECTED.equals(action)) {
            Log.i("XC", device.getName() + "ACTION_ACL_CONNECTED");
            Toast.makeText(context, device.getName() + "该设备已连接", Toast.LENGTH_LONG).show();
            intent.putExtra("Bluetooth", "btMessage");
            intent.setClass(context, OrderDetailsActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            context.startActivity(intent);
        } else if (BluetoothDevice.ACTION_ACL_DISCONNECT_REQUESTED.equals(action)) {
            Log.i("XC", device.getName() + "ACTION_ACL_DISCONNECT_REQUESTED");
            Toast.makeText(context, device.getName() + "该设备断开连接2", Toast.LENGTH_LONG).show();
        } else if (BluetoothDevice.ACTION_ACL_DISCONNECTED.equals(action)) {
            Log.i("XC", device.getName() + "ACTION_ACL_DISCONNECTED");
            Toast.makeText(context, device.getName() + "该设备断开连接1", Toast.LENGTH_LONG).show();
        } else if (BluetoothAdapter.ACTION_STATE_CHANGED.equals(action)) {
        	if (getBluetoothAdapter().getState() == BluetoothAdapter.STATE_ON) {
        		Log.i("XC", "蓝牙打开");
            } else if (getBluetoothAdapter().getState() == BluetoothAdapter.STATE_OFF) {
            	Log.i("XC", "蓝牙关闭");
            }
        }
    }    
};

// 注册蓝牙监听广播
public void initIntentFilter(Context context) {    
    // 设置广播信息过滤    
    IntentFilter intentFilter = new IntentFilter();
    intentFilter.addAction(BluetoothDevice.ACTION_FOUND);
    intentFilter.addAction(BluetoothDevice.ACTION_ACL_CONNECTED);
    intentFilter.addAction(BluetoothDevice.ACTION_ACL_DISCONNECT_REQUESTED);
    intentFilter.addAction(BluetoothDevice.ACTION_ACL_DISCONNECTED);
    intentFilter.addAction(BluetoothAdapter.ACTION_STATE_CHANGED);
    // 注册广播接收器，接收并处理搜索结果
    context.registerReceiver(receiver, intentFilter);
}

// 取消注册蓝牙监听广播
public void unregisterReceiver(Context context) {
	context.unregisterReceiver(receiver);
}
```

## 音视频 ##

### 声音处理 ###

#### 播放系统.ogg文件 ####

``` bash
// 需要注意的是，如果MediaPlayer实例是由create方法创建的，那么第一次启动播放前不需要再调用prepare（）了，因为create方法里已经调用过了。
public void playOgg(Context context, int resid) {
	// 按键声音播放设置及初始化
	boolean mDTMFToneEnabled = false;         // 系统参数“按键操作音”标志位
	try {
		// 获取系统参数“按键操作音”是否开启(1:开,0:关)
		mDTMFToneEnabled = Settings.System.getInt(context.getContentResolver(), Settings.System.DTMF_TONE_WHEN_DIALING, 1) == 1;
		Log.i(TAG, "系统按键操作音是否开启" + mDTMFToneEnabled);
		synchronized (mToneGeneratorLock) {
			if (mDTMFToneEnabled) {
				// “按键操作音”是否开启的
				return ;
			}
		}
	} catch (Exception e) {
		 Log.d(TAG, e.getMessage());
		 mDTMFToneEnabled = false;
	}
	if (mediaPlayer == null) {
		mediaPlayer = MediaPlayer.create(context, resid);
	}
	Log.i(TAG, "mediaPlayer=" + mediaPlayer);
	// 播放
	play();
}

// 播放
private void play() {
	try {
		mediaPlayer.setAudioStreamType(AudioManager.STREAM_MUSIC);
		mediaPlayer.setVolume(BEEP_VOLUME, BEEP_VOLUME);
		mediaPlayer.setOnPreparedListener(preparedListener);
		mediaPlayer.setOnErrorListener(errorListener);
		// 当MediaPlayer调用seek()方法时触发该监听器
//			mediaPlayer.setOnSeekCompleteListener(seekCompleteListener);
		// Media Player的播放完成事件绑定事件监听器
		// When the beep has finished playing, rewind to queue up another one.
		mediaPlayer.setOnCompletionListener(conpletionListener);
	} catch (Exception  e) {
		e.printStackTrace();
	}
}

// 监听资源是否准备好
OnPreparedListener preparedListener = new OnPreparedListener() {
	@Override
	public void onPrepared(MediaPlayer mp) {
		Log.i(TAG, "onPrepared--->start()");
		// 启动文件播放的方法，
		mediaPlayer.start();
		// 定位方法，可以让播放器从指定的位置开始播放,注意的是该方法是个异步方法
//			mp.seekTo(0);
	}
};

// 监听播放是否seekTo()
OnSeekCompleteListener seekCompleteListener = new OnSeekCompleteListener() {
	@Override
	public void onSeekComplete(MediaPlayer mp) {
		Log.i(TAG, "onSeekComplete");
	}
};

// 监听播放是否完成
OnCompletionListener conpletionListener = new OnCompletionListener() {
	@Override
	public void onCompletion(MediaPlayer mp) {
		Log.i(TAG, "onCompletion");
		mediaPlayer.release();
		mediaPlayer = null;
	}
};

// 监听播放是否异常
OnErrorListener errorListener = new OnErrorListener() {
	@Override
	public boolean onError(MediaPlayer mp, int what, int extra) {
		Log.i(TAG, "onError");
		mediaPlayer.release();
		mediaPlayer = null;
		return false;
	}
};
```

## 密码格式是否正确 ##

1. 8到10位并且包含字母和数字

``` bash
boolean isTrue = Pattern.compile("^(?=.*[0-9])(?=.*[a-zA-Z]).{8,10}$").matcher(pass).matches();
```

2. 8到10位数字

``` bash
boolean isTrue = Pattern.compile("^(?=.*[0-9]).{8,10}$").matcher(pass).matches();
```

## 小数处理 ##

### 计算百分比 ###

``` bash
/**
 * 计算百分比
 * @param num 当前数
 * @param total 总和
 * @param scale 精确几位小数
 * @return
 */
public static String percent(double num, double total, int scale) {
	DecimalFormat df = (DecimalFormat) NumberFormat.getInstance();
	// 可以设置精确几位小数
    df.setMaximumFractionDigits(scale);
    // 模式 例如四舍五入
    df.setRoundingMode(RoundingMode.HALF_UP);
    double accuracy_num = num / total * 100; 
    return df.format(accuracy_num);
}
```

### 按pattern格式化小数 ###

``` bash
/**
 * 按pattern格式化小数
 * @param value 小数
 * @param pattern 格式化类型
 * @return
 */
public static String formatDecimal(double value, String pattern) {
	DecimalFormat df = (DecimalFormat) NumberFormat.getInstance();
	// 数值类型
	df.applyPattern(pattern);
	// 模式 例如四舍五入
	df.setRoundingMode(RoundingMode.HALF_UP);
	return df.format(value);
}
```

``` bash
// 通过下面调用
formatDecimal(value, "0.00");
```

## 程序是否在前台运行 ##

``` bash
public boolean isAppOnForeground(Context context) {
	// Returns a list of application processes that are running on the device
	ActivityManager activityManager = (ActivityManager) context
			.getApplicationContext().getSystemService(
					Context.ACTIVITY_SERVICE);
	String packageName = context.getApplicationContext().getPackageName();
	List<RunningAppProcessInfo> appProcesses = activityManager
			.getRunningAppProcesses();
	if (appProcesses == null) {
		return false;
	}
	for (RunningAppProcessInfo appProcess : appProcesses) {
		// The name of the process that this object is associated with.
		if (appProcess.processName.equals(packageName)
				&& appProcess.importance == RunningAppProcessInfo.IMPORTANCE_FOREGROUND) {
			return true;
		}
	}
	return false;
}
```

## 关闭软键盘 ##

### 软键盘(一) ###

``` bash
/**
 * 切换输入法的显示和隐藏状态，这里做了判断，所以默认情况下只能够隐藏键盘
 * 
 * 这个是让输入法状态发生逆转，如果当前未显示则显示出来。如果显示出来，则隐藏。
 */
public static void hideSoftKeyboard() {
	InputMethodManager mInputMethodManager = (InputMethodManager) FrameworkController.getInstance()
			.getSystemService(Context.INPUT_METHOD_SERVICE);
	if (mInputMethodManager.isActive()) {
		mInputMethodManager.toggleSoftInput(InputMethodManager.SHOW_IMPLICIT, InputMethodManager.HIDE_NOT_ALWAYS);
	}
}
```

### 软键盘(二) ###

``` bash
// 隐藏android键盘
/**
 * imm.hideSoftInputFromInputMethod(getActivity().getCurrentFocus().getWindowToken(), 0);
 * 这个经本人在android4.2机子上测试无效。
 * @param view   为EditText（passwdEdit）
 */
@Deprecated
public static void hideSoftKeyboard2(View view) {
	InputMethodManager mInputMethodManager = (InputMethodManager) FrameworkController.getInstance()
			.getSystemService(Context.INPUT_METHOD_SERVICE);
	if (null != view) {
		mInputMethodManager.hideSoftInputFromInputMethod(view.getWindowToken(), InputMethodManager.HIDE_NOT_ALWAYS);			
	}
}
```

### 软键盘(三) ###

``` bash
/**
 * 影藏软键盘
 * mInputMethodManager.hideSoftInputFromWindow(view.getWindowToken(), 0);
 * 
 * 经过测试，这是唯一有效的方法！
 * @param view   编辑的EditText
 */
public static void hideSoftKeyboard3(View view) {
	InputMethodManager mInputMethodManager = (InputMethodManager) FrameworkController.getInstance()
			.getSystemService(Context.INPUT_METHOD_SERVICE);
	if (null != view) {
		mInputMethodManager.hideSoftInputFromWindow(view.getWindowToken(), 0);			
	}
}
```

## 改变透明度 ##

``` bash
/**
 * 设置添加屏幕的背景透明度 
 * @param activity 当前Activity
 * @param bgAlpha 透明度值0.0-1.0
 */
public static void backgroundAlpha(Activity activity, float bgAlpha) {  
    WindowManager.LayoutParams lp = activity.getWindow().getAttributes();  
    lp.alpha = bgAlpha; //0.0-1.0  
    activity.getWindow().setAttributes(lp);  
}
```
 
## 获取每个应用程序最高可用内存 ##

``` bash
// 可以通过以下代码获取当前应用程序最高可用内存
int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);
Log.d("TAG", "Max memory is " + maxMemory + "kb");
```
