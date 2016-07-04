### Maven 多模块项目配置


平时的项目中，由于子模块偏多， 如果全部使用一个POM文件来管理，可能会比较复杂。这时就可以使用maven的多模块管理的功能。

#### 配置示例

	<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.sonatype.mavenbook.multi</groupId>
    <artifactId>simple-parent</artifactId>
    <packaging>pom</packaging>
    <version>1.0</version>
    <name>Multi Chapter Simple Parent Project</name>

    <modules>
        <module>simple-weather</module>
        <module>simple-webapp</module>
    </modules>

    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <configuration>
                        <source>1.5</source>
                        <target>1.5</target>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>


此处最关键的配置在:

	<modules>
        <module>simple-weather</module>
        <module>simple-webapp</module>
    </modules>


此时父groupID为: **org.sonatype.mavenbook.multi**, 父artifactId: **simple-parent**. 需要注意的是,父POM文件的配置， 都会被子模块继承。

再观察子模块配置:

	<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.sonatype.mavenbook.multi</groupId>
        <artifactId>simple-parent</artifactId>
        <version>1.0</version>
    </parent>
    <artifactId>simple-weather</artifactId>
    <packaging>jar</packaging>

    <name>Multi Chapter Simple Weather API</name>

    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <configuration>
                        <testFailureIgnore>true</testFailureIgnore>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>

    <dependencies>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.14</version>
        </dependency>
        <dependency>
            <groupId>dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>1.6.1</version>
        </dependency>
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.1.1</version>
        </dependency>
        <dependency>
            <groupId>velocity</groupId>
            <artifactId>velocity</artifactId>
            <version>1.5</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-io</artifactId>
            <version>1.3.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
		
此处必须声明父依赖:

	<parent>
		<groupId>org.sonatype.mavenbook.multi</groupId>
		<artifactId>simple-parent</artifactId>
		<version>1.0</version>
	</parent>


另外配置: 

	<pluginManagement>
       <plugins>
           <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-surefire-plugin</artifactId>
               <configuration>
                   <testFailureIgnore>true</testFailureIgnore>
               </configuration>
           </plugin>
       </plugins>
   </pluginManagement>
        
 作用以及与plugins的区别:
 
 maven会在当前项目中加载plugins声明的插件；

pluginManagement是表示插件声明，即你在项目中的pluginManagement下声明了插件，maven不会加载该插件，pluginManagement声明可以被继承。

pluginManagement的一个使用案例是当有父子项目的时候，父项目中可以利用pluginManagement声明子项目中需要用到的插件，之后，当某个或者某几个子项目需要加载该插件的时候，就可以在子项目中plugins节点只配置 groupId 和 artifactId就可以完成插件的引用。
pluginManagement主要是为了统一管理插件，确保所有子项目使用的插件版本保持一致，类似的还是dependencies和dependencyManagement。





