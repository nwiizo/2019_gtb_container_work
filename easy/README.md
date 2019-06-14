# ハンズオン
講義はどうでしたか？ここが良かった。悪かったなどがあるなら積極的に[nwiizo](https://twitter.com/nwiizo)にメンション飛ばしてください。

### 1. ConoHaの登録と VMの準備
  - アカウントの登録
  - 推奨OS Ubuntu 18.04 LTS (検証これでやった)
  - 推奨VMサイズ 4GB以上 ()
  
### 2. OSの基本設定とKubernetes

### 2.1 Ansibleでインストール
パッケージのダウンロード
```
# apt-get update
# apt-get install software-properties-common
# apt-add-repository --yes --update ppa:ansible/ansible
# apt-get install ansible
```
今回はk8sやDockerに触れるファーストなので全部サクッとAnsibleで構築してしまいます  
 - 詳しくやりたい場合は [kubernetes the hard way](https://github.com/kelseyhightower/kubernetes-the-hard-way)をやってください．
```
# git clone https://github.com/nwiizo/minicamp-fukuoka2019
```
待てばなおりますがapt がどうしても終わらない人 
```
# ps aux |grep apt
# kill -9 <PID>
```

### 2.1.1  Ansibleの実行
Ansibleの実行を行います.ここで詰まったら一緒に泣きながらデバッグしましょう  
[Document](https://docs.ansible.com/#coreversionselect)  
`vim /etc/ansible/ansible.cfg`
```
[defaults]
host_key_checking = False
```
`vim hosts.yml`
```
[kubernetes:children]
Kubernetes

[Kubernetes]
<ex-ip>
```
コマンドの実行
```
# ansible-playbook -i ./hosts.yml ./site.yml -l "<ex-IP>" -k
```

### 

### 3.確認
インフラを自動で管理する方法はいくつかあって有名なものでいうと[serverspec](https://github.com/mizzy/serverspec) や[Goss](https://github.com/aelsabbahy/goss)などのツールがあります．

- Dockerの確認
```
# docker -v
```
- Kubernetesの確認
```
# su - k8sops
# bashrcにでも入れとけばいい
# export KUBECONFIG=/home/k8sops/.kube/config
# cluster の確認
# kubectl version
Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.4", GitCommit:"c27b913fddd1a6c480c229191a087698aa92f0b1", GitTreeState:"clean", BuildDate:"2019-02-28T13:37:52Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.4", GitCommit:"c27b913fddd1a6c480c229191a087698aa92f0b1", GitTreeState:"clean", BuildDate:"2019-02-28T13:30:26Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
```
クラスターの詳細情報
```
# kubectl cluster-info
Kubernetes master is running at https://*.*.*.*:6443
KubeDNS is running at https://*.*.*.*:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```
nodeの確認
```
# kubectl get nodes
NAME             STATUS   ROLES    AGE   VERSION
*-*-*-*          Ready    master   16m   v1.13.4
```
Namespaceの確認
```
# kubectl get namespaces
NAME          STATUS   AGE
default       Active   22m
kube-public   Active   22m
kube-system   Active   22m
```

Podの確認
```
# kubectl get pods --all-namespaces
NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
kube-system   coredns-86c58d9df4-d45kz                 1/1     Running   0          26m
kube-system   coredns-86c58d9df4-xt7lk                 1/1     Running   0          26m
kube-system   etcd-150-95-147-209                      1/1     Running   0          25m
kube-system   kube-apiserver-150-95-147-209            1/1     Running   0          25m
kube-system   kube-controller-manager-150-95-147-209   1/1     Running   0          25m
kube-system   kube-proxy-5k79s                         1/1     Running   0          26m
kube-system   kube-scheduler-150-95-147-209            1/1     Running   0          25m
kube-system   tiller-deploy-5b7c66d59c-p5gdc           1/1     Running   0          26m
kube-system   weave-net-rcpkr                          2/2     Running   0          26m
```

### 4. Docker の実行
ないなら良いが`# docker login`をやっとくと今後は楽．こちらの演習はrootユーザーで実行してください．
- とりあえず，Hello world
```
# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
```
確認しました．
```
# docker images | grep hello
hello-world                          latest              fce289e99eb9        8 weeks ago         1.84kB
```
- Nginxの公開
```
# docker pull docker.io/nginx
# docker run -d -p 8000:80 --name nginx-latest docker.io/nginx:latest
# wget 127.0.0.1:8000
```
ブラウザで確認後 Docker の削除
```
# docker ps | grep nginx
9fd4701f4953        nginx:latest                    "nginx -g 'daemon of…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp   nginx-latest
# docker stop 9fd4701f4953
9fd4701f4953
# docker ps | grep nginx
```
- DockerFileでのデプロイ
```
# Gist上のコピペしてください.DockerFileを書いて
# vim Dockerfile
# build して
# docker build . -t gtb_example01
# docker images | grep gtb
gtb_example01
latest              ecb3f44aea9f        2 minutes ago       109MB 
# 走らせませす．
# docker run -d -p 8080:80 --name gtb_example01 gtb_example01:latest

ブラウザで確認してください アクセスできませんでしかた？残念… 
`Docker -p` とかでoption について調べてみましょう‼
終わったらちゃんと削除してくださいね
```
# docker ps | grep gtb
# docker kill <CONTAINER ID>
```
※チャレンジ課題：同様にDockerFileからApacheをビルドしてみてください．
### 5. Kubernetes の実行
ないなら良いが`# docker login`をやっとくと今後は楽．k8sopsユーザーで実行してください
```
vim demo.yaml
# kubectl apply -f demo.yaml
# kubectl get pods --selector app=demo
# kubectl get service
NAME         TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
demo         LoadBalancer   10.108.16.22   <pending>     8080:30080/TCP   3m41s
kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP          108m
```

削除します
```
# kubectl delete -f demo.yaml
deployment.extensions "demo" deleted
service "demo" deleted
```

※チャレンジ課題：同様にApacheで動作するPodを動作させてください。
### 6.課題
[main.go](./main.go)をコンテナ化及びKubernetesにて外部へ公開してみてください。

### さいごに
一旦、ここまで終了できた貴方を褒めたいと思います。[hard](../hard)に進むかと用意した環境で下記のコンテンツで遊ぶか話してる無駄話を聞いてもらってもどっちでもかまいません．
- https://docs.docker.com/get-started/
- https://kubernetes.io/docs/home/
