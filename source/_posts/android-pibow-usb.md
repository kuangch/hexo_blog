---
title: 树莓派的无法USB调式
date: 2016-01-21 17:28:19
tags:
- 树莓派
- USB
categories: android
---
## 问题简介：
由于香蕉派某芯片停产，要换为使用树莓派，但是树莓派连接电脑调试时，数据线插入电脑一直没有反应。
折腾了半天无果，（下载驱动，360手机助手，豌豆荚都试过了）。
## 找到原因：
和做硬件的哥们请教了一下，发现是端口默认设置成了OTG模式，导致无法连接数据线调试。
## 解决方案：
``` java
public  void change2usb(){
        File fileOTG = new File("/sys/devices/platform/sunxi_otg/otg_role");
        File fileUDC = new File("/sys/bus/platform/devices/sunxi_usb_udc/otg_role");
        if(fileOTG.exists()) {
            Log.i("USB_SWITCH", "note is /sys/devices/platform/sunxi_otg/otg_role");
            usb_role = "/sys/devices/platform/sunxi_otg/otg_role";
        } else if(fileUDC.exists()) {
            Log.i("USB_SWITCH", "note is /sys/bus/platform/devices/sunxi_usb_udc/otg_role");
            usb_role = "/sys/bus/platform/devices/sunxi_usb_udc/otg_role";
        }
        try {
            state = execCommand("cat " + usb_role);
        } catch(IOException e) {
            e.printStackTrace();
        }

        Log.i("USB_SWITCH", "try to set usb as device");
        try {
            FileWriter wr = new FileWriter(usb_role);
            wr.write("2");
            wr.close();
            return;
        } catch(IOException e) {
            Log.i("USB_SWITCH", " write \"  error: " + e.getMessage());
            return;
        }
        Log.i("USB_SWITCH", "try to set usb as host");
        try {
            FileWriter wr = new FileWriter(usb_role);
            wr.write("1");
            wr.close();
            return;
        } catch(IOException e) {
            Log.i("USB_SWITCH", " write \"  error: " + e.getMessage());

}
public String execCommand(String command) throws IOException {
    Runtime runtime = Runtime.getRuntime();
    Process proc = runtime.exec(command);
    InputStream inputstream = proc.getInputStream();
    InputStreamReader inputstreamreader = new InputStreamReader(inputstream);
    BufferedReader bufferedreader = new BufferedReader(inputstreamreader);
    while(line != null) {
        sb.append(line);
        sb.append(0xa);
    }
    Log.i("USB_SWITCH", "exec \"" + command + "\"");
    Log.i("USB_SWITCH", "output: ".append((line)).toString());
    try {
        if(proc.waitFor() != 0) {
            Log.e("USB_SWITCH", "exec \"" + command + "\" fail! exit value = " + proc.exitValue());
        }
    } catch(InterruptedException e) {
        System.err.println(e);
    }
    return sb.toString();
}
```
## apk下载地址：
[USBModeSwitch APK下载](http://pan.baidu.com/s/1dEdeR0t)