---
title: Azure portalını kullanarak Azure ön kapısı bir web uygulaması güvenlik duvarı ilkesi oluştur
titlesuffix: Azure web application firewall
description: Azure portalını kullanarak bir web uygulaması Güvenlik Duvarı (WAF) ilkesi oluşturmayı öğrenin.
services: frontdoor
documentationcenter: na
author: KumudD
manager: twooley
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud;tyao
ms.openlocfilehash: 15a80dac0e0601480e22ad960f2827f3d8f290c0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66479079"
---
# <a name="create-a-waf-policy-for-azure-front-door-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure ön kapısı WAF ilkesi oluşturma

Bu makalede, temel bir Azure web uygulaması Güvenlik Duvarı (WAF) ilkesi oluşturma ve ön uç bir konağa adresindeki Azure ön kapısı uygulamak açıklar.

## <a name="prerequisites"></a>Önkoşullar

Açıklanan yönergeleri izleyerek bir ön kapısı profili oluşturma [hızlı başlangıç: Ön kapısı profil oluşturma](quickstart-create-front-door.md). 

## <a name="create-a-waf-policy"></a>Bir WAF ilkesi oluşturma

İlk olarak portalını kullanarak yönetilen varsayılan kuralı ayarlayın (DRS) ile temel bir WAF ilkesi oluşturun. 

1. Ekranın sol üst tarafında seçin **kaynak Oluştur**> arama **WAF**> seçin **Web uygulaması Güvenlik Duvarı (Önizleme)** > seçin  **Oluşturma**.
2. İçinde **Temelleri** sekmesinde **WAF ilkesi oluşturma** sayfasında, girin veya aşağıdaki bilgileri seçin, geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir +Oluştur**:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik            |Ön kapısı aboneliğinizin adı seçin.|
    | Kaynak grubu          |Ön kapısı kaynak grubunuzun adı seçin.|
    | İlke adı             |WAF ilkeniz için benzersiz bir ad girin.|

   ![Bir WAF ilkesi oluşturma](./media/waf-front-door-create-portal/basic.png)

3. İçinde **ilişkilendirme** sekmesinde **WAF ilkesi oluşturma** sayfasında **frontend ana bilgisayar Ekle**, aşağıdaki ayarları girin ve ardından **Ekle**:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Ön kapısı              | Ön kapısı profil adınızı seçin.|
    | Frontend ana bilgisayar           | Ön kapısı ana bilgisayarınızın adını seçin ve ardından **Ekle**.|
    
    > [!NOTE]
    > Frontend ana bilgisayar bir WAF ilkesiyle ilişkilendirilmiş ise gri olarak gösterilir. Frontend ana bilgisayar ilişkili ilkeyi silmeniz ve ardından yeni bir WAF ilke ön uç konağı yeniden ilişkilendirin.
1. Seçin **gözden geçir + Oluştur**, ardından **Oluştur**.

## <a name="configure-waf-rules-optional"></a>(İsteğe bağlı) WAF kurallarını yapılandırma

### <a name="change-mode"></a>Modu Değiştir

Bir WAF ilkesi oluşturduğunuzda, varsayılan WAF tarafından ilke bulunduğu **algılama** modu. İçinde **algılama** modu, WAF, tüm istekleri engellemez, bunun yerine, WAF kurallarını eşleşen istekler WAF günlüklerine kaydedilir.
WAF nasıl çalıştığını görmek için mod ayarlarından değiştirebilirsiniz **algılama** için **önleme**. İçinde **önleme** modu, varsayılan kuralı ayarlayın (de DRS) tanımlanan eşleştirme kuralları engellenmediğinden ve WAF günlüklerine oturum ister.

 ![Değişiklik WAF ilke modu](./media/waf-front-door-create-portal/policy.png)

### <a name="custom-rules"></a>Özel kurallar

Seçerek özel bir kural oluşturabilirsiniz **Ekle özel kural** altında **özel kurallar** bölümü. Bu, özel kural yapılandırma sayfasında çalıştırır. İstek sorgu dizesini içeriyorsa engellemek için özel bir kural yapılandırma örneği aşağıdadır **blockme**.

![Değişiklik WAF ilke modu](./media/waf-front-door-create-portal/customquerystring2.png)

### <a name="default-rule-set-drs"></a>Varsayılan kural kümesi (DRS)

Azure tarafından yönetilen varsayılan kural kümesi, varsayılan olarak etkindir. Bir kural grubu içindeki tek bir kuralı devre dışı bırakmak için bu kural grubu, select kuralların genişletin **onay kutusunu** kural sayısı ve select önünde **devre dışı** yukarıdaki sekmesinde. Kural içinde bireysel kuralları ayarlamak, kural numarası önünde onay kutusunu işaretleyin ve ardından Eylemler türleri değiştirmek için **değiştirme eylemi** yukarıdaki sekmesi.

 ![Değişiklik WAF kural kümesi](./media/waf-front-door-create-portal/managed2.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure web uygulaması güvenlik duvarı](waf-overview.md).
- Daha fazla bilgi edinin [Azure ön kapısı](front-door-overview.md).
