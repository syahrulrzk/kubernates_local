#  Kubernetes lokal menggunakan Minikube + Docker
Kubernetes (sering disingkat: K8s) adalah platform open-source yang digunakan untuk mengelola container secara otomatis — termasuk deployment, scaling, dan orchestration (pengaturan).

![Minikube Logo](/image/kubernates_wp.png "Logo Minikube")

## 🧠 Konsep sederhana
Bayangkan kamu punya banyak aplikasi dalam bentuk container (misalnya dari Docker), lalu kamu ingin:

- Menjalankan banyak container di banyak server.
- Pastikan aplikasi tetap jalan walaupun ada error/crash.
- Secara otomatis scaling (nambah/ngurangin container sesuai kebutuhan).
- Update aplikasi tanpa downtime.
- Mengatur komunikasi antar container dengan aman.

Nah, Kubernetes adalah "otak pusat" yang mengatur semua itu secara otomatis.





## 🧩 Apa itu Minikube?
Minikube adalah alat ringan untuk menjalankan Kubernetes secara lokal.
- Cocok untuk belajar, uji coba, atau pengembangan lokal.
- Minikube membuat cluster Kubernetes satu node (control-plane dan worker di satu mesin).
- Bisa menggunakan berbagai driver: Docker, VirtualBox, KVM, dll.

## 🐳 Apa itu Docker?
Docker adalah platform containerisasi yang:
- Mengemas aplikasi dalam bentuk container.
- Menyediakan runtime (containerd) untuk menjalankan container.
- Memungkinkan image manajemen, volume, network, dan image registry.

## 🔗 Minikube + Docker: Bagaimana Kerjanya Bersama?
Minikube membutuhkan mesin (VM atau container) untuk menjalankan Kubernetes. Saat kamu pakai:

<pre>minikube start --driver=docker</pre>

Berarti:

- Minikube membuat 1 container Docker bernama minikube.
- Di dalam container itu, terdapat:
- Komponen Kubernetes: kubelet, apiserver, scheduler, controller-manager, dll.
- Komponen container runtime: Docker/Containerd.
- Semua workload Kubernetes (Pod, Service, dsb.) dijalankan dalam container Docker Minikube ini.

📦 Kamu menjalankan Kubernetes dalam sebuah Docker container di lokalmu.

## 🧠 Mengapa Gunakan Docker Sebagai Driver Minikube?
- Lebih ringan & cepat dibanding VM.
- Tidak perlu install VirtualBox/KVM.
- Mudah dihapus/dibuat ulang.
- Bisa share image langsung:
  - Image yang kamu build di Docker lokal bisa langsung digunakan di cluster Minikube tanpa push ke registry.

## 🔄 Alur Minikube (dengan Docker):
1. `minikube start --driver=docker`
2. Minikube membuat container (misal `minikube`)
3. Kubernetes berjalan dalam container itu
4. Kamu bisa:
   - Deploy Pod lewat `kubectl`
   - Lihat container pakai `docker ps`
   - Gunakan `docker image` untuk build image dan langsung pakai di Kubernetes
  
Contoh: Deploy Aplikasi Lokal
<pre>
docker build -t myapp:v1 .
minikube image load myapp:v1
kubectl run mypod --image=myapp:v1
</pre>












##  install Kubernetes lokal menggunakan Minikube + Docker di Ubuntu 24.04
## 1. Update sistem
<pre>sudo apt update && sudo apt upgrade -y </pre>

## 2. Install dependencies
<pre>sudo apt install -y curl wget apt-transport-https ca-certificates gnupg </pre>

## 3. Install kubectl
<pre>curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/ </pre>

## 4. Install Minikube
<pre>
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
</pre>

### Cek versi:
<pre>minikube version</pre>

## 5. Install Virtualisasi (Docker)
❗ Opsi: Gunakan Docker sebagai driver (direkomendasikan)
- Pastikan Docker sudah terpasang:
<pre>
sudo apt install docker.io -y
sudo usermod -aG docker $USER
newgrp docker
</pre>

## 6. Mulai Minikube
<pre>
minikube start --driver=docker
</pre>
contoh berhasil
<pre>
╭────[ reborn@linux ] [~] 
╰────[ ~ minikube start --driver=docker
😄  minikube v1.35.0 on Ubuntu 24.04
✨  Using the docker driver based on existing profile
👍  Starting "minikube" primary control-plane node in "minikube" cluster
🚜  Pulling base image v0.0.46 ...
🏃  Updating the running docker "minikube" container ...

🧯  Docker is nearly out of disk space, which may cause deployments to fail! (87% of capacity). You can pass '--force' to skip this check.
💡  Suggestion: 

    Try one or more of the following to free up space on the device:
    
    1. Run "docker system prune" to remove unused Docker data (optionally with "-a")
    2. Increase the storage allocated to Docker for Desktop by clicking on:
    Docker icon > Preferences > Resources > Disk Image Size
    3. Run "minikube ssh -- docker system prune" if using the Docker container runtime
🍿  Related issue: https://github.com/kubernetes/minikube/issues/9024

🐳  Preparing Kubernetes v1.32.0 on Docker 27.4.1 ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
╭────[ reborn@linux ] [~] 
╰────[ ~ 
</pre>

## 7. Cek status Minikube
<pre>
minikube status
</pre>
Output :
<pre>
╭────[ reborn@linux ] [~] 
╰────[ ~ minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured
╭────[ reborn@linux ] [~] 
╰────[ ~ 
</pre>

## 8. Gunakan kubectl
<pre>
kubectl get nodes
</pre>
Output :
<pre>
╭────[ reborn@linux ] [~] 
╰────[ ~ kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   40m   v1.32.0
╭────[ reborn@linux ] [~] 
╰────[ ~ 
</pre>

## 🔧 Tips Tambahan
## 💡 Perintah Dasar Minikube

```bash
minikube start       # Memulai kluster Kubernetes lokal
minikube stop        # Menghentikan kluster tanpa menghapus data
minikube delete      # Menghapus kluster sepenuhnya
minikube status      # Memeriksa status kluster saat ini
