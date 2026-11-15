# 🚀 Day 01: Kubernetes Kirish — Tarixi, Yechilgan Muammolar va Asosiy Konsepsiyalar

Xush kelibsiz! Ushbu 1-kunning maqsadi — sizda Kubernetes nima ekanligi, u qanday zarurat ortidan paydo bo'lganligi va qanday global muammolarni hal qilishi haqida mustahkam poydevor yaratishdir.

---

## 📌 Kun Mundarijasi

1. **[Ma'ruza (Nazariy qism)](maruza.md)**
   - Dasturlarni deploy qilish evolyutsiyasi (Monolit → Virtualizatsiya → Konteynerlar)
   - Nega Docker yetarli emas? Konteyner orkestratsiyasi nima?
   - Kubernetes nima va uning nomi qayerdan kelgan?
   - Kubernetes tarixi: Google Borg, Omega va CNCF ga topshirilishi
   - Kubernetes qanday muammolarni hal qiladi? (Avtomatik masshtablash, Self-Healing, Zero-downtime Rollout va h.k.)
   - Asosiy arxitektura tushunchalari: Control Plane va Worker Node
   - Asosiy ob'yektlar: Pod, ReplicaSet, Deployment, Service, Namespace
   - Kubernetes qachon kerak va qachon kerak emas?
   - O'z-o'zini tekshirish uchun savollar

2. **[Amaliy Mashg'ulot (LAB 01)](lab/README.md)**
   - Ishchi muhitni sozlash (Docker, Minikube va `kubectl`)
   - Birinchi Kubernetes klasterini ishga tushirish
   - Klaster komponentlari va holatini tekshirish
   - Birinchi Pod va Deployment yaratish (Nginx misolida)
   - Konteyner portini yo'naltirish (port-forwarding) va brauzerda natijani ko'rish
   - Resurslarni tozalash
   - Mustaqil amaliy vazifa (Homework)

---

## 🧭 Tezkor Havolalar

| Bo'lim | Tavsif | Havola |
| :--- | :--- | :---: |
| 📖 **Nazariy Ma'ruza** | Kubernetes tarixi, mezonlari va konseptual modeli | [O'qishga o'tish ➡️](maruza.md) |
| 🧪 **LAB 01 Yo'riqnomasi** | Minikube o'rnatish va birinchi Podni ishga tushirish | [Amaliyotga o'tish ➡️](lab/README.md) |
| 📁 **YAML Manifestlar** | Dars davomida qo'llaniladigan Kubernetes konfiguratsiyalari | [Manifestlar papkasi ➡️](lab/manifests/) |

---

## 💡 1-Kunning Asosiy Kalit So'zlari

- **K8s:** Kubernetes so'zining qisqartmasi (K va s o'rtasida 8 ta harf).
- **Container Orchestration:** Yuzlab yoki minglab konteynerlarning hayot siklini avtomatik boshqarish.
- **Borg:** Google kompaniyasining ichki klaster boshqaruv tizimi (Kubernetesning ajdodi).
- **Control Plane:** Klastirning "miyyasi" (API Server, etcd, Scheduler, Controller Manager).
- **Worker Node:** Konteynerlar haqiqiy ishlaydigan ishchi server (kubelet, kube-proxy, runtime).
- **Pod:** Kubernetesdagi eng kichik ishga tushirish birligi.
