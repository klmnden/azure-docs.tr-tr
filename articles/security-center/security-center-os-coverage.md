---
title: Özellikler ve Azure Güvenlik Merkezi tarafından desteklenen platformlar | Microsoft Docs
description: Bu belge, özellikleri ve Azure Güvenlik Merkezi tarafından desteklenen platformlar listesini sağlar.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/17/2019
ms.author: monhaber
ms.openlocfilehash: b5eafd15344156965d0a191688f602ffe1b5a498
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678318"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Platformlar ve Azure Güvenlik Merkezi tarafından desteklenen özellikler

Güvenlik durumunu izleme ve öneriler hem Klasik ve Resource Manager dağıtım modelleri ve Bilgisayarları'nı kullanarak oluşturulan sanal makineleri (VM'ler) için kullanılabilir.

> [!NOTE]
> Daha fazla bilgi edinin [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
>
>

## <a name="platforms-that-support-the-data-collection-agent"></a>Veri toplama Aracısı destekleyen platformlar 

Bu bölümde, Azure Güvenlik Merkezi Aracısı çalıştırabilir ve bunu veri toplayabilirsiniz platformlar listelenir.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Windows bilgisayarlar ve sanal makineleri için desteklenen platformlar
Aşağıdaki Windows işletim sistemleri desteklenir:

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

> [!NOTE]
> Windows Defender ATP ile tümleştirme, yalnızca Windows Server 2012 R2 ve Windows Server 2016'yı destekler.
>
>

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Linux bilgisayarları ve sanal makineleri için desteklenen platformlar
Aşağıdaki Linux işletim sistemleri desteklenir:

* Ubuntu sürüm 12.04 LTS, 14.04 LTS ve 16.04 LTS.
* Debian sürüm 6, 7, 8 ve 9.
* CentOS sürüm 5, 6 ve 7.
* Red Hat Enterprise Linux (RHEL) sürüm 5, 6 ve 7.
* SUSE Linux Enterprise Server (SLES) sürüm 11 ve 12.
* Oracle Linux sürüm 5, 6 ve 7.
* Amazon Linux 2012.09 2017.
* OpenSSL 1.1.0 yalnızca 64 bit x86_64 platformları üzerinde desteklenir.

## <a name="vms-and-cloud-services"></a>VM'ler ve bulut Hizmetleri
Bir bulut hizmetinde çalışan sanal makineleri de desteklenir. Üretim yuvalarını çalıştırılan yalnızca cloud services web ve çalışan rolleri izlenir. Bulut hizmetleri hakkında daha fazla bilgi için bkz. [Azure Cloud Services Genel Bakış](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Desteklenen Iaas özellikleri

> [!div class="mx-tableFixed"]
> 

|Sunucu|Windows||Linux||
|----|----|----|----|----|
|Ortam|Azure|Azure Dışı|Azure|Azure Dışı|
|VMBA tehdit algılama uyarıları|✔|✔|✔ (üzerinde desteklenen sürümleri)|✔|
|Ağ tabanlı tehdit algılama uyarıları|✔|X|✔|X|
|Windows Defender ATP tümleştirme|✔ (üzerinde desteklenen sürümleri)|✔|X|X|
|Yamaları eksik|✔|✔|✔|✔|
|Güvenlik yapılandırmaları|✔|✔|✔|✔|
|Uç nokta koruması|✔|✔|X|X|
|JIT VM erişimi|✔|X|✔|X|
|Uyarlamalı uygulama denetimleri|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Disk şifrelemesi|✔|X|✔|X|
|Üçüncü taraf dağıtım|✔|X|✔|X|
|NSG'ler|✔|X|✔|X|
|Fileless tehdit algılama|✔|✔|X|X|
|Ağ haritası|✔|X|✔|X|
|Uyarlamalı ağ denetimleri|✔|X|✔|X|


### <a name="supported-endpoint-protection-solutions"></a>Desteklenen uç nokta koruma çözümleri

Aşağıdaki tabloda bir matrisi verilmektedir:
 - Olup Azure Güvenlik Merkezi, her çözüm için yüklemek için kullanabilirsiniz.
 - Hangi uç nokta koruma çözümleri Güvenlik Merkezi bulabilir. Bu uç nokta koruma çözümleri bulunursa, Güvenlik Merkezi bir yükleme tavsiyede bulunmaz.

| Uç Nokta Koruması| Platformlar | Güvenlik Merkezi Yüklemesi | Güvenlik Merkezi Bulma |
|------|------|-----|-----|
| Windows Defender (Microsoft Kötü Amaçlı Yazılım Koruması)| Windows Server 2016| Hayır, işletim sisteminde yerleşik| Evet |
| System Center Endpoint Protection (Microsoft Kötü Amaçlı Yazılım Koruması) | Windows Server 2012 R2, 2012, 2008 R2 (aşağıdaki nota bakın) | Uzantı ile | Evet |
| Trend Micro – Tüm sürümler | Windows Server Ailesi  | Hayır | Evet |
| Symantec v12.1.1100+| Windows Server Ailesi  | Hayır | Evet |
| McAfee v10+ | Windows Server Ailesi  | Hayır | Evet |
| Kaspersky| Windows Server Ailesi  | Hayır | Hayır  |
| Sophos| Windows Server Ailesi  | Hayır | Hayır  |

> [!NOTE]
> - Algılama System Center Endpoint Protection (SCEP) bir Windows Server 2008 R2 sanal makine üzerinde SCEP PowerShell 3.0 (veya üst bir sürümünü) sonra yüklü olmasını gerektirir.
>
>

## <a name="supported-paas-features"></a>Desteklenen PaaS özellikleri 


|Hizmet|Öneriler|Tehdit algılama|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Azure Blob Depolama hesapları *|✔| ✔|
|Uygulama hizmetleri|✔| ✔|
|Cloud Services|✔| X|
|Sanal Ağlar|✔| NA|
|Alt ağlar|✔| NA|
|NIC’ler|✔| ✔|
|NSG'ler|✔| NA|
|Abonelik|✔| ✔|

\* Bu özellikler şu anda genel önizlemede desteklenmektedir. 



## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [planlama ve anlama Azure Güvenlik Merkezi'ni benimsemek için tasarım konuları](security-center-planning-and-operations-guide.md).
- Daha fazla bilgi edinin [sanal makine davranış analizi ve kilitlenme dökümü bellek analizi Güvenlik Merkezi'nde](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Bulma [Azure Güvenlik Merkezi'ni kullanma hakkında sık sorulan sorular](security-center-faq.md).
- Bulma [Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını](https://blogs.msdn.com/b/azuresecurity/).
