# ☸️ Kubernetes Darslari: Noldan Pro Darajagacha (O'zbek Tilida)

<div align="center">

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

**Ochiq manbali va to'liq amaliyotga asoslangan Kubernetes (K8s) qo'llanmasi.**  
*Har bir kun uchun batafsil ma'ruza (nazariya) va amaliy topshiriqlar (LAB).*

[Mundarija](#-kurs-mundarijasi) • [Talablar](#-talab-qilinadigan-bilim-va-dasturlar) • [LAB Muhiti](#-lab-muhitini-sozlash) • [Muallif](#-muallif)

</div>

---

## 🎯 Kurs Maqsadi

Zamonaviy **DevOps** va **Cloud-Native** dunyosida dasturlarni konteynerlarda boshqarish standartga aylandi. Ushbu darslik o'zbek tilida Kubernetes texnologiyasini fundamental asoslaridan tortib, production darajasidagi klasterlarni boshqarishgacha bo'lgan bilimlarni bosqichma-bosqich o'rgatish uchun yaratildi.

Har bir dars quyidagi formatda tashkil etilgan:
- 📖 **Ma'ruza (Nazariya):** Mavzuning ichki mexanizmlari, arxitekturasi va nima uchun kerakligi chuqur tushuntiriladi.
- 🧪 **LAB (Amaliyot):** Haqiqiy YAML manifestlar, terminal buyruqlari va real stsenariylar ustida ishlash.
- 💡 **Mustaqil topshiriqlar:** O'rganilgan mavzuni mustahkamlash uchun mashqlar.

---

## 🗺️ Kurs Mundarijasi (Roadmap)

| Kun | Mavzu | Nazariya | Amaliyot (LAB) | Holat |
| :---: | :--- | :---: | :---: | :---: |
| **Day 01** | **Kubernetes Kirish: Tarixi, Muammolar va Asosiy Konsepsiyalar** | [📖 O'qish](day-01/maruza.md) | [🧪 Boshlash](day-01/lab/README.md) | ✅ Tayyor |
| **Day 02** | **Kubernetes Arxitekturasi: Control Plane va Worker Node chuqurlashtirilgan** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 03** | **Podlar va Container Lifecycle (YAML bilan ishlash)** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 04** | **ReplicaSet va Deployments (Zero-Downtime Rolling Updates & Rollback)** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 05** | **Networking va Services: ClusterIP, NodePort, LoadBalancer** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 06** | **Ingress Controller va Marshrutlash (Routing)** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 07** | **Konfiguratsiyalar: ConfigMaps va Secrets bilan ishlash** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 08** | **Persistent Storage: Volumes, PV, PVC va StorageClass** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 09** | **Resource Management, Requests/Limits va Avtomatik Masshtablash (HPA)** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 10** | **StatefulSets, DaemonSets va Jobs/CronJobs** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 11** | **Security: RBAC (Role-Based Access Control), ServiceAccounts** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 12** | **Helm: Kubernetes Package Manager** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 13** | **Monitoring va Logging: Prometheus, Grafana va Loki** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |
| **Day 14** | **Production Klaster: CI/CD Pipeline (GitOps / ArgoCD)** | *Tez kunda* | *Tez kunda* | ⏳ Rejada |

---

## 💻 Talab qilinadigan bilim va dasturlar

### Bilimlar:
- Linux operatsion tizimi asoslari (Terminal buyruqlari)
- Docker asoslari (Images, Containers, Dockerfile)
- Tarmoq asoslari (IP, Port, DNS, HTTP/HTTPS)

### Dasturlar (O'rnatilishi lozim):
1. **Docker Desktop** yoki **Docker Engine**
2. **Kubectl** (Kubernetes buyruqlar satri CLI vositasi)
3. **Minikube** yoki **Kind** (Lokal klaster yaratish uchun)

---

## 🛠️ LAB Muhitini Sozlash

Darslarni amalda bajarish uchun o'z kompyuteringizda lokal Kubernetes klasterini ishga tushirishingiz kifoya:

```bash
# 1. Minikube orqali klaster ishga tushirish (Docker drayveri bilan)
minikube start --driver=docker

# 2. Klaster holatini tekshirish
kubectl cluster-info
kubectl get nodes
```

> Batafsil sozlash yo'riqnomasi [Day 01 LAB](day-01/lab/README.md) qo'llanmasida yoritilgan.

---

## 📂 Loyiha Tuzilishi

```text
kubernetes/
├── README.md                  # Asosiy kurs mundarijasi va yo'riqnoma
├── .gitignore                 # Git e'tiborsiz qoldiradigan fayllar
└── day-01/                    # 1-kun darslari
    ├── README.md              # Day 01 qisqacha tavsifi va rejasi
    ├── maruza.md              # Day 01: To'liq nazariya va ma'ruza
    └── lab/                   # Day 01: Amaliy topshiriqlar
        ├── README.md          # Bosqichma-bosqich LAB yo'riqnomasi
        └── manifests/         # Amaliyot uchun Kubernetes YAML fayllari
            ├── 01-first-pod.yaml
            ├── 02-first-deployment.yaml
            └── 03-first-service.yaml
```

---

## 🤝 Hissa Qo'shish (Contributing)

Ushbu kurs barcha uchun ochiq! Agar darslarda xatolik topsangiz yoki yangi ma'lumotlar qo'shmoqchi bo'lsangiz:
1. Loyihani **Fork** qiling
2. Yangi tarmoq (branch) oching: `git checkout -b feature/yangi-dars`
3. O'zgarishlarni commit qiling: `git commit -m 'Yangi ma'lumot qo'shildi'`
4. Branchga push qiling: `git push origin feature/yangi-dars`
5. **Pull Request (PR)** yuboring!

---

## 👨‍💻 Muallif

- **GitHub:** [@sarvartech](https://github.com/sarvartech)
- **Repo:** [github.com/sarvartech/kubernetes](https://github.com/sarvartech/kubernetes)

Loyihani kuzatib borish uchun repository-ga **Star ⭐️** bosishni unutmang!
