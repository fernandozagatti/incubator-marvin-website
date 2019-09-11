---
layout: page
title: Interface
description: Project Community Page
group: nav-right
---
<!--
{% comment %}
Licensed to the Apache Software Foundation (ASF) under one or more
contributor license agreements.  See the NOTICE file distributed with
this work for additional information regarding copyright ownership.
The ASF licenses this file to you under the Apache License, Version 2.0
(the "License"); you may not use this file except in compliance with
the License.  You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
{% endcomment %}
-->

{% include JB/setup %}

# Toolbox CLI

### Command line interface
Usage: marvin [OPTIONS] COMMAND [ARGS]

Options:

```
  --debug       #Enable debug mode.
  --version     #Show the version and exit.
  --help        #Show this command line interface and exit.
```

Commands:

```
  engine-generate     #Generate a new marvin engine project and install...
  engine-generateenv  #Generate a new marvin engine environment and install...
  hive-dataimport     #Export and import data samples from a hive databse to...
  hive-generateconf   #Generate default configuration file
  hive-resetremote    #Drop all remote tables from informed engine on host
```

----

* [Documentation](/marvin-platform-book/ch4_documentation/overview)
