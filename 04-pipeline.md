# Jenkins Pipeline: Structure, Types, and Advantages

## What is a Jenkins Pipeline?
A Jenkins Pipeline is a suite of plugins that supports integrating and implementing continuous delivery pipelines as code. Pipelines define the entire build process, from code checkout to deployment, in a single script (usually called a Jenkinsfile).

---

## Types of Jenkins Pipelines
1. **Declarative Pipeline**
   - Modern, recommended syntax.
   - Easier to read and write.
   - Enforces a specific structure.
2. **Scripted Pipeline**
   - Uses Groovy scripting.
   - More flexible but complex.
   - Suitable for advanced use cases.

---

## Declarative Pipeline Structure
A typical Declarative Pipeline (Jenkinsfile) looks like this:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
```

### Key Sections
- **pipeline**: Root block for the pipeline.
- **agent**: Where the pipeline runs (e.g., `any`, `label`, `none`).
- **stages**: Contains one or more `stage` blocks (e.g., Build, Test, Deploy).
- **steps**: Commands or scripts to execute in each stage.
- **post**: Actions to run after the pipeline (e.g., on success or failure).

---

## Advantages of Pipelines over Freestyle Jobs
- **Pipeline as Code**: Store the build process in version control (Jenkinsfile).
- **Complex Workflows**: Support for parallelism, conditional logic, and advanced flows.
- **Repeatability**: Consistent builds across environments.
- **Scalability**: Easily manage large, multi-stage projects.
- **Auditability**: Track changes to the build process over time.
- **Resilience**: Can resume from checkpoints after failures (with proper configuration).

---

## When to Use Pipelines
- For any project requiring multiple build, test, or deploy steps.
- When you want to version control your CI/CD process.
- For complex workflows (e.g., parallel testing, multi-branch builds).
- When you need better maintainability and scalability.

---

## Why Choose Pipeline over Freestyle Jobs?
- **Freestyle jobs** are simple and good for basic tasks, but:
  - Hard to maintain for complex workflows.
  - Not easily versioned or shared.
  - Limited support for advanced features (parallelism, conditional logic).
- **Pipelines** are more powerful, maintainable, and scalable for modern CI/CD needs.

---

## Best Practices
- Use Declarative Pipeline for most use cases.
- Store your Jenkinsfile in the root of your repository.
- Use stages to organize your workflow.
- Use post actions for notifications and cleanup.



## Example: Declarative Pipeline for a Java Application

Below is a Jenkins Declarative Pipeline that clones a Java project from Git, builds it with Maven, runs tests, and starts the application using the generated JAR file.

```groovy
pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/cloudprudhvi/java-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Run') {
            steps {
                sh 'java -jar target/*.jar &'
                echo 'Java application started.'
            }
        }
    }
    post {
        success {
            echo 'Java pipeline completed successfully!'
        }
        failure {
            echo 'Java pipeline failed.'
        }
    }
}
```


