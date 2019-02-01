---
title: Azure AD Connect Health'i - sistem sağlığı hizmeti verileri değil en fazla tarih uyarı | Microsoft Docs
description: Bu belgede nedenini "sistem sağlığı hizmeti verileri güncel değil" uyarısı ve nasıl giderileceği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: zhiweiwangmsft
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 3f97e9696a5138d9102037103a45c86988a7506c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55489284"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Sistem sağlığı hizmeti verileri güncel uyarı değil

## <a name="overview"></a>Genel Bakış
<li>Azure AD Connect Health, tüm veri noktalarına sunucudan iki saatlik almaz yeni veri uyarısı oluşturur. Uyarı başlığı **sistem sağlığı hizmeti verileri güncel değil**. </li>
<li>**Uyarı** Connect Health kısmi veri öğeleri sunucudan iki saat boyunca gönderilen almazsa durumu uyarı tetikler. Uyarı durumu, Kiracı yöneticisine e-posta bildirimlerini tetiklenmiyor </li>
<li>**Hata** Connect Health, iki saatlik sunucusundan onlara gönderilen veri öğeleri almazsa durumu uyarı tetikler. Hata durumu uyarı Tetikleyicileri e-posta bildirimleri için Kiracı Yöneticisi </li>

>[!IMPORTANT] 
> Bu uyarı Connect Health aşağıdaki [veri bekletme ilkesi](reference-connect-health-user-privacy.md#data-retention-policy)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları 
* Azure portalına ve karşılamak emin [gereksinimler bölümüne](how-to-connect-health-agent-install.md#requirements).
* Kullanım [test bağlantısı aracı](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) bağlantı sorunları bulmak için.
* HTTP Proxy varsa, lütfen izleyin [yapılandırma adımları burada](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy). 

### <a name="connect-health-for-sync"></a>Eşitleme için connect Health

| Veri öğeleri | Sorun giderme adımları |
| --- | --- | 
| PerfCounter | - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Giden trafik için SSL incelemesi filtrelenmiş ya da devre dışı](https://technet.microsoft.com/library/ee796230.aspx) <br /> - [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [IE Artırılmış Güvenlik etkinse, belirlenen Web sitelerine izin ver](https://technet.microsoft.com/windows/ms537180(v=vs.60)) |
| AadSyncService-SynchronizationRules <br /> AadSyncService-bağlayıcılar <br /> AadSyncService-GlobalConfigurations <br /> AadSyncService-RunProfileResults <br /> AadSyncService-ServiceConfigurations <br /> AadSyncService-ServiceStatus | -Giden bağlantı IP adreslerine göre başvurduğu [Azure IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) <br /> - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> -  [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) | 

### <a name="connect-health-for-adfs"></a>AD FS için connect Health

İş akışında izleyin ve doğrulamak için AD FS için ek adımlar [AD FS Yardım](https://adfshelp.microsoft.com/TroubleshootingGuides/Workflow/3ef51c1f-499e-4e07-b3c4-60271640e282).

| Veri öğeleri | Sorun giderme adımları |
| --- | --- | 
| PerfCounter, TestResult | - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br />- [Giden trafik için SSL incelemesi filtrelenmiş ya da devre dışı](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [IE Artırılmış Güvenlik etkinse, belirlenen Web sitelerine izin ver](https://technet.microsoft.com/windows/ms537180(v=vs.60)) |
|  Adfs-UsageMetrics | IP adreslerini temel alan giden bağlantı başvurduğu [Azure IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653) | 

### <a name="connect-health-for-adds"></a>EKLER için connect Health

| Veri öğeleri | Sorun giderme adımları |
| --- | --- | 
| PerfCounter, ekler-TopologyInfo-Json, ortak test Json | - [Azure hizmet uç noktasına giden bağlantı](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections) <br /> - [Giden trafik için SSL incelemesi filtrelenmiş ya da devre dışı](https://technet.microsoft.com/library/ee796230.aspx) <br />-  [Aracıyı çalıştıran sunucudaki güvenlik duvarı bağlantı noktaları](https://technet.microsoft.com/library/ms345310(v=sql.100).aspx) <br /> - [IE Artırılmış Güvenlik etkinse, belirlenen Web sitelerine izin ver](https://technet.microsoft.com/windows/ms537180(v=vs.60)) <br />  -Giden bağlantı IP adreslerine göre başvurduğu [Azure IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653)  |


## <a name="next-steps"></a>Sonraki adımlar
* [Azure AD Connect Health ile ilgili SSS](reference-connect-health-faq.md)
