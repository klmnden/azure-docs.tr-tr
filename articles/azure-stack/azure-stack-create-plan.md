---
title: Azure Stack'te plan oluşturma | Microsoft Docs
description: Bulut Yöneticisi olarak, abonelerin sanal makine sağlamasına olanak tanıyan bir plan oluşturun.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 8fb36ac19e731f9274975a3bf1183869f15fbceb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55984622"
---
# <a name="create-a-plan-in-azure-stack"></a>Azure Stack'te plan oluşturma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

[Planlar](azure-stack-key-features.md), bir veya birden fazla hizmetten oluşan gruplardır. Bir sağlayıcı, kullanıcılarınıza sunabileceğiniz planlar oluşturabilirsiniz. Buna karşılık, kullanıcılarınızın içerirler hizmetler ve planlar kullanılacak Teklifleriniz için abone olun. Bu örnekte işlem, ağ ve depolama kaynağı sağlayıcılarını içeren bir plan oluşturma işlemini gösterir. Bu plan aboneleri sanal makineler sağlama olanağı sağlar.

1. Oturum [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external).

2. Bir plan ve kullanıcıların abone olabileceği teklifi oluşturmak için seçin **+ kaynak Oluştur**, ardından **sunar + planlar**, ardından **planı**.
  
   ![Bir plan seçin](media/azure-stack-create-plan/select-plan.png)

3. Altında **yeni plan**, girin bir **görünen ad** ve **kaynak adı**. Görünen ad kullanıcılar görebilir planın kolay adıdır. Yalnızca yönetici Azure Resource Manager kaynağı olarak planla çalışmak için hangi yöneticileri kullanın kaynak adını görebilirsiniz.

   ![Ayrıntılarını belirtin](media/azure-stack-create-plan/plan-name.png)

4. Yeni bir **kaynak grubu**, veya varolan bir plan için kapsayıcı olarak seçin.

   ![Kaynak grubunu belirtin](media/azure-stack-create-plan/resource-group.png)

5. Seçin **Hizmetleri** ve onay kutusunu seçip **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**. Ardından, **seçin** yapılandırmayı kaydetmek için. Onay kutularını her seçenek üzerine fare geldiğinde görüntülenir.
  
   ![Hizmetleri seçin](media/azure-stack-create-plan/services.png)

6. Seçin **kotalar**, **Microsoft.Storage (yerel)**, seçin veya varsayılan kota seçin **yeni kota oluştur** özelleştirilmiş bir kota oluşturmak için.
  
   ![Kotalar](media/azure-stack-create-plan/quotas.png)

7. Yeni kota oluşturuyorsanız girin bir **adı** kotası için > kota değerlerini belirtin > seçin **Tamam**. **Kota oluştur** iletişim kutusunu kapatır.

   ![Yeni kota](media/azure-stack-create-plan/new-quota.png)

   Ardından, oluşturduğunuz yeni kota de seçin. Kota seçerek atar ve seçimi iletişim kutusunu kapatır.
  
   ![Kota atayın](media/azure-stack-create-plan/assign-quota.png)

8. 6 ve 7 oluşturmak ve atamak için kotalar için adımları yineleyin **Microsoft.Network (yerel)** ve **Microsoft.Compute (yerel)**. Tüm üç hizmeti atanan kota olduğunda, sonraki örnekte olduğu gibi göz atacağız.

   ![Tam kota atamaları](media/azure-stack-create-plan/all-quotas-assigned.png)

9. Altında **kotalar**, seçin **Tamam**ve ardından altındaki **yeni plan**, seçin **Oluştur** planı oluşturun.

    ![Plan oluşturma](media/azure-stack-create-plan/create.png)

10. Yeni planınızı görmek için seçin **tüm kaynakları**adını seçin ve ardından planlama arayın. Kaynakları listesini uzunsa kullanın **arama** adıyla planınızı bulunacak.

   ![Planını gözden geçirin](media/azure-stack-create-plan/plan-overview.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Teklif oluşturma](azure-stack-create-offer.md)
