# Log
로그는 기록을 남기는 것이다. 프로그램 개발이나 운영 시 발생하는 문제점을 추적하거나 운영 상태를 모니터링하는 정보를 기록하는 것이다.   
또한, 분석을 통해 통계를 낼 수도 있기 때문에 기록을 남긴다. 하지만 로그를 남기면 성능이 나빠지는데   
그 보다 로그를 통해 얻는 정보가 훨씬 많기 때문에 필요한 부분에 파일로써 로그를 남기는 것이 중요하다.   
## 환경 설정 : pom.xml
```xml
  <properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<org.springframework-version>4.3.2.RELEASE</org.springframework-version>
		<jcloverslf4j.version>1.7.6</jcloverslf4j.version>
		<logback.version>1.1.1</logback.version>
	</properties>

	<dependencies>
		<!-- spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>

			<!-- JCL 제외 -->
			<exclusions>
			   <exclusion>
				  <groupId>commons-logging</groupId>
				  <artifactId>commons-logging</artifactId>
			   </exclusion>
			</exclusions>
		</dependency>
		
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- Logback --> 
		<dependency>                                    
			<groupId>org.slf4j</groupId>                
			<artifactId>jcl-over-slf4j</artifactId>     
			<version>${jcloverslf4j.version}</version>  
		</dependency>
		<dependency>
			<groupId>ch.qos.logback</groupId>
			<artifactId>logback-classic</artifactId>
			<version>${logback.version}</version>
		</dependency>
	</dependencies>
```
## src/main/resources/logback.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <!-- 콘솔로 로그를 남긴다 -->
	<appender name="consoleAppender" class="ch.qos.logback.core.ConsoleAppender">
		<encoder>
			<charset>UTF-8</charset>
			<Pattern>
				%d{HH:mm:ss.SSS} [%thread] %-5level %logger{5} - %msg%n
			</Pattern>
		</encoder>
	</appender>
  
  <!-- 파일로 로그를 남긴다 -->
	<appender name="fileAppender2" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<file>/logex/logex2.log</file>
		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
			<Pattern>
				%d{HH:mm:ss.SSS} [%thread] %-5level %logger{5} - %msg%n
			</Pattern>
		</encoder>
    <!-- 파일을 덮어쓰는 정책 -->
		<rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
			<FileNamePattern>/logex/logex2.%i.log.zip</FileNamePattern>
      <!--
        MinIndex가 1이고, MaxIndex가 10이므로 위의 파일 이름 패턴에 따라 로그 파일이 생긴다.   
        logex2.1.log.zip, ... logex2.10.log.zip 이 상태에서 10KB가 넘으면 logex2.1.log.zip이 된다.
      -->
			<MinIndex>1</MinIndex>
			<MaxIndex>10</MaxIndex>
		</rollingPolicy>
    <!-- 로그를 남기는 파일의 용량이 50KB가 넘으면 이를 압축 파일로 만들고 새로 로그 파일을 만들라는 정책
		<triggeringPolicy
			class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
			<MaxFileSize>50KB</MaxFileSize>
		</triggeringPolicy>
	</appender>
	
	<appender name="fileAppender3" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<file>/logex/logex3.log</file>
		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
			<Pattern>
				%d{HH:mm:ss.SSS} [%thread] %-5level %logger{5} - %msg%n
			</Pattern>
		</encoder>
		<rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
			<FileNamePattern>/logex/logex3.%i.log.zip</FileNamePattern>
			<MinIndex>1</MinIndex>
			<MaxIndex>10</MaxIndex>
		</rollingPolicy>
		<triggeringPolicy
			class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
			<MaxFileSize>10KB</MaxFileSize>
		</triggeringPolicy>
	</appender>	


	<!--
		com.douzone.logex2 아래 패키지 로그들만  \logex\logex2.log 파일에만  출력하는 로거
	-->
	<logger name="com.douzone.logex2"  level="debug" additivity="false">
            <appender-ref ref="fileAppender2" />
    </logger>
    
	<!--
		com.douzone.logex3 아래 패키지 로그들만  \logex\logex3.log 파일과 콘솔로 출력하는 로거
	-->
	<logger name="com.douzone.logex3"  level="warn" additivity="false">
           <appender-ref ref="fileAppender3" />
			<appender-ref ref="consoleAppender" />
    </logger>    
	
	<!-- 루트(글로벌) 로거 : 위의 logger에 해당하지 않으면 root 로거가 실행된다. -->
	<root level="warn">
		<appender-ref ref="consoleAppender" />
	</root>

</configuration>
```
## Test
```java
package com.douzone.logex1.controller;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class ExampleController {
	
	/**
	 *  Logger 생성
	 */
	private static final Log LOG = LogFactory.getLog( ExampleController.class );
	
	@RequestMapping( "/ex1" )
	@ResponseBody
	public String ex1() {
		
		/**
		 *  이 예제에서는 logback.xml 에서,
		 *  
		 *  1. consoleAppender 와 root logger 만 세팅해서 테스트 해 볼 수 있습니다. 
		 *  2. root logger의 level="DEBUG" 조정해 봅니다.
		 *  
		 *  3. 테스트 해보면,
		 *     DEBUG  > INFO > WARN > ERROR 순으로 로그가 출력 되는 것을 확인할
		 *     수 있습니다.
		 *     
		 *     가령, WARN 설정하면, warn, error 메서드의 로그 메세지만 출력 됩니다.
		 *     직접 테스트 해 보세요.
		 *  
		 */
		LOG.debug( "#ex1 - debug log" );
		LOG.info( "#ex1 - info log" );
		LOG.warn( "#ex1 - warn log" );
		LOG.error( "#ex1 - error log" );
		
		return "Logback Logging Example1";
	}
}
```
