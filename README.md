exadel-code-standards
=====================

CheckStyle and PMD configurations for maven

== How to add Checkstyle, PMD and FindBugs into your project ==

```xml
    <pluginRepositories>
        <pluginRepository>
            <id>exadel.release.repo</id>
            <url>https://github.com/AlexKolonitsky/exadel-code-standards/tree/master/releases</url>
        </pluginRepository>
        <pluginRepository>
            <id>exadel.snapshot.repo</id>
            <url>https://github.com/AlexKolonitsky/exadel-code-standards/tree/master/snapshots/</url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </pluginRepository>
    </pluginRepositories>

    ...

    <build>
        <plugins>
            <!-- Configure Checkers -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <version>2.9.1</version>
                <configuration>
                    <consoleOutput>true</consoleOutput>
                    <enableRulesSummary>false</enableRulesSummary>
                    <configLocation>/exadel-checkstyle.xml</configLocation>
                </configuration>
                <executions>
                    <execution>
                        <phase>process-sources</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
                <dependencies>
                    <dependency>
                        <groupId>com.exadel</groupId>
                        <artifactId>exadel-code-standards</artifactId>
                        <version>1.0-SNAPSHOT</version>
                    </dependency>
                </dependencies>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-pmd-plugin</artifactId>
                <version>2.7.1</version>
                <configuration>
                    <sourceEncoding>utf-8</sourceEncoding>
                    <targetJdk>1.6</targetJdk>
                    <verbose>true</verbose>
                    <rulesets>
                        <!-- go back on one dir to parent project -->
                        <ruleset>/exadel-pmd.xml</ruleset>
                    </rulesets>
                </configuration>
                <executions>
                    <execution>
                        <phase>process-sources</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
                <dependencies>
                    <dependency>
                        <groupId>com.exadel</groupId>
                        <artifactId>exadel-code-standards</artifactId>
                        <version>1.0-SNAPSHOT</version>
                    </dependency>
                </dependencies>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>findbugs-maven-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <effort>Max</effort>
                    <!-- do max effort at CI Server -->
                    <threshold>Low</threshold>
                    <!-- make it 'low' at CI Server -->
                </configuration>
                <executions>
                    <execution>
                        <phase>process-classes</phase>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

    ...
```

And you probably find useful profile to skip all checks

```xml

    <profile>
        <id>fast-and-dirty</id>

        <properties>
            <pmd.skip>true</pmd.skip>
            <maven.test.skip>true</maven.test.skip>
            <checkstyle.skip>true</checkstyle.skip>
            <findbugs.skip>true</findbugs.skip>
        </properties>
    </profile>
```


