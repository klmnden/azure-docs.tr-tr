---
title: Azure Stack'te MySQL kaynak sağlayıcısını kaldırma | Microsoft Docs
description: Azure Stack dağıtımınıza MySQL kaynak sağlayıcısı nasıl kaldırabileceğiniz öğrenin.
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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: ca5322c3c874c434f16a42900227a47245851b02
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2018
ms.locfileid: "51854042"
---
# <a name="remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını kaldırma

MySQL kaynak sağlayıcısı kaldırmadan önce tüm sağlayıcısı bağımlılıklarını kaldırmanız gerekir. Kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasını da gerekir.

> [!NOTE]
> Kaynak sağlayıcısı yükleyiciler, indirme bağlantıları bulabilirsiniz [kaynak sağlayıcı önkoşulları dağıtma](.\azure-stack-mysql-resource-provider-deploy.md#prerequisites).

## <a name="dependency-cleanup"></a>Bağımlılık temizleme

Kaynak Sağlayıcısı'nı kaldırmak için DeployMySqlProvider.ps1 betik çalıştırılmadan önce yapmak için birkaç temizleme görevi vardır.

Azure Stack Kiracı kullanıcıları için aşağıdaki temizleme görevlerini sorumludur:

* Tüm veritabanları, kaynak Sağlayıcısı'ndan silin. (Kiracı veritabanlarını silerek verileri silmez.)
* Sağlayıcı ad alanından kaydını silin.

Azure Stack operatörü için aşağıdaki temizleme görevlerini sorumludur:

* Barındırma sunucusu MySQL bağdaştırıcısından siler.
* MySQL bağdaştırıcı başvuru planları siler.
* MySQL bağdaştırıcısı ile ilişkili olan herhangi bir kota siler.

## <a name="to-remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcıyı kaldırmak için

1. Tüm mevcut MySQL kaynak sağlayıcısı bağımlılıkları kaldırmış olduğunuz doğrulayın.

   >[!NOTE]
   >Bağımlı kaynaklar halen kaynak sağlayıcısı kullanıyor olsanız bile, MySQL kaynak sağlayıcısını kaldırma devam edecek.
  
2. İkili MySQL kaynak sağlayıcısı bir kopyasını alın ve ardından içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.
3. İkili SQL kaynak sağlayıcısı bir kopyasını alın ve ardından içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.
4. Yeni yükseltilmiş bir PowerShell konsol penceresi açın ve MySQL kaynak sağlayıcısı ikili dosyaları ayıkladığınız dizine geçin.
5. Aşağıdaki parametreleri kullanarak DeployMySqlProvider.ps1 betiği çalıştırın:
    - **Kaldırma**. Kaynak sağlayıcısı ile ilişkili tüm kaynakları kaldırır.
    - **PrivilegedEndpoint**. Ayrıcalıklı uç noktasının DNS adı veya IP adresi.
    - **AzureEnvironment**. Azure Stack dağıtmak için kullanılan Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir.
    - **CloudAdminCredential**. Ayrıcalıklı uç noktasına erişmek gerekli bulut Yöneticisi kimlik bilgileri.
    - **DirectoryTenantID**
    - **AzCredential**. Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[PaaS olarak App Services teklifi](azure-stack-app-service-overview.md)
