### Installing Java SDK (*viz.* ORM driver)
1. Download

Add the dependency into the project within `pom.xml` and download the package and its transitive dependencies.
```
<dependencies>
    <dependency>
        <groupId>com.surrealdb</groupId>
        <artifactID>surrealdb</artifactID>
        <version>0.2.1</version>
    </dependency>
</dependencies>
```
[sudo xbps-install -S apache-maven;] mvn clean install
Check if surrealdb-<version>.jar file is present: `ls ~/.m2/repository/com/surrealdb/surrealdb/<version>`

*Nota bene*: `.jar` stands for 'Java archive" and aggregates classes, metadata and hypermedia resources. 
Jar file's format are intended for distribution.


2. Package and import
```
`package com.surrealdb.starter;`

import com.surrealdb.Surreal;
```

### Local connexion
Start an in-memory server for getting started without the environment's configuration complexity.
1. In a terminal window:
surreal start --log debug (--user root --pass root) / (--unauthenticated) memory
2. On another terminal window:
surreal sql --ns ns --db db [--user root --pass root] --pretty


### GUI
Reach https://www.surrealist.app/start to use the graphical user interface and click "Create connexion"
Try out the following settings for extra simplicity:
- protocol: WS
- hostname and port: localhost:8000 (as reported by the starting node event logged by debug-level setting)
- user: root
- pass word: root
- Click "Create" button then rendez-vous to the "Query" sub-menu to the left of the screen.