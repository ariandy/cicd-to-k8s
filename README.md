# cicd-to-k8s

Repo ini berhubungan dengan DO-CICD-P8 dan DO-MON-P9.

## DO-CICD-P8

- Buat terlebih dahulu instance untuk Jenkins. Gunakan arsitektur Standalone yang telah disediakan di [direktori DO-CICD-P8](https://github.com/ariandy/cilsy_small_project/tree/main/DO-CICD-P8/standalone)

- Setelah melakukan image baking (menggunakan Packer) dan instance provisioning (menggunakan Terraform) untuk Jenkins, daftarkan Jenkins tersebut ke Route 53 dengan domain `jenkins.ariandy.com`

- Jalankan `chmod +x init-kubernetes.sh && ./init-kubernetes.sh`

- Install `ingress-nginx` menggunakan HELM. Adapun caranya dapat dilihat pada dokumentasi [berikut](https://kubernetes.github.io/ingress-nginx/deploy/).

- Setelah itu, kita akan mendapatkan DNS Load Balancer yang digenerate saat kita menginstall `ingress-nginx`. Masuk ke Route53, lalu buatlah A Record baru (yang mana saya menggunakan `kube.ariandy.com` sebagai nama recordnya). Lalu pilihlah `Alias` > `Alias to Application and Classic Load Balancer`. Pilih `Region` dimana Load Balancer tersebut berada. Lalu pilih Load Balancer yang dituju.

- Masuk ke direktori `kubernetes`, jalankan `helm install ariandy helm-manifest/`

- Buatlah pipeline di Jenkins dan buatlah perubahan pada codebase untuk menguji berjalannya pipeline

## DO-MON-P9

- DO-MON-P9 adalah kelanjutan dari DO-CICD-P8. Oleh karena itu, lakukan terlebih dahulu apa yang telah dilakukan di `DO-CICD-P8`. Lalu lanjutkan ke dokumentasi [berikut](https://github.com/ariandy/cilsy_small_project/tree/main/DO-MON-P9)