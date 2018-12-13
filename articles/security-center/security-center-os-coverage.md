---
title: Özellikler ve Azure Güvenlik Merkezi tarafından desteklenen platformlar | Microsoft Docs
description: Bu belge, özellikleri ve Azure Güvenlik Merkezi tarafından desteklenen platformlar listesini sağlar.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 4108355415d1230f98db36a4f83497de2fa848f7
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53185588"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Platformlar ve Azure Güvenlik Merkezi tarafından desteklenen özellikler

Güvenlik durumunu izleme ve öneriler hem Klasik ve Resource Manager dağıtım modelleri ve Bilgisayarları'nı kullanarak oluşturulan sanal makineleri (VM'ler) için kullanılabilir.

> [!NOTE]
> Daha fazla bilgi edinin [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
>
>

## <a name="supported-platforms"></a>Desteklenen platformlar 

Bu bölümde, Azure Güvenlik Merkezi Aracısı çalıştırabilir ve bunu veri toplayabilirsiniz platformlar listelenir.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Windows bilgisayarlar ve sanal makineleri için desteklenen platformlar
Aşağıdaki Windows işletim sistemleri desteklenir:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


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

> [!NOTE]
> Sanal makine davranış analizi, Linux işletim sistemleri için henüz kullanılamamaktadır.
>
>

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
|Windows Defender ATP tümleştirme *|✔ (üzerinde desteklenen sürümleri)|✔|X|X|
|Yamaları eksik|✔|✔|✔|✔|
|Güvenlik yapılandırmaları|✔|✔|✔|✔|
|Kötü amaçlı yazılımdan koruma programları|✔|✔|X|X|
|JIT VM erişimi|✔|X|✔|X|
|Uyarlamalı uygulama denetimleri|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Disk şifrelemesi|✔|X|✔|X|
|Üçüncü taraf dağıtım|✔|X|✔|X|
|NSG'ler|✔|X|✔|X|
|Fileless tehdit algılama|✔|✔|X|X|
|Ağ eşlemesi|✔|X|✔|X|
|Uyarlamalı ağ denetimleri|✔|X|✔|X|

\* Bu özellikler şu anda genel önizlemede desteklenmektedir.


## <a name="supported-paas-features"></a>Desteklenen PaaS özellikleri 


|Hizmet|Öneriler|Tehdit algılama|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL *|✔| ✔|
|MySQL *|✔| ✔|
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
