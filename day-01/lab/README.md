# 🧪 Day 01: LAB — Birinchi Kubernetes Klasteri va Podni Ishga Tushirish

Ushbu amaliy mashg'ulotda biz noldan boshlab o'z kompyuterimizda mahalliy (lokal) Kubernetes klasterini ishga tushiramiz, `kubectl` orqali klaster bilan muloqot qilamiz, imperativ va deklarativ (YAML) usulda dasturlarni deploy qilamiz hamda Kubernetesning "Self-healing" (o'z-o'zini davolash) qobiliyatini amalda sinab ko'ramiz.

---

## 🎯 LAB Maqsadlari

1. Lokal Kubernetes klasterini (Minikube orqali) sozlash va ishga tushirish.
2. `kubectl` asosiy buyruqlarini o'rganish.
3. Klaster tugunlari (Nodes) va holatini tekshirish.
4. Imperativ usulda birinchi Podni yaratish va tekshirish.
5. Deklarativ (YAML) usulda Pod, Deployment va Service ishga tushirish.
6. Konteyner portini yo'naltirish (port-forwarding) orqali Nginx sahifasini brauzerda ko'rish.
7. Podni o'chirib, Deployment uni avtomatik qayta tiklashini (Self-healing) kuzatish.
8. Yaratilgan barcha resurslarni tozalash.

---

## 📋 0-Qadam: Talab qilinadigan dasturlar

LAB'ni bajarish uchun kompyuteringizda quyidagilar o'rnatilgan bo'lishi kerak:
- **Docker Desktop** (ishga tushirilgan bo'lishi shart)
- **kubectl**
- **Minikube**

### O'rnatish yo'riqnomasi (Windows uchun):

Agar ushbu vositalar o'rnatilmagan bo'lsa, PowerShell terminalida (Administrator huquqi bilan) quyidagi buyruqlar orqali o'rnatishingiz mumkin:

```powershell
# Chocolatey orqali:
choco install minikube kubernetes-cli -y

# Yoki Winget orqali:
winget install Kubernetes.kubectl
winget install Kubernetes.minikube
```

*(Linux yoki macOS uchun [minikube rasmiy saytidan](https://minikube.sigs.k8s.io/docs/start/) yuklab olishingiz mumkin).*

O'rnatilganini tekshirish:
```bash
docker --version
kubectl version --client
minikube version
```

---

## 🚀 1-Qadam: Minikube Klasterini Ishga Tushirish

Minikube — bu o'rganish va test qilish uchun mo'ljallangan yengil 1-tugunli (single-node) Kubernetes klasteridir.

Klaster ishga tushirish uchun quyidagi buyruqni bering:

```bash
minikube start --driver=docker
```

> 💡 **Izoh:** Minikube Docker konteyneri ichida to'liq Kubernetes klasterini ko'taradi. Bu jarayon birinchi marta 2-3 daqiqa vaqt olishi mumkin.

Klaster muvaffaqiyatli ishga tushgach, quyidagi xabarni ko'rasiz:
```text
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

---

## 🔍 2-Qadam: Klaster Holatini Tekshirish

Klastirimiz qanday holatda ekanligini `kubectl` orqali tekshiramiz:

### 1. Klaster ma'lumotlarini ko'rish:
```bash
kubectl cluster-info
```

### 2. Klastirdagi serverlarni (Node) ko'rish:
```bash
kubectl get nodes
```
*Natija taxminan quyidagicha bo'ladi:*
```text
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   2m    v1.30.0
```

### 3. Mavjud Namespacelarni ko'rish:
```bash
kubectl get namespaces
```

---

## ⚡ 3-Qadam: Imperativ Usulda Birinchi Podni Yaratish

Keling, eng sodda usulda — buyruq orqali Nginx veb-serveri Podini ishga tushiramiz:

```bash
kubectl run test-nginx --image=nginx:alpine --port=80
```

Natija:
```text
pod/test-nginx created
```

Pod holatini tekshiramiz:
```bash
kubectl get pods
```

Agar uning holati `Running` bo'lsa, pod muvaffaqiyatli ishga tushdi:
```text
NAME         READY   STATUS    RESTARTS   AGE
test-nginx   1/1     Running   0          25s
```

Pod haqida kengaytirilgan ma'lumot olish (qaysi IP berilgani va qaysi nodeda ekanligi):
```bash
kubectl get pods -o wide
```

---

## 🔎 4-Qadam: Pod Ichini Ko'rish va Loglarni O'qish

### 1. Podning batafsil holati va hodisalarni (Events) o'rganish:
```bash
kubectl describe pod test-nginx
```
> 📌 *Ushbu buyruq podda biron xatolik (masalan, CrashLoopBackOff yoki ImagePullBackOff) yuz berganda sababini aniqlash uchun eng asosiy buyruqdir.*

### 2. Pod loglarini o'qish:
```bash
kubectl logs test-nginx
```

### 3. Pod konteyneri ichiga kirish (Interactive Shell):
```bash
kubectl exec -it test-nginx -- sh
```
Konteyner ichiga kirganingizdan so'ng, lokal Nginx veb-sahifasini tekshiring:
```sh
wget -qO- http://localhost
```
Konteynerdan chiqish uchun:
```sh
exit
```

---

## 📄 5-Qadam: Deklarativ (YAML) Usulda Resurslar Yaratish

Productionda hamma narsa Gitda saqlanadigan deklarativ YAML fayllar orqali boshqariladi.

Ushbu darslikning `day-01/lab/manifests/` papkasida 3 ta tayyor fayl bor:
1. `01-first-pod.yaml`
2. `02-first-deployment.yaml`
3. `03-first-service.yaml`

Terminalda `day-01/lab` papkasiga o'ting:
```bash
cd day-01/lab
```

### 1. YAML orqali Pod yaratish:
```bash
kubectl apply -f manifests/01-first-pod.yaml
```

Natijani tekshirish:
```bash
kubectl get pods
```
Siz ro'yxatda `my-first-pod` nomli yangi podni ko'rasiz!

---

## 📦 6-Qadam: Deployment va Service Yaratish

Alohida (standalone) Pod yaratish xavfli, chunki pod o'chib qolsa, uni hech kim qayta tiriltirmaydi. Shuning uchun har doim **Deployment** ishlatiladi!

### 1. Deployment yaratamiz (3 ta Nginx nusxasi):
```bash
kubectl apply -f manifests/02-first-deployment.yaml
```

Tekshiramiz:
```bash
kubectl get deployments
kubectl get pods -l app=my-web-app
```
*Siz 3 ta bir xil Nginx podlari parallel ishlayotganini ko'rasiz!*

### 2. Service yaratamiz (Podlarni tashqi tarmoqqa ochish):
```bash
kubectl apply -f manifests/03-first-service.yaml
```

Servislarni ko'rish:
```bash
kubectl get services
```

---

## 🌐 7-Qadam: Dasturni Brauzerda Ochish

Podlar klaster ichki tarmog'ida bo'lgani sababli, unga o'z kompyuteringiz brauzeridan to'g'ridan-to'g'ri ulanish uchun portni yo'naltirish (Port Forwarding) qilamiz:

```bash
kubectl port-forward svc/my-first-service 8080:80
```

Endi brauzeringizni ochib, quyidagi manzilga kiring:
👉 **[http://localhost:8080](http://localhost:8080)**

Siz **"Welcome to nginx!"** yozuvini ko'rasiz! 🎉

*(Terminalda port-forwardingni to'xtatish uchun `Ctrl + C` tugmasini bosing).*

---

## 🛡️ 8-Qadam: Kubernetesning "Self-Healing" Mo'jizasini Sinash

Deployment yaratganimizda `replicas: 3` deb ko'rsatgan edik.
Keling, eksperiment o'tkazamiz: Podlardan birini majburan o'chirib tashlaymiz!

1. Avval mavjud podlar nomini bilib oling:
```bash
kubectl get pods -l app=my-web-app
```

2. Podlardan ixtiyoriy bittasini o'chiring:
```bash
kubectl delete pod <POD_NOMI>
```
*(Masalan: `kubectl delete pod my-first-deployment-7bb5df75c7-abcde`)*

3. Darhol yana podlarni tekshiring:
```bash
kubectl get pods -l app=my-web-app
```

**Natija:** Siz o'chirgan pod o'chadi, ammo Kubernetes **1 soniya ichida yangi pod yaratib**, umumiy podlar sonini yana 3 taga yetkazib qo'yadi!  
Mana bu — **Self-Healing** (O'z-o'zini davolash) deb ataladi! 🚀

---

## 🧹 9-Qadam: Klaster Resurslarini Tozalash

Tajribalardan so'ng yaratilgan resurslarni tozalab qo'yamiz:

```bash
# Imperativ podni o'chirish
kubectl delete pod test-nginx

# YAML orqali yaratilgan barcha resurslarni o'chirish
kubectl delete -f manifests/03-first-service.yaml
kubectl delete -f manifests/02-first-deployment.yaml
kubectl delete -f manifests/01-first-pod.yaml

# Minikube klasterni to'xtatish (kerak bo'lganda)
minikube stop
```

---

## 📝 10-Qadam: Mustaqil Amaliy Vazifa (Homework)

O'rgangan bilimlaringizni mustahkamlash uchun quyidagi vazifani bajaring:

1. `day-01/lab/manifests/` ichida `04-homework-pod.yaml` nomli yangi fayl oching.
2. Unda quyidagi parametrlarga ega Pod tasvirlang:
   - Pod nomi: `my-homework-app`
   - Label: `app: homework`, `env: dev`
   - Konteyner nomi: `httpd-container`
   - Image: `httpd:alpine` (Apache veb serveri)
   - Port: `80`
3. Ushbu podni klasterda ishga tushiring: `kubectl apply -f manifests/04-homework-pod.yaml`
4. `kubectl port-forward pod/my-homework-app 8081:80` qiling va brauzerda `http://localhost:8081` ga kirib, **"It works!"** yozuvi chiqqaniga ishonch hosil qiling.
5. Bajarganingizdan so'ng podni o'chiring.

---

> 👏 **Tabriklaymiz! Siz 1-kun amaliyotini muvaffaqiyatli yakunladingiz!**  
> Keyingi darsda biz Kubernetes arxitekturasini chuqurlashtirilgan tarzda o'rganamiz.
