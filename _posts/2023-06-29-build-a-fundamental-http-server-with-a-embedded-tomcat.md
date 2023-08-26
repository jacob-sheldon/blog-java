---
layout: post
title:  "Build a fundamental HTTP Server with a embedded Tomcat"
date:   2023-06-29 19:03:07 +0800
categories: Hand On
---

# Build a fundamental HTTP Server with a embedded Tomcat

There are some occasions need to create a very fundamental server to test, verify or learn something.

Of course, you can create your server from java.net and javax.servlet package, but you know for this purpose you need to do too much work. Making a container from scratch is hard and complicated, even if it only covers some necessary functionalities.

For creating this server quickly, we can use an existing servlet container, such as Tomcat. In this blog, I will introduce how to use Tomcat to create a basic HTTP server and explain some important points.

## Step1 # Add dependency in Maven
For this simple project, you just need to add tomcat-embed-core dependency to Maven.


```
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-core</artifactId>
    <version>10.1.5</version>
</dependency>
```

The above tomcat-embed-core dependency's version is 10.1.5, you can use any latest version.

Then trigger maven to install dependencies and wait for it completely.

## Step2 # Create a HelloServlet class extends from HttpServlet
For an HTTP web server, handling requests is the main function, so let's create a class to complete this work.

Because this is a demo project, so we just handle GET requests and respond to a single string.

```
public static class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().println("hello servlet!");
    }
}
```


The superclass HttpServlet is in jakarta.servlet.http package which is in the tomcat library.

When the HelloServlet receives a GET request, and it will respond to hello servlet.

It's simple and clear, but it's enough for this demo project.

You can add arbitrary code to enhance this doGet method, or add other methods to handle POST, HEAD, DELETE and OPTION requests, everything depends on your business.

## Step3 # Create and start a Tomcat server
Up to now, we have everything ready. It's time to create a real tomcat server.

```
Tomcat tomcat = new Tomcat();
tomcat.setPort(8080);
tomcat.getConnector();

Context ctx = tomcat.addContext("", null);

Tomcat.addServlet(ctx, "hello", new HelloServlet());
ctx.addServletMappingDecoded("/hello", "hello");

tomcat.start();
tomcat.getServer().await();
```

First, we create a tomcat instance using Tomcat class and specify 8080 as its port, then get a connector from this tomcat instance.

The setPort can be omitted, the default port for Tomcat is also 8080. The getConnector is necessary, otherwise, your tomcat server cannot work, and you will get an error "refused to connect."

Second, add a context to the tomcat object, the ctx object represent a servlet container, and also is an individual web application.

Then, assign HelloServlet to handle "hello" requests, and mapping "/hello" URI.

Finally, start this server.

Now you can request "YOUR-IP:8080/hello" from arbitrary web browsers or other tools.

## Conclusion
In this blog, we discuss how to create a fundamental HTTP web server with the embedded Tomcat and explain some important points.

I hope this can give you some help with your confusion.

The source code for this sample project is in my GitHub.