---
title: "Azure yönetim gruplarıyla - Azure kaynaklarınızı düzenleme | Microsoft Docs"
description: "Yönetim grupları ve bunları nasıl kullanacağınızı öğrenin."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: rithorn
ms.openlocfilehash: 18541c68b02ae1b59ae4a6a85122dff614c9978c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Kaynaklarınızı Azure Yönetim grupları ile düzenleme 

Birden çok aboneliğiniz varsa, bunları "Yönetim grupları" olarak adlandırılan kapsayıcılarına düzenleyebilirsiniz erişim, ilke, maliyetleri ve uyumluluk aboneliklerinizi yönetmenize yardımcı olmak için. Örnek olarak, hangi kaynak türlerinin oluşturulabilir bu sınırı bir yönetim grubuna ilkelerini uygulayabilirsiniz.

> [!Note]
> Bu özellik şu anda bir özel önizlemede değil. [Burada oturum](https://aka.ms/MGPreviewSignup) kaydınızı Önizleme katılmak için.   
 


Yönetim grupları bir hiyerarşiye düzenlenebilir. Gösterilen yapı bulunabilir. bir yönetim grubu hiyerarşisi örnek gösterimidir:


![Hiyerarşi ağacı](media/billing-enterprise-mgmt-groups/tree.png)



## <a name="how-management-groups-are-related-to-your-azure-enterprise-enrollment"></a>Yönetim grupları, Azure işletme kaydı için nasıl ilişkilendirilir

Giriş, Yönetim grupları bir geçiş, daha büyük gezisine adımdır [Enterprise portal](https://ea.azure.com) özelliklerini [Azure portal](https://portal.azure.com).

Yönetim grubu yapısı Enterprise portal içinde tanımlanan oluşturulur. Kayıt, bölümler ve hesapları oluşan tüm hiyerarşiye karşılık gelen yönetim gruplarına eşlenir. 

İşte geçerli EA yapısı yönetim grubu hiyerarşisine nasıl eşleştirir. 

![Hiyerarşi ağacı](media/billing-enterprise-mgmt-groups/tree2.png)

Aşağıdaki tabloda, Enterprise portal kullanıcılardan yönetim grubu hiyerarşisine nasıl eşlendiğini gösterir.

|    EA rolü                                       |    Eşlenen yönetim grubu düğümünü rolü    |    Yönetim grubu düğümü üzerindeki izinleri                                                          |
|--------------------------------------------------|--------------------------------------------------|----------------------------------------------------------------------------------------------------|
|    EA yönetici                              |    Kaynak İlkesi katkıda bulunan                   |    Maliyetlerini görüntüleyin, kaynak İlkesi ve görünüm hiyerarşi adresindeki ve kayıt düğümü altındaki yönetin    |
|    Salt okunur modda EA yönetici            |    Faturalama okuyucusu                                |    Maliyetleri okuyabilir ve adresindeki ve kayıt düğümü altındaki hiyerarşisini görüntüleme                              |
|    Bölüm Yöneticisi                      |    Faturalama okuyucusu                                |    Maliyetleri okuyabilir ve hiyerarşi görüntülemek ve departman düğümü altında                                 |
|    Bölüm Yöneticisi'nde salt okunur modda.    |    Faturalama okuyucusu                                |    Maliyetleri okuyabilir ve hiyerarşi görüntülemek ve departman düğümü altında                                 |
|    Hesap sahibi                                 |    Kaynak İlkesi katkıda bulunan                   |    Maliyetlerini görüntüleyin, kaynak İlkesi ve görünüm hiyerarşi adresindeki ve hesap düğümü altındaki yönetin       |




## <a name="view-management-groups-in-the-azure-portal"></a>Azure portalındaki Yönetim grupları görüntüle

Bir kayıt, bölüme veya Önizleme bir hesaptaki görüntülemek için Azure Portalı'na Hoş Geldiniz e-posta bağlantısı ile oturum açın.   

![Hiyerarşisi](media/billing-enterprise-mgmt-groups/hierarchy.png)

### <a name="viewing-costs"></a>Maliyetlerini görüntüleme 
Yönetim grupları ayrıntı ekranlarda geçerli ay tarih maliyetleri bakın. Bu maliyetleri kullanıma dayalı ve ön ödemeli tutarların, fazlalığı, dahil edilen miktarlar, değişiklikler ve vergileri hesabı değil. Kaydınızı için karşılık gelen yönetim grubu için maliyetleri bölümünde kalan taahhüt gösterilir.  

![Kayıt ayrıntıları](media/billing-enterprise-mgmt-groups/enrollment.png)

 "Ayrı olarak faturalandırılır maliyetleri" Market, fazlalığı ve kaydınızı 's taahhüt karşı geçmemektedir diğer maliyetlerin gibi ayrı değişiklikleri aylık toplamı ' dir.  Maliyet çözümleme hakkında daha fazla bilgi için bkz: [Enterprise portal](https://ea.azure.com). 

### <a name="enabling-access-to-costs"></a>Maliyetleri erişimini etkinleştirme
Maliyetleri görmüyorsanız, bkz: bizim [sorun giderme Kurumsal Maliyet görünümleri](https://aka.ms/enableazurecosts) Yardım için belge.  

### <a name="delays-between-the-enterprise-portal-and-azure-portal"></a>Azure portal ve Enterprise Portal'da arasındaki gecikme 
* Önizleme sırasında Azure portalını gösterilen tutarlar Enterprise portal değerlerde karşılaştırıldığında gecikebilir. Bu sorun geçicidir ve üzerinde çalışılan.
* Enterprise portal güncelleştirilmiş ayarlarında güncelleştirmeleri Azure portalında yansıtılması birkaç dakikalık bir gecikme vardır. 

## <a name="management-groups-have-a-relationship-with-your-directory"></a>Yönetim grupları dizininize ile ilişkisi   
Abonelikler gibi yönetim grupları da Azure AD ile bir güven ilişkisine sahip. Bir yönetim grubu hiyerarşisi kullanıcıların kimliklerini doğrulamak için tek bir dizin güvenir. Bir yönetim grubu hiyerarşisi ile ilişkili tüm yöneticileri aynı dizine ait olmalıdır. 

Kayıt hiyerarşinizi yönetim gruplarına eşlenmiş şekilde bir güven ilişkisi tek bir dizin oluşturulur. Mümkünse, kayıt ait kullanıcı hesapları ile ilişkili olan bir dizini seçilir. Bazı durumlarda, yeni bir dizin oluşturulur ve tüm mevcut kayıt kullanıcılar bu dizine davet. Kayıt ait abonelikleri ile ilişkili dizinler etkilenmez. Bu nedenle, hiyerarşi aboneliklerden farklı bir dizinde oluşturulmasına. [Daha fazla bilgi edinin](billing-enterprise-mgmt-grp-find.md) bu işlem hiyerarşi ve onun abonelikler arasında gezinme deneyimini nasıl etkilediğini hakkında.

## <a name="administering-your-management-groups"></a>Yönetim gruplarınızı yönetme
Azure portalındaki Yönetim grupları önizlemede ve ilk bu sürümde salt okunurdur. Herhangi bir güncelleştirme yapmak için Enterprise portal gidin. Güncelleştirmelerinizi Azure Portalı'nda otomatik olarak yansıtılır. Düzenlemeler ve eklemeler yapma hakkında Yardım için enterprise portal dahilinde belgelerine bakın.   

## <a name="policy-management"></a>İlke yönetimi
Resource Manager kaynaklarınızı yönetmek üzere özelleştirilmiş ilkeler oluşturmanıza olanak tanır Yönetim grupları ilkeler hiyerarşideki herhangi bir düzeyde atanabilir ve bu ilkeleri kaynakları devralır.  [Daha fazla bilgi](https://go.microsoft.com/fwlink/?linkid=858942)

> [!Note]
> İlke dizinlerde zorlanmaz. 


