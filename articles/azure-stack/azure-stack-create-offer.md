---
title: Bir teklifi Azure yığınında oluşturun | Microsoft Docs
description: Bulut Yöneticisi olarak, kullanıcılarınız için bir teklif Azure yığınında oluşturmayı öğrenin.
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
ms.date: 06/07/2018
ms.author: brenduns
ms.openlocfilehash: e5b96a9464bf4d0e3b69d2f635da32c6648ce793
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247526"
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

[Sunar](azure-stack-key-features.md) satın almak veya abone olmak için kullanıcılara sağlayıcılardan bir veya daha fazla plan gruplarıdır. Bu belge içeren bir teklifi oluşturmak gösterilmiştir [oluşturduğunuz planı](azure-stack-create-plan.md). Bu teklif aboneleri sanal makineleri ayarlamanıza olanak sağlar.

1. Azure yığın Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) seçip **yeni** > **Kiracı sunar + planları** > **teklif**.

   ![Teklif oluşturma](media/azure-stack-create-offer/image01.png)
  
2. Altında **yeni teklif**, girin bir **görünen adı** ve **kaynak adı**ve ardından **kaynak grubu**seçin **oluştur Yeni** veya **var olanı kullan**. Görünen ad, teklifin kolay addır. Teklife abone olduğunuzda, kullanıcıları görmek teklif hakkında bilgiler yalnızca bu kolay addır. Kullanıcıların hangi teklifi gelen anlamasına yardımcı olur sezgisel bir ad kullanın. Kaynak Adını yalnızca yönetici görebilir. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![Yeni Teklif](media/azure-stack-create-offer/image01a.png)
  
3. Seçin **temel planları** açmak için **planlama**. Teklife dahil ve ardından istediğiniz planı seçin **seçin**. Teklif seçin oluşturmak için **oluşturma**.

   ![planı seçin](media/azure-stack-create-offer/image02.png)
  
4. Teklif oluşturduktan sonra durumu değiştirebilirsiniz. Teklifler yapılmalı *ortak* bunlar abone olduğunuzda tam görünümünü almak kullanıcılar için. Teklifler olabilir:

   - **Ortak**: kullanıcılar için görünür.
   - **Özel**: Yöneticiler buluta yalnızca görünür. Plan veya teklif taslağı oluşturma sırasında bu ayar yararlıdır veya Bulut Yöneticisi istiyorsa, [kullanıcılar için her Abonelik Oluştur](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Yetkisi Alınmış**: Yeni abonelere kapalıdır. Bulut yöneticisine kullanabilirsiniz yetkisi alınmış gelecekteki abonelikleri engellemek ancak geçerli aboneleri etkilenmemesini bırakın.

   > [!TIP]  
   > Teklif değişiklikler hemen kullanıcıya görünmez. Değişiklikleri görmek için oturumu kapatın ve yeniden kullanıcı portalına yeni teklif görmek için oturum açma kullanıcıları olabilir.

   Teklif Durumu değiştirmek için:

   - **1803 ve sonraki sürümleri**:  
     Bir genel bakış teklifin, seçin **erişilebilirlik durumu**. Kullanmak istediğiniz durumu seçin (örneğin, *ortak*) ve ardından **kaydetmek**.
 
     ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/change-state.png)

     Bir teklif eriştikten sonra alternatif olarak, gidebilirsiniz **ayarları teklif**. Seçin **erişilebilirlik durumu** durumunu değiştirme.

   - **Sürüm 1803 önce**:  
     Seçin **tüm kaynakları**, yeni teklifiniz için arama yapın ve ardından yeni teklif seçin. Seçin **durum değiştirme**ve ardından **ortak**.

   > [!NOTE]
   > Varsayılan teklifler, planlar ve kotalar oluşturmak için PowerShell'i de kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure yığın Hizmet Yöneticisi Benioku](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).

## <a name="next-steps"></a>Sonraki adımlar

- [Abonelikleri oluşturma](azure-stack-subscribe-plan-provision-vm.md)
- [Sanal makine sağlama](azure-stack-provision-vm.md)
