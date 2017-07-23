Appium Python Client
====================

An extension library for adding [Selenium 3.0 draft](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html) and [Mobile JSON Wire Protocol Specification draft](https://code.google.com/p/selenium/source/browse/spec-draft.md?repo=mobile)
functionality to the Python language bindings, for use with the mobile testing
framework [Appium](https://appium.io).

作为一个[Selenium 3.0 草案](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html)和[移动JSON线协议规范草案](https://code.google.com/p/selenium/source/browse/spec-draft.md?repo=mobile)

# Getting the Appium Python client
# 获取Appium python 客户端
There are three ways to install and use the Appium Python client.

这里有三种方式安装和使用Appiun客户端
1. Install from [PyPi](https://pypi.python.org/pypi), as
['Appium-Python-Client'](https://pypi.python.org/pypi/Appium-Python-Client).

1.通过[PyPi](https://pypi.python.org/pypi)上进行安装[Appium-Python-Client](https://pypi.python.org/pypi/Appium-Python-Client)

    ```shell
    pip install Appium-Python-Client
    ```

2. Install from source, via [PyPi](https://pypi.python.org/pypi). From ['Appium-Python-Client'](https://pypi.python.org/pypi/Appium-Python-Client),
download and unarchive the source tarball (Appium-Python-Client-X.X.tar.gz).

2. 源文件安装，通过[PyPi](https://pypi.python.org/pypi)下载['Appium Python客户端'](https://pypi.python.org/pypi/Appium-Python-Client)
并解压压缩包(Appium-Python-Client-X.X.tar.gz)

    ```shell
    tar -xvf Appium-Python-Client-X.X.tar.gz
    cd Appium-Python-Client-X.X
    python setup.py install
    ```

3. Install from source via [GitHub](https://github.com/appium/python-client).

3. [GitHub](https://github.com/appium/python-client)源文件安装
    ```shell
    git clone git@github.com:appium/python-client.git
    cd python-client
    python setup.py install
    ```


# Usage
# 用法
The Appium Python Client is fully compliant with the Selenium 3.0 specification
draft, with some helpers to make mobile testing in Python easier. The majority of
the usage remains as it has been for Selenium 2 (WebDriver), and as the [official
Selenium Python bindings](https://pypi.python.org/pypi/selenium) begins to
implement the new specification that implementation will be used underneath, so
test code can be written that is utilizable with both bindings。


Appium Python客户端严格遵循Selenium 3.0规范，再加上一些帮助文档在python上实现移动测试更加简单。
大多数的用法从Selenium2(WebDriver)延续了下来,而作为[official Selenium Python bindings](https://pypi.python.org/pypi/selenium)
开始实现新的规范，该实现将被使用在底层。所以，可以使用两种绑定来编写测试代码。

To use the new functionality now, and to use the superset of functions, instead of
including the Selenium `webdriver` module in your test code, use that from
Appium instead.

开始使用新的功能，并且使用超集函数。不是Selenium测试代码中的'webdriver'模块，而是使用Appium代替


```python
from appium import webdriver
```

From there much of your test code will work with no change.
大多数代码将不会被改变

As a base for the following code examples, the following sets up the [UnitTest](https://docs.python.org/2/library/unittest.html)
environment:

下面的代码最为一个基础的实例，建立[UniTest](https://docs.python.org/2/library/unittest.html)环境：

```python
# Android environment
import unittest
from appium import webdriver

desired_caps = {}
desired_caps['platformName'] = 'Android'
desired_caps['platformVersion'] = '4.2'
desired_caps['deviceName'] = 'Android Emulator'
desired_caps['app'] = PATH('../../../apps/selendroid-test-app.apk')

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
```

```python
# iOS environment
import unittest
from appium import webdriver

desired_caps = {}
desired_caps['platformName'] = 'iOS'
desired_caps['platformVersion'] = '7.1'
desired_caps['deviceName'] = 'iPhone Simulator'
desired_caps['app'] = PATH('../../apps/UICatalog.app.zip')

self.driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)
```


## Changed or added functionality
## 修改或增加功能

The methods that do change are...

修改的方法是...


### Switching between 'Native' and 'Webview'

### 'Native'和'Webview'之间的切换
For mobile testing the Selenium methods for switching between windows was previously
commandeered for switching between native applications and webview contexts. Methods
explicitly for this have been added to the Selenium 3 specification, so moving
forward these 'context' methods are to be used.

对于移动测试，之前在窗口之间切换的Selenium方法是被强征的：本地应用程序和webview上下文之间的切换
Selenium3规范中明确增加了这个方法。因此需要移动来使用这些上context的方法


To get the current context, rather than calling `driver.current_window_handle` you
use

获取当前的context，不再是调用‘driver.current_window_handle’

```python
current = driver.current_context
```

The available contexts are not retrieved using `driver.window_handles` but with

使用‘driver.window_handles’时，可用的contexts没有被检索到，可以使用


```python
driver.contexts
```

Finally, to switch to a new context, rather than `driver.switch_to.window(name)`,
use the comparable context method

最后，使用 comparable context方法去切换一个新的context，而不是使用`driver.switch_to.window(name)`

```python
context_name = "WEBVIEW_1"
driver.switch_to.context(context_name)
```


### Finding elements by iOS UIAutomation search
### 通过iOS UiAutomation查找元素

This allows elements in iOS applications to be found using recursive element
search using the UIAutomation library. This method is supported on iOS devices
that still support UIAutomation, that is, versions which predate XCUITEST.



Adds the methods `driver.find_element_by_ios_uiautomation`
and `driver.find_elements_by_ios_uiautomation`.

```python
el = self.driver.find_element_by_ios_uiautomation('.elements()[0]')
self.assertEqual('UICatalog', el.get_attribute('name'))
```

```python
els = self.driver.find_elements_by_ios_uiautomation('.elements()')
self.assertIsInstance(els, list)
```


### Finding elements by Android UIAutomator search
### 通过Android UiAutomator查找元素

This allows elements in an Android application to be found using recursive element
search using the UIAutomator library. Adds the methods `driver.find_element_by_android_uiautomator`
and `driver.find_elements_by_android_uiautomator`.

可以使用UiAutomator的库递归搜索android应用界面上所有的元素。添加`driver.find_element_by_android_uiautomator`
和`driver.find_elements_by_android_uiautomator`。

```python
el = self.driver.find_element_by_android_uiautomator('new UiSelector().description("Animation")')
self.assertIsNotNone(el)
```

```python
els = self.driver.find_elements_by_android_uiautomator('new UiSelector().clickable(true)')
self.assertIsInstance(els, list)
```


### Finding elements by iOS predicates
### 通过iOS predicates查看元素

This method allows finding elements using iOS predicates. The methods take a
string in the format of a predicate, including element type and the value of
fields.

使用iOS predicates这种方法允许查找元素。

Adds the methods
`driver.find_element_by_ios_predicate` and `find_elements_by_ios_predicate`.

```python
el = self.driver.find_element_by_ios_predicate('wdName == "Buttons"')
self.assertIsNotNone(el)
```

```python
els = self.driver.find_elements_by_ios_predicate('wdValue == "SearchBar" AND isWDDivisible == 1')
self.assertIsInstance(els, list)
```


### Finding elements by iOS class chain
### 通过iOS的class chain查找元素

**This method is only for [XCUITest driver](https://github.com/appium/appium-xcuitest-driver)**

This method allows finding elements using iOS class chain. The methods take
a string in the format of a class chain, including element type.

Adds the methods
`driver.find_element_by_ios_class_chain` and `find_elements_by_ios_class_chain`.

```python
el = self.driver.find_element_by_ios_class_chain('XCUIElementTypeWindow/XCUIElementTypeButton[3]')
self.assertIsNotNone(el)
```

```python
els = self.driver.find_elements_by_ios_class_chain('XCUIElementTypeWindow/XCUIElementTypeButton')
self.assertIsInstance(els, list)
```

### Finding elements by Accessibility ID


Allows for elements to be found using the "Accessibility ID". The methods take a
string representing the accessibility id or label attached to a given element, e.g., for iOS the accessibility identifier and for Android the content-description. Adds the methods
`driver.find_element_by_accessibility_id` and `find_elements_by_accessibility_id`.

```python
el = self.driver.find_element_by_accessibility_id('Animation')
self.assertIsNotNone(el)
```

```python
els = self.driver.find_elements_by_accessibility_id('Animation')
self.assertIsInstance(els, list)
```


### Touch actions
### 触摸操作

In order to accommodate mobile touch actions, and touch actions involving
multiple pointers, the Selenium 3.0 draft specifies ["touch gestures"](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html#touch-gestures) and ["multi actions"](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html#multiactions-1), which build upon the touch actions.

为了支持移动触摸操作和多点触摸操作。Selenium3.0草案中指定了[手势触摸](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html#touch-gestures)和[多点触摸](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html#multiactions-1),
只要建立在这些触摸操作之上，都能很好的支持


move_to: note that use keyword arguments if no element

move_to: 注意，如果没有元素请使用关键字参数

The API is built around `TouchAction` objects, which are chains of one or more actions to be performed in a sequence. The actions are:

这个API是围绕‘TouchAction’对象构建的，每一个链都是在一个序列中的一个或多个动作。这个Action是：

#### `perform`
#### 'perform'

The `perform` method sends the chain to the server in order to be enacted. It also empties the action chain, so the object can be reused. It will be at the end of all single action chains, but is unused when writing multi-action chains.

‘perform’方法的目的是为了制定链到服务之间的发送。它也会清空这个动作链，所以对象可以被重用。它将在所有单一动作链的末端，但是在编写多动作链的时候，它是没用的


#### `tap`
#### ‘tap’

The `tap` method stands alone, being unable to be chained with other methods. If you need a `tap`-like action that starts a longer chain, use `press`.

It can take either an element with an optional x-y offset, or absolute x-y coordinates for the tap, and an optional count.

```python
el = self.driver.find_element_by_accessibility_id('Animation')
action = TouchAction(self.driver)
action.tap(el).perform()
el = self.driver.find_element_by_accessibility_id('Bouncing Balls')
self.assertIsNotNone(el)
```

#### `press`

#### `long_press`

#### `release`

#### `move_to`

#### `wait`

#### `cancel`


### Multi-touch actions
### 多点触摸操作

In addition to chains of actions performed with in a single gesture, it is also possible to perform multiple chains at the same time, to simulate multi-finger actions. This is done through building a `MultiAction` object that comprises a number of individual `TouchAction` objects, one for each "finger".

Given two lists next to each other, we can scroll them independently but simultaneously:

```python
els = self.driver.find_elements_by_class_name('listView')
a1 = TouchAction()
a1.press(els[0]) \
    .move_to(x=10, y=0).move_to(x=10, y=-75).move_to(x=10, y=-600).release()

a2 = TouchAction()
a2.press(els[1]) \
    .move_to(x=10, y=10).move_to(x=10, y=-300).move_to(x=10, y=-600).release()

ma = MultiAction(self.driver, els[0])
ma.add(a1, a2)
ma.perform();
```

### Appium-Specific touch actions

There are a small number of operations that mobile testers need to do quite a bit that can be relatively complicated to build using the Touch and Multi-touch Action API.  For these we provide some convenience methods in the Appium client.

#### `driver.tap`

This method, on the WebDriver object, allows for tapping with multiple fingers, simply by passing in an array of x-y coordinates to tap.

```python
el = self.driver.find_element_by_name('Touch Paint')
action.tap(el).perform()

# set up array of two coordinates
positions = []
positions.append((100, 200))
positions.append((100, 400))

self.driver.tap(positions)
```

#### `driver.swipe`

Swipe from one point to another point.

#### `driver.zoom`

Zoom in on an element, doing a pinch out operation.

#### `driver.pinch`

Zoom out on an element, doing a pinch in operation.



### Application management methods
### 应用管理方法

There are times when you want, in your tests, to manage the running application,
such as installing or removing an application, etc.

在测试中，有些时候，你想要管理、运行应用，比如：安装或者移除一个应用等等

####   an application
####   一个应用

The method `driver.background_app` sends the running application to the background
for the specified amount of time, in seconds. After that time, the application is
brought back to the foreground.

‘river.background_app()这个方法，可以使一个正在运行的应用进入到后台模式并指定后台模式的时间(短时间范围内)，
指定时间结束后，应用会返回到前台模式

```python
driver.background_app(1)
sleep(2)
el = driver.find_element_by_name('Animation')
assertIsNotNone(el)
```


#### Checking if an application is installed
#### 检查是否安装了应用程序

To check if an application is currently installed on the device, use the `device.is_app_installed`
method. This method takes the bundle id of the application and return `True` or
`False`.

device.is_app_installed()方法，检查一个应用程序是否被安装在当前设备上。这个方法将会绑定应用ID，
以及返回值为"true"或"false"

```python
assertFalse(self.driver.is_app_installed('sdfsdf'))
assertTrue(self.driver.is_app_installed('com.example.android.apis'))
```


#### Installing an application
#### 安装一个应用
To install an uninstalled application on the device, use `device.install_app`,
sending in the path to the application file or archive.

device.install_app()方法：用于在设备上安装一个未安装的应用，
参数传入一个应用的路径或者档案文件。
```python
assertFalse(driver.is_app_installed('io.selendroid.testapp'))
driver.install_app('/Users/isaac/code/python-client/test/apps/selendroid-test-app.apk')
assertTrue(driver.is_app_installed('io.selendroid.testapp'))
```


#### Removing an application
#### 移除一个应用
If you need to remove an application from the device, use `device.remove_app`,
passing in the application id.

device.remove_app()方法,用于在设备上移除一个应用，传入应用的包名

```python
assertTrue(driver.is_app_installed('com.example.android.apis'))
driver.remove_app('com.example.android.apis')
assertFalse(driver.is_app_installed('com.example.android.apis'))
```


#### Closing and Launching an application
#### 关闭和启动一个应用程序
To launch the application specified in the desired capabilities, call `driver.launch_app`.
Closing that application is initiated by `driver.close_app`

driver.launch_app()方法，用于在一个desired capabilities中去启动一个应用程序
driver.close_app()方法，用于在一个desired capabilities中去关闭一个应用程序
```python
el = driver.find_element_by_name('Animation')
assertIsNotNone(el)
driver.close_app();

try:
    driver.find_element_by_name('Animation')
except Exception as e:
    pass # should not exist

driver.launch_app()
el = driver.find_element_by_name('Animation')
assertIsNotNone(el)
```

#### Resetting an application
#### 重置一个应用
To reset the running application, use `driver.reset`.
driver.reset()方法，用于重置正在运行的应用

```python
el = driver.find_element_by_name('App')
el.click()

driver.reset()
sleep(5)

el = driver.find_element_by_name('App')
assertIsNotNone(el)
```


### Other methods
### 其他的方法

#### Start an arbitrary activity
#### 开始一个任意Activiy

The `driver.start_activity` method opens arbitrary activities on a device.
If the activity is not part of the application under test, it will also
launch the activity's application.

driver.start_activity("包名","Activity名")方法，在设备上打开任意一个Activity
如果这个Activity不是测试应用中的一部分，它也会启动这个Activity

```python
driver.start_activity('com.foo.app', '.MyActivity')
```


#### Retrieving application strings
#### 获取应用程序的字符串

The property method `driver.app_strings` returns the application strings from
the application on the device.

driver.app_strings属性方法，可以返回应用在设备上显示的字符串

```python
strings = driver.app_strings
```


#### Sending a key event to an Android device
#### 向android设备中发送一个keyCode事件
The `driver.keyevent` method sends a keycode to the device. The keycodes can be
found [here](http://developer.android.com/reference/android/view/KeyEvent.html).
Android only.
driver.keyevent方法，像一个设备中发送keycode事件。关于keycodes事件可以查找[here](http://developer.android.com/reference/android/view/KeyEvent.html)
只适用于android

```python
# sending 'Home' key event
driver.press_keycode(3)
```


#### Hiding the keyboard in iOS
#### ios中隐藏的按键

To hide the keyboard from view in iOS, use `driver.hide_keyboard`. If a key name
is sent, the keyboard key with that name will be pressed. If no arguments are
passed in, the keyboard will be hidden by tapping on the screen outside the text
field, thus removing focus from it.



```python
# get focus on text field, so keyboard comes up
el = driver.find_element_by_class_name('android.widget.TextView')
el.set_value('Testing')

el = driver.find_element_by_class_name('keyboard')
assertTrue(el.is_displayed())

driver.hide_keyboard('Done')

assertFalse(el.is_displayed())
```

```python
# get focus on text field, so keyboard comes up
el = driver.find_element_by_class_name('android.widget.TextView')
el.set_value('Testing')

el = driver.find_element_by__name('keyboard')
assertTrue(el.is_displayed())

driver.hide_keyboard()

assertFalse(el.is_displayed())
```


#### Retrieving the current running package and activity
#### 获取当前运行应用的包名和Activity名

The property method `driver.current_package` returns the name of the current
package running on the device.

driver.current_package属性方法，可以返回正在设备当前界面上运行的应用程序的包名

```python
package = driver.current_package
assertEquals('com.example.android.apis', package)
```

The property method `driver.current_activity` returns the name of the current
activity running on the device.

driver.current_activity属性方法，可以返回正在设备当前界面上运行的应用程序的Activity名

```python
activity = driver.current_activity
assertEquals('.ApiDemos', activity)
```


#### Set a value directly on an element
#### 在一个控件元素上设置直面量

Sometimes one needs to directly set the value of an element on the device. To do
this, the method `driver.set_value` or `element.set_value` is invoked.

element.set_value()属性方法，有事需要在设备上的控件元素中设置一个直面量，
可以使用该方法，也可以使用driver.set_value()

```python
el = driver.find_element_by_class_name('android.widget.EditText')
driver.set_value(el, 'Testing')

text = el.get_attribute('text')
assertEqual('Testing', text)

el.set_value('More testing')
text = el.get_attribute('text')
assertEqual('More testing', text)
```


#### Retrieve a file from the device
#### 从设备中获取(pull)一个文件

To retrieve the contents of a file from the device, use `driver.pull_file`, which
returns the contents of the specified file encoded in [Base64](https://docs.python.org/2/library/base64.html).

driver.pull_file()属性方法，用于从设备中获取一个文件的内容，返回指定文件编码[Base64](https://docs.python.org/2/library/base64.html)的内容

```python
# pulling the strings file for our application
data = driver.pull_file('data/local/tmp/strings.json')
strings = json.loads(data.decode('base64', 'strict'))
assertEqual('You can\'t wipe my data, you are a monkey!', strings[u'monkey_wipe_data'])
```


#### Place a file on the device
#### 向设备中push一个文件

To put a file onto the device at a particular place, use the `driver.push_file`
method, which takes the path and the data, encoded as [Base64](https://docs.python.org/2/library/base64.html), to be written to the file.

driver.push_file()方法，向设备中push一个文件到指定的目录上，需要指定路径和数据，使用[Base64](https://docs.python.org/2/library/base64.html)编码写入文件
```python
path = 'data/local/tmp/test_push_file.txt'
data = 'This is the contents of the file to push to the device.'
driver.push_file(path, data.encode('base64'))
data_ret = driver.pull_file('data/local/tmp/test_push_file.txt').decode('base64')
self.assertEqual(data, data_ret)
```


#### End test coverage
#### 测试报告

There is functionality in the Android emulator to instrument certain activities.
For information on this, see the [Appium docs](https://github.com/appium/appium/blob/master/docs/en/android_coverage.md). To end this coverage
and retrieve the data, use `driver.end_test_coverage`, passing in the `intent`
that is being instrumentalized, and the path to the `coverage.ec` file on the
device.

android模拟器的instrument一些功能可以测试某些活动 ，这个信息可以查看[Apium docs](https://github.com/appium/appium/blob/master/docs/en/android_coverage.md)。
结束这个测试，获取测试数据，使用driver.end_test_coverage()方法,通过‘intent’初始化工具，path指定输出报告在设备上的存储目录


```python
coverage_ec_file = driver.end_test_coverage(intent='android.intent.action.MAIN', path='')
```


#### Lock the device
#### 设备锁

To lock the device for a certain amount of time, on iOS, use `driver.lock`. The
argument is the number of seconds to wait before unlocking.

iso中的driver.lock(argument) 方法，这个参数是在解锁钱等待的秒数

#### Shake the device
#### 设备震动

To shake the device, use `driver.shake`.

driver.shake(),震动设备

#### Appium Settings
#### Appium设置

Settings are a new concept introduced by appium. They are currently not a part of the Mobile JSON Wire Protocol, or the Webdriver spec.

设置是Appium引入的新观点，并不是Mobile JSON Wire Protocol和webdriver规范的一部分

Settings are a way to specify the behavior of the appium server.

设置是一种指定Appium服务行为的方法。

Settings are:

Mutable, they can be changed during a session
Only relevant during the session they are applied. They are reset for each new session.
Control the way the appium server behaves during test automation. They do not apply to controlling the app or device under test.

See [the docs](https://github.com/appium/appium/blob/master/docs/en/advanced-concepts/settings.md) for more information.

To get settings:
```python
settings = driver.get_settings()
```

To set settings:
```python
driver.update_settings({"some setting": "the value"})
```
