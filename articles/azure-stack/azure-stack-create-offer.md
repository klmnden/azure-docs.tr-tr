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
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: sethm
ms.reviewer: efemmano
ms.lastreviewed: 03/07/2019
ms.openlocfilehash: 42336205726823dd04e0412f29c3e7a23d134d39
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57764002"
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

[Sunar](azure-stack-key-features.md) sağlayıcıları bu kullanıcıları satın alın veya abone, kullanıcılara sunmak bir veya daha fazla plan gruplarıdır. Bu makalede içeren bir teklif oluşturma [oluşturduğunuz planı](azure-stack-create-plan.md). Bu teklifin aboneleri sanal makineleri (VM'ler) ayarlama olanağı sağlar.

## <a name="create-an-offer-1902-and-later"></a>Teklif (1902 ve üzeri) oluşturma

1. Oturum [Azure Stack Yönetici portalı](https://adminportal.local.azurestack.external) seçip **+ kaynak Oluştur**, ardından **sunar + planlar**ve ardından **teklif**.

   ![Teklif oluşturma](media/azure-stack-create-offer/offers.png)

2. Teklif adını tanımlamak ve varolan ekleyin veya yeni bir temel plan ve eklenti planları oluşturma sağlayan bir sekmeli kullanıcı arabirimi görüntülenir. En önemlisi de oluşturmak karar vermeden önce oluşturduğunuz teklif ayrıntılarını gözden geçirebilirsiniz.

   İçinde **Temelleri** sekmesindeki **yeni teklif**, girin bir **görünen ad** ve **kaynak adı**ve ardından altındaki **kaynak Grup**seçin **Yeni Oluştur** veya **var olanı kullan**. Görünen ad, teklifin kolay addır. Yalnızca Kullanıcı Portalı'nda bir teklife abone olduğunda kullanıcıların gördüğü teklif ilgili bilgilerin Bu kolay addır. Kullanıcılar teklife ne geldiğini anlamak yardımcı olan sezgisel bir ad kullanın. Yalnızca yönetici kaynak adını görebilirsiniz. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır. Bu sekmede, bu teklif genel hale getirmek ya da özel, onu varsayılan koruma seçebilirsiniz. Yapabilecekleriniz [teklif genel veya özel durumunu değiştirme](#change-the-state-of-an-offer) daha sonra da.

   ![Yeni Teklif](media/azure-stack-create-offer/new-offer.png)
  
3. Seçin **temel planlar** sekmesi. Teklife dahil etmek istediğiniz plana seçin.

   ![Plan seçin](media/azure-stack-create-offer/select-plan.png)

4. Bu noktada, temel plan değiştirmek için bir eklenti planı oluşturabilirsiniz, ancak bu özellik isteğe bağlıdır. Sonraki makalede, bir eklenti planı oluşturacağız [Azure Stack eklenti planları](create-add-on-plan.md).

5. Seçin **gözden geçir + Oluştur** sekmesi. Tüm değerlerin doğru olduğundan emin olmak için teklif özeti gözden geçirin. Arabirim kotaları tüm gerekli düzenlemeleri yapmak için seçtiğiniz planlarında, bir plandaki her kota ayrıntılarını görüntülemek ve geri dönmek için teker teker genişletmenize olanak sağlar.

6. Seçin **Oluştur** teklifi oluşturmak için.

   ![Gözden geçirme ve oluşturma](media/azure-stack-create-offer/review-offer.png)

### <a name="change-the-state-of-an-offer"></a>Bir teklif durumunu değiştirme

Teklif oluşturduktan sonra kendi durumunu değiştirebilirsiniz. Teklifler yapılacak **genel** kullanıcıların abone tam bir görünüm elde edin. Teklifler olabilir:

   - **Genel**: Kullanıcılara görünür.
   - **Özel**: Yalnızca Yöneticiler buluta görünür. Bu ayar yararlıdır plan veya teklif taslağı oluşturma sırasında veya Bulut Yöneticisi istiyorsa, [kullanıcılar için her bir abonelik oluşturmak](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Kullanımdan**: Yeni abonelere kapalıdır. Bulut yöneticisine sonraki abonelikleri engellemek, ancak mevcut aboneler etkilenmeyen bırakmak için teklifleri yetkisini alabilir.

   > [!TIP]  
   > Kullanıcı için teklif değişiklikler hemen görünmez. Değişiklikleri görmek için kullanıcıların oturumu kapatın ve Kullanıcı Portalı'na yeni teklif görmek için tekrar oturum açmanızın gerekebilir.

Teklif Durumu değiştirmek için iki yolu vardır:

1. İçinde **tüm kaynakları**, teklif adını seçin. Üzerinde **genel bakış** select teklif için ekran **durumunu değiştir**. Kullanmak istediğiniz durumu seçin (örneğin, **genel**).

   ![Durum seçin](media/azure-stack-create-offer/change-state.png)

2. İçinde **tüm kaynakları**, teklif adını seçin. Ardından **teklif ayarları**. Kullanmak istediğiniz durumu seçin (örneğin, **genel**), ardından **Kaydet**.

   ![Erişilebilirlik durumunu seçin](media/azure-stack-create-offer/offer-settings.png)

## <a name="create-an-offer-1901-and-earlier"></a>(1901 ve öncesi) bir teklif oluşturun

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
