---
title: Azure Stack'te teklif oluşturma | Microsoft Docs
description: Bir bulutun Yöneticisi olarak Azure Stack'te teklif kullanıcılarınız için oluşturma konusunda bilgi edinin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/09/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 01/09/2019
ms.openlocfilehash: 230415b84f06beac549693d49055d7cb998163d8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251461"
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

[Sunar](azure-stack-key-features.md) sağlayıcıları bu kullanıcıları satın alın veya abone, kullanıcılara sunmak bir veya daha fazla plan gruplarıdır. Bu makalede içeren bir teklif oluşturma [oluşturduğunuz planı](azure-stack-create-plan.md). Bu teklifin aboneleri sanal makineleri (VM'ler) ayarlama olanağı sağlar.

1. Oturum [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external) seçip **+ kaynak Oluştur**, ardından **Kiracı sunar + planlar**ve ardından **teklif**.

   ![Teklif oluşturma](media/azure-stack-create-offer/image01.png)
  
2. Altında **yeni teklif**, girin bir **görünen ad** ve **kaynak adı**ve ardından altındaki **kaynak grubu**seçin **oluştur Yeni** veya **var olanı kullan**. Görünen ad, teklifin kolay addır. Yalnızca bir teklife abone olduğunda kullanıcıların gördüğü teklif ilgili bilgilerin Bu kolay addır. Kullanıcılar teklife ne geldiğini anlamak yardımcı olan sezgisel bir ad kullanın. Yalnızca yönetici kaynak adını görebilirsiniz. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![Yeni Teklif](media/azure-stack-create-offer/image01a.png)
  
3. Seçin **temel planlar** açmak için **planlama**. Teklife eklemek ve ardından istediğiniz planları seçin **seçin**. Teklif seçin oluşturmak için **Oluştur**.

   ![Plan seçin](media/azure-stack-create-offer/image02.png)
  
4. Teklif oluşturduktan sonra kendi durumunu değiştirebilirsiniz. Teklifler yapılacak **genel** kullanıcıların abone tam bir görünüm elde edin. Teklifler olabilir:

   - **Genel**: Kullanıcılara görünür.
   - **Özel**: Yalnızca Yöneticiler buluta görünür. Bu ayar yararlıdır plan veya teklif taslağı oluşturma sırasında veya Bulut Yöneticisi istiyorsa, [kullanıcılar için her bir abonelik oluşturmak](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Kullanımdan**: Yeni abonelere kapalıdır. Bulut yöneticisine sonraki abonelikleri engellemek, ancak mevcut aboneler etkilenmeyen bırakmak için teklifleri yetkisini alabilir.

   > [!TIP]  
   > Kullanıcı için teklif değişiklikler hemen görünmez. Değişiklikleri görmek için kullanıcıların oturumu kapatın ve Kullanıcı Portalı'na yeni teklif görmek için tekrar oturum açmanızın gerekebilir.

   Teklif için genel bakış ekranında seçin **erişilebilirlik durumu**. Kullanmak istediğiniz durumu seçin (örneğin, **genel**) ve ardından **Kaydet**.

     ![Durum seçin](media/azure-stack-create-offer/change-stage-1807.png)

     Alternatif, **durumunu değiştir** ve ardından bir eyalet seçin.

    ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/change-stage-select-1807.png)

   > [!NOTE]
   > Varsayılan tekliflerini, planları ve kotaları oluşturmak için PowerShell de kullanabilirsiniz. Daha fazla bilgi için [Azure Stack PowerShell modülü 1.4.0](/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0).

## <a name="next-steps"></a>Sonraki adımlar

- [Abonelik oluşturma](azure-stack-subscribe-plan-provision-vm.md)
- [Sanal makine sağlama](azure-stack-provision-vm.md)
