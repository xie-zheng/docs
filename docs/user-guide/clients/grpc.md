# gRPC

## Java SDK

### Install the Java Development Kit(JDK)
Make sure that your system has JDK 8 or later installed. For more information on how to
check your version of Java and install the JDK, see the [Oracle Overview of JDK Installation documentation](https://www.oracle.com/java/technologies/javase-downloads.html)

### Create the Project
This guide demonstrates how to add Java SDK dependencies using Maven. It is advisable to utilize an integrated development environment (IDE) like Intellij IDEA or Eclipse IDE for easier configuration of Maven in building and running your project.

If you are not using an IDE, see [Building Maven](https://maven.apache.org/user-guide/development/guide-building-maven.html) for more information on how to set up your project.

### Add GreptiemDB Java SDK as a Dependency
If you are using [Maven](https://maven.apache.org/), add the following to your pom.xml
dependencies list:

```
<dependencies>
    <dependency>
        <groupId>io.greptime</groupId>
        <artifactId>greptimedb-all</artifactId>
        <version>${latest_version}</version>
    </dependency>
</dependencies>
```
The latest version can be viewed [here](https://central.sonatype.com/search?q=io.greptime).

After configuring your dependencies, make sure they are available to your project. This may require refreshing the project in your IDE or running the dependency manager.

### Connect database

Now, we can create a file to contain your application called `QuickStart.java` in the base
package directory of you project. Use the following sample code to connect database,
replacing the value of `endpoints` variable with your real connection string.

```java
package io.greptime.example;

import io.greptime.GreptimeDB;
import io.greptime.models.ColumnDataType;
import io.greptime.models.Err;
import io.greptime.models.QueryOk;
import io.greptime.models.QueryRequest;
import io.greptime.models.Result;
import io.greptime.models.SelectExprType;
import io.greptime.models.SelectRows;
import io.greptime.models.SemanticType;
import io.greptime.models.TableName;
import io.greptime.models.TableSchema;
import io.greptime.models.WriteOk;
import io.greptime.models.WriteRows;
import io.greptime.options.GreptimeOptions;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.util.List;
import java.util.Map;
import java.util.concurrent.CompletableFuture;

/**
 * @author jiachun.fjc
 */
public class QuickStart {

    private static final Logger LOG = LoggerFactory.getLogger(QuickStart.class);

    public static void main(String[] args) throws Exception {
        String endpoint = "127.0.0.1:4001";
        AuthInfo authInfo = new AuthInfo("username", "password");
        GreptimeOptions opts = GreptimeOptions.newBuilder(endpoint) //
                .authInfo(authInfo)
                .writeMaxRetries(1) //
                .readMaxRetries(2) //
                .routeTableRefreshPeriodSeconds(-1) //
                .build();

        GreptimeDB greptimeDB = new GreptimeDB();

        if (!greptimeDB.init(opts)) {
            throw new RuntimeException("Fail to start GreptimeDB client");
        }
    }
}
```

See [Java SDK in reference](/reference/sdk/java.md) to get more configurations and metrics.

### Write Data

Please refer to [write data](../write-data/grpc.md#java).

### Query Data

Please refer to [Query data](../query-data/grpc.md#java).

## Go SDK

### Install

```sh
go get github.com/GreptimeTeam/greptimedb-client-go
```

### Create Client

```go
package example

import (
    greptime "github.com/GreptimeTeam/greptimedb-client-go"
    "google.golang.org/grpc"
    "google.golang.org/grpc/credentials/insecure"
)

func InitClient() *greptime.Client {
    options := []grpc.DialOption{
        grpc.WithTransportCredentials(insecure.NewCredentials()),
    }
    // To connect a database that needs authentication, for example, those on Greptime Cloud,
    // `Username` and `Password` are needed when connecting to a database that requires authentication.
    // Leave the two fields empty if connecting a database without authentication.
    cfg := greptime.NewCfg("127.0.0.1").
        WithDatabase("public").      // change to your real database
        WithPort(4001).              // default port
        WithAuth("", "").            // `Username` and `Password`
        WithDialOptions(options...). // specify your gRPC dail options
        WithCallOptions()            // specify your gRPC call options

    client, err := greptime.NewClient(cfg)
    if err != nil {
        panic("failed to init client")
    }
    return client
}
```

See [Go SDK in reference](/reference/sdk/go.md) to get more configurations.

### Write Data

Please refer to [write data](../write-data/grpc.md#go).

### Query Data

Please refer to [Query data](../query-data/grpc.md#go).
