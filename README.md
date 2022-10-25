# Kubernetes Project operator with ansible

##### Создание шаблона оператора

```shell
$ operator-sdk init \
    --plugins=ansible.sdk.operatorframework.io/v1 \
    --domain k8s.romanow \
    --generate-role \
    --generate-playbook

$ operator-sdk create api \
    --group ops \
    --version v1alpha1 \
    --kind Project \
    --generate-role   
```

##### Custom Resource Definition

[ops.k8s.romanow_projects.yaml](config/crd/bases/ops.k8s.romanow_projects.yaml)

##### Sample

[ops_v1alpha1_project.yaml](config/samples/ops_v1alpha1_project.yaml)

### Тестирование

```shell
$ molecule converge
```

### Тестирование на локальном кластере

```shell
$ kind create cluster --config kind.yml

$ make docker-build IMG=romanowalex/project-operator:v1.0

$ make docker-push IMG=romanowalex/project-operator:v1.0 

$ make deploy IMG=romanowalex/project-operator:v1.0

$ kubectl get namespace
NAME                      STATUS   AGE
default                   Active   20m
education-prod            Active   10m
education-stage           Active   10m
project-operator-system   Active   18m

$ kubectl describe namespace education-prod                                                                    master 
Name:         education-prod
Labels:       app.kubernetes.io/managed-by=project-operator
              kubernetes.io/metadata.name=education-prod
Annotations:  <none>
Status:       Active

Resource Quotas
  Name:            resource-quota
  Resource         Used  Hard
  --------         ---   ---
  limits.cpu       0     1
  limits.memory    0     1Gi
  requests.cpu     0     1
  requests.memory  0     1Gi

No LimitRange resource.
```

## Ссылки

1. [Собственный Kubernetes оператор за час (видео)](https://www.youtube.com/watch?v=tFzM-2pwL8A)
2. [Собственный Kubernetes оператор за час (текст)](https://habr.com/ru/company/southbridge/blog/658451/)