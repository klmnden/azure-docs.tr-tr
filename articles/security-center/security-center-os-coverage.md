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
ms.date: 5/27/2019
ms.author: monhaber
ms.openlocfilehash: 807bde76bb6bb50490ee599768273a59c49d5e45
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258698"
---
# <a name="platforms-and-features-supported-by-azure-security-center"></a>Platformlar ve Azure Güvenlik Merkezi tarafından desteklenen özellikler

Güvenlik durumunu izleme ve öneriler hem Klasik ve Resource Manager dağıtım modelleri ve Bilgisayarları'nı kullanarak oluşturulan sanal makineleri (VM'ler) için kullanılabilir.

> [!NOTE]
> Daha fazla bilgi edinin [Klasik ve Resource Manager dağıtım modellerinde](../azure-classic-rm.md) Azure kaynakları için.
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

### <a name="supported-platforms-for-linux-computers-and-vms"></a>Linux bilgisayarları ve sanal makineleri için desteklenen platformlar

Aşağıdaki Linux işletim sistemleri desteklenir:

> [!NOTE]
> Tercih ederseniz desteklenen Linux işletim sistemlerinin listesi sürekli, değişir olduğundan, tıklayın [burada](https://github.com/microsoft/OMS-Agent-for-Linux#supported-linux-operating-systems) olmuştur durumunda değişiklikler bu konunun son yayınlandığı desteklenen sürümleri, en güncel listesini görüntülemek için.

64 bit
* CentOS 6 ve 7
* Amazon Linux 2017.09
* Oracle Linux 6 ve 7
* Red Hat Enterprise Linux Server 6 ve 7
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS, 16.04 LTS ve 18.04 LTS
* SUSE Linux Enterprise Server 12

32 bit
* CentOS 6
* Oracle Linux 6
* Red Hat Enterprise Linux Server 6
* Debian GNU/Linux 8 ve 9
* Ubuntu Linux 14.04 LTS ve 16.04 LTS

## <a name="vms-and-cloud-services"></a>VM'ler ve bulut Hizmetleri
Bir bulut hizmetinde çalışan sanal makineleri de desteklenir. Üretim yuvalarını çalıştırılan yalnızca cloud services web ve çalışan rolleri izlenir. Bulut hizmetleri hakkında daha fazla bilgi için bkz. [Azure Cloud Services Genel Bakış](../cloud-services/cloud-services-choose-me.md).


## <a name="supported-iaas-features"></a>Desteklenen Iaas özellikleri

> [!div class="mx-tableFixed"]
> 

|Sunucusu|Windows||Linux||||Fiyatlandırma|
|----|----|----|----|----|----|----|----|
|**Ortam**|**Azure**||**Azure dışı**|**Azure**||**Azure dışı**||
||**Sanal Makine**|**Sanal makine ölçek kümesi**||**Sanal Makine**|**Sanal makine ölçek kümesi**|
|VMBA tehdit algılama uyarıları|✔|✔|✔|✔ (üzerinde desteklenen sürümleri)|✔ (üzerinde desteklenen sürümleri)|✔|Öneriler (ücretsiz) tehdit algılama (standart)|
|Ağ tabanlı tehdit algılama uyarıları|✔|✔|X|✔|✔|X|Standart|
|Windows Defender ATP tümleştirme|✔ (üzerinde desteklenen sürümleri)|✔ (üzerinde desteklenen sürümleri)|✔|X|X|X|Standart|
|Yamaları eksik|✔|✔|✔|✔|✔|✔|Boş|
|Güvenlik yapılandırmaları|✔|✔|✔|✔|✔|✔|Boş|
|Uç nokta koruma değerlendirmesi|✔|✔|✔|X|X|X|Boş|
|JIT VM erişimi|✔|X|X|✔|X|X|Standart|
|Uyarlamalı uygulama denetimleri|✔|X|✔|✔|X|✔|Standart|
|FIM|✔|✔|✔|✔|✔|✔|Standart|
|Disk şifreleme değerlendirme|✔|✔|X|✔|✔|X|Boş|
|Üçüncü taraf dağıtım|✔|X|X|✔|X|X|Boş|
|Nsg'ler değerlendirmesi|✔|✔|X|✔|✔|X|Boş|
|Fileless tehdit algılama|✔|✔|✔|X|X|X|Standart|
|Ağ eşlemesi|✔|✔|X|✔|✔|X|Standart|
|Uyarlamalı ağ denetimleri|✔|✔|X|✔|✔|X|Standart|
|Yasal uyumluluk Pano ve raporlar|✔|✔|✔|✔|✔|✔|Standart|
|Öneriler ve Iaas barındırılan Docker kapsayıcılarına tehdit algılama|X|X|X|✔|✔|✔|Standart|

### <a name="supported-endpoint-protection-solutions"></a>Desteklenen uç nokta koruma çözümleri

Aşağıdaki tabloda bir matrisi verilmektedir:
 - Olup Azure Güvenlik Merkezi, her çözüm için yüklemek için kullanabilirsiniz.
 - Hangi uç nokta koruma çözümleri Güvenlik Merkezi bulabilir. Bu uç nokta koruma çözümleri bulunursa, Güvenlik Merkezi bir yükleme tavsiyede bulunmaz.

Ne zaman, her biri bu korumalar için oluşturulan öneriler hakkında daha fazla bilgi için bkz: [uç nokta koruma değerlendirmesi ve öneriler](security-center-endpoint-protection.md).

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

## <a name="supported-paas-features"></a>Desteklenen PaaS özellikleri


|Hizmet|Öneriler (ücretsiz)|Tehdit algılama (standart)|
|----|----|----|
|SQL|✔| ✔|
|PostGreSQL*|✔| ✔|
|MySQL*|✔| ✔|
|Azure Blob Depolama hesapları *|✔| ✔|
|Uygulama hizmetleri|✔| ✔|
|Cloud Services|✔| X|
|VNets|✔| NA|
|Alt ağlar|✔| NA|
|NIC’ler|✔| NA|
|NSG'ler|✔| NA|
|Abonelik|✔ **| ✔|
|App Service|✔| NA|
|Batch|✔| NA|
|Service Fabric|✔| NA|
|Otomasyon hesabı|✔| NA|
|Yük dengeleyici|✔| NA|
|Ara|✔| NA|
|Service Bus|✔| NA|
|Stream Analytics|✔| NA|
|Olay hub'ı|✔| NA|
|Mantıksal uygulamalar|✔| NA|
|Alt ağ|✔| NA|
|Sanal ağ|✔| NA|
|Depolama hesabı|✔| NA|
|Redis|✔| NA|
|SQL|✔| NA|
|Data lake analytics|✔| NA|
|Depolama hesabı|✔| NA|
|Abonelik|✔| NA|
|Key Vault|✔| NA|




\* Bu özellikler şu anda genel önizlemede desteklenmektedir.

\*\* AAD önerileri yalnızca standart abonelikler için kullanılabilir



## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [planlama ve anlama Azure Güvenlik Merkezi'ni benimsemek için tasarım konuları](security-center-planning-and-operations-guide.md).
- Daha fazla bilgi edinin [sanal makine davranış analizi ve kilitlenme dökümü bellek analizi Güvenlik Merkezi'nde](security-center-alerts-type.md#virtual-machine-behavioral-analysis).
- Bulma [Azure Güvenlik Merkezi'ni kullanma hakkında sık sorulan sorular](security-center-faq.md).
- Bulma [Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını](https://blogs.msdn.com/b/azuresecurity/).
