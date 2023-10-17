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

