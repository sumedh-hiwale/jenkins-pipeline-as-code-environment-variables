# 🚀 Jenkins Pipeline as Code - Environment Variables

## 📌 Objective

Jenkins Declarative Pipeline mein:

* ✅ Jenkins Predefined Environment Variables use karna
* ✅ Pipeline-Level Custom Environment Variables create karna
* ✅ Stage-Level Environment Variables create karna
* ✅ Variable Scope ko verify karna

---

## 📋 Prerequisites

Before starting, ensure:

* 🖥️ Jenkins Server is running
* 📂 Existing Pipeline Job: **Pipeline-as-code-Hello-world**
* 👤 Jenkins user has permission to configure and build jobs

---

# 🧪 Demo 1: Jenkins Predefined Environment Variable

## 🔧 Steps

1. Open Jenkins Dashboard.
2. Open **Pipeline-as-code-Hello-world** job.
3. Click **Configure**.
4. In the Pipeline section, add the following code:

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

### 🔍 Verification

* Jenkins predefined variable **BUILD_ID** is displayed successfully.

---

# 🧪 Demo 2: Create a Pipeline-Level Environment Variable

## 🔧 Changes Made

### Add Pipeline-Level Environment Block

```groovy
pipeline {
   agent any

   environment {
       name = 'sumedh'
   }

   stages {
```

### Update Environment Variable Stage

```groovy
stage('Environment Variable') {
   steps {
       sh 'echo "${BUILD_ID}"'
       sh 'echo "${name}"'
   }
}
```

---

## ▶️ Steps

1. Open **Configure**.
2. Add the pipeline-level environment block.
3. Add:

```groovy
sh 'echo "${name}"'
```

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

### 🔍 Verification

* ✅ BUILD_ID is displayed.
* ✅ Custom variable **name** is displayed successfully.

---

# 🧪 Demo 3: Verify Pipeline-Level Variable Scope

## 🔧 Changes Made

Update **Deploy on test** stage:

```groovy
stage('Deploy on test') {
   steps {
       echo 'Deploy on test1'
       sh 'echo "${name}"'
   }
}
```

---

## ▶️ Steps

1. Open **Configure**.
2. Add:

```groovy
sh 'echo "${name}"'
```

inside Deploy on test stage.

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

### 🔍 Verification

* ✅ Variable **name** is accessible in another stage.
* ✅ Pipeline-level variables are available throughout the pipeline.

---

# 🧪 Demo 4: Create a Stage-Level Environment Variable

## 🔧 Changes Made

### Add Stage-Level Environment Block

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

### Update Deploy on Test Stage

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

## ▶️ Steps

1. Open **Configure**.
2. Add stage-level variable:

```groovy
environment {
   username = 'myusername'
}
```

inside Environment Variable stage.

3. Add:

```groovy
sh 'echo "${username}"'
```

inside Deploy on test stage.

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

### Deploy on Test Stage

```bash
+ echo sumedh
sumedh

+ echo
```

---

# 🏁 Final Pipeline Code

```groovy
pipeline {
    agent any

    environment {
        name = 'sumedh'
    }

    stages {
        stage('Run a Command') {
            steps {
                sh 'date'
                sh 'ls'
                sh 'pwd'
            }
        }

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

        stage('Deploy on test') {
            steps {
                echo 'Deploy on test1'
                sh 'echo "${name}"'
                sh 'echo "${username}"'
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

---

# 🎯 Final Verification

| Variable | Scope              | Available In                    |
| -------- | ------------------ | ------------------------------- |
| BUILD_ID | Jenkins Predefined | Entire Pipeline                 |
| name     | Pipeline-Level     | All Stages                      |
| username | Stage-Level        | Only Environment Variable Stage |

### 📌 Result

✅ **name** is available in all stages.

✅ **username** is available only inside the **Environment Variable** stage.

✅ Blank output in the **Deploy on test** stage confirms that **username** is out of scope.

---

# 📚 Key Learnings

* 🔹 Jenkins provides predefined environment variables like `BUILD_ID`.
* 🔹 Pipeline-level variables are globally accessible throughout the pipeline.
* 🔹 Stage-level variables are limited to their respective stage.
* 🔹 Environment blocks help manage configuration cleanly and securely.
* 🔹 Understanding variable scope is essential for writing maintainable Jenkins pipelines.

---

## 🎉 Conclusion

In this lab, we successfully explored Jenkins Environment Variables by using predefined variables, creating custom pipeline-level variables, and defining stage-level variables. We also verified how variable scope works within Declarative Pipelines, making pipeline configurations more organized and manageable.
