# 🚀 Jenkins Pipeline as Code - Environment Variables

## 🎯 Objective

Demonstrate the use of:

- Jenkins Predefined Environment Variables
- Pipeline-Level Environment Variables
- Stage-Level Environment Variables
- Variable Scope in Declarative Pipeline

---

## 📋 Prerequisites

- Jenkins Server Running
- Existing Pipeline Job: `Pipeline-as-code-Hello-world`

---

# 🧪 Demo 1: Jenkins Predefined Environment Variable

## 📝 Steps

1. Open Jenkins Dashboard.
2. Open **Pipeline-as-code-Hello-world**.
3. Click **Configure**.
4. In the **Pipeline** section, add:

```groovy
pipeline {
    agent any

    stages {
        stage('Run a Command') {
            steps {
                sh 'date'
                sh 'ls'
                sh 'pwd'
            }
        }

        stage('Environment Variable') {
            steps {
                sh 'echo "${BUILD_ID}"'
            }
        }

        stage('Deploy on test') {
            steps {
                echo 'Deploy on test1'
            }
        }

        stage('Deploy on prod') {
            steps {
                echo 'Deploy on prod1'
            }
        }
    }
}
```

5. Click **Save**.
6. Click **Build Now**.
7. Open **Console Output**.

---

## ✅ Expected Output

```bash
+ echo 7
7
```

---

## 🔍 Verification

- Jenkins predefined variable `BUILD_ID` is displayed successfully.

---

# 🧪 Demo 2: Create a Pipeline-Level Environment Variable

## 🔄 Changes Made

### Before

```groovy
pipeline {
    agent any

    stages {
```

### After

```groovy
pipeline {
    agent any

    environment {
        name = 'sumedh'
    }

    stages {
```

### Before

```groovy
stage('Environment Variable') {
    steps {
        sh 'echo "${BUILD_ID}"'
    }
}
```

### After

```groovy
stage('Environment Variable') {
    steps {
        sh 'echo "${BUILD_ID}"'
        sh 'echo "${name}"'
    }
}
```

---

## 📝 Steps

1. Open **Configure**.
2. Add the pipeline-level environment block.
3. Add `echo "${name}"` in the **Environment Variable** stage.
4. Click **Save**.
5. Click **Build Now**.
6. Open **Console Output**.

---

## ✅ Expected Output

```bash
+ echo 8
8

+ echo sumedh
sumedh
```

---

## 🔍 Verification

- `BUILD_ID` is displayed.
- Custom variable `name` is displayed successfully.

---

# 🧪 Demo 3: Verify Pipeline-Level Variable Scope

## 🔄 Changes Made

### Before

```groovy
stage('Deploy on test') {
    steps {
        echo 'Deploy on test1'
    }
}
```

### After

```groovy
stage('Deploy on test') {
    steps {
        echo 'Deploy on test1'
        sh 'echo "${name}"'
    }
}
```

---

## 📝 Steps

1. Open **Configure**.
2. Add:

```groovy
sh 'echo "${name}"'
```

inside **Deploy on test** stage.

3. Click **Save**.
4. Click **Build Now**.
5. Open **Console Output**.

---

## ✅ Expected Output

```bash
Deploy on test1

+ echo sumedh
sumedh
```

---

## 🔍 Verification

- Variable `name` is accessible in another stage.
- Pipeline-level variables are available throughout the pipeline.

---

# 🧪 Demo 4: Create a Stage-Level Environment Variable

## 🔄 Changes Made

### Before

```groovy
stage('Environment Variable') {
    steps {
        sh 'echo "${BUILD_ID}"'
        sh 'echo "${name}"'
    }
}
```

### After

```groovy
stage('Environment Variable') {

    environment {
        username = 'myusername'
    }

    steps {
        sh 'echo "${BUILD_ID}"'
        sh 'echo "${name}"'
        sh 'echo "${username}"'
    }
}
```

### Before

```groovy
stage('Deploy on test') {
    steps {
        echo 'Deploy on test1'
        sh 'echo "${name}"'
    }
}
```

### After

```groovy
stage('Deploy on test') {
    steps {
        echo 'Deploy on test1'
        sh 'echo "${name}"'
        sh 'echo "${username}"'
    }
}
```

---

## 📝 Steps

1. Open **Configure**.
2. Add the following stage-level variable inside the **Environment Variable** stage:

```groovy
environment {
    username = 'myusername'
}
```

3. Add:

```groovy
sh 'echo "${username}"'
```

inside **Deploy on test** stage.

4. Click **Save**.
5. Click **Build Now**.
6. Open **Console Output**.

---

## ✅ Expected Output

### Environment Variable Stage

```bash
+ echo 11
11

+ echo sumedh
sumedh

+ echo myusername
myusername
```

### Deploy on test Stage

```bash
+ echo sumedh
sumedh

+ echo
```

---

## 🔍 Verification

- `name` is available in all stages.
- `username` is available only in the **Environment Variable** stage.
- `username` is not available in the **Deploy on test** stage.
- Blank output confirms the variable is out of scope.

---

# 📊 Variable Scope Summary

| Variable | Type | Scope |
|-----------|---------|---------|
| `BUILD_ID` | Jenkins Predefined | Entire Pipeline |
| `name` | Pipeline-Level Variable | All Stages |
| `username` | Stage-Level Variable | Specific Stage Only |

---

# 🎓 Key Learning

✅ Jenkins provides built-in environment variables such as `BUILD_ID`

✅ Pipeline-level variables are available across all stages

✅ Stage-level variables are available only within the stage where they are declared

✅ Accessing a stage-level variable outside its scope returns an empty value

✅ Environment variables help manage reusable values across Jenkins Pipelines

---

# 🏆 Conclusion

In this demo, we successfully:

- Used Jenkins predefined variables
- Created custom pipeline-level variables
- Created custom stage-level variables
- Verified variable scope behavior
- Understood the difference between pipeline-level and stage-level environment variables

🚀 This demonstrates how Environment Variables work in Jenkins Declarative Pipelines using Pipeline as Code.
