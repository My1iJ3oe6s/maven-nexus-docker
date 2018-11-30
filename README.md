### 1.首先下载nexus镜像并把私服跑起来

- 运行build-image.sh下载镜像

    ```js
     ./build-image.sh
    ```

- 将nexus运行起来

    ```js
      //在项目根目录下执行
      docker-compose up -d
    ```

### 2.打开浏览器登录进私服默认密码是 admin 密码是 admin123

- 首先进去创建一个用户，赋予admin的权限
![Image text](https://raw.githubusercontent.com/hongmaju/light7Local/master/img/productShow/20170518152848.png)
   
- 再创建一个自己的仓库
![Image text](https://raw.githubusercontent.com/hongmaju/light7Local/master/img/productShow/20170518152848.png)
   
### 3. 修改本地maven的.m2/settings.xml文件
```xml
  <servers>
    <server>
        <!--记住这个id,后期推送jar包到私服需要用-->
      <id>niezhiliang</id>
      <username>niezhiliang</username>
      <password>niezhiliang</password>
    </server>
  </servers>
```

### 4.推送本地jar包到私服

- 打包成jar上传的项目pom文件，我们拿本项目为例

    ```xml
          <groupId>com.niezhiliang</groupId>
          <artifactId>test-mod</artifactId>
          <!--version后缀必须为RELEASE因为我们在创建项目的时候设置只接受RELEASE-->
          <version>1.0.RELEASE</version>
      
          <!--指定仓库地址-->
          <distributionManagement>
              <repository>
                  <!--此名称要和.m2/settings.xml中设置的ID一致-->
                  <id>niezhiliang</id>
                  <url>http://127.0.0.1:8081/repository/test-rep/</url>
              </repository>
          </distributionManagement>
      
          <build>
              <plugins>
                  <!--发布代码Jar插件-->
                  <plugin>
                      <groupId>org.apache.maven.plugins</groupId>
                      <artifactId>maven-deploy-plugin</artifactId>
                      <version>2.7</version>
                  </plugin>
                  <!--发布源码插件-->
                  <plugin>
                      <groupId>org.apache.maven.plugins</groupId>
                      <artifactId>maven-source-plugin</artifactId>
                      <version>2.2.1</version>
                      <executions>
                          <execution>
                              <phase>package</phase>
                              <goals>
                                  <goal>jar</goal>
                              </goals>
                          </execution>
                      </executions>
                  </plugin>
              </plugins>
          </build>
    ```
    
    
- 到此我们就能直接使用命令推送jar到私服啦

```js
    mvn clean deploy
```

### 5.这个时候我们去私服查询我们的jar，看下是否上传成功啦
![Image text](https://raw.githubusercontent.com/hongmaju/light7Local/master/img/productShow/20170518152848.png)



### 6.在别的项目中pom.xml引用我们私服里面的jar，然后就可以直接用啦

```xml
        <dependencies>
            <dependency>
                <groupId>com.niezhiliang</groupId>
                <artifactId>test-mod</artifactId>
                <version>1.0.RELEASE</version>
            </dependency>
        </dependencies>
    
        <repositories>
            <repository>
                <id>test</id>
                <url>http://127.0.0.1:8081/repository/test-rep/</url>
            </repository>
        </repositories>
```




