---
title: Azure geçişi Server değerlendirmesi ile değerlendirme oluşturmak için en iyi yöntemler | Microsoft Docs
description: Azure geçişi Server değerlendirmesi ile değerlendirmeleri oluşturmaya yönelik ipuçları verilmektedir.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: d417efd4abf14247af171ea77b479f590e14fe76
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812970"
---
# <a name="best-practices-for-creating-assessments"></a>Değerlendirmeleri oluşturmaya yönelik en iyi uygulamalar

[Azure geçişi](migrate-overview.md) hub'ı keşfedin, değerlendirin ve uygulamalar ve altyapı iş yükleri Microsoft Azure'a geçirmek için yardımcı araçlar sağlar. Hub, Azure geçişi araçları ve üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri içerir. 

Bu makalede Azure geçişi Server değerlendirmesi aracını kullanarak değerlendirmeler oluştururken en iyi uygulamalar özetlenmektedir. 

## <a name="about-assessments"></a>Değerlendirmeler hakkında

Azure geçişi Server değerlendirmesi ile oluşturduğunuz değerlendirmeleri veri noktası zamanında anlık görüntüsünü ' dir. Azure Geçişi'ndeki değerlendirmeleri iki tür vardır.

**Değerlendirme türü** | **Ayrıntılar** | **Veri**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerini temel alarak önerilerde değerlendirmeleri | Sanal makine boyutu önerisi, CPU ve bellek kullanım verileri temel alır.<br/><br/> Disk türü önerisi (standart veya premium yönetilen diskler), şirket içi disk aktarım hızı ve IOPS hakkında temel alır.
**Olarak-şirket içi** | Değerlendirme önerilerde için performans verilerini kullanmayın. | Sanal makine boyutu önerisi, şirket içi VM boyutuna göre<br/><br> Önerilen disk türü, hangi depolama türünü değerlendirme için seçtiğiniz üzerinde temel alır.

### <a name="example"></a>Örnek
Örnek olarak, % 20 oranında kullanılan ve 8 GB bellek % 10 kullanımı ile dört çekirdek içeren bir şirket içi VM varsa değerlendirmeleri gibi olur:

- **Performans tabanlı değerlendirme**:
    - Çekirdek ve bellek çekirdek (0,8 çekirdek) ve (0,8 GB) bellek kullanımı göre önerir.
    - Değerlendirme % 30'luk varsayılan konfor katsayısı geçerlidir.
    - Sanal makine önerisi: ~1.4 çekirdek (0,8 x1.3) ve ~1.4 GB bellek.
- **Olarak-(şirket içi) değerlendirmesi**:
    -  Bir VM ile dört çekirdek önerir; 8 GB bellek.

## <a name="best-practices-for-creating-assessments"></a>Değerlendirmeleri oluşturmaya yönelik en iyi uygulamalar

Azure geçişi Gereci sürekli olarak şirket içi ortamınızın profilini ve meta verileri ve performans verilerini Azure'a gönderir. Değerlendirme oluşturmak için bu en iyi uygulamaları izleyin:

- **Olarak oluşturma-değerlendirmeleri olan**: Olarak oluşturabileceğiniz-değerlendirmeleri olduğu hemen sonra bulma.
- **Performans tabanlı Değerlendirme Oluştur**: Discovery'yi ayarlama ayarladıktan sonra en az bir performans tabanlı değerlendirmesi çalıştırmadan önce bir gün beklemenizi öneririz:
    - Performans verileri toplama zaman alır. En az bir gün bekledikten, değerlendirmeyi çalıştırmadan önce yeterli performans veri noktası olmasını sağlar.
    - Performans verileri, gereç her 20 saniyede her performans ölçümü için gerçek zamanlı veri noktaları toplar ve bunları tek bir beş dakikalık veri noktaya kadar yapar. Gereç, beş dakikalık veri değerlendirmesi hesaplaması için her saat için Azure'nın üzerine gönderir.  
- **En son verileri alın**: Değerlendirmeler, en son verilerle otomatik olarak güncelleştirilmez. En son verileriyle bir değerlendirme güncelleştirmek için yeniden gerekir. 
- **Süreleri eşleştiğinden emin olun**: Performans tabanlı değerlendirmeleri çalıştırırken profilinizi emin olun, ortamınız için değerlendirme süresi. Örneğin, bir hafta için ayarlanmış bir performans süresini bir değerlendirme oluşturursanız, bulma, toplanacak tüm veri noktaları için başlattıktan sonra en az bir hafta bekleyin gerekir. Değerlendirme, aksi takdirde, beş Yıldıza elde etmezsiniz. 
- **Veri noktaları gözden kaçırmamak**: Aşağıdaki sorunlar, veri noktalarının performans tabanlı değerlendirmede eksik neden olabilir:
    - Vm'leri kapatmak değerlendirme sırasında desteklenir ve performans verileri toplanmaz. 
    - Performans geçmişi temel ay boyunca VM oluşturursanız. Bu sanal makineler için verilerin bir aydan kısa olacaktır. 
    - Değerlendirme bulunduktan hemen sonra oluşturulur veya performans verileri toplama süresi değerlendirme anında eşleşmiyor.

## <a name="best-practices-for-confidence-ratings"></a>Güvenle derecelendirmeleri için en iyi uygulamalar

Performans tabanlı değerlendirmeleri çalıştırdığınızda, 1 yıldızdan 5 yıldızlı için (yüksek) (en düşük) derecelendirme bir güvenle değerlendirmesiyle hak kazanan. Güvenle derecelendirmeleri kullanmak için etkili bir şekilde:
- Azure geçişi Server değerlendirmesi, sanal makine CPU/bellek ve disk IOPS/işleme veri kullanım verileri gerekir.
- Bir VM'ye bağlı her ağ bağdaştırıcısı için Azure geçişi, içeri/dışarı veri ağ gerekir.
- Kullanım verileri vCenter Server'da mevcut değilse Azure Geçişi'nin yaptığı boyut önerisi güvenilir olmayabilir. 

Veri noktaları kullanılabilir yüzdesi bağlı olarak, bir değerlendirme için güvenilirlik derecelendirmelerini aşağıdaki tabloda özetlenmiştir.

   **Veri noktası kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

- Güvenilirlik derecelendirmesi, beş yıldızlı değerlendirme için alırsanız, en az bir gün bekleyin ve ardından değerlendirmeyi yeniden hesaplamak.
- Düşük derecelendirme önerileri boyutlandırma güvenilir olmayabileceği anlamına gelir. Bu durumda, olarak kullanılacak değerlendirme özelliklerini değiştirmenizi öneririz-şirket içi değerlendirmesi.

## <a name="common-assessment-issues"></a>Ortak değerlendirme sorunları

Değerlendirmeler etkileyen bazı ortak bir ortam sorunları nasıl çözeceğinize aşağıda verilmiştir.

###  <a name="out-of-sync-assessments"></a>Eşitleme dışı değerlendirmesi

Ekleme veya bir değerlendirme oluşturduktan sonra makineleri gruptan kaldırdığınızda, oluşturduğunuz değerlendirme işaretlenecek **eşitleme dışı**. Değerlendirmeyi yeniden çalıştırın (**yeniden hesapla**) grubu değişiklikleri yansıtacak şekilde.

### <a name="outdated-assessments"></a>Güncel olmayan değerlendirmeleri

Şirket içi değişiklikleri dikkate alınır bir grup VM'ler varsa, değerlendirme işaretlenmiş **eski**. Değişiklikleri yansıtacak şekilde yeniden değerlendirmeyi çalıştırın.

### <a name="missing-data-points"></a>Veri noktaları

Değerlendirme için pek çok tüm veri noktalarına sahip olmayabilir:

- Sanal makineleri değerlendirme sırasında kapalı ve performans verileri toplanmaz. 
- VM'lerin hangi performans geçmişi temel alır bir ay sırasında oluşturulmuş olabilir, böylece performans verilerine bir aydan kısa olan. 
- Değerlendirme bulunduktan hemen sonra oluşturulur. Belirtilen bir zaman miktarı için performans verilerini toplamak için bir değerlendirmeyi çalıştırmadan önce belirtilen süre beklemeniz gerekir. Örneğin, bir hafta için performans verilerini değerlendirmek istiyorsanız, bir hafta sonra bulma beklemenize gerek. Değerlendirme yoksa beş Yıldıza elde etmezsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

- [Bilgi](concepts-assessment-calculation.md) değerlendirmelerin nasıl hesaplandığı.
- [Bilgi](how-to-modify-assessment.md) kapsamında bir değerlendirmeyi özelleştirme.
