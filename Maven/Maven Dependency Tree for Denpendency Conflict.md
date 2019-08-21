# Maven Dependency Tree for Denpendency Conflict

Description:

Sometimes, the differenct dependencies defined in pom.xml file may import several different versions of a same jar file to our project. Theses jar files with different versions may cause dependency conflict:

Some version will not be loaded by ClassLoader, but in some case, the code need the one which is not loaded, then error (can be compile error or runtime error) occurs.



To investigate:

To investigate this, we can use `mvn dependency:tree` under the directory which contains pom.xml to check the transitive dependencies. 

If we would like to know more information about these dependency jar files in our project, we can use `mvn dependency:tree -Dverbose` to show more info.

If you do not want to analyze the result in your terminal console, you can add `-DoutputFile` to print the result into a local file.



### Example:

Error:

```
......
Caused by: org.springframework.beans.BeanInstantiation: Failed to instantiate [<xxx calss related to cassandra>]: ......
Caused by: java.lang.AbstractMethodError: Reveiver class io.netty.channel.nio.NioEventLoopGroup does not define or inherit an implementation of the resolved method abstract newChild(Ljava/util/concurrent/Executor;[Ljava/lang/Object;) Lio/netty/util/concurrent/EventExecutor; of abstract class io.netty.util.concurrent.MultithreadEventExecutorGroup
```

Investigate:

Try to find io.netty.util.concurrent.MultithreadEventExecutorGroup is in which jar file --> net-common

Then check the dependency tree

```
cd <directory which contains pom.xml>
mvn dependency:tree -Dverbose -DoutputFile=<any file name>.txt
```

Result Example:

```
| | | ......
| | +-io.netty:netty-common:jar:4.1.36.Final:compile
| ......
+-com.datastax.cassandra:cassandra-driver-core:jar:3.6.0:compile
| +-io.netty:netty-handler:jar:4.0.56.Final:compile
| | +-io.netty:netty-buffer:jar:4.0.56.Final:compile
| | | \-(io.netty:netty-common:jar:4.0.56.Final:compile - omitted for conflict with 4.1.36)
| | +-io.netty:netty-transport:jar:4.0.56.Final:compile
| | | \-(io.netty:netty-buffer:jar:4.0.56.Final:compile - omitted for duplicate)
| | \-io.netty:netty-codec:jar:4.0.56.Final:compile
| |   \-(io.netty:netty-transport:jar:4.0.56.Final:compile - omitted for duplicate)
```

Analyze:

1. The problem is related to netty and Cassandra. 

2. From the dependency tree, `io.netty:netty-common:jar:4.1.36.Final:compile` is imported from somewhere else (not from Cassandra) compiled. `io.netty:netty-common:jar:4.0.56.Final` is imported from Cassandra, but **omitted** (is not loaded by ClassLoader) for the conflict with version `4.1.36` .
3. Guess maybe Cassandra need `4.0.56`
4. Project Structure --> library --> replace the dependency `4.1.36`'s source and class with `jar: 4.0.56`
5. Step 4 fixed the issue, now we know that we need to make `4.0.56` be the "default" one in our pom.xml.

Fix:

Add `<exclusion>` tage under the dependency which has a denpendency on `4.1.36` . Fixed.



