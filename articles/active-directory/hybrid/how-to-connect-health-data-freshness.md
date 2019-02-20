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
ms.openlocfilehash: 0ad829b976d8b712ee8027c89fb618c6c07de1bc
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429040"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Sistem sağlığı hizmeti verileri güncel uyarı değil

## <a name="overview"></a>Genel Bakış
Azure AD Connect Health düzenli aralıklarla izler şirket içi makinelere aracılar, Azure AD Connect Health hizmeti için veri yükler. Portalı'nda sunulan bilgiler, hizmet Aracısı'ndan veri almazsa, eski olacaktır. Hizmet sorunu vurgulamak için oluşturacağı **sistem sağlığı hizmeti verileri güncel değil** uyarı. Bu hizmet son iki saat içinde veri almadı olduğunda oluşturulur.  

* **Uyarı** Connect Health kısmi veri öğeleri sunucudan iki saat boyunca gönderilen almazsa durumu uyarı tetikler. Uyarı durumu, Kiracı yöneticisine e-posta bildirimlerini tetiklenmiyor
* **Hata** Connect Health, iki saatlik sunucusundan onlara gönderilen veri öğeleri almazsa durumu uyarı tetikler. Hata durumu uyarı Tetikleyicileri e-posta bildirimleri için Kiracı Yöneticisi


## <a name="troubleshooting-steps"></a>Sorun giderme adımları 

> [!IMPORTANT] 
> Bu uyarı Connect Health aşağıdaki [veri bekletme ilkesi](reference-connect-health-user-privacy.md#data-retention-policy)

* Azure AD Connect Health aracılarını hizmetlerin makinede çalışır durumda olduğundan emin olun. Örneğin, AD FS için Connect Health, üç hizmet olması gerekir.  
  ![Azure AD Connect Health'i doğrulama](./media/how-to-connect-health-agent-install/install5.png)

* Azure portalına ve karşılamak emin [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements).
* Kullanım [test bağlantısı aracı](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) bağlantı sorunları bulmak için.
* Bir HTTP Proxy'si varsa, bunları izleyin [yapılandırma adımları](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 

Uyarı ayrıntısı dikey eksik veri çıkışı bir sunucudan öğeyi/öğeleri listeler. Aşağıdaki tabloda daha ayrıntılı sorun daraltın yardımcı olur. 
### <a name="connect-health-for-sync"></a>Eşitleme için connect Health

| Veri öğeleri | Sorun giderme adımları |
| --- | --- | 
| PerfCounter | - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Giden trafik için SSL incelemesi filtrelenmiş ya da devre dışı](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
| AadSyncService-SynchronizationRules <br /> AadSyncService-bağlayıcılar <br /> AadSyncService-GlobalConfigurations <br /> AadSyncService-RunProfileResults <br /> AadSyncService-ServiceConfigurations <br /> AadSyncService-ServiceStatus | - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>AD FS için connect Health

İş akışında izleyin ve doğrulamak için AD FS için ek adımlar [AD FS Yardım](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

| Veri öğeleri | Sorun giderme adımları |
| --- | --- | 
| PerfCounter, TestResult | - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Giden trafik için SSL incelemesi filtrelenmiş ya da devre dışı](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |
|  Adfs-UsageMetrics | IP adreslerini temel alan giden bağlantı başvurduğu [Azure IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) | 

### <a name="connect-health-for-adds"></a>EKLER için connect Health

| Veri öğeleri | Sorun giderme adımları |
| --- | --- | 
| PerfCounter, ekler-TopologyInfo-Json, ortak test Json | - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) |


## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
