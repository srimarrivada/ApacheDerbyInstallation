## **Apache Derby Installation in Windows**
**Apache Derby** is an open source relational database written in Java. Apache Derby is used in the JDK and is called Java DB (**Apache Derby** and **Java DB** are same). Apache Derby is the reference implementation for JDBC4.0 and compatible for ANSI-SQL. 

Follow these steps to install Apache Derby in Windows operating system. You can also go through [this PDF document](doc/Apache%20Derby%20Installation%20in%20Windows.pdf) for installation steps along with screenshots.
<br>
<br>

## **Installation:**
Apache Derby database requires Java Runtime Environment (JRE) to run and it works on all Java releases starting from 1.3 to 21. 

### **Install JDK:**
To run Apache Derby, we must have either **JRE** or fully packed **JDK** (Java Development Kit) that includes JRE, JVM and other tools.

If JDK or JRE is not currently available in your machine, then install JDK from [Oracle Java Downloads](https://www.oracle.com/java/technologies/downloads/) website and configure JAVA_HOME and PATH environment variables. For the complete JDK installation steps, refer [here](https://github.com/srimarrivada/JDK-Installation/blob/main/Install%20JDK8%20on%20Windows.md). 

If you are unable to download Oracle JDK, then install JRE 8 for Windows from the [official Java Download](https://www.java.com/en/download/) website.

After JDK is installed and configured, run the following command on **Windows Command Prompt** or **Windows PowerShell** to verify the installed Java version.
```
java -version
```

### **Install Derby:**
After JDK is installed, we need to install Apache Derby from the [Apache Derby Downloads](https://db.apache.org/derby/derby_downloads.html) website. </br>

1. Go to **Apache Derby Downloads** website and choose the appropriate Derby version for the corresponding Java release running in your machine.</br>
For example, I am choosing **Apache Derby for Java 8** since I have JDK 8 release in my machine. The latest available **Derby version for Java 8** is **10.14.2.0** which we are going to install.

2. Click on **10.14.2.0** link in **Apache Derby Downloads** website and it takes us to [Apache Derby 10.14.2.0 Release](https://db.apache.org/derby/releases/release-10_14_2_0.html) page.

3. Download `db-derby-10.14.2.0-bin.zip` file for Windows installation and it gets downloaded to your default **Downloads** folder.

4. After the Derby file is downloaded, choose the installation directory in your machine and copy `db-derby-10.14.2.0-bin.zip` file to that directory.
For example, I am choosing my Derby installation directory as `D:\ProgramFiles\Derby`

5. Then, unzip `db-derby-10.14.2.0-bin.zip` file using WinRAR or 7-Zip tool and it creates a directory named `db-derby-10.14.2.0-bin` in the installation folder with all necessary binaries of Derby.


### **Setup Derby Home:**
After Derby binaries are downloaded, the next step is to set DERBY_HOME environment variable to the location where Derby is installed in your local machine.

1. In the Windows search bar, start typing “environment variables” and select the first match which opens up **System Properties** dialog.

2. On the **System Properties** window, press **Environment Variables** button.

3. We will add DERBY_HOME variable to **User variables** since we are configuring Derby for a single user. If you want to configure Derby for multiple users then add it to **System variables**.

4. On the **Environment Variables** dialog, press New under **User variables** tab.

5. In the New User Variable window, enter the Variable name as `DERBY_HOME` and Variable value as `D:\ProgramFiles\Derby\db-derby-10.14.2.0-bin` where Derby was installed and then press OK.

6. Now choose **Path** variable under **User variables** and click on Edit button.

7. Click on New and enter value as `%DERBY_HOME%\bin` and press OK.

8. Again, press OK to apply the changes for Environment Variables and close the window.

 
### **Verify Derby System:**
Derby provides `sysinfo` script to get the Derby system information. This script is available at `DERBY_HOME\bin` location.

Simply run the following command in **Command Prompt** or **Windows PowerShell**.
```
sysinfo
```
<br>

## **Derby Modes:**
Derby can be used in **Embedded Mode** as well as in **Server Mode** (also called **Network Mode**). </br>

In embedded mode, Derby runs within the JVM of the application in which case only that application can access the Derby database while other applications cannot access it. </br>

In server mode, Derby Network server is started and it is responsible for handling multiple database requests for different applications.</br>

### **Embedded Mode:**
When an application accesses the Derby database using the Embedded JDBC driver, the Derby engine does not run on a separate process but runs inside the JVM of application.
In embedded mode, only one JVM can boot the database at a time and multiple applications running on different JVMs cannot access the same database at once. When one application booted the database and other application tries to access it, Derby gives us the following error:</br>
`Another instance of Derby may have already booted the database.`

#### **Configure Embedded Derby:**
To use Derby in Embedded mode, first include the following jar files in the CLASSPATH variable. </br>
`derby.jar` which contains the Derby engine and the Derby Embedded JDBC driver. </br>
`derbytools.jar` which is optional, it provides the Derby tools such as **ij**. <br>

Derby provides `setEmbeddedCP.bat` script to set the CLASSPATH variable for embedded usage.

Open Command Prompt and run the following commands to call `setEmbeddedCP.bat` script.
```
cd %DERBY_HOME%\bin
setEmbeddedCP.bat
```

#### **Run Sample App in Embedded mode:**
We will run a sample application `SampleApp.java` provided by Derby and is located in `DERBY_HOME\demo\programs\simple` directory. By default, this application runs in Embedded mode. 

The `SimpleApp.java` application performs operations such as: </br>
•	Load Derby Embedded JDBC driver and start the Derby engine </br>
•	Gets an embedded connection by creating and connecting to derbyDB database </br>
•	Create a table </br>
•	Insert data to table </br>
•	Update data in table </br>
•	Select data from table </br>
•	Drop the table </br>
•	Commit transaction </br>
•	Shut down the Derby database to release resources </br>

**Note:** In embedded mode, the application should shut down the Derby database. If the database is not shutdown properly, Derby cannot perform the checkpoint when the JVM shuts down which makes longer time to reboot the database next time since Derby needs to perform recovery operation.

Go to `DERBY_HOME\demo\programs\simple` directory, compile `SimpleApp.java` application and run it using the following commands
```
cd %DERBY_HOME%\demo\programs\simple
javac SimpleApp.java
java SimpleApp
```

If you encounter an error, verify whether CLASSPATH variable is set properly or not.

After the successful execution of above application, it creates a `derbyDB` database in the location where ever `SimpleApp.java` got executed.

### **Network Mode:**
Derby Network provides a framework that embeds Derby and handles database connections even from applications running in remote machines through network client.

#### **Configure Network Derby:**
To use Derby in Network mode, first include the following jar files in the CLASSPATH variable.
`derbynet.jar` which contains the Derby network server and reference to Derby engine jar file.
`derbyclient.jar` which contains the JDBC driver for Derby engine.
`derbytools.jar`which is optional, it provides the Derby tools such as **ij**.

Derby provides `setNetworkServerCP.bat` script to set the CLASSPATH variable for network usage.

Open a new Command Prompt and run the following commands
```
cd %DERBY_HOME%\bin
setNetworkServerCP.bat 
```

**Note:**  The `setNetworkServerCP.bat` script does not set CLASSPATH variable to `derbyclient.jar` which is mandatory for Derby to know the JDBC driver to use.
So, we need to set `derbyclient.jar` to existing CLASSPATH variable manually using the following command.
```
set CLASSPATH=%CLASSPATH%;%DERBY_HOME%/lib/derbyclient.jar
```

#### **Start NetworkServer:**
Next, start the Derby NetworkServer to accept multiple client connections.

There are multiple ways to start NetworkServer based on the need.

**Start NetworkServer on Default Port:**
Run the following command in Command Prompt or Windows PowerShell to start the Derby NetworkServer which runs on default port 1527.
```
startNetworkServer
```

**Start NetworkServer on Custom Port:**
To start the NetworkServer on a different port, use `-p` option as below. By default, Derby NetworkServer accepts multiple connections from the localhost only. 
```
startNetworkServer -p 3301
```
 
**Start NetworkServer to Accept Different Host Connections:**
To allow the Derby NetworkServer accepting multiple connections from a specific host, provide the specific host IP address using `-h` option in the StartNetworkServer command. The NetworkServer will then accept connections from another host only as the localhost.
```
startNetworkServer -h IP_of_host_name
```

**Start NetworkServer to Accept all Host Connections:**
If NetworkServer is required to accept connections from localhost as well as from any other server, then start it using the following command.
```
startNetworkServer -h 0.0.0.0
```

#### **Run Sample App in Network Mode:**
We will use the same sample application `SampleApp.java` from `%DERBY_HOME%\demo\programs\simple` directory to run Derby in Network mode. 

By default, `SampleApp.java` application runs in embedded mode but when we pass the `derbyclient` argument at run time, it will create and connect to the database using the Derby Network Client JDBC driver instead.

The `SimpleApp.java` application when executed with `derbyclient` argument performs operations such as:

•	Load the Client JDBC driver and start the Network Derby engine </br>
•	Get a network connection by creating and connecting to `derbyDB` database </br>
•	Create a table </br>
•	Insert data to table </br>
•	Update data in table </br>
•	Select data from the table </br>
•	Drop the table </br>
•	Commits transaction </br>
•	DO NOT shut down the Derby database </br>

**Note:** When connecting via Network Server, we should not shut down the Derby database because other applications might be accessing the same database.

Run the following commands in Command Prompt to execute `SimpleApp.java` application connected to Network Derby.
```
cd %DERBY_HOME%\demo\programs\simple
javac SimpleApp.java
java SimpleApp derbyclient
```

If you encounter any error, verify whether CLASSPATH variable is set properly or not.

After successful execution of above application, it creates a `derbyDB` database in the location where ever NetworkServer is started. 

#### **Stop NetworkServer:**
We can directly stop the NetworkServer using `stopNetworkServer.bat` script available in `DERBY_HOME\bin` location.
```
cd %DERBY_HOME%\bin
stopNetworkServer.bat
```
<br>

## **Derby Tools:**
Derby provides various tools such as **ij**, **dblook** etc.

### **ij Tool:**
**ij** is Derby’s interactive SQL scripting tool that allows to run scripts or interactive queries against the Derby database. This tool can be used with both Derby Embedded JDBC driver and Derby Network Client driver.

To start **ij** tool, open a **Command Prompt** and run the following commands:
```
cd %DERBY_HOME%\bin
ij
```

**Connect to Embedded Derby:**
On the **ij** tool, execute the following command to connect `derbyDB` located at `D:\ProgramFiles\Derby\db-derby-10.14.2.0-bin\demo\programs\simple` directory in embedded mode.
```
connect 'jdbc:derby:D:\ProgramFiles\Derby\db-derby-10.14.2.0-bin\demo\programs\simple\derbyDB';
```

After database is connected successfully, we can run regular SQL queries to create a table, insert data, update data and select data etc.
```
create table derbyDB(num int, addr varchar(40));
insert into derbyDB values (1956,'Webster St.');
insert into derbyDB values (1910,'Union St.');
update derbyDB set num=180, addr='Grand Ave.' where num=1956;
select * from derbyDb;
```

To exit from the current database, use `disconnect;` command.

**Connect to Network Derby:**
On the **ij** tool, execute the following to command to connect `derbyDB` located at `C:\Users\hp\derbyDB` in network mode. Make sure that NetworkServer is running before connecting in network mode.
```
connect 'jdbc:derby://localhost:1527/C:\Users\hp\derbyDB;create=true';
```

After database is connected successfully, we can directly run regular SQL queries or run the SQL script file. 

For example, to run `sample_sql_script.sql` file available in the current directory.
```
run 'sample_sql_script.sql';
```

To come out of **ij** tool, simple type `exit;` command.

We can also execute the SQL script by passing it to **ij** tool directly
```
ij run_network_derby_script.sql
```

### **dblook Tool:**
**dblook** is another Derby’s tool that provides the SQL dump of the database schema. This tool can be used with both Derby Embedded JDBC driver and Derby Network Client driver.

`dblook` expects `-d` argument with a database URL that it needs to connect.</br>

For example, run the following `dblook` command to write the table `emp` to `dblook_emp_ddl.sql` file.
```
cd %DERBY_HOME%\bin
dblook -d 'jdbc:derby://localhost:1527/C:\Users\hp\derbyDB' -t emp1 -o C:\Users\hp\dblook_emp_ddl.sql
```
