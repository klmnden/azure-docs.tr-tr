---
title: Azure Stack ADFS için kullanıcılar ekleyin | Microsoft Docs
description: Azure Stack, ADFS dağıtımları için kullanıcı ekleme hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: patricka
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.openlocfilehash: f8abacbcb05d1346931b5c2e1097660cfbd8e594
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344175"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>AD FS, Azure Stack kullanıcıları ekleme
Kullanabileceğiniz **Active Directory Kullanıcıları ve Bilgisayarları** yararlanarak AD FS, kimlik sağlayıcısı olarak Azure Stack ortamına ek kullanıcılar eklemek için ek bileşenini.

## <a name="add-windows-server-active-directory-users"></a>Windows Server Active Directory kullanıcısı Ekle
> [!TIP]
> Bu örnek, varsayılan azurestack.local ASDK active directory kullanır. 

1.  Windows Yönetim Araçları için erişim sağlayan bir hesapla bir bilgisayarda oturum açın ve yeni bir Microsoft Yönetim Konsolu (MMC) açın.
2.  Tıklayın **Dosya > Ekle veya Kaldır ek bileşenini**.
3.  Seçin **Active Directory Kullanıcıları ve Bilgisayarları** > **AzureStack.local** > **kullanıcılar**.
4.  Tıklayın **eylem** > **yeni** > **kullanıcı**.
5.  Yeni nesne – kullanıcı penceresi, sağlamak ve bir parolayı onaylayın
6.  Tıklayın **sonraki** değerleri sonlandırma ve kullanıcı oluşturmak için Son'a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
[Hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md)