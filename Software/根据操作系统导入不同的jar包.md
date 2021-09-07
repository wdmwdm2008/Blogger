
```
根据操作系统导入不同的jar包
简介
有一些基于开源项目封装的jar包需要放入到了项目目录中，同时需要根据操作系统加载不同的jar包。比如，linux系统加载对应的linux jar包，mac系统加载对应的mac jar包。
操作
1. 修改pom.xml
  1. 依赖os-maven-plugin
在<build>标签下增加依赖，可参考官方文档：https://github.com/trustin/os-maven-plugin/
<build>
    <extensions>
        <extension>
            <groupId>kr.motd.maven</groupId>
            <artifactId>os-maven-plugin</artifactId>
            <version>1.7.0</version>
        </extension>
    </extensions>
</build>

2. 拷贝jar包到项目
  1. 路径，必须以linux、osx结尾
    1. Linux: src/main/resource/xxxx/xxx-0.0.1-linux.jar
    2. Mac: src/main/resource/xxxx/xxx-0.0.1-osx.jar
  2. 如果项目有配置.gitignore，需要手动将jar包加入git索引中
    1. git add  ./*.jar
3. jar包引入
<!-- 此处的${os.detected.name}是由os-maven-plugin提供 -->
<dependency>
    <groupId>com.github.levyfan</groupId>
    <artifactId>kenlm-jni</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <!-- 指定Maven scope -->
    <scope>system</scope>
    <!-- 指定jar包路径 -->
    <systemPath>${project.basedir}/src/main/resources/kenlm/kenlm-jni-0.0.1-SNAPSHOT-${os.detected.name}.jar</systemPath>
</dependency>

4. 打包配置修改
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <version>2.2.6.RELEASE</version>
    <configuration>
        <jvmArguments>-Dfile.encoding=UTF-8</jvmArguments>
        <!-- 项目打包时，同时将本地的jar加进去 -->
        <includeSystemScope>true</includeSystemScope>
    </configuration>
    <executions>
        <execution>
            <goals>
                <goal>repackage</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
