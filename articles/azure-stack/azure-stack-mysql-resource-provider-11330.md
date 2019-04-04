---
title: Azure Stack MySQL kaynak sağlayıcısı 1.1.30.0 sürüm notları | Microsoft Docs
description: Bilinen sorunlar da dahil olmak üzere en son Azure Stack MySQL kaynak sağlayıcısı güncelleştirmede nedir ve indirmek üzere nerede hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: e0101aebadcaef71f35c72b54f9126e69cff0f61
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882844"
---
# <a name="mysql-resource-provider-11330--release-notes"></a>MySQL kaynak sağlayıcısı 1.1.33.0 sürüm notları

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları geliştirmeleri ve MySQL kaynak sağlayıcı sürümü 1.1.33.0 bilinen sorunları açıklar.

## <a name="build-reference"></a>Yapı Başvurusu
İkili MySQL kaynak Sağlayıcısı'nı indirin ve geçici bir dizine içeriğini ayıklamak için ayıklayıcısı çalıştırın. Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. MySQL kaynak Sağlayıcısı'nın bu sürümü yüklemek için gereken en düşük Azure Stack sürümü aşağıda verilmiştir:

> |Azure Stack en düşük sürüm|MySQL kaynak sağlayıcı sürümü|
> |-----|-----|
> |Sürüm 1808 (1.1808.0.97)|[MySQL RP 1.1.33.0 sürümü](https://aka.ms/azurestackmysqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> En düşük desteklenen Azure Stack güncelleştirme, Azure Stack tümleşik sistemi için geçerli veya MySQL kaynak sağlayıcısı en son sürümünü dağıtmadan önce en son Azure Stack geliştirme Seti'ni (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack MySQL kaynak Sağlayıcısı'nın bu sürümü, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

### <a name="fixes"></a>Düzeltmeleri
- **MySQL kaynak sağlayıcısı portal uzantısı yanlış aboneliğe seçin**. MySQL kaynak sağlayıcısı değil olabilecek kullanmak için ilk hizmet yönetici aboneliği belirlemek için Azure Resource Manager çağrılarını kullanır *varsayılan sağlayıcı aboneliği*. MySQL kaynak sağlayıcısı, bu durumda, normalde çalışmaz. 

- **Barındırma sunucusu listelenmeyen MySQL veritabanları barındırılan.** Kullanıcı tarafından oluşturulan veritabanları için barındırma sunucuları MySQL Kiracı kaynaklarını görüntülerken listelenebilir değil.

- **TLS 1.2 etkin değilse önceki MySQL kaynak sağlayıcısı (1.1.30.0) dağıtımı başarısız**. MySQL kaynak sağlayıcısı 1.1.33.0 TLS 1.2 kaynak sağlayıcısı güncelleştiriliyor veya gizli anahtarları döndürme kaynak sağlayıcısı dağıtırken etkinleştirmek için güncelleştirildi. 

- **MySQL kaynak sağlayıcısı gizli döndürme başarısız**. Aşağıdaki hata kodunu gizli dizileri döndürürken kaynaklanan sorun düzeltildi:
`New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>Bilinen sorunlar 

- **MySQL SKU'ları alabilir saate portalda görünür olmasını**. Bu yeni bir saate yeni MySQL veritabanları oluştururken kullanım için görünür olmasını oluşturulan SKU'ları kadar sürebilir. 

    **Geçici çözüm**: Yok.

- **MySQL oturum açmalar yeniden**. Yeni MySQL oluşturulmaya çalışılırken oturum açma ile aynı abonelik altında var olan bir oturum olarak aynı kullanıcı adı aynı oturum açma ve mevcut parolayı yeniden kullanma neden olur. 

    **Geçici çözüm**: Farklı kullanıcı adları aynı abonelik altında yeni bir oturum açmalar oluşturulurken kullanın veya farklı bir abonelik altında aynı kullanıcı adı ile oturum açma bilgileri oluşturun.

- **Paylaşılan MySQL oturum açma bilgileri, veri tutarsızlığına neden**. Bir MySQL oturum açma için aynı abonelik altında birden çok MySQL veritabanlarını paylaşılıyorsa, oturum açma parolasını değiştirme veri tutarsızlığına neden olur.

    **Geçici çözüm**: Her zaman aynı abonelik altında farklı veritabanları için farklı kimlik bilgileri kullanın.


### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Çalışan Azure Stack bulut yöneticileri için bilinen sorunlar
Belgeye başvurun [Azure Stack sürüm notları](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Sonraki adımlar
[MySQL kaynak sağlayıcısı hakkında daha fazla bilgi](azure-stack-mysql-resource-provider.md).

[MySQL kaynak Sağlayıcı ' yı dağıtmaya hazırlanma](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[MySQL kaynak Sağlayıcı'önceki bir sürümünden yükseltme](azure-stack-mysql-resource-provider-update.md). 