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
ms.date: 11/20/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 2740da5a51e95a327a868734a7f009dddf40219a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964943"
---
# <a name="remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcısını kaldırma

MySQL kaynak sağlayıcısı kaldırmadan önce tüm sağlayıcısı bağımlılıklarını kaldırmanız gerekir. Kaynak sağlayıcısını yüklemek için kullanılan dağıtım paketinin bir kopyasını da gerekir.

> [!NOTE]
> Kaynak sağlayıcısı yükleyiciler, indirme bağlantıları bulabilirsiniz [kaynak sağlayıcı önkoşulları dağıtma](./azure-stack-mysql-resource-provider-deploy.md#prerequisites).

MySQL kaynak sağlayıcısı kaldırıldığında, Kiracı veritabanlarını barındıran sunucu silinmez.

## <a name="dependency-cleanup"></a>Bağımlılık temizleme

Kaynak Sağlayıcısı'nı kaldırmak için DeployMySqlProvider.ps1 betik çalıştırılmadan önce yapmak için birkaç temizleme görevi vardır.

Azure Stack operatörü için aşağıdaki temizleme görevlerini sorumludur:

* MySQL bağdaştırıcı başvuru planları silin.
* MySQL bağdaştırıcısı ile ilişkili olan herhangi bir kota silin.

## <a name="to-remove-the-mysql-resource-provider"></a>MySQL kaynak sağlayıcıyı kaldırmak için

1. Tüm mevcut MySQL kaynak sağlayıcısı bağımlılıkları kaldırmış olduğunuz doğrulayın.

   > [!NOTE]
   > Bağımlı kaynaklar halen kaynak sağlayıcısı kullanıyor olsanız bile, MySQL kaynak sağlayıcısını kaldırma devam edecek.
  
2. MySQL kaynak Sağlayıcı yükleme paketinin bir kopyasını alın ve ardından içeriği geçici bir dizine ayıklayın ayıklayıcısı çalıştırın.
3. Yeni yükseltilmiş bir PowerShell konsol penceresi açın ve MySQL kaynak sağlayıcısı yükleme dosyaları ayıkladığınız dizine geçin.
4. Aşağıdaki parametreleri kullanarak DeployMySqlProvider.ps1 betiği çalıştırın:
    - **Kaldırma**. Kaynak sağlayıcısı ile ilişkili tüm kaynakları kaldırır.
    - **PrivilegedEndpoint**. Ayrıcalıklı uç noktasının DNS adı veya IP adresi.
    - **AzureEnvironment**. Azure Stack dağıtmak için kullanılan Azure ortamı. Yalnızca Azure AD dağıtımları için gereklidir.
    - **CloudAdminCredential**. Ayrıcalıklı uç noktasına erişmek gerekli bulut Yöneticisi kimlik bilgileri.
    - **DirectoryTenantID**
    - **AzCredential**. Azure Stack hizmet yönetici hesabının kimlik bilgileri. Azure Stack dağıtmak için kullanılan kimlik bilgilerini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[PaaS olarak App Services teklifi](azure-stack-app-service-overview.md)
