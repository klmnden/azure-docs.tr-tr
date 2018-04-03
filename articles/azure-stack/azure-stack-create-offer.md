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
ms.date: 03/27/2018
ms.author: brenduns
ms.openlocfilehash: 6a59d0c8144492ef907e5c395f05e4e8a16678a5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

[Sunar](azure-stack-key-features.md) satın alma veya abone olmak için kullanıcılara sağlayıcılardan bir veya daha fazla plan gruplarıdır. Bu belge içeren bir teklifi oluşturmak gösterilmiştir [oluşturduğunuz planı](azure-stack-create-plan.md) son adımda. Bu teklif aboneleri sanal makineler sağlamak için olanak sağlar.

1. Azure yığın Yönetici portalında oturum açın (https://adminportal.local.azurestack.external) > tıklatın **yeni** > **Kiracı sunar + planları** > **teklif**.

   ![](media/azure-stack-create-offer/image01.png)
2. İçinde **yeni teklif** bölmesinde, doldurun **görünen adı** ve **kaynak adı**ve ardından yeni veya varolan bir seçin **kaynak grubu**. Görünen ad, teklifin kolay addır. Abone olurken kullanıcıları görmek teklif hakkında bilgiler yalnızca bu kolay addır. Bu nedenle, hangi teklifi geldiğini anlamak kullanıcı yardımcı olan bir sezgisel adı kullandığınızdan emin olun. Kaynak Adını yalnızca yönetici görebilir. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![](media/azure-stack-create-offer/image01a.png)
3. Tıklatın **temel planları** açmak için **planlama** bölmesinde teklife dahil ve ardından istediğiniz planı seçin **seçin**. Teklifi oluşturmak için **Create** (Oluştur) seçeneğine tıklayın.

   ![](media/azure-stack-create-offer/image02.png)
4. Teklif oluşturduktan sonra durumu değiştirebilirsiniz. Teklifler yapılmalı *ortak* bunlar abone olduğunuzda tam görünümünü almak kullanıcılar için. Teklifler olabilir:
   - **Ortak**: kullanıcılar için görünür.
   - **Özel**: Yöneticiler buluta yalnızca görünür. Plan veya teklif taslağı oluşturma sırasında kullanışlı veya Bulut Yöneticisi istiyorsa, [kullanıcılar için her Abonelik Oluştur](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Yetkisi Alınmış**: Yeni abonelere kapalıdır. Bulut yöneticisine kullanabilirsiniz gelecekteki abonelikleri engellemek, ancak geçerli aboneleri dokunulmadan bırakmak için yetkisi alınmış.

   > [!TIP]  
   > Teklif değişiklikleri kullanıcıya hemen görünür değildir. Değişiklikleri görmek için oturum kapatma ve oturum açma yeni teklif görmek için yeniden kullanıcı portalına kullanıcıları olabilir. 

   Teklif Durumu değiştirmek için: 

   - **1803 ve sonraki sürümleri**:  
     Genel Bakış dikey teklifin penceresinde **erişilebilirlik durumu**, olduğu gibi kullanmak istediğiniz durumu seçin *ortak*ve ardından **kaydetmek**. 
 
     ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/change-state.png) 

     Alternatif olarak, bir teklif eriştikten sonra goto olabilir **ayarları teklif**ve ardından **erişilebilirlik durumu** durumunu değiştirme. 

   - **Sürüm 1803 önce**:  
     Tıklatın **tüm kaynakları**, arama yeni teklifiniz için yeni teklifini tıklatın, **durum değiştirme**ve ardından **ortak**.

  
   > [!NOTE] 
   > Varsayılan teklifler, planlar ve kotalar oluşturmak için PowerShell'i de kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure yığın Hizmet Yöneticisi Benioku](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
   >


### <a name="next-steps"></a>Sonraki adımlar
[Abonelikleri oluşturma](azure-stack-subscribe-plan-provision-vm.md)      
[Sanal makine sağlama](azure-stack-provision-vm.md)
