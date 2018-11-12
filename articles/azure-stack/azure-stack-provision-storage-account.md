---
title: Azure stack'teki depolama hesapları | Microsoft Docs
description: Bir Azure Stack depolama hesabı oluşturmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: e1152110-b756-4c1a-9fa2-73fe3ab0ad8e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2018
ms.author: mabrigg
ms.openlocfilehash: 608525a1f06a396c7df7d764f236f33af77dccc6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51235352"
---
# <a name="storage-accounts-in-azure-stack"></a>Azure Stack'teki depolama hesapları
Depolama hesapları, Blob ve Tablo hizmetlerinin yanı sıra depolama verisi nesneleriniz için benzersiz ad alanı içerir. Varsayılan olarak hesabınızdaki veriler, depolama hesabı sahibi olarak sizin kullanımınıza sunulur.

1. Azure Stack POC bilgisayarda oturum açın `https://adminportal.local.azurestack.external` olarak [yönetici](azure-stack-connect-azure-stack.md)ve ardından **+ kaynak Oluştur** > **veri + depolama**  >  **Depolama hesabı**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. İçinde **depolama hesabı oluşturma** dikey penceresinde, depolama hesabınız için bir ad yazın. Yeni bir **kaynak grubu**, veya varolan bir tanesini seçin ve ardından tıklayın **Oluştur** depolama hesabı oluşturmak için.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Yeni depolama hesabınızı görmek için tıklayın **tüm kaynakları**, ardından depolama hesabı için arama yapın ve onun adına tıklayın.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Sonraki adımlar
[Azure Resource Manager şablonlarını kullanma](user/azure-stack-arm-templates.md)

[Azure depolama hesapları hakkında bilgi edinin](../storage/common/storage-create-storage-account.md)

[Azure Stack Azure ile tutarlı depolama doğrulama kılavuzunu indirin](https://aka.ms/azurestacktp1doc)
