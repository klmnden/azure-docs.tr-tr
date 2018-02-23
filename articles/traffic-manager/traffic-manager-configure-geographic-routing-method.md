---
title: "Azure trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma | Microsoft Docs"
description: "Bu makalede Azure trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma açıklanmaktadır"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 7b49e2a4eef5a966f1ef2aa283a3089bb5b73734
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma

Coğrafi trafik yönlendirme yöntemini doğrudan trafiğe burada istekleri kaynaklanan coğrafi konum temelinde belirli Uç noktalara sağlar. Bu öğretici yönlendirme bu yöntemle bir trafik Yöneticisi profili oluşturun ve belirli coğrafi trafiği almak için uç noktalarını yapılandırma gösterilmektedir.

## <a name="create-a-traffic-manager-profile"></a>Trafik Yöneticisi profili oluştur

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz.
2. Tıklatın **kaynak oluşturma** > **ağ** > **trafik Yöneticisi profili** > **oluşturma**.
4. İçinde **oluşturma trafik Yöneticisi profili**:
    1. Profiliniz için bir ad sağlayın. Bu ad trafficmanager.net bölge içinde benzersiz olması gerekir. Traffic Manager profilinizin erişmek için DNS adı kullanın. <profilename>. trafficmanager.net.
    2. Seçin **Geographic** yönlendirme yöntemi.
    3. Bu profili altında oluşturmak istediğiniz aboneliği seçin.
    4. Varolan bir kaynak grubunu kullanın veya bu profili altında yerleştirmek için yeni bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmayı seçerseniz, kullanın **kaynak grubu konumu** kaynak grubu konumunu belirtmek için açılır. Bu ayar, kaynak grubu konumunu gösterir ve genel olarak dağıtıldı trafik Yöneticisi profili üzerinde hiçbir etkisi olmaz.
    5. Tıklattıktan sonra **oluşturma**, Traffic Manager profilinizin oluşturulur ve genel olarak dağıtıldı.

![Traffic Manager profili oluşturma](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Uç noktaları ekleme

1. Portal'ın arama çubuğunda oluşturulan trafik Yöneticisi profili adını arayın ve gösterildiğinde sonuca tıklayın.
2. Gidin **ayarları** -> **uç noktaları** trafik Yöneticisi'nde.
3. Tıklatın **Ekle** göstermek için **uç nokta Ekle**.
3. Tıklatın **Ekle** ve **uç nokta ekleme** görüntülenen, aşağıdaki gibi tamamlayın:
4. Seçin **türü** eklemekte olduğunuz endpoint türüne bağlı olarak. Üretimde kullanılan, yönlendirme coğrafi profilleri birden fazla uç alt profiliyle içeren iç içe geçmiş uç nokta türleri kullanarak öneririz. Daha fazla ayrıntı için bkz: [coğrafi trafik yönlendirme yöntemleri hakkında sık sorulan sorular](traffic-manager-FAQs.md).
5. Bu uç noktayı tanımak istediğiniz bir **Ad** belirtin.
6. Bu sayfadaki bazı alanlar eklediğiniz uç nokta türüne bağlıdır:
    1. Azure uç ekliyorsanız seçin **hedef kaynak türünün** ve **hedef** trafiği yönlendirmek istediğiniz kaynak göre
    2. Ekliyorsanız bir **dış** uç noktasını sağlamak **tam etki alanı adı (FQDN)** uç noktanız için.
    3. Ekliyorsanız bir **iç içe endpoint**seçin **hedef kaynak** kullanın ve belirtmek istediğiniz alt profilini karşılık gelen **en düşük alt uç noktaları sayısı**.
7. Coğrafi eşleme bölümünde bölgelerin trafiğinin Bu uç noktaya gönderilmesini istediğiniz eklemek için aşağı açılır kullanın. En az bir bölge eklemeniz gerekir ve birden çok bölgeye eşlenmiş olabilir.
8. Bu, bu profili altında eklemek istediğiniz tüm uç noktaları için yineleyin

![Trafik Yöneticisi uç noktası ekleme](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Trafik Yöneticisi profili kullan
1.  Portal'ın arama çubuğunda arama **trafik Yöneticisi profili** sonuçlarında trafik Yöneticisi profili tıklayın ve önceki bölümde oluşturduğunuz adı, görüntülenen.
2. Tıklatın **genel bakış**.
3. **Trafik Yöneticisi profili** yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) sağ uç noktası olarak yönlendirilen için yönlendirme türü tarafından belirlenir.  Coğrafi yönlendirme, söz konusu olduğunda trafik Yöneticisi gelen isteğin kaynak IP adresinde arar ve kendisinden kaynaklanan bölgeyi belirler. Bu bölge için bir uç nokta eşlendiğinde trafik için orada yönlendirilir. Bu bölge için bir uç nokta eşlenmemiş, trafik Yöneticisi NODATA sorgu yanıtı döndürür.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [Geographic trafik yönlendirme yöntemini](traffic-manager-routing-methods.md#geographic).
- Bilgi edinmek için nasıl [test trafik Yöneticisi Ayarları](traffic-manager-testing-settings.md).
