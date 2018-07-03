---
title: Azure Stack'te teklif oluşturma | Microsoft Docs
description: Bir bulutun Yöneticisi olarak Azure Stack'te teklif kullanıcılarınız için oluşturma konusunda bilgi edinin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/2/2018
ms.author: brenduns
ms.openlocfilehash: eed715a7c2cb967f6c9ea0b7d4442a4f9976bd17
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37345898"
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

[Sunar](azure-stack-key-features.md) sağlayıcıları satın almanız veya abone kullanıcılara sunmak bir veya daha fazla plan gruplarıdır. Bu belgede, içeren bir teklif oluşturma işlemini göstermektedir [oluşturduğunuz planı](azure-stack-create-plan.md). Bu teklifin aboneleri sanal makineleri ayarlama olanağı sağlar.

1. Azure Stack Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) seçip **yeni** > **Kiracı sunar + planlar** > **teklif**.

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

   Teklif Durumu değiştirmek için:

   - **1803 ve sonraki sürümleri**:  
     Genel Bakış'ta teklif için seçin **erişilebilirlik durumu**. Kullanmak istediğiniz durumu seçin (örneğin, *genel*) ve ardından **Kaydet**.
 
     ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/change-state.png)

     Bir teklif eriştikten sonra alternatif olarak, gidebilirsiniz **teklif ayarları**. Seçin **erişilebilirlik durumu** durumunu değiştirmek için.

   - **Sürümünden 1803**:  
     Seçin **tüm kaynakları**yeni teklifiniz için arama yapın ve ardından yeni bir teklif seçin. Seçin **durumunu değiştir**ve ardından **genel**.

   > [!NOTE]
   > Varsayılan tekliflerini, planları ve kotaları oluşturmak için PowerShell de kullanabilirsiniz. Daha fazla bilgi için [Azure Stack PowerShell modülü 1.3.0](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0).

## <a name="next-steps"></a>Sonraki adımlar

- [Abonelik oluşturma](azure-stack-subscribe-plan-provision-vm.md)
- [Sanal makine sağlama](azure-stack-provision-vm.md)
