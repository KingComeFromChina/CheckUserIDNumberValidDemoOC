# CheckUserIDNumberValidDemoOC
通过正则和算法判断身份证合法性和正确性。
#[简介](http://www.jianshu.com/p/33ed0d7cb413)
最近项目中用到了判断身份证合法性，本来想着网上正则一大堆，就随便复制粘贴了一个，谁曾想遇到一个身份证号带X的测试，测试说把X换成数字，一定不是正确的身份证号，你这样写不对，很早以前就听说身份证号正则只能判断格式是否正确，而对于身份证号正确性需要用算法计算出来。
###开始普及了啊
#[OCDemo](https://github.com/KingComeFromChina/CheckUserIDNumberValidDemoOC)
#[SwiftDemo](https://github.com/KingComeFromChina/CheckUserIDNumberValidDemoSwift)
看看下面的身份证常识，再看代码的话，你的逻辑一下就明了了。
####身份证常识
我国的身份证号分为15位和18位两种。身份证是国民的身份编号，编号是有一定规律的。
居民身份证号码，根据〖中华人民共和国国家标准 GB 11643-1999〗中有关公民身份号码的规定，公民身份号码是特征组合码，由十七位数字本体码和一位数字校验码组成。排列顺序从左至右依次为：六位数字地址码，八位数字出生日期码，三位数字顺序码和一位数字校验码。 居民身份证是国家法定的证明公民个人身份的有效证件。
####结构和形式
1.号码的结构

公民身份号码是特征组合码，由十七位数字本体码和一位校验码组成。排列顺序从左至右依次为：六位数字地址码，八位数字出生日期码，三位数字顺序码和一位数字校验码。

2．地址码

　  表示编码对象常住户口所在县（市、旗、区）的行政区划代码，按GB/T2260的规定执行。

3．出生日期码

　  表示编码对象出生的年、月、日，按GB/T7408的规定执行，年、月、日代码之间不用分隔符。

4．顺序码

　  表示在同一地址码所标识的区域范围内，对同年、同月、同日出生的人编定的顺序号，顺序码的奇数分配给男性，偶数分配给女性。

5．校验码

　 根据前面十七位数字码，按照ISO7064:1983.MOD11-2校验码计算出来的检验码。

#####地址码
华北地区： 北京市|110000，天津市|120000，河北省|130000，山西省|140000，内蒙古自治区|150000，

东北地区： 辽宁省|210000，吉林省|220000，黑龙江省|230000，

华东地区： 上海市|310000，江苏省|320000，浙江省|330000，安徽省|340000，福建省|350000，江西省|360000，山东省|370000，

华中地区： 河南省|410000，湖北省|420000，湖南省|430000，

华南地区： 广东省|440000，广西壮族自治区|450000，海南省|460000，

西南地区： 重庆市|500000，四川省|510000，贵州省|520000，云南省|530000，西藏自治区|540000，

西北地区： 陕西省|610000，甘肃省|620000，青海省|630000，宁夏回族自治区|640000，新疆维吾尔自治区|650000，

特别地区：台湾地区(886)|710000，香港特别行政区（852)|810000，澳门特别行政区（853)|820000

#####中国大陆居民身份证号码中的地址码的数字编码规则为：
第一、二位表示省（自治区、直辖市、特别行政区）。

第三、四位表示市（地级市、自治州、盟及国家直辖市所属市辖区和县的汇总码）。其中，01-20，51-70表示省直辖市；21-50表示地区（自治州、盟）。

第五、六位表示县（市辖区、县级市、旗）。01-18表示市辖区或地区（自治州、盟）辖县级市；21-80表示县（旗）；81-99表示省直辖县级市。

#####生日期码
身份证号码第七位到第十四位）表示编码对象出生的年、月、日，其中年份用四位数字表示，年、月、日之间不用分隔符。例如：1981年05月11日就用19810511表示。

#####顺序码
身份证号码第十五位到十七位）地址码所标识的区域范围内，对同年、月、日出生的人员编定的顺序号。其中第十七位奇数分给男性，偶数分给女性

#####校验码
作为尾号的校验码，是由号码编制单位按统一的公式计算出来的，如果某人的尾号是0-9，都不会出现X，但如果尾号是10，那么就得用X来代替，因为如果用10做尾号，那么此人的身份证就变成了19位，而19位的号码违反了国家标准，并且中国的计算机应用系统也不承认19位的身份证号码。Ⅹ是罗马数字的10，用X来代替10，可以保证公民的身份证符合国家标准。

###身份证校验码的计算方法
1、将前面的身份证号码17位数分别乘以不同的系数。从第一位到第十七位的系数分别为：7－9－10－5－8－4－2－1－6－3－7－9－10－5－8－4－2。
2、将这17位数字和系数相乘的结果相加。
3、用加出来和除以11，看余数是多少？
4、余数只可能有0－1－2－3－4－5－6－7－8－9－10这11个数字。其分别对应的最后一位身份证的号码为1－0－X－9－8－7－6－5－4－3－2。(即余数0对应1，余数1对应0，余数2对应X...)
5、通过上面得知如果余数是3，就会在身份证的第18位数字上出现的是9。如果对应的数字是2，身份证的最后一位号码就是罗马数字x。

##下面直接粘贴代码
##OC版本的
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
regularExpression = [[NSRegularExpression alloc]initWithPattern:@"^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}[0-9Xx]$" options:NSRegularExpressionCaseInsensitive error:nil];//测试出生日期的合法性
}else {
regularExpression = [[NSRegularExpression alloc]initWithPattern:@"^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}[0-9Xx]$" options:NSRegularExpressionCaseInsensitive error:nil];//测试出生日期的合法性
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
##Swift版本的
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
regularExpression = try! NSRegularExpression.init(pattern: "^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}[0-9Xx]$", options: NSRegularExpression.Options.caseInsensitive)

}else{

//测试出生日期的合法性
regularExpression = try! NSRegularExpression.init(pattern: "^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}[0-9Xx]$", options: NSRegularExpression.Options.caseInsensitive)
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

#总结
如果你感觉有用的话，拿走，点个喜欢就OK
