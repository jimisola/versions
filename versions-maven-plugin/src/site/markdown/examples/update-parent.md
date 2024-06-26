title: Update Parent
author: Stephen Connolly
date: 2008-09-02

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

# Update Parent

Here's an example:

```shell
svn checkout http://svn.codehaus.org/mojo/tags/xmlbeans-maven-plugin-2.3.1 xmlbeans-maven-plugin
cd xmlbeans-maven-plugin
mvn versions:update-parent
```

Which produces the following output:

```log
[INFO] Scanning for projects...
...
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven XML Beans Plugin
[INFO]    task-segment: [versions:update-parent]
[INFO] ------------------------------------------------------------------------
[INFO] [versions:update-parent]
...
[INFO] artifact org.codehaus.mojo:mojo: checking for updates from codehaus.org
[INFO] artifact org.codehaus.mojo:mojo: checking for updates from central
[INFO] Updating parent from 14 to 17
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 24 seconds
[INFO] Finished at: Thu Aug 14 15:17:04 IST 2008
[INFO] Final Memory: 9M/166M
[INFO] ------------------------------------------------------------------------
```

You can restrict the versions to be considered:

```shell
mvn versions:update-parent "-DparentVersion=[14,16)"
```

Note that, by default, -SNAPSHOT versions are ignored. You can force snapshots to be
included with the allowSnapshots parameter:

```shell
mvn versions:update-parent -DallowSnapshots=true
```

