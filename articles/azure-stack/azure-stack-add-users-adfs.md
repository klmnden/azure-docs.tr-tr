---
title: Azure Stack ADFS için kullanıcılar ekleyin | Microsoft Docs
description: Azure Stack, ADFS dağıtımları için kullanıcı ekleme hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2019
ms.author: patricka
ms.reviewer: unknown
ms.lastreviewed: 02/21/2019
ms.openlocfilehash: 1b47739200c79317273ea0c788f21a7ee4a3b818
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56648515"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>AD FS, Azure Stack kullanıcıları ekleme
Kullanabileceğiniz **Active Directory Kullanıcıları ve Bilgisayarları** yararlanarak AD FS, kimlik sağlayıcısı olarak Azure Stack ortamına ek kullanıcılar eklemek için ek bileşenini.

## <a name="add-windows-server-active-directory-users"></a>Windows Server Active Directory kullanıcısı Ekle
> [!TIP]
> Bu örnek, varsayılan azurestack.local ASDK active directory kullanır. 

1. Windows Yönetim Araçları için erişim sağlayan bir hesapla bir bilgisayarda oturum açın ve yeni bir Microsoft Yönetim Konsolu (MMC) açın.
2. Seçin **Dosya > Ekle veya Kaldır ek bileşenini**.
3. Seçin **Active Directory Kullanıcıları ve Bilgisayarları** > **AzureStack.local** > **kullanıcılar**.
4. Seçin **eylem** > **yeni** > **kullanıcı**.
5. Yeni nesne – kullanıcı, kullanıcı bilgilerini ayrıntıları sağlayın. **İleri**’yi seçin.
6. Sağlar ve bir parolayı onaylayın.
7. Seçin **sonraki** değerleri sonlandırmak için. Seçin **son** kullanıcı oluşturmak için.


## <a name="next-steps"></a>Sonraki adımlar
[Hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md)