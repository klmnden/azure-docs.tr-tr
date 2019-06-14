---
title: Azure AD Connect Health'i - sistem sağlığı hizmeti verileri değil en fazla tarih uyarı | Microsoft Docs
description: Bu belgede nedenini "sistem sağlığı hizmeti verileri güncel değil" uyarısı ve nasıl giderileceği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: SamuelD
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ffd783ec41b1b0c4a11ee426648c1e36fbbbf75
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60349959"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Sistem sağlığı hizmeti verileri güncel uyarı değil

## <a name="overview"></a>Genel Bakış

Azure AD Connect Health düzenli aralıklarla izler şirket içi makinelere aracılar, Azure AD Connect Health hizmeti için verileri karşıya yükleyin. Hizmet Aracısı'ndan veri almazsa, portal sunar bilgi eski olacaktır. Hizmet sorunu vurgulamak için oluşturacağı **sistem sağlığı hizmeti verileri güncel değil** uyarı. Hizmet, son iki saat içinde eksiksiz bir veri almadı olduğunda bu uyarı oluşturulur.  

- **Uyarı** durum uyarısı tetikler sistem sağlığı hizmeti yalnızca aldıysa **kısmi** sunucudan son iki saat içinde gönderilen veri türleri. Uyarı durumu, yapılandırılmış alıcılara e-posta bildirimleri tetiklemez. 
- **Hata** sistem sağlığı hizmeti, son iki saat içinde sunucudan tüm veri türleri almadı, durumu uyarı tetikler. Hata durumu uyarı Tetikleyiciler yapılandırılmış alıcılara e-posta bildirimleri.

Hizmet, hizmet türüne bağlı olarak şirket içi makinelerde çalışan aracılardan gelen verileri alır. Aşağıdaki tabloda, makine, yaptıkları ve hizmet oluşturur veri türleri üzerinde çalışan aracıları listeler. Bazı durumlarda, birden çok hizmet mevcuttur işleminde yer alan için bunlardan herhangi birini sorunlu olabilir. 

## <a name="understanding-the-alert"></a>Uyarı anlama

**Uyarı ayrıntıları** ne zaman uyarı oluştu ve son algılama dikey pencerede gösterilir. Her iki saatte bir çalışan bir arka plan işlemi oluşturur ve uyarıyı yeniden değerlendirir. Aşağıdaki örnekte, 9: 59'da 03/10'da ilk uyarı oluştu. Uyarı, 10:00 ne zaman uyarı yeniden Hesaplandı'da hala 03/12 vardı. Dikey penceresinde, da sistem sağlığı hizmeti, belirli bir veri türüne en son aldığı zaman ayrıntıları. 
 
 ![Azure AD Connect Health uyarı ayrıntıları](./media/how-to-connect-health-data-freshness/data-freshness-details.png)
 
Aşağıdaki tabloda hizmet türleri karşılık gelen gerekli veri türlerine eşlenir:

| Hizmet türü | Aracı (Windows hizmeti adı) | Amaç | Oluşturulan veri türü  |
| --- | --- | --- | --- |  
| Azure AD Connect (eşitleme) | Azure AD Connect Health Eşitleme Öngörü Hizmeti | AAD Connect özgü bilgileri toplama (bağlayıcılar, eşitleme kuralları, vb.) | AadSyncService SynchronizationRules <br />  AadSyncService bağlayıcılar <br /> AadSyncService GlobalConfigurations  <br />  - AadSyncService-RunProfileResults <br /> - AadSyncService-ServiceConfigurations <br /> - AadSyncService-ServiceStatus   |
|  | Azure AD Connect Health Eşitleme İzleme Hizmeti | AAD Connect özgü performans sayaçları, ETW izlemelerini, dosyaları Topla | Performans sayacı |
| AD DS | Azure AD Connect Health AD DS Insights Hizmeti | Yapay testler gerçekleştirin, topoloji bilgilerini, çoğaltma meta verilerini topla |  Ekler-TopologyInfo-Json <br /> Ortak-test-(test sonuçlarını oluşturur) Json   | 
|  | Azure AD Connect Health AD DS İzleme Hizmeti | ADDS özgü performans sayaçları, ETW izlemelerini, dosyaları Topla | -Performans sayacı  <br /> Ortak-test-(test sonuçlarını yükler) Json  |
| AD FS | Azure AD Connect Health AD FS Tanılama Hizmeti | Yapay testleri gerçekleştir | TestResult (test sonuçlarını oluşturur) | 
| | Azure AD Connect Health AD FS Öngörü Hizmeti  | ADFS kullanım ölçümlerini Topla | Adfs-UsageMetrics |
| | Azure AD Connect Health AD FS İzleme Hizmeti | ADFS özgü performans sayaçları, ETW izlemelerini, dosyaları Topla | TestResult (test sonuçlarını yükler) |

## <a name="troubleshooting-steps"></a>Sorun giderme adımları 

Sorunu tanılamak için gereken adımlar aşağıda verilmiştir. İlk tüm hizmet türleri için ortak olan temel denetimler kümesidir. Her hizmet türü ve veri türü için belirli adımları listeleyen aşağıdaki tabloda. 

> [!IMPORTANT] 
> Bu uyarı Connect Health aşağıdaki [veri bekletme ilkesi](reference-connect-health-user-privacy.md#data-retention-policy)

* En son sürümlerini aracıları yüklü olduğundan emin olun. Görünüm [sürüm geçmişi](reference-connect-health-version-history.md). 
* Azure AD Connect Health aracılarını Hizmetleri olduğundan emin olun **çalıştıran** makine üzerinde. Örneğin, AD FS için Connect Health, üç hizmet olması gerekir.
  ![Azure AD Connect Health'i doğrulama](./media/how-to-connect-health-agent-install/install5.png)

* Azure portalına ve karşılamak emin [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements).
* Kullanım [test bağlantısı aracı](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) bağlantı sorunları bulmak için.
* Bir HTTP Proxy'si varsa, bunları izleyin [yapılandırma adımları](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 


## <a name="next-steps"></a>Sonraki adımlar
Yukarıdaki adımların hiçbirini tanımlanan bir sorun varsa düzeltin ve uyarıyı çözümlemek bekleyin. Uyarıyı çözmek için 2 saat sürer. Bu nedenle uyarı arka plan işlemi, her 2 saatte çalışır. 

* [Azure AD Connect Health veri bekletme ilkesi](reference-connect-health-user-privacy.md#data-retention-policy)
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
