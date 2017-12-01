
![](http://upload-images.jianshu.io/upload_images/3873966-5236b2de272ce686.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### A simple Id card validation rules written in pure swift, lightweight but powerful.

###### [🇨🇳中文介绍](http://www.jianshu.com/p/33ed0d7cb413)
>Recent projects used to judge the legality of the id card original thinking of regular online a lot of, just copy and paste the one, who wanted to meet a id number with X-ray test, the test says change X to Numbers, is not the correct id number, you write wrong, id long ago heard that regular can only judge the format is correct, and calculated to id for the correctness of algorithm.
## Requirements

- iOS 8.0+ 
- Xcode 8
- Swift 3.0
## GitHub
### [OCDemo](https://github.com/KingComeFromChina/CheckUserIDNumberValidDemoOC)
### [SwiftDemo](https://github.com/KingComeFromChina/CheckUserIDNumberValidDemoSwift)
Look at the following id common sense, then look at the code, your logic is clear.
#### Identification of common sense
Our country's id number is divided into 15 and 18.Id is a national identity number, the number is not random.
Resident identity card number, according to the showing of the People's Republic of China national standard GB 11643-11643] in the regulations on citizen id Numbers, citizenship combined code number is characteristics, by 17 digit code of ontology and a number of check code.The order from left to right in turn for: six figures address code, eight digit birth date code, the three digital sequence code and a digital check code.Resident identity card is the national legal proof of valid certificates of the identity of the individual citizens.

#### Structure and form

1. The structure of the number

>Citizen identity number is characteristics of combination code, the seventeen of digital ontology yards and a check code.The order from left to right in turn for: six figures address code, eight digit birth date code, the three digital sequence code and a digital check code.

2．Address code

>Encoding the object the permanent residence county (city, flag, area) of administrative division code, shall be governed by the provisions of the GB/T2260.
　
  
3．Birth date code
　  
　  
>said coding object birth year, month, day, according to the provisions of the GB/T7408, year, month, day without separators between code.

　  
4．The order code
　  
> said at the same address code identified areas within the scope of the people born in the same year, in the same month, on the same day of serial number, sequence code odd number assigned to the male, the even assigned to women.　  

5．Check code
　  
> according to the previous 17 digit word, according to ISO7064:1983 MOD11-2 check code calculated check code.

6. Address code


  ```
  In north China： The Beijing municipal|110000，tianjin|120000，In hebei province|130000，Shanxi Province|140000，The Inner Mongolia autonomous region|150000，

  In the northeast： Liaoning province|210000，Jilin province|220000，Heilongjiang province|230000，

  East China area： Shanghai|310000，Jiangsu province|320000，Zhejiang province|330000，In anhui province|340000，Fujian province|350000，Jiangxi province|360000，In shandong province|370000，

  In central China： Henan province|410000，Hubei province|420000，Hunan province|430000

  In south China： Guangdong province|440000，The guangxi zhuang autonomous region|450000，Hainan province|460000，

  In the southwest： chongqing|500000，Sichuan province|510000，The guizhou province|520000，In yunnan province|530000，The Tibet autonomous region|540000，

  The northwest region： Shanxi province|610000，Gansu province|620000，Qinghai province|630000，The ningxia hui autonomous region|640000，The xinjiang uygur autonomous region|650000，
 
  Special area：The Taiwan region(886)|710000，The Hong Kong special administrative region（852)|810000，The Macao special administrative region（853)|820000
   ```
#### The Chinese mainland residents, digital coding rules for id number in the address：
　
 ```
 
Said the first, two provinces (autonomous regions and municipalities directly under the central government, special administrative region).　 

The third and fourth said city (prefecture, autonomous prefectures and African union (au) and the national municipality directly under the central government's municipal districts and counties of summary code).Among them, 01-20: pp.49-60 70 provinces municipalities directly under the central government;21 to 50 said region (autonomous prefectures and au).

Fifth, said the county (municipal, county-level city, flag).01-18 municipal district or region (autonomous prefectures and au) governs county;21-80 counties (banners);81-99 indicates directly under province county.

 ```


7. Birthday period code
> id number 7 to 14 encoding the object birth year, month, day, one year with four digits, said no separators between year, month, day.For example, May 11, 1981 in 19810511.　  　 
　  　 

8. The order code
> id number to fifteenth to address identified areas within the scope of seventeen, born of the same year, month, the staff of serial number.17 odd among men, even among women　  　 
　  　 

9. Check code
　
> as a private check code, it is calculated by number unit according to the unified formula, if someone's tail number is 0-9, X will not appear, but if the tail is 10, then to use X to replace, because if made tail number 10, then this person id card becomes a 19, and violated the national standard, the number 19 and China's computer application system does not recognize identification number 19.Ⅹ is Roman numeral 10, with 10 X instead of, can guarantee citizen id card is in line with national standards.　  　 


10. The calculation method of id check code

```
1、 will be in front of the id card number 17 digit number multiplied by the coefficient of different respectively.From the first to 17 coefficient respectively: 7-9-10-5-8-4-5-8-4-3-7-9-7-9-10-4-2.

2、 the coefficient of 17 Numbers and the result of the multiplication addition.

3、 in and out, and divided by 11, the remainder is how much?

4、 the remainder may have only 0-1-2-3-1-2-3 - the 7-8-9-10 this 11 Numbers.Its respectively corresponding to the last id number is 1-0 - X - 9-8-7-9-8-7-3-2.(that is, the remainder 0 corresponds to 1, the remainder corresponding to 0, 1 remainder 2 corresponding X...)

5、Through that if the remainder is above 3, in the id card on the 18th of digital is 9.If the corresponding number is 2, the last of the id card number is the Roman numerals x.

```

## Directly paste the code below
## OC version 
　  　
　 
```
 -(BOOL)validateIDCardNumber:(NSString *)value {
　  　 
　  　 value = [value stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
　  　 NSInteger length =0;
　  　 if (!value) {
　  　 return NO;
　  　 }else {
　  　 length = value.length;
　  　 //不满足15位和18位，即身份证错误
　  　 if (length !=15 && length !=18) {
　  　 return NO;
　  　 }
　  　 }
　  　 // 省份代码
　  　 NSArray *areasArray = @[@"11",@"12", @"13",@"14", @"15",@"21", @"22",@"23", @"31",@"32", @"33",@"34", @"35",@"36", @"37",@"41", @"42",@"43", @"44",@"45", @"46",@"50", @"51",@"52", @"53",@"54", @"61",@"62", @"63",@"64", @"65",@"71", @"81",@"82", @"91"];
　  　 
　  　 // 检测省份身份行政区代码
　  　 NSString *valueStart2 = [value substringToIndex:2];
　  　 BOOL areaFlag =NO; //标识省份代码是否正确
　  　 for (NSString *areaCode in areasArray) {
　  　 if ([areaCode isEqualToString:valueStart2]) {
　  　 areaFlag =YES;
　  　 break;
　  　 }
　  　 }
　  　 
　  　 if (!areaFlag) {
　  　 return NO;
　  　 }
　  　 
　  　 NSRegularExpression *regularExpression;
　  　 NSUInteger numberofMatch;
　  　 
　  　 int year =0;
　  　 //分为15位、18位身份证进行校验
　  　 switch (length) {
　  　 case 15:
　  　 //获取年份对应的数字
　  　 year = [value substringWithRange:NSMakeRange(6,2)].intValue +1900;
　  　 
　  　 if (year %4 ==0 || (year %100 ==0 && year %4 ==0)) {
　  　 //创建正则表达式 NSRegularExpressionCaseInsensitive：不区分字母大小写的模式
　  　 regularExpression = [[NSRegularExpression alloc]initWithPattern:@"^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}$"
　  　 options:NSRegularExpressionCaseInsensitive error:nil];//测试出生日期的合法性
　  　 }else {
　  　 regularExpression = [[NSRegularExpression alloc]initWithPattern:@"^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}$"
　  　 options:NSRegularExpressionCaseInsensitive error:nil];//测试出生日期的合法性
　  　 }
　  　 //使用正则表达式匹配字符串 NSMatchingReportProgress:找到最长的匹配字符串后调用block回调
　  　 numberofMatch = [regularExpression numberOfMatchesInString:value
　  　 options:NSMatchingReportProgress
　  　 range:NSMakeRange(0, value.length)];
　  　 
　  　 if(numberofMatch >0) {
　  　 return YES;
　  　 }else {
　  　 return NO;
　  　 }
　  　 case 18:
　  　 year = [value substringWithRange:NSMakeRange(6,4)].intValue;
　  　 if (year %4 ==0 || (year %100 ==0 && year %4 ==0)) {
　  　 regularExpression = [[NSRegularExpression alloc]initWithPattern:@"^((1[1-5])|(2[1-3])|(3[1-7])|(4[1-6])|(5[0-4])|(6[1-5])|71|(8[12])|91)\\d{4}(((19|20)\\d{2}(0[13-9]|1[012])(0[1-9]|[12]\\d|30))|((19|20)\\d{2}(0[13578]|1[02])31)|((19|20)\\d{2}02(0[1-9]|1\\d|2[0-8]))|((19|20)([13579][26]|[2468][048]|0[048])0229))\\d{3}(\\d|X|x)?$" options:NSRegularExpressionCaseInsensitive error:nil];//测试出生日期的合法性
　  　 }else {
　  　 regularExpression = [[NSRegularExpression alloc]initWithPattern:@"^((1[1-5])|(2[1-3])|(3[1-7])|(4[1-6])|(5[0-4])|(6[1-5])|71|(8[12])|91)\\d{4}(((19|20)\\d{2}(0[13-9]|1[012])(0[1-9]|[12]\\d|30))|((19|20)\\d{2}(0[13578]|1[02])31)|((19|20)\\d{2}02(0[1-9]|1\\d|2[0-8]))|((19|20)([13579][26]|[2468][048]|0[048])0229))\\d{3}(\\d|X|x)?$" options:NSRegularExpressionCaseInsensitive error:nil];//测试出生日期的合法性
　  　 }
　  　 numberofMatch = [regularExpression numberOfMatchesInString:value
　  　 options:NSMatchingReportProgress
　  　 range:NSMakeRange(0, value.length)];
　  　 
　  　 
　  　 if(numberofMatch >0) {
　  　 //1：校验码的计算方法 身份证号码17位数分别乘以不同的系数。从第一位到第十七位的系数分别为：7－9－10－5－8－4－2－1－6－3－7－9－10－5－8－4－2。将这17位数字和系数相乘的结果相加。
　  　 
　  　 int S = [value substringWithRange:NSMakeRange(0,1)].intValue*7 + [value substringWithRange:NSMakeRange(10,1)].intValue *7 + [value substringWithRange:NSMakeRange(1,1)].intValue*9 + [value substringWithRange:NSMakeRange(11,1)].intValue *9 + [value substringWithRange:NSMakeRange(2,1)].intValue*10 + [value substringWithRange:NSMakeRange(12,1)].intValue *10 + [value substringWithRange:NSMakeRange(3,1)].intValue*5 + [value substringWithRange:NSMakeRange(13,1)].intValue *5 + [value substringWithRange:NSMakeRange(4,1)].intValue*8 + [value substringWithRange:NSMakeRange(14,1)].intValue *8 + [value substringWithRange:NSMakeRange(5,1)].intValue*4 + [value substringWithRange:NSMakeRange(15,1)].intValue *4 + [value substringWithRange:NSMakeRange(6,1)].intValue*2 + [value substringWithRange:NSMakeRange(16,1)].intValue *2 + [value substringWithRange:NSMakeRange(7,1)].intValue *1 + [value substringWithRange:NSMakeRange(8,1)].intValue *6 + [value substringWithRange:NSMakeRange(9,1)].intValue *3;
　  　 
　  　 //2：用加出来和除以11，看余数是多少？余数只可能有0－1－2－3－4－5－6－7－8－9－10这11个数字
　  　 int Y = S %11;
　  　 NSString *M =@"F";
　  　 NSString *JYM =@"10X98765432";
　  　 M = [JYM substringWithRange:NSMakeRange(Y,1)];// 3：获取校验位
　  　 
　  　 NSString *lastStr = [value substringWithRange:NSMakeRange(17,1)];
　  　 
　  　 NSLog(@"%@",M);
　  　 NSLog(@"%@",[value substringWithRange:NSMakeRange(17,1)]);
　  　 //4：检测ID的校验位
　  　 if ([lastStr isEqualToString:@"x"]) {
　  　 if ([M isEqualToString:@"X"]) {
　  　 return YES;
　  　 }else{
　  　 
　  　 return NO;
　  　 }
　  　 }else{
　  　 
　  　 if ([M isEqualToString:[value substringWithRange:NSMakeRange(17,1)]]) {
　  　 return YES;
　  　 }else {
　  　 return NO;
　  　 }
　  　 
　  　 }
　  　 
　  　 
　  　 }else {
　  　 return NO;
　  　 }
　  　 default:
　  　 return NO;
　  　 }
　  　 }
```
　  　 
## Swift version 

　  　
　  　 
　  　
```
func isTrueIDNumber(text:String) -> Bool{
　  　 
　  　 var value = text
　  　 
　  　 value = value.trimmingCharacters(in: CharacterSet.whitespacesAndNewlines)
　  　 
　  　 var length : Int = 0
　  　 
　  　 length = value.characters.count
　  　 
　  　 if length != 15 && length != 18{
　  　 //不满足15位和18位，即身份证错误
　  　 return false
　  　 }
　  　 
　  　 // 省份代码
　  　 let areasArray = ["11","12", "13","14", "15","21", "22","23", "31","32", "33","34", "35","36", "37","41", "42","43", "44","45", "46","50", "51","52", "53","54", "61","62", "63","64", "65","71", "81","82", "91"]
　  　 
　  　 // 检测省份身份行政区代码
　  　 let index = value.index(value.startIndex, offsetBy: 2)
　  　 let valueStart2 = value.substring(to: index)
　  　 
　  　 //标识省份代码是否正确
　  　 var areaFlag = false
　  　 
　  　 for areaCode in areasArray {
　  　 
　  　 if areaCode == valueStart2 {
　  　 areaFlag = true
　  　 break
　  　 }
　  　 }
　  　 
　  　 if !areaFlag {
　  　 return false
　  　 }
　  　 
　  　 var regularExpression : NSRegularExpression?
　  　 
　  　 var numberofMatch : Int?
　  　 
　  　 var year = 0
　  　 
　  　 switch length {
　  　 case 15:
　  　 
　  　 //获取年份对应的数字
　  　 let valueNSStr = value as NSString
　  　 
　  　 let yearStr = valueNSStr.substring(with: NSRange.init(location: 6, length: 2)) as NSString
　  　 
　  　 year = yearStr.integerValue + 1900
　  　 
　  　 if year % 4 == 0 || (year % 100 == 0 && year % 4 == 0) {
　  　 //创建正则表达式 NSRegularExpressionCaseInsensitive：不区分字母大小写的模式
　  　 //测试出生日期的合法性
　  　 regularExpression = try! NSRegularExpression.init(pattern: "^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}$", options: NSRegularExpression.Options.caseInsensitive)
　  　 }else{
　  　 
　  　 //测试出生日期的合法性
　  　 regularExpression = try! NSRegularExpression.init(pattern: "^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}$", options: NSRegularExpression.Options.caseInsensitive)
　  　 }
　  　 
　  　 numberofMatch = regularExpression?.numberOfMatches(in: value, options: NSRegularExpression.MatchingOptions.reportProgress, range: NSRange.init(location: 0, length: value.characters.count))
　  　 
　  　 if numberofMatch! > 0 {
　  　 return true
　  　 }else{
　  　 
　  　 return false
　  　 }
　  　 
　  　 case 18:
　  　 
　  　 let valueNSStr = value as NSString
　  　 
　  　 let yearStr = valueNSStr.substring(with: NSRange.init(location: 6, length: 4)) as NSString
　  　 
　  　 year = yearStr.integerValue
　  　 
　  　 if year % 4 == 0 || (year % 100 == 0 && year % 4 == 0) {
　  　 
　  　 //测试出生日期的合法性
　  　 regularExpression = try! NSRegularExpression.init(pattern: "^((1[1-5])|(2[1-3])|(3[1-7])|(4[1-6])|(5[0-4])|(6[1-5])|71|(8[12])|91)\\d{4}(((19|20)\\d{2}(0[13-9]|1[012])(0[1-9]|[12]\\d|30))|((19|20)\\d{2}(0[13578]|1[02])31)|((19|20)\\d{2}02(0[1-9]|1\\d|2[0-8]))|((19|20)([13579][26]|[2468][048]|0[048])0229))\\d{3}(\\d|X|x)?$", options: NSRegularExpression.Options.caseInsensitive)
　  　 
　  　 }else{
　  　 
　  　 //测试出生日期的合法性
　  　 regularExpression = try! NSRegularExpression.init(pattern: "^((1[1-5])|(2[1-3])|(3[1-7])|(4[1-6])|(5[0-4])|(6[1-5])|71|(8[12])|91)\\d{4}(((19|20)\\d{2}(0[13-9]|1[012])(0[1-9]|[12]\\d|30))|((19|20)\\d{2}(0[13578]|1[02])31)|((19|20)\\d{2}02(0[1-9]|1\\d|2[0-8]))|((19|20)([13579][26]|[2468][048]|0[048])0229))\\d{3}(\\d|X|x)?$", options: NSRegularExpression.Options.caseInsensitive)
　  　 }
　  　 
　  　 numberofMatch = regularExpression?.numberOfMatches(in: value, options: NSRegularExpression.MatchingOptions.reportProgress, range: NSRange.init(location: 0, length: value.characters.count))
　  　 
　  　 if numberofMatch! > 0 {
　  　 
　  　 let a = getStringByRangeIntValue(Str: valueNSStr, location: 0, length: 1) * 7
　  　 
　  　 let b = getStringByRangeIntValue(Str: valueNSStr, location: 10, length: 1) * 7
　  　 
　  　 let c = getStringByRangeIntValue(Str: valueNSStr, location: 1, length: 1) * 9
　  　 
　  　 let d = getStringByRangeIntValue(Str: valueNSStr, location: 11, length: 1) * 9
　  　 
　  　 let e = getStringByRangeIntValue(Str: valueNSStr, location: 2, length: 1) * 10
　  　 
　  　 let f = getStringByRangeIntValue(Str: valueNSStr, location: 12, length: 1) * 10
　  　 
　  　 let g = getStringByRangeIntValue(Str: valueNSStr, location: 3, length: 1) * 5
　  　 
　  　 let h = getStringByRangeIntValue(Str: valueNSStr, location: 13, length: 1) * 5
　  　 
　  　 let i = getStringByRangeIntValue(Str: valueNSStr, location: 4, length: 1) * 8
　  　 
　  　 let j = getStringByRangeIntValue(Str: valueNSStr, location: 14, length: 1) * 8
　  　 
　  　 let k = getStringByRangeIntValue(Str: valueNSStr, location: 5, length: 1) * 4
　  　 
　  　 let l = getStringByRangeIntValue(Str: valueNSStr, location: 15, length: 1) * 4
　  　 
　  　 let m = getStringByRangeIntValue(Str: valueNSStr, location: 6, length: 1) * 2
　  　 
　  　 let n = getStringByRangeIntValue(Str: valueNSStr, location: 16, length: 1) * 2
　  　 
　  　 let o = getStringByRangeIntValue(Str: valueNSStr, location: 7, length: 1) * 1
　  　 
　  　 let p = getStringByRangeIntValue(Str: valueNSStr, location: 8, length: 1) * 6
　  　 
　  　 let q = getStringByRangeIntValue(Str: valueNSStr, location: 9, length: 1) * 3
　  　 
　  　 let S = a + b + c + d + e + f + g + h + i + j + k + l + m + n + o + p + q
　  　 
　  　 let Y = S % 11
　  　 
　  　 var M = "F"
　  　 
　  　 let JYM = "10X98765432"
　  　 
　  　 M = (JYM as NSString).substring(with: NSRange.init(location: Y, length: 1))
　  　 
　  　 let lastStr = valueNSStr.substring(with: NSRange.init(location: 17, length: 1))
　  　 
　  　 if lastStr == "x" {
　  　 if M == "X" {
　  　 return true
　  　 }else{
　  　 
　  　 return false
　  　 }
　  　 }else{
　  　 
　  　 if M == lastStr {
　  　 return true
　  　 }else{
　  　 
　  　 return false
　  　 }
　  　 }
　  　 }else{
　  　 
　  　 return false
　  　 }
　  　 
　  　 default:
　  　 return false
　  　 }
　  　 }
　  　 
　  　 func getStringByRangeIntValue(Str : NSString,location : Int, length : Int) -> Int{
　  　 
　  　 let a = Str.substring(with: NSRange(location: location, length: length))
　  　 
　  　 let intValue = (a as NSString).integerValue
　  　 
　  　 return intValue
　  　 }
```

# Recommend the article
## [iPhone X 适配（Swift篇）](http://www.jianshu.com/p/c01da05c5e40)
## [iOS身份证判断正则加算法](http://www.jianshu.com/p/33ed0d7cb413)
## [RN环境搭建及与原生交互](http://www.jianshu.com/p/1537bb431d12)
## [RxSwift使用手册](http://www.jianshu.com/p/d06b87e368fd)
## [RxSwift日常项目使用（持续更新。。。）](http://www.jianshu.com/p/63a03788f4cf)
## [比较RAC和RxSwift](http://www.jianshu.com/p/c38f027f55e9)
# The author
## [伪文艺的程序员](http://www.jianshu.com/u/6ae311ad394d)

# Conclusion

>If you feel useful, and some like is OK

