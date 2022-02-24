# 模拟器跑不起来

https://stackoverflow.com/questions/63607158/xcode-12-building-for-ios-simulator-but-linking-in-object-file-built-for-ios

1. 在project-target-buildSetting里“Excluded Architecture”， 设置好arm64
2. 在Pod-target-buildSetting里“Excluded Architecture”， 设置好arm64
3. 在podfile里添加以下（在target的同级目录下）,以下代码相当于给各个Pod库里的xcconfig文件添加这个句话`EXCLUDED_ARCHS[sdk=iphonesimulator*] = arm64
`

```
post_install do |installer|
      installer.pods_project.build_configurations.each do |config|
        config.build_settings["EXCLUDED_ARCHS[sdk=iphonesimulator*]"] = "arm64"
      end
    end
```
1. 在自己的podspec文件里添加

```
s.pod_target_xcconfig = { 'EXCLUDED_ARCHS[sdk=iphonesimulator*]' => 'arm64' }
s.user_target_xcconfig = { 'EXCLUDED_ARCHS[sdk=iphonesimulator*]' => 'arm64' }
```