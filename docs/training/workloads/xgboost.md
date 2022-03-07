---
sidebar_position: 5
---
# XGBoost

## Example
```yaml
apiVersion: training.kubedl.io/v1alpha1
kind: "XGBoostJob"
metadata:
  name: "xgboost-dist-iris-test-train"
spec:
  xgbReplicaSpecs:
    Master:
      replicas: 1
      restartPolicy: Never
      template:
        apiVersion: v1
        kind: Pod
        spec:
          containers:
            - name: xgboostjob
              image: docker.io/merlintang/xgboost-dist-iris:1.1
              ports:
                - containerPort: 9991
                  name: xgboostjob-port
              imagePullPolicy: Always
              args:
                - --job_type=Train
                - --xgboost_parameter=objective:multi:softprob,num_class:3
                - --n_estimators=10
                - --learning_rate=0.1
                - --model_path=autoAI/xgb-opt/2
                - --model_storage_type=oss
                - --oss_param=unknown
    Worker:
      replicas: 2
      restartPolicy: ExitCode
      template:
        apiVersion: v1
        kind: Pod
        spec:
          containers:
            - name: xgboostjob
              image: docker.io/merlintang/xgboost-dist-iris:1.1
              ports:
                - containerPort: 9991
                  name: xgboostjob-port
              imagePullPolicy: Always
              args:
                - --job_type=Train
                - --xgboost_parameter="objective:multi:softprob,num_class:3"
                - --n_estimators=10
                - --learning_rate=0.1
```
## CRD Spec
Check the CRD definition. [Go ->](https://github.com/alibaba/kubedl/blob/master/apis/training/v1alpha1/xgboostjob_types.go)
