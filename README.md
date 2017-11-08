# TinkerDemo
  
## 集成步骤
1.  依赖引入

         dependencies {
                // ...
                //可选，用于生成application类
                provided('com.tencent.tinker:tinker-android-anno:1.7.7')
                //tinker的核心库
                compile('com.tencent.tinker:tinker-android-lib:1.7.7')
            }
2.  配置签名
3.  新建一个class extends ApplicationLike，并配置Application 名称，这里名称随便写，之后make project 会按照这个名字生成Application 类，然后在AndroidManifest.xml
4.  loadPatch，这里随便添加按钮点击才loadPatch

        public void loadPatch(View view) {
            TinkerInstaller.onReceiveUpgradePatch(getApplicationContext(),
                    Environment.getExternalStorageDirectory().getAbsolutePath() + "/patch_signed.apk");
        }
        注意权限：6.0还需要动态申请
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
5.  在AndroidManifest里面注册TICK ID
     
      <application>
      <meta-data
                android:name="TINKER_ID"
                android:value="tinker_id_6235657" />
        //...
    </application>
6.  生成patch
    使用tinker-patch-cli 工具

    需要注意的就是tinker_config.xml，里面包含tinker的配置，例如签名文件等。
    
    这里我们直接使用tinker提供的签名文件，所以不需要做修改，不过里面有个Application的item修改为与本例一致：
    
    <loader value="com.wangpos.tinkerdemo.SimpleApplication"/>


        java -jar tinker-patch-cli-1.7.7.jar -old old.apk -new new.apk -config tinker_config.xml -out output






## 问题

   1.运行安装报错；原因没有添加混淆。
   
   2.修复失败；
    
    1)6.0要添加写卡动态询问权限。
    2)两次mapping必须一致，签名必须一致。
    
   
   