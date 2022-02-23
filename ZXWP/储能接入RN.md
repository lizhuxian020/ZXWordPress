# 打包
`node node_modules/react-native/local-cli/cli.js bundle --platform ios --dev false --reset-cache --entry-file index.js --bundle-output ./bundle/ios/index.ios.jsbundle --assets-dest ./bundle/ios`
打出来的jsbundle和asset要放到xcode, asset要用create reference, 蓝色文件夹

#搭建RN环境过程
先确定RN版本: 0.57.8, React:16.6.3
引入react-navigation: 去github里找. 找到合适RN的版本, 
