---
title: JDV Webservice Datasources
date: 2017-02-04
tags: jboss jdv webservice datasource
---

JBoss Data Virtualization has the ability to connect with Web Services, and treat them as a plain datasource.

## Instructions
For information on how to create a web service based datasource, take a look at these articles.
- [https://developer.jboss.org/wiki/ConsumingWebServicesAsADatasourceInTeiidDesignerJustGotWAYEasier](https://developer.jboss.org/wiki/ConsumingWebServicesAsADatasourceInTeiidDesignerJustGotWAYEasier)
- [https://access.redhat.com/site/sites/default/files/attachments/consumerest-ws-datasource_version2.pdf](https://access.redhat.com/site/sites/default/files/attachments/consumerest-ws-datasource_version2.pdf)

As the administrator, you'll need to manage these services on the server side. Setting up these data sources is not handled in the same fashion as JDBC data sources.

- You will need to configure these through the Resource Adapters tab.
  - Click on the View > to open the webservice adapter.
  - Click the Add button and enter the requested information.


- For the REST/SOAP web services, enter `org.teiid.resource.adapter.ws.WSManagedConnectionFactory`


Depending on how the web service is set up, you'll need to set up additional configuration items such as username/password for HTTPBasic authentication. A list of different configurations can be found here [https://docs.jboss.org/author/display/TEIID/Web+Service+Data+Sources](https://docs.jboss.org/author/display/TEIID/Web+Service+Data+Sources)


## Alternative Method

This method uses the `JBoss CLI`, which is less error-prone once you get the script created, you eliminate the need for manual intervention to set up. You can use the following as a template. These commands set up an endpoint with HTTP Basic Auth, which requires sending a username and password as headers on the request.
```sh
/subsystem=resource-adapters/resource-adapter=webservice/connection-definitions=mywebds:add(jndi-name=java:/mywebds, class-name=org.teiid.resource.adapter.ws.WSManagedConnectionFactory, enabled=true, use-java-context=true)
/subsystem=resource-adapters/resource-adapter=webservice/connection-definitions= mywebds /config-properties=EndPoint:add(value="http://abc.defghi")
/subsystem=resource-adapters/resource-adapter=webservice/connection-definitions= mywebds /config-properties=AuthPassword:add(value="password")
/subsystem=resource-adapters/resource-adapter=webservice/connection-definitions= mywebds /config-properties=SecurityType:add(value="HTTPBasic")
/subsystem=resource-adapters/resource-adapter=webservice/connection-definitions= mywebds /config-properties=AuthUserName:add(value="username")
/subsystem=resource-adapters/resource-adapter=webservice:activate
```