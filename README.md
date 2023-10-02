# poc-maven-versioning-wf-github

A proof of concept for a workflow using Maven on GitHub actions:
- Pull requests:
  - A build on every push to a PR or new PR
  - File: [maven-build.yml](.github/workflows/maven-build.yml)
- `master`:
  - A Maven prepare release
  - A Maven release + upload to GitHub packages
  - File: [maven-release.yml](./.github/workflows/maven-release.yml)
- Weekly dependabot
  - File: [dependabot.yml](.github/dependabot.yml)

Important notes:
- Make sure the Maven wrapper is executable in Unix: `git update-index --chmod=+x mvnw`
- Add the Maven releases plugin to [pom.xml](pom.xml):

````xml
            <plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-release-plugin</artifactId>
				<version>${maven.releases.plugin.version}</version>
				<configuration>
					<scmCommentPrefix>[skip ci]</scmCommentPrefix>
					<tagNameFormat>v@{project.version}</tagNameFormat>
				</configuration>
			</plugin>
````

- Add SCM configuration to [pom.xml](pom.xml):

````xml
	<scm>
		<developerConnection>scm:git:https://github.com/ninckblokje/poc-maven-versioning-wf-github</developerConnection>
		<tag>HEAD</tag>
	</scm>
````

- Add distribution management to [pom.xml](pom.xml):

````xml
	<distributionManagement>
		<repository>
			<id>github</id>
			<url>https://maven.pkg.github.com/ninckblokje/poc-maven-versioning-wf-github</url>
		</repository>
	</distributionManagement>
````

- Optional: Add repository to [pom.xml](pom.xml):

````xml
		<repository>
			<id>github</id>
			<url>https://maven.pkg.github.com/ninckblokje/poc-maven-versioning-wf-github</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</repository>
````
