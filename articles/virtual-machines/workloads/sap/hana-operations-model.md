---
title: SAP hana (büyük örnekler) azure'da işlem modeli | Microsoft Docs
description: SAP HANA (büyük örnekler) azure'da işlem modeli.
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 36a648e2d46cce96a8ff663f45ccf45326898a84
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60477886"
---
# <a name="operations-model-and-responsibilities"></a>İşlem modeli ve sorumluluklar

SAP HANA (büyük örnekler) azure'da ile sağlanan hizmeti, Azure Iaas Hizmetleri ile hizalanır. SAP HANA için en iyi duruma getirilmiş yüklü bir işletim sistemi bir HANA büyük örneği örneğini sahip olursunuz. Azure Iaas Vm'lerle, işletim sistemini sağlamlaştırma, ek yazılım yüklemeniz, HANA yükleme, işletim sistemi ve HANA işletim ve işletim sistemi ve HANA güncelleştirme görevlerin çoğunu gibidir sizin sorumluluğunuzdadır. Microsoft işletim sistemi güncelleştirmeleri veya HANA güncelleştirmeleri size zorlamaz.

![SAP hana (büyük örnekler) azure'da sorumlulukları](./media/hana-overview-architecture/image2-responsibilities.png)

Diyagramda gösterildiği gibi SAP HANA (büyük örnekler) azure'da Iaas sunan çok kiracılı ' dir. Çoğunlukla, sorumluluk bölümünün OS altyapısı sınırında ' dir. Microsoft, hizmet işletim sisteminin çizginin altındaki tüm yönlerini sorumludur. Bu satırın yukarısında hizmetin tüm boyutlarından sorumlu olursunuz. İşletim sistemi sizin sorumluluğunuzdur. Uyumluluk, güvenlik, uygulama yönetimi, temel ve işletim sistemi yönetim için en güncel şirket içi yöntemler uyguluyor kullanmaya devam edebilirsiniz. Sistemleri, ağınızdaki tüm ilgili oldukları gibi görünür.

Bu hizmet, alanları, en iyi sonuçlar için temel alınan altyapı özellikleri kullanmak için Microsoft ile çalışmak için gereken şekilde SAP HANA için optimize edilmiştir.

Aşağıdaki listede her katmanları ve sizin Sorumluluklarınız daha fazla ayrıntı sağlar:

**Ağ**: Tüm iç ağlar için çalışan SAP HANA büyük örneği damgasında. Sizin sorumluluğunuzdadır, VM'ler depolama, örnekleri (için ölçek genişletme ve diğer işlevler), yatay bağlantısı ve SAP uygulama katmanı barındırıldığı Azure bağlantısı arasında bağlantı erişimi içerir. Ayrıca, olağanüstü durum kurtarma amacıyla çoğaltma için Azure veri merkezleri arasında WAN bağlantısı içerir. Tüm ağlar, Kiracı tarafından bölümlenir ve uygulanan hizmet kalitesi sahip.

**Depolama**: Sanallaştırılmış depolama anlık görüntüleri yanı sıra, SAP HANA sunucuları tarafından gerekli olan tüm birimler için bölümlenmiş. 

**Sunucuları**: SAP HANA veritabanlarını çalıştırmak için ayrılmış fiziksel sunuculara kiracılara atanmış. Sunucuları türü miyim sınıfı soyutlanır donanım SKU'lar. Bu sunucu türleri ile sunucu yapılandırmasını toplanır ve başka bir fiziksel donanıma taşınabilir bir fiziksel donanım profilleri, saklanır. Böyle bir (el ile) taşıma profili işlem tarafından Azure hizmet onarımı için biraz karşılaştırılabilir. Type II sınıfı SKU'ları sunucularında, böyle bir özellik sunmamaktadır.

**SDDC**: Verileri yönetmek için kullanılan yönetim yazılımı, yazılım tanımlı varlıklar olarak merkezlendirir. Microsoft, Ölçek, kullanılabilirlik ve performansı artırmak için kaynakları havuza sağlar.

**İŞLETİM SİSTEMİ**: İşletim sistemi (SUSE Linux veya Red Hat Linux) seçtiğiniz sunucular üzerinde çalışıyor. İle sağlanan işletim sistemi görüntüleri, SAP HANA çalıştırmak için tek tek Linux satıcı tarafından Microsoft'a sağlanmadı. SAP HANA için iyileştirilmiş görüntü için Linux satıcıyla aboneliğiniz olmalıdır. Görüntüleri işletim sistemi satıcıyla birlikte kaydetmek için sorumlu olursunuz. 

Microsoft tarafından devreden MultiPath noktasından herhangi ek Linux işletim sistemi düzeltme eki uygulama için sorumlu olursunuz. Bu düzeltme eki uygulama olabilecek başarılı bir SAP HANA yüklemesi için gerekli ve belirli kullanıcıların SAP HANA Linux satıcı tarafından dahil olmayan ek paketleri içeren işletim sistemi görüntüleri en iyi duruma getirilmiş. (Daha fazla bilgi için SAP'nin SAP notları ve HANA yükleme belgelerine bakın.) 

Arızasını veya işletim sistemi iyileştirme ve belirli sunucu donanımı göre sürücülerini owing to işletim sistemi yaması için sorumlu olursunuz. Ayrıca güvenlik veya işlev işletim sistemi düzeltme eki için sizin sorumluluğunuzdadır. 

Sizin sorumluluğunuzdadır, izleme ve kapasite planlama de içerir:

- CPU kaynak kullanımı.
- Bellek tüketimi.
- Disk birimleri boş alan, IOPS ve gecikme süresi için ilgili.
- HANA büyük örneği ile SAP uygulama katmanı arasında ağ birim trafiği.

HANA büyük örneği temel altyapısını yedekleme ve geri yükleme, işletim sistemi birimi için işlevsellik sağlar. Bu işlevleri kullanarak da sizin sorumluluğunuzdur.

**Ara yazılım**: SAP HANA örneği, öncelikli olarak. Yönetim, işlemler ve izleme sizin sorumluluğunuzdadır. Sağlanan işlevselliği, yedekleme ve geri yükleme, olağanüstü durum kurtarma amacıyla depolama anlık görüntüleri kullanmak için kullanabilirsiniz. Bu özellikler altyapısı tarafından sağlanır. Sizin Sorumluluklarınız de yüksek kullanılabilirlik veya depolama anlık görüntüleri başarıyla gerçekleştirilip gerçekleştirilmediğini belirlemek için izleme ve bunları yararlanarak bu özelliklere sahip, olağanüstü durum kurtarma tasarımı içerir.

**Veri**: SAP HANA tarafından yönetilen veri ve yedeklemeler gibi diğer veri birimlerinde bulunan dosya veya dosya paylaşımları. Sizin Sorumluluklarınız birimlerde içeriği yönetme ve boş disk alanı izleme içerir. Ayrıca yedeklemeler, birimlerin ve depolama anlık görüntüleri başarıyla yürütüldü izlemekten sorumlu olursunuz. Olağanüstü durum kurtarma sitelerinde veri çoğaltmasının başarılı yürütme Microsoft sorumluluğundadır.

**Uygulamalar:** SAP uygulama örnekleri veya SAP olmayan uygulamalarınızı, söz konusu uygulamaların uygulama katmanı söz konusu olduğunda. Sizin Sorumluluklarınız dağıtım, yönetim, işlemler ve bu uygulamaların izlenmesi içerir. CPU kaynak tüketimi, bellek tüketimi, Azure depolama tüketimi ve sanal ağ içinde ağ bant genişliği tüketimi, kapasite planlaması için sorumlu olursunuz. Siz de Kapasite (büyük örnekler) Azure üzerinde SAP HANA için kaynak tüketimi sanal ağlardan planlama sorumlu olursunuz.

**WAN**: , Şirket içinden Azure'a dağıtımlar iş yükleri için bağlantı. HANA büyük örneği olan tüm müşterileri, Azure ExpressRoute için bağlantı kullanın. Bu bağlantının SAP HANA (büyük örnekler) Azure çözüm üzerinde bir parçası değil. Bu bağlantının kurulumu için sorumlu olursunuz.

**Arşiv**: Depolama hesaplarında kendi yöntemlerini kullanarak verilerin kopyaları arşivlemek tercih edebilirsiniz. Arşivleme, yönetim, uyumluluk, maliyetleri ve işlemleri gerektirir. Arşiv kopyalarını oluşturma ve yedekleri Azure ve bunları uyumlu bir şekilde depolamak için sorumlu olursunuz.

Bkz: [(büyük örnekler) Azure üzerinde SAP HANA için SLA](https://azure.microsoft.com/support/legal/sla/sap-hana-large/).

**Sonraki adımlar**
- Başvuru [azure'da SAP HANA (büyük örnekler) mimarisi](hana-architecture.md)
