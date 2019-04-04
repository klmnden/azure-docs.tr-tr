---
title: Azure Stack SQL kaynak sağlayıcısı 1.1.30.0 sürüm notları | Microsoft Docs
description: Tüm bilinen sorunlar da dahil olmak üzere en son Azure Stack SQL kaynak sağlayıcısı güncelleştirmede nedir ve indirmek üzere nerede hakkında bilgi edinin.
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
ms.openlocfilehash: ae5a824fbd96a9a76eb18811a46bfc17afa15073
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879361"
---
# <a name="sql-resource-provider-11330-release-notes"></a>SQL kaynak sağlayıcısı 1.1.33.0 sürüm notları

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, yenilikleri ve bilinen sorunların SQL kaynak sağlayıcısı sürüm 1.1.33.0 açıklar.

## <a name="build-reference"></a>Yapı Başvurusu
İkili SQL kaynak Sağlayıcısı'nı indirin ve geçici bir dizine içeriğini ayıklamak için ayıklayıcısı çalıştırın. Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. SQL kaynak Sağlayıcısı'nın bu sürümü yüklemek için gereken en düşük Azure Stack sürümü aşağıda verilmiştir:

> |Azure Stack en düşük sürüm|SQL kaynak sağlayıcısı sürümü|
> |-----|-----|
> |Sürüm 1808 (1.1808.0.97)|[SQL RP 1.1.33.0 sürümü](https://aka.ms/azurestacksqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> En düşük desteklenen Azure Stack güncelleştirme, Azure Stack tümleşik sistemi için geçerli veya SQL kaynak sağlayıcısı en son sürümünü dağıtmadan önce en son Azure Stack geliştirme Seti'ni (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack SQL kaynak Sağlayıcısı'nın bu sürümü, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

### <a name="fixes"></a>Düzeltmeleri
- **SQL kaynak sağlayıcısı portal uzantısı yanlış aboneliğe seçin**. SQL kaynak sağlayıcısı değil olabilecek kullanmak için ilk hizmet yönetici aboneliği belirlemek için Azure Resource Manager çağrılarını kullanır *varsayılan sağlayıcı aboneliği*. SQL kaynak sağlayıcısı, bu durumda, normalde çalışmaz. 

- **SQL barındırma sunucusu barındırılan veritabanlarını listelemez.** Kullanıcı tarafından oluşturulan veritabanları için SQL barındırma sunucuları Kiracı kaynaklarını görüntülerken listelenebilir değil.

- **TLS 1.2 etkinleştirilmemişse, önceki SQL kaynak sağlayıcısı (1.1.30.0) dağıtımı başarısız**. SQL kaynak sağlayıcısı 1.1.33.0 TLS 1.2 kaynak sağlayıcısı güncelleştiriliyor veya gizli anahtarları döndürme kaynak sağlayıcısı dağıtırken etkinleştirmek için güncelleştirildi. 

- **SQL kaynak sağlayıcısı gizli döndürme başarısız**. Aşağıdaki hata kodunu gizli dizileri döndürürken kaynaklanan sorun düzeltildi:
`New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>Bilinen sorunlar 

- **SQL SKU'ları alabilir saate portalda görünür olmasını**. Bu yeni bir saate yeni SQL veritabanı oluşturma sırasında kullanım için görünür olmasını oluşturulan SKU'ları kadar sürebilir. 

    **Geçici çözüm**: Yok.

- **SQL oturum açmaları yeniden**. Yeni bir SQL oluşturulmaya çalışılırken oturum açma ile aynı abonelik altında var olan bir oturum olarak aynı kullanıcı adı aynı oturum açma ve mevcut parolayı yeniden kullanma neden olur. 

    **Geçici çözüm**: Farklı kullanıcı adları aynı abonelik altında yeni bir oturum açmalar oluşturulurken kullanın veya farklı bir abonelik altında aynı kullanıcı adı ile oturum açma bilgileri oluşturun.

- **Paylaşılan SQL oturum açmaları veri tutarsızlığına neden**. Bir SQL oturum açma için aynı abonelik altında birden çok SQL veritabanlarını paylaşılıyorsa, oturum açma parolasını değiştirme veri tutarsızlığına neden olur.

    **Geçici çözüm**: Her zaman aynı abonelik altında farklı veritabanları için farklı kimlik bilgileri kullanın.

- **SQL kaynak sağlayıcısı başarısız SQL Server Always On dinleyici eklemek**. SQL kaynak sağlayıcısı VM, SQL Server Always On dinleyicisi dinleyici IP adresi kullanılırken dinleyicinin konak adı çözümlenemiyor.

    **Geçici çözüm**: DNS düzgün dinleyici ana bilgisayar adı için bir dinleyici IP çözmek için çalıştığından emin olun.

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Çalışan Azure Stack bulut yöneticileri için bilinen sorunlar
Belgeye başvurun [Azure Stack sürüm notları](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Sonraki adımlar
[SQL kaynak sağlayıcısı hakkında daha fazla bilgi](azure-stack-sql-resource-provider.md).

[SQL kaynak sağlayıcısı dağıtmaya hazırlanma](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[SQL kaynak sağlayıcısı önceki bir sürümden yükseltme](azure-stack-sql-resource-provider-update.md). 