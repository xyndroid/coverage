1. The configuration of Clover for a Maven project is the same for ALL Maven projects. Following steps allowed to implement Clover with Maven
and generate coverage results.

1.1 Add Clover open souce plugin under &lt;build&gt; &lt;pluginManagement&gt; &lt;plugins&gt; 

&lt;build&gt;
    ...
    &lt;pluginManagement&gt;
        ...
        &lt;plugins&gt; 
            ...
            &lt;plugin&gt; 
                &lt;groupId&gt; org.openclover&lt;/groupId&gt; 
                &lt;artifactId&gt; clover-maven-plugin&lt;/artifactId&gt; 
                &lt;version&gt; 4.2.0&lt;/version&gt; 
            &lt;/plugin&gt; 
            ...
        &lt;/plugins&gt; 
        ...
    &lt;/pluginManagement&gt; 
    &lt;plugins&gt; 
        ...
         &lt;plugin&gt; 
            &lt;groupId&gt; org.openclover&lt;/groupId&gt; 
            &lt;artifactId&gt; clover-maven-plugin&lt;/artifactId&gt; 
            &lt;version&gt; 4.2.0&lt;/version&gt; 
        &lt;/plugin&gt; 
        ...
    &lt;/plugins&gt; 
    ...
&lt;/build&gt; 


1.2 Add Clover open souce profile under &lt;project&gt; &lt;profiles&gt; 

&lt;project&gt; 
    ...
    &lt;profiles&gt; 
        ...
        &lt;profile&gt; 
          &lt;id&gt; clover&lt;/id&gt; 
          &lt;activation&gt; 
            &lt;activeByDefault&gt; true&lt;/activeByDefault&gt; 
            &lt;property&gt; 
              &lt;name&gt; clover&lt;/name&gt; 
            &lt;/property&gt; 
          &lt;/activation&gt; 
          &lt;properties&gt; 
            &lt;cloverDatabase&gt; ${project.build.directory}/clover/coverage.db&lt;/cloverDatabase&gt; 
            cloverAlwaysReport&gt; true&lt;/cloverAlwaysReport&gt; 
            &lt;cloverGenHtml&gt; true&lt;/cloverGenHtml&gt; 
            &lt;cloverGenXml&gt; true&lt;/cloverGenXml&gt; 
            &lt;cloverGenHistorical&gt; true&lt;/cloverGenHistorical&gt; 
            &lt;myHistoryDir&gt; target/clover&lt;/myHistoryDir&gt; 
          &lt;/properties&gt; 
          &lt;build&gt;
            &lt;plugins&gt;
              &lt;plugin&gt;
                &lt;groupId&gt;org.openclover&lt;/groupId&gt;
                &lt;artifactId&gt;clover-maven-plugin&lt;/artifactId&gt;
                &lt;version&gt;4.2.0&lt;/version&gt;
                &lt;configuration&gt;
                  &lt;includesAllSourceRoots&gt;true&lt;/includesAllSourceRoots&gt;
                  &lt;includesTestSourceRoots&gt;true&lt;/includesTestSourceRoots&gt;
                  &lt;cloverDatabase&gt;${cloverDatabase}&lt;/cloverDatabase&gt;
                  &lt;targetPercentage&gt;50%&lt;/targetPercentage&gt;
                  &lt;outputDirectory&gt;target/clover&lt;/outputDirectory&gt;
                  &lt;alwaysReport&gt;${cloverAlwaysReport}&lt;/alwaysReport&gt;
                  &lt;generateHtml&gt;${cloverGenHtml}&lt;/generateHtml&gt;
                  &lt;generateXml&gt;${cloverGenXml}&lt;/generateXml&gt;
                  &lt;generateHistorical&gt;${cloverGenHistorical}&lt;/generateHistorical&gt;
                  &lt;historyDir&gt;${myHistoryDir}&lt;/historyDir&gt;
                &lt;/configuration&gt;
                &lt;executions&gt;
                  &lt;execution&gt;
                    &lt;id&gt;clover-setup&lt;/id&gt;
                    &lt;phase&gt;process-sources&lt;/phase&gt;
                    &lt;goals&gt;
                      &lt;goal&gt;setup&lt;/goal&gt;
                    &lt;/goals&gt;
                  &lt;/execution&gt;
                  &lt;execution&gt;
                    &lt;id&gt;clover&lt;/id&gt;
                    &lt;phase&gt;test&lt;/phase&gt;
                    &lt;goals&gt;
                      &lt;goal&gt;clover&lt;/goal&gt;
                    &lt;/goals&gt;
                  &lt;/execution&gt;
                &lt;/executions&gt;
              &lt;/plugin&gt;
            &lt;/plugins&gt;
          &lt;/build&gt;
        &lt;/profile&gt;
        ...
    &lt;/profiles&gt;
    ...
&lt;/project&gt;

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
