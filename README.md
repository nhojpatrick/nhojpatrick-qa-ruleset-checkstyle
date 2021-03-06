# Checkstyle Custom Configuration

[![Maven Central](https://img.shields.io/maven-central/v/com.github.nhojpatrick.qa/nhojpatrick-qa-ruleset-checkstyle?style=plastic)](https://search.maven.org/artifact/com.github.nhojpatrick.qa/nhojpatrick-qa-ruleset-checkstyle)
[![CircleCI](https://img.shields.io/circleci/build/github/nhojpatrick/nhojpatrick-qa-ruleset-checkstyle/develop?label=circleci&style=plastic)](https://circleci.com/gh/nhojpatrick/nhojpatrick-qa-ruleset-checkstyle/tree/develop)
[![coveralls](https://img.shields.io/coveralls/github/nhojpatrick/reop/branch?style=plastic)](https://coveralls.io/github/nhojpatrick/nhojpatrick-qa-ruleset-checkstyle?branch=develop)
[![Known Vulnerabilities](https://snyk.io/test/github/nhojpatrick/nhojpatrick-qa-ruleset-checkstyle/develop/badge.svg?style=plastic)](https://snyk.io/test/github/nhojpatrick/nhojpatrick-qa-ruleset-checkstyle/develop)
[![GitHub](https://img.shields.io/github/license/nhojpatrick/nhojpatrick-qa-ruleset-checkstyle?style=plastic)](https://github.com/nhojpatrick/nhojpatrick-qa-ruleset-checkstyle/blob/develop/LICENSE)

Checkstyle is a static code analysis tool to see if code adheres to 'your'
coding standing. This project contains custom checkstyle rules.

The initial version just does checks for line length and tabs.

To use checkstyle the following best practice is suggested, with these notes;
* Put into your parent pom so to simplify your projects.
* Configure the plugin via pluginManagement, as tools like versions-maven-plugin can detect available updates.
* Configure version range of this i.e. `<version>[1.0.0,1.0.999)</version>`, so latest `1.0.x` will automatically be used.
* Execution is done via a profile.
* Execution is as simple as;
  * fail at 1st violation, use `./mvnw -Pqa_checkstyle`.
  * find all violations (i.e. jenkins report), use `./mvnw -Pqa_checkstyle -Dcheckstyle.maxAllowedViolations=999999`.

```
<project>
	...
	<build>
		...
		<pluginManagement>
			<plugins>
				...
				<plugin>
					<groupId>org.apache.maven.plugins</groupId>
					<artifactId>maven-checkstyle-plugin</artifactId>
					<version>${plugins.maven.maven-checkstyle-plugin}</version>
					<configuration>
						<consoleOutput>true</consoleOutput>
						<failsOnError>true</failsOnError>
					</configuration>
					<dependencies>
						<dependency>
							<groupId>com.puppycrawl.tools</groupId>
							<artifactId>checkstyle</artifactId>
							<version>${version.com.puppycrawl.tools.checkstyle}</version>
						</dependency>
						<dependency>
							<groupId>com.github.nhojpatrick.qa</groupId>
							<artifactId>nhojpatrick-qa-ruleset-checkstyle</artifactId>
							<version>[1.0.0,1.0.999)</version>
						</dependency>
					</dependencies>
				</plugin>
				...
			<plugins>
		<pluginManagement>
		...
	<build>
	...
	<profiles>
		...
			<id>qa_checkstyle</id>
			<build>
				<defaultGoal>verify</defaultGoal>
				<plugins>
					<plugin>
						<groupId>org.apache.maven.plugins</groupId>
						<artifactId>maven-checkstyle-plugin</artifactId>
						<executions>
							<execution>
								<id>checkstyle-check</id>
								<phase>validate</phase>
								<goals>
									<goal>check</goal>
								</goals>
							</execution>
						</executions>
					</plugin>
				</plugins>
			</build>
			<properties>
				<skipTests>true</skipTests>
				<sort.skip>true</sort.skip>
			</properties>
		</profile>
		...
	</profiles>
	...
	<properties>
		<checkstyle.config.location>com/github/nhojpatrick/qa/ruleset/checkstyle/rules.xml</checkstyle.config.location>
		<checkstyle.maxAllowedViolations>0</checkstyle.maxAllowedViolations>
		<plugins.maven.maven-checkstyle-plugin>3.1.1</plugins.maven.maven-checkstyle-plugin>
		<version.com.puppycrawl.tools.checkstyle>8.30</version.com.puppycrawl.tools.checkstyle>
	</properties>
</project>
```
