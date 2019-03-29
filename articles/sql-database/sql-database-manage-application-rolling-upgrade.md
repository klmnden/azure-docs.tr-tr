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
ms.date: 02/13/2019
ms.openlocfilehash: 47fd6c1e2bb342bc1a31fb16a45a5ebc749dca69
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621456"
---
# <a name="manage-rolling-upgrades-of-cloud-applications-by-using-sql-database-active-geo-replication"></a>Sıralı yükseltmeler, bulut uygulamalarının SQL veritabanı etkin coğrafi çoğaltma'yı kullanarak yönetme

Nasıl kullanacağınızı öğrenin [etkin coğrafi çoğaltma](sql-database-auto-failover-group.md) bulut uygulamanızın sıralı yükseltmelerini etkinleştirmek için Azure SQL veritabanı'nda. Yükseltme işlemleri karışıklığa neden olduğundan, bunlar, iş sürekliliği planlama ve tasarım parçası olmalıdır. Bu makalede, yükseltme işlemini yönetmek için iki farklı yöntem arayın ve avantajlarını ve dezavantajlarını her bir seçeneğin tartışın. Bu makalenin amaçları için tek veritabanı olarak kendi veri katmanına bağlı bir Web sitesi içeren bir uygulama diyoruz. Hedefimiz sürüm 1 (V1) sürüm 2 (V2) kullanıcı deneyimini önemli bir etkisi olmadan uygulamanın yükseltilmesidir.

Yükseltme seçenekleri değerlendirirken, aşağıdaki etmenleri dikkate alın:

* Uygulama kullanılabilirliği gibi uygulama işlevlerini ne kadar süreyle sınırlı veya düşürülmüş yükseltmeleri sırasında etkisi.
* Geri yükseltme başarısız olursa geri alma olanağı.
* Güvenlik Açığı yükseltme sırasında ilgisiz, geri dönülemez bir hata oluşursa uygulama.
* Maliyet toplam dolar. Bu ek veritabanı yedekliliği ve yükseltme işlemi tarafından kullanılan geçici bileşenlerin artımlı maliyetlerini içerir.

## <a name="upgrade-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Veritabanı Yedeklemeleri için olağanüstü durum kurtarma kullanan uygulamaları yükseltme

Uygulamanız üzerinde otomatik veritabanı yedeklemelerini kullanır ve olağanüstü durum kurtarma için coğrafi geri yükleme kullanır, bunu tek bir Azure bölgesine dağıtılır. Kullanıcı uğramasını azaltmak için bu bölgedeki yükseltmesinde dahil olan tüm uygulama bileşenleri ile bir hazırlık ortamı oluşturun. Yükseltme işlemi önce işletimsel ortamı birinci diyagramda gösterilmektedir. Uç nokta `contoso.azurewebsites.net` bir üretim ortamında web uygulamasının temsil eder. Yükseltmeyi geri yapabilmek için bir hazırlık ortamı ile tam bir eşitleme bir veritabanının kopyasını oluşturmanız gerekir. Yükseltme için bir hazırlık ortamı oluşturmak için aşağıdaki adımları izleyin:

1. Aynı Azure bölgesinde ikinci bir veritabanı oluşturun. Dengeli dağıtım işlemi tamamlandı (1) olup olmadığını görmek için ikincil izleyin.
2. Web uygulamanız için yeni bir ortam oluşturun ve 'Hazırlama' çağrısı. URL ile Azure DNS'te kaydedilecek `contoso-staging.azurewebsites.net` (2).

> [!NOTE]
> Tam erişim modda çalışabilir üretim ortamını hazırlama adımları etkilemez.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

Hazırlık adımları tamamlandığında, uygulama, gerçek yükseltme için hazır. Yükseltme işleminde yer alan adımların bir sonraki diyagramda gösterilmektedir:

1. Birincil veritabanı salt okunur moda (3) ayarlayın. Bu mod (V1) web uygulamasının üretim ortamına böylece V1 ve V2 veritabanı örnekleri arasında veri Geçitler önleme yükseltme işlemi sırasında salt okunur kalmasını güvence altına alır.
2. İkincil veritabanı planlı sonlandırma modu (4) kullanılarak bağlantısını kesin. Bu eylem, tam bir eşitleme, bağımsız bir birincil veritabanının kopyasını oluşturur. Bu veritabanı yükseltilir.
3. İkincil veritabanı için okuma-yazma modunda açın ve yükseltme betiği (5) çalıştırın.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Yükseltme başarıyla tamamlanırsa artık kullanıcılar yükseltilen kopya bir üretim ortamında olur uygulama geçiş yapmak hazırsınız. Geçiş, bir sonraki diyagramda gösterildiği gibi birkaç daha fazla adımları içerir:

1. Üretim ve hazırlık ortamları web uygulamasının (6) arasında bir takas işlemi etkinleştirin. Bu işlem, iki ortam URL'lerini geçer. Artık `contoso.azurewebsites.net` web sitesi ve veritabanı (üretim ortamı) V2 sürümüne işaret eder. 
2. Hazırlama kopyasını değiştirme işleminden sonra başlarsa, V1 sürümü artık ihtiyacınız kalmadığında, hazırlık ortamı (7) yetkisini alabilirsiniz.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Yükseltme işlemi başarısızsa (örneğin, bir hata nedeniyle yükseltme komut dosyası) tehlikeye üzere hazırlama ortamını göz önünde bulundurun. Uygulama yükseltme öncesi durumuna geri almak için tam erişim için üretim ortamında uygulama döndürün. Geri Al adımlar bir sonraki diyagramda gösterilmiştir:

1. Veritabanı kopyasını (8) okuma-yazma moduna ayarlayın. Bu eylem, üretim kopyalama V1 tam işlevselliğini geri yükler.
2. Kök neden analizi gerçekleştirin ve hazırlık ortamı (9) yetkisini alma.

Bu noktada, uygulamayı tamamen çalışır durumdadır ve yükseltme adımları tekrarlayabilirsiniz.

> [!NOTE]
> Henüz bir değiştirme işlemi gerçekleştirmedi çünkü geri DNS değişiklik yapılması gerekmez.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

Bu seçenek, önemli bir avantajı, basit adımları izleyerek uygulamanın tek bir bölgede yükseltebilirsiniz ' dir. Yükseltme dolar maliyeti oldukça düşüktür. 

Ana artırabilen yükseltme sırasında bir arıza oluşması durumunda, yükseltme öncesi durumu için kurtarma farklı bir bölgede uygulamanızı ve coğrafi geri yükleme kullanarak veritabanını yedekten geri yükleme, içermesidir. Bu işlem, önemli kapalı kalma süresi sonuçlanır.

## <a name="upgrade-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Veritabanı olağanüstü durum kurtarma için coğrafi çoğaltmayı kullanan uygulamaları yükseltme

Uygulamanız için iş sürekliliği etkin coğrafi çoğaltma veya otomatik yük devretme grupları kullanıyorsa, en az iki farklı bölgeye dağıtılır. Bir birincil bölgede bir etkin, birincil veritabanı ile salt okunur, ikincil bir veritabanı yedekleme bölgede yoktur. Bu makalenin başında belirtilen Etkenler yanı sıra, yükseltme işlemi ayrıca, garanti etmeniz gerekir:

* Uygulama yükseltme işlemi sırasında her zaman yıkıcı arızasına karşı korumalı olarak kalır.
* Coğrafi olarak yedekli bileşenler uygulamanın etkin bileşenleri ile paralel yükseltilir.

Web Apps ortamları kullanmanın yanı sıra bu hedeflere ulaşmak için Azure Traffic Manager'ın bir etkin uç noktası ve bir yedekleme uç nokta ile bir yük devretme profilini kullanarak yararlanırsınız. Yükseltme işlemi önce işletimsel ortamı sonraki diyagramda gösterilmektedir. Web siteleri `contoso-1.azurewebsites.net` ve `contoso-dr.azurewebsites.net` bir üretim ortamında uygulamanın tam coğrafi artıklık ile temsil eder. Üretim ortamında aşağıdaki bileşenleri içerir:

* Web uygulamasını üretim ortamına `contoso-1.azurewebsites.net` birincil bölgede (1)
* Birincil veritabanı birincil bölgede (2)
* Bir web uygulamasını yedekleme bölgede (3) bekleme örneği
* Coğrafi çoğaltmalı ikincil bölgede veritabanı yedekleme (4)
* Çevrimiçi bir uç nokta Traffic Manager performans profiliyle adlı `contoso-1.azurewebsites.net` ve çevrimdışı bir uç nokta adı `contoso-dr.azurewebsites.net`

Yükseltmeyi geri mümkün hale getirmek için bir hazırlık ortamı ile uygulamanın tam bir eşitleme bir kopyasını oluşturmanız gerekir. Uygulama yükseltme işlemi sırasında bir arıza oluşması durumunda hızlı bir şekilde kurtarabilirsiniz emin olmak gerektiği için hazırlık ortamı da coğrafi olarak yedekli olması gerekir. Aşağıdaki adımlar, yükseltme için bir hazırlık ortamı oluşturmak için gereklidir:

1. Birincil bölgede (6) web uygulamasının hazırlama ortamına dağıtın.
2. Bir ikincil veritabanı Birincil Azure bölgesinde (7) oluşturun. Web uygulamasının bağlanmak için hazırlık ortamı yapılandırın. 
3. Başka bir coğrafi olarak yedekli, ikincil veritabanı, ikincil veritabanı birincil bölgede çoğaltarak yedekleme bölgede oluşturun. (Bu yöntem çağrılır *coğrafi çoğaltma zincirleme*.) (8).
4. Yedekleme bölgede (9) web uygulaması örneğinin hazırlama ortamına dağıtın ve (8) coğrafi olarak yedekli ikincil veritabanlarından bağlanmak için yapılandırın.

> [!NOTE]
> Hazırlama adımları uygulamayı üretim ortamında etkilemez. Okuma-yazma modunda tamamen işlevsel kalır.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

Hazırlık adımları tamamlandığında, yükseltme için hazırlık ortamı hazırdır. Bir sonraki diyagramda yükseltme adımları göstermektedir:

1. Birincil veritabanı salt okunur moda (10) üretim ortamında ayarlayın. Bu mod, bu nedenle V1 ve V2 veritabanı örnekleri arasında veri Geçitler önleme yükseltme işlemi sırasında üretim veritabanı (V1) değişmez garanti eder.

```sql
-- Set the production database to read-only mode
ALTER DATABASE <Prod_DB>
SET (ALLOW_CONNECTIONS = NO)
```

2. Coğrafi çoğaltma ikincil (11) bağlantısını keserek sonlandırın. Bu eylem, bağımsız ancak tam bir eşitleme bir üretim veritabanının kopyasını oluşturur. Bu veritabanı yükseltilir. Aşağıdaki örnek, Transact-SQL kullanır ancak [PowerShell](/powershell/module/az.sql/remove-azsqldatabasesecondary?view=azps-1.5.0) de kullanılabilir. 

```sql
-- Disconnect the secondary, terminating geo-replication
ALTER DATABASE <Prod_DB>
REMOVE SECONDARY ON SERVER <Partner-Server>
```

3. Yükseltme betiği karşı çalıştırmak `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net`ve hazırlama birincil veritabanı (12). Veritabanı değişiklikleri otomatik olarak ikincil hazırlama çoğaltılır.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

Yükseltme başarıyla tamamlanırsa artık kullanıcılar V2 uygulama sürümüne geçiş yapmak hazırsınız. Bir sonraki diyagramda yer alan adımların gösterilmektedir:

1. Üretim ve hazırlama ortamları (13) birincil bölgedeki ve yedekleme bölgede (14) web uygulaması arasında bir takas işlemi etkinleştirin. V2 uygulama artık yedekleme bölgede yedekli bir kopyasını içeren bir üretim ortamını olur.
2. V1 uygulaması (15 ve 16) artık ihtiyacınız kalmadığında, hazırlık ortamı yetkisini alabilir.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Yükseltme işlemi başarısızsa (örneğin, bir hata nedeniyle yükseltme komut dosyası) tutarsız bir durumda olması için hazırlık ortamı göz önünde bulundurun. Uygulama yükseltme öncesi durumuna geri almak için üretim ortamında uygulama V1 kullanmaya döner. Gerekli adımlar, sonraki diyagramda gösterilmiştir:

1. Birincil veritabanı kopyasını, üretim ortamında okuma-yazma moduna (17) ayarlayın. Bu eylem, üretim ortamında tam V1 işlevselliği geri yükler.
2. Kök neden analizi ve onarım gerçekleştirin veya hazırlama ortamına (18 ile 19) kaldırın.

Bu noktada, uygulamayı tamamen çalışır durumdadır ve yükseltme adımları tekrarlayabilirsiniz.

> [!NOTE]
> Geri alma, DNS değiştirir, bir değiştirme işlemi gerçekleştirmedi çünkü gerektirmez.

![Bulutta olağanüstü durum kurtarma için SQL veritabanı coğrafi çoğaltma yapılandırması.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

Bu seçenek, önemli bir avantajı, hem uygulamayı hem de, coğrafi olarak yedekli bir kopyasını paralel yükseltme sırasında iş sürekliliği ödün vermeden yükseltebilirsiniz, ' dir.

Ana artırabilen her uygulama bileşeninin çift yedeklilik gerektirir ve bu nedenle daha yüksek Doları maliyet doğurur ' dir. Ayrıca, daha karmaşık bir iş akışı içerir.

## <a name="summary"></a>Özet

Makalede açıklanan iki yükseltme yöntemi, karmaşıklığı ve maliyeti dolar farklılık gösterir ancak her ikisi de kullanıcının ne kadar süreyle salt okunur işlemler için sınırlı olduğu en aza üzerinde odaklanın. Bu süre doğrudan süresini yükseltme komut dosyası tarafından tanımlanır. Veritabanı boyutu, seçtiğiniz hizmet katmanı, Web sitesi yapılandırması veya kolayca denetleyemezsiniz diğer etkenler bağımlı değildir. Tüm hazırlık adımları yükseltme adımları birbirinden ayrılmıştır ve üretim uygulama etkisi yoktur. Yükseltme betiği verimliliğini yükseltmeler sırasında kullanıcı deneyimini belirleyen önemli bir faktördür. Bu nedenle, bu deneyimi geliştirmek için en iyi yolu çalışmalarınızı yükseltme betiği mümkün olduğunda verimli yapmaya odaklanın sağlamaktır.

## <a name="next-steps"></a>Sonraki adımlar

* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Azure SQL veritabanı etkin coğrafi çoğaltma hakkında bilgi edinmek için [etkin coğrafi çoğaltmayı kullanarak okunabilir ikincil veritabanı oluşturma](sql-database-active-geo-replication.md).
* Azure SQL veritabanı otomatik yük devretme grupları hakkında daha fazla bilgi için bkz: [birden fazla veritabanının saydam ve Eşgüdümlü yük devretmeyi etkinleştirmek için otomatik yük devretme grupları kullanma](sql-database-auto-failover-group.md).
* Azure App Service'te hazırlık hakkında bilgi edinmek için [Azure App Service ortamlarında hazırlık ayarlama](../app-service/deploy-staging-slots.md).
* Azure Traffic Manager profilleri hakkında bilgi edinmek için [bir Azure Traffic Manager profilini yönetme](../traffic-manager/traffic-manager-manage-profiles.md).
