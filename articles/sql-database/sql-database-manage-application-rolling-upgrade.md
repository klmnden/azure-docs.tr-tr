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
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 01/29/2019
ms.openlocfilehash: 50f6f114a4d90f48218f751e1649e8694e664491
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295755"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>SQL veritabanı etkin coğrafi çoğaltmayı kullanarak bulut uygulamaların yükseltmelerini yönetme

Nasıl kullanacağınızı öğrenin [etkin coğrafi çoğaltma](sql-database-auto-failover-group.md) bulut uygulamanızın sıralı yükseltmelerini etkinleştirmek için SQL veritabanı'nda. Yükseltme kesintiye uğratan bir işlem olduğundan, iş sürekliliği planlama ve tasarım parçası olması gerekir. Bu makalede biz yükseltme işlemini yönetmek, iki farklı yöntem ele bakın ve avantaj ve dezavantajlarına her bir seçeneğin tartışın. Bu makalenin amaçları için tek veritabanı olarak kendi veri katmanına bağlı bir web sitesi içeren bir uygulama kullanacağız. Hedefimiz, sürüm 2 için son kullanıcı deneyimi üzerinde önemli bir etkisi olmadan uygulamanın sürüm 1 yükseltmektir.

Yükseltme seçenekleri değerlendirirken, aşağıdaki faktörleri dikkate almanız gerekir:

* Yükseltme sırasında uygulama kullanılabilirliği üzerindeki etkisi. Ne kadar süreyle uygulama işlevi sınırlı düşürülmüş veya olabilir.
* Yükseltme başarısız olursa geri alma olanağı.
* Uygulama yükseltme sırasında ilgisiz bir arıza oluşması durumunda bir güvenlik açığı.
* Maliyet toplam dolar.  Bu ek artıklık ve yükseltme işlemi tarafından kullanılan geçici bileşenlerin artımlı maliyetlerini içerir.

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Veritabanı Yedeklemeleri için olağanüstü durum kurtarma kullanan uygulamaları yükseltme

Uygulamanız üzerinde otomatik veritabanı yedeklemelerini kullanır ve olağanüstü durum kurtarma için coğrafi geri yükleme kullanır, bunu tek bir Azure bölgesine dağıtılır. Son kullanıcı kesinti en aza indirmek için bu bölgede yükseltmesinde dahil olan tüm uygulama bileşenleri sahip bir hazırlık ortamı oluşturur. Aşağıdaki diyagram, yükseltme işlemi önce işletimsel ortamı gösterir. Uç nokta `contoso.azurewebsites.net` web uygulamasının üretim yuvasını temsil eder. Yükseltmeyi geri alma özelliğini etkinleştirmek için bir hazırlama yuvası ile tam bir eşitleme bir veritabanının kopyasını oluşturmanız gerekir. Aşağıdaki adımlar, yükseltme için bir hazırlık ortamı oluşturacak:

1. Aynı Azure bölgesinde ikinci bir veritabanı oluşturun. Dengeli dağıtım işlemi tamamlandı (1) olup olmadığını görmek için ikincil izleyin.
2. 'Hazırlama' adlı web uygulamanız için yeni bir dağıtım yuvası oluşturur. URL ile DNS'de kaydedilecek `contoso-staging.azurewebsites.net` (2).

> [!NOTE]
> Hazırlık adımlarını üretim yuvasına etkilenmez ve tam erişim modunda düzgün unutmayın.
>  

![SQL veritabanı Git çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Hazırlık adımları tamamlandıktan sonra gerçek yükseltme için hazır bir uygulamadır. Aşağıdaki diyagram yükseltme işleminde yer alan adımların gösterir.

1. Birincil veritabanı salt okunur moda (3) ayarlayın. Bu mod, üretim yuvası (V1) web uygulamasının böylece V1 ve V2 veritabanı örnekleri arasında veri Geçitler önleme yükseltme sırasında salt okunur kalır garanti eder.  
2. Planlanan sonlandırma modu (4) kullanarak ikincil veritabanı bağlantısını kesin. Birincil veritabanına eşitlenen tamamen bağımsız bir kopyasını oluşturun. Bu veritabanı yükseltilir.
3. Birincil veritabanına okuma-yazma modunda açın ve yükseltme betiği (5) çalıştırın.

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Yükseltme başarıyla tamamlandı, artık son kullanıcıların uygulamayı yükseltilmiş kopyaya geçiş yapmak hazırsınız. Üretim yuvası artık hale gelir.  Geçiş, aşağıdaki diyagramda gösterildiği gibi birkaç daha fazla adımdan oluşur.

1. Üretim ve hazırlama yuvaları web uygulamasının (6) arasında bir takas işlemi etkinleştirin. Bu iki yuvası URL'lerini geçiş yapar. Artık `contoso.azurewebsites.net` V2 sürümüne web sitesi ve veritabanı (üretim ortamı) işaret etmesini sağlayacaksınız.  
2. Hazırlama kopyasını değiştirme işleminden sonra başlarsa, V1 sürümü artık ihtiyacınız kalmadığında, hazırlık ortamı (7) yetkisini alabilirsiniz.

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Yükseltme işlemi başarısız olursa, örneğin yükseltme komut dosyası, bir hata nedeniyle aşama yuvası sayılacağı tehlikeye. Uygulama yükseltme öncesi durumuna geri almak için tam erişim üretim yuvasındaki uygulamaya geri dönün. Adımlar sonraki diyagramda gösterilmektedir.

1. Veritabanı kopyasını (8) okuma-yazma moduna ayarlayın. Bu, üretim kopyasının işlevsel olarak tam V1 geri yükler.
2. Kök neden analizi gerçekleştirin ve hazırlık ortamı (9) yetkisini alma.

Bu noktada uygulama tamamen çalışır durumdadır ve yükseltme adımlarını yinelenebilir.

> [!NOTE]
> Henüz bir değiştirme işlemi gerçekleştirmedi gibi DNS değişiklikleri geri alma gerektirmez.

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

Anahtar **avantajı** bu uygulamanın basit adımları kullanarak tek bir bölgede yükseltebilirsiniz seçeneğidir. Yükseltme dolar maliyeti oldukça düşüktür. Ana **tradeoff** yükseltme sırasında bir arıza oluşması durumunda kurtarma için yükseltme öncesi durumu yeniden dağıtma işlemi uygulamanın farklı bir bölge ve Yedekleme kullanarak veritabanını geri içerecektir olduğu coğrafi geri yükleme. Bu işlem, önemli kapalı kalma süresi neden olur.

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Veritabanı olağanüstü durum kurtarma için coğrafi çoğaltma özelliğine dayanır uygulamalarını yükseltme

Etkin coğrafi çoğaltma veya iş sürekliliği için yük devretme grupları, uygulamanızın yararlanır, en az iki farklı bölge ile birincil bölgedeki etkin bir birincil veritabanı ve salt okunur bir ikincil veritabanı yedekleme bölgede dağıtılır. Daha önce bahsedilen Etkenler ek olarak, yükseltme işlemi, etmeleri gerekir:

* Yükseltme işlemi sırasında her zaman uygulama yıkıcı arızasına karşı korumalı kalır
* Coğrafi olarak yedekli bileşenler uygulamanın etkin bileşenleri ile paralel yükseltildi

Web App dağıtım yuvalarını kullanmaya ek olarak, bu hedeflere ulaşmak için Azure Traffic Manager (ATM) tane aktif, bir yedekleme uç noktaları ile bir yük devretme profili kullanılarak özelliğinden yararlanır.  Aşağıdaki diyagram, yükseltme işlemi önce işletimsel ortamı gösterir. Web siteleri `contoso-1.azurewebsites.net` ve `contoso-dr.azurewebsites.net` bir üretim ortamında uygulamanın tam coğrafi artıklık ile temsil eder. Üretim ortamında aşağıdaki bileşenleri içerir:

1. Üretim yuvası web uygulamasının `contoso-1.azurewebsites.net` birincil bölgede (1)
2. Birincil bölgede (2) birincil veritabanı 
3. Bir bekleme örneği web uygulamasının yedekleme bölgede (3)
4. Coğrafi çoğaltmalı ikincil veritabanına yedekleme bölgede (4)
5. Azure traffic manager performans profili çevrimiçi uç noktası ile `contoso-1.azurewebsites.net` ve çevrimdışı uç noktası `contoso-dr.azurewebsites.net`

Yükseltmeyi geri alma özelliğini etkinleştirmek için bir hazırlık ortamı ile uygulamanın tam bir eşitleme bir kopyasını oluşturmanız gerekir. Uygulama yükseltme işlemi sırasında bir arıza oluşması durumunda hızlı bir şekilde kurtarabilirsiniz emin olmak gerektiği için hazırlık ortamı da coğrafi olarak yedekli olması gerekir. Aşağıdaki adımlar, yükseltme için bir hazırlık ortamı oluşturmak için gereklidir:

1. Birincil bölgede (6) web uygulamasının bir hazırlama yuvasına dağıtma
2. Bir ikincil veritabanı Birincil Azure bölgesinde (7) oluşturun. Hazırlama yuvası kendisine bağlanmak için web uygulamasının yapılandırın. 
3. Birincil bölgede (Bu çağrılır "coğrafi çoğaltma zincirleme") ikincil veritabanı çoğaltarak yedekleme bölgede başka bir coğrafi çoğaltmalı ikincil veritabanı oluşturma (8).
3. Web uygulama örneğini yedekleme bölgede (9), bir hazırlama yuvasına dağıtın ve coğrafi-ikincil (9). adımda oluşturduğunuz bağlanmak için yapılandırın.


> [!NOTE]
> Hazırlık adımları uygulamayı üretim yuvasındaki etkilemez ve okuma-yazma modunda tamamen işlevsel kalır unutmayın.

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Hazırlık adımları tamamlandıktan sonra yükseltme için hazırlık ortamı hazırdır. Aşağıdaki diyagram, yükseltme adımlarını gösterir.

1. Birincil veritabanı salt okunur moda (10) üretim yuvası ayarlayın. Bu mod, üretim veritabanını (V1), bu nedenle V1 ve V2 veritabanı örnekleri arasında veri Geçitler önleme yükseltme sırasında değiştirmez garanti eder.  
2. Planlanan sonlandırma modu (11) kullanılarak aynı bölgede ikincil veritabanı bağlantısını kesin. Üretim veritabanı bağımsız ancak tam bir eşitleme bir kopyasını oluşturun. Bu veritabanı yükseltilir.
3. Yükseltme betiği karşı çalıştırmak `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net` ve hazırlama birincil veritabanı (12). Veritabanı değişiklikleri otomatik olarak ikincil hazırlama çoğaltılır 

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Yükseltme başarıyla tamamlandı, artık son kullanıcıların uygulamayı V2 sürümüne geçiş yapmak hazırsınız. Aşağıdaki şemada adımlar gösterilmektedir.

1. Üretim ve hazırlama yuvaları web uygulamasının birincil bölgede (13) ve yedekleme bölgede (14) arasında bir takas işlemi etkinleştirin. V2 uygulama artık yedekleme bölgede yedekli bir kopyasını üretim yuvasıyla olur.
2. V1 uygulaması (15 ve 16) artık ihtiyacınız kalmadığında, hazırlık ortamı yetkisini alabilirsiniz.  

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Yükseltme işlemi başarısız olursa, örneğin yükseltme komut dosyası, bir hata nedeniyle hazırlık ortamı tutarsız durumda kabul. Uygulama yükseltme öncesi durumuna geri almak için üretim ortamında uygulama V1 kullanmaya geri. Gerekli adımlar sonraki diyagramda gösterilmektedir.

1. Birincil veritabanı kopyasını üretim yuvasına okuma-yazma modunda (17) olarak ayarlayın. Bu, üretim yuvasında işlevsel olarak tam V1 geri yükler.
2. Kök neden analizi ve onarım gerçekleştirin veya hazırlama ortamına (18 ile 19) kaldırın.

Bu noktada, uygulamayı tamamen çalışır durumdadır ve yükseltme adımlarını yinelenebilir.

> [!NOTE]
> Bir değiştirme işlemi gerçekleştirmedi çünkü geri DNS değişiklik gerektirmez.

![SQL veritabanı coğrafi çoğaltma yapılandırması. Bulutta olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

Anahtar **avantajı** Bu, hem uygulamayı hem de, coğrafi olarak yedekli bir kopyasını paralel yükseltme sırasında iş sürekliliği ödün vermeden yükseltebilirsiniz seçeneğidir. Ana **tradeoff** her uygulama bileşeninin çift yedeklilik gerektirir ve bu nedenle daha yüksek Doları maliyet doğurur. Ayrıca, daha karmaşık bir iş akışı içerir.

## <a name="summary"></a>Özet

Makalede açıklanan iki yükseltme yöntemi karmaşıklığı farklılık gösterir ve maliyet dolar ancak her iki son kullanıcıya salt okunur işlemler için sınırlı olduğunda süresini en aza indirerek üzerinde odaklanın. Bu süre doğrudan süresini yükseltme komut dosyası tarafından tanımlanır. Veritabanı boyutu, seçtiğiniz hizmet katmanı, web sitesi yapılandırması ve kolayca denetleyemezsiniz diğer etkenlere bağlı değildir. Tüm hazırlık adımları yükseltme adımları birbirinden ayrılmıştır ve üretim uygulamayı etkilemez. Yükseltme betiği verimliliğini yükseltmeler sırasında son kullanıcı deneyimini belirleyen önemli bir faktördür. Bu nedenle geliştirmek en iyi yükseltme betiği mümkün olduğunda verimli hale çabalarınıza odaklanmak yoludur.  

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Azure SQL veritabanı etkin coğrafi çoğaltma hakkında bilgi edinmek için [etkin coğrafi çoğaltmayı kullanarak okunabilir ikincil veritabanı oluşturma](sql-database-active-geo-replication.md).
* Azure SQL veritabanı yük devretme grupları hakkında daha fazla bilgi için bkz: [birden fazla veritabanının saydam ve Eşgüdümlü yük devretmeyi etkinleştirmek için otomatik yük devretme grupları kullanma](sql-database-auto-failover-group.md).
* Dağıtım yuvası ve Azure App Service'te hazırlık ortamı hakkında bilgi edinmek için bkz. [Azure App Service ortamlarında hazırlık ayarlama](../app-service/deploy-staging-slots.md).  
* Azure traffic manager profilleri bilgi edinmek için [bir Azure Traffic Manager profilini yönetme](../traffic-manager/traffic-manager-manage-profiles.md).  


