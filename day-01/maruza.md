# 📖 Day 01: Ma'ruza — Kubernetes Tarixi, Yechilgan Muammolar va Asosiy Konsepsiyalar

---

## 📑 Mundarija

1. [Dasturlarni Serverga Joylash (Deployment) Evolyutsiyasi](#1-dasturlarni-serverga-joylash-deployment-evolyutsiyasi)
2. [Docker Inqilobi va Nega Uning O'zi Yetarli Bo'lmay Qoldi?](#2-docker-inqilobi-va-nega-uning-ozi-yetarli-bolmay-qoldi)
3. [Konteyner Orkestratsiyasi (Container Orchestration) Nima?](#3-konteyner-orkestratsiyasi-container-orchestration-nima)
4. [Kubernetes Nima? Nomi va Ma'nosi](#4-kubernetes-nima-nomi-va-manosi)
5. [Kubernetesning Yaratilish Tarixi va Xronologiyasi](#5-kubernetesning-yaratilish-tarixi-va-xronologiyasi)
6. [Kubernetes Qanday Asosiy Muammolarni Hal Qiladi?](#6-kubernetes-qanday-asosiy-muammolarni-hal-qiladi)
7. [Kubernetes Arxitekturasi Asoslari (High-Level Overview)](#7-kubernetes-arxitekturasi-asoslari-high-level-overview)
8. [Asosiy Konsepsiyalar va Ob'yektlar (Basic Concepts)](#8-asosiy-konsepsiyalar-va-obyektlar-basic-concepts)
9. [Imperativ va Deklarativ Yondashuv](#9-imperativ-va-deklarativ-yondashuv)
10. [Kubernetes Qachon Kerak va Qachon Ortiqcha?](#10-kubernetes-qachon-kerak-va-qachon-ortiqcha)
11. [O'z-o'zini Sinash Uchun Nazorat Savollari](#11-oz-ozini-sinash-uchun-nazorat-savollari)

---

## 1. Dasturlarni Serverga Joylash (Deployment) Evolyutsiyasi

Kubernetes nima uchun kerakligini to'liq anglash uchun, dasturlarni serverlarda yurgizish tarixiga nazar solishimiz kerak:

```text
┌────────────────────────┐      ┌────────────────────────┐      ┌────────────────────────┐
│ Traditional Deployment │  ──> │ Virtualized Deployment │  ──> │  Container Deployment  │
│  (Jismoniy Serverlar)  │      │ (Virtual Mashinalar)   │      │ (Docker, Konteynerlar) │
└────────────────────────┘      └────────────────────────┘      └────────────────────────┘
```

### 1. Traditional Era (An'anaviy davr — Jismoniy Serverlar)
* Dasturlar to'g'ridan-to'g'ri bitta jismoniy server (Bare Metal) ustidagi operatsion tizimda ishga tushirilgan.
* **Muammo:** Resurslarni chegaralab bo'lmas edi. Agar bitta dastur xotirani to'ldirib yuborsa (Memory Leak), boshqa barcha dasturlar ham to'xtab qolardi. Shuningdek, turli dasturlarning kutubxonalari to'qnashardi (Dependency Hell). Bitta jismoniy serverda faqat bitta dastur yurgizish esa juda qimmatga tushardi.

### 2. Virtualized Era (Virtualizatsiya davri — Virtual Machines)
* Hypervisor (VMware, VirtualBox, KVM) texnologiyasi paydo bo'ldi. Bitta jismoniy server ustida bir nechta mustaqil Virtual Mashinalar (VM) ochish imkoni tug'ildi.
* Har bir VM o'zining to'liq Operatsion Tizimi (Guest OS), xotirasi va drayverlariga ega bo'ldi.
* **Muammo:** Har bir VM uchun alohida OS talab qilinishi sababli ular juda og'ir (gigabaytlab hajm) va sekin yuklanardi (bir necha daqiqa).

### 3. Container Era (Konteynerlar davri — Docker)
* Konteynerlar VM ga o'xshash izolyatsiyani taqdim etadi, ammo **Host OS yadrosini (Linux Kernel)** umumiy ishlatadi.
* **Afzalligi:** Konteynerlar bir necha megabayt hajmga ega, soniyalar ichida ishga tushadi, barcha bog'liqliklari (dependencies) bilan birga paketlanadi va istalgan joyda bir xil ishlaydi ("Write once, run anywhere").

---

## 2. Docker Inqilobi va Nega Uning O'zi Yetarli Bo'lmay Qoldi?

2013-yilda Docker dunyoga keldi va dasturiy ta'minotni ishlab chiqish (Development) jarayonini butunlay o'zgartirdi. Ishlab chiquvchi o'z kompyuterida yaratgan konteyner production serverida ham xuddi shunday ishlardi.

Biroq, tizim kattalashib, mikroxizmatlar (microservices) arxitekturasiga o'tilganda, ishlab chiqarish (production) muhitida Dockerning o'zi yetarli bo'lmay qoldi.

### Docker duch kelgan muammolar:
- **Konteyner to'xtab qolsa (Crash bo'lsa):** Uni kim avtomatik qayta ishga tushiradi?
- **Server (Node) yonib ketsa yoki ishdan chiqsa:** Uning ichidagi 50 ta konteynerni boshqa sog'lom serverga kim ko'chiradi?
- **Trafik keskin oshib ketsa:** Yangi konteyner nusxalarini kim avtomatik ko'paytiradi (Auto-scale)? Trafik kamayganda kim qisqartiradi?
- **Yuklamani taqsimlash (Load Balancing):** Foydalanuvchilar so'rovlarini 10 ta konteyner nusxasiga kim teng taqsimlab beradi?
- **Yangi versiyani deploy qilish (Rollout):** Foydalanuvchilar uchun dastur 1 soniya ham to'xtamasdan (Zero Downtime) qanday yangilanadi? Yangi versiyada xatolik chiqsa, eski versiyaga qanday darhol qaytariladi (Rollback)?
- **Tarmoq va Xavfsizlik:** 100 lab konteynerlar o'zaro qanday xavfsiz gaplashadi? Maxfiy parollar (Secrets) qanday saqlanadi?

> 📌 **Xulosa:** Docker — bu konteynerlarni yaratish va bitta hostda yurgizish uchun ajoyib vosita. Ammo yuzlab serverlar va minglab konteynerlardan iborat tizimni boshqarish uchun **Konteyner Orkestratori (Container Orchestrator)** zarur bo'ldi.

---

## 3. Konteyner Orkestratsiyasi (Container Orchestration) Nima?

Tasavvur qiling, har bir konteyner — bu simfonik orkestrdagi bitta musiqachi (skripkachi, barabanchi, trubachi).
Har bir musiqachi o'z asbobini chalishni biladi (xuddi Docker konteyner o'z dasturini yurgizgani kabi). Ammo orkestrda birgalikda mukammal simfoniya yangrashi uchun **Dirijyor** kerak.

**Konteyner Orkestratsiyasi** — bu bir nechta serverlar klasterida konteynerlarning joylashuvi, tarmoq ulanishi, masshtablanishi, xavfsizligi va hayot siklini avtomatik tarzda boshqaruvchi tizimdir.

Va bugungi kunda dunyodagi eng yetakchi, de-fakto standartga aylangan dirijyor — bu **Kubernetes**!

---

## 4. Kubernetes Nima? Nomi va Ma'nosi

**Kubernetes** (ko'pincha **K8s** deb yoziladi) — bu konteynerlashgan ilovalarni avtomatlashtirilgan tarzda deploy qilish, masshtablash va boshqarish uchun mo'ljallangan ochiq kodli (Open Source) platformadir.

### Nomi qayerdan kelgan?
* **Etimologiyasi:** *Kubernetes* (grekcha: κυβερνήτης) yunoncha so'z bo'lib, **"Kema boshqaruvchisi", "Rulchi" (Helmsman)** yoki **"Uchuvchi" (Pilot)** degan ma'nolarni anglatadi. "Kiber" (Cybernetics) so'zi ham aynan shu o'zakdan kelib chiqqan.
* **Logotipi:** Kubernetes logotipidagi 7 burchakli kema rulining (shturvalining) sababi — loyiha dastlab Google ichida **"Project Seven of Nine"** (Star Trek filmidagi qahramon sharafiga) kodi ostida boshlangan.
* **Nega K8s?** Kubernetes so'zida "K" va "s" harflari orasida roppa-rosa 8 ta harf bor (`u-b-e-r-n-e-t-e`). Xuddi i18n (internationalization) yoki a11y (accessibility) kabi bu numronim hisoblanadi.

---

## 5. Kubernetesning Yaratilish Tarixi va Xronologiyasi

Kubernetes noldan birdaniga paydo bo'lmagan. U Google kompaniyasining 15 yildan ortiq vaqt davomida ishlab chiqarish muhitida milliardlab konteynerlarni boshqarish tajribasiga tayanadi:

### Xronologiya:

```text
  2003-2004              2013                   2014                    2015                    Hozirgi kunda
┌──────────────┐       ┌──────────────┐       ┌────────────────┐      ┌─────────────────┐     ┌────────────────┐
│ Google BORG  │ ───>  │ Google OMEGA │ ───>  │ Kubernetes     │ ───> │ Kubernetes v1.0 │ ──> │ CNCF Standarti │
│ (Ichki sirli │       │ (Borgning    │       │ E'lon qilindi  │      │ va CNCF ga      │     │ (Dunyo standarti│
│  tizim)      │       │  davomi)     │       │ (Ochiq kodli)  │      │  topshirildi)   │     │  Cloud-Native) │
└──────────────┘       └──────────────┘       └────────────────┘      └─────────────────┘     └────────────────┘
```

1. **2003–2004: Google Borg davri**  
   Google o'zining barcha xizmatlarini (Gmail, Google Search, YouTube) konteynerlarda boshqarish uchun **Borg** nomli o'ta qudratli ichki klaster boshqaruv tizimini yaratdi. U har hafta milliardlab konteynerlarni ishga tushirardi, ammo bu tizim Google ichki siri bo'lib qolayotgandi.
2. **2013: Google Omega**  
   Borg arxitekturasidagi kamchiliklarni to'g'irlash uchun C++ da yozilgan yangi Omega boshqaruv tizimi yaratildi.
3. **2014-yil o'rtalari: Kubernetesning e'lon qilinishi**  
   Google muhandislari — **Joe Beda, Brendan Burns va Craig McLuckie** (keyinchalik ularga Brian Grant va Tim Hockin qo'shilgan) Borg va Omega ning barcha eng yaxshi tajribalarini olib, **Go (Golang)** tilida noldan yangi ochiq kodli loyiha yaratishga qaror qildilar. Loyiha **Kubernetes** deb nomlandi va ommaga e'lon qilindi.
4. **2015-yil 21-iyul: Kubernetes v1.0 va CNCF**  
   Kubernetes 1.0 versiyasi rasman taqdim etildi. Shu bilan birga, Google ushbu loyihani Linux Foundation qoshida yangi tuzilgan **CNCF (Cloud Native Computing Foundation)** fondiga sovg'a qildi. Shundan so'ng u hech qaysi bitta kompaniyaga (hatto Google ga ham) qaram bo'lmagan mustaqil xalqaro jamoatchilik loyihasiga aylandi.
5. **Bugungi kunda:**  
   Kubernetes — dunyodagi eng faol ochiq kodli loyihalardan biri (Linux yadrosidan keyin 2-o'rinda). Dunyoning yetakchi bulutli provayderlari (Google Cloud GKE, AWS EKS, Microsoft Azure AKS, DigitalOcean, Yandex Cloud) boshqariladigan Kubernetes xizmatlarini taqdim etadi.

---

## 6. Kubernetes Qanday Asosiy Muammolarni Hal Qiladi?

Kubernetes shunchaki konteyner yurgizuvchi emas, u infratuzilmaning to'liq avtopilotidir:

### 1. High Availability (Yuqori bardoshlilik) va Self-Healing (O'z-o'zini davolash)
- Konteyner to'xtab qolsa (crash bo'lsa), Kubernetes uni darhol qayta ishga tushiradi (`Restart`).
- Agar konteyner ichidagi dastur qotib qolsa (Deadlock), Liveness Probe orqali buni aniqlab, konteynerni yangisiga almashtiradi.
- Agar jismoniy server (Node) to'liq o'chib qolsa, uning ichida turgan barcha ilovalarni boshqa tirik serverlarga avtomatik ko'chiradi (`Rescheduling`).

### 2. Auto-scaling (Avtomatik Masshtablash)
- **HPA (Horizontal Pod Autoscaler):** CPU yoki RAM yuklamasi 80% dan oshsa, dastur nusxalarini 2 tadan 10 tagacha avtomatik ko'paytiradi. Yuklama tushganda yana 2 taga kamaytiradi.
- **Cluster Autoscaler:** Agar serverlarda joy qolmasa, avtomatik ravishda bulutdan (AWS/GCP) yangi server (Virtual Machine) sotib olib klasterga qo'shadi.

### 3. Zero-Downtime Rollouts va Instant Rollbacks
- Yangi versiyani chiqarishda (`RollingUpdate`), Kubernetes yangi versiyadagi 1-podni ishga tushiradi, u to'liq tayyor bo'lgachгина eski podni o'chiradi. Natijada foydalanuvchilar bir soniya ham uzilish sezmaydi.
- Agar yangi versiyada xatolik (bug) chiqsa, bitta buyruq bilan oldingi barqaror versiyaga qaytish (`Rollback`) mumkin.

### 4. Service Discovery va Load Balancing
- Har bir konteyner o'zining shaxsiy IP manziliga ega bo'ladi.
- Kubernetes o'zining ichki DNS xizmatiga ega. Dasturlar bir-birini qiyin IP manzillar orqali emas, oddiy nom orqali topadi (masalan, `http://auth-service`).
- Trafik bir nechta konteynerlar o'rtasida teng taqsimlanadi.

### 5. Storage Orchestration (Moslashuvchan Xotira)
- Konteynerlar o'zgaruvchan (ephemeral) bo'lib, o'chib ketsa ichidagi ma'lumot yo'qoladi.
- Kubernetes avtomatik tarzda lokal xotira yoki bulutli disklarni (AWS EBS, Google Persistent Disk, NFS, Ceph) konteynerga ulab beradi. Konteyner qayta ishga tushsa ham ma'lumotlar saqlanib qoladi.

### 6. Secret va Configuration Management
- Parollar, API tokenlar, SSH kalitlar va konfiguratsiyalarni dastur kodining ichiga yozish (hardcode) yoki konteyner image'ini qayta yig'ish shart emas.
- Ular **ConfigMap** va **Secret** ob'yektlari orqali xavfsiz holda konteynerga uzatiladi.

### 7. Resurslarni Tejamkor Taqsimlash (Automatic Bin Packing)
- Har bir konteynerga qancha CPU va Xotira kerakligini belgilaysiz (Requests / Limits).
- Kubernetes serverlarning bo'sh joylarini hisoblab, konteynerlarni maksimal zich va samarali joylashtiradi (xuddi Tetris o'yini kabi).

---

## 7. Kubernetes Arxitekturasi Asoslari (High-Level Overview)

Kubernetes klasteri 2 ta asosiy qismdan iborat bo'ladi:
1. **Control Plane (Master Node)** — Klastirning "Miyyasi" (qaror qabul qiluvchi markaz).
2. **Worker Nodes** — Ishchi serverlar (konteynerlar haqiqiy ishlaydigan joy).

```text
┌────────────────────────────────────────────────────────────────────────┐
│                      CONTROL PLANE (MASTER NODE)                       │
│                                                                        │
│   ┌────────────────┐   ┌────────────────┐   ┌──────────────────────┐   │
│   │ kube-apiserver │ <─│ kube-scheduler │   │ kube-controller-mgr  │   │
│   └───────┬────────┘   └────────────────┘   └──────────────────────┘   │
│           │                                                            │
│   ┌───────┴────────┐                                                   │
│   │      etcd      │                                                   │
│   └────────────────┘                                                   │
└───────────┬────────────────────────────────────────────────────────────┘
            │
            ├────────────────────────────────────────┬───────────────────...
            │                                        │
┌───────────▼───────────────┐            ┌───────────▼───────────────┐
│       WORKER NODE 1       │            │       WORKER NODE 2       │
│                           │            │                           │
│  ┌──────────────────────┐ │            │  ┌──────────────────────┐ │
│  │       kubelet        │ │            │  │       kubelet        │ │
│  └──────────────────────┘ │            │  └──────────────────────┘ │
│  ┌──────────────────────┐ │            │  ┌──────────────────────┐ │
│  │      kube-proxy      │ │            │  │      kube-proxy      │ │
│  └──────────────────────┘ │            │  └──────────────────────┘ │
│  ┌──────────────────────┐ │            │  ┌──────────────────────┐ │
│  │  Container Runtime   │ │            │  │  Container Runtime   │ │
│  │  (containerd/CRI-O)  │ │            │  │  (containerd/CRI-O)  │ │
│  └──────────────────────┘ │            │  └──────────────────────┘ │
│    [Pod]  [Pod]  [Pod]    │            │    [Pod]  [Pod]  [Pod]    │
└───────────────────────────┘            └───────────────────────────┘
```

### Control Plane Komponentlari:
- **`kube-apiserver`:** Klastirning darvozasi. Barcha ichki va tashqi so'rovlar (masalan, `kubectl` orqali) faqat shu orqali o'tadi. REST API sifatida ishlaydi.
- **`etcd`:** Klastirning barcha holati, sozlamalari va ma'lumotlari saqlanadigan yuqori tezlikdagi taqsimlangan kalit-qiymat (Key-Value) ma'lumotlar bazasi.
- **`kube-scheduler`:** Yangi yaratilgan Podlarni tahlil qilib, ularni qaysi Worker Node'ga joylashtirishni tanlaydi (resurslar bo'shlig'iga qarab).
- **`kube-controller-manager`:** Klasterdagi holatni nazorat qiluvchi turli xil kontrollerlar (Node controller, Deployment controller, Endpoint controller) to'plami. U amaldagi holat bilan kutilgan holatni bir xil bo'lishini ta'minlaydi.

### Worker Node Komponentlari:
- **`kubelet`:** Har bir worker node'da ishlaydigan agent. U `kube-apiserver`dan buyruqlarni qabul qiladi va konteynerlarning to'g'ri ishlab turishini nazorat qiladi.
- **`kube-proxy`:** Har bir node'dagi tarmoq qoidalarini (iptables / IPVS) boshqaradi, podlarga IP berilishi va tarmoq trafigining to'g'ri o'tishini ta'minlaydi.
- **`Container Runtime`:** Konteynerlarni bevosita yurgizuvchi vosita (masalan, `containerd` yoki `CRI-O`).

---

## 8. Asosiy Konsepsiyalar va Ob'yektlar (Basic Concepts)

Kubernetesda hamma narsa **Ob'yekt (Resource)** sifatida ifodalanadi:

| Ob'yekt | Nima bu? | Real hayotiy analogiya |
| :--- | :--- | :--- |
| **Pod** | Kubernetesdagi eng kichik hisoblash birligi. Odatda 1 ta (kam hollarda bir-biriga bog'liq bir nechta) konteynerni o'z ichiga oladi. | Kema kajutasi (yoki no'xat qobig'i ichidagi no'xatlar) |
| **Node** | Podlar ishlaydigan jismoniy yoki virtual server. | Bitta ishchi kemachi / Yuk mashinasi |
| **ReplicaSet** | Klasterda bir vaqtning o'zida bir xil Poddan nechta nusxa ishlab turishi kerakligini kafolatlaydi. | Zaxira xodimlar nazoratchisi |
| **Deployment** | Pod va ReplicaSet'larni yuqori darajada boshqarish: versiyalarni yangilash (rolling update) va orqaga qaytarish. | Fabrika boshqaruvchisi |
| **Service** | Podlar guruhi uchun doimiy IP manzil va ichki Load Balancer vazifasini bajaruvchi tarmoq ob'yekti. | Kompaniya qabulxonasi (Reception) |
| **Namespace** | Bitta jismoniy klaster ichida jamoalar yoki loyihalar uchun mantiqiy ajratilgan virtual klasterlar (masalan: `development`, `staging`, `production`). | Binodagi turli xonalar |
| **ConfigMap / Secret** | Dastur konfiguratsiyalari (matnlar) va maxfiy ma'lumotlar (parollar, tokenlar) saqlanadigan joy. | Sozlamalar fayli / Seyf |

---

## 9. Imperativ va Deklarativ Yondashuv

Kubernetes bilan 2 xil usulda ishlash mumkin:

### 1. Imperativ (Imperative) — "Qanday qilishni aytish":
Siz buyruqlar satrida qadam-baqadam nima qilish kerakligini buyurasiz:
```bash
# Nginx deployment yarat
kubectl create deployment my-nginx --image=nginx

# Nusxalarini 3 taga oshir
kubectl scale deployment my-nginx --replicas=3
```
*Bu usul tezkor testlar va tajribalar uchun qulay, ammo production uchun tavsiya etilmaydi.*

### 2. Deklarativ (Declarative) — "Nima xohlashingizni aytish":
Siz yakuniy maqsadni **YAML** faylda tasvirlab berasiz, Kubernetes esa tizimni o'sha holatga olib keladi:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```
Keyin esa bitta buyruq bilan qo'llaysiz:
```bash
kubectl apply -f deployment.yaml
```
*Production muhitida barcha infratuzilma aynan Deklarativ usulda (GitOps orqali) boshqariladi.*

---

## 10. Kubernetes Qachon Kerak va Qachon Ortiqcha?

Kubernetes juda qudratli vosita, ammo u "har dardga darmon" emas.

### ✅ Qachon Kubernetes kerak:
- Sizda mikroxizmatlar (microservices) arxitekturasi bo'lsa va o'nlab servislar bir-biri bilan bog'langan bo'lsa.
- Yuqori yuklama (High Traffic) bo'lsa va tizim avtomatik masshtablanishi (Auto-scaling) shart bo'lsa.
- Downtime (to'xtab qolish) qat'iyan man etilgan tizimlar (Bank, E-commerce, FinTech).
- Multi-cloud yoki gibrid bulut strategiyasida ishlash talab etilganda.

### ❌ Qachon Kubernetes ortiqcha (Overengineering):
- Oddiy monolit ilovangiz bo'lsa (bitta VPS yoki Docker Compose yetarli).
- Jamoada Kubernetesni boshqarish va muammolarni bartaraf etish (troubleshooting) bo'yicha DevOps tajribasi bo'lmasa.
- Trafik kam va doimiy bo'lsa (masshtablash shart bo'lmagan saytlar).
- Serverless xizmatlari (AWS Lambda, Cloud Run) arzonroq va soddaroq tushadigan vaziyatlarda.

---

## 11. O'z-o'zini Sinash Uchun Nazorat Savollari

Ushbu ma'ruzani o'qib bo'lgach, quyidagi savollarga mustaqil javob berishga harakat qiling:
1. Docker va Kubernetes o'rtasidagi asosiy farq nima?
2. Google ichida Kubernetes yaratilishiga asos bo'lgan tizim qanday atalgan?
3. Nima uchun Kubernetes qisqartirib "K8s" deb ataladi?
4. Control Plane va Worker Node o'rtasidagi farq nimada?
5. `kube-apiserver` va `etcd` komponentlari nima vazifani bajaradi?
6. Pod nima va nega konteyner to'g'ridan-to'g'ri emas, Pod ichida yurgiziladi?
7. "Self-Healing" xususiyati qanday ishlaydi?

---

> 🎉 **Nazariy qism yakunlandi!**  
> Endi o'rgangan bilimlaringizni amalda sinab ko'rish uchun **[LAB 01: Amaliy Mashg'ulotga](lab/README.md)** o'ting!
