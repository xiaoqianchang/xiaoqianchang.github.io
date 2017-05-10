title: Android插件化架构设计简述
date: 2016-03-16 17:29:29
categories: Android
tags: 
  - Android
  - 插件化
---

近期在整理插件化方面的知识，今天以换皮肤效果的例子总结如下：

## 插件开发好处 ##

 1. 有助于协同开发，有助于功能扩展等等...
 2. 如支付宝主界面都是模块分类，每一个入口就是插件

## 成为插件的要求 ##

 1. 插件程序是不需要程序主入口的
 2. 插件程序必须要遵循主程序的协议才能成为插件
 3. 插件开发不管插件是图片还是功能，它们都是通过类反射来找到插件资源
 4. 在manifest中具有相同的`android:sharedUserId`

<!--more-->

## 插件开发步骤 ##

 1. 新建一个工程，准备图片资源
 2. 新建布局文件
 3. 初始化UI主键
 4. 初始化popupWindow，新建popupWindow布局文件（提示框）
 5. 查找插件列表
 6. 显示皮肤列表
 7. 加载插件资源
 8. 规范插件资源协议
 9. 添加插件程序
 
## 要用到的知识点 ##
 1. android基本UI组件的使用
 2. 掌握android PackageManager类的基本使用(包括app的基本信息：应用程序的名称、应用程序的包名)
 3. 掌握android资源加载器(资源：图片、文字、字体大小、样式)
 4. 掌握如何定义插件开发协议(插件开发协议：图片命名规范、文字命名规范、类的命名规范)
 5. 掌握java类反射机制
 6. 掌握android shareuserid的使用
 7. 掌握android 插件架构设计
 
## 主程序 ##

### MainActivity ###

``` bash
public class MainActivity extends Activity implements OnClickListener,
		OnItemClickListener {

	private static final String TAG = MainActivity.class.getSimpleName();
	private ImageView bg;
	private Button plugin;

	List<Map<String, String>> pluginList;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		initView();

	}

	private void initView() {
		bg = (ImageView) findViewById(R.id.img_bg);
		plugin = (Button) findViewById(R.id.btn_plugin);
		plugin.setOnClickListener(this);

	}

	@Override
	public void onClick(View v) {
		View contentView = getLayoutInflater().inflate(R.layout.popupwindow,
				null);
		PopupWindow popupWindow = new PopupWindow(contentView,
				ViewGroup.LayoutParams.WRAP_CONTENT,
				ViewGroup.LayoutParams.WRAP_CONTENT);
		popupWindow.setBackgroundDrawable(getResources().getDrawable(
				R.drawable.ic_launcher));

		// 设置该属性后。我们点击提示框意外部分，提示框会消失
		popupWindow.setFocusable(true);
		popupWindow.setTouchable(true);

		// 查找插件
		pluginList = findPluginList();

		if (pluginList == null || pluginList.size() == 0) {
			Toast.makeText(this, "目前没有皮肤，请下载", Toast.LENGTH_SHORT).show();
			return;
		}

		// 显示皮肤
		ListView lv_plugins = (ListView) contentView
				.findViewById(R.id.lv_plugins);
		lv_plugins.setOnItemClickListener(this);
		SimpleAdapter simpleAdapter = new SimpleAdapter(this, pluginList,
				android.R.layout.simple_list_item_1, new String[] { "label" },
				new int[] { android.R.id.text1 });
		lv_plugins.setAdapter(simpleAdapter);

		// 设置popiuWindow的宽高
		popupWindow.setWidth(150);
		popupWindow.setHeight(pluginList.size() * 100);

		// 显示popupWindow（显示提示框）
		popupWindow.showAsDropDown(v);

	}

	private List<Map<String, String>> findPluginList() {
		List<Map<String, String>> list = new ArrayList<Map<String, String>>();
		// 获取包管理器
		PackageManager packageManager = this.getPackageManager();
		// 获取手机已安装的App包信息
		List<PackageInfo> installedPackages = packageManager
				.getInstalledPackages(PackageManager.GET_ACTIVITIES);

		try {
			// 获取当前app的包信息
			PackageInfo currentPackageInfo = this.getPackageManager()
					.getPackageInfo(getPackageName(), 0);
			for (PackageInfo packageInfo : installedPackages) {
				String packageName = packageInfo.packageName;
				String sharedUsId = packageInfo.sharedUserId;
				if (sharedUsId == null
						|| !sharedUsId.equals(currentPackageInfo.sharedUserId)
						|| packageName.equals(getPackageName())) {
					continue;
				}

				// 记载插件
				Map<String, String> pluginMap = new HashMap<String, String>();
				// 获取插件程序的名称
				String label = packageInfo.applicationInfo.loadLabel(
						packageManager).toString();
				// 获取包名称
				pluginMap.put("packageName", packageName);
				pluginMap.put("label", label);

				list.add(pluginMap);

			}

		} catch (NameNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

		return list;
	}

	@Override
	public void onItemClick(AdapterView<?> parent, View view, int position,
			long id) {
		// 加载插件资源
		// 1、获取插件上下文（其实就是获取插件程序的资源加载器
		// h获取当前要加载的插件
		Map<String, String> map = pluginList.get(position);
		// 获取插件上下文
		Context pluginContext = findPluginContext(map);

		// 2、根据插件资源加载器，加载图片资源
		int resId = findResourceId(pluginContext, map);

		if (resId != 0) {
			// 直接设置是不能加载到资源(解决办法：我们必须通过插件加载器来帮助我们加载资源)
			bg.setImageDrawable(pluginContext.getResources().getDrawable(resId));
			
		}

	}

	/**
	 * 获取插件上下文
	 * @param map
	 * @return
	 */
	private Context findPluginContext(Map<String, String> map) {
		Log.i(TAG, map.get("packageName") + "-------------------");
		try {
			return this.createPackageContext(map.get("packageName"),
					Context.CONTEXT_IGNORE_SECURITY);
		} catch (NameNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

		return null;
	}

	/**
	 * 根据插件资源加载器，加载图片资源
	 * 
	 * 查找资源id
	 * 
	 * 反射
	 * 
	 * @param pluginContext 插件上下文
	 * @param map 插件包名的Map
	 * @return
	 */
	private int findResourceId(Context pluginContext, Map<String, String> map) {
		String packageName = map.get("packageName");
		// 把ClassLoader包装一下，包装为插件的类加载器(Class.forName里面默认的classLoader是本类的类加载器)
		PathClassLoader classLoader = new PathClassLoader(
				pluginContext.getPackageResourcePath(),
				PathClassLoader.getSystemClassLoader());
		try {
			Class<?> forName = Class.forName(packageName + ".R$drawable", true,
					classLoader);
			Field[] fields = forName.getFields();
			for (Field field : fields) {
				// 找到想要的图片(我们所有插件的背景图片的名称：icon_main_bg---图片协议：main_bg)
				String name = field.getName();
				if (name.equals("icon_main_bg")) {
					return field.getInt(R.drawable.class);
				}

			}

		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IllegalArgumentException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

		return 0;
	}
}
```

### MainActivity的布局文件 ###

``` bash
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="${relativePackage}.${activityClass}" >

    <ImageView
        android:id="@+id/img_bg"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:scaleType="centerCrop"
        android:src="@drawable/ic_launcher"
        android:contentDescription="@null" />
    
    <Button
        android:id="@+id/btn_plugin"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="切换" />

</RelativeLayout>
```

## 插件程序 ##

插件程序没有程序入口，所以manifest中application节点不定义入口activity，并且与主程序有相同的`android:sharedUserId="com.example.plugin.background"`
