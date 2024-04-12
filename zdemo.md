# spring-petclinic on OpenShift

https://github.com/spring-projects/spring-petclinic

GUI s2i using https://github.com/spring-projects/spring-petclinic.git

# Add database

```
oc project s2i-spring-petclinic-database
oc apply -f manifest/postgres.yml
```

# Add env to app

```
oc project s2i-spring-petclinic
oc apply -f manifest/spring-petclinic-config.yml
```

```
env:
    - name: SPRING_PROFILES_ACTIVE
      value: postgres
    - name: POSTGRES_DB
      valueFrom:
        configMapKeyRef:
          name: spring-petclinic-postgres-configmap
          key: database
    - name: POSTGRES_USER
      valueFrom:
        configMapKeyRef:
          name: spring-petclinic-postgres-configmap
          key: user
    - name: POSTGRES_PASSWORD
      valueFrom:
        secretKeyRef:
          name: spring-petclinic-postgres-secret
          key: password
    - name: POSTGRES_URL
      valueFrom:
        configMapKeyRef:
          name: spring-petclinic-postgres-configmap
          key: url
```

