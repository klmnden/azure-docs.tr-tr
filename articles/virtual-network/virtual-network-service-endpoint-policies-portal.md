---
title: Oluştur ve ilişkilendir hizmet uç noktası İlkesi - Azure portalı
titlesuffix: Azure Virtual Network
description: Bu makalede, nasıl yapılır ve Azure portalını kullanarak ilişkili hizmet uç noktası İlkesi öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 09/18/2018
ms.author: kumud
ms.openlocfilehash: b1d2d04e74828323166810d93c52a60671bf71e8
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64710924"
---
# <a name="create-change-or-delete-service-endpoint-policy-using-the-azure-portal"></a>Oluşturma, değiştirme veya Azure portalını kullanarak hizmet uç noktası İlkesi Sil

Hizmet uç noktası ilkeleri, hizmet uç noktaları belirli Azure kaynaklarına, sanal ağ trafiğini filtreleme sağlar. Hizmet uç noktası ilkeleri ile ilgili bilgi sahibi değilseniz bkz [hizmet uç noktası ilkelerine genel bakış](virtual-network-service-endpoint-policies-overview.md) daha fazla bilgi için.

 Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir hizmet uç noktası ilkesi oluşturma
> * Hizmet uç noktası ilke tanımı oluşturma
> * Bir alt ağ ile sanal ağ oluşturma
> * Bir alt ağ için bir hizmet uç noktası İlkesi ilişkilendirin

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-service-endpoint-policy"></a>Bir hizmet uç noktası ilkesi oluşturma

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. Arama bölmesinde "hizmet uç noktası İlkesi" yazın ve **hizmet uç noktası İlkesi (Önizleme)** seçip **Oluştur**.
3. Girin veya seçin, aşağıdaki bilgileri **temelleri** 

   - Abonelik: İlke için aboneliğinizi seçin.    
   - Kaynak grubu: **Yeni oluştur**’u seçin ve *myResourceGroup* değerini girin.     
   - Ad: myEndpointPolicy
   - Konum: Batı Orta ABD     
 
   ![Hizmet uç noktası İlkesi temellerini oluşturma](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-startpane.PNG)
   
4. Girin veya seçin, aşağıdaki bilgileri **ilke tanımları**

   - Tıklayın **+ bir kaynak ekleyin**, girin veya seçin, aşağıdaki bilgileri, kalan ayarlar için varsayılan değerleri kabul edin ve tıklayın **Ekle**.  
   - Kapsam: Seçin **tek hesap** veya **Abonelikteki tüm hesapları** veya **kaynak grubundaki tüm hesapları**.    
   - Abonelik: Depolama hesabı için aboneliğinizi seçin. İlke ve depolama hesapları farklı Aboneliklerde olabilir.   
   - Kaynak grubu: Kaynak grubunuzu seçin. Kapsam "Tüm hesapları kaynak grubunda" veya "Tek hesap" olarak ayarlandıysa gereklidir.  
   - Kaynak: mystorageaccountportal    
   - Tıklayın **+ bir kaynak ekleyin** diğer kaynakları eklemeye devam etmek için.
   
   ![Hizmet uç noktası ilke tanımları oluşturma](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-policydefinitionspane.PNG)
   
5. İsteğe bağlı: Girin veya seçin, aşağıdaki bilgileri **etiketleri**:
   
   - Anahtar: İlke için anahtarınızı seçin. Örn: Bölüm     
   - Değer: Değer çifti için anahtarı girin. Örn: Finans

6. Seçin **gözden geçir + Oluştur**. ' A tıklayın ve bilgi doğrulamak **Oluştur**. Daha ayrıntılı düzenlemeler yapmak için tıklatın **önceki**. 

   ![Hizmet uç noktası ilke son doğrulamaları oluşturma](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-create-finalcreatereview.PNG)
  
 
## <a name="view-endpoint-policies"></a>Uç noktası ilkeleri görüntüle 

1. İçinde *tüm hizmetleri* portalda kutusunda, yazmaya başlayın *hizmet uç noktası ilkesine*. Seçin **hizmet uç noktası Policies(Preview)**.
2. Altında **abonelikleri**, aşağıdaki resimde gösterildiği gibi abonelik ve kaynak grubu seçin

   ![İlke Göster](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-viewpolicies.PNG)
       
3. İlkeyi seçin ve tıklayın **ilke tanımları** görüntülemek veya daha fazla ilke tanımları eklemek için.

   ![İlke tanımları Göster](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-viewpolicy-adddefinitions.PNG)

4. Seçin **ilişkili alt ağlar** alt ağlarını görüntülemek için ilke ilişkilendirilir. Bir alt ağ, ilke ilişkilendirmek için "Sanal ağ aynı bölgede Git" tıklayın.

   ![İlişkili alt ağlarını Göster](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-view-associatedsubnets.PNG)
 
## <a name="associate-a-policy-to-a-subnet"></a>Bir alt ağa bir ilkesi ilişkilendirin

>[!WARNING] 
> İlke ilişkilendirmeden önce alt ağ için seçili hizmet erişilen tüm kaynakların ilkeye eklendiğinden emin olun. İlke ilişkilendirildikten sonra yalnızca listelenen kaynaklara erişim ilkesi, hizmeti için uç nokta bölgeler için izin verilir. 

Bir alt ağ için bir ilke ilişkilendirmeden önce bir sanal ağ ve alt ağ oluşturmanız gerekir ve ardından alt ağa ilke ilişkilendirebilirsiniz:

1. Azure portalının sol üst köşesinde bulunan **+ Kaynak oluştur** seçeneğini belirleyin.
2. **Ağ**’ı ve sonra **Sanal ağ**’ı seçin.
3. **Sanal ağ oluştur** altında aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve sonra **Oluştur**’u seçin:
   - Ad: myVirtualNetwork      
   - Adres alanı: 10.0.0.0/16      
   - Abonelik: Aboneliğinizi seçin. İlke, sanal ağ ile aynı abonelikte olmalıdır     
   - Kaynak grubu: Seçin **var olanı kullan** seçip *myResourceGroup*     
   - Konum: Batı Orta ABD     
   - Alt ağ adı: özel     
   - Adres aralığı: 10.0.0.0/24
     
4. Portalın üst kısmındaki **Kaynak, hizmet ve belgeleri arayın** kutusuna *myVirtualNetwork* yazmaya başlayın. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin.
5. Altında **ayarları**seçin **alt ağlar** seçip **özel**.
6. Aşağıdaki resimde gösterildiği gibi seçin **hizmet uç noktalarını**seçin **Microsoft.Storage**seçin **hizmet uç noktası ilkesine**seçin  **myEndpointPolicy**ve ardından **Kaydet**:

   ![İlkeyi ilişkilendirme](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-associatepolicies.PNG)

>[!WARNING] 
>Ağ güvenlik grupları (Nsg'ler) göre bu alt ağdan diğer bölgelerde hizmeti kaynaklarına erişimi izin verilir. Yalnızca endpoint bölgeleri için erişimi kısıtlamak için Nsg'ler yalnızca hizmet trafiği uç noktası bölgelerde sınırlayın. Hizmet etiketleriyle bölge başına Nsg'ler oluşturma hakkında daha fazla bilgi için bkz. [NSG Azure hizmet etiketleri.](manage-network-security-group.md?toc=%2fcreate-a-security-rule%2f.json)

Aşağıdaki örnekte, yalnızca Azure ile depolama kaynaklarını WestCentralUS, WestUS2, düşük öncelikli bir kural olarak bir "Reddetme tüm" kuralı erişmek için NSG sınırlıdır.

![Tüm NSG Reddet](./media/virtual-network-service-endpoint-policies-portal/virtual-network-endpoint-policies-nsg-rules.PNG)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir hizmet uç noktası İlkesi oluşturduğunuz ve bir alt ağ ile ilişkilendirilmiş. Hizmet uç noktası ilkeleri hakkında daha fazla bilgi için bkz: [hizmet uç noktası ilkelerine genel bakış.](virtual-network-service-endpoint-policies-overview.md)

