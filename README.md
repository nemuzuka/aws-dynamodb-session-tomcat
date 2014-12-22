Amazon DynamoDB Session Manager for Apache Tomcat
=================================================

Usage Information
-----------------

This project builds on top of the [AWS SDK for Java](http://aws.amazon.com/sdkforjava) 
to provide a session manager for Tomcat 7 that persists session data in [Amazon DynamoDB](http://aws.amazon.com/dynamodb).

You can download release builds of the session manager through the 
[releases section of this project]
(https://github.com/aws/aws-dynamodb-session-tomcat/releases).

For more information on using the session manager, see the 
[session manager section in the AWS SDK for Java Developer Guide]
(http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-tomcat-session-manager.html).  

Developer Information
---------------------

You can check out the source for the session manager here, and build it with Maven.  
The official release builds use JarJar
to package all the dependencies in the session manager jar *(to provide an easy, one-jar install)* and rename classes 
*(to avoid exposing the SDK code to all web apps running in Tomcat)*.  To run with a development build, 
you'll need to copy the SDK third-party dependencies into your Tomcat install's <code>lib</code> directory.

If you encounter problems with the session manager, feel free to report them as GitHub issues for this project.  

**If you'd like to contribute a new feature or bug fix, we'd love to see GitHub pull requests from you!**



改修内容
---------------------
* sticky sessionを使用し、一度振り分けたTomcatにずっと振り分けられるようにする
* Tomcatが動作している間はDynamoDBにputしないようにcontext.xmlのmaxIdleBackupの値を調整する

ことで、

1. Tomcatが動作している間はTomcatのメモリ上のみに存在
2. Tomcatの停止前にメモリ上のSessionをDynamoDBにput
3. 別のTomcatに振り分けられた時に、DynamoDBから取得しメモリ上のSessionに格納(1の状態になる)

という状態になります。DynamoDBはあくまでもTomcatが停止した時のSession情報の格納先として使用します。
