---
title: "Nasıl ayarlayacağınız veya Azure API Management ilkeleri düzenleme | Microsoft Docs"
description: "Bu konu, ayarlama veya Azure API yönetimi ilkelerini düzenleme gösterilmektedir."
services: api-management
documentationcenter: 
author: Juliako
manager: cflower
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 409069cbc382610a48139df75f0f64b1682d8ee6
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="how-to-set-or-edit-azure-api-management-policies"></a>Nasıl ayarlayacağınız veya Azure API Management ilkeleri düzenleme

İlke tanımı gelen ve giden ifadeler tanımlayan bir XML dosyasıdır. XML tanımı penceresinden doğrudan düzenlenebilir. Önceden tanımlanmış bir ilke sağlanan listeden İlkesi penceresi sağındaki öğesini de seçebilirsiniz. Geçerli kapsam için geçerli deyimleri etkin ve vurgulanır. Etkin bir deyimi tıklatarak uygun XML tanım görünümünde imleç konumu ekler. 

İlkeleri hakkında ayrıntılı bilgi için bkz: [Azure API Management ilkeleri](api-management-howto-policies.md).

## <a name="set-or-edit-a-policy"></a>Ayarlama veya bir ilkeyi Düzenle

Ayarlamak veya bir ilke düzenlemek için aşağıdaki adımları izleyin:

1. Oturum açmak için Azure portalında [https://portal.azure.com](https://portal.azure.com).
2. APIM örneğine göz atın.
3. Tıklatın **API'leri** sekmesi.
4. Daha önce aldığınız API'leri birini seçin.
5. Seçin **tasarım** sekmesi.
6. İlkeyi uygulamak istediğiniz işlemi seçin. Tüm işlemler için ilkeyi uygulamak istiyorsanız seçin **tüm işlemleri**.
7. Üçgen tıklayın **gelen** veya **giden** Kurşun Kalem.
8. Seçin **Kod düzenleyicisinde** öğesi.

    ![İlkeyi düzenle](./media/set-edit-policies/set-edit-policies01.png)

9. İstenen ilke kodu uygun blokları birine yapıştırın.
         
        <policies>
             <inbound>
                 <base />
             </inbound>
             <backend>
                 <base />
             </backend>
             <outbound>
                 <base />
             </outbound>
             <on-error>
                 <base />
             </on-error>
         </policies>
 
## <a name="configure-scope"></a>Kapsam yapılandırın

İlkeleri, genel olarak veya bir ürün, API veya işlemi kapsamda yapılandırılabilir. Bir ilke yapılandırmaya başlamak için önce ilkenin uygulanacağı kapsam seçmeniz gerekir.

İlke kapsamları aşağıdaki sırayla değerlendirilir:

1. Genel kapsamlı
2. Ürün kapsamı
3. API kapsamı
4. İşlem kapsamı

Deyimleri ilkeleri içinde yerleşimini göre değerlendirilir `base` varsa, öğesi. Genel ilke üst öğeye sahip İlkesi ve kullanarak `<base>` öğesi içindeki etkisi yoktur.

İlkeler ilke düzenleyicisinde geçerli kapsamdaki görmek için tıklatın **yeniden hesapla seçili kapsam için etkin ilke**.

### <a name="global-scope"></a>Genel kapsamlı

Genel kapsam için yapılandırılmış **tüm API'leri** APIM Örneğinizde.

1. Oturum [Azure portal](https://portal.azure.com/) ve APIM örneğine gidin.
2. Tıklatın **tüm API'leri**.

    ![Genel kapsamlı](./media/api-management-howto-policies/global-scope.png)

3. Üçgen simgesine tıklayın.
4. Seçin **Kod düzenleyicisinde**.
5. İlkeleri ekleme veya düzenleme.
6. Tuşuna **kaydetmek**. 

    Değişiklikler hemen API Yönetimi ağ geçidi yayılır.

### <a name="product-scope"></a>Ürün kapsamı

Ürün kapsamı seçilen ürün için yapılandırılır.

1. Tıklatın **ürünleri**.

    ![Ürün kapsamı](./media/api-management-howto-policies/product-scope.png)

2. İlkeleri uygulamak istediğiniz ürün seçin.
3. Tıklatın **ilkeleri**.
4. İlkeleri ekleme veya düzenleme.
5. Tuşuna **kaydetmek**. 

### <a name="api-scope"></a>API kapsamı

API kapsam için yapılandırılmış **tüm işlemleri** seçili API.

1. Seçin **API** ilkelerini uygulamak istiyor.

    ![API kapsamı](./media/api-management-howto-policies/api-scope.png)

2. Seçin **tüm işlemleri**
3. Üçgen simgesine tıklayın.
4. Seçin **Kod düzenleyicisinde**.
5. İlkeleri ekleme veya düzenleme.
6. Tuşuna **kaydetmek**. 

### <a name="operation-scope"></a>İşlem kapsamı 

İşlem kapsam için seçilen işlem yapılandırılır.

1. Seçin bir **API**.
2. İlkelerini uygulamak istediğiniz işlemi seçin.

    ![İşlem kapsamı](./media/api-management-howto-policies/operation-scope.png)

3. Üçgen simgesine tıklayın.
4. Seçin **Kod düzenleyicisinde**.
5. İlkeleri ekleme veya düzenleme.
6. Tuşuna **kaydetmek**. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki konulara bakın:

+ [API dönüştürme](transform-api.md)
+ [Grup İlkesi başvurusu](api-management-policy-reference.md) ilke deyimleri ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)