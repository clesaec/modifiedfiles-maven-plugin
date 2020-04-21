# modifiedfiles-maven-plugin
Plugin maven to set git local modified files in a maven project property. Use jgit to retrieve the list.

Include the plugin in your project:
```
<plugin>
  <groupId>com.github.yp44</groupId>
  <artifactId>modifiedfiles-maven-plugin</artifactId>
  <version>0.1-SNAPSHOT</version>
  <executions>
      <execution>
          <id>setModifiedFiles</id>
          <phase>validate</phase>
          <goals>
              <goal>set-property</goal>
          </goals>
      </execution>
  </executions>
  <configuration>
   <!-- configuration -->
  </configuration>
</plugin>
```
Here available configuration:
* **propertyName** : name of the maven property that will be set with list of modified files
    * For spotless set it to *spotlessFiles* : https://github.com/diffplug/spotless/blob/7ba6d1a8b1be27455bf08f07130b6a34a9f1d983/plugin-maven/src/main/java/com/diffplug/spotless/maven/AbstractSpotlessMojo.java#L149
    * Default: *modifiedFiles*
* **extensions** : only files with given extension will be selected.
    * Default : java,xml
* **files** : fixed list of modified files.
    * Useful with spotless if you want to configure *spotlessFiles* with a specified list of files or patterns.
    * Default : null
* **forceEmpty** : force the property to be empty.
    * Default : false
* **emptyListValue** : if empty string is not what is expected with *forceEmpty* is used.
    * Useful with spotless since *empty string* in *spotlessFiles* means all files. For Spotless could be set to *THIS_IS_EMPTY_LIST_PATTERN* : https://github.com/diffplug/spotless/blob/45d2122c792927ec4ac7402ee7b10a5d495097d0/plugin-gradle/src/test/java/com/diffplug/gradle/spotless/SpecificFilesTest.java#L129
    * Default : empty string
* **gitStatusElements** : add files from git status given elements (comma separated).
    * Can be any method name *Set<String> getXXX()* from : http://download.eclipse.org/jgit/site/5.7.0.202003110725-r/apidocs/org/eclipse/jgit/api/Status.html
    * Default : modified,uncommittedChanges,untracked

Example to format only modified source code with spotless plugin:
```
<plugin>
  <groupId>com.github.yp44</groupId>
  <artifactId>modifiedfiles-maven-plugin</artifactId>
  <version>0.1-SNAPSHOT</version>
  <executions>
      <execution>
          <id>SetModifiedFiles</id>
          <phase>validate</phase>
          <goals>
              <goal>set-property</goal>
          </goals>
      </execution>
  </executions>
  <configuration>
     <propertyName>spotlessFiles</propertyName>
     <extensions>java,xml</extensions>
  </configuration>
</plugin>
```