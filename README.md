maven-release
=============

A skeletal multi module Spring MVC project for setting up automated deployment to Amazon EC2 Servers. This guide assumes you have
the following correctly configured:
<ul>
<li> Maven 3 </li> 
<li>Git and the project correctly cloned from git and with remotes configured.</li>
<li>Amazon EC2 Instance with your private key</li>
<li>Tomcat 7 with a user with role <b>"manager-script"</b> on both localhost and Amazon EC2</li>
</ul>
Settings.xml
============
Use the settings.xml provided and change the passwords.

Maven Commands
==============
Run all maven commands on the parent pom.xml.
Arifacts were created using the standard quickstart archetypes for java and webapps.

POMs
====
Parent pom.xml contains the version of the artifacts to use in the dependencyManagement and pluginManagement
tags. <br/>
Child pom.xml will <b>still need to defined their dependencies</b>, but without versions since they are managed
by the parent pom.xml. This is NOT required for plugins.


Maven Profiles
==============
The parent pom.xml defines 3 profiles which are used for deploying the project to 3 different enviornments:

<p><b>Development</b> : This is your local environment. Maven will create war and upload it to your local server.
<br/>
To use this profile use:<br />
<code>mvn clean install tomcat7:deploy -Denv=dev</code><br/>
If an earlier version of the app is already deployed on tomcat then use:<br/>
<code>mvn clean install tomcat7:redeploy -Denv=dev</code><br/>
<b>NOTE</b>: This command will not start your tomcat, but will simply deploy. Please ensure your tomcat is already
running prior to issuing this command
</p>
<br>
<p><b>Deploy</b> : Your artifacts will be deployed to your distribution repository. Edit distributionManangement
tag to one which is appropriate for your organization.
<br>
To use this profile use:<br/>
<code>mvn clean install release:prepare -Denv=dev</code><br/>
<code>release:preform -Denv=dev</code><br/>
<b>NOTE:</b> In order to do a dry run and check if everythings is fine before a release use add --dryRun=true
to the above. Also note, that this expects all code to be committed into git, else it will fail. This will automatically
tag your code with appropriate versions in git too.
</p>
<br>
<p><b>Live</b>: This is your live server. Instead of your local tomcat instance, your live tomcat will be used.
<br>To use this profile use:<br/>
<code>mvn clean install tomcat7:deploy -Denv=live</code><br/>
If an earlier version of the app is already deployed on tomcat then use:<br/>
<code>mvn clean install tomcat7:redeploy -Denv=live</code>
</p>

Usage notes
===========
<p>Once you are ready to make your release, make sure you run your Live profile immediately after the Deploy profile since this will
ensure that correct versions are uploaded to live instead of snapshot versions.
Recommended way to completely automate the deployment process would be to issue the deploy and live profiles together
like so:<br>
<code>mvn clean install release:prepare tomcat7:deploy release:perform -Pdeploy,live</code><br/>
If the artifact is already running on the tomcat then use tomcat7:redeploy instead.<br/>
or<br>
<code>mvn clean install release:prepare tomcat7:undeploy tomcat7:deploy release:perform -Pdeploy,live</code> 
</p>
