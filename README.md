#Requirements
If used as git sub-module, the parent repository must have a folder **Configuration** in the root directory. **Configuration** must contain a file `AppConfiguration.rb` with the following information provided:

```ruby
# Update these environment settings for your specific project
def settings
    {
        :project_name => '<PROJECT_NAME>', # ex: TodoList
        :hockey_QA_Token => '<HOCKEY_QA_TOKEN>',
        :hockey_PROD_Token => '<HOCKEY_PROD_TOKEN>',
        :slack_channel => '<SLACK_CHANNEL>', # Optional
        :xcode_project => '<XCODE_PROJECT>', # ex: TodoList.xcodeproj
        :github_repo => '<GITHUB_REPO>' # ex: ChaiOne/swift-style-guide
    }
end
```

fastlane documentation
================
# Installation
```
sudo gem install fastlane
```
# Available Actions
### buildQA
```
fastlane buildQA
```

### Nightly
```
fastlane Nightly
```

### buildProd
```
fastlane buildProd
```


----

This README.md is auto-generated and will be re-generated every time to run [fastlane](https://fastlane.tools).
More information about fastlane can be found on [https://fastlane.tools](https://fastlane.tools).
The documentation of fastlane can be found on [GitHub](https://github.com/fastlane/fastlane/tree/master/fastlane).