# Declarative vs Scripted Pipeline

## Introduction

Jenkins provides two ways to write Pipelines:

1. Declarative Pipeline
2. Scripted Pipeline

One of the most common Jenkins interview questions is:

> What is the difference between Declarative and Scripted Pipelines?

Both are used to automate CI/CD workflows but differ in syntax, flexibility, and complexity.

---

# Pipeline Overview

```text
Jenkins Pipeline
       |
+------+------+
|             |
Declarative  Scripted
```

---

# What is a Declarative Pipeline?

Declarative Pipeline is a structured and simplified way to define CI/CD pipelines.

Introduced to make Jenkins Pipelines:

- Easier to read
- Easier to maintain
- Less error-prone

---

# Example

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {
                echo 'Building Application'
            }

        }

    }

}
```

---

# Characteristics

## Structured Syntax

Must follow predefined structure.

---

## Easy to Learn

Suitable for beginners.

---

## Better Readability

Easy for teams to understand.

---

## Built-in Validation

Jenkins validates syntax before execution.

---

# Declarative Pipeline Architecture

```text
Pipeline
    |
Agent
    |
Stages
    |
Steps
```

---

# What is a Scripted Pipeline?

Scripted Pipeline is the original Jenkins Pipeline format.

Uses:

```text
Groovy Programming Language
```

Provides maximum flexibility.

---

# Example

```groovy
node {

    stage('Build') {

        echo 'Building Application'

    }

}
```

---

# Characteristics

## Flexible

Supports advanced logic.

---

## Groovy Based

Requires Groovy knowledge.

---

## Complex

Can become difficult to maintain.

---

## Full Programming Capability

Supports:

```text
Loops

Conditions

Functions

Variables
```

---

# Scripted Pipeline Architecture

```text
Node
  |
Stage
  |
Groovy Logic
```

---

# Example: Conditional Logic

## Declarative

```groovy
pipeline {

    agent any

    stages {

        stage('Deploy') {

            when {
                branch 'main'
            }

            steps {
                echo 'Deploying'
            }

        }

    }

}
```

---

## Scripted

```groovy
node {

    if (env.BRANCH_NAME == "main") {

        stage('Deploy') {

            echo 'Deploying'

        }

    }

}
```

---

# Example: Loop

Scripted Pipeline:

```groovy
node {

    for(int i=1; i<=3; i++) {

        echo "Build ${i}"

    }

}
```

---

Declarative Pipeline has limited support and usually requires a script block.

```groovy
script {

    for(int i=1; i<=3; i++) {

        echo "Build ${i}"

    }

}
```

---

# Comparison Table

| Feature | Declarative | Scripted |
|----------|------------|-----------|
| Syntax | Structured | Flexible |
| Learning Curve | Easy | Difficult |
| Readability | High | Medium |
| Validation | Built-in | Less Strict |
| Groovy Knowledge | Minimal | Required |
| Recommended | Yes | Advanced Use Cases |
| Maintenance | Easy | Harder |

---

# Pipeline Structure Comparison

## Declarative

```groovy
pipeline {

    agent any

    stages {

        stage('Build') {

            steps {

                echo 'Build'

            }

        }

    }

}
```

---

## Scripted

```groovy
node {

    stage('Build') {

        echo 'Build'

    }

}
```

---

# Real World Example

Suppose you need:

```text
Build

Test

Deploy
```

Simple CI/CD workflow.

Use:

```text
Declarative Pipeline
```

---

Suppose you need:

```text
Complex Conditions

Dynamic Stages

Loops

Custom Logic
```

Use:

```text
Scripted Pipeline
```

---

# Advantages of Declarative Pipeline

## Simpler Syntax

Easy to understand.

---

## Better Team Collaboration

Readability is higher.

---

## Built-in Features

Supports:

```text
Post Actions

Parameters

Environment Variables
```

directly.

---

## Easier Maintenance

Suitable for enterprise projects.

---

# Advantages of Scripted Pipeline

## Maximum Flexibility

Full Groovy programming support.

---

## Dynamic Behavior

Can create stages dynamically.

---

## Advanced Automation

Useful for complex workflows.

---

# Disadvantages of Declarative Pipeline

## Less Flexible

Complex logic may be difficult.

---

## Limited Customization

Some advanced workflows require script blocks.

---

# Disadvantages of Scripted Pipeline

## Complex Syntax

Harder for beginners.

---

## Maintenance Challenges

Large pipelines become difficult to manage.

---

# Production Recommendation

Most organizations prefer:

```text
Declarative Pipeline
```

because it is:

- Cleaner
- Easier to maintain
- Easier to review

---

Use Scripted Pipeline only when:

```text
Advanced Logic Required
```

---

# Interview Question

## What is a Declarative Pipeline?

### Answer

A Declarative Pipeline is a structured Jenkins Pipeline syntax that provides a simplified and readable way to define CI/CD workflows. It is recommended for most use cases because it is easier to maintain and validate.

---

# Interview Question

## What is a Scripted Pipeline?

### Answer

A Scripted Pipeline is a Groovy-based Jenkins Pipeline that provides maximum flexibility and supports advanced programming constructs such as loops, conditions, and custom logic.

---

# Interview Question

## Difference Between Declarative and Scripted Pipeline

### Answer

Declarative Pipelines use a structured syntax and are easier to read and maintain, while Scripted Pipelines use Groovy scripting and provide greater flexibility for complex workflows.

---

# Interview Question

## Which Pipeline Type Do You Prefer?

### Answer

I generally prefer Declarative Pipelines because they are easier to read, maintain, and validate. For highly customized workflows requiring advanced logic, I use Scripted Pipelines.

---

# Interview Question

## Can Declarative Pipelines Execute Groovy Code?

### Answer

Yes. Declarative Pipelines can execute Groovy code using the script block.

Example:

```groovy
script {

    if(true) {

        echo 'Hello'

    }

}
```

---

# Best Practices

- Prefer Declarative Pipelines.
- Store Jenkinsfiles in Git.
- Keep stages small.
- Use Shared Libraries.
- Avoid excessive Groovy complexity.
- Implement proper error handling.

---

# Memory Trick

```text
Declarative
     |
Simple
Readable
Structured
```

---

```text
Scripted
     |
Flexible
Groovy
Advanced
```

---

# Summary

Declarative Pipeline:

```text
Easy
Structured
Recommended
```

---

Scripted Pipeline:

```text
Flexible
Powerful
Complex
```

Comparison:

```text
Declarative -> Most Projects

Scripted -> Advanced Logic
```

For modern Jenkins environments, Declarative Pipelines are generally preferred because they provide cleaner syntax, easier maintenance, and better collaboration across teams.
