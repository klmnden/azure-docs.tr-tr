---
title: Azure geçişi, Toplayıcı gerecini yükseltme | Microsoft Docs
description: Azure geçiş toplayıcısı adlı Gereci için yükseltme hakkında bilgi sağlar.
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: 7cd44318716200d665ece9ffecc45225bdfb85eb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60685928"
---
# <a name="collector-appliance-updates"></a>Toplayıcı Gereci güncelleştirmeleri

Bu makalede, Toplayıcı Gereci için yükseltme bilgileri özetler [Azure geçişi](migrate-overview.md).

Azure geçişi toplayıcısı bir şirket içi vCenter ortam azure'a geçiş işleminden önce değerlendirme amaçları için keşfetmek için hafif bir gereçtir. [Daha fazla bilgi edinin](concepts-collector.md).

## <a name="how-to-upgrade-the-appliance"></a>Gereç yükseltme

OVA yeniden indirmeden Toplayıcı en son sürüme yükseltebilirsiniz.

1. Kapat tüm tarayıcı pencerelerini ve tüm dosyaların/klasörlerin Gereci açın.
2. Aşağıda, bu makalede bahsedilen güncelleştirmeler listesinden en son yükseltme paketini indirin.
3. İndirilen paketi güvenli olmasını sağlamak için yönetici komut penceresi açın ve karma ZIP dosyası oluşturmak için aşağıdaki komutu çalıştırın. Oluşturulan karma karşı belirli bir sürüm belirtilen karma değeri ile eşleşmesi gerekir:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Örnek: **C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
4. Toplayıcı gerecini VM zip dosyasını kopyalayın.
5. Zip dosyasını sağ tıklayın > **tümünü Ayıkla**.
6. Sağ **Setup.ps1** > **PowerShell ile Çalıştır**ve yükleme yönergelerini izleyin.

## <a name="collector-update-release-history"></a>Toplayıcı güncelleştirme sürüm geçmişi

### <a name="continuous-discovery-upgrade-versions"></a>Sürekli bulma: Sürüm yükseltme

#### <a name="version-101014-released-on-03292019"></a>Sürüm 1.0.10.14 (29/03/2019 üzerinde yayımlanan)

Birkaç kullanıcı Arabirimi geliştirmeleri içerir.

Karma değerleri yükseltme [paketini 1.0.10.14](https://aka.ms/migrate/col/upgrade_10_14)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 846b1eb29ef2806bcf388d10519d78e6
SHA1 | 6243239fa49c6b3f5305f77e9fd4426a392d33a0
SHA256 | fb058205c945a83cc4a31842b9377428ff79b08247f3fb8bb4ff30c125aa47ad

#### <a name="version-101012-released-on-03132019"></a>Sürüm 1.0.10.12 (03/13/2019'da yayımlanan)

Sorunlar için düzeltmeler içerir Azure seçme içinde bulut Gereci.

Karma değerleri yükseltme [paketini 1.0.10.12](https://aka.ms/migrate/col/upgrade_10_12)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 27704154082344c058238000dff9ae44
SHA1 | 41e9e2fb71a8dac14d64f91f0fd780e0d606785e
SHA256 | c6e7504fcda46908b636bfe25b8c73f067e3465b748f77e50027e66f2727c2a9

### <a name="one-time-discovery-deprecated-now-previous-upgrade-versions"></a>Tek seferlik bulma (artık kullanım dışı): Önceki yükseltme sürümleri

> [!NOTE]
> Bu yöntem, vCenter Server'ın performans veri noktası kullanılabilirlik için istatistik ayarları yararlandı ve sanal makinelerin Azure'a geçiş için eksik boyutlandırma içinde sonuçlanan ortalama performans sayaçlarının toplanan gibi tek seferlik gereç artık kullanım dışı bırakılmıştır.

#### <a name="version-10916-released-on-10292018"></a>Sürüm 1.0.9.16 (29/10/2018 tarihinde yayımlanan)

Gereç kurulurken karşılaşılan Powerclı sorunlar için düzeltmeler içerir.

Karma değerleri yükseltme [paketini 1.0.9.16](https://aka.ms/migrate/col/upgrade_9_16)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | d2c53f683b0ec7aaf5ba3d532a7382e1
SHA1 | e5f922a725d81026fa113b0c27da185911942a01
SHA256 | a159063ff508e86b4b3b7b9a42d724262ec0f2315bdba8418bce95d973f80cfc

#### <a name="version-10914"></a>Sürüm 1.0.9.14

Karma değerleri yükseltme [paketini 1.0.9.14](https://aka.ms/migrate/col/upgrade_9_14)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

#### <a name="version-10913"></a>Sürüm 1.0.9.13

Karma değerleri yükseltme [paketini 1.0.9.13](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e


## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](concepts-collector.md) Toplayıcı gerecini hakkında.
- [Değerlendirme çalıştırma](tutorial-assessment-vmware.md) VMware Vm'leri için.
