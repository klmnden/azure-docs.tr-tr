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
ms.openlocfilehash: 13190189074b24b2d28cd3ce46cf8571f3e1e1d1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Trafik Yöneticisi'ni kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma

Coğrafi trafik yönlendirme yöntemini doğrudan trafiğe burada istekleri kaynaklanan coğrafi konum temelinde belirli Uç noktalara sağlar. Bu öğretici yönlendirme bu yöntemle bir trafik Yöneticisi profili oluşturun ve belirli coğrafi trafiği almak için uç noktalarını yapılandırma gösterilmektedir.

## <a name="create-a-traffic-manager-profile"></a>Trafik Yöneticisi profili oluştur

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz.
2. Hub menüsünde **yeni** > **ağ** > **tümünü görmek**ve ardından **trafik Yöneticisi profili**açmak için **oluşturma trafik Yöneticisi profili** dikey.
3. Üzerinde **oluşturma trafik Yöneticisi profili** dikey penceresinde:
    1. Profiliniz için bir ad sağlayın. Bu ad trafficmanager.net içinde benzersiz olması gereken bölge ve DNS adıyla sonuçlanır <profilename>, Traffic Manager profilinizin erişmek için kullanılacak olan trafficmanager.net.
    2. Seçin **Geographic** yönlendirme yöntemi.
    3. Bu profili altında oluşturmak istediğiniz aboneliği seçin.
    4. Varolan bir kaynak grubunu kullanın veya bu profili altında yerleştirmek için yeni bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmayı seçerseniz, kullanın **kaynak grubu konumu** kaynak grubu konumunu belirtmek için açılır. Bu ayar, kaynak grubu konumunu gösterir ve genel olarak dağıtılacak trafik Yöneticisi profili üzerinde hiçbir etkisi olmaz.
    5. Tıklattıktan sonra **oluşturma**, Traffic Manager profilinizin oluşturulur ve genel olarak dağıtıldı.

![Traffic Manager profili oluşturma](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Uç noktaları ekleme

1. Portal'ın arama çubuğunda oluşturduğunuz trafik Yöneticisi profili adını arayın ve gösterildiğinde sonuca tıklayın.
2. Gidin **ayarları** -> **uç noktaları** trafik Yöneticisi dikey penceresinde.
3. Tıklatın **Ekle** göstermek için **uç nokta Ekle** dikey.
3. İçinde **uç noktaları** dikey penceresinde tıklatın **Ekle** ve **uç nokta ekleme** görüntülenir, dikey tamamlamak aşağıdaki gibi:
4. Seçin **türü** eklemekte olduğunuz endpoint türüne bağlı olarak. Üretimde kullanılan, yönlendirme coğrafi profilleri birden fazla uç alt profiliyle içeren iç içe geçmiş uç nokta türleri kullanarak öneririz. Daha fazla ayrıntı için bkz: [coğrafi trafik yönlendirme yöntemleri hakkında sık sorulan sorular](traffic-manager-FAQs.md).
5. Bu uç noktayı tanımak istediğiniz bir **Ad** belirtin.
6. Bu dikey bazı alanları eklediğiniz uç nokta türüne bağlıdır:
    1. Azure uç ekliyorsanız seçin **hedef kaynak türünün** ve **hedef** trafiği yönlendirmek istediğiniz kaynak göre
    2. Ekliyorsanız bir **dış** uç noktasını sağlamak **tam etki alanı adı (FQDN)** uç noktanız için.
    3. Ekliyorsanız bir **iç içe endpoint**seçin **hedef kaynak** kullanın ve belirtmek istediğiniz alt profilini karşılık gelen **en düşük alt uç noktaları sayısı**.
7. Coğrafi eşleme bölümünde bölgelerin trafiğinin Bu uç noktaya gönderilmesini istediğiniz eklemek için aşağı açılır kullanın. En az bir bölge eklemeniz gerekir ve birden çok bölgeye eşlenmiş olabilir.
8. Bu, bu profili altında eklemek istediğiniz tüm uç noktaları için yineleyin

![Trafik Yöneticisi uç noktası ekleme](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Trafik Yöneticisi profili kullan
1.  Portal'ın arama çubuğunda arama **trafik Yöneticisi profili** sonuçlarında trafik Yöneticisi profili tıklayın ve önceki bölümde oluşturduğunuz adı, görüntülenen.
2. İçinde **trafik Yöneticisi profili** dikey penceresinde tıklatın **genel bakış**.
3. **Trafik Yöneticisi profili** dikey penceresinde, yeni oluşturulan Traffic Manager profilinizin DNS adını görüntüler. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) sağ uç noktası olarak yönlendirilen için yönlendirme türü tarafından belirlenir.  Coğrafi yönlendirme, söz konusu olduğunda trafik Yöneticisi gelen isteğin kaynak IP adresinde arar ve kendisinden kaynaklanan bölgeyi belirler. Bu bölge için bir uç nokta eşlendiğinde trafik için orada yönlendirilir. Bu bölge için bir uç nokta eşlenmemiş, trafik Yöneticisi NODATA sorgu yanıtı döndürür.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinmek [Geographic trafik yönlendirme yöntemini](traffic-manager-routing-methods.md#geographic).
- Bilgi edinmek için nasıl [test trafik Yöneticisi Ayarları](traffic-manager-testing-settings.md).
