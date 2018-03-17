---
title: "Bu öğreticide, bir Azure yığın teklifi oluştur | Microsoft Docs"
description: "Planları ve kotalar dahil olmak üzere Azure yığın teklif oluşturmayı öğrenin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 083b5e20b89f22cb8e523926858fe9ffb1441319
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-offer-azure-stack-iaas-services"></a>Öğretici: Azure yığın Iaas hizmetleri sunar
Azure yığın bulut yönetici olarak (bazen kiracılar adlandırılır), kullanıcılarınızın abone olabilirsiniz teklifleri oluşturabilirsiniz. Aboneliğini kullanarak, kullanıcılar sonra Azure yığın hizmetleri kullanmasını sağlayabilirsiniz.

Bu öğretici, kullanıcıların, yüklediğiniz Azure yığın Market Windows Server 2016 Datacenter görüntüsü göre sanal makineler oluşturmak bir teklif oluşturulacağını gösterir [önceki Öğreticisi](asdk-marketplace-item.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Plan oluşturma
> * Kotaları belirleyin
> * Ortak kümesi teklife

Azure yığınında services Abonelikleri, teklifleri ve planları kullanan kullanıcılar teslim edilir. Kullanıcıların birden çok teklifleri için abone olabilirsiniz. Bir veya daha fazla plan teklifleri olabilir ve planları, aşağıdaki çizimde gösterildiği gibi bir veya daha fazla hizmet sahip olabilir:

![Abonelikler, teklifleri ve planları](media/asdk-offer-services/sop.png)

Hizmetleri sunan bir Azure yığın operatör olarak, ilk teklif sonra bir planı ve son olarak kotaları oluşturmanız istenir. Bir teklif oluşturulduktan sonra Azure yığın kullanıcılar sonra Kullanıcı Portalı aracılığıyla teklifleri abone olabilirsiniz.

## <a name="create-an-offer"></a>Teklif oluşturma
**Sunar** , satın alma veya abone olmak için kullanıcılara Azure yığın işleçleri sunan bir veya daha fazla planları gruplarıdır.

1. Oturum [Azure yığın portal](https://adminportal.local.azurestack.external) bulut Yöneticisi olarak.

2. Tıklatın **yeni** > **sunar + planları** > **teklif**.

   ![Yeni teklif](media/asdk-offer-services/new-offer.png)

2. İçinde **yeni teklif** bölümünde, doldurmak **görünen adı** ve **kaynak adı**ve ardından yeni veya varolan bir seçin **kaynak grubu**. Görünen ad kullanıcıların gördüğü teklif ait ortak kolay addır. Yalnızca bulut operatörü yöneticileri tarafından teklif bir Azure Resource Manager kaynak olarak çalışmak için kullanılan kaynak adı görebilirsiniz.

   ![Görünen ad](media/asdk-offer-services/offer-display-name.png)


## <a name="create-a-plan"></a>Plan oluşturma
Temel teklif bilgileri girdikten sonra en az bir temel plan teklife eklemeniz gerekir. 

**Planları** kullanıcılara sunulması Azure yığın işletmenlerinin Grup hizmetlere ve bunların ilişkili kotaları izin verme.

1. Tıklatın **temel planları**hem de **planı** 'yi tıklatın **Ekle** yeni bir plan için teklif eklemek için.

   ![Bir plan Ekle](media/asdk-offer-services/new-plan.png)

2. İçinde **yeni Plan** bölümünde, doldurmak **görünen adı** ve **kaynak adı**. Görünen ad kullanıcıların gördüğü planın kolay addır. Yalnızca bulut operatörü, bir Azure Resource Manager kaynak olarak bir plan ile çalışmak için bulut işleçleri tarafından kullanılan kaynak adı görebilirsiniz.

   ![Plan görünen adı](media/asdk-offer-services/plan-display-name.png)

3. Ardından, plana dahil edilecek hizmetleri seçin. Iaas Hizmetleri sunmak için tıklatın **Hizmetleri**seçin **Microsoft.Compute**, **Microsoft.Network**, ve **Microsoft.Storage**ve ardından tıklatın **seçin**.

   ![Plan hizmetleri](media/asdk-offer-services/select-services.png)


## <a name="set-quotas"></a>Kotaları belirleyin
Teklif hizmetleri içeren bir planına sahip, bunların her biri için kotalar ayarlamanız gerekir. 

**Kotalar** önerilmesini plana dahil her hizmet için bir kullanıcı tüketebileceği kaynakları miktarını belirler.

1. Tıklatın **kotaları**ve ardından bir kota oluşturmak istediğiniz hizmeti seçin. 

   Başlamak için ilk işlem hizmeti için bir kota oluşturun. Ad alanı listesinde **Microsoft.Compute** ad alanı ve ardından **yeni kota oluştur**.
   
   ![Yeni kota oluştur](media/asdk-offer-services/create-quota.png)

2. Üzerinde **kota oluştur** bölümünde, kota için bir ad yazın, kota için istediğiniz parametreleri ayarlama ve tıklatın **Tamam**.

   ![Kota adı](media/asdk-offer-services/quota-properties.png)

3. İçin oluşturduğunuz kota seçin **Microsoft.Compute** hizmet.

   ![Kota seçin](media/asdk-offer-services/set-quota.png)

4. Ağ ve depolama hizmetleri için kotaları ayarlayabilir ve kotalar bölümünde Tamam'ı için 1-3 adımları yineleyin. Bundan sonra öğesini **Tamam** üzerinde **yeni plan** planı kurulumu tamamlamak için bölüm. 

   ![Tüm kotaları ayarlama](media/asdk-offer-services/all-quotas-set.png)

5. Teklif tıklayarak oluşturmayı tamamlamak **oluşturma** planı bölümünde. Teklif başarıyla oluşturulduğunda ve kullanılabilir bir teklif listelenir, bir bildirim görürsünüz.

   ![Oluşturulan teklif](media/asdk-offer-services/offer-complete.png)

## <a name="set-offer-to-public"></a>Ortak kümesi teklife
Teklifler, kullanıcıların abone olmak için bir teklif seçerken görmek genel yapılmalıdır. 

Teklifler olabilir:
- **Ortak**: tüm kullanıcılar için görünür.
- **Özel**: Bulut yöneticileri yalnızca görünür. Plan veya teklif taslağı oluşturma sırasında kullanışlı veya Bulut yönetici kullanıcılar için her abonelik oluşturmak isterse.
- **Yetkisi Alınmış**: Yeni abonelere kapalıdır. Bulut yöneticisine kullanabilirsiniz gelecekteki abonelikleri engellemek, ancak geçerli aboneleri dokunulmadan bırakmak için yetkisi alınmış.

> [!TIP]
> Teklif değişiklikleri kullanıcılara hemen görünür değildir. Değişiklikleri görmek için kullanıcıların oturum kapatma ve yeniden oturum açma gerekebilir [kullanıcı portalı](https://portal.local.azurestack.external) yeni teklif görmek için.

Yeni Teklif ortak olarak ayarlamak için: 

1. Pano menüsünde **sunar** ve oluşturduğunuz teklif'ı tıklatın.

2. **Change State** (Durumu Değiştir) öğesine ve **Public** (Genel) seçeneğine tıklayın.

   ![Genel durum](media/asdk-offer-services/set-public.png)

3. Teklif artık Azure yığın Kullanıcı Portalı'nda kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Teklif oluşturma
> * Plan oluşturma
> * Kotaları belirleyin
> * Ortak kümesi teklife

Bir Azure yığın kullanıcı olarak oluşturduğunuz teklif abone öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Bir teklife abone olma](asdk-subscribe-services.md)