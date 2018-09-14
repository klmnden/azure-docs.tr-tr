---
title: Azure Stack'te SQL kaynak sağlayıcısı kaldırılıyor | Microsoft Docs
description: Bilgi nasıl, SQL kaynak sağlayıcısı, Azure Stack dağıtımdan kaldırın.
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
ms.date: 09/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 42d8a5a8073d2650b9e023305472f28d4f1c738f
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580108"
---
# <a name="remove-the-sql-resource-provider"></a>SQL kaynak sağlayıcısını kaldırma

SQL kaynak sağlayıcısı kaldırmadan önce tüm sağlayıcısı bağımlılıklarını kaldırmanız gerekir. Kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasını da gerekir.

Çalıştırmadan önce uygulamanız gereken birkaç temizleme görev _DeploySqlProvider.ps1_ kaynak sağlayıcısını kaldırma komut dosyası.
Kiracılar için aşağıdaki temizleme görevlerini sorumludur:

* Tüm veritabanları, kaynak Sağlayıcısı'ndan silin. (Kiracı veritabanlarını silerek verileri silmez.)
* Kaynak sağlayıcısı ad alanından kaydını silin.

Yönetici için aşağıdaki temizleme görevlerini sorumludur:

* SQL kaynak Sağlayıcısı'ndan barındırma sunucuları siler.
* SQL kaynak sağlayıcısı referans planları siler.
* SQL kaynak sağlayıcısı ile ilişkili olan herhangi bir kota siler.

## <a name="to-remove-the-sql-resource-provider"></a>SQL kaynak Sağlayıcısı'nı kaldırmak için

1. Tüm mevcut SQL kaynak sağlayıcısı bağımlılıkları kaldırmış olduğunuz doğrulayın.

   > [!NOTE]
   > Bağımlı kaynaklar halen kaynak sağlayıcısı kullanıyor olsanız bile, SQL kaynak sağlayıcısı kaldırma devam edecek.
  
2. İkili SQL kaynak sağlayıcısı bir kopyasını alın ve ardından içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.

3. Yeni yükseltilmiş bir PowerShell konsol penceresi açın ve SQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin.

4. Aşağıdaki parametreleri kullanarak DeploySqlProvider.ps1 betiği çalıştırın:

    * **Kaldırma**. Kaynak sağlayıcısı ile ilişkili tüm kaynakları kaldırır.
    * **PrivilegedEndpoint**. Ayrıcalıklı uç noktasının DNS adı veya IP adresi.
    * **AzureEnvironment**. Azure Stack dağıtmak için kullanılan Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir.
    * **CloudAdminCredential**. Ayrıcalıklı uç noktasına erişmek gerekli bulut Yöneticisi kimlik bilgileri.
    * **AzCredential**. Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[PaaS olarak App Services teklifi](azure-stack-app-service-overview.md)
