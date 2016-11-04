---
title: Azure Güvenlik Merkezi'nde iş ortağı çözümlerini yönetme | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nin Azure aboneliğinizle tümleşik iş ortağı çözümlerinizin sistem durumunu bir bakışta nasıl izleyeceğiniz konusunda size rehberlik sağlar.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: ''

ms.service: security-center
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/22/2016
ms.author: terrylan

---
# Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme
Bu belge, Azure Güvenlik Merkezi'ndeki iş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz konusunda size rehberlik sağlar.

> [!NOTE]
> Bu belgedeki bilgiler Azure Güvenlik Merkezi önizleme sürümü için geçerlidir. Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır. Bu, adım adım ilerleyen bir kılavuz değildir.
> 
> 

## Güvenlik Merkezi nedir?
 Güvenlik Merkezi, artırılmış görünürlük aracılığıyla tehditleri engellemenize, algılamanıza ve yanıtlamanıza, ayrıca Azure kaynaklarınızın güvenliğini denetlemenize yardımcı olur. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar, normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

## İş ortağı çözümlerini izleme
**Güvenlik Merkezi** dikey penceresindeki **İş ortağı çözümleri** kutucuğu, Azure aboneliğinizle tümleşik iş ortağı çözümlerinizin sistem durumunu bir bakışta nasıl izleyeceğiniz konusunda size rehberlik sağlar.
![İş ortağı çözümleri kutucuğu][1]

**İş ortağı çözümleri** kutucuğu, iş ortağı çözümü sayısını ve bu çözümler için durum özetini görüntüler.

İş ortağı çözümünün **DURUM**'u şu olabilir:

* Korumalı (yeşil) - sistem durumuyla ilgili bir sorun yok.
* Sağlıksız (kırmızı) - sistem durumunda hemen ilgilenilmesi gereken bir sorun var.
* Raporlama durduruldu (turuncu) - çözüm, sistem durumunu raporlamayı durdurdu.
* Bilinmeyen koruma durumu (turuncu) - var olan çözümle ilgili yeni kaynak ekleme işlemi başarısız olduğu için çözümün sistem durumu şu anda bilinmiyor.
* Bildirilmedi (gri) - çözümle ilgili herhangi bir şey bildirilmedi, çözümle ilgili bağlantı henüz kurulduysa ve hala dağıtılıyorsa çözümün durumu bildirilmeyebilir.

Aboneliğinizle tümleşik çözüm yoksa kutucuk, çözüm olmadığını belirtir. **İş ortağı çözümleri** kutucuğunu seçmek, iş ortağı güvenlik çözümlerini dağıtmak için **Öneriler** dikey penceresini etkinleştirmenizi sağlar.
![İş ortağı çözümü yok][2]

İş ortağı çözümlerinizin durumunu görüntülemek için:

1. **İş ortağı çözümleri** kutucuğunu seçin. Güvenlik Merkezi'ne bağlı iş ortağı çözümlerinizin listesini görüntüleyen dikey pencere açılır.
   ![İş ortağı çözümleri][3]
2. İş ortağı çözümü seçin. Bu örnekte **F5 WAF2** çözümünü seçelim.  İş ortağı çözümünün durumunu ve çözümle ilişkili kaynakları gösteren dikey pencere açılır. Bu çözüme ilişkin iş ortağı yönetimi deneyimini açmak için **Çözüm konsolunu** seçin.
   ![İş ortağı çözümü ayrıntısı][4]
3. **F5 WAF2** dikey penceresine geri gidin ve **Bağlantı uygulaması** seçeneğini seçin. **Bağlantı Uygulamaları** dikey penceresi açılır. Burada, kaynakları iş ortağı çözümüne bağlayabilirsiniz.
   ![İş ortağı çözümüne kaynakları bağlama][5]

## Sonraki adımlar
Bu belgeyle,Güvenlik Merkezi'ndeki **İş Ortağı Çözümleri** kutucuğuna giriş yaptınız. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) - Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) - Önerilerin Azure kaynaklarınızı korumanıza nasıl yardım edeceği hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](http://blogs.msdn.com/b/azuresecurity/) - En son Azure güvenlik haberlerini ve bilgilerini edinin.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/no-partner-solutions-to-display.png
[3]: ./media/security-center-partner-solutions/partner-solutions.png
[4]: ./media/security-center-partner-solutions/partner-solutions-detail.png
[5]: ./media/security-center-partner-solutions/link-applications.png



<!----HONumber=Jun16_HO2-->


