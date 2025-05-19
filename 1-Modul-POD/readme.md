# POD
Dalam Kubernetes, Pod adalah unit terkecil dan paling dasar yang dapat dikelola. Pod merepresentasikan satu atau lebih container (biasanya Docker container) yang:

- Berjalan bersama dalam satu node
- Berbagi network (IP address dan port)
- Berbagi storage (jika ada volume yang dimount)

## Penjelasan Lebih Lanjut:
- Satu Pod bisa berisi satu container (kasus paling umum), atau beberapa container yang saling tergantung dan perlu berjalan bersama (contohnya, satu container sebagai aplikasi utama dan satu sebagai sidecar logging agent).
- Container dalam satu Pod berkomunikasi satu sama lain lewat localhost, karena mereka berbagi IP dan namespace.
- Kubernetes tidak menjalankan container langsung, melainkan menjalankan Pod yang berisi container.

## Contoh Kasus:
Misalnya kamu punya aplikasi web dan logging agent:
```shell
apiVersion: v1
kind: Pod
metadata:
  name: my-web-app
spec:
  containers:
    - name: web
      image: nginx
    - name: log-agent
      image: busybox
      command: ["sh", "-c", "tail -f /var/log/nginx/access.log"]
```
Kedua container di atas akan berjalan bersama dalam satu Pod.

## Kesimpulan:
- Pod = 1 atau lebih container + shared environment
- Dikelola oleh Kubernetes (biasanya melalui Deployment, StatefulSet, dll.)
- Merupakan abstraksi dari aplikasi yang berjalan di dalam cluster

## Daftar perintah kubectl paling umum untuk mengelola Pod di Kubernetes:
### 🔍 Melihat Pod
| Perintah                                      | Fungsi                                   |
| --------------------------------------------- | ---------------------------------------- |
| `kubectl get pods`                            | Melihat semua Pod di namespace aktif     |
| `kubectl get pods -n <nama-namespace>`        | Melihat Pod di namespace tertentu        |
| `kubectl describe pod <nama-pod>`             | Melihat detail lengkap Pod               |
| `kubectl logs <nama-pod>`                     | Melihat log dari Pod                     |
| `kubectl logs <nama-pod> -c <nama-container>` | Melihat log container tertentu dalam Pod |

### 🚀 Membuat & Menghapus Pod
| Perintah                        | Fungsi                                 |
| ------------------------------- | -------------------------------------- |
| `kubectl apply -f <file.yaml>`  | Membuat atau update Pod dari file YAML |
| `kubectl create -f <file.yaml>` | Membuat Pod dari file YAML             |
| `kubectl delete pod <nama-pod>` | Menghapus Pod tertentu                 |
| `kubectl delete -f <file.yaml>` | Menghapus Pod berdasarkan file YAML    |

## 🔧 Sintaks Port Forwarding Pod:
```shell
kubectl port-forward pod/<nama-pod> <port-lokal>:<port-pod>
```
Contoh
```shell
kubectl port-forward pod/nginx 8080:80
```

### 🛠️ Debugging & Interaksi
| Perintah                                                      | Fungsi                                   |
| ------------------------------------------------------------- | ---------------------------------------- |
| `kubectl exec -it <nama-pod> -- bash`                         | Masuk ke dalam Pod (jika ada `bash`)     |
| `kubectl exec -it <nama-pod> -- sh`                           | Masuk ke dalam Pod (jika hanya ada `sh`) |
| `kubectl port-forward pod/<nama-pod> <local-port>:<pod-port>` | Akses Pod dari lokal                     |

### 🧪 Contoh Perintah Praktis
```shell
kubectl get pods
kubectl describe pod my-pod
kubectl logs my-pod
kubectl exec -it my-pod -- sh
kubectl delete pod my-pod
kubectl apply -f my-pod.yaml
```





























