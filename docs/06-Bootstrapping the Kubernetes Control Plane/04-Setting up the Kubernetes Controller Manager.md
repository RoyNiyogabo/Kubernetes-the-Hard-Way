## Setting up the Kubernetes Controller Manager.

Now that we have set up kube-apiserver, we are ready to configure kube-controller-manager. This lesson walks you through the process of configuring a systemd service for the Kubernetes Controller Manager. After completing this lesson, you should have the kubeconfig and systemd unit file set up and ready to run the kube-controller-manager service on both of your control nodes.

You can configure the Kubernetes Controller Manager like so:
```
sudo cp kube-controller-manager.kubeconfig /var/lib/kubernetes/
```

Generate the kube-controller-manager systemd unit file:
```
cat << EOF | sudo tee /etc/systemd/system/kube-controller-manager.service
[Unit]
Description=Kubernetes Controller Manager
Documentation=https://github.com/kubernetes/kubernetes

[Service]
ExecStart=/usr/local/bin/kube-controller-manager \\
  --address=0.0.0.0 \\
  --cluster-cidr=10.200.0.0/16 \\
  --cluster-name=kubernetes \\
  --cluster-signing-cert-file=/var/lib/kubernetes/ca.pem \\
  --cluster-signing-key-file=/var/lib/kubernetes/ca-key.pem \\
  --kubeconfig=/var/lib/kubernetes/kube-controller-manager.kubeconfig \\
  --leader-elect=true \\
  --root-ca-file=/var/lib/kubernetes/ca.pem \\
  --service-account-private-key-file=/var/lib/kubernetes/service-account-key.pem \\
  --service-cluster-ip-range=10.32.0.0/24 \\
  --use-service-account-credentials=true \\
  --v=2
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF
```
