# JBoos

### standalone JBoss


#### follow the steps to configure the standalone JBoss:


Open the standalone.xml from the <JBoss_Home>/standalone/configuration.

Configure Hostname/IP in the standalone.xml file for JBoss, as below:

```<interfaces>```
	```	<interface name="management">```
		```	<inet-address value="127.0.0.1"/>```
		```</interface>```
		```<interface name="public">```
			```<inet-address value="<myHostName>"/>```
		```</interface>```
	```</interfaces>```


#### Configure JBoss to listen for remote management requests as below:

```Add <socket-binding name="management-native" interface="management"```

```port="${jboss.management.native.port:9999}"/> under the <socket-binding-group>```

Add following section under the <management-interfaces>

```<native-interface security-realm="ManagementRealm">```

```<socket-binding native="management-native"/>```

```</native-interface>```

For Engagement Services to work, remove the following subsystem:

```<subsystem xmlns="urn:jboss:domain:jpa:1.1">```
```<jpa default-datasource="" default-extended-persistence-inheritance="DEEP"/>|```
```</subsystem>```



#### Follow these steps to increase heap size by setting the JAVA_OPTS in the <JBOSS_DIR>\standalone\bin\standalone.sh/bat:

standalone.bat:

```set "JAVA_OPTS= -server -Xms2048m -Xmx2048m"```

standalone.sh:

```JAVA_OPTS="-server -Xms1024M -Xmx1024M"```






--------------------------------


### Domain mode JBoss



#### follow these steps to configure the domain mode JBoss:

From the <JBoss_Home>/domain/configuration folder, open the domain.xml file.

Configure Hostname/IP in the domain.xml file for JBoss, as specified in the following code snippet:


<interfaces>
	<interface name="management"/>
	<interface name="private">
		<inet-address value="${jboss.bind.address.private:127.0.0.1}"/>
	</interface>
	<interface name="public"/>
	<interface name="unsecure">
		<inet-address value="${jboss.bind.address.unsecure:127.0.0.1}"/>
	</interface>
</interfaces>


In case you are installing all the Quantum Fabric components, you must increase the heap size by changing the values under jvms in the host-master.xml file:

<jvms>
<jvm name="default">
	<heap size="2048m" max-size="2048m"/>
	<jvm-options>
		<option value="-server"/>
		<option value="-XX:MetaspaceSize=2048m"/>
		<option value="-XX:MaxMetaspaceSize=2048m"/>
	</jvm-options>
</jvm>
</jvms>


#### Configure the Log Locations - JBoss

To specify the log location where the logs for all Quantum Fabric components will be generated, you must add the following parameter in the JVM arguments present in standalone.bat/domain.bat( for Windows) or standalone.sh/domain.sh(for Unix):

```Duser.home="<log location>"```


#### Configure the Standalone Existing JBoss with Self-Signed Certificate (JBoss 7.1)

If you need to use existing JBoss with self-signed certificate, follow these steps:

Add an Existing SSL Certificate to Cacerts. For more details, [click](https://docs.kony.com/konylibrary/konyfabric/kony_fabric_linux_install_guide/Content/Post-Installation_Tasks.htm#ExistingSelf-signedcertificate) How to Add an Existing Secure Sockets Layer (SSL) Certificate.
Copy the keystore file to <JBoss_Home>/standalone/configuration folder.

Modify the standalone.xml by adding the following security-realm in the security-realms section.

   <security-realm name="WebSocketRealm">
                 <server-identities>
                        <ssl>
                             <keystore path="<Keystore_file_name>" relative-to="jboss.server.config.dir" keystore password="<Keystore_password>"/>
                     </ssl>
                 </server-identities>
            </security-realm>                            


Here <Keystore_file_name> = Name of the keystore file. (for example, keystore.jks)
<Keystore_password> = Password of keystore file.

In the standalone.xml, add the following https-listener tag for default-server in the Subsystem urn:jboss:domain:undertow:3.1 .

<https-listener name="https" max-post-size="262144000" security-realm="WebSocketRealm" socket-binding="https"/>


[Configure Port Settings for Multinode Loadbalancer Setups](https://docs.kony.com/konylibrary/konyfabric/kony_fabric_linux_install_guide/Content/AppServ_Prerequisites.htm)










