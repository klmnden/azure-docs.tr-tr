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
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 4ccff997c7e9f29aafc6966730ab36dfcf72ca9f
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077349"
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

[Sunar](azure-stack-key-features.md) sağlayıcıları satın almanız veya abone kullanıcılara sunmak bir veya daha fazla plan gruplarıdır. Bu belgede, içeren bir teklif oluşturma işlemini göstermektedir [oluşturduğunuz planı](azure-stack-create-plan.md). Bu teklifin aboneleri sanal makineleri ayarlama olanağı sağlar.

1. Azure Stack Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) seçip **+ kaynak Oluştur** > **Kiracı sunar + planlar** > **Teklif**.

   ![Teklif oluşturma](media/azure-stack-create-offer/image01.png)
  
2. Altında **yeni teklif**, girin bir **görünen ad** ve **kaynak adı**ve ardından altındaki **kaynak grubu**seçin **oluştur Yeni** veya **var olanı kullan**. Görünen ad, teklifin kolay addır. Yalnızca bir teklife abone olduğunda kullanıcıların gördüğü teklif ilgili bilgilerin Bu kolay addır. Kullanıcılar teklife ne geldiğini anlamak yardımcı olan sezgisel bir ad kullanın. Kaynak Adını yalnızca yönetici görebilir. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![Yeni Teklif](media/azure-stack-create-offer/image01a.png)
  
3. Seçin **temel planlar** açmak için **planlama**. Teklife eklemek ve ardından istediğiniz planları seçin **seçin**. Teklif seçin oluşturmak için **Oluştur**.

   ![Plan seçin](media/azure-stack-create-offer/image02.png)
  
4. Teklif oluşturduktan sonra kendi durumunu değiştirebilirsiniz. Teklifler yapılacak *genel* kullanıcıların abone tam bir görünüm elde edin. Teklifler olabilir:

   - **Genel**: kullanıcılara görünür.
   - **Özel**: Yöneticiler buluta yalnızca görünür. Bu ayar yararlıdır plan veya teklif taslağı oluşturma sırasında veya Bulut Yöneticisi istiyorsa, [kullanıcılar için her bir abonelik oluşturmak](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Yetkisi Alınmış**: Yeni abonelere kapalıdır. Bulut yöneticisine kullanabilirsiniz yetkisi alınmış sonraki abonelikleri engellemek, ancak mevcut aboneler etkilenmeyen bırakın.

   > [!TIP]  
   > Teklif değişiklikleri kullanıcıya hemen görünmez. Değişiklikleri görmek için kullanıcıların oturumu kapatın ve Kullanıcı Portalı'na yeni teklif görmek için tekrar oturum açmanızın gerekebilir.

   Genel Bakış'ta teklif için seçin **erişilebilirlik durumu**. Kullanmak istediğiniz durumu seçin (örneğin, **genel**) ve ardından **Kaydet**.
 
     ![Durum seçin](media/azure-stack-create-offer/change-stage-1807.png)

     Alternatif, **durumunu değiştir** ve ardından bir eyalet seçin.

    ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/change-stage-select-1807.png)

   > [!NOTE]
   > Varsayılan tekliflerini, planları ve kotaları oluşturmak için PowerShell de kullanabilirsiniz. Daha fazla bilgi için [Azure Stack PowerShell modülü 1.4.0](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0).

## <a name="next-steps"></a>Sonraki adımlar

- [Abonelik oluşturma](azure-stack-subscribe-plan-provision-vm.md)
- [Sanal makine sağlama](azure-stack-provision-vm.md)
