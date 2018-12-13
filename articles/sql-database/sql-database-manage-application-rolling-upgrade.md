---
title: Uygulama yükseltme - Azure SQL veritabanı rolling | Microsoft Docs
description: Azure SQL veritabanı coğrafi çoğaltma, bulut uygulamanızın çevrimiçi yükseltmeler desteklemek için kullanmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 08/23/2018
ms.openlocfilehash: 63078450cc17167da11d0c8de021005b3798d8d3
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52877462"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>SQL veritabanı etkin coğrafi çoğaltmayı kullanarak bulut uygulamaların yükseltmelerini yönetme
> [!NOTE]
> [Etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md) tüm katmanlarda tüm veritabanları için kullanıma sunulmuştur.
> 

Nasıl kullanacağınızı öğrenin [coğrafi çoğaltma](sql-database-geo-replication-overview.md) bulut uygulamanızın sıralı yükseltmelerini etkinleştirmek için SQL veritabanı'nda. Yükseltme kesintiye uğratan bir işlem olduğundan, iş sürekliliği planlama ve tasarım parçası olması gerekir. Bu makalede biz yükseltme işlemini yönetmek, iki farklı yöntem ele bakın ve avantaj ve dezavantajlarına her bir seçeneğin tartışın. Tek veritabanı olarak kendi veri katmanına bağlı bir web sitesi oluşan basit bir uygulama bu makalenin amaçları için kullanacağız. Hedefimiz, sürüm 2 için son kullanıcı deneyimi üzerinde önemli bir etkisi olmadan uygulamanın sürüm 1 yükseltmektir. 

Yükseltme seçenekleri değerlendirirken aşağıdaki etmenleri düşünmeniz:

* Yükseltme sırasında uygulama kullanılabilirliği üzerindeki etkisi. Ne kadar süreyle uygulama işlevi sınırlı düşürülmüş veya olabilir.
* Bir yükseltmenin başarısız olması durumunda geri alma olanağı.
* Uygulama yükseltme sırasında ilgisiz bir arıza oluşması durumunda bir güvenlik açığı.
* Maliyet toplam dolar.  Bu ek artıklık ve yükseltme işlemi tarafından kullanılan geçici bileşenlerin artımlı maliyetlerini içerir. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Veritabanı Yedeklemeleri için olağanüstü durum kurtarma kullanan uygulamaları yükseltme
Uygulamanız üzerinde otomatik veritabanı yedeklemelerini kullanır ve olağanüstü durum kurtarma için coğrafi geri yükleme kullanır, genellikle tek bir Azure bölgesine dağıtılır. Bu durumda tüm uygulama bileşenleri yedekleme dağıtımını yükseltme katılan oluşturma yükseltme işlemi içerir. Son kullanıcı kesinti en aza indirmek için yük devretme profili ile Azure Traffic Manager'ı (ATM) özelliğinden yararlanır.  Aşağıdaki diyagram, yükseltme işlemi önce işletimsel ortamı gösterir. Uç nokta <i>contoso 1.azurewebsites.net</i> yükseltilmesi gereken uygulamanın üretim yuvasını temsil eder. Geri alma özelliğini etkinleştirmek için yükseltme, aşama yuvası ile uygulamanın tam bir eşitleme bir kopyasını oluşturmanız gerekir. Uygulama yükseltme için hazırlamak için aşağıdaki adımlar gerekir:

1. Yükseltme için bir aşama yuvası oluşturun. İçin (1) ikincil bir veritabanı oluşturmak ve aynı Azure bölgesinde aynı bir web sitesi dağıtımı yapın. Dengeli dağıtım işlemi tamamladığını görmek için ikincil izleyin.
2. Bir yük devretme profili ile ATM oluşturun <i>contoso 1.azurewebsites.net</i> çevrimiçi uç noktası olarak ve <i>contoso 2.azurewebsites.net</i> çevrimdışı olarak. 

> [!NOTE]
> Hazırlık adımları uygulamayı üretim yuvasındaki etkilenmez ve tam erişim modunda düzgün unutmayın.
>  

![SQL veritabanı Git çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Hazırlık adımları tamamlandıktan sonra uygulama gerçek yükseltme için hazır. Aşağıdaki diyagram yükseltme işleminde yer alan adımların gösterir. 

1. Birincil veritabanı salt okunur moda (3) üretim yuvası ayarlayın. Bu, üretim örneği (V1) uygulamanın bu nedenle V1 ve V2 veritabanı örnekleri arasında veri Geçitler önleme yükseltme sırasında salt okunur kalır garanti eder.  
2. Planlanan sonlandırma modu (4) kullanarak ikincil veritabanı bağlantısını kesin. Birincil veritabanına eşitlenen tamamen bağımsız bir kopyasını oluşturun. Bu veritabanı yükseltilir.
3. Birincil veritabanına okuma-yazma modunda açın ve aşama yuvası (5) yükseltme betiği çalıştırın.     

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Yükseltme başarıyla tamamlandı, artık uygulama hazırlanmış kopya için son kullanıcıların geçiş yapmak hazırsınız. Üretim yuvası uygulamanın artık hale gelir.  Bu, aşağıdaki diyagramda gösterildiği gibi birkaç adım içerir.

1. ATM profiline çevrimiçi uç nokta anahtarının <i>contoso 2.azurewebsites.net</i>, web sitesi (6) V2 sürümünü gösterir. V2 uygulamasını üretim yuvasıyla artık hale gelir ve son kullanıcı trafiğini, kendisine yönlendirilir.  
2. Güvenli şekilde V1 uygulama bileşenleri artık ihtiyacınız yoksa bunları (7) kaldırın.   

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Yükseltme işlemi başarısız olursa, örneğin yükseltme komut dosyası, bir hata nedeniyle aşama yuvası sayılacağı tehlikeye. Uygulama yükseltme öncesi durumuna geri almak için yalnızca tam erişim üretim yuvasındaki uygulamaya döndürün. Adımlar sonraki diyagramda gösterilmektedir.    

1. Veritabanı kopyasını (8) okuma-yazma moduna ayarlayın. Bu, üretim yuvasında işlevsel olarak tam V1 geri yükler.
2. Kök neden analizi gerçekleştirin ve güvenliği aşılan bileşenleri aşama yuvası (9) kaldırın. 

Bu noktada uygulama tamamen çalışır durumdadır ve yükseltme adımlarını yinelenebilir.

> [!NOTE]
> Geri alma zaten işaret gibi ATM profilinde değişiklik gerektirmez <i>contoso 1.azurewebsites.net</i> etkin uç nokta olarak.
> 
> 

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

Anahtar **avantajı** bu uygulamanın basit adımları kullanarak tek bir bölgede yükseltebilirsiniz seçeneğidir. Yükseltme dolar maliyeti oldukça düşüktür. Ana **tradeoff** yükseltme sırasında bir arıza oluşması durumunda kurtarma yükseltme öncesi durumu için farklı bir bölge ve Yedekleme kullanarak veritabanını geri uygulamanın yeniden dağıtımını içerecektir olduğu coğrafi geri yükleme. Bu işlem, önemli kapalı kalma süresi neden olur.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Veritabanı olağanüstü durum kurtarma için coğrafi çoğaltma özelliğine dayanır uygulamalarını yükseltme
İş sürekliliği için coğrafi çoğaltma, uygulamanızın yararlanır, en az iki farklı bölge birincil bölgede etkin bir dağıtım ve hazır bekleyen bir dağıtım yedekleme bölgede dağıtılır. Daha önce bahsedilen Etkenler ek olarak, yükseltme işlemi, etmeleri gerekir:

* Yükseltme işlemi sırasında her zaman uygulama yıkıcı arızasına karşı korumalı kalır
* Coğrafi olarak yedekli bileşenler uygulamanın etkin bileşenleri ile paralel yükseltildi

Bu hedeflere ulaşmak için Azure Traffic Manager (ATM) tane aktif, üç yedekleme uç noktaları ile yük devretme profili kullanarak özelliğinden yararlanır.  Aşağıdaki diyagram, yükseltme işlemi önce işletimsel ortamı gösterir. Web siteleri <i>contoso 1.azurewebsites.net</i> ve <i>contoso dr.azurewebsites.net</i> bir üretim yuvası uygulamanın tam coğrafi artıklık ile temsil eder. Geri alma özelliğini etkinleştirmek için yükseltme, aşama yuvası ile uygulamanın tam bir eşitleme bir kopyasını oluşturmanız gerekir. Uygulama yükseltme işlemi sırasında bir arıza oluşması durumunda hızlı bir şekilde kurtarabilirsiniz emin olmak gerektiğinden, aşama yuvası de coğrafi olarak yedekli olması gerekir. Uygulama yükseltme için hazırlamak için aşağıdaki adımlar gerekir:

1. Yükseltme için bir aşama yuvası oluşturun. İçin (1) ikincil bir veritabanı oluşturmak ve web sitesinin aynı Azure bölgesinde özdeş birer kopyası dağıtan yapın. Dengeli dağıtım işlemi tamamladığını görmek için ikincil izleyin.
2. Coğrafi olarak yedekli bir ikincil veritabanı aşama yuvada coğrafi olarak çoğaltarak yedekleme bölgeye (Buna "coğrafi çoğaltma zincirleme" adı verilir) ikincil veritabanı oluşturun. Dengeli dağıtım işlemi tamamlandı (3) olup olmadığını görmek için ikincil yedekleme izleyin.
3. Yedekleme bölgede web sitesi yedek bir kopyasını oluşturun ve coğrafi olarak yedekli ikincil (4) bağlayabilirsiniz.  
4. Ek uç noktalar ekleme <i>contoso 2.azurewebsites.net</i> ve <i>contoso 3.azurewebsites.net</i> ATM çevrimdışı (5) uç noktalar olarak yük devretme profiline. 

> [!NOTE]
> Hazırlık adımları uygulamayı üretim yuvasındaki etkilenmez ve tam erişim modunda düzgün unutmayın.
> 
> 

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Hazırlık adımları tamamlandıktan sonra aşama yuvası yükseltme için hazır olur. Aşağıdaki diyagram, yükseltme adımlarını gösterir.

1. Birincil veritabanı salt okunur moda (6) üretim yuvası ayarlayın. Bu, üretim örneği (V1) uygulamanın bu nedenle V1 ve V2 veritabanı örnekleri arasında veri Geçitler önleme yükseltme sırasında salt okunur kalır garanti eder.  
2. Planlanan sonlandırma modu (7) kullanılarak aynı bölgede ikincil veritabanı bağlantısını kesin. Tam olarak eşitlenen bir bağımsız birincil sonlandırmasından sonra otomatik olarak olacak birincil veritabanının kopyasını oluşturur. Bu veritabanı yükseltilir.
3. Birincil veritabanını okuma-yazma moduna aşama yuvasındaki açın ve yükseltme komut dosyası (8) çalıştırın.    

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Yükseltme başarıyla tamamlandı, artık son kullanıcıların uygulamayı V2 sürümüne geçiş hazır olup olmadığını. Aşağıdaki şemada adımlar gösterilmektedir.

1. ATM profiline etkin uç nokta anahtarının <i>contoso 2.azurewebsites.net</i>, şimdi web sitesi (9) V2 sürümünü gösterir. V2 uygulamasını üretim yuvasıyla artık hale gelir ve son kullanıcı trafiğini, kendisine yönlendirilir. 
2. V1 uygulaması artık ihtiyacınız yoksa bu nedenle güvenli bir şekilde (10 ve 11) kaldırabilirsiniz.  

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Yükseltme işlemi başarısız olursa, örneğin yükseltme komut dosyası, bir hata nedeniyle aşama yuvası sayılacağı tehlikeye. Uygulamayı yalnızca tam erişime sahip üretim yuvasındaki uygulama kullanmaya geri yükseltme öncesi duruma geri almak için. Adımlar sonraki diyagramda gösterilmektedir.    

1. Birincil veritabanı kopyasını üretim yuvasına okuma-yazma modunda (12) olarak ayarlayın. Bu, üretim yuvasında işlevsel olarak tam V1 geri yükler.
2. Kök neden analizi gerçekleştirin ve güvenliği aşılan bileşenleri aşama yuvada (13 ve 14) kaldırın. 

Bu noktada uygulama tamamen çalışır durumdadır ve yükseltme adımlarını yinelenebilir.

> [!NOTE]
> Geri alma zaten işaret gibi ATM profilinde değişiklik gerektirmez <i>contoso 1.azurewebsites.net</i> etkin uç nokta olarak.
> 
> 

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

Anahtar **avantajı** Bu, hem uygulamayı hem de, coğrafi olarak yedekli bir kopyasını paralel yükseltme sırasında iş sürekliliği ödün vermeden yükseltebilirsiniz seçeneğidir. Ana **tradeoff** her uygulama bileşeninin çift yedeklilik gerektirir ve bu nedenle daha yüksek Doları maliyet doğurur. Ayrıca, daha karmaşık bir iş akışı içerir. 

## <a name="summary"></a>Özet
Makalede açıklanan iki yükseltme yöntemi karmaşıklığı farklılık gösterir ve maliyet dolar ancak her iki son kullanıcıya salt okunur işlemler için sınırlı olduğunda süresini en aza indirerek üzerinde odaklanın. Bu süre doğrudan süresini yükseltme komut dosyası tarafından tanımlanır. Veritabanı boyutu, seçtiğiniz hizmet katmanı, web sitesi yapılandırması ve kolayca denetleyemezsiniz diğer etkenlere bağlı değildir. Tüm hazırlık adımları yükseltme adımları birbirinden ayrılmıştır ve bir üretim uygulaması etkilemeden gerçekleştirilebilir olmasıdır. Yükseltme betiği verimliliğini yükseltmeler sırasında son kullanıcı deneyimini belirleyen önemli bir faktördür. Bu nedenle geliştirmek en iyi yükseltme betiği mümkün olduğunda verimli hale çabalarınıza odaklanmak yoludur.  

## <a name="next-steps"></a>Sonraki adımlar
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
* Otomatik yedekleme, kurtarma için kullanma hakkında bilgi edinmek için [otomatik yedeklemelerden veritabanını geri yükleme](sql-database-recovery-using-backups.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md).


