---
title: "How I migrated MariaDB on Napptive Playground? and how you can migrate your own projects too."
datePublished: Sun Apr 16 2023 09:42:37 GMT+0000 (Coordinated Universal Time)
cuid: clgj7xunr000g09lc7rsc3x3y
slug: how-i-migrated-mariadb-on-napptive-playground-and-how-you-can-migrate-your-own-projects-too
tags: databases, mariadb, napptive, wemakedevs

---

Hi everyone! In this blog post, I'm going to share with you how I migrated MariaDB on Napptive Playground. **Napptive Playground** is a cloud-native platform that lets you deploy and manage applications using Open Application Model (OAM) specifications. It also has a catalog of ready-to-use applications that you can browse and install with a few clicks.

**MariaDB** is a popular open source relational database that is compatible with MySQL. It offers high performance, scalability, and security features. I wanted to migrate MariaDB database to Napptive Playground so that I can take advantage of its benefits and features.

The migration process was surprisingly easy and fast. All I needed to do was to create three files for pushing MariaDB application to the catalog:

* 2 `YAML` files and
    
* 1 `README.md` file.
    
    Let me explain what each file does and show you the code I used.
    

## Defining App configurations

The first file is `mariadb.app.yaml`. This file defines the application component and its properties, such as the image, ports, environment variables, and traits. The component type is `webservice`, which means it is long-running, scalable, containerized services that have a network endpoint attached to it. [This list](https://docs.napptive.com/oam_definitions/component_types.html) helps do the required comparison for various types of components to choose appropriate type.

The trait type is storage, which mounts an ephemeral storage to the container for storing data. Here is the code for `mariadb.app.yaml`:

```yaml
apiVersion: core.oam.dev/v1beta1
kind: Application
metadata:
  name: mariadb
  annotations:
    version: "latest"
    description: "MariaDB Server is a high performing open source relational database, forked from MySQL."
spec:
  components:
    - name: mariadb
      type: webservice
      properties:
        image: mariadb:latest
        ports:
        - port: 3306
          expose: true
        env:
        - name: MARIADB_ROOT_PASSWORD
          value: "example"
      traits:
        - type: storage
          properties:
            emptyDir:
              - name: datadir
                mountPath: /var/lib/mysql
```

Here, we have mentioned `mariadb:latest` in `image` under `properties` which takes image by default from docker hub. Depending upon the requirements of your application you might want to look at various [traits](https://docs.napptive.com/oam_definitions/trait_types.html).

## Defining Metadata for application

The second file is `mariadb.metadata.yaml`. This file provides metadata information about the application, such as the name, version, description, keywords, license, url, doc, and logo. The metadata helps other users to search and find your application in the catalog. Here is the code for `mariadb.metadata.yaml`:

```yaml
apiVersion: core.napptive.com/v1alpha1
kind: ApplicationMetadata
name: "MariaDB"
version: "latest"
description: MariaDB database
keywords:
  - "mariadb"
  - "database"
license: "GNU GPLv2"
url: "https://mariadb.org"
doc: "https://hub.docker.com/_/mariadb"
logo:
  - src: "https://i.ibb.co/8jYC88Q/mariadb.png"
    type: "image/png"
```

## Starter Instructions or guides

The third file is `README.md`. This file provides instructions for the developers who are going to use your application. You can include information such as how to install, configure, and access your application. Here is an example of `README.md`:

```markdown
# MariaDB
> MariaDB Server is one of the most popular database servers in the world. It’s made by the original developers of MySQL and guaranteed to stay open source. Notable users include Wikipedia, DBS Bank, and ServiceNow.

> The intent is also to maintain high compatibility with MySQL, ensuring a library binary equivalency and exact matching with MySQL APIs and commands. MariaDB developerscontinue to develop new features and improve performance to better serve its users.

- Deploy the application and connect to it on `0.0.0.0:3306` or `http://localhost:3306` from other applications or components in the same environment.

- Current configuration uses ephemeral storage for storing data named as `datadir`

- Default Root password can be found under **properties** section of the app after deployment.
```

That's it! With these three files, I was able to migrate MariaDB database to Napptive Playground and make it available in the catalog for others to use.

I hope you found this blog post helpful and informative. If you have any questions or feedback, please leave a comment below. Thanks for reading.