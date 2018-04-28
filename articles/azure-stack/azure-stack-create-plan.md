---
title: Azure yığınında bir plan oluşturun | Microsoft Docs
description: Bulut yönetici olarak aboneleri sanal makine sağlamak olanak sağlayan bir plan oluşturun.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 3dc92e5c-c004-49db-9a94-783f1f798b98
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/23/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: b1bfff16c4f51a9fa53204930df78cbd2cf19b8d
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-plan-in-azure-stack"></a>Azure Stack'te plan oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

[Planlar](azure-stack-key-features.md), bir veya birden fazla hizmetten oluşan gruplardır. Bir sağlayıcısı olarak, kullanıcılarınıza sunmak için planları oluşturabilirsiniz. Buna karşılık, kullanıcılarınızın planları ve içerirler Hizmetleri'ni kullanmak için Teklifleriniz için abone olun. Bu örnekte işlem, ağ ve depolama kaynak sağlayıcıları içeren bir planı oluşturulacağını gösterir. Bu plan aboneleri sanal makineler sağlamak için olanak sağlar.

1. Azure yığın Yönetici portalında oturum açın (https://adminportal.local.azurestack.external).

2. Bir planı ve kullanıcıların abone olabilirsiniz teklifi oluşturmak için seçin **yeni** > **sunar + planları** > **planı**.  
   ![Bir plan seçin](media/azure-stack-create-plan/select-plan.png)

3. İçinde **yeni plan** bölmesinde, doldurun **görünen adı** ve **kaynak adı**. Görünen ad kullanıcıların gördüğü planın kolay addır. Yalnızca yönetici admins plan bir Azure Resource Manager kaynak olarak çalışmak için kullandığınız adı kaynak adı görebilirsiniz.  
   ![Ayrıntıları belirtin](media/azure-stack-create-plan/plan-name.png)

4. Yeni bir **kaynak grubu**, veya plan için bir kapsayıcı olarak varolan bir tanesini seçin.  
   ![Kaynak grubu belirtin](media/azure-stack-create-plan/resource-group.png)

5. Seçin **Hizmetleri** onay kutusunu seçin **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**. Ardından, seçin **seçin** yapılandırmayı kaydetmek için. Fare her seçenek üzerine geldiğinde onay kutuları görüntülenir.  
   ![Hizmetleri seçin](media/azure-stack-create-plan/services.png)

6. Seçin **kotaları**, **Microsoft.Storage (yerel)** ve ardından varsayılan kota ya da seçin **yeni kota oluştur** kota özelleştirmek için.  
   ![Kotalar](media/azure-stack-create-plan/quotas.png)

7. Yeni kota oluşturuyorsanız, girin bir **adı** kota için > kota değerlerini belirtin > seçin **Tamam**. **Kota oluştur** bölmesini kapatır.
   ![Yeni kota](media/azure-stack-create-plan/new-quota.png)

   Ardından, oluşturduğunuz yeni kota de seçin. Kota seçerek atar ve Seçim Bölmesi kapatır.  
   ![Kota atayın](media/azure-stack-create-plan/assign-quota.png)

8. Adım 6 ve 7 oluşturmak ve kotaları atamak için yineleyin **Microsoft.Network (yerel)** ve **Microsoft.Compute (yerel)**.  Tüm üç hizmeti atanmış kotalar varsa, bunlar aşağıdaki görüntüye benzer görünür.  
   ![Tam kota atamaları](media/azure-stack-create-plan/all-quotas-assigned.png)

9. İçinde **kotaları** bölmesinde seçin **Tamam**ve ardından **yeni plan** bölmesinde seçin **oluşturma** planı oluşturmak için.  
    ![Planı oluşturma](media/azure-stack-create-plan/create.png)
10. Yeni planınız görmek için seçin **tüm kaynakları**, plan için arama yapın ve adını seçin. Kaynak listenizi uzunsa kullanmak **arama** planınız ada göre bulmak için.  
   ![Planını gözden geçirin](media/azure-stack-create-plan/plan-overview.png)

### <a name="next-steps"></a>Sonraki adımlar
[Teklif oluşturma](azure-stack-create-offer.md)
