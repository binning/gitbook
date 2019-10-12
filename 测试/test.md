# 单元测试
### 1 karma + mocha + power assert
- karma - 是一款测试流程管理工具，包含在执行测试前进行一些动作，自动在指定的环境（可以是真实浏览器，也可以是 PhantamJS 等 headless browser）下运行测试代码等功能。
- mocha - 测试框架，类似的有 jasmine 和 jest 等。个人感觉 mocha 对异步的支持和反馈信息的显示都非常不错。
- power assert - 断言库，特点是 No API is the best API。错误显示异常清晰，自带完整的自描述性。

```js
1) Array #indexOf() should return index when the value is present:
 AssertionError: # path/to/test/mocha_node.js:10

assert(ary.indexOf(zero) === two)
     |   |       |     |   |
     |   |       |     |   2
     |   -1      0     false
     [1,2,3]

[number] two
=> 2
[number] ary.indexOf(zero)
=> -1
```
#### 1.1安装依赖以及初始化

```js
# 为了操作方便在全局安装命令行支持
~/test-demo $ npm install karma-cli -g

# 安装 karma 包以及其他需要的插件和库，这里不一一阐述每个库的作用
~/test-demo $ npm install karma mocha power-assert karma-chrome-launcher karma-mocha karma-power-assert karma-spec-reporter karma-espower-preprocessor cross-env -D

# 创建测试目录
~/test-demo $ mkdir test

# 初始化 karma
~/test-demo $ karma init ./test/karma.conf.js
```

#### 1.12生成的配置文件略作修改
```js
module.exports = function(config) {
  config.set({
    basePath: '',

    // 表示可以在测试文件中不需引入即可使用两个库的全局方法
    frameworks: ['mocha', 'power-assert'],
    files: [
      '../src/utils.js',
      './specs/utils.spec.js.js'
    ],
    exclude: [
    ],
    preprocessors: {
      './specs/utils.spec.js': ['espower']
    },
    reporters: ['spec'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: false,
    browsers: ['Chrome'],
    singleRun: false,
    concurrency: Infinity
  })
}
```
#### 1.2 待测试代码
我们把源文件放在src目录下
```js
//src/utils.js
function reversrString(string){
  return string.split('').reverse.join('');
}
```
#### 1.3测试代码
测试代码放在test/specs目录下，每个测试文件以 .spec.js 作为后缀
```js
//test/spes/utill.spec.js
describe('first test',function(){
  it('test string reverse => true',function(){
      assert(reverseString('abc') === 'cba');
  })
  it('test string reverse => false', function() {
    assert(reverseString('abc') === 'cba1');
  });
})
```
#### 1.4运行测试
回到项目根目录，运行命令 npm run test 开始执行测试，然后看到浏览器会自动打来执行测试，命令杭输出结果如下：
```js
[karma]: Karma v2.0.0 server started at http://0.0.0.0:9876/
[launcher]: Launching browser Chrome with unlimited concurrency
[launcher]: Starting browser Chrome
[Chrome 63.0.3239 (Mac OS X 10.13.1)]: Connected on socket HEw50fXV-d24BZGBAAAA with id 24095855

  first test
    ✓ test string reverse => true
    ✗ test string reverse => false
    AssertionError:   # utils.spec.js:9

      assert(reverseString('abc') === 'cba1')
             |                    |
             "cba"                false

      --- [string] 'cba1'
      +++ [string] reverseString('abc')
      @@ -1,4 +1,3 @@
       cba
      -1

Chrome 63.0.3239 (Mac OS X 10.13.1): Executed 2 of 2 (1 FAILED) (0.022 secs / 0.014 secs)
TOTAL: 1 FAILED, 1 SUCCESS
```
可以看出一个册书成功和一个测试失败

### 2测试覆盖率
测试覆盖率是衡量测试质量的主要标准之一，含义是当前对于源代码的指令覆盖程度。在karma中使用karma-coverage插件即可输出测试覆盖率，插件底层使用的是istanbul/
```js
~/test-demo $ npm i karma-coverage -D
```
修改karma.config.js文件
```js
preprocessors: {
  '../src/utils.js': ['coverage'],
  './specs/utils.spec.js': ['espower']
},

reporters: ['spec', 'coverage'],
coverageReporter: {
  dir: './coverage', // 覆盖率结果文件放在 test/coverage 文件夹中
  reporters: [
    { type: 'lcov', subdir: '.' },
    { type: 'text-summary' }
  ]
},
```
再次运行测试命令，在最后会输出测试覆盖率信息
```js
=============================== Coverage summary ===============================
Statements   : 100% ( 2/2 )
Branches     : 100% ( 0/0 )
Functions    : 100% ( 1/1 )
Lines        : 100% ( 2/2 )
================================================================================
```
打开 test/coverage/lcov-report/index.html 网页可以看到详细数据

