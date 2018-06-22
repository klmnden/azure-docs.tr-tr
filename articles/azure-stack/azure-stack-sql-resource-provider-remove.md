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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 150d1c40463aa04527bdd6e356a4c24ef68b02ef
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301907"
---
# <a name="remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını kaldırma

SQL kaynak sağlayıcısı kaldırmadan önce tüm sağlayıcı bağımlılıklarını kaldırmanız gerekir. Kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasını da gerekir.

## <a name="to-remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısı kaldırmak için

1. Tüm mevcut SQL kaynak sağlayıcısı bağımlılıkları kaldırdık doğrulayın.

   > [!NOTE]
   > Bağımlı kaynaklar kaynak sağlayıcısı şu anda kullanıyorsanız bile SQL kaynak sağlayıcısını kaldırma devam edecek.
  
2. SQL kaynak sağlayıcısı bir kopyasını ikili almak ve geçici bir dizine içeriği ayıklamak için ayıklayıcısı çalıştırın.

3. Yeni yükseltilmiş bir PowerShell konsol penceresi açın ve SQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin.

4. Aşağıdaki parametreleri kullanarak DeploySqlProvider.ps1 komut dosyasını çalıştırın:

    - **Kaldırma**. İlişkili tüm kaynakları ve kaynak sağlayıcısını kaldırır.
    - **PrivilegedEndpoint**. IP adresi veya ayrıcalıklı uç noktanın DNS adı.
    - **CloudAdminCredential**. Bulut Yöneticisi, ayrıcalıklı uç noktasına erişmek için gerekli kimlik bilgileri.
    - **AzCredential**. Azure yığın hizmet yönetici hesabının kimlik bilgileri. Azure yığın dağıtmak için kullanılan kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulama Hizmetleri PaaS teklifi](azure-stack-app-service-overview.md)
