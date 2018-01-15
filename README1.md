# Spring Cloud 配置服务器
## 搭建Spring Cloud Config Server
### 基于文件系统
#### 创建本地仓库
1. 激活应用配置服务器

  在引导类上标注`@EnableConfigServer`

  ```java
  @SpringBootApplication
  @EnableConfigServer
  public class SpringCloudLesson3ConfigServerApplication {
  	public static void main(String[] args) {
  		SpringApplication.run(SpringCloudLesson3ConfigServerApplication.class, args);
  	}
  }
  ```

  ​

2. `创建`本地目录
  理解 Java 中的`${user.dir}`,在IDE中是指的当前项目物理路径
  在IDEA中src/main/resources目录下，创建一个名为“config”，它的绝对路径：`${user.dir}/src/main/resources/config`

3. 配置git本地仓库URI
   ```properties
   ## 配置服务器文件系统git仓库
   ## ${user.dir} 减少平台文件系统的不一致
   spring.cloud.config.server.git.uri=${user.dir}/src/main/resources/configs
   ```

4. 创建应用三个环境的配置文件

   ```
   segmentfault.properties
   segmentfault-prod.properties
   segmentfault-test.properties
   ```
   三个文件的环境profile分别（从上至下）是：`default`、`prod`、`test`

5. 初始化本地git仓库

   ```
   git init
   git add .
   git commit -m "First commit"

   ## 提示信息如下：
   [master (root-commit) 0de559c] First commit
    3 files changed, 2 insertions(+)
    create mode 100644 segmentfault-prod.properties
    create mode 100644 segmentfault-test.properties
    create mode 100644 segmentfault.properties
   ```

####测试配置服务器

通过浏览器访问：http://127.0.0.1:9090/segmentfault/test

应用为“segmentfault”,profile为：“test”的配置内容是：

```json
{
    "name": "segmentfault",
    "profiles": [
        "test"
    ],
    "label": null,
    "version": "0de559c114ca32b303f68c3d280455a6a0206e5c",
    "state": null,
    "propertySources": [
        {
            "name": "/Users/yulei/IdeaProjects/spring-cloud-lesson-3-config-server/src/main/resources/configs/segmentfault.properties",
            "source": {
                "name": "segmentfault"
            }
        }
    ]
}
```

请注意：当指定了profile时，默认的profile（不指定）配置信息也会输出：

```json
"propertySources": [
        {
            "name": "/Users/yulei/IdeaProjects/spring-cloud-lesson-3-config-server/src/main/resources/configs/segmentfault.properties",
            "source": {
                "name": "segmentfault"
            }
        }
    ]
```

##基于远程git仓库

1. 激活应用配置服务器

   在引导类上标注`@EnableConfigServer`

2. 配置远程Git仓库地址

   ```properties
   ## 配置远程服务器文件系统git仓库
   spring.cloud.config.server.git.uri=https://github.com/yuleizhuai/tmp
   ```

   ​

3. ​
