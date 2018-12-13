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
ms.date: 12/10/2018
ms.author: jeffgilb
ms.reviewer: georgel
ms.openlocfilehash: 2f300e496873c0b048ccc1acc078bf1650e6bd9c
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166294"
---
# <a name="mysql-resource-provider-11300--release-notes"></a>MySQL kaynak sağlayıcısı 1.1.30.0 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları geliştirmeleri ve MySQL kaynak sağlayıcı sürümü 1.1.30.0 bilinen sorunları açıklar.

## <a name="build-reference"></a>Yapı Başvurusu
İkili MySQL kaynak Sağlayıcısı'nı indirin ve geçici bir dizine içeriğini ayıklamak için ayıklayıcısı çalıştırın. Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. MySQL kaynak Sağlayıcısı'nın bu sürümü yüklemek için gereken en düşük Azure Stack sürümü aşağıda verilmiştir:

> |Azure Stack en düşük sürüm|MySQL kaynak sağlayıcı sürümü|
> |-----|-----|
> |Azure Stack 1808 Güncelleştirmesi (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestackmysqlrp11300)|
> |     |     |

> [!IMPORTANT]
> En düşük desteklenen Azure Stack güncelleştirme, Azure Stack tümleşik sistemi için geçerli veya MySQL kaynak sağlayıcısı en son sürümünü dağıtmadan önce en son Azure Stack geliştirme Seti'ni (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack MySQL kaynak Sağlayıcısı'nın bu sürümü, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **MySQL kaynak sağlayıcısı dağıtımları için etkin telemetri**. Telemetri toplama MySQL kaynak sağlayıcısı dağıtımları için etkinleştirildi. Kaynak sağlayıcısı dağıtım toplanan telemetri içerir, Başlat ve kez durdurmak, durumu, çıkış iletileri ve hata ayrıntıları (varsa) çıkın.

- **TLS 1.2 şifrelemeyi güncelleştirme**. Azure Stack iç bileşenleri ile kaynak sağlayıcısı iletişim için etkinleştirilmiş TLS 1.2 yalnızca destekler. 

### <a name="fixes"></a>Düzeltmeleri

- **MySQL kaynak sağlayıcısı, Azure Stack PowerShell Uyumluluk**. MySQL kaynak sağlayıcısı, AzureRM 1.3.0 uyum sağlamak için Azure Stack 2018-03-01-karma PowerShell profili ile çalışma ve daha sonra güncelleştirildi.

- **MySQL oturum açma değişiklik parola dikey**. Burada parola değişikliği parola dikey pencerede değiştirilemez bir sorun düzeltildi. Parola kaldırılan bağlantılardan değişiklik bildirimleri.

## <a name="known-issues"></a>Bilinen sorunlar 

- **MySQL SKU'ları alabilir saate portalda görünür olmasını**. Bu yeni bir saate yeni MySQL veritabanları oluştururken kullanım için görünür olmasını oluşturulan SKU'ları kadar sürebilir. 

    **Geçici çözüm**: Yok.

- **MySQL oturum açmalar yeniden**. Yeni MySQL oluşturulmaya çalışılırken oturum açma ile aynı abonelik altında var olan bir oturum olarak aynı kullanıcı adı aynı oturum açma ve mevcut parolayı yeniden kullanma neden olur. 

    **Geçici çözüm**: Farklı kullanıcı adları aynı abonelik altında yeni bir oturum açmalar oluşturulurken kullanın veya farklı bir abonelik altında aynı kullanıcı adı ile oturum açma bilgileri oluşturun.

- **TLS 1.2 desteği gereksinim**. TLS 1.2 etkin olduğu bir bilgisayardan MySQL kaynak sağlayıcısını güncelle ve dağıtımı çalışırsanız, işlem başarısız olabilir. TLS 1.2 desteklenen verdiğini doğrulamak için kaynak sağlayıcısını güncelle veya dağıtmak için kullanılan bilgisayarda aşağıdaki PowerShell komutunu çalıştırın:

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  Varsa **Tls12** olan komut çıktısında dahil değil, TLS 1.2 bilgisayarda etkin değil.

    **Geçici çözüm**: TLS 1.2 etkinleştirip sonra kaynak sağlayıcısı dağıtımı başlatın veya aynı PowerShell oturumunda komut dosyasını güncelleştirmek için aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```
 
### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Çalışan Azure Stack bulut yöneticileri için bilinen sorunlar
Belgeye başvurun [Azure Stack sürüm notları](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Sonraki adımlar
[MySQL kaynak sağlayıcısı hakkında daha fazla bilgi](azure-stack-mysql-resource-provider.md).

[MySQL kaynak Sağlayıcı ' yı dağıtmaya hazırlanma](azure-stack-mysql-resource-provider-deploy.md#prerequisites).

[MySQL kaynak Sağlayıcı'önceki bir sürümünden yükseltme](azure-stack-mysql-resource-provider-update.md). 