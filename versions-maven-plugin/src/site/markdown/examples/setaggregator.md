title: Changing the version of an Aggregator
author: Karl Heinz Marbaise
date: 2017-06-17

<!---
Licensed to the Apache Software Foundation (ASF) under one
or more contributor license agreements.  See the NOTICE file
distributed with this work for additional information
regarding copyright ownership.  The ASF licenses this file
to you under the Apache License, Version 2.0 (the
"License"); you may not use this file except in compliance
with the License.  You may obtain a copy of the License at
https://www.apache.org/licenses/LICENSE-2.0
Unless required by applicable law or agreed to in writing,
software distributed under the License is distributed on an
"AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
KIND, either express or implied.  See the License for the
specific language governing permissions and limitations
under the License.
-->

# Changing the version of an Aggregator

Let us assume we have a multi-module project which consists of the following
modules:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>
  <groupId>localdomain.localhost</groupId>
  <artifactId>parent</artifactId>
  <packaging>pom</packaging>
  <version>1.2.0</version>
  <name>Parent</name>

  <description>
    separate-module does not use this project as parent. (aggregator)
  </description>

  <modules>
    <module>separate-module</module>
    <module>module-a2</module>
    <module>module-a3</module>
  </modules>
..
</project>
```

A module `separate-module`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.soebes.smpp</groupId>
    <artifactId>smpp</artifactId>
    <version>2.3.0</version>
    <relativePath/>
  </parent>

  <groupId>com.soebes.separate</groupId>
  <artifactId>separate-module</artifactId>
  <packaging>pom</packaging>
  <version>2.0.7-SNAPSHOT</version>

</project>
```

And two other modules first `module-a2` which looks like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>localdomain.localhost</groupId>
    <artifactId>parent</artifactId>
    <version>1.2.0</version>
  </parent>

  <groupId>localdomain.localhost</groupId>
  <artifactId>module-a2</artifactId>
  <packaging>pom</packaging>
  <version>1.2.0</version>

</project>
```

An finally the module `module-a3` which looks like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>localdomain.localhost</groupId>
    <artifactId>parent</artifactId>
    <version>1.2.0</version>
  </parent>

  <groupId>localdomain.localhost</groupId>
  <artifactId>module-a3</artifactId>
  <packaging>pom</packaging>

</project>
```

So now you decide to change the version of the module to a different version like `3.6.0`.

```shell
mvn versions:set -DnewVersion=3.6.0
```

So the following output will be generated:

```log
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] separate-module
[INFO] Parent
[INFO] module-a2
[INFO] module-a3
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Parent 1.2.0
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- versions-maven-plugin:2.4:set (default-cli) @ parent ---
[INFO] Searching for local aggregator root...
[INFO] Local aggregation root: /../project
[INFO] Processing change of localdomain.localhost:parent:1.2.0 -> 3.6.0
[INFO] Processing localdomain.localhost:parent
[INFO]     Updating project localdomain.localhost:parent
[INFO]         from version 1.2.0 to 3.6.0
[INFO]
[INFO] Processing localdomain.localhost:module-a2
[INFO]     Updating parent localdomain.localhost:parent
[INFO]         from version 1.2.0 to 3.6.0
[INFO]     Updating project localdomain.localhost:module-a2
[INFO]         from version 1.2.0 to 3.6.0
[INFO]
[INFO] Processing localdomain.localhost:module-a3
[INFO]     Updating parent localdomain.localhost:parent
[INFO]         from version 1.2.0 to 3.6.0
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] separate-module .................................... SKIPPED
[INFO] Parent ............................................. SUCCESS [  1.081 s]
[INFO] module-a2 .......................................... SKIPPED
[INFO] module-a3 .......................................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.973 s
[INFO] Finished at: 2017-06-17T14:17:50+02:00
[INFO] Final Memory: 23M/369M
[INFO] ------------------------------------------------------
```

If you have carefully read the output you have seen that the module `separate-module`
is not listed here with something like `Processing ...`.

This means that this module has not been changed to get a new version as you wished by the
call.

To get that working you need to add supplemental command line parameters:

```shell
mvn versions:set -DnewVersion=3.6.0 -DoldVersion=* -DgroupId=* -DartifactId=*
```

This is needed otherwise the filter for the versions (via `-DoldVersion=*`) would have
filtered out that module cause it has a different version than the rest of modules.
Furthermore you need to add the `-DgroupId=*` and `-DartifactId=*` otherwise
the `separate-module` will also filtered out based on the differences in groupId
and artifactId.

So in end the result of the above call looks like this:

```log
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] separate-module
[INFO] Parent
[INFO] module-a2
[INFO] module-a3
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building Parent 1.2.0
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- versions-maven-plugin:2.4:set (default-cli) @ parent ---
[INFO] Searching for local aggregator root...
[INFO] Local aggregation root: /Users/kama/ws-git-so/versions-maven-plugin-issue
[INFO] Processing change of *:*:* -> 3.6.0
[INFO] Processing com.soebes.separate:separate-module
[INFO]     Updating project com.soebes.separate:separate-module
[INFO]         from version  to 3.6.0
[INFO]
[INFO] Processing localdomain.localhost:parent
[INFO]     Updating project localdomain.localhost:parent
[INFO]         from version  to 3.6.0
[INFO]
[INFO] Processing localdomain.localhost:module-a2
[INFO]     Updating parent localdomain.localhost:parent
[INFO]         from version  to 3.6.0
[INFO]     Updating project localdomain.localhost:module-a2
[INFO]         from version 1.2.0 to 3.6.0
[INFO]
[INFO] Processing localdomain.localhost:module-a3
[INFO]     Updating parent localdomain.localhost:parent
[INFO]         from version  to 3.6.0
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] separate-module .................................... SKIPPED
[INFO] Parent ............................................. SUCCESS [  1.080 s]
[INFO] module-a2 .......................................... SKIPPED
[INFO] module-a3 .......................................... SKIPPED
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.473 s
[INFO] Finished at: 2017-06-17T16:17:51+02:00
[INFO] Final Memory: 23M/310M
[INFO] ------------------------------------------------------------------------
```

