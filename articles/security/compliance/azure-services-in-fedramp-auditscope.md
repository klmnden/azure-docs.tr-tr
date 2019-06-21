---
title: FedRAMP ve DoD SRG denetim kapsamı içinde Azure Hizmetleri
description: Bu makale, Azure genel ve Azure kamu için hangi FedRAMP (Orta vs. gösteren tablo içerir Yüksek) ve DoD SRG (2, 4 ve 5 etki düzeyi) denetim kapsamı belirli bir hizmete ulaştı.
author: davib
ms.author: davib
ms.date: 5/17/2019
ms.topic: article
ms.service: security
ms.reviewer: rochiou
ms.openlocfilehash: 922098a67d148d5860145663ea1c5bf543c8aaec
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67294838"
---
# <a name="azure-services-by-fedramp-and-dod-cc-srg-audit-scope"></a>Denetim kapsamı FedRAMP ve DoD CC SRG göre Azure Hizmetleri

Microsoft kamu bulut Hizmetleri ABD Federal Risk ve yetkilendirme yönetimi programı (FedRAMP) ve ABD Savunma Bakanlığı, bilgi etkisi Düzey 2'den 5 gereksinimleri karşılayın. Azure devlet kurumları, Office 365 ABD de dahil olmak üzere hizmetler dağıtarak korumalı Kamu ve Dynamics 365 kamu, federal ve savunma kurumları uyumlu Hizmetleri zengin bir dizi yararlanabilir.

Bu makalede ayrıntılı bir liste kapsamdaki bulut Hizmetleri için FedRAMP ve DoD CC SRG uyumluluk teklifi Azure'da ve Azure kamu sağlar.

#### <a name="terminologysymbols-used"></a>Kullanılan terminoloji/semboller

* DoD CC SRG Bakanlığı bulut güvenlik gereksinimleri Kılavuzu =
* IIL bilgi etki düzeyi =
* FedRAMP Federal Risk ve yetkilendirme yönetimi programı =  
* : heavy_check_mark: = Hizmet bu denetim kapsamı elde edilen gösterir.

## <a name="azure-public-services-by-audit-scope"></a>Azure genel Hizmetleri Denetim kapsama göre
| _Son güncelleme tarihi: Haziran 2019_ |

| Azure Hizmeti| DoD CC SRG IIL 2 | FedRAMP Orta | FedRAMP Yüksek | Planlanan 2019 |
| ------------ |:---------------:|:----------------:|:------------:|:------------:|
| [API Management](https://azure.microsoft.com/services/api-management/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Application Gateway](https://azure.microsoft.com/services/application-gateway/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Otomasyon](https://azure.microsoft.com/services/automation/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Active Directory (ücretsiz ve temel)](https://azure.microsoft.com/services/active-directory/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Active Directory (Premium P1 + P2)](https://azure.microsoft.com/services/active-directory/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) |  |  |  | :heavy_check_mark: |
| [Azure Active Directory Domain Services](https://azure.microsoft.com/services/active-directory-ds/) | |  |  | :heavy_check_mark: |
| [Azure Gelişmiş tehdit koruması](https://azure.microsoft.com/features/azure-advanced-threat-protection/) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Advisor](https://azure.microsoft.com/services/advisor/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure arşiv depolama](https://azure.microsoft.com/services/storage/archive/) |  |  |  | :heavy_check_mark: |
| [Azure kapsayıcı hizmeti](https://docs.microsoft.com/azure/container-service/) |  |  |  | :heavy_check_mark: |
| [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Data Box](https://azure.microsoft.com/services/databox/) | |  |  | :heavy_check_mark: |
| [Azure Data Lake Storage Gen1](https://azure.microsoft.com/services/storage/data-lake-storage/) |  |  |  | :heavy_check_mark: |
| [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [MariaDB için Azure Veritabanı](https://azure.microsoft.com/services/mariadb/) |  |  |  | :heavy_check_mark: |
| [Azure veritabanı geçiş hizmeti](https://azure.microsoft.com/services/database-migration/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Databricks](https://azure.microsoft.com/services/databricks/) |  |  |  |  |
| [Azure DDoS koruması](https://azure.microsoft.com/services/ddos-protection/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure ayrılmış HSM](https://azure.microsoft.com/services/azure-dedicated-hsm/) |  |  |  | :heavy_check_mark: |
| [Azure DevOps (eski adıyla VSTS)](https://azure.microsoft.com/services/devops/) | |  |  | |
| [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/) |  |  |  | :heavy_check_mark: |
| [Azure DNS](https://azure.microsoft.com/services/dns/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure dosya eşitleme](https://azure.microsoft.com/services/storage/files/) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure ön kapısı](https://azure.microsoft.com/services/frontdoor/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Information Protection](https://azure.microsoft.com/services/information-protection/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Intune](https://docs.microsoft.com/intune/what-is-intune) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Kubernetes Service'i (AKS)](https://azure.microsoft.com/services/kubernetes-service/) | |  |  | :heavy_check_mark: |
| [Azure Haritalar](https://azure.microsoft.com/services/azure-maps/) |  |  |  | :heavy_check_mark: |
| [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure İzleyici](https://azure.microsoft.com/services/monitor/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure İlkesi](https://azure.microsoft.com/services/azure-policy/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Search](https://azure.microsoft.com/services/search/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Azure Service Manager (RDFE)](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100)) | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Backup](https://azure.microsoft.com/services/backup/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Batch](https://azure.microsoft.com/services/batch/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Cloud Services](https://azure.microsoft.com/services/cloud-services/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Görüntü İşleme](https://azure.microsoft.com/services/cognitive-services/computer-vision/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Content Moderator](https://azure.microsoft.com/services/cognitive-services/content-moderator/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Özel görüntü işleme](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Yüz tanıma](https://azure.microsoft.com/services/cognitive-services/face/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Dil anlama](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Soru-cevap Oluşturucu](https://azure.microsoft.com/services/cognitive-services/qna-maker/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Konuşma Hizmetleri](https://azure.microsoft.com/services/cognitive-services/directory/speech/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Metin Analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Translator Metin Çevirisi](https://azure.microsoft.com/services/cognitive-services/speech-translation/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Bilişsel hizmetler: Video Indexer](https://azure.microsoft.com/services/media-services/video-indexer/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Container Instances](https://azure.microsoft.com/services/container-instances/) |  |  |  | :heavy_check_mark: |
| [Container Registry](https://azure.microsoft.com/services/container-registry/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [İçerik teslim ağı](https://azure.microsoft.com/services/cdn/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Veri Kataloğu](https://azure.microsoft.com/services/data-catalog/) |  |  |  | :heavy_check_mark: |
| [Data Factory](https://azure.microsoft.com/services/data-factory/) |  |  |  | :heavy_check_mark: |
| [Event Grid](https://azure.microsoft.com/services/event-grid/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Defender Gelişmiş tehdit koruması](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Event Hubs](https://azure.microsoft.com/services/event-hubs/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [ExpressRoute](https://azure.microsoft.com/services/expressroute/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Akış](https://docs.microsoft.com/flow/getting-started) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [İşlevler](https://azure.microsoft.com/services/functions/) |  |  |  | :heavy_check_mark: |
| [HDInsight](https://azure.microsoft.com/services/hdinsight/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [İçeri / dışarı aktarma](https://azure.microsoft.com/services/storage/import-export/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [IoT Central](https://azure.microsoft.com/services/iot-central/) |  |  |  | :heavy_check_mark: |
| [IoT Hub’ı](https://azure.microsoft.com/services/iot-hub/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Yük Dengeleyici](https://azure.microsoft.com/services/load-balancer/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Logic Apps](https://azure.microsoft.com/services/logic-apps/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Machine Learning Hizmetleri](https://azure.microsoft.com/services/machine-learning-service/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Machine Learning Studio'da](https://azure.microsoft.com/services/machine-learning-studio/) |  |  |  | :heavy_check_mark: |
| [Media Services](https://azure.microsoft.com/services/media-services/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Microsoft Azure portal](https://azure.microsoft.com/features/azure-portal/)| :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Microsoft Cloud App Security'ye](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Microsoft Genomiks](https://azure.microsoft.com/services/genomics/) |  |  |  | :heavy_check_mark: |
| [Microsoft PowerApps](https://docs.microsoft.com/powerapps/powerapps-overview) | | :heavy_check_mark: | :heavy_check_mark: |  |
| [Microsoft Stream](https://docs.microsoft.com/stream/overview) | | :heavy_check_mark: | :heavy_check_mark: |  |
| [Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ağ İzleyicisi](https://azure.microsoft.com/services/network-watcher/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ağ İzleyicisi trafik analizi](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Notification Hubs](https://azure.microsoft.com/services/notification-hubs/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Power BI Embedded](https://azure.microsoft.com/services/power-bi-embedded/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Redis Önbelleği](https://azure.microsoft.com/services/cache/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Scheduler](https://azure.microsoft.com/services/scheduler/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Service Bus](https://azure.microsoft.com/services/service-bus/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Service Fabric](https://azure.microsoft.com/services/service-fabric/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [SQL Veri Ambarı](https://azure.microsoft.com/services/sql-data-warehouse/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [SQL Server Stretch Database](https://azure.microsoft.com/services/sql-server-stretch-database/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Depolama: Blobs](https://azure.microsoft.com/services/storage/blobs/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Depolama: Diskler (dahil olmak üzere yönetilen diskler)](https://azure.microsoft.com/services/storage/disks/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Depolama: Dosyaları](https://azure.microsoft.com/services/storage/files/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Depolama: Kuyruklar](https://azure.microsoft.com/services/storage/queues/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Depolama: Tabloları](https://azure.microsoft.com/services/storage/tables/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [StorSimple](https://azure.microsoft.com/services/storsimple/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Akış Analizi](https://azure.microsoft.com/services/stream-analytics/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Sanal Makine Ölçek Kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Sanal makineler (dahil ayrılmış örnekler)](https://azure.microsoft.com/services/virtual-machines/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Sanal Ağ](https://azure.microsoft.com/services/virtual-network/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [VPN Gateway](https://azure.microsoft.com/services/vpn-gateway/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Web uygulamaları (uygulama hizmeti)](https://azure.microsoft.com/services/app-service/web/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |


## <a name="azure-government-services-by-audit-scope"></a>Azure kamu hizmetlerinde denetim kapsama göre
| _Son güncelleme tarihi: Haziran 2019_ |

| Azure Hizmeti | DoD CC SRG IIL 2 | DoD CC SRG IIL 4 | DoD CC SRG IIL 5 | FedRAMP Yüksek | Planlanan 2019
| ------------- |:---------------:|:---------------:|:---------------:|:------------:|:------------:
| [API Management](https://azure.microsoft.com/services/api-management/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Application Gateway](https://azure.microsoft.com/services/application-gateway/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) |  |  |  |  | :heavy_check_mark:
| [Otomasyon](https://azure.microsoft.com/services/automation/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: |
| [Azure Active Directory (ücretsiz ve temel)](https://azure.microsoft.com/services/active-directory/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Active Directory (Premium P1 + P2)](https://azure.microsoft.com/services/active-directory/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Advisor](https://azure.microsoft.com/services/advisor/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Bot Hizmeti](https://docs.microsoft.com/azure/bot-service/) |  |  |  |  | :heavy_check_mark:
| [Container Registry](https://azure.microsoft.com/services/container-registry/) |  |  |  |  | :heavy_check_mark:
| [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Data Factory](https://azure.microsoft.com/services/data-factory/)  |  |  |  |  | :heavy_check_mark:
| [MySQL için Azure DB](https://azure.microsoft.com/services/mysql/)| :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [PostgreSQL için Azure DB](https://azure.microsoft.com/services/postgresql/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [MariaDB için Azure DB](https://azure.microsoft.com/services/mariadb/)  |  |  |  |  | :heavy_check_mark:
| [Azure DDoS koruması](https://azure.microsoft.com/services/ddos-protection/) |  |  |  |  | :heavy_check_mark:
| [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/)  |  |  |  |  | :heavy_check_mark:
| [Azure DNS](https://azure.microsoft.com/services/dns/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Event Grid](https://azure.microsoft.com/services/event-grid/)  |  |  |  |  | :heavy_check_mark:
| [Azure dosya eşitleme](https://azure.microsoft.com/services/storage/files/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Azure ön kapısı](https://azure.microsoft.com/services/frontdoor/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Information Protection](https://azure.microsoft.com/services/information-protection/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Azure Intune](https://docs.microsoft.com/intune/what-is-intune) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Azure Lab Services'i](https://azure.microsoft.com/services/lab-services/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Geçişi](https://azure.microsoft.com/services/azure-migrate/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Azure İzleyici](https://azure.microsoft.com/services/monitor/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure İlkesi](https://azure.microsoft.com/services/azure-policy/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: **&ast;** | :heavy_check_mark: |
| [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Azure Service Manager (RDFE)](https://docs.microsoft.com/previous-versions/azure/ee460799(v=azure.100)) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Azure Site Recovery](https://azure.microsoft.com/services/site-recovery/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Backup](https://azure.microsoft.com/services/backup/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Batch](https://azure.microsoft.com/services/batch/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Cloud Services](https://azure.microsoft.com/services/cloud-services/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Bilişsel hizmetler: Görüntü İşleme](https://azure.microsoft.com/services/cognitive-services/computer-vision/)  |  |  |  |  | :heavy_check_mark:
| [Bilişsel hizmetler: Content Moderator](https://azure.microsoft.com/services/cognitive-services/content-moderator/)  |  |  |  |  | :heavy_check_mark:
| [Bilişsel hizmetler: Yüz tanıma](https://azure.microsoft.com/services/cognitive-services/face/)  |  |  |  |  | :heavy_check_mark:
| [Bilişsel hizmetler: Dil anlama](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/)  |  |  |  |  | :heavy_check_mark:
| [Bilişsel hizmetler: Metin Analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/)  |  |  |  |  | :heavy_check_mark:
| [Bilişsel hizmetler: Translator Metin Çevirisi](https://azure.microsoft.com/services/cognitive-services/speech-translation/) |  |  |  |  | :heavy_check_mark:
| [Event Hubs](https://azure.microsoft.com/services/event-hubs/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [ExpressRoute](https://azure.microsoft.com/services/expressroute/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Akış](https://docs.microsoft.com/flow/getting-started) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [İşlevler](https://azure.microsoft.com/services/functions/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [HDInsight](https://azure.microsoft.com/services/hdinsight/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [İçeri / dışarı aktarma](https://azure.microsoft.com/services/storage/import-export/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [IoT Hub’ı](https://azure.microsoft.com/services/iot-hub/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Anahtar Kasası](https://azure.microsoft.com/services/key-vault/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Yük Dengeleyici](https://azure.microsoft.com/services/load-balancer/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Logic Apps](https://azure.microsoft.com/services/logic-apps/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Media Services](https://azure.microsoft.com/services/media-services/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Microsoft Azure portal](https://azure.microsoft.com/features/azure-portal/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: **&ast;** | :heavy_check_mark: |
| [Microsoft Defender Gelişmiş tehdit koruması](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)  |  |  |  |  | :heavy_check_mark:
| [Microsoft Graph](https://docs.microsoft.com/graph/overview) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Microsoft PowerApps](https://docs.microsoft.com/powerapps/powerapps-overview) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Microsoft Stream](https://docs.microsoft.com/stream/overview) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Ağ İzleyicisi](https://azure.microsoft.com/services/network-watcher/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Notification Hubs](https://azure.microsoft.com/services/notification-hubs/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Power BI Embedded](https://azure.microsoft.com/services/power-bi-embedded/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Redis Önbelleği](https://azure.microsoft.com/services/cache/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Scheduler](https://azure.microsoft.com/services/scheduler/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Bus](https://azure.microsoft.com/services/service-bus/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Service Fabric](https://azure.microsoft.com/services/service-fabric/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [SQL Veri Ambarı](https://azure.microsoft.com/services/sql-data-warehouse/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [SQL Server Stretch Database](https://azure.microsoft.com/services/sql-server-stretch-database/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Depolama: Blobs](https://azure.microsoft.com/services/storage/blobs/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Depolama: Diskler (dahil olmak üzere yönetilen diskler)](https://azure.microsoft.com/services/storage/disks/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Depolama: Dosyaları](https://azure.microsoft.com/services/storage/files/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Depolama: Kuyruklar](https://azure.microsoft.com/services/storage/queues/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Depolama: Tabloları](https://azure.microsoft.com/services/storage/tables/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [StorSimple](https://azure.microsoft.com/services/storsimple/) | :heavy_check_mark: | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark:
| [Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makine Ölçek Kümeleri](https://azure.microsoft.com/services/virtual-machine-scale-sets/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Makineler](https://azure.microsoft.com/services/virtual-machines/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Sanal Ağ](https://azure.microsoft.com/services/virtual-network/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [VPN Gateway](https://azure.microsoft.com/services/vpn-gateway/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Web uygulamaları (uygulama hizmeti)](https://azure.microsoft.com/services/app-service/web/) | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

**&ast;** Azure portalı ve Azure Resource Manager depolamaz veya DoD CC SRG IIL 5 içerik, işleme gibi bazı hizmetler IIL5 denetim kümesi ancak hala devralır.

