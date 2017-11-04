---
title: "Bulut hizmeti dağıtım sorunlarını giderme | Microsoft Docs"
description: "Bir bulut hizmeti Azure'a dağıtırken çalışabilir bazı yaygın sorunlar vardır. Bu makale, bunlardan bazıları çözümleri sağlar."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: 944a29aebf7abfe32a7789ab239718b1cd2d7b15
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Bulut hizmeti dağıtım sorunlarını giderme
Azure için bir bulut hizmeti uygulama paketini dağıtırken dağıtımından hakkında bilgi edinebilirsiniz **özellikleri** Azure portalında bölmesi. Bu bilgiler Azure desteklemek için yeni bir destek isteği açarken sağlayabilir ve bulut hizmetinin ilgili sorunları gidermenize yardımcı olması için bu bölmede ayrıntıları kullanabilirsiniz.

Bulabileceğiniz **özellikleri** şekilde bölmesi:

* Azure portalında, bulut hizmeti dağıtımının tıklatın, **tüm ayarları**ve ardından **özellikleri**.
* Klasik Azure portalında, bulut hizmeti dağıtımının tıklatın, **PANO**, sayfanın sağ alt köşesinde bulunan (altında **Hızlı Bakış**). Bu bölmede yok "Özellikler" etiketli olduğunu unutmayın.

> [!NOTE]
> İçeriğini kopyaladığınız **özellikleri** bölmesi sağ üst köşesindeki simgesini tıklatarak panoya bölmesi.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Sorun: my Web sitesi dosyasına erişemiyor, ancak my dağıtım başlatılır ve tüm rol örneklerini hazır
Portalda gösterilen Web sitesi URL'si bağlantı bağlantı noktası içermez. Web siteleri için varsayılan bağlantı noktası 80'dir. Uygulamanız farklı bir bağlantı noktası çalıştırmak için yapılandırılmışsa, doğru bağlantı noktası numarasını URL için Web sitesi erişirken eklemelisiniz.

1. Azure portalında, bulut hizmeti dağıtımının'ı tıklatın.
2. İçinde **özellikleri** bölmesinde Azure portal'ın rol örnekleri için bağlantı noktalarını denetleyin (altında **giriş uç noktaları**).
3. Bağlantı noktası 80 değilse, doğru bağlantı noktası değeri uygulama eriştiğinde URL'sini ekleyin. Varsayılan olmayan bağlantı noktası belirtmek için bağlantı noktası numarasıyla boşluk ve ardından iki nokta (:) ve ardından URL'yi yazın.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Sorun: hiçbir şey bana geri dönüşüm My rol örnekleri
Azure sorun düğümleri algılar ve bu nedenle rol örnekleri için yeni düğümler taşır iyileştirme hizmeti otomatik olarak oluşur. Bu durumda, rolü örneklerinizi otomatik olarak geri dönüştürme görebilirsiniz. Hizmet onarma oluştu olmadığını öğrenmek için:

1. Azure portalında, bulut hizmeti dağıtımının'ı tıklatın.
2. İçinde **özellikleri** Azure portal'ın bölmesinde bilgileri gözden geçirin ve hizmet onarma geri dönüştürme rolleri gözlemlenen süre boyunca oluşup oluşmadığını belirleyin.

Rolleri aynı zamanda kabaca ayda bir kez ana bilgisayar işletim sistemi ve konuk işletim sistemi güncelleştirmeleri sırasında geri.  
Daha fazla bilgi için blog gönderisine bakın [rol örneği yeniden son işletim sistemi yükseltme](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Sorun: t olamaz bir VIP takası yapın ve bir hata alıyorsunuz
Dağıtım güncelleştirme işlemi devam ediyorsa bir VIP takası izin verilmiyor. Dağıtım güncelleştirmeleri otomatik olarak oluşabilir zaman:

* Yeni bir konuk işletim sistemi kullanılabilir ve otomatik güncelleştirmeler için yapılandırılır.
* Hizmet iyileştirme oluşur.

Bir otomatik güncelleştirme bir VIP takası engel engellemediğini öğrenmek için:

1. Azure portalında, bulut hizmeti dağıtımının'ı tıklatın.
2. İçinde **özellikleri** bölmesinde değeri, Azure portalında, Ara **durum**. Eğer öyleyse **hazır**, ardından denetleyin **son işlem** biri son meydana geldiği olmadığını görmek için VIP takas engel olabilir.
3. Adım 1 ve 2 üretim dağıtımı için yineleyin.
4. Bir otomatik güncelleştirme işlemi varsa, VIP takas yapmak denemeden önce tamamlanmasını bekleyin.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Sorun: Rol örneği başlatıldı, başlatılıyor, meşgul ve durdurulmuş arasında döngü
Bu durum, uygulama kodunuz, paketiniz veya yapılandırma dosyanızla ilgili bir sorundan kaynaklanıyor olabilir. Bu durumda, birkaç dakikada değiştirme durum görmeye olmalıdır ve Azure portalını şöyle gözükebilir **geri dönüştürme**, **meşgul**, veya **başlatılıyor**. Bu olduğunu rol örneği çalışmasını engelliyor uygulamayla yanlış bir şeyler gösterir.

Bu sorun için sorun giderme hakkında daha fazla bilgi için blog gönderisine bakın [Azure PaaS işlem tanılama verilerini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) ve [rollerin geri dönüştürülmesine neden sık karşılaşılan sorunları](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Sorun: Uygulamam durdu
1. Azure portalında rol örneği'ı tıklatın.
2. İçinde **özellikleri** bölmesinde Azure portal'ın, sorununuzu çözmek için aşağıdaki koşulları göz önünde bulundurun:
   * Rol örneği yakın zamanda durdurulmuşsa (değerini kontrol edebilirsiniz **Abort sayısı**), dağıtım güncelleştirme. Rol örneği, kendi işlevine devam olmadığını görmek için bekleyin.
   * Rol örneği ise **meşgul**, uygulama kodunuz olup olmadığını denetleyin [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) olayı işlenir. Eklemek veya bu olayını işler biraz kod düzeltmek gerekebilir.
   * Tanılama verilerine gidin ve sorun giderme senaryoları blog postasında [Azure PaaS işlem tanılama verilerini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> Bulut hizmetinizi Geri Dönüşüm, etkili bir şekilde özgün sorun bilgilerini silme dağıtım özelliklerini sıfırlayın.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi görüntülemek [sorun giderme makalelerini](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-allocation-failures) bulut Hizmetleri için.

Azure PaaS bilgisayar tanılama verilerini kullanarak bulut hizmeti rolü sorunlarını giderme konusunda bilgi almak için bkz: [Kevin Williamson'ın blog dizisini](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
