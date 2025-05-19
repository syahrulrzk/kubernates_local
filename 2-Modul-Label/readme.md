# Label
Label pada Pod (dan resource lain di Kubernetes) adalah pasangan kunci-nilai (key-value pair) yang digunakan untuk mengidentifikasi, mengelompokkan, dan memilih resource.

## 🏷️ Fungsi Label:
1. Seleksi Resource
Contoh: pilih semua Pod yang label-nya app=nginx

2. Mengelompokkan Resource
Contoh: tandai semua Pod yang termasuk ke dalam deployment tertentu

3. Digunakan oleh Service, Deployment, dsb
Service akan memilih Pod berdasarkan label-nya agar tahu ke mana request harus diarahkan.

## 📌 Contoh Penulisan Label di Pod YAML:

```shell
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
    env: production
spec:
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80

```
## ✅ Cara Melihat Label:
```shell
kubectl get pods --show-labels
```

## 🔍 Cara Filter Pod Berdasarkan Label:
```shell
kubectl get pods -l app=nginx
```

## 🧩 Tambah / Edit Label (Manual):

```shell
kubectl label pod nginx env=staging
```
Hapus label dari Pod:

```shell
kubectl label pod nginx env-
```

## 💡 Ringkasnya:

Label = cara cerdas Kubernetes untuk tahu "mana yang mana" di antara resource-resource yang mirip.