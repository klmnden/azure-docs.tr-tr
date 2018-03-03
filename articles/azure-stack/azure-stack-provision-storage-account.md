---
title: "Azure yığın depolama hesaplarında | Microsoft Docs"
description: "Bir Azure yığın depolama hesabı oluşturmayı öğrenin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: e1152110-b756-4c1a-9fa2-73fe3ab0ad8e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: a599d809ba3da8487a6c5d115bf04922a546e6ad
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="storage-accounts-in-azure-stack"></a>Azure Stack'teki depolama hesapları
Depolama hesapları, Blob ve Tablo hizmetlerinin yanı sıra depolama verisi nesneleriniz için benzersiz ad alanı içerir. Varsayılan olarak hesabınızdaki veriler, depolama hesabı sahibi olarak sizin kullanımınıza sunulur.

1. Azure yığın POC bilgisayarda oturum `https://adminportal.local.azurestack.external` olarak [yönetici](azure-stack-connect-azure-stack.md)ve ardından **yeni** > **veri + depolama**  >  **Depolama hesabı**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. İçinde **depolama hesabı oluşturma** dikey penceresinde, depolama hesabınız için bir ad yazın. Yeni bir **kaynak grubu**, veya varolan bir tanesini seçin ve ardından **oluşturma** depolama hesabı oluşturmak için.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Yeni depolama hesabınız görmek için tıklatın **tüm kaynakları**, depolama hesabı için arama yapın ve adına tıklayın.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Sonraki adımlar
[Azure Resource Manager şablonlarını kullanma](user/azure-stack-arm-templates.md)

[Azure depolama hesapları hakkında bilgi edinin](../storage/common/storage-create-storage-account.md)

[Azure yığın Azure tutarlı depolama doğrulama Kılavuzu'nu indirin](http://aka.ms/azurestacktp1doc)
