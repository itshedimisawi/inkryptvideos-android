# Publish library to Github Packages

To publish this library to Github Packages follow these steps:

### 1. Generate Github Access Token
Go to Github account settings > Developer settings > Personal access tokens (classic)
Then create a new access token.

Don't forget to enable package creation permission when creating the token.

### 2. Add access token to your Gradle project
Create a github.gradle file at your project root and add these lines:
```gradle
ext {
    usr='YOUR_USER_ID'
    key='YOUR_GENERATED_ACCESS_TOKEN'
}

```
USER_ID can be found at https://api.github.com/users/your_github_user_name

### 4. Create a new Github Repository
This repository will contain the library files

### 4. Update the Publishing Gradle Task
**This task already exists in build.gradle (com.inkryptvideos.android).**

You just need to replace repository name, username, artifact ID and group ID if needed.

And don't forget to update the version with each release.
The version doesn't need to follow a specific format. You can start over from 1.0.

```gradle
plugins {
    id 'maven-publish'
}

// Include the Gradle file you just created
apply from: rootProject.file("github.gradle")

// Gradle Task
afterEvaluate {
        publishing {
            publications {
                release(MavenPublication) {
                    from components.release
    
                    // Replace with your own group ID.
                    groupId 'com.inkryptvideos.android'
    
                    // Replace with the name of your library
                    artifactId 'inkryptvideos-android'
                    version '2.8'
                }
    
            }
    
            repositories {
                maven {
                    name = "GitHubPackages"
    
                    // Replace GITHUB_USERID with your personal or organisation user ID and
                    // REPOSITORY with the name of the repository on GitHub
                    url = uri("https://maven.pkg.github.com/YOUR_USERNAME/REPOSITORY_NAME")
    
                    credentials {
                        username = usr
                        password = key
                    }
                }
            }
        }
    }
```
### 4. Publish
All you need to do now is to run the **publishReleasePublicationToGitHubPackagesRepository** to publsih the library to the repository.

You can either do it from the terminal by executing:
```bash
./gradlew publishReleasePublicationToGitHubPackagesRepository
```
Or from the Gradle side panel:

**Inkryptvideos Android > inkryptvideos-android > Tasks > publishing > publishReleasePublicationToGitHubPackagesRepository**
