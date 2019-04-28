---
title: Azure API Management ilkeleri ayarlama veya düzenleme nasıl | Microsoft Docs
description: Bu konuda, Azure API Management ilkeleri ayarlama veya düzenleme gösterilmektedir.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cflower
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2018
ms.author: apimpm
ms.openlocfilehash: 3d1847b6001ef8e32f00a4e1cd9728d5ca0662f8
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62097216"
---
# <a name="how-to-set-or-edit-azure-api-management-policies"></a>Azure API Management ilkeleri ayarlama veya düzenleme yapma

İlke tanımı gelen ve giden ifadeler açıklayan bir XML belgesidir. XML tanım penceresinden doğrudan düzenleyebilirsiniz. Ayrıca, önceden yapılandırılmış bir ilkeyle sağlanan listeden ilke pencerenin sağında seçebilirsiniz. Geçerli kapsam için geçerli ifadeler etkin ve vurgulanır. Etkin bir deyim tıklayarak uygun XML tanımı görünümünün imleç konumu ekler. 

İlkeleri hakkında ayrıntılı bilgi için bkz. [Azure API Management ilkeleri](api-management-howto-policies.md).

## <a name="set-or-edit-a-policy"></a>Ayarlayın veya bir ilkeyi düzenleyebilirsiniz

Ayarlamak veya bir ilkeyi düzenlemek için aşağıdaki adımları izleyin:

1. [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.
2. APIM örneğinize göz atın.
3. **API'ler** sekmesine tıklayın.

    ![İlkeyi düzenleme](./media/set-edit-policies/code-editor.png)

4. Daha önce içeri aktardığınız API'lerden birini seçin.
5. **Tasarım** sekmesini seçin.
6. İlkeyi uygulamak istediğiniz bir işlem seçin. Tüm işlemler için ilkeyi uygulamak istiyorsanız seçin **tüm işlemleri**.
7. Seçin **</>** (Kod düzenleyicisinde) simgesine **gelen işlem** veya **giden işlem** bölümü.
8. İstenen ilke kod uygun blok birine yapıştırın.
         
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
 
## <a name="configure-scope"></a>Kapsam yapılandırma

Genel olarak veya bir ürün, API veya işlem kapsamında ilkeleri yapılandırılabilir. Bir ilke yapılandırmaya başlamak için önce ilkenin uygulanacağı kapsamı seçmeniz gerekir.

İlke kapsamları şu sırada değerlendirilir:

1. Genel kapsam
2. Ürün kapsamı
3. API kapsamı
4. İşlem kapsamı

Deyimleri içinde ilkeleri yerleşimini göre değerlendirilir `base` varsa öğe. Genel ilke sahip üst öğe İlkesi ve kullanarak `<base>` öğesi içindeki etkisizdir.

İlkeler ilke düzenleyicisinde geçerli kapsamda görmek için tıklayın **etkin ilke seçilen kapsam için'yeniden hesapla**.

### <a name="global-scope"></a>Genel kapsam

Genel kapsam için yapılandırılmış **tüm API'leri** APIM Örneğinize içinde.

1. Oturum [Azure portalında](https://portal.azure.com/) ve APIM Örneğinize gidin.
2. Tıklayın **tüm API'leri**.

    ![Genel kapsam](./media/api-management-howto-policies/global-scope.png)

3. Bu üçgen simgesine tıklayın.
4. **Kod düzenleyicisi**’ni seçin.
5. Ekleyebilir veya ilkeleri düzenleyebilirsiniz.
6. **Kaydet**’e basın. 

    Değişiklikler hemen API Management ağ geçidi dağıtılır.

### <a name="product-scope"></a>Ürün kapsamı

Ürün kapsamı, seçili ürün için yapılandırılır.

1. Tıklayın **ürünleri**.

    ![Ürün kapsamı](./media/api-management-howto-policies/product-scope.png)

2. İlkeleri uygulamak istediğiniz ürünü seçin.
3. Tıklayın **ilkeleri**.
4. Ekleyebilir veya ilkeleri düzenleyebilirsiniz.
5. **Kaydet**’e basın. 

### <a name="api-scope"></a>API kapsamı

API kapsam için yapılandırılmış **tüm işlemleri** seçili API.

1. Seçin **API** ilkelerini uygulamak istediğiniz.

    ![API kapsamı](./media/api-management-howto-policies/api-scope.png)

2. **Tüm işlemler**’i seçin
3. Bu üçgen simgesine tıklayın.
4. **Kod düzenleyicisi**’ni seçin.
5. Ekleyebilir veya ilkeleri düzenleyebilirsiniz.
6. **Kaydet**’e basın. 

### <a name="operation-scope"></a>İşlem kapsamı 

İşlem kapsamı seçili işlemi için yapılandırılmıştır.

1. Seçin bir **API**.
2. İlkelerini uygulamak istediğiniz işlemi seçin.

    ![İşlem kapsamı](./media/api-management-howto-policies/operation-scope.png)

3. Bu üçgen simgesine tıklayın.
4. **Kod düzenleyicisi**’ni seçin.
5. Ekleyebilir veya ilkeleri düzenleyebilirsiniz.
6. **Kaydet**’e basın. 

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ilgili konulara bakın:

+ [API'leri dönüştürme](transform-api.md)
+ [İlke başvurusu](api-management-policy-reference.md) ilke bildirimlerine ve ayarlarının tam listesi için
+ [İlke örnekleri](policy-samples.md)