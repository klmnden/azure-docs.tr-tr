---
title: Azure için Avere vFXT
description: HPC için bulut önbellek katmanı olan Azure için Avere vFXT'ye giriş
author: ekpgh
ms.service: avere-vfxt
ms.topic: overview
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 724352ae9f7c66dfeab293473ea79438e3b36254
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60409431"
---
# <a name="what-is-avere-vfxt-for-azure"></a>Azure için Avere vFXT nedir? 

Azure için Avere vFXT, yoğun veri kullanan yüksek performanslı bilgi işlem (HPC) görevlerine yönelik bir dosya sistemi önbellek çözümüdür. Bulut bilişimin ölçeklenebilirlik avantajlarından faydalanarak şirket içi donanımlarınızda bulunan veriler dahil olmak üzere tüm verilerinize ihtiyaç duyulduğu yerde ve zamanda erişim sağlanmasını mümkün kılar.

Avere vFXT şu yaygın bilgi işlem senaryolarını destekler: 

* Karma bulut mimarisi: Azure için Avere vFXT dosyaları taşımak zorunda kalmadan bulut avantajlarını sağlar bir donanım depolama sistemi çalışabilirsiniz. 
* Bulut Patlaması: Azure için Avere vFXT yardımcı olabilir, verilerinizi tek bir proje için buluta taşıyın veya "kaldırma ve kaydırma" iş akışının tamamını kalıcı olarak. 

![Blob depolama alanına ve şirket içi veri merkezine bağlı bir Azure aboneliğindeki Avere vFXT sisteminin ayrıntılarını gösteren diyagram](media/avere-vfxt-hybrid.png)

Azure için Avere vFXT, şu durumlar için ideal çözümdür: 

* HPC iş yükleri için yoğun okuma işlemleri
* Ortak NFS protokolünü kullanan uygulamalar
* 1000-40.000 CPU çekirdeğine sahip işlem grupları
* Şirket içi NAS donanımı, Azure Blob depolama alanı veya ikisiyle birden tümleştirme

Daha fazla bilgi için <https://azure.microsoft.com/services/storage/avere-vfxt/> adresini ziyaret edin.

## <a name="who-uses-avere-vfxt-for-azure"></a>Azure için Avere vFXT kimlere yöneliktir? 

Avere vFXT, okuma açısından yoğun bilgi işlem görevlerinde yardımcı olabilir:

### <a name="visual-effects-rendering"></a>Görsel efekt oluşturma 

Avere vFXT kümesi medya ve eğlence sektöründe zaman açısından kritik projeler için veri erişimini hızlandırabilir. Azure'da daha fazla önbellek alanı ve işlem düğümü ekleme imkanına sahip olduğunuz için büyük projeleri verimli bir şekilde işleyebilirsiniz. 

### <a name="life-sciences"></a>Yaşam bilimleri 

Avere vFXT, araştırmacıların ikincil analiz iş akışlarını Azure üzerinde çalıştırmasını ve bulundukları konumdan bağımsız olarak genomik verilere erişmesini sağlar.

Avere vFXT kümeleri ilaç araştırmalarında araştırmacıların ilaç hedef etkileşimlerini tahmin etmesine ve araştırma verilerini analiz etmesine yardımcı olarak ilaç bulma sürecini hızlandırmak için kullanılabilir.

### <a name="financial-services-analytics"></a>Finansal hizmetler analizi

Avere vFXT kümesi nicel analiz hesaplamalarını hızlandırmaya yardımcı olarak finansal kuruluşlara stratejik karar alma konusunda daha iyi içgörüler sunabilir. 

## <a name="features-and-specifications"></a>Özellikler ve belirtimler

Avere vFXT sistemi, bir küme halinde yapılandırılmış üç veya daha fazla sanal kenar dosya düğümünden oluşur. İstemci makinelerin yanında konumlandırılarak doğrudan depolama alanını takmak yerine kümeyi takma seçeneği sunar. 

Avere vFXT kümesi, istenen dosyaları önbelleğe alır. Tekrarlanan istekler %80'den daha fazla önbellekten sunulabilir.

### <a name="compatibility"></a>Uyumluluk 

* NetApp veya Dell EMC Isilon donanımsal NAS sistemleri ile uyumludur
* Azure Blob ile uyumludur
* NFSv3 veya SMB2 protokolünü kullanır

Avere vFXT şu Azure kaynaklarını kullanır: 

|Azure bileşeni|   |
|----------|-----------|
|Sanal makineler|3 veya daha fazla E32s_v3|
|Premium SSD depolama alanı|200 GB işletim sistemi alanına ek olarak düğüm başına 1 TB-4 TB önbellek alanı |
|Depolama hesabı (isteğe bağlı) |v2|
|Veri arka uç depolama alanı (isteğe bağlı) | Boş bir LRS Blob kapsayıcısı |

## <a name="next-steps"></a>Sonraki adımlar

Kendi Avere vFXT dağıtımınızı oluşturmaya başlamak için aşağıdaki bağlantılardan faydalanabilirsiniz. 

* [Sisteminizi planlama](avere-vfxt-deploy-plan.md)
* [Dağıtıma genel bakış](avere-vfxt-deploy-overview.md)
* [vFXT ortamını oluşturma](avere-vfxt-deploy.md)
