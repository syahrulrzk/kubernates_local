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
