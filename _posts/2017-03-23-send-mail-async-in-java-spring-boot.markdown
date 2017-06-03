---
layout: post
title: "Send Mail Async in Java Spring Boot"
date: 2017-03-23 16:52:00 +0700
categories: java, spring, spring-boot
meta_keywords: send mail async, send mail spring boot, async spring boot
meta_description: "Send Mail Async in Java Spring Boo"
comments: true
---

Java is an old "toy", but still good to use until now. I'm interested in exploring more about Java Framework. One of most popular Java framework is Spring.

I want to share my experience when trying to send email asyncronuously in Spring Boot. I use 3rd party mail sender, it is Sendgrid. You can [register Sendgrid for free](https://sendgrid.com) then try sending email up to 100 per day. Before start make sure Maven already installed on your machine.

## Project folder structure
```
- send-mail-async/
  - src/
    - main/
      - java/
        - mail/
          - Application.java
          - MainController.java
          - MailerService.java
          - MailerHelper.java
  - pom.xml
```

Let's create the directories.

```
mkdir -p send-mail-async/src/main/java/mail
```

Then create the pom file.

```
cd send-mail-async
touch pom.xml
```

**pom.xml**

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.springframework</groupId>
  <artifactId>send-mail-async</artifactId>
  <version>0.1.0</version>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.2.RELEASE</version>
  </parent>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>com.jayway.jsonpath</groupId>
      <artifactId>json-path</artifactId>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>com.sendgrid</groupId>
      <artifactId>sendgrid-java</artifactId>
      <version>3.1.0</version>
    </dependency>
  </dependencies>

  <properties>
    <java.version>1.8</java.version>
  </properties>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
    </plugins>
  </build>

  <repositories>
    <repository>
      <id>spring-releases</id>
      <url>https://repo.spring.io/libs-release</url>
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
      <id>spring-releases</id>
      <url>https://repo.spring.io/libs-release</url>
    </pluginRepository>
  </pluginRepositories>
</project>
{% endhighlight %}

We are using Send Grid SDK to help us send request.
## Source code

**MailerHelper.java**

{% highlight java %}
package mail;

import java.io.IOException;
import com.sendgrid.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Component
public class MailerHelper {
  private static final Logger logger = LoggerFactory.getLogger(MailerHelper.class);

  public void sendMail(String address, String subject, String message) {
    Email from = new Email("wilianto.indr@gmailss.com");
    Email to = new Email(address);
    Content emailContent = new Content("text/plain", message);

    SendGrid sendgrid = new SendGrid("YOUR_SENDGRID_KEY");
    Request request = new Request();

    Mail mail = new Mail(from, subject, to, emailContent);

    try {
      request.method = Method.POST;
      request.endpoint = "mail/send";
      request.body = mail.build();
      Response response = sendgrid.api(request);
      logger.info("--> Status: " + response.statusCode);
      logger.info("--> Body: " + response.body);
      logger.info("--> Header: " + response.headers);
    } catch(IOException ex) {
      logger.error(ex.getMessage());
    }
  }
}
{% endhighlight %}

**MailService.java**

{% highlight java %}
package mail;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Service;
import org.springframework.scheduling.annotation.Async;
import org.springframework.beans.factory.annotation.Autowired;

@Service
public class MailerService {
  private static final Logger logger = LoggerFactory.getLogger(MailerService.class);

  @Autowired
  private MailerHelper mailerHelper;

  @Async
  public void sendMail(String address, String subject, String message) {
    mailerHelper.sendMail(address, subject, message);
  }
}
{% endhighlight %}

Make sure use **@Async** annotation.

**MailController.java**

{% highlight java %}
package mail;

import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.beans.factory.annotation.Autowired;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@RestController
public class MainController {
  private static final Logger logger = LoggerFactory.getLogger(MainController.class);

  @Autowired
  private MailerService mailerService;

  @Autowired
  private MailerHelper mailerHelper;

  @RequestMapping("send-mail")
  public String sendMail(@RequestParam(value="address") String address,
                         @RequestParam(value="subject") String subject,
                         @RequestParam(value="message") String message) {
    Long start = System.currentTimeMillis();
    logger.info("starting controller");
    mailerHelper.sendMail(address, subject, message);
    return String.format("Message sent to %s in %d ms", address, System.currentTimeMillis() - start);
  }

  @RequestMapping("send-mail-async")
  public String sendMailAsync(@RequestParam(value="address") String address,
                         @RequestParam(value="subject") String subject,
                         @RequestParam(value="message") String message) {
    Long start = System.currentTimeMillis();
    logger.info("starting controller");
    mailerService.sendMail(address, subject, message);
    return String.format("Message sent to %s in %d ms", address, System.currentTimeMillis() - start);
  }
}

{% endhighlight %}

**Application.java**

{% highlight java %}
package mail;

import java.util.concurrent.Executor;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.annotation.AsyncConfigurerSupport;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@SpringBootApplication
@EnableAsync
public class Application extends AsyncConfigurerSupport {
  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
  }

  @Override
  public Executor getAsyncExecutor() {
    ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
    executor.setCorePoolSize(2);
    executor.setMaxPoolSize(2);
    executor.setQueueCapacity(500);
    executor.setThreadNamePrefix("send-mailer-");
    executor.initialize();
    return executor;
  }
}
{% endhighlight %}

You need **@EnableAsync** annotation on your Main class. Then extends a **AsyncConfigurerSupport** override the executor method. You can configure your async thread.

Start the application via terminal by running

```
mvn spring-boot:run
```

Then try to access both send mail endpoints.

This is the console output when running syncronous send mail **/send-mail**.
```
Request:
http://localhost:8080/send-mail?address=indrawan@gmail.com&amp;subject=this-is-subject&amp;message=hello

Response:
Message sent to indrawan@gmail.com in 2955 ms
```

Console Output:
**Console Output for Blocking Send Mail**

```
2017-03-24 00:08:51.223 INFO 8475 --- [nio-8080-exec-1] mail.MainController : starting controller
2017-03-24 00:08:52.584 INFO 8475 --- [nio-8080-exec-1] mail.MailerHelper : --> Status: 202
2017-03-24 00:08:52.584 INFO 8475 --- [nio-8080-exec-1] mail.MailerHelper : --> Body: 
2017-03-24 00:08:52.584 INFO 8475 --- [nio-8080-exec-1] mail.MailerHelper : --> Header: {X-Frame-Options=DENY, Server=nginx, Access-Control-Allow-Origin=https://sendgrid.api-docs.io, Access-Control-Allow-Methods=POST, Connection=keep-alive, X-Message-Id=9S6h7IUjSouQW0HLjplRyQ, X-No-CORS-Reason=https://sendgrid.com/docs/Classroom/Basics/API/cors.html, Content-Length=0, Access-Control-Max-Age=600, Date=Thu, 23 Mar 2017 17:08:52 GMT, Access-Control-Allow-Headers=Authorization, Content-Type, On-behalf-of, x-sg-elas-acl, Content-Type=text/plain; charset=utf-8}
```

This endpoint will run in a thread (nio-8080-exec-1). So it must wait until sending process done then send response to client. Client will get longer response time.

This is the console output when running syncronous send mail **/send-mail-async**.
```
Request:
http://localhost:8080/send-mail-async?address=indrawan@gmail.com&amp;subject=this-is-subject&amp;message=hello

Response:
Message sent to indrawan@gmail.com in 6 ms
```

Console Output:
**Console Output for Async Send Mail**

```
2017-03-24 00:10:49.337 INFO 8475 --- [nio-8080-exec-3] mail.MainController : starting controller
2017-03-24 00:10:50.056 INFO 8475 --- [ send-mailer-1] mail.MailerHelper : --> Status: 202
2017-03-24 00:10:50.057 INFO 8475 --- [ send-mailer-1] mail.MailerHelper : --> Body: 
2017-03-24 00:10:50.057 INFO 8475 --- [ send-mailer-1] mail.MailerHelper : --> Header: {X-Frame-Options=DENY, Server=nginx, Access-Control-Allow-Origin=https://sendgrid.api-docs.io, Access-Control-Allow-Methods=POST, Connection=keep-alive, X-Message-Id=iNLB4E4uRBiHiu-Fhin68g, X-No-CORS-Reason=https://sendgrid.com/docs/Classroom/Basics/API/cors.html, Content-Length=0, Access-Control-Max-Age=600, Date=Thu, 23 Mar 2017 17:10:50 GMT, Access-Control-Allow-Headers=Authorization, Content-Type, On-behalf-of, x-sg-elas-acl, Content-Type=text/plain; charset=utf-8}
```

This endpoint will run in two thread (nio-8080-exec-1 and send-mailer-1), first the controller thread to handle request then the send-mailer thread to send request to sendgrid. So it will not wait until sending process done. Client will get faster response.

That's my simple experiment with Async Spring Boot.
