---
title: MySQL kaynak sağlayıcı Azure yığında kaldırma | Microsoft Docs
description: Azure yığın dağıtımınızdan MySQL kaynak sağlayıcısı nasıl kaldırabileceğiniz öğrenin.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: d3a615e3b92a62709a787d0463dfa3148f14d07e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085807"
---
# <a name="remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını kaldırma

MySQL kaynak sağlayıcı kaldırmadan önce tüm sağlayıcı bağımlılıklarını kaldırmanız gerekir. Kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasını da gerekir.

## <a name="dependency-cleanup"></a>Bağımlılık temizleme

Kaynak sağlayıcısı kaldırmak için DeployMySqlProvider.ps1 komut dosyası çalıştırılmadan önce yapmak için birkaç temizleme görevleri vardır.

Kiracılar aşağıdaki temizleme görevlerden sorumlu şunlardır:

* Tüm veritabanları kaynak Sağlayıcısı'ndan silin. (Kiracı veritabanlarını silerek verileri silmez.)
* Sağlayıcı ad alanından kaydını silin.

Aşağıdaki temizleme görevler için yönetici sorumludur:

* Barındırma sunucuları MySQL bağdaştırıcısından siler.
* MySQL bağdaştırıcısı başvuru herhangi bir plan siler.
* MySQL bağdaştırıcısı ile ilişkili tüm kotalar siler.

## <a name="to-remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcı kaldırmak için

1. Tüm mevcut MySQL kaynak sağlayıcı bağımlılıkları kaldırdık doğrulayın.

   >[!NOTE]
   >Bağımlı kaynaklar şu anda kaynak sağlayıcısı kullanıyor olsanız bile, MySQL kaynak sağlayıcısını kaldırma devam edecek.
  
2. MySQL kaynak sağlayıcı bir kopyasını ikili almak ve geçici bir dizine içeriği ayıklamak için ayıklayıcısı çalıştırın.
3. SQL kaynak sağlayıcısı bir kopyasını ikili almak ve geçici bir dizine içeriği ayıklamak için ayıklayıcısı çalıştırın.
4. Yeni yükseltilmiş bir PowerShell konsol penceresi açın ve MySQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin.
5. Aşağıdaki parametreleri kullanarak DeployMySqlProvider.ps1 komut dosyasını çalıştırın:
    - **Kaldırma**. İlişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır.
    - **PrivilegedEndpoint**. IP adresi veya ayrıcalıklı uç noktanın DNS adı.
    - **CloudAdminCredential**. Bulut Yöneticisi, ayrıcalıklı uç noktasına erişmek için gerekli kimlik bilgileri.
    - **DirectoryTenantID**
    - **AzCredential**. Azure yığın hizmet yönetici hesabının kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama Hizmetleri PaaS teklifi](azure-stack-app-service-overview.md)
