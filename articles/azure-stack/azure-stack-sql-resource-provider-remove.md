---
title: SQL kaynak sağlayıcısı Azure yığında kaldırma | Microsoft Docs
description: Bilgi nasıl SQL kaynak sağlayıcısı Azure yığın dağıtımınızdan kaldırmanızı sağlar.
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
ms.openlocfilehash: b73deebb10d0c81a06df9cd192eaa2ef28de744d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083051"
---
# <a name="remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını kaldırma

SQL kaynak sağlayıcısı kaldırmadan önce tüm sağlayıcı bağımlılıklarını kaldırmanız gerekir. Kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasını da gerekir.

Çalıştırmadan önce yapmak için birkaç temizleme görev _DeploySqlProvider.ps1_ kaynak sağlayıcısı kaldırmak için komut dosyası.
Kiracılar aşağıdaki temizleme görevlerden sorumlu şunlardır:

* Tüm veritabanları kaynak Sağlayıcısı'ndan silin. (Kiracı veritabanlarını silerek verileri silmez.)
* Kaynak sağlayıcısı ad alanı kaydını silin.

Aşağıdaki temizleme görevler için yönetici sorumludur:

* Barındırma sunucuları SQL kaynak sağlayıcısından siler.
* SQL kaynak sağlayıcısı referans herhangi bir plan siler.
* SQL kaynak sağlayıcı ile ilişkili tüm kotalar siler.

## <a name="to-remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı kaldırmak için

1. Tüm mevcut SQL kaynak sağlayıcısı bağımlılıkları kaldırdık doğrulayın.

   > [!NOTE]
   > Bağımlı kaynaklar kaynak sağlayıcısı şu anda kullanıyorsanız bile SQL kaynak sağlayıcısını kaldırma devam edecek.
  
2. SQL kaynak sağlayıcısı bir kopyasını ikili almak ve geçici bir dizine içeriği ayıklamak için ayıklayıcısı çalıştırın.

3. Yeni yükseltilmiş bir PowerShell konsol penceresi açın ve SQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin.

4. Aşağıdaki parametreleri kullanarak DeploySqlProvider.ps1 komut dosyasını çalıştırın:

    * **Kaldırma**. İlişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır.
    * **PrivilegedEndpoint**. IP adresi veya ayrıcalıklı uç noktanın DNS adı.
    * **CloudAdminCredential**. Bulut Yöneticisi, ayrıcalıklı uç noktasına erişmek için gerekli kimlik bilgileri.
    * **AzCredential**. Azure yığın hizmet yönetici hesabının kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama Hizmetleri PaaS teklifi](azure-stack-app-service-overview.md)
