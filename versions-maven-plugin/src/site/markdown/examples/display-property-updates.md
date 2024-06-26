title: Checking for new property-linked updates
author: Stephen Connolly
date: 2009-08-12

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

# Checking for new property-linked updates

The `display-property-updates` goal will check all the properties in your project and display a list
of those properties which are used to control versions of dependencies, plugins and plugin dependencies and
it will detail which properties have newer versions available.

Here are some examples of what this looks like:

```sh
svn checkout http://svn.codehaus.org/mojo/trunk/mojo/build-helper-maven-plugin build-helper-maven-plugin
cd build-helper-maven-plugin
mvn versions:display-property-updates
```

Which produces the following output:

```log
[INFO] ------------------------------------------------------------------------
[INFO] Building Build Helper Maven Plugin
[INFO]    task-segment: [versions:display-property-updates]
[INFO] ------------------------------------------------------------------------
[INFO] [versions:display-property-updates]
[INFO]
[INFO] This project does not have any properties associated with versions.
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 6 seconds
[INFO] Finished at: Wed Aug 12 13:28:04 BST 2009
[INFO] Final Memory: 11M/22M
[INFO] ------------------------------------------------------------------------
```

Another example, using a project which uses properties to control the version of multiple artifacts:

```sh
svn checkout http://svn.codehaus.org/mojo/trunk/mojo/versions-maven-plugin versions-maven-plugin
cd versions-maven-plugin
mvn versions:display-property-updates
```

Which produces the following output:

```log
[INFO] ------------------------------------------------------------------------
[INFO] Building Versions Maven Plugin
[INFO]    task-segment: [versions:display-property-updates]
[INFO] ------------------------------------------------------------------------
[INFO] [versions:display-property-updates]
[INFO]
[INFO] The following version property updates are available:
[INFO]   ${doxiaVersion} ........................................ 1.0 -> 1.1.1
[INFO]   ${doxia-sitetoolsVersion} .............................. 1.0 -> 1.1.1
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESSFUL
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 6 seconds
[INFO] Finished at: Wed Aug 12 13:28:49 BST 2009
[INFO] Final Memory: 11M/24M
[INFO] ------------------------------------------------------------------------
```

