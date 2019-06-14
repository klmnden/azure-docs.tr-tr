---
title: Hata analizi hizmeti genel bakış | Microsoft Docs
description: Bu makalede, Service fabric'te hata analizi hizmeti inducing hataların ve test senaryoları hizmetlerinizi karşı çalıştırmak için açıklanır.
services: service-fabric
documentationcenter: .net
author: anmolah
manager: chackdan
editor: vturecek
ms.assetid: 1f064276-293a-4989-a513-e0d0b9fdf703
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: 3581550779b2387515b4f300d211b4e0a894edc7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60544825"
---
# <a name="introduction-to-the-fault-analysis-service"></a>Hata analizi hizmeti giriş
Hata analizi hizmeti, Microsoft Azure Service Fabric'te yerleşik hizmetlere test etmek için tasarlanmıştır. Hata analizi hizmeti ile anlamlı hataları anlamına ve uygulamalarınızı karşı tam test senaryoları çalıştırın. Bu hataları ve senaryoları çalışma ve çeşitli durumları ve hizmet ömrü boyunca, tüm denetimli, güvenli ve tutarlı bir şekilde yaşar geçişleri doğrulayın.

Eylemler, bunu test etmek için bir hizmet hedefleyen tek hatalarıdır. Bir hizmet Geliştirici karmaşık senaryoları yazmak için bu yapı taşları olarak kullanabilirsiniz. Örneğin:

* Burada bir makine veya sanal makine yeniden durumlar herhangi bir sayıda benzetimini yapmak için bir düğümü yeniden başlatın.
* Yük Dengeleme, yük devretme veya uygulama yükseltmesi benzetimini yapmak için durum bilgisi olan hizmet çoğaltmasını taşıyın.
* Çekirdek kayıp yeni verileri kabul etmek için yeterli "yedekleme" veya "ikincil" çoğaltma olmadığından burada yazma işlemleri devam edemiyor bir durum oluşturmak için bir durum bilgisi olan hizmet olarak çağırır.
* Burada tüm bellek içi durumu tamamen silindikten bir durum oluşturmak için bir durum bilgisi olan hizmet veri kaybı çağırma.

Bir veya daha fazla eylem oluşan karmaşık işlemleri senaryolardır. Hata analizi hizmeti, iki yerleşik tam senaryoları sağlar:

* Kaos senaryosu
* Yük devretme senaryosu

## <a name="testing-as-a-service"></a>Hizmet olarak test etme
Hata analizi hizmeti ile bir Service Fabric kümesini otomatik olarak başlatılan bir Service Fabric sistemi hizmetidir. Bu hizmet, hata ekleme testi senaryosu yürütme ve sistem durumu çözümleme için ana bilgisayarı olarak görev yapar. 

![Hata analizi hizmeti][0]

Hata eylemi veya test senaryosu başlatıldığında hata eylemi veya test senaryosu çalıştırmak için bir komut hata analizi hizmeti için gönderilir. Hata analizi hizmeti durum bilgisi olan, böylelikle, güvenilir bir şekilde hataları ve senaryoları çalıştırabilir ve sonuçları doğrulayın. Örneğin, uzun süre çalışan test senaryosu güvenilir bir şekilde hata analizi hizmeti tarafından gerçekleştirilebilir. Ve hizmet kümesi içinde testleri yürütülmekte olduğundan, küme ve hataları hakkında daha ayrıntılı bilgi sağlamak için hizmetlerinizin durumunu inceleyebilirsiniz.

## <a name="testing-distributed-systems"></a>Dağıtılmış sistemler test etme
Service Fabric, ölçeklenebilir dağıtılmış uygulamaları önemli ölçüde daha kolay yönetme ve yazma iş sağlar. Hata analizi hizmeti, benzer şekilde daha kolay bir dağıtılmış uygulama testi kolaylaştırır. Test ederken çözülmesi gereken üç ana sorunları vardır:

1. Gerçek senaryolar ortaya çıkabilecek benzetimi/oluşturma hataları: Service Fabric önemli yönlerini çeşitli arızalardan kurtarmak dağıtılmış uygulamalar sağlayan biridir. Ancak, uygulamanın Bu hatalardan kurtarma mümkün olduğunu test etmek için bu denetimli bir test ortamında gerçek hata benzetimi/oluşturmak için bir mekanizma ihtiyacımız var.
1. Bağlantılı hataları oluşturabilme özelliği: Sisteminde, ağ hataları ve makine hataları gibi temel hataları ayrı ayrı oluşturmak kolaydır. Çok sayıda gerçek dünyada etkileşim bu tek hata sonucunda oluşabilir senaryoları oluşturma önemsiz değil.
1. Birleşik deneyim çeşitli düzeyde geliştirme ve dağıtım: Çeşitli hataları yapmak için çok sayıda hata ekleme sistemler mevcuttur. Ancak, tüm bu testleri üretim için bunları kullanarak büyük test ortamlarında, aynı testleri çalıştırmak için hazır bir geliştirici senaryolarından taşırken kötü deneyimidir.

Gerekli garanti ile--aynı ortamından üretim kümeleri--test etmek için tamamen hazır bir geliştirici, yapan bir sistem bu sorunları çözmek için birçok mekanizma varken eksik. Hata analizi hizmeti, uygulama geliştiricilerin kendi iş mantığının test edilmesi hakkında yoğunlaşabilirsiniz yardımcı olur. Hata analizi hizmeti temel Dağıtılmış Sistem etkileşimlerine hizmeti test etmek için gerekli tüm yetenekleri sağlar.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Gerçek hata benzetimi/oluşturma
Dağıtılmış bir sistemin arızalarına karşı sağlamlık test etmek için hataları oluşturmak için bir mekanizma ihtiyacımız var. Teoride karşın, bir düğüm kapalı kolay görünüyor gibi bir hata oluşturma Service Fabric çözmeye çalışırken tutarlılık sorunları aynı dizi ulaşmaktan başlar. Örneğin, bir düğüm kapatmak istiyorsanız gerekli iş akışı aşağıda verilmiştir:

1. İstemciden bir kapatma düğüm isteği göndermektir.
1. İstek için doğru düğümü gönderin.
   
    a. Düğüm bulunamadı, başarısız olması.
   
    b. Düğüm bulunursa, yalnızca düğümün kapatma durumunda döndürmelidir.

Test perspektifini hatayı doğrulamak için test korunamaması kopyaladığınızda, hatanın gerçekten gerçekleştiğinden bilmesi gerekir. Ya da olan Service Fabric sağlayan garanti düğümü geçer veya zaten çöktü komut düğümü erişildiğinde. Her iki durumda da test doğru durumunu neden ve başarılı veya doğru şekilde kendi doğrulama başarısız olması gerekir. Hata aynı yapmak için Service Fabric dışında uygulanan bir sistem, birçok ağ, donanım ve yazılım konuları, önceki garanti vermesini engeller oluşabilir. Önce belirtilen sorunları varlığında Service Fabric küme durumu sorunları çözmenin yeniden yapılandırır ve bu nedenle hata analizi hizmeti hala garanti doğru ortaklık kümesi vermek mümkün olacaktır.

### <a name="generating-required-events-and-scenarios"></a>Gerekli olayları ve senaryolar oluşturma
Tutarlı bir şekilde gerçek hata benzetimi başlamak zor olsa da, ilişkili hataları oluşturmak için bile tougher yeteneğidir. Örneğin, aşağıdakiler meydana geldiğinde kalıcı bir durum bilgisi olan hizmet olarak veri kaybı olur:

1. Yalnızca bir yazma çekirdek çoğaltmaların yakalanır çoğaltma. Tüm İkincil çoğaltma birincil lag.
1. Yazma çekirdek (nedeniyle, bir kod paketi veya gitme düğümü) giderek çoğaltmaları nedeniyle düşer.
1. Çoğaltma verileri (disk bozulması veya makine görüntüsü yeniden oluşturuluyor nedeniyle) kayıp olduğundan yazma çekirdek geri gelmesi olamaz.

Bu bağlantılı hataları gerçek dünyada, ancak sık olarak tek tek hatalar olarak değil, ortaya çıkar. Üretimde gerçekleşmeden bu senaryolar için test etmek için kritik önem taşır. Denetlenen durumlarda (ortasında Destesi tüm mühendisleri gün) üretim iş yükleri ile bu senaryolarının benzetimini yapın özelliği daha da önemlidir. Bu üretim saat 2: 00'da ilk kez ortaya olması çok iyidir

### <a name="unified-experience-across-different-environments"></a>Farklı ortamlar genelinde birleşik bir deneyim
Uygulama deneyimleri, bir geliştirme ortamı için testler için diğeri üretim için üç farklı kümesi oluşturmak için geleneksel olarak olmuştur. Model şöyleydi:

1. Geliştirme ortamında her bir yöntem, birim testleri izin durumu geçişleri üretir.
1. Test ortamında çeşitli hata senaryolarını alıştırma uçtan uca testleri izin hatası üretir.
1. Üretim ortamına doğal olmayan hataları önlemek ve İnsan son derece hızlı yanıt hatası olduğundan emin olmak için pristine tutun.

Hata analizi hizmeti aracılığıyla Service fabric'teki Biz bu geri dönüş ve geliştirici ortamından üretim için aynı yöntemi kullanmak önerdiği. Bunu yapmanın iki yolu vardır:

1. Anlamına denetimli hataları için hata analizi hizmeti API'lerinden one-box ortamı tüm üretim kümeleri için kullanın.
1. Küme hata otomatik endüksiyon neden olan bir humması vermek için hata analizi hizmeti otomatik hataları oluşturmak için kullanın. Yapılandırma yoluyla hata oranı denetlenmesi, aynı hizmetin farklı ortamlarda farklı bir şekilde test edilmesini sağlar.

Service Fabric ile hataları ölçeğini farklı ortamlarda farklı olacaktır ancak gerçek mekanizmaları aynı olacaktır. Bu çok kod dağıtım daha hızlı işlem hattı ve gerçek dünyadaki yüklerin hizmetler test etme olanağı sağlar.

## <a name="using-the-fault-analysis-service"></a>Hata analizi hizmeti kullanma
**C#**

Hata analizi hizmeti özellikler System.Fabric ad alanında Microsoft.ServiceFabric NuGet paketini alır. Hata analizi hizmeti özellikleri kullanmak için projenize bir başvuru olarak nuget paketini ekleyin.

**PowerShell**

PowerShell kullanmak için Service Fabric SDK'sını yüklemelisiniz. ServiceFabric PowerShell modülü, SDK'yı yükledikten sonra kullanabilmeniz için yüklenen otomatik olarak.

## <a name="next-steps"></a>Sonraki adımlar
Gerçek anlamda bulut ölçekli hizmetler oluşturmak için önce ve dağıtımdan sonra hizmetleri gerçek dünya hataları dayanabilir sağlamak önemlidir. Hizmetleri dünyanın hemen, hızlı bir şekilde yenilik yapın ve kod üretim için hızlı bir şekilde taşıma olanağı çok önemlidir. Hata analizi hizmeti hizmet geliştiricileri, tam olarak bunu yapmak için yardımcı olur.

Uygulamalarınızı ve hizmetlerinizi yerleşik kullanarak test etmeye [test senaryolarında](service-fabric-testability-scenarios.md), ya da kendi test senaryoları kullanarak Yazar [hata eylemleri](service-fabric-testability-actions.md) hata analizi hizmeti tarafından sağlanan.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
