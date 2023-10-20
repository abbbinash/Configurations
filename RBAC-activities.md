# Role Based Access Control

- There are mainly 2 types of cluster roles.
  - Cluster Role
    - cluster admin - can change or update anything in the cluster.
    - cluster status - Only can see the things in the cluster.
    - self-provisioned - can create project.
  - Local Role
    - admin
    - basic-user
    - edit
    - view


## Configuring Authentication & Autherization

1. Create 4 types of users - admin, leader, developer, tester
2. Make the 'admin' user a cluster administrator.
3. As the 'admin' user, remove the ability to create projects cluster wide.
4. Create 'managers' group, and add the 'leader' user to the group.
5. Grant project creation privilages to the 'managers' group
6. Create 'developers' and 'testers' group and add the 'developer' and 'tester' user to the group.
7. Grant edit role to 'developers' and view role to 'testers' group. 

## Creating user and assigning roles 

1. Create user with htpasswd and it file.
```
htpasswd -c -B -b htpasswdfile new_user1 password
htpasswd -B -b htpasswdfile new_user2 password
```

2. Create secret.

```
oc create secret generic mylocalusers --from-file htpasswd=htpasswdfile -n openshift-config
oc get secret -n openshift-config
```
3. Update the oauth. Change the oauth file and replce it.
```
oc get oauth cluster -o yaml > oauth.yaml
vi oauth.yaml  (add the required params)
oc replace -f oauth.yaml
```
![Alt text](image-12.png)
One name is the secret which you have created and another give a name to the provider.

4. Now after the pods gets restarted you can login with the new user.

```
watch oc get pod -n openshift-authentication
```

![Alt text](image-13.png)
![Alt text](image-14.png)