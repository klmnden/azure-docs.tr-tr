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
ms.openlocfilehash: f4bc90b2d1a80125ae88b4b5c4c11e42a34a985a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51240435"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Platformlar ve Azure Güvenlik Merkezi tarafından desteklenen özellikler

Güvenlik durumunu izleme ve öneriler, hem Klasik ve Resource Manager dağıtım modelleri ve Bilgisayarları'nı kullanarak oluşturulan sanal makineler için (VM'ler) kullanılabilir.

> [!NOTE]
> Daha fazla bilgi edinin [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
>
>

## <a name="supported-platforms"></a>Desteklenen platformlar 

Bu bölümde, Azure Güvenlik Merkezi Aracısı çalıştırabilir ve bunu veri toplayabilirsiniz platformlar listelenir.

### <a name="supported-platforms-for-windows-computers-and-vms"></a>Windows bilgisayarlar ve sanal makineleri için desteklenen platformlar
Desteklenen Windows işletim sistemleri:

* Windows Server 2008
* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016


### <a name="supported-platforms-for-linux-computers-and-vms"></a>Linux bilgisayarları ve sanal makineleri için desteklenen platformlar
Desteklenen Linux işletim sistemleri:

* Ubuntu sürüm 12.04 LTS, 14.04 LTS, 16.04 LTS
* Debian sürüm 6, 7, 8, 9
* CentOS sürüm 5, 6, 7
* Red Hat Enterprise Linux (RHEL) sürümlerini 5, 6, 7
* SUSE Linux Enterprise Server (SLES) sürüm 11, 12
* Oracle Linux sürüm 5, 6, 7
* Amazon Linux 2012.09 2017
* Openssl 1.1.0 yalnızca (64-bit) x86_64 platformlarında desteklenir

> [!NOTE]
> Sanal makine davranış analizi henüz Linux işletim sistemleri için kullanılabilir değildir.
>
>

## <a name="vms-and-cloud-services"></a>VM'ler ve bulut Hizmetleri
Bir bulut hizmetinde çalışan sanal makineler de desteklenir. Yuva izlenen üretimde çalışan web ve çalışan rolleri yalnızca bulut Hizmetleri. Bulut hizmeti hakkında daha fazla bilgi için bkz: [Cloud Services'e genel bakış](../cloud-services/cloud-services-choose-me.md).


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
|Kötü amaçlı yazılımdan koruma|✔|✔|X|X|
|JIT VM erişimi|✔|X|✔|X|
|Uyarlamalı uygulama denetimleri|✔|X|X|X|
|FIM|✔|✔|✔|✔|
|Disk şifrelemesi|✔|X|✔|X|
|Üçüncü taraf dağıtım|✔|X|✔|X|
|NSG'ler|✔|X|✔|X|
|Filess tehdit algılama|✔|✔|X|X|
|Ağ eşlemesi|✔|X|✔|X|
|Uyarlamalı ağ denetimleri|✔|X|✔|X|

\* Bu özellikler şu anda genel önizlemede desteklenmektedir.


## <a name="supported-paas-features"></a>Desteklenen PaaS özellikleri


|Hizmet|Öneriler|Tehdit algılama|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL *|✔| ✔|
|MySQL *|✔| ✔|
|BLOB Depolama hesapları *|✔| ✔|
|Uygulama hizmetleri|✔| ✔|
|Bulut hizmetleri|✔| X|
|Sanal ağlar|✔| NA|
|Alt ağlar|✔| NA|
|NIC’ler|✔| ✔|
|NSG'ler|✔| NA|
|Abonelik|✔| ✔|

\* Bu özellikler şu anda genel önizlemede desteklenmektedir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md) — planlama ve anlama Azure Güvenlik Merkezi'ni benimsemek için tasarım konuları hakkında bilgi edinin
- [Azure Güvenlik Merkezi'nde türe göre güvenlik uyarıları](security-center-alerts-type.md#virtual-machine-behavioral-analysis) - sanal makine davranış analizi hakkında daha fazla bilgi edinin ve kilitlenme dökümü Güvenlik Merkezi'nde bellek analizi
- [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmet kullanımı ile ilgili sık sorulan soruları burada bulabilirsiniz
- [Azure güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) — Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz
