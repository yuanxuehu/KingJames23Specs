# KingJames23Specs
KingJames23Specs私有索引库

iOS组件化实践之一: Cocoapods创建私有库
1. 创建远程私有索引库Specs
登录github, 创建私有索引库

2. 将远程索引库关联到本地cocopods的repos文件夹下
cd /Users/yuanxuehu/.cocoapods/repos;
pod repo add xxxSpecs https://github.com/yuanxuehu/KingJames23Specs.git
/Users/yuanxuehu/.cocoapods/repos目录下面多了个KingJames23Specs目录

3. 创建组件化基础库管理文件夹
cd到指定的目录，这个ComponentPrivatePods是自己创建的一个文件目录, 里面放置各个组件化基础库,
本人选择cd /Users/yuanxuehu/目录下创建

4. 创建一个基础组件库
cd 到ComponentPrivatePods基础库文件夹, 比如常见的XXKit组件库

cd /Users/yuanxuehu/ComponentPrivatePods
pod lib create KJKit
根据选项自行选择,创建完毕后会自动使用Xcode打开刚创建的组件库工程文件

5. 创建对应组件类文件,并拖入组件库
存放到组件库KJKit>Classes文件夹下, 并删除原有文件

6. Example工程测试创建的组件库
cd 到Example文件
Podfile文件, 可以看到本地引用路径pod 'KJKit', :path => '../'

7. github创建对应的远程组件库ZYKit
https://github.com/yuanxuehu/KJKit.git

8. 配置本地ZYKit组件库的podspec文件

Pod::Spec.new do |s|
  s.name             = 'KJKit'
  s.version          = '3.0.0'
  s.summary          = 'A short description of KJKit.'
  s.description      = <<-DESC
TODO: Add long description of the pod here.
                       DESC

  s.homepage         = 'https://gitee.com/yuanxuehu2016/KJKit'
  # s.screenshots     = 'www.example.com/screenshots_1', 'www.example.com/screenshots_2'
  s.license          = { :type => 'MIT', :file => 'LICENSE' }
  s.author           = { 'yuanxuehu' => 'yuanxuehu2016@126.com' }
  s.source           = { :git => 'https://gitee.com/yuanxuehu2016/KJKit.git', :tag => s.version.to_s }
  # s.social_media_url = 'https://twitter.com/<TWITTER_USERNAME>'

  s.ios.deployment_target = '9.0'

  s.source_files = 'KJKit/Classes/**/*'
  
  # s.resource_bundles = {
  #   'KJKit' => ['KJKit/Assets/*.png']
  # }

  # s.public_header_files = 'Pod/Classes/**/*.h'
  # s.frameworks = 'UIKit', 'MapKit'
  # s.dependency 'AFNetworking', '~> 2.3'
end

9. 将本地组件库KJKit提交到远程github的KJKit中
cd /Users/yuanxuehu/ComponentPrivatePods/KJKit
git init  # git初始化
git remote add origin https://github.com/yuanxuehu/KJKit.git # 远程关联
git pull origin master:master  # 从远程分支拉取master分支并与本地master分支合并
git push -u origin master -f   # 提交本地分支到远程分支
git add .  
git commit -m '增加组件库KJKit'
git push --set-upstream origin master

10. 提交增大的tag提及到到远端KJKit,以便进行验证
将tag增大,保存podspec文档
每次修改提交到github对应的组件库都需要增加tag
git tag '0.1.1'
git push --tag

11. 对文件进行本地验证和远程验证
11.1 从本地验证你的pod能否通过验证
pod lib lint --use-libraries --allow-warnings
--verbose：有些非语法错误是不会给出错误原因的，这个时候可以使用--verbose来查看详细的验证过程来帮助定位错误。
--use-libraries：表示使用静态库或者是framework，这里主要是解决当我们依赖一些framework库后校验提示找不到库的时候用到。
--allow-warnings：表示允许警告。

11.2 从本地和远程验证的pod能否通过验证
pod spec lint --use-libraries --allow-warnings

12. 将spec文件提交到本地的私有索引仓库，然后再push到远程索引仓库
pod repo push KingJames23Specs KJKit.podspec --use-libraries --allow-warnings

注意：
CocoaPods公有库和私有库区别：（.podspec文件保存位置不同）
推送上不同：
公有库：本地库制作或者更新完后， 将podspec文件提交至CocoaPods源仓库 （所有人都可以安装）
私有库：本地库制作或者更新完后， 将podspec文件提交至私有源仓库 （私有仓库授权后可以安装）

公有库发布版本方法
pod trunk push [name].podspec
私有库发布版本方法
pod repo push [repoName] [name].podspec

使用上不同：
私有库：面向内部开发人员；
公有库：面向所有开发人员；

13. 查看是否操作成功
本地KingJames23Specs索引库增加成功

14. 创建demo引入组件化库
在工程Podfile中引入组件地址
执行pod install


