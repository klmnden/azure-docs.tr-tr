---
title: "Bir teklifi Azure yığınında oluşturun | Microsoft Docs"
description: "Bulut Yöneticisi olarak, kullanıcılarınız için bir teklif Azure yığınında oluşturmayı öğrenin."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 2666aacbbaf44ae9b3bcf2df480e835457e6f449
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Azure Stack'te teklif oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

[Sunar](azure-stack-key-features.md) satın alma veya abone olmak için kullanıcılara sağlayıcılardan bir veya daha fazla plan gruplarıdır. Bu belge içeren bir teklifi oluşturmak gösterilmiştir [oluşturduğunuz planı](azure-stack-create-plan.md) son adımda. Bu teklif aboneleri sanal makineler sağlamak için olanak sağlar.

1. Azure yığın Yönetici portalı (https://adminportal.local.azurestack.external) oturum açma > tıklatın **yeni** > **Kiracı sunar + planları**  >   **Teklif**.

   ![](media/azure-stack-create-offer/image01.png)
2. İçinde **yeni teklif** bölmesinde, doldurun **görünen adı** ve **kaynak adı**ve ardından yeni veya varolan bir seçin **kaynak grubu**. Görünen ad, teklifin kolay addır. Abone olurken kullanıcıları görmek teklif hakkında bilgiler yalnızca bu kolay addır. Bu nedenle, hangi teklifi geldiğini anlamak kullanıcı yardımcı olan bir sezgisel adı kullandığınızdan emin olun. Kaynak Adını yalnızca yönetici görebilir. Bu ad, yöneticilerin teklifle Azure Resource Manager kaynağı olarak çalışmak için kullandıkları addır.

   ![](media/azure-stack-create-offer/image01a.png)
3. Tıklatın **temel planları** açmak için **planlama** bölmesinde teklife dahil ve ardından istediğiniz planı seçin **seçin**. Teklifi oluşturmak için **Create** (Oluştur) seçeneğine tıklayın.

   ![](media/azure-stack-create-offer/image02.png)
4. Tıklatın **tüm kaynakları**, arama yeni teklifiniz için yeni teklifini tıklatın, **durum değiştirme**ve ardından **ortak**.

   ![](media/azure-stack-create-offer/image03.png)

Teklifler, kullanıcıların abone olurken tam görünümünü almak için genel yapılmalıdır. Teklifler olabilir:

* **Ortak**: kullanıcılar için görünür.
* **Özel**: Bulut yöneticileri yalnızca görünür. Plan veya teklif taslağı oluşturma sırasında kullanışlı veya Bulut Yöneticisi istiyorsa, [kullanıcılar için her Abonelik Oluştur](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
* **Yetkisi Alınmış**: Yeni abonelere kapalıdır. Bulut yöneticisine kullanabilirsiniz gelecekteki abonelikleri engellemek, ancak geçerli aboneleri dokunulmadan bırakmak için yetkisi alınmış.

Teklif değişiklikleri kullanıcıya hemen görünür değildir. Değişiklikleri görmek için "abonelik Seçici" Yeni Abonelik görmek için oturum kapatma/oturum açma gerekebilir kaynakları/kaynak grupları oluştururken.

> [!NOTE]
>Açıklandığı gibi PowerShell kullanarak varsayılan teklifler, planları ve kotalar ayrıca oluşturabilirsiniz [Azure yığın Hizmet Yöneticisi Benioku](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Sonraki adımlar
[Abonelikleri oluşturma](azure-stack-subscribe-plan-provision-vm.md)      
[Sanal makine sağlama](azure-stack-provision-vm.md)
