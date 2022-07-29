# Private Packages
## Resources
- [Medium Blog](https://medium.com/swlh/devops-with-github-part-1-github-packages-with-gradle-c4253cdf7ca6)
- [Github Reference](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry)

## Brief

### In `build.gradle`
- `plugins`
    ```groovy
    plugins {
        id 'maven-pulish'
    }
    ```
- `publishing`
    - NOTES
        - looks for `gpr.user` and `gpr.key` in `gradle.properties`
        - `System.getenv()` gets from `env` var in `github-actions`
    ```groovy
    publishing {
        repositories {
            maven {
                name = "GithubPackages"
                url = uri("https://maven.pkg.github.com/jessepinkman9900/grpc-interface-package")
                credentials {
                    username = project.findProperty("gpr.user") ?: System.getenv("USERNAME")
                    password = project.findProperty("gpr.key") ?: System.getenv("TOKEN")
                }
            }
        }
    
        publications {
            gpr(MavenPublication) {
                from(components.java)
            }
        }
    }
    ```
  
### Package Naming
- maven
    ```xml
    <dependency>
      <groupId>sample.grpc.interface</groupId>
      <artifactId>test</artifactId>
      <version>0.0.2</version>
    </dependency>
    ```
- gradle
    ```groovy
    implementation("sample.grpc.interface:test:0.0.2")
    ```
 - NOTES
    - `groupId` comes from `group` variable in `build.gradle`
    - `artifactId` comes from `rootProject.name` in `settings.gradle`
    - `version` comes from `version` variable in `build.gradle`
    
### Github Actions
- Actions File
    - built using the base `gradle` actions file from github actions marketplace
    ```yaml
    name: Java Publish Private Package with Gradle
    
    on:
      push:
        branches: [ main ]
      pull_request:
        branches: [ main ]
    
    jobs:
      build:
    
        runs-on: ubuntu-latest
    
        steps:
        - uses: actions/checkout@v2
        - name: Set up JDK 15
          uses: actions/setup-java@v2
          with:
            java-version: '15'
            distribution: 'adopt'
        - name: Grant execute permission for gradlew
          run: chmod +x gradlew
        - name: Build with Gradle
          run: ./gradlew publish
          env:
            TOKEN: ${{ secrets.GITHUB_TOKEN }}
    
    ```
    - `${{ secrets.GITHUB_TOKEN }}` provided automatically by github actions on each run of the actions workflow
    - __IMPORTANT__
        - if `group` in `build.gradle` and `rootProject.name` stay constant in code
        - then `version` must change
            - else the `action` will error out
            