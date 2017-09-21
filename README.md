1. The configuration of Clover for a Maven project is the same for ALL Maven projects. Following steps allowed to implement Clover with Maven
and generate coverage results. 

1.1 Add Clover open souce plugin under <build><pluginManagement><plugins> 

<build>
    ...
    <pluginManagement>
        ...
        <plugins>
            ...
            <plugin> 
                <groupId>org.openclover</groupId>
                <artifactId>clover-maven-plugin</artifactId>
                <version>4.2.0</version>
            </plugin> 
            ... 
        </plugins>
        ...
    </pluginManagement>
    <plugins>
        ...
         <plugin> 
            <groupId>org.openclover</groupId>
            <artifactId>clover-maven-plugin</artifactId>
            <version>4.2.0</version>
        </plugin> 
        ...
    </plugins>
    ...
</build>


1.2 Add Clover open souce profile under <project><profiles>

<project>
    ...
    <profiles>
        ...
        <profile> 
          <id>clover</id> 
          <activation> 
            <activeByDefault>true</activeByDefault> 
            <property> 
              <name>clover</name> 
            </property> 
          </activation> 
          <properties> 
            <cloverDatabase>${project.build.directory}/clover/coverage.db</cloverDatabase> 
            cloverAlwaysReport>true</cloverAlwaysReport> 
            <cloverGenHtml>true</cloverGenHtml> 
            <cloverGenXml>true</cloverGenXml> 
            <cloverGenHistorical>true</cloverGenHistorical> 
            <myHistoryDir>target/clover</myHistoryDir> 
          </properties> 
          <build> 
            <plugins> 
              <plugin> 
                <groupId>org.openclover</groupId> 
                <artifactId>clover-maven-plugin</artifactId> 
                <version>4.2.0</version> 
                <configuration> 
                  <includesAllSourceRoots>true</includesAllSourceRoots> 
                  <includesTestSourceRoots>true</includesTestSourceRoots> 
                  <cloverDatabase>${cloverDatabase}</cloverDatabase> 
                  <targetPercentage>50%</targetPercentage> 
                  <outputDirectory>target/clover</outputDirectory> 
                  <alwaysReport>${cloverAlwaysReport}</alwaysReport> 
                  <generateHtml>${cloverGenHtml}</generateHtml> 
                  <generateXml>${cloverGenXml}</generateXml> 
                  <generateHistorical>${cloverGenHistorical}</generateHistorical> 
                  <historyDir>${myHistoryDir}</historyDir> 
                </configuration> 
                <executions> 
                  <execution> 
                    <id>clover-setup</id> 
                    <phase>process-sources</phase> 
                    <goals> 
                      <goal>setup</goal> 
                    </goals> 
                  </execution> 
                  <execution> 
                    <id>clover</id> 
                    <phase>test</phase> 
                    <goals> 
                      <goal>clover</goal> 
                    </goals> 
                  </execution> 
                </executions> 
              </plugin> 
            </plugins> 
          </build> 
        </profile>        
        ... 
    </profiles>
    ...
</project>

2. Once the setup is done. run following commands

mvn clover:setup test clover:save-history clover:aggregate clover:clover

3. When coverage testing is done running, run following commands to get paths to report directories. 

find . -name index.html | grep target\/clover\/index\.html | sed 's/.//' | sed 's/index.html//' > path.to.index.html

4. Run this command to extract name of the modules

find . -name index.html | grep target\/clover\/index\.html | sed 's/.//' | sed 's/index.html//' | awk -F/ '{print $(NF-3)}' > module.names

5. Create directory called 'report'

6. Open up the 'parser.rb' and change the $project_name variable to the project name you're running

7. Finaly, run 'report_sh' from tools directory and wait for it to finish. 

8. Once the step 7 is done, result files shall be generated and stored in 'report' folder you created in step 5. 
