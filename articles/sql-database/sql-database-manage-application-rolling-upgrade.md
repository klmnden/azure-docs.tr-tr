---
title: Azure SQL veritabanı çalışırken uygulama yükseltme - | Microsoft Docs
description: Azure SQL Database coğrafi çoğaltma bulut uygulamanızın çevrimiçi yükseltmeler desteklemek için nasıl kullanılacağını öğrenin.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sashan
ms.openlocfilehash: a73284d679b4be1fbae6d5e1688915c98cbf2392
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649508"
---
# <a name="managing-rolling-upgrades-of-cloud-applications-using-sql-database-active-geo-replication"></a>SQL veritabanı etkin coğrafi çoğaltma'yı kullanarak bulut uygulamalarının yükseltmelerini yönetme
> [!NOTE]
> [Aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md) tüm katmanlar tüm veritabanları için kullanıma sunulmuştur.
> 

Nasıl kullanacağınızı öğrenin [coğrafi çoğaltma](sql-database-geo-replication-overview.md) yükseltmelerini bulut uygulamanızın etkinleştirmek için SQL veritabanında. Yükseltme benzer bir işlem olduğundan, iş sürekliliği planlama ve tasarım parçası olması gerekir. Bu makalede biz yükseltme işlemini yönetme iki farklı yöntem bakın ve avantajları ve dengelemeler her seçenekle tartışın. Bu makalede amaçları doğrultusunda, veri katmanı olarak tek bir veritabanına bağlı bir web sitesi oluşan basit bir uygulama kullanacağız. Amacımız sürüm 1, sürüm 2 son kullanıcı deneyimi üzerinde önemli bir etkisi olmadan uygulamanın yükseltmektir. 

Yükseltme seçenekleri değerlendirirken, aşağıdaki etmenleri dikkate alın:

* Yükseltme sırasında uygulama kullanılabilirliği üzerindeki etkisini. Ne kadar süreyle uygulama işlevi sınırlı düşürülmüş veya olabilir.
* Yükseltmenin başarısız olması durumunda geri yeteneği.
* Yükseltme sırasında ilgisiz geri dönülemez bir hata oluşursa, uygulamanın bir güvenlik açığı.
* Maliyet toplam dolar.  Bu ek artıklık ve yükseltme işlemi tarafından kullanılan geçici bileşenlerinin artımlı maliyetlerini içerir. 

## <a name="upgrading-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Olağanüstü durum kurtarma için veritabanı yedeklemeleri kullanan uygulamalar yükseltme
Uygulamanızın üzerinde otomatik veritabanı yedeklemeyi kullanır ve olağanüstü durum kurtarma için coğrafi geri yükleme kullanıyorsa, genellikle tek bir Azure bölgesine dağıtılır. Bu durumda yükseltme işlemi, tüm uygulama bileşenleri yedekleme dağıtımını yükseltme katılan oluşturma içerir. Son kullanıcı kesintiyi en aza indirmek için yük devretme profiliyle Azure trafik Yöneticisi (WATM) özelliğinden yararlanır.  Aşağıdaki diyagramda, yükseltme işlemi önce işletimsel ortamı gösterilmektedir. Uç nokta <i>contoso 1.azurewebsites.net</i> yükseltilmesi gereken uygulamanın üretim yuvasını temsil eder. Geri alma olanağı sağlamak için yükseltme aşaması yuvası tam olarak eşitlenen bir kopyasını uygulama ile oluşturduğunuz. Aşağıdaki adımları uygulama yükseltme için hazırlamak üzere gereklidir:

1. Yükseltme için hazırlamak yuva oluşturun. İçin ikincil bir veritabanı (1) oluşturmak ve aynı Azure bölgesinde özdeş bir web sitesi dağıtımı yapın. Dengeli dağıtım işlemi tamamladığını görmek için ikincil izleyin.
2. WATM'ye ile bir yük devretme profili <i>contoso 1.azurewebsites.net</i> çevrimiçi uç noktası olarak ve <i>contoso 2.azurewebsites.net</i> çevrimdışı olarak. 

> [!NOTE]
> Hazırlık adımları üretim yuvasına uygulamada etkilemez ve tam erişim modunda çalışabilen unutmayın.
>  

![SQL veritabanı Git çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-1.png)

Hazırlık adımları tamamladıktan sonra gerçek yükseltme için hazır bir uygulamadır. Aşağıdaki diyagram yükseltme işleminde adımlarını gösterir. 

1. Birincil veritabanı salt okunur modda (3) üretim yuvasına ayarlayın. Bu uygulama (V1) üretim örneği, salt okunur böylece farklılıklara V1 ve V2 veritabanı örnekleri arasında önleme yükseltme sırasında kalacağına garanti.  
2. Planlanan sonlandırma modu (4) kullanılarak ikincil veritabanı bağlantısını kesin. Birincil veritabanı tam olarak eşitlenen bağımsız bir kopyasını oluşturur. Bu veritabanı yükseltilir.
3. Birincil veritabanına okuma-yazma modunda açın ve hazırlama yuvası (5) yükseltme komut dosyası çalıştırın.     

![SQL Database coğrafi çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-2.png)

Yükseltme başarıyla tamamlandı, artık son kullanıcıların aşamalı kopyalama uygulama geçiş yapmak hazırsınız. Şimdi uygulamayı üretim yuvasına haline gelir.  Bu, aşağıdaki diyagramda gösterildiği gibi birkaç adım içerir.

1. WATM'ye profilde çevrimiçi endpoint geçiş <i>contoso 2.azurewebsites.net</i>, web sitesi (6) V2 sürümüne işaret eder. Şimdi V2 uygulama üretim yuvasıyla olur ve son kullanıcı trafiği için yönlendirilir.  
2. Güvenli bir şekilde şekilde V1 uygulama bileşenleri artık ihtiyacınız varsa bunları (7) kaldırın.   

![SQL Database coğrafi çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-3.png)

Yükseltme işlemi başarısız olursa, örneğin yükseltme komut dosyası bir hata nedeniyle aşama yuvası sayılacağı tehlikeye. Uygulama yükseltme öncesi durumuna geri almak için yalnızca tam erişim üretim yuvasına uygulamaya geri dönün. Sonraki diyagramı adımlar gösterilmektedir.    

1. Veritabanı kopyasını okuma-yazma modunda (8) ayarlayın. Bu, üretim yuvasında işlevsel olarak tam V1 geri yükler.
2. Kök neden analizi yapmak ve hazırlama yuvası (9) güvenliği aşılmış bileşenleri kaldırın. 

Bu noktada tam olarak işlevsel bir uygulamasıdır ve yükseltme adımlarının yinelenebilir.

> [!NOTE]
> Geri alma zaten işaret gibi WATM profilinde değişiklik gerektirmez <i>contoso 1.azurewebsites.net</i> etkin uç noktası olarak.
> 
> 

![SQL Database coğrafi çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option1-4.png)

Anahtar **avantajı** bu uygulamanın basit adımları kümesini kullanarak tek bir bölgedeki yükseltebilirsiniz seçeneğidir. Yükseltme dolar maliyeti oldukça düşüktür. Ana **kolaylığını** yükseltme sırasında yıkıcı bir hata oluşursa, kurtarma için yükseltme öncesi durumu uygulamanın farklı bir bölgeye ve Yedekleme'yi kullanarak veritabanını geri yeniden dağıtımı gerektireceğini olduğu coğrafi geri yükleme. Bu işlem, önemli kapalı kalma sürelerine neden olur.   

## <a name="upgrading-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Olağanüstü durum kurtarma için veritabanı coğrafi çoğaltma kullanan uygulamalar yükseltme
Uygulamanızı coğrafi çoğaltma iş sürekliliği için kullanırsa, etkin bir birincil bölge dağıtım ve yedekleme bölge bekleme bir dağıtımda sahip en az iki farklı bölgelere dağıtılır. Daha önce bahsedilen Etkenler ek olarak, yükseltme işlemi, etmeleri gerekir:

* Uygulama, yükseltme işlemi sırasında her zaman geri dönülemez hatalarından korumalı kalır
* Coğrafi olarak yedekli bileşenleri uygulamanın paralel etkin bileşenlerle yükseltilir

Bu hedefleri başarmanın tane aktif, üç yedekleme uç noktaları ile yük devretme profilini kullanarak Azure trafik Yöneticisi (WATM) özelliğinden yararlanır.  Aşağıdaki diyagramda, yükseltme işlemi önce işletimsel ortamı gösterilmektedir. Web siteleri <i>contoso 1.azurewebsites.net</i> ve <i>contoso dr.azurewebsites.net</i> üretim yuvasına uygulamasının tam coğrafi artıklık ile temsil eder. Geri alma olanağı sağlamak için yükseltme aşaması yuvası tam olarak eşitlenen bir kopyasını uygulama ile oluşturduğunuz. Yükseltme işlemi sırasında yıkıcı bir hata oluşması durumunda uygulama hızlıca kurtarılabileceğinden emin olmak gerektiğinden, aşama yuvası da coğrafi olarak yedekli olması gerekir. Aşağıdaki adımları uygulama yükseltme için hazırlamak üzere gereklidir:

1. Yükseltme için hazırlamak yuva oluşturun. İçin ikincil bir veritabanı (1) oluşturmak ve web sitesi aynı Azure bölgesinde özdeş bir kopyasını dağıtan yapın. Dengeli dağıtım işlemi tamamladığını görmek için ikincil izleyin.
2. Coğrafi olarak yedekli ikincil bir veritabanı aşama yuvasında tarafından coğrafi çoğaltma (Buna "coğrafi çoğaltma zincirleme" adı verilir) yedekleme bölgeye ikincil veritabanı oluşturun. Dengeli dağıtım işlemi tamamlanmış (3) olup olmadığını görmek için ikincil yedekleme izleyin.
3. Yedekleme bölgede web sitesi yedek bir kopyasını oluşturun ve coğrafi olarak yedekli ikincil (4) bağlayın.  
4. Ek uç noktaları ekleme <i>contoso 2.azurewebsites.net</i> ve <i>contoso 3.azurewebsites.net</i> WATM çevrimdışı uç noktalar (5) olarak yük devretme profiline. 

> [!NOTE]
> Hazırlık adımları üretim yuvasına uygulamada etkilemez ve tam erişim modunda çalışabilen unutmayın.
> 
> 

![SQL Database coğrafi çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-1.png)

Hazırlık adımları tamamlandığında, aşama yuvası yükseltme için hazır olur. Aşağıdaki diyagram, yükseltme adımlarını gösterir.

1. Birincil veritabanı salt okunur modda (6) üretim yuvasına ayarlayın. Bu uygulama (V1) üretim örneği, salt okunur böylece farklılıklara V1 ve V2 veritabanı örnekleri arasında önleme yükseltme sırasında kalacağına garanti.  
2. Planlanan sonlandırma modunu (7) kullanarak aynı bölgede ikincil veritabanı bağlantısını kesin. Birincil sonlanmasından sonra otomatik olarak olacak birincil veritabanı tam olarak eşitlenen bağımsız bir kopyasını oluşturur. Bu veritabanı yükseltilir.
3. Birincil veritabanını okuma-yazma moduna aşama yuvadaki açın ve yükseltme komut dosyası (8) çalıştırın.    

![SQL Database coğrafi çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-2.png)

Yükseltme başarıyla tamamlandı, şimdi son kullanıcılar uygulamayı V2 sürümüne geçiş hazır olduğunuzda. Aşağıdaki diyagram adımlarını gösterir.

1. WATM'ye profilde etkin uç nokta geçiş <i>contoso 2.azurewebsites.net</i>, şimdi web sitesi (9) V2 sürümüne işaret eder. Şimdi V2 uygulama üretim yuvasıyla olur ve son kullanıcı trafiği için yönlendirilir. 
2. Güvenli bir şekilde şekilde V1 uygulama artık ihtiyacınız varsa onu (10 ve 11) kaldırın.  

![SQL Database coğrafi çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-3.png)

Yükseltme işlemi başarısız olursa, örneğin yükseltme komut dosyası bir hata nedeniyle aşama yuvası sayılacağı tehlikeye. Uygulama yükseltme öncesi durumuna geri almak için yalnızca tam erişimi olan üretim yuvasındaki uygulamayı kullanmaya geri dönün. Sonraki diyagramı adımlar gösterilmektedir.    

1. Birincil veritabanı kopyasını okuma-yazma modunda (12) üretim yuvasına ayarlayın. Bu, üretim yuvasında işlevsel olarak tam V1 geri yükler.
2. Kök neden analizi yapmak ve güvenliği aşılan bileşenleri aşama yuvada (13 ve 14) kaldırın. 

Bu noktada tam olarak işlevsel bir uygulamasıdır ve yükseltme adımlarının yinelenebilir.

> [!NOTE]
> Geri alma zaten işaret gibi WATM profilinde değişiklik gerektirmez <i>contoso 1.azurewebsites.net</i> etkin uç noktası olarak.
> 
> 

![SQL Database coğrafi çoğaltma yapılandırması. Bulut olağanüstü durum kurtarma.](media/sql-database-manage-application-rolling-upgrade/Option2-4.png)

Anahtar **avantajı** bu uygulama ve onun coğrafi olarak yedekli kopyalama paralel yükseltme sırasında iş sürekliliği ödün vermeden yükseltebilirsiniz, seçeneğidir. Ana **kolaylığını** her uygulama bileşeninin çift artıklığı gerektirir ve bu nedenle daha yüksek dolar maliyet oluşturur. Ayrıca, daha karmaşık bir iş akışı içerir. 

## <a name="summary"></a>Özet
Makalede açıklanan iki yükseltme yöntemlerini karmaşıklığı farklıdır ve son kullanıcı salt okunur işlemler için sınırlı olduğunda süresini en aza indirerek üzerinde maliyet dolar ancak her iki odaklanabilirsiniz. Bu süre doğrudan yükseltme komut dosyası süreye göre tanımlanır. Veritabanı boyutu, seçtiğiniz hizmet katmanı, web sitesi yapılandırması ve kolayca denetleyemezsiniz diğer etkenlere bağlı değildir. Tüm hazırlık adımları yükseltme yer alan adımları ayrılmış ve üretim uygulama etkilemeden yapılabilir olmasıdır. Yükseltme komut dosyası verimliliğini yükseltmeler sırasında son kullanıcı deneyimini belirleyen önemli bir faktördür. Bu nedenle geliştirmek en iyi yükseltme komut dosyası olarak verimli hale çabalarınız odaklanan yoludur.  

## <a name="next-steps"></a>Sonraki adımlar
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Veritabanı Yedeklemeleri otomatik Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı yedeklemeleri otomatik](sql-database-automated-backups.md).
* Kurtarma için otomatik yedeklemeler kullanma hakkında bilgi edinmek için bkz: [bir veritabanını otomatik yedeklerden geri](sql-database-recovery-using-backups.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [aktif coğrafi çoğaltma](sql-database-geo-replication-overview.md).


