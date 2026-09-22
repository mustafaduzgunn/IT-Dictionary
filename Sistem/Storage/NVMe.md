````markdown
# NVMe

## Tanım

**Non-Volatile Memory Express (NVMe)**, özellikle SSD'ler gibi non-volatile storage cihazlarına erişim için tasarlanmış bir storage protokolüdür.

NVMe, geleneksel SATA/AHCI tabanlı storage erişiminin sınırlamalarını aşmak amacıyla geliştirilmiştir. PCIe (Peripheral Component Interconnect Express) üzerinden çalışarak düşük latency ve yüksek I/O performansı sağlamayı hedefler.

---

## Neden Gereklidir?

SSD'ler NAND Flash teknolojisi sayesinde mekanik disklerden çok daha yüksek performans sağlayabilir. Ancak SSD'nin fiziksel olarak hızlı olması, storage erişim protokolünün de bu performansa uygun olması gerektiği anlamına gelir.

Geleneksel SATA ve AHCI mimarileri başlangıçta mekanik disklerin çalışma modeline göre tasarlanmıştır.

NVMe ise flash tabanlı storage cihazlarının yüksek paralellik ve düşük latency özelliklerinden yararlanmak amacıyla tasarlanmıştır.

Temel yaklaşım:

```text
HDD
 ↓
SATA
 ↓
AHCI
````

yerine yüksek performanslı SSD'lerde:

```text
NAND Flash
 ↓
SSD
 ↓
NVMe
 ↓
PCIe
```

mimarisinin kullanılabilmesidir.

---

## Hangi Problemi Çözer?

NVMe'nin temel olarak çözmeye çalıştığı problem, yüksek hızlı SSD'lerin performansının geleneksel storage erişim protokolleri tarafından sınırlandırılmasıdır.

NVMe:

* Çok daha fazla I/O queue destekler.
* Queue başına çok sayıda command destekler.
* Düşük latency hedefler.
* Çok çekirdekli CPU mimarilerine daha uygun bir I/O modeli sunar.
* PCIe'nin yüksek bant genişliğinden yararlanır.

Bu nedenle özellikle yüksek IOPS ve düşük latency gerektiren workload'larda önemlidir.

---

## Nasıl Çalışır?

Basitleştirilmiş NVMe storage erişimi:

```text
Application
     |
     ↓
Operating System
     |
     ↓
NVMe Driver
     |
     ↓
PCIe
     |
     ↓
NVMe Controller
     |
     ↓
NAND Flash
```

İşletim sistemi NVMe cihazıyla iletişim kurmak için NVMe driver kullanır.

NVMe controller, gelen I/O komutlarını storage üzerindeki NAND Flash üzerinde gerçekleştirir.

NVMe'nin önemli özelliklerinden biri **queue-based** mimarisidir.

Geleneksel storage protokollerine kıyasla çok daha yüksek sayıda queue ve command destekleyerek paralel I/O işlemlerinin daha verimli gerçekleştirilmesini sağlar.

---

## Temel Bileşenler

### NVMe Controller

NVMe SSD'nin işletim sistemiyle iletişim kurmasını sağlayan controller'dır.

Host tarafından gönderilen NVMe command'lerini işler ve storage üzerinde gerekli işlemleri gerçekleştirir.

### NVMe Driver

İşletim sistemi ile NVMe controller arasındaki iletişimi sağlar.

### PCIe

NVMe SSD'nin host sistemiyle yüksek hızlı iletişim kurmasını sağlayan bus/interconnect teknolojisidir.

### NAND Flash

Verilerin kalıcı olarak saklandığı non-volatile memory teknolojisidir.

---

## NVMe ve Diğer Disk Teknolojileri

| Özellik             | HDD            | SATA SSD             | NVMe SSD                    |
| ------------------- | -------------- | -------------------- | --------------------------- |
| Storage teknolojisi | Manyetik disk  | NAND Flash           | NAND Flash                  |
| Arabirim            | SATA/SAS       | SATA                 | PCIe                        |
| Protokol            | HDD/SATA/SAS   | AHCI                 | NVMe                        |
| Hareketli parça     | Var            | Yok                  | Yok                         |
| Latency             | Yüksek         | Düşük                | Çok düşük                   |
| IOPS                | Düşük          | Orta/Yüksek          | Yüksek/Çok yüksek           |
| Paralellik          | Düşük          | Sınırlı              | Yüksek                      |
| Tipik kullanım      | Kapasite/Arşiv | Genel amaçlı storage | Yüksek performanslı storage |

---

## Avantajları

* Düşük latency sağlar.
* Yüksek IOPS sağlayabilir.
* PCIe'nin yüksek bant genişliğinden yararlanır.
* Çok sayıda paralel I/O işlemini destekler.
* Çok çekirdekli sistemlerde yüksek I/O performansı sağlayabilir.
* Modern server ve data center workload'ları için uygundur.

---

## Dezavantajları

* SATA SSD'lere göre daha yüksek maliyetli olabilir.
* NVMe performansından yararlanabilmek için host sistemin PCIe ve NVMe desteğine sahip olması gerekir.
* Yüksek performanslı NVMe cihazlarında ısı üretimi önemli olabilir.
* Her workload için NVMe'nin sağladığı yüksek performans gerekli değildir.

---

## Nerelerde Kullanılır?

NVMe özellikle yüksek storage performansı gerektiren ortamlarda kullanılır:

* Server
* Data Center
* Virtualization
* Database
* Cloud
* Kubernetes
* High Performance Computing
* Enterprise Storage

Örneğin bir virtualization sunucusunda çok sayıda sanal makinenin aynı anda yoğun I/O gerçekleştirmesi durumunda NVMe'nin düşük latency ve yüksek IOPS özellikleri önemli olabilir.

---

## Ne Zaman Tercih Edilir?

Yüksek I/O performansı ve düşük latency önemliyse NVMe tercih edilebilir.

Örneğin:

```text
Yüksek IOPS ihtiyacı
        +
Düşük latency ihtiyacı
        +
PCIe desteği
        ↓
      NVMe
```

Buna karşılık yüksek performans ihtiyacının düşük olduğu ve maliyetin öncelikli olduğu bir storage workload'unda HDD veya SATA SSD yeterli olabilir.

---

## Gerçek Hayattan Örnek

Bir virtualization sunucusunda:

```text
             Virtualization Host
                    |
          ┌─────────┼─────────┐
          ↓         ↓         ↓
         VM1       VM2       VM3
          \         |         /
           \        |        /
            ↓       ↓       ↓
             NVMe Storage
                  |
                PCIe
```

VM'lerin aynı anda yoğun disk I/O gerçekleştirdiği bir ortamda NVMe'nin yüksek IOPS ve düşük latency özellikleri storage performansına katkı sağlayabilir.

Benzer şekilde database sunucularında yüksek I/O gerektiren workload'larda NVMe kullanılabilir.

---

## Diğer Konularla İlişkisi

```text
NVMe
│
├── Storage
│   ├── SSD
│   ├── NAND Flash
│   └── Storage Performance
│
├── PCIe
│
├── Server
│
├── Virtualization
│
├── Database
│
└── Performance
    ├── IOPS
    ├── Throughput
    ├── Latency
    └── Queue Depth
```

NVMe'nin özellikle **PCIe**, **SSD**, **NAND Flash**, **IOPS**, **Latency** ve **Queue Depth** kavramlarıyla birlikte öğrenilmesi önemlidir.

---

## Bilinmesi Gereken İlgili Kavramlar

* PCIe (Peripheral Component Interconnect Express)
* SSD (Solid State Drive)
* NAND Flash
* SATA
* AHCI (Advanced Host Controller Interface)
* IOPS (Input/Output Operations Per Second)
* Throughput
* Latency
* Queue Depth
* NVMe-oF (NVMe over Fabrics)
* Storage
* Virtualization
* Server

```
```
