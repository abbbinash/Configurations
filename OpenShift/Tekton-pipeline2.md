## Pipeline-Example
### Here we have create a sample pipeline project where we doesn't require pvc or workspace or any other parameters. 
### We always need to paas the parameters in the pipeline. Parameters like workspace, storage, git url, image, etc. 

1. Create a Project where you will run your pipelines.

```
oc new-project pipeline-project
```

2. Create tasks to use in the pipeline.

```
touch task{1-6}.yaml
touch ctask{1-8}.yaml
touch ctaskfinisher.yaml
oc create task{1-8}.yaml
oc create ctaskfinisher.yaml
```

- Task1
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: task1
spec:
  steps:
    - name: echo
      image: ubuntu
      command:
        - echo
      args:
        - "hello world"
```
- Task2
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: task2
spec:
  steps:
    - name: echo
      image: ubuntu
      command:
        - echo
      args:
        - "Uth was here"
```
- Task3
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: task3
spec:
  steps:
    - name: echo
      image: ubuntu
      command:
        - echo
      args:
        - "Goodbye world!"
```
- Task4
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: task4
spec:
  steps:
    - name: createfile
      image: ubuntu
      command:
        - touch
      args:
        - "/uth/test.txt"
  workspaces:
    - name: working
      mountPath: /uth
```
- Task5
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: task5
spec:
  steps:
    - name: checkfile
      image: ubuntu
      command:
        - ls
      args:
        - "-al"
        - /uth/test.txt
  workspaces:
    - name: working
      mountPath: /uth
```
- Task6
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: task6
spec:
  steps:
    - name: removefile
      image: ubuntu
      command:
        - rm
      args:
        - /uth/test.txt
  workspaces:
  - name: working
    mountPath: /uth
```
- ctask1
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask1
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 1"
```
- ctask2
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask2
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 2"
```
- ctask3.yaml
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask3
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 3"
```
- ctask4.yaml
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask4
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 4"
```
- ctask5.yaml
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask5
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 5"
```
- ctask6 
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask6
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 6"
```
- ctask7.yaml
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask7
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 7"
```
- ctask8.yaml
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctask8
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "In task 8"
```
- ctaskfinisher
```
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: ctaskfinisher
spec:
  steps:
    - name: id
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - cat
      args:
        - /etc/redhat-release
    - name: echo
      image: registry.access.redhat.com/ubi8/ubi:latest
      command:
        - echo
      args:
        - "Finished!"
```
`tkn task ls`
![Alt text](image-1.png)


3. Create the pipeline.
```
vi cpipeline.yaml
apiVersion: tekton.dev/v1beta1
kind: Pipeline
metadata:
  name: cpipeline1
spec:
  tasks:
    - name: task1
      taskRef:
        kind: Task
        name: ctask1
    - name: task2
      taskRef:
        kind: Task
        name: ctask2
    - name: task3
      taskRef:
        kind: Task
        name: ctask3
    - name: task4
      taskRef:
        kind: Task
        name: ctask4
      runAfter:
        - task1
        - task2
    - name: task5
      taskRef:
        kind: Task
        name: ctask5
      runAfter:
        - task2
        - task3
    - name: task6
      taskRef:
        kind: Task
        name: ctask6
      runAfter:
        - task4
        - task5

  finally:
    - name: cleanup
      taskRef:
        kind: Task
        name: ctaskfinisher

# oc create -f cpipeline.yaml
# tkn pipeline ls
```
![Alt text](image-2.png)

4. Now go to the webconsole and start the pipeline.

![Alt text](image-4.png)
![Alt text](image-3.png)
![Alt text](image-5.png)


