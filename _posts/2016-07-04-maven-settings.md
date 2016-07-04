### Maven 配置常用:

#### 设置编码:

	 <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <scala.version>2.10.6</scala.version>
    </properties>

#### 设置main函数方便测试:

	<plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.1.1</version>
      <executions>
          <execution>
              <phase>test</phase>
              <goals>
                  <goal>java</goal>
              </goals>
              <configuration>
                  <mainClass>test.IP2Geo</mainClass>
                  <arguments>
                      <argument>arg0</argument>
                      <argument>arg1</argument>
                  </arguments>
              </configuration>
          </execution>
      </executions>
  </plugin>

#### 指定source目录:

	<sourceDirectory>${base_dir}/src/main/java</sourceDirectory>
	
或者使用IDE工具指定source目录。 Mac OSX 下 IntelliJ IDEA.
File -> Project Structure -> Modules -> 选中你的module -> 指定source

#### package带上依赖:

	<plugin>
      <!--复制需要的依赖到/jars/目录-->
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-dependency-plugin</artifactId>
      <version>2.9</version>
      <executions>
          <execution>
              <id>copy</id>
              <phase>package</phase>
              <goals>
                  <goal>copy</goal>
              </goals>
              <configuration>
                  <artifactItems>

                      <artifactItem>
                          <groupId>com.zamplus</groupId>
                          <artifactId>xray-common</artifactId>
                          <type>jar</type>
                          <overWrite>yes</overWrite>
                      </artifactItem>

                      <artifactItem>
                          <groupId>com.zamplus</groupId>
                          <artifactId>xray-meta</artifactId>
                          <type>jar</type>
                          <overWrite>yes</overWrite>
                      </artifactItem>

                      <artifactItem>
                          <groupId>com.zamplus</groupId>
                          <artifactId>zampdata</artifactId>
                          <type>jar</type>
                          <overWrite>yes</overWrite>
                      </artifactItem>
                      <artifactItem>
                          <groupId>com.zamplus</groupId>
                          <artifactId>zampdata-records</artifactId>
                          <type>jar</type>
                          <overWrite>yes</overWrite>
                      </artifactItem>

                  </artifactItems>
                  <outputDirectory>${basedir}/../../jobs/jars/</outputDirectory>
                  <overWriteReleases>false</overWriteReleases>
                  <overWriteSnapshots>true</overWriteSnapshots>
              </configuration>
          </execution>
      </executions>
  </plugin>

可以看到主要是使用如下工具: 

		<groupId>org.apache.maven.plugins</groupId>
	  	<artifactId>maven-dependency-plugin</artifactId>
	  	<version>2.9</version>

