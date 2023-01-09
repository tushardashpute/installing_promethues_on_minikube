# installing_promethues_on_minikube

**Prerequisite**

- Minikube cluster
- Helm 

Steps:
=====
Install Prometheus
Access Prometheus UI
Install Grafana
Get Grafana admin credentials
Access Grafana Web UI

Install Prometheus

1. Add prometheus repo

        helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

2. Install prometheus comminity chart

        helm install prometheus prometheus-community/prometheus

3. Expose prometheus-server service via Nodeport
    
        kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-np

4. Check Services:

            kubectl get svc

            [root@ip-172-31-82-229 opt]# kubectl get pods
            NAME                                                 READY   STATUS    RESTARTS   AGE
            prometheus-alertmanager-0                            1/1     Running   0          2m55s
            prometheus-kube-state-metrics-7c668ff876-sgjkd       1/1     Running   0          2m56s
            prometheus-prometheus-node-exporter-c2zgh            1/1     Running   0          2m56s
            prometheus-prometheus-pushgateway-76dd88bb6b-bk24g   1/1     Running   0          2m56s
            prometheus-server-6cfc854b6f-zt9tb                   2/2     Running   0          2m56s
            [root@ip-172-31-82-229 opt]# kubectl get pv
            NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                       STORAGECLASS   REASON   AGE
            pvc-48badce7-44aa-43f7-b516-1c0976f2a96c   8Gi        RWO            Delete           Bound    default/prometheus-server                   standard                3m6s
            pvc-5965ca3c-83ba-4b7f-bef9-44c4d7fd12c8   2Gi        RWO            Delete           Bound    default/storage-prometheus-alertmanager-0   standard                3m5s
            [root@ip-172-31-82-229 opt]# kubectl get pvc
            NAME                                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
            prometheus-server                   Bound    pvc-48badce7-44aa-43f7-b516-1c0976f2a96c   8Gi        RWO            standard       3m10s
            storage-prometheus-alertmanager-0   Bound    pvc-5965ca3c-83ba-4b7f-bef9-44c4d7fd12c8   2Gi        RWO            standard       3m9s
            [root@ip-172-31-82-229 opt]# kubectl get svc
            NAME                                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
            kubernetes                            ClusterIP   10.96.0.1        <none>        443/TCP        14m
            prometheus-alertmanager               ClusterIP   10.103.8.18      <none>        9093/TCP       30s
            prometheus-alertmanager-headless      ClusterIP   None             <none>        9093/TCP       30s
            prometheus-kube-state-metrics         ClusterIP   10.101.171.151   <none>        8080/TCP       30s
            prometheus-prometheus-node-exporter   ClusterIP   10.100.2.116     <none>        9100/TCP       30s
            prometheus-prometheus-pushgateway     ClusterIP   10.96.87.50      <none>        9091/TCP       30s
            prometheus-server                     ClusterIP   10.106.127.25    <none>        80/TCP         30s
            prometheus-server-np                  NodePort    10.109.77.6      <none>        80:30021/TCP   13s

Access Prometheus UI
  
