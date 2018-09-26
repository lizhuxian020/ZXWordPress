#Data Formatting
数字, 日期, 尺寸和其他数值 <=> 本地化的字符串, 即互相转换.


#Topics
##数字和货币(Numbers and Currency)
* NSNumberFormatter

    * formatter: 数字 <=> 文本代表

##名字(Names)
* NSPersonNameComponentsFormatter
    * Formatter: 用于个人名字的组成本地化
* NSPersonNameComponents
    * 管理个人名字的分割部分, 进行本地格式化

    
##日期和时间(Dates and Times)
* NSDateFormatter
    * Formatter: 用于日期 <=> 文本代表, 之间互相转换
* NSDateComponentsFormatter
    * Formatter: 用于生成代表时间数量的字符串
* NSDateIntervalFormatter
    * Formatter: 用于生成代表时间戳的字符串
* NSISO8601DateFormatter
    * Formatter: 用于时间 <=> ISO8601字符串代表之间互相转换

    
##数据大小(Data Sizes)
* NSByteCountFormatter
    * Formatter: 用于将那些被格式化成KB, MB, GB的那些描述转化为字节数

##尺寸(Measurements)
* NSMeasurementFormatter
    * Formatter: 提供本地化单位和尺寸的代表

##国际化(Internationalization)
* NSLocale 
    * 装载着有关语言的\文化的\技术的convention的信息, 这些信息用来展示的(=.=马德不会翻译:Information about linguistic, cultural, and technological conventions for use in formatting data for presentation.)

##自定义Formatter(Custom Formatter)
* NSFormatter
    * 一个抽象类, 用于声明一个对象的接口, 

    


