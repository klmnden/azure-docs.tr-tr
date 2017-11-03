---
title: "Hata analizi hizmetine genel bakış | Microsoft Docs"
description: "Bu makalede, hataları inducing ve test senaryoları hizmetlerinizi karşı çalıştırmak için Service Fabric hataya analiz hizmetinde açıklanmaktadır."
services: service-fabric
documentationcenter: .net
author: anmolah
manager: timlt
editor: vturecek
ms.assetid: 1f064276-293a-4989-a513-e0d0b9fdf703
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: f275fa5d3d6d727b016e55c188321d7e68091a33
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-the-fault-analysis-service"></a>Hata analizi hizmetine giriş
Hata analizi hizmeti Microsoft Azure Service Fabric üzerinde oluşturulmuş Hizmetleri test etmek için tasarlanmıştır. Hata analizi hizmeti ile anlamlı hataları anlamına ve uygulamalarınızı karşı tam test senaryoları çalıştırın. Bu hataları ve senaryoları çalışma ve çok sayıda durumları ve yaşam süresi, tüm denetimli, güvenli ve tutarlı bir şekilde boyunca bir hizmet yaşar geçişleri doğrulayın.

Eylemler, test etmek için bir hizmet hedefleme tek tek hatalarıdır. Bir hizmet Geliştirici yapı taşı olarak karmaşık senaryolarda yazmak için kullanabilirsiniz. Örneğin:

* Burada bir makine ya da VM yeniden durumlarda herhangi bir sayıda benzetimini yapmak için bir düğümü yeniden başlatın.
* Yük Dengeleme, yük devretme veya uygulama yükseltme benzetimini yapmak için durum bilgisi olan hizmet çoğaltmasını taşıyın.
* Çekirdek kayıp yeni verileri kabul etmek için yeterli "yedek" veya "ikincil" çoğaltmaları olmadığından burada yazma işlemleri devam edemiyor bir durum oluşturmak için durum bilgisi olan bir hizmet olarak çağırır.
* Burada tüm bellek içi durumu tamamen silinmeden çıkışı bir durum oluşturmak için bir durum bilgisi olan hizmet veri kaybı çağırır.

Bir veya daha fazla eylemlerini oluşan karmaşık işlemleri senaryolar verilmiştir. Hata analizi hizmeti iki yerleşik tam senaryoları sağlar:

* Chaos senaryosu
* Yük devretme senaryosu

## <a name="testing-as-a-service"></a>Bir hizmet olarak test etme
Hataya analiz hizmeti Service Fabric kümesi ile otomatik olarak başlatılan bir Service Fabric sistem hizmetidir. Bu hizmet, ana bilgisayar arıza ekleme, test senaryosu yürütme ve sistem durumu çözümleme için olarak görev yapar. 

![Hata analizi hizmeti][0]

Bir hata eylemi veya test senaryosu başlatıldığında, bir komut hata eylemi veya test senaryoları çalıştırılacağını hataya Analysis Service olarak gönderilir. Böylece, güvenilir bir şekilde hataları ve senaryoları çalıştırabilir ve sonuçlarını doğrulamak hataya analiz durum bilgisi olan bir hizmettir. Örneğin, bir uzun süre çalışan testi senaryosu güvenilir bir şekilde hata analizi hizmeti tarafından çalıştırılabilir. Ve testleri kümesi içinde yürütülmekte olduğundan, hizmet küme ve hataları hakkında daha ayrıntılı bilgi sağlamak için hizmetlerinizi durumunu inceleyebilirsiniz.

## <a name="testing-distributed-systems"></a>Dağıtılmış sistemlerin test etme
Service Fabric yazma ve dağıtılmış ölçeklenebilir uygulamalar önemli ölçüde daha kolay yönetme işi yapar. Benzer şekilde daha kolay bir dağıtılmış uygulamayı test hata analizi hizmeti sağlar. Test ederken çözülmesi gereken üç ana sorunları vardır:

1. Gerçek dünya senaryolarında oluşabilecek hatalar benzetimi/oluşturuluyor:, Service Fabric, önemli yönlerinden biri çeşitli arızalardan kurtarmak dağıtılmış uygulamaları etkinleştirir. Ancak, uygulama bu arızalardan kurtarmak mümkün olup olmadığını test için bu gerçek hataları denetimli test ortamında benzetimini/oluşturmak için bir mekanizma gerekiyor.
2. Bağıntılı hataları oluşturma yeteneği: sisteminde, ağ hataları ve makine hataları gibi temel hataları ayrı ayrı oluşturmak kolaydır. Bu tek tek başarısızlık etkileşimleri sonucunda gerçek dünyadaki oluşabilir senaryoları önemli sayıda oluşturma Önemsiz olmayan.
3. Geliştirme ve dağıtım çeşitli düzeylerde arasında birleşik deneyim: hataları çeşitli türlerde yapabilmeniz için birçok hata ekleme sistemi vardır. Ancak, tüm bunlar büyük sınama ortamlarında, bunları üretimde sınamaları kullanarak aynı testleri çalıştırma için bir kutusu Geliştirici senaryolarından taşırken, zayıf deneyimidir.

Gerekli garanti ile--aynı ortamından üretim kümeleri--test etmek için bu süreç boyunca tüm kutusu bir geliştirici, yapan bir sistem bu sorunları çözmek için birçok mekanizmaları olduğunuzda eksik. Hata analizi hizmeti uygulama geliştiricileri kendi iş mantığı sınama odaklanmasına yardımcı olur. Hata analizi hizmeti hizmetinin temel Dağıtılmış Sistem etkileşimlerine test etmek için gerekli tüm özellikleri sağlar.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Gerçek dünya hatası senaryoları benzetimini yapma ve oluşturma
Dağıtılmış bir sistemde hatalarına karşı sağlamlık test için hataları oluşturmak için bir mekanizma gerekiyor. Teorik karşın, bir düğüm aşağı kolay görünüyor gibi bir hata oluşturma Service Fabric çözmeye çalışırken tutarlılık sorunları aynı kümesini basarsa başlar. Örnek olarak, bir düğüm kapatmaya istiyorsanız gerekli iş akışı aşağıda verilmiştir:

1. İstemciden kapatma düğümü isteği gönderin.
2. İstek için doğru düğümü gönderin.
   
    a. Düğümü bulunamadı, başarısız olması.
   
    b. Düğüm bulunursa, yalnızca düğüm kapatma durumunda döndürmelidir.

Bir test perspektifini hatasından doğrulamak için test bu hatanın kopyaladığınızda, hata gerçekte olur, bilmek ister. Service Fabric, her iki sağlar garantisi düğüm gidecek veya komutu düğüm erişildiğinde zaten oldu. Her iki durumda da test doğru durumunu neden ve başarılı veya doğru şekilde kendi doğrulama başarısız yapabiliyor olmanız gerekir. Hataları aynı kümesini yapmak için Service Fabric dışında uygulanan bir sistem, birçok ağ, donanım ve önceki garanti sağlayan önleyen yazılım sorunları oluşabilir. Önce belirtilen sorunları varlığında Service Fabric sorunlarını geçici olarak çözmek için küme durumunu yeniden yapılandıracak ve bu nedenle hata analizi hizmeti hala garanti doğru yetenek kümesini vermek olacaktır.

### <a name="generating-required-events-and-scenarios"></a>Gerekli olayları ve senaryolar oluşturma
Gerçek dünya kesintisi tutarlı bir şekilde benzetimi başlamak zor olsa da, bağıntılı hataları oluşturmak için bile tougher yeteneğidir. Örneğin, aşağıdaki olacaklar veri kaybı bir durum bilgisi olan kalıcı hizmetinde olur:

1. Yalnızca bir yazma çekirdek çoğaltmalarının yakalandı çoğaltma. Tüm İkincil çoğaltmaları birincil öteleme.
2. (Nedeniyle, bir kod paketi veya gitme düğüm) giderek çoğaltmaları nedeniyle yazma çekirdek arıza.
3. Çoğaltma verileri (disk bozulması veya makine yeniden görüntüsünü oluşturuyor nedeniyle) kayıp olduğundan yazma çekirdek geri gelmesi olamaz.

Bu ilişkili hatalar, gerçek dünyadaki ancak sık olarak tek tek hataları olarak değil gerçekleşir. Üretimde gerçekleşmeden önce bu senaryolar için test etme olanağı önemlidir. Bu senaryolar (ortasında deste üzerindeki tüm mühendisleri gün) denetimli durumlarda üretim iş yükleri ile benzetimini olanağı daha da önemlidir. Bu ilk kez üretim saat 2: 00'da gerçekleşir olması daha iyi

### <a name="unified-experience-across-different-environments"></a>Farklı ortamlar üzerinde birleşik deneyim
Uygulama, geleneksel deneyimleri, bir geliştirme ortamı için testler için diğeri üretim için üç farklı kümesi oluşturmak için bırakıldı. Model oluştu:

1. Geliştirme ortamında birim testleri tek tek yöntemlerin izin durumu geçişleri üretir.
2. Test ortamında, çeşitli hatası senaryoları çalışma uçtan uca testleri izin verilecek üretir.
3. Üretim ortamına doğal olmayan hataları önlemek için ve son derece hızlı İnsan hatası yanıta olduğundan emin olmak için pristine tutun.

Hata analizi hizmeti aracılığıyla Service Fabric, biz bu Aç ve geliştirici ortamından aynı yönteme üretime kullanmak önerdiği. Bunu başarmak için iki yolu vardır:

1. Denetimli hataları anlamına için hataya analiz hizmeti API'lerinin bir Kutulu ortamı tüm üretim kümeleri için kullanın.
2. Küme hatalar otomatik endüksiyon neden olan bir fever vermek için arıza analiz hizmeti otomatik hataları oluşturmak için kullanın. Yapılandırma yoluyla hata oranı denetleme farklı ortamlarda farklı sınanacak aynı hizmeti sağlar.

Hataları ölçeğini farklı ortamlarda farklı olabilir ancak Service Fabric ile gerçek mekanizmaları aynı olacaktır. Bu kadar kod dağıtım daha hızlı işlem hattı ve gerçek yükleri hizmetler sınama olanağı sağlar.

## <a name="using-the-fault-analysis-service"></a>Hata analizi hizmeti kullanma
**C#**

Hata analizi hizmeti Microsoft.ServiceFabric NuGet paketi System.Fabric ad alanında özellikleridir. Hataya analiz hizmet özelliklerini kullanmak için projenizdeki bir başvuru olarak nuget paketini ekleyin.

**PowerShell**

PowerShell kullanmak için Service Fabric SDK yüklemeniz gerekir. SDK'yı yükledikten sonra ServiceFabric PowerShell kullanabilmeniz için yüklenen otomatik modülüdür.

## <a name="next-steps"></a>Sonraki adımlar
Gerçekten bulut ölçekli hizmetler oluşturmak için önce ve dağıtım sonra hizmetleri gerçek dünya hataları dayanabilir emin olmak için önemlidir. Hizmetleri dünyada Bugün, hızlı bir şekilde yenilik ve kod üretime hızlıca taşımak olanağı çok önemlidir. Hata analizi hizmeti tam olarak bunu yapmak için hizmet geliştiricileri yardımcı olur.

Yerleşik kullanarak hizmetleri ve uygulamaları sınama başlamak [testi senaryolarında](service-fabric-testability-scenarios.md), veya kullanarak kendi test senaryoları Yazar [arıza Eylemler](service-fabric-testability-actions.md) hata analizi hizmeti tarafından sağlanan.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png
