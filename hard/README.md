### 概要	
貴方がここを読んでいるということは[課題](./easy)を終えられたのでしょう...   	
つまり、私が貴方に私が用意しなくてはいけない課題がもう少し難しいものということになります。  
課題の解法を得るには[Resources](./Resources.md)を読めば基本的には答えのようなものが書いてあります。


### CI/CDの導入
[ArgoCD](https://argoproj.github.io/argo-cd/)を導入して先ほどまで上げた様々なコンテンツをCDに載せましょう。つまり、何らかの形でGithub などが必要です。

### 可視化
PrometheusとGrafanaでシュッと可視化してみよう、あと、dashboardを追加しましょう。

### WorkerNodeの追加
とりあえず、kubeadmでKubernetes基盤を構築しているのでWorkerNodeの追加も簡単です。頑張れ!!!!
終わったらMasterで動作しておかねばならないコンテナ以外は全部そっちに移動させてみよう

### ServiceMesh 導入
とりあえず、Envoy導入しよっか

### Golangによるクライアントの開発(Docker)
[docker: github.com/docker/docker/client](https://godoc.org/github.com/docker/docker/client)を参考に一つ機能を開発していきましょう。
### Golangによるクライアントの開発(Kubernetes)
[kubernetes/client-go](https://github.com/kubernetes/client-go)を参考に一つ機能を開発していきましょう。
### Kubernetes Operator 
[Kubernetes Operator](https://github.com/operator-framework/operator-sdk)を使って任意のオペレーションをやりましょう。
### CRDの作成
[Kubebuilder](https://github.com/kubernetes-sigs/kubebuilder)を使ってCRDの開発をしましょう。


