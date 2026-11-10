# 🌐 NetPractice - TCP/IP ve Ağ Yapılandırma Eğitimi

<div align="center">

![42 School](https://img.shields.io/badge/School-42-black?style=for-the-badge&logo=42)
![Networking](https://img.shields.io/badge/Networking-TCP%2FIP-0A66C2?style=for-the-badge&logo=cisco&logoColor=white)
![NetPractice](https://img.shields.io/badge/Project-NetPractice-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

**Bir ağ topolojisindeki IP adreslerini, alt ağ maskelerini ve yönlendirme (routing) tablolarını doğru yapılandırarak tüm cihazların birbirine ve internete ulaşmasını sağlama projesi**

*NetPractice, kod yazılmayan bir 42 projesidir. Amaç; IP adresleme, alt ağ (subnet) hesabı ve yönlendirme mantığını web tabanlı bir simülatörde 10 seviye boyunca uygulayarak kavramaktır.*

[Projenin Amacı](#-projenin-amacı) • [Temel Kavramlar](#-temel-kavramlar) • [Alt Ağ Hesabı](#-alt-ağ-subnet-hesabı) • [Yönlendirme](#-yönlendirme-routing) • [Seviyeler](#-seviyeler)

</div>

---

## 🎯 Projenin Amacı

<p align="center">
  <img
    src="https://github.com/user-attachments/assets/8b0893c9-6b78-4d34-ba95-8d2c43ce1a3a"
    width="450"
    alt="Running Man ASCII GIF"
  />
</p>

Bu proje, bir **ağ temelleri** projesi olarak, TCP/IP protokol ailesinin en temel parçası olan **adreslemeyi** ve **yönlendirmeyi** öğretmeyi amaçlamaktadır. Her seviyede bir ağ diyagramı verilir: bilgisayarlar, switch'ler, router'lar ve internet. Bazı IP adresleri, maskeler ve yönlendirme girdileri eksik ya da hatalıdır. Görev, **düzenlenebilir alanları** doğru değerlerle doldurarak tüm cihazların istenen hedeflere ulaşmasını sağlamaktır.

Proje boyunca hiçbir kod yazılmaz; bunun yerine **mantık yürütme** ve **hesaplama** ön plandadır. Savunma (defense) sırasında yalnızca doğru cevabı bulmak yeterli değildir, **neden doğru olduğunu** açıklayabilmek gerekir.

### 🎓 **Öğrenme Hedefleri:**

#### 🔢 **IP Adresleme**
- IPv4 adres yapısı (32 bit, 4 oktet) ve ikili (binary) gösterim
- Public ve private IP aralıkları (RFC 1918)
- Network adresi, broadcast adresi ve kullanılabilir host aralığı kavramları

#### ✂️ **Alt Ağ (Subnet) Hesabı**
- Alt ağ maskesi ve CIDR gösterimi (`/24`, `/26`, `/30`...)
- Bir IP adresinin hangi ağa ait olduğunu bulma
- İhtiyaca göre doğru boyutta alt ağ seçme

#### 🛣️ **Yönlendirme**
- Default gateway kavramı
- Router üzerinde yönlendirme tablosu (routing table) okuma ve yazma
- Varsayılan rota (`0.0.0.0/0`) ve spesifik rotalar
- Paketin kaynaktan hedefe giderken izlediği yolu adım adım takip etme

---

## 📁 Proje Yapısı

Proje kod içermez. Teslim edilen depo, her seviyenin çözümünü ve açıklamalarını barındırır:

```
netpractice/
│
├── 📄 README.md                 # Proje dokümantasyonu
│
└── 📂 configs/                  # Her seviyenin kaydedilmiş yapılandırması
    ├── level_1.json
    ├── level_2.json
    ├── ...
    └── level_10.json
 
```

> 📌 Klasör adları ve dosya biçimi, o dönemin konu (subject) dosyasına göre değişebilir. Kendi deponuzdaki yapıya göre düzenleyin.

---

## 🧱 Temel Kavramlar

### 🖥️ Ağdaki Cihazlar

| Cihaz | Görevi | Yapılandırma |
|---|---|---|
| **Bilgisayar (Host)** | Uç cihaz; veri gönderir/alır | IP adresi, alt ağ maskesi, default gateway |
| **Switch** | Aynı ağdaki cihazları birbirine bağlar (L2) | IP adresi **yoktur**, yapılandırılmaz |
| **Router** | Farklı ağları birbirine bağlar, paketleri yönlendirir (L3) | Her arayüz (interface) için ayrı IP + maske, yönlendirme tablosu |
| **Internet** | Dış dünyayı temsil eden özel cihaz | Genellikle sabit, yalnızca bazı alanlar düzenlenebilir |

### 🔢 IPv4 Adresi

Bir IPv4 adresi **32 bitten** oluşur ve noktalarla ayrılmış 4 oktet olarak yazılır:

```
192.168.1.10   →   11000000.10101000.00000001.00001010
```

Her adres iki parçadan oluşur: **ağ kısmı** (hangi ağdayım?) ve **host kısmı** (o ağda hangi cihazım?). Bu ayrımı **alt ağ maskesi** yapar.

### 🎭 Alt Ağ Maskesi

Maske, adresin kaç bitinin ağ kısmına ait olduğunu söyler. Ağ kısmı `1`, host kısmı `0` ile gösterilir:

```
255.255.255.0   →   11111111.11111111.11111111.00000000   →   /24
255.255.255.192 →   11111111.11111111.11111111.11000000   →   /26
255.255.255.252 →   11111111.11111111.11111111.11111100   →   /30
```

Bir IP adresi ile maskesi **AND** işlemine sokulursa **ağ adresi** bulunur.

### 🏠 Private IP Aralıkları (RFC 1918)

| Aralık | CIDR | Tipik Kullanım |
|---|---|---|
| `10.0.0.0` – `10.255.255.255` | `10.0.0.0/8` | Büyük kurumsal ağlar |
| `172.16.0.0` – `172.31.255.255` | `172.16.0.0/12` | Orta ölçekli ağlar |
| `192.168.0.0` – `192.168.255.255` | `192.168.0.0/16` | Ev ve küçük ofis ağları |

Bu aralıklar internette yönlendirilmez; yalnızca yerel ağlarda kullanılır.

---

## ✂️ Alt Ağ (Subnet) Hesabı

### 📐 Hızlı Başvuru Tablosu

| CIDR | Maske | Toplam Adres | Kullanılabilir Host | Blok Boyutu (son oktet) |
|---|---|---|---|---|
| `/24` | `255.255.255.0` | 256 | 254 | 256 |
| `/25` | `255.255.255.128` | 128 | 126 | 128 |
| `/26` | `255.255.255.192` | 64 | 62 | 64 |
| `/27` | `255.255.255.224` | 32 | 30 | 32 |
| `/28` | `255.255.255.240` | 16 | 14 | 16 |
| `/29` | `255.255.255.248` | 8 | 6 | 8 |
| `/30` | `255.255.255.252` | 4 | 2 | 4 |

> **Kural:** Kullanılabilir host sayısı = `2^(host bit sayısı) - 2`. İki adres her zaman ayrılmıştır: **ağ adresi** (tüm host bitleri `0`) ve **broadcast adresi** (tüm host bitleri `1`). Bu ikisi bir cihaza **atanamaz**.

### 🧮 Örnek: `192.168.1.77/26`

```
Blok boyutu       : 64   (256 - 192)
Bloklar           : 0, 64, 128, 192
77 hangi blokta?  : 64 ≤ 77 < 128  →  64'lük blok

Ağ adresi         : 192.168.1.64
İlk kullanılabilir: 192.168.1.65
Son kullanılabilir: 192.168.1.126
Broadcast adresi  : 192.168.1.127
```

### ✅ Aynı Ağda Olmanın Koşulu

İki cihazın **doğrudan** (router olmadan) haberleşebilmesi için:

1. İkisinin **ağ adresi aynı** olmalı,
2. İkisinin **alt ağ maskesi aynı** olmalı,
3. IP adresleri **benzersiz** olmalı,
4. Hiçbiri **ağ adresi** ya da **broadcast adresi** olmamalı.

---

## 🛣️ Yönlendirme (Routing)

Bir paket, **hedef IP'si kendi ağında değilse** doğrudan gönderilemez; bir **gateway**'e (çıkış kapısı) verilmelidir.

### 🚪 Default Gateway

Bir host'un kendi alt ağı dışına çıkmak için paketi teslim ettiği router arayüzünün IP adresidir. Gateway:

- Host ile **aynı alt ağda** olmalıdır,
- Router'ın o ağa bakan **arayüzünün IP adresi** olmalıdır.

### 📋 Yönlendirme Tablosu

Router'daki her satır şu soruyu yanıtlar: *"Bu hedef ağa gidecek paketi kime vereyim?"*

| Alan | Anlamı |
|---|---|
| **Destination** | Hedef ağ (ör. `192.168.2.0/24`) |
| **Next hop (Gateway)** | Paketin bir sonraki durağı (komşu router'ın IP'si) |
| **Interface** | Paketin çıkacağı arayüz |

### 🌍 Varsayılan Rota (Default Route)

```
0.0.0.0/0  →  next hop: <internet'e giden router IP'si>
```

`0.0.0.0/0` tüm adresleri kapsar. Tabloda daha spesifik bir eşleşme yoksa paket bu rotaya gönderilir. Internet'e erişim için genellikle gereklidir.

### 🔍 Kural: En Uzun Prefix Eşleşmesi

Birden fazla satır hedefle eşleşiyorsa, **maskesi en uzun olan** (en spesifik) satır seçilir. Bu yüzden `0.0.0.0/0` her zaman en son başvurulan rotadır.

### 🔄 Gidiş-Dönüş Mantığı

Haberleşmenin çalışması için paketin **hem gidişte hem dönüşte** yol bulması gerekir. Sık yapılan hata, yalnızca gidiş rotasını yazıp **dönüş rotasını unutmaktır**. Bir seviye çözülmüyorsa önce şunu sorun: *"Cevap paketi nasıl geri dönecek?"*

---

## 🧭 Çözüm Yöntemi

Her seviyede izlediğim adımlar:

1. **Diyagramı oku:** Hangi cihazlar var, hangi alanlar düzenlenebilir, hedef ne (hangi cihaz hangisine ulaşmalı)?
2. **Ağları ayır:** Switch'lerle birbirine bağlı gruplar **tek bir ağdır**. Router'lar ağları ayırır. Kaç farklı ağ olduğunu belirle.
3. **Sabit değerleri incele:** Değiştirilemeyen IP/maskelerden ağ adresini ve aralığı çıkar.
4. **Maskeleri belirle:** Aynı ağdaki tüm cihazların maskesi aynı olmalı. Gerekli host sayısına göre uygun boyutu seç.
5. **IP'leri ata:** Ağ ve broadcast adreslerinden kaçın, çakışma olmasın.
6. **Gateway'leri yaz:** Her host'un gateway'i kendi ağındaki router arayüzü olsun.
7. **Yönlendirme tablolarını doldur:** Gidiş **ve** dönüş yolunu kontrol et. Gerekirse `0.0.0.0/0` kullan.
8. **Test et:** Simülatörün "Check again" butonuyla doğrula, ardından yapılandırmayı kaydet.

---

## 🚫 Projedeki Kurallar

- Yalnızca **düzenlenebilir** alanlar değiştirilebilir; sabit alanlara dokunulamaz.
- Bir ağda **iki cihaz aynı IP'yi** kullanamaz.
- **Ağ adresi** ve **broadcast adresi** bir cihaza atanamaz.
- Loopback aralığı (`127.0.0.0/8`) kullanılamaz.
- Çözüm yalnızca "çalışan" değil, **mantıklı ve gerekçelendirilebilir** olmalıdır. Savunmada sorulan her IP/maske seçimi açıklanabilmelidir.

---

## 📊 Seviyeler

Proje, zorluğu kademeli artan **10 seviyeden** oluşur. Genel olarak ilerleyiş şöyledir:

| Seviye Grubu | Odak | Öğrenilen Kavram |
|---|---|---|
| **1 – 3** | Doğrudan bağlı hostlar, switch kullanımı | IP, maske, aynı ağ koşulu |
| **4 – 6** | Birden fazla ağ, tek router | Gateway, alt ağ bölme, basit yönlendirme |
| **7 – 9** | Birden fazla router, internet bağlantısı | Yönlendirme tabloları, default route, gidiş-dönüş yolu |
| **10** | En kapsamlı topoloji | Tüm bilgilerin birleşimi |

### 📝 Seviye Çözüm Şablonu

Her seviye için kendi çözümünüzü şu formatta belgeleyebilirsiniz:

```
### Seviye X

**Hedef:** (hangi cihazlar haberleşmeli?)

| Cihaz / Arayüz | IP Adresi | Maske | Gateway |
|---|---|---|---|
| A | ... | ... | ... |
| R1 (eth0) | ... | ... | - |

**Yönlendirme tablosu (R1):**
| Destination | Next hop |
|---|---|
| ... | ... |

**Gerekçe:** (neden bu maske? neden bu rota?)
```

---

## 💡 İpuçları

1. **Önce ağları say.** Switch'ler ağı bölmez, router'lar böler. Kaç ağ olduğunu bilmek yarı işi çözer.
2. **Bloklarla düşün.** İkili sisteme dalmak yerine blok boyutunu (`256 - maske`) kullanmak çok daha hızlıdır.
3. **İki yönlü kontrol et.** Gidiş çalışıyor ama cevap dönmüyorsa sorun dönüş rotasındadır.
4. **Router arayüzlerini ayrı say.** Bir router'ın her arayüzü **farklı bir ağdadır**, farklı IP alt ağlarına ait olmalıdır.
5. **Point-to-point bağlantılar için `/30`.** İki router arasındaki bağlantıda yalnızca 2 adres gerekir.
6. **Tabloyu yazıya dök.** Karmaşık seviyelerde ağları, maskeleri ve rotaları kağıda yazmak hataları azaltır.

---

## 📚 Notlar

### ⚠️ **Dikkat Edilmesi Gerekenler**

- Sistem, her giriş yaptığınızda **farklı bir sorun seti** verebilir; bu yüzden çözümü ezberlemek yerine **yöntemi** öğrenmek gerekir.
- Savunmada, değerlendirici **simülatörden yeni bir yapılandırma** üretip anında çözmenizi isteyebilir.
- Alt ağ hesabını **elle** yapabilmek beklenir; online hesaplayıcılar öğrenme sürecinde kontrol için kullanılmalıdır.
- Yapılandırma dosyalarını ve ekran görüntülerini teslim etmeyi unutmayın.

### 📖 **Faydalı Kaynaklar**

- RFC 791 — Internet Protocol (IPv4)
- RFC 1918 — Address Allocation for Private Internets
- RFC 4632 — Classless Inter-domain Routing (CIDR)

---

<div align="center">

**📝 "Know your subnets, route your packets."**

---

### 👨‍💻 Created by Sude Naz Karayıldırım

[![42 Profile](https://img.shields.io/badge/42%20Profile-skarayil-black?style=flat-square&logo=42&logoColor=white)](https://profile.intra.42.fr/users/skarayil)
[![GitHub](https://img.shields.io/badge/GitHub-skarayil-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/skarayil)

**⭐ Eğer bu proje işinize yaradıysa, repo'ya star vermeyi unutmayın!**

</div>
