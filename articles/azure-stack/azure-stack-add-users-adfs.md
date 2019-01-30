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
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.lastreviewed: 10/15/2018
ms.openlocfilehash: 7ecffacee03650adc709a055ba942ef182da5fb8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239997"
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