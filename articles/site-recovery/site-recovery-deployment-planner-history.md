---
title: Azure Site Recovery dağıtım Planlayıcısı sürüm geçmişi
description: Farklı Site Recovery dağıtım Planlayıcısı Sürüm düzeltmeleri ve bilinen sınırlamalar, yayın tarihleri birlikte bilinen.
services: site-recovery
author: Daya-Patil
manager: carmonm
ms.topic: article
ms.service: site-recovery
ms.date: 04/24/2019
ms.author: dapatil
ms.openlocfilehash: 2edf7ce3be1402a497ceab5b826a89ee43c5c39b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64927380"
---
# <a name="azure-site-recovery-deployment-planner-version-history"></a>Azure Site Recovery dağıtım Planlayıcısı sürüm geçmişi

Bu makalede, Azure Site Recovery dağıtım Planlayıcısı bilinen sınırlamalar her ve bunların yayın tarihleri düzeltmelerin yanı sıra tüm sürümlerini geçmişini sağlar.

## <a name="version-24"></a>2\.4 sürüm

**Yayın Tarihi: 17 Nisan 2019**

**Düzeltmeleri:**

- Özellikle yerelleştirme tabanlı hataları işleme işletim sistemi uyumluluğu İyileştirildi.
- En fazla 20 MB/sn veri eklenen Vm'lerle uyumluluk denetim listesine (erime) hızı değiştirin.
- Geliştirilmiş hata iletileri
  - VCenter 6.7 desteği eklendi.
  - Windows Server 2019 ve Red Hat Enterprise Linux (RHEL) iş istasyonu desteği eklendi.



## <a name="version-23"></a>2\.3 sürümü

**Yayın Tarihi: 3 Aralık 2018'e**

**Düzeltmeleri:**

- Dağıtım Planlayıcısını sağlanan hedef konumu ve abonelik ile bir rapor oluşturulmasını önleyen bir sorun düzeltildi.

## <a name="version-22"></a>Sürüm 2.2 

**Yayın Tarihi: 25 Nisan 2018**

**Düzeltmeleri:**

- GetVMList işlemler:
  - GetVMList belirtilen klasör mevcut değilse başarısız olmasına neden olan bir sorun düzeltildi. Bunu şimdi varsayılan dizini oluşturur veya outputfile parametresinde belirtilen dizin oluşturur.
  - Daha ayrıntılı hata nedeniyle GetVMList eklendi.
- Dağıtım Planlayıcısı raporun uyumlu VM'ler sayfasında sütun olarak VM türü bilgiler eklendi.
- Hyper-V Azure'a olağanüstü durum kurtarma için:
  - Hariç tutulan Vm'lerle paylaşılan VHD'ler ve doğrudan geçiş diskleri öğesinden profil oluşturma. Startprofiling işlem konsolunda dışlanan VM'lerin listesini gösterir.
  - Uyumsuz VM'lerin listesini 64'ten fazla disklere Vm'lerle eklendi.
  - Delta çoğaltma (DR) sıkıştırma faktörü ve ilk çoğaltmayı (IR) güncelleştirildi.
  - SMB depolama alanı için sınırlı destek eklendi.

## <a name="version-21"></a>Sürüm 2.1

**Yayın Tarihi: 3 Ocak 2018**

**Düzeltmeleri:**

- Excel raporu güncelleştirildi.
- GetThroughput işlemi düzeltilen hatalar.
- Profil veya raporu oluşturmak için VM sayısını sınırlamak için seçeneği eklendi. Varsayılan, 1.000 VM sınırıdır.
- Vmware'den Azure'a olağanüstü durum kurtarma için:
  - Windows Server 2016 VM uyumsuz tablosuna giden bir sorun düzeltildi. 
  - Uyumluluk iletilerini Genişletilebilir Bellenim Arabirimi (EFI) Windows Vm'leri için güncelleştirildi.
- Güncelleştirilmiş VMware VM başına sınır, Azure ve Hyper-V'den azure'a VM veri değişim sıklığı. 
- Geliştirilmiş Güvenilirlik VM listesi dosyasının ayrıştırma.

## <a name="version-201"></a>Sürüm 2.0.1

**Yayın Tarihi: 7 Aralık 2017**

**Düzeltmeleri:**

- Ağ bant genişliğini iyileştirmek için öneri eklendi.

## <a name="version-20"></a>Sürüm 2.0

**Yayın Tarihi: 28 Kasım 2017**

**Düzeltmeleri:**

- Hyper-V için Azure'a olağanüstü durum kurtarma için eklenen destek.
- Ek maliyet hesaplayıcı.
- Eklenen işletim sistemi sürüm denetimi vmware'den Azure'a olağanüstü durum kurtarma VM koruması için uyumsuz veya uyumlu olup olmadığını belirlemek için. Aracı için bu VM vCenter sunucusu tarafından döndürülen işletim sistemi sürüm dizesi kullanır. Konuk işletim sistemi sürümü olduğundan VMware VM oluşturulurken kullanıcı seçildi.

**Bilinen sınırlamalar:**

- Hyper-V'den Azure'a olağanüstü durum kurtarma, VM adı içeren ile gibi karakterler: `,`, `"`, `[`, `]`, ve ``` ` ``` desteklenmez. Profili, rapor oluşturma başarısız olur veya hatalı bir sonuç olur.
- Vmware'den Azure'a olağanüstü durum kurtarma için adı virgül içeren VM desteklenmiyor. Profili, rapor oluşturma başarısız veya hatalı bir sonuç olur.

## <a name="version-131"></a>Sürüm 1.3.1

**Yayın Tarihi: 19 Temmuz 2017** 

**Düzeltmeleri:**

- Rapor oluşturma büyük diskler (> 1 TB) için destek eklendi. 1 TB'den büyük disk boyutlarına sahip sanal makineler için çoğaltma planlamak için dağıtım Planlayıcısı'nı kullanabilir (4095 GB'ye kadar).
[Azure Site Recovery'de büyük disk desteği](https://azure.microsoft.com/blog/azure-site-recovery-large-disks/)

## <a name="version-13"></a>1\.3 sürümü

**Yayın Tarihi: 9 Mayıs 2017**

**Düzeltmeleri:**

- Yönetilen disk rapor oluşturma için destek eklendi. Yönetilen disk için yük devretme/yük devretme testi seçilirse, bir tek bir depolama hesabına yerleştirilebilecek sanal makine sayısı temel alınarak hesaplanır.

## <a name="version-12"></a>Sürüm 1.2

**Yayın Tarihi: 7 Nisan 2017**

**Düzeltmeleri:**

- Eklenen önyükleme türü (BIOS veya EFI) denetimleri VM koruması için uyumsuz veya uyumlu olup olmadığını belirlemek her bir VM için.
- Eklenen işletim sistemi, uyumlu VM'ler ve uyumsuz VM'ler çalışma sayfalarında her sanal makine bilgilerini yazın.
- GetThroughput işlemi ABD hükümeti ve Çin Microsoft Azure bölgeleri için destek eklendi.
- vCenter ve ESXi Server için birkaç tane daha ön koşul denetimi eklendi.
- Yanlış raporun yerel ayarları İngilizce için ayarlandığında oluşturulan bir sorun düzeltildi.

## <a name="version-11"></a>Sürüm 1.1

**Yayın Tarihi: 9 Mart 2017**

**Düzeltmeleri:**

- Çeşitli vCenter ESXi ana bilgisayarları üzerinde aynı ad veya IP adresine sahip iki veya daha fazla sanal olduğunda Vm'lerinin profilini oluşturmak önleyen bir sorun düzeltildi.
- Kopyalama ve arama uyumlu VM'ler ve uyumsuz VM'ler çalışma sayfaları için devre dışı bırakılmasına neden olan bir sorun düzeltildi.

## <a name="version-10"></a>Sürüm 1.0

**Yayın Tarihi: 23 Şubat 2017**

**Bilinen sınırlamalar:**

- Yalnızca Vmware'den Azure'a olağanüstü durum kurtarma senaryoları için destekler. Hyper-V'den Azure'a olağanüstü durum kurtarma senaryoları, kullanın [Hyper-V kapasite Planlayıcısı aracını](./site-recovery-capacity-planning-for-hyper-v-replication.md).
- US Government ve Çin Microsoft Azure bölgeleri için GetThroughput işlemini desteklemiyor.
- Aracı dizeye profili vCenter sunucusunun çeşitli ESXi ana bilgisayarları üzerinde aynı ad veya IP adresine sahip iki veya daha fazla VM varsa VM'ler.
Bu sürümde, araç VMListFile dosyasındaki yinelenen sanal makine adları veya IP adresleri için profil oluşturmayı atlar. Bunun geçici çözümü, sanal makine profillerinin vCenter sunucusu yerine bir ESXi ana bilgisayarı kullanılarak oluşturulmasıdır. Her ESXi konağı için bir örnek çalıştırmak için emin olun.
