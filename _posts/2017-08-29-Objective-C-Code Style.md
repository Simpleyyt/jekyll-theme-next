#### 代码组织
```
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```
#### 间距
* 针对 if/else/switch/while 等
```
if (user.isHappy) {
  //Do something
} else {
  //Do something else
}
```
* 文件内各方法之间用空行隔开
* 针对只含单条执行语句的条件句
```
if (condition){
  statement;
}

or

if (condition) statement;

//不好的方式:
if(condition)
	 statement;
```
#### 注释
* 在一些不易理解的代码部分，应适当加以注释说明。同时在.m中也应避免大段的成块注释，而只应写一些简单的说明。

#### 方法
* 在方法签名中，- or + 后面最好接一个空格
```
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```
#### 变量
* *应该紧贴变量名 (有const修饰的情况除外)
```
NSString *text
```
* 尽可能使用@property, 避免使用实例变量
```
//prferred:
@interface RWTTutorial : NSObject
@property (strong, nonatomic) NSString *tutorialName;
@end

//not preferred:
@interface RWTTutorial : NSObject {
  NSString *tutorialName;
}
```
除了init, initWithCoder, dealloc, 及自定义的setter和getter, 应尽量避免直接访问实例变量
* 变量属性的修饰顺序应该先放置内存管理，再放置原子性。这样就可以和interface builder拖拽默认生成的一致。
```
@property (weak, nonatomic) IBOutlet UIView *containerView;
```
* 具有可变对象（列入NSString）最好优先选择copy而不是strong修饰
```
 @property（copy，nonatomic） NSString * tutorialName; 
```
#### 协议 
* 针对 delegate 或 data source协议，第一个参数应该是发送该协议的对象 (处理一对多的情况)
```
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;
```
#### Imports
* 如果imports过多的情况，建议简单的按照用途分在一起
```
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```
#### 错误处理
* 当执行的方法中error是以引用的形式返回时，我们应该先检查返回值，而不是直接对error进行判空操作
```
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}

// Not:
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```
原因是某些Apple的API可能会往error中写入一些垃圾值，这就会使某些正确的情况被判断为错误。
#### 命名
* 常量命名: 驼峰 + 前缀（相关类）
```
static const NSTimeInterval THSArticleViewControllerNavigationFadeAnimationDuration = 0.3;
//not:
static const NSTimeInterval fadetime = 1.7;
```
* 属性: 驼峰命名 + 首字母小写
* 实例变量：驼峰命名 + 下划线前缀
* category:  按照功能命名
```
@interface NSString (NSStringEncodingDetection)
//not:
@interface NSString (THSAdditions)
```
category所添加的属性和方法需要加上 特定的 组织或者APP前缀，这样可以避免复写某些已经存在的方法
```
@interface NSArray (NYTAccessors)
- (id)ths_objectOrNilAtIndex:(NSUInteger)index;
@end
```
#### 字面量
多用字面量语法，少用与之等价的方法
```
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;

//not：
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```
#### CGRect 相关函数
* 当需要获取x, y, width, height等值时，最好使用CGRect的相关函数来获得，而不是直接获取CGRect结构体的值(确保宽，高都为正数等等)
```
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```
http://stackoverflow.com/questions/5970823/cgrectgetwidth-vs-cgrect-size-width
#### 布尔值
* 不要直接与YES比较！
```
if (isAwesome == YES) // Never do this.
```
非0之外的任何一个值都是YES.但以上表达式中的YES只是其中一个值, 这样会造成：
```
BOOL b = 42;
if (b) {
    // true
}
if (b != YES) {
    // also true
}
```
如果属性名称带形容词性，可以在getter前加一个is的前缀
```
@property (assign, getter=isEditable) BOOL editable;
```
#### 条件语句
* 尽量不要嵌套多个if语句。 （可用多个return语句来取代if的嵌套）
```
- (void)someMethod {
    if (![someOther boolValue]) return;
    //Do something important
}

//not: 
- (void)someMethod {
    if ([someOther boolValue]) {;
        //Do something important
    }
}
```
#### 私有变量
* 私有变量应放在.m的实现文件中。不应该在.h中堆砌。
