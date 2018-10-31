---
title: Azure geçişi, Toplayıcı gerecini yükseltme | Microsoft Docs
description: Azure geçiş toplayıcısı adlı Gereci için yükseltme hakkında bilgi sağlar.
author: musa-57
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: hamusa
services: azure-migrate
ms.openlocfilehash: cd48b824845a0195fc78814a88dd449507c99394
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50241472"
---
# <a name="collector-update-release-history"></a>Toplayıcı güncelleştirme sürüm geçmişi

Bu makalede, Toplayıcı Gereci için yükseltme bilgileri özetler [Azure geçişi](migrate-overview.md).

Azure geçişi toplayıcısı bir şirket içi vCenter ortam azure'a geçiş işleminden önce değerlendirme amaçları için keşfetmek için hafif bir gereçtir. [Daha fazla bilgi edinin](concepts-collector.md).


## <a name="one-time-discovery-upgrade-versions"></a>Tek seferlik: sürüm yükseltme

### <a name="version-10916-released-on-10292018"></a>Sürüm 1.0.9.16 (29/10/2018 tarihinde yayımlanan)

Gereç kurulurken karşılaşılan Powerclı sorunlar için düzeltmeler içerir. 

Karma değerleri yükseltme [paketini 1.0.9.16](https://aka.ms/migrate/col/upgrade_9_16)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | d2c53f683b0ec7aaf5ba3d532a7382e1
SHA1 | e5f922a725d81026fa113b0c27da185911942a01
SHA256 | a159063ff508e86b4b3b7b9a42d724262ec0f2315bdba8418bce95d973f80cfc

### <a name="version-10914"></a>Sürüm 1.0.9.14

Karma değerleri yükseltme [paketini 1.0.9.14](https://aka.ms/migrate/col/upgrade_9_14)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | c5bf029e9fac682c6b85078a61c5c79c
SHA1 | af66656951105e42680dfcc3ec3abd3f4da8fdec
SHA256 | 58b685b2707f273aa76f2e1d45f97b0543a8c4d017cd27f0bdb220e6984cc90e

### <a name="version-10913"></a>Sürüm 1.0.9.13

Karma değerleri yükseltme [paketini 1.0.9.13](https://aka.ms/migrate/col/upgrade_9_13)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 739f588fe7fb95ce2a9b6b4d0bf9917e
SHA1 | 9b3365acad038eb1c62ca2b2de1467cb8eed37f6
SHA256 | 7a49fb8286595f39a29085534f29a623ec2edb12a3d76f90c9654b2f69eef87e

### <a name="version-10911"></a>Sürüm 1.0.9.11

Karma değerleri yükseltme [paketini 1.0.9.11](https://aka.ms/migrate/col/upgrade_9_11)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 0e36129ac5383b204720df7a56b95a60
SHA1 | aa422ef6aa6b6f8bc88f27727e80272241de1bdf
SHA256 | 5f76dbbe40c5ccab3502cc1c5f074e4b4bcbf356d3721fd52fb7ff583ff2b68f

### <a name="version-1097"></a>Sürüm 1.0.9.7

Karma değerleri yükseltme [paketini 1.0.9.7](https://aka.ms/migrate/col/upgrade_9_7)

**Algoritma** | **Karma değeri**
--- | ---
MD5 | 01ccd6bc0281f63f2a672952a2a25363
SHA1 | 3e6c57523a30d5610acdaa14b833c070bffddbff
SHA256 | e3ee031fb2d47b7881cc5b13750fc7df541028e0a1cc038c796789139aa8e1e6

## <a name="continuous-discovery-upgrade-versions"></a>Sürekli bulma: sürüm yükseltme

Henüz hiç yükseltme sürekli bulma Gereci için kullanılabilir.

## <a name="run-an-upgrade"></a>Bir yükseltmeyi çalıştırma

OVA yeniden indirmeden Toplayıcı en son sürüme yükseltebilirsiniz.

1. Aşağıdaki listede en son yükseltme paketini indirin.
2. İndirilen düzeltme güvenli olmasını sağlamak için yönetici komut penceresi açın ve karma ZIP dosyası oluşturmak için aşağıdaki komutu çalıştırın. Oluşturulan karma karşı belirli bir sürüm belirtilen karma değeri ile eşleşmesi gerekir:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    Örnek: **C:\>CertUtil - HashFile C:\AzureMigrate\CollectorUpdate_release_1.0.9.14.zip SHA256)**
3. Toplayıcı gerecini VM zip dosyasını kopyalayın.
4. Zip dosyasını sağ tıklayın > **tümünü Ayıkla**.
5. Sağ **Setup.ps1** > **PowerShell ile Çalıştır**ve yükleme yönergelerini izleyin.


## <a name="next-steps"></a>Sonraki adımlar

- [Daha fazla bilgi edinin](concepts-collector.md) Toplayıcı gerecini hakkında.
- [Değerlendirme çalıştırma](tutorial-assessment-vmware.md) VMware Vm'leri için.
