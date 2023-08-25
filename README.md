文件 EFK 7.10.2
全部创建即可




修改Fluentd的部署文件
由于在Kubernetes集群中，可能不需要对所有的机器都采集日志，因此可以更改Fluentd的部署文件。添加一个NodeSelector，只部署至需要采集日志的主机即可。

grep "nodeSelector" fluentd-es-ds.yaml -A 3
      nodeSelector: #标签选择器
        fluentd: "true" #选择fluentd=true 的节点node =true就采集日志
      volumes:
      - name: varlog
------------------------------------------------------------------------------------------------------------------------
为需要采集日志的服务器设置标签
kubectl label node k8s-node01 fluentd=true
node/k8s-node01 labeled

kubectl label node k8s-node02 fluentd=true
node/k8s-node02 labeled

kubectl label node k8s-master fluentd=true

kubectl get node -l fluentd=true --show-labels

------------------------------------------------------------------------------------------------------------------------
kubectl get svc -n logging
访问kibana
使用任意一个部署了kube-proxy服务的节点的IP+32734端口即可访问kibana。
http://192.168.10.101:32734/kibana
