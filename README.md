# istio-helm-circuit-breaker-test

minikube での Istio の Circuit Breaker (ingress 経由) の個人確認用

## 事前準備

- minikube のインストール
- kubectl のインストール
- helm のインストール
- helmfile のインストール

```bash
minikube start
minikube addons enable ingress
kubectl label namespace default istio-injection=enabled
```

## 手順

1. istio-base, istiod, istio-ingressgateway のデプロイ

```bash
cd helm
helmfile apply
```

2. サンプルアプリケーションのデプロイ

リポジトリのルートディレクトリに移動して以下のコマンドを実行

```bash
kubectl apply -f ./
```

3. minikube tunnel の起動

```bash
minikube tunnel
```

4. /etc/hosts に追加

```bash
127.0.0.1 my-app.local
```

5. ブラウザでアクセス

- http://my-app.local/
- http://my-app.local/status/200 で 200 が返ってくることを確認
- http://my-app.local/status/500 で 500 が返ってくることを確認
- http://my-app.local/status/200 で 503 が 3 分間返ってくることを確認
- http://my-app.local/status/200 で 200 が 3 分後に返ってくることを確認
