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
ms.date: 1/09/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 1/09/2019
ms.openlocfilehash: 072f3fba08f9a6703379e404d4be00896f8d4bc6
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241854"
---
# <a name="sql-resource-provider-11300-release-notes"></a>SQL kaynak sağlayıcısı 1.1.30.0 sürüm notları

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu sürüm notları, yenilikleri ve bilinen sorunların SQL kaynak sağlayıcısı sürüm 1.1.30.0 açıklar.

## <a name="build-reference"></a>Yapı Başvurusu
İkili SQL kaynak Sağlayıcısı'nı indirin ve geçici bir dizine içeriğini ayıklamak için ayıklayıcısı çalıştırın. Kaynak sağlayıcısı bir minimum karşılık gelen Azure yapı yığınına sahiptir. SQL kaynak Sağlayıcısı'nın bu sürümü yüklemek için gereken en düşük Azure Stack sürümü aşağıda verilmiştir:

> |Azure Stack en düşük sürüm|SQL kaynak sağlayıcısı sürümü|
> |-----|-----|
> |Sürüm 1808 (1.1808.0.97)|[1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
> |     |     |

> [!IMPORTANT]
> En düşük desteklenen Azure Stack güncelleştirme, Azure Stack tümleşik sistemi için geçerli veya SQL kaynak sağlayıcısı en son sürümünü dağıtmadan önce en son Azure Stack geliştirme Seti'ni (ASDK) dağıtın.

## <a name="new-features-and-fixes"></a>Yeni özellikler ve düzeltmeler
Azure Stack SQL kaynak Sağlayıcısı'nın bu sürümü, aşağıdaki geliştirmeleri ve düzeltmeleri içerir:

- **SQL kaynak sağlayıcısı dağıtımları için etkin telemetri**. SQL kaynak sağlayıcısı dağıtımları için telemetri toplama etkinleştirildi. Kaynak sağlayıcısı dağıtım toplanan telemetri içerir, Başlat ve kez durdurmak, durumu, çıkış iletileri ve hata ayrıntıları (varsa) çıkın.

- **TLS 1.2 şifrelemeyi güncelleştirme**. Azure Stack iç bileşenleri ile kaynak sağlayıcısı iletişim için etkinleştirilmiş TLS 1.2 yalnızca destekler. 

### <a name="fixes"></a>Düzeltmeleri

- **SQL kaynak sağlayıcısı, Azure Stack PowerShell Uyumluluk**. SQL kaynak sağlayıcısı, AzureRM 1.3.0 uyum sağlamak için Azure Stack 2018-03-01-karma PowerShell profili ile çalışma ve daha sonra güncelleştirildi.

- **SQL oturum açma değişiklik parola dikey**. Burada parola değişikliği parola dikey pencerede değiştirilemez bir sorun düzeltildi. Parola kaldırılan bağlantılardan değişiklik bildirimleri.

- **SQL Server ayarları dikey penceresinde güncelleştirme barındırma**. Ayarlar dikey penceresinde "Parola" yanlış yere başlığı bir sorun düzeltildi.

## <a name="known-issues"></a>Bilinen sorunlar 

- **SQL SKU'ları alabilir saate portalda görünür olmasını**. Bu yeni bir saate yeni SQL veritabanı oluşturma sırasında kullanım için görünür olmasını oluşturulan SKU'ları kadar sürebilir. 

    **Geçici çözüm**: Yok.

- **SQL oturum açmaları yeniden**. Yeni bir SQL oluşturulmaya çalışılırken oturum açma ile aynı abonelik altında var olan bir oturum olarak aynı kullanıcı adı aynı oturum açma ve mevcut parolayı yeniden kullanma neden olur. 

    **Geçici çözüm**: Farklı kullanıcı adları aynı abonelik altında yeni bir oturum açmalar oluşturulurken kullanın veya farklı bir abonelik altında aynı kullanıcı adı ile oturum açma bilgileri oluşturun.

- **Paylaşılan SQL oturum açmaları veri tutarsızlığına neden**. Bir SQL oturum açma için aynı abonelik altında birden çok SQL veritabanlarını paylaşılıyorsa, oturum açma parolasını değiştirme veri tutarsızlığına neden olur.

    **Geçici çözüm**: Her zaman aynı abonelik altında farklı veritabanları için farklı kimlik bilgileri kullanın.

- **TLS 1.2 desteği gereksinim**. Dağıtma veya TLS 1.2 etkin olduğu bir bilgisayardan SQL kaynak sağlayıcısını güncelle denerseniz işlem başarısız olabilir. TLS 1.2 desteklenen verdiğini doğrulamak için kaynak sağlayıcısını güncelle veya dağıtmak için kullanılan bilgisayarda aşağıdaki PowerShell komutunu çalıştırın:

  ```powershell
  [System.Net.ServicePointManager]::SecurityProtocol
  ```

  Varsa **Tls12** olan komut çıktısında dahil değil, TLS 1.2 bilgisayarda etkin değil.

    **Geçici çözüm**: TLS 1.2 etkinleştirip sonra kaynak sağlayıcısı dağıtımı başlatın veya aynı PowerShell oturumunda komut dosyasını güncelleştirmek için aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
    ```
- **SQL kaynak sağlayıcısı başarısız SQL Server Always On dinleyici eklemek**. SQL kaynak sağlayıcısı VM, SQL Server Always On dinleyicisi dinleyici IP adresi kullanılırken dinleyicinin konak adı çözümlenemiyor.

    **Geçici çözüm**: DNS düzgün dinleyici ana bilgisayar adı için bir dinleyici IP çözmek için çalıştığından emin olun.
    
### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>Çalışan Azure Stack bulut yöneticileri için bilinen sorunlar
Belgeye başvurun [Azure Stack sürüm notları](azure-stack-servicing-policy.md).

## <a name="next-steps"></a>Sonraki adımlar
[SQL kaynak sağlayıcısı hakkında daha fazla bilgi](azure-stack-sql-resource-provider.md).

[SQL kaynak sağlayıcısı dağıtmaya hazırlanma](azure-stack-sql-resource-provider-deploy.md#prerequisites).

[SQL kaynak sağlayıcısı önceki bir sürümden yükseltme](azure-stack-sql-resource-provider-update.md). 