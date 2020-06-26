# Install

- ubuntu.sh
- ROS install



# Build 

## jmavsim simulation

- Build successful but not working!
- Seems problem in GUI environment

```
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by javax.media.j3d.JoglPipeline (rsrc:j3dcore.jar) to method sun.awt.AppContext.getAppContext()
WARNING: Please consider reporting this to the maintainers of javax.media.j3d.JoglPipeline
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
Inconsistency detected by ld.so: dl-lookup.c: 111: check_match: Assertion `version->filename == NULL || ! _dl_name_match_p (version->filename, map)' failed!
```

- Try1: change java version and rebuild

```
sudo apt install openjdk-8-jdk # install java 8
sudo update-alternatives --config java # choose java 8
rm -rf Tools/jMAVSim/out
```


Also there are exception

```
Exception in thread "main" java.lang.reflect.InvocationTargetException
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.eclipse.jdt.internal.jarinjarloader.JarRsrcLoader.main(JarRsrcLoader.java:61)
Caused by: java.awt.AWTError: Assistive Technology not found: org.GNOME.Accessibility.AtkWrapper
	at java.awt.Toolkit.loadAssistiveTechnologies(Toolkit.java:807)
	at java.awt.Toolkit.getDefaultToolkit(Toolkit.java:886)
	at java.awt.Window.getToolkit(Window.java:1358)
	at java.awt.Window.init(Window.java:506)
	at java.awt.Window.<init>(Window.java:537)
	at java.awt.Frame.<init>(Frame.java:420)
	at java.awt.Frame.<init>(Frame.java:385)
	at javax.swing.JFrame.<init>(JFrame.java:189)
	at me.drton.jmavsim.Visualizer3D.<init>(Visualizer3D.java:107)
	at me.drton.jmavsim.Simulator.<init>(Simulator.java:185)
	at me.drton.jmavsim.Simulator.main(Simulator.java:930)
	... 5 more
```

- change settings

open property files

```
sudo gedit /etc/java-8-openjdk/accessibility.properties 
```
And comment out follo

```
#assistive_technologies=org.GNOME.Accessibility.AtkWrapper
```

- Reference 
- https://github.com/PX4/jMAVSim/issues/96
- https://github.com/PX4/Firmware/issues/9557 


### with flight controller PS4



## Build with gazebo

```
make px4_sitl gazebo
```

- failed

<details>
<summary>最初に出たエラー</summary>

```
FAILED: CMakeFiles/gazebo_opticalflow_plugin.dir/src/gazebo_opticalflow_plugin.cpp.o 
/usr/bin/c++  -DLIBBULLET_VERSION=2.87 -DLIBBULLET_VERSION_GT_282 -Dgazebo_opticalflow_plugin_EXPORTS -isystem /usr/include/gazebo-9 -isystem /usr/include/bullet -isystem /usr/include/simbody -isystem /usr/include/sdformat-6.2 -isystem /usr/include/ignition/math4 -isystem /usr/include/OGRE -isystem /usr/include/OGRE/Terrain -isystem /usr/include/OGRE/Paging -isystem /usr/include/ignition/transport4 -isystem /usr/include/ignition/msgs1 -isystem /usr/include/ignition/common1 -isystem /usr/include/ignition/fuel_tools1 -isystem /usr/include/x86_64-linux-gnu/qt5 -isystem /usr/include/x86_64-linux-gnu/qt5/QtCore -isystem /usr/lib/x86_64-linux-gnu/qt5/mkspecs/linux-g++ -I/home/yoshi/Firmware/Tools/sitl_gazebo/include -I. -I/usr/include/eigen3 -I/usr/include/eigen3/eigen3 -I/usr/include/gazebo-9/gazebo/msgs -I/home/yoshi/Firmware/mavlink/include -isystem /usr/include/opencv -I/home/yoshi/Firmware/Tools/sitl_gazebo/external/OpticalFlow/include -I/usr/include/gstreamer-1.0 -I/usr/include/glib-2.0 -I/usr/lib/x86_64-linux-gnu/glib-2.0/include -isystem /usr/include/uuid -isystem /usr/include/x86_64-linux-gnu -I/home/yoshi/Firmware/Tools/sitl_gazebo/external/OpticalFlow/external/klt_feature_tracker/include -Wno-deprecated-declarations -fPIC   -I/usr/include/uuid -I/usr/include/x86_64-linux-gnu -std=gnu++1z -MD -MT CMakeFiles/gazebo_opticalflow_plugin.dir/src/gazebo_opticalflow_plugin.cpp.o -MF CMakeFiles/gazebo_opticalflow_plugin.dir/src/gazebo_opticalflow_plugin.cpp.o.d -o CMakeFiles/gazebo_opticalflow_plugin.dir/src/gazebo_opticalflow_plugin.cpp.o -c /home/yoshi/Firmware/Tools/sitl_gazebo/src/gazebo_opticalflow_plugin.cpp
In file included from /home/yoshi/Firmware/Tools/sitl_gazebo/src/gazebo_opticalflow_plugin.cpp:24:0:
/home/yoshi/Firmware/Tools/sitl_gazebo/src/gazebo_opticalflow_plugin.cpp: In member function ‘virtual void gazebo::OpticalFlowPlugin::Load(gazebo::sensors::SensorPtr, sdf::ElementPtr)’:
/home/yoshi/Firmware/Tools/sitl_gazebo/include/gazebo_opticalflow_plugin.h:43:18: error: ‘TRUE’ was not declared in this scope
 #define HAS_GYRO TRUE
                  ^
/home/yoshi/Firmware/Tools/sitl_gazebo/include/gazebo_opticalflow_plugin.h:43:18: note: in definition of macro ‘HAS_GYRO’
 #define HAS_GYRO TRUE
                  ^~~~
```
</details>



<details>
<summary>修正１</summary>

情報源：
https://github.com/PX4/sitl_gazebo/pull/408

- `/Firmware/Tools/sitl_gazebo/include/gazebo_opticalflow_plugin.h`のTRUEをtrueに。

```
error: ‘TRUE’ was not declared in this scope
#define HAS_GYRO TRUE

changing this to:
#define HAS_GYRO true
```
</details>


<details>
<summary>修正１後のエラー</summary>
```

```

</details>


# ROS 



