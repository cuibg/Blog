
## NSError 

`NSError`是`Cocoa`/`Cocoa Touch`框架中向用户传达运行时所产生错误信息的类，这种错误信息通常是以对话框或其它方式告知用户。

`NSError`类主要包含几个属性：`error domain`，和domain相关的`error code`，以及一个包含了和错误相关描述信息的`NSDictionary`类型的`user info`。

#### domain

`domain`提供了分层的概念，通过`domain`我们就可以看出错误产生自哪个模块；同时也避免了因为不同`domain`中定义的`error codes`值相同可能引起的冲突。

由于历史的原因，`OS X`主要`error domains`有以下几种：

* NSMachErrorDomain
* NSPOSIXErrorDomain
* NSOSStatusErrorDomain
* NSCocoaErrorDomain

除了以上四种主要的以外，还有很多`frameworks`甚至以一系列类来划分的`domains`。比如`Web Kit framework`定义的`WebKitErrorDomain`；`Foundation framework`中得URL系列类使用的`NSURLErrorDomain`，以及XML系列类使用的`NSXMLParserErrorDomain`等。

需要注意的是，`NSCocoaErrorDomain`包含了所有`Cocoa framess`中定义的 `error codes`，当然除了各`frameworks`或那些特定类自己定义的以外。

我们也可以为自己的模块定义一个`domain`，格式是：`com.company.framework_or_app.ErrorDomain`。

#### error code

用于定义同一个`domain`下不同的错误类型。

`error code`的使用要结合其所属的`domain`，举例如下：

```objective-c
// underError is underlying-error object of a Cocoa-domain error
if ( [[underError domain] isEqualToString:NSPOSIXErrorDomain] ) {
        switch([underError code]) {
            case EIO:
            {
                // handle POSIX I/O error
            }
            case EACCES:
            {
                // handle POSIX permissions error
            {
        // etc.
        }
}
```

#### user info

`user info`是一个包含了和错误相关的信息的NSDictionary，其特定的key中常用的有以下几个：

* Error description

User info key: `NSLocalizedDescriptionKey`
Method: localizedDescription (never returns nil)

* Failure reason
 
User info key: `NSLocalizedFailureReasonErrorKey`
Method: localizedFailureReason (can return nil)

* Recovery suggestion
 
User info key: `NSLocalizedRecoverySuggestionErrorKey`
Method: localizedRecoverySuggestion (can return nil)

* Recovery options

User info key: `NSLocalizedRecoveryOptionsErrorKey`
Method: localizedRecoveryOptions (if returns nil, implies a single “OK button)

`user info`中包含的不同信息，提供给用户看时应该是这样的：

![error_description](https://github.com/wangzz/Blog/blob/master/image/NSError/error_description.gif?raw=true)

来自官网的创建`NSError`的例子：

```objective-c
- (NSString *)fooFromPath:(NSString *)path error:(NSError **)anError {
 
    const char *fileRep = [path fileSystemRepresentation];
    int fd = open(fileRep, O_RDWR|O_NONBLOCK, 0);
 
    if (fd == -1) {
 
        if (anError != NULL) {
            NSString *description;
            NSDictionary *uDict;
            int errCode;
 
            if (errno == ENOENT) {
                description = NSLocalizedString(@"No file or directory at requested location", @"");
                errCode = MyCustomNoFileError;
            } else if (errno == EIO) {
                // Continue for each possible POSIX error...
            }
 
            // Create the underlying error.
            NSError *underlyingError = [[NSError alloc] initWithDomain:NSPOSIXErrorDomain
                code:errno userInfo:nil];
            // Create and return the custom domain error.
            NSDictionary *errorDictionary = @{ NSLocalizedDescriptionKey : description,
                NSUnderlyingErrorKey : underlyingError, NSFilePathErrorKey : path };
 
            *anError = [[NSError alloc] initWithDomain:MyCustomErrorDomain
                    code:errCode userInfo:errorDictionary];
        }
        return nil;
    }
    // ...
```

#### 参考文档

* [Error Handling Programming Guide](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ErrorHandlingCocoa/ErrorHandling/ErrorHandling.html#//apple_ref/doc/uid/TP40001806-CH201-SW1)


