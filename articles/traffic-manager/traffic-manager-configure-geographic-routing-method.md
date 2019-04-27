---
title: Azure Traffic Manager'ı kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma
description: Bu makalede, Azure Traffic Manager kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma açıklanmaktadır
services: traffic-manager
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 47cc56aac7d3e0147ef8577aac19776c6cacf7a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60884164"
---
# <a name="configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>Traffic Manager'ı kullanarak coğrafi trafik yönlendirme yöntemini yapılandırma

Coğrafi trafik yönlendirme yöntemini trafik burada istekleri kaynaklı coğrafi konum temelinde belirli Uç noktalara yönlendirmek sağlar. Bu öğreticide bu yönlendirme yöntemi ile bir Traffic Manager profili oluşturma ve belirli coğrafyalar trafiği almak için uç noktalarını yapılandırma gösterilmektedir.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

1. Bir tarayıcıdan [Azure portalında](https://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz.
2. **Kaynak oluştur** > **Ağ** > **Traffic Manager profili** > **Oluştur** seçeneğine tıklayın.
4. İçinde **Traffic Manager profili oluştur**:
    1. Profiliniz için bir ad sağlayın. Bu adın trafficmanager.net bölgesinde benzersiz olması gerekir. Traffic Manager profilinize erişmek için DNS adı kullanın. `<profilename>.trafficmanager.net`.
    2. Seçin **Geographic** yönlendirme yöntemi.
    3. Bu profilin oluşturmak istediğiniz aboneliği seçin.
    4. Mevcut bir kaynak grubunu kullanın veya bu profilin yerleştirileceği yeni bir kaynak grubu oluşturun. Yeni bir kaynak grubu oluşturmayı seçerseniz, kullanın **kaynak grubu konumu** kaynak grubunun konumunu belirtmek için açılır. Bu ayar, kaynak grubunun konumunu ifade eder ve küresel olarak dağıtılan Traffic Manager profilini etkilemez.
    5. Tıkladıktan sonra **Oluştur**, Traffic Manager profilinizin oluşturulur ve Global olarak dağıtılır.

![Traffic Manager profili oluşturma](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Uç noktaları ekleyin

1. Portalın arama çubuğunda oluşturulan Traffic Manager profili adını arayın ve gösterildiğinde sonuca tıklayın.
2. Gidin **ayarları** -> **uç noktaları** Traffic Manager'da.
3. Tıklayın **Ekle** gösterilecek **uç nokta Ekle**.
3. Tıklayın **Ekle** ve **uç noktası ekleme** görüntülenen, aşağıdaki gibi tamamlayın:
4. Seçin **türü** eklemekte olduğunuz uç nokta türüne bağlı olarak. Üretim ortamında kullanılan coğrafi yönlendirme profilleri için kesin bir alt profili ile birden fazla uç nokta içeren iç içe uç nokta türleri kullanmanızı öneririz. Daha fazla ayrıntı için [coğrafi trafik yönlendirme yöntemleri hakkında sık sorulan sorular](traffic-manager-FAQs.md).
5. Bu uç noktayı tanımak istediğiniz bir **Ad** belirtin.
6. Bu sayfadaki bazı alanlar, eklemekte olduğunuz uç nokta türüne bağlıdır:
    1. Bir Azure uç noktası ekliyorsanız seçin **hedef kaynak türü** ve **hedef** trafiği yönlendirmek için istediğiniz kaynağa göre
    2. Ekliyorsanız bir **dış** uç noktasını sağlamak **tam etki alanı adı (FQDN)** uç noktanız için.
    3. Ekliyorsanız bir **iç içe uç nokta**seçin **hedef kaynak** belirtin ve kullanmak istediğiniz alt profiline karşılık gelen **en düşük alt uç noktaları sayısı**.
7. Coğrafi eşleme bölümünde, bölge, trafiğin Bu uç noktaya gönderilmesini istediğiniz eklemek için aşağı açılan kullanın. En az bir bölge eklemelisiniz ve eşleşen birden çok bölgede olabilir.
8. Bu profilin eklemek istediğiniz tüm uç noktalar için bu işlemi yineleyin

![Traffic Manager uç noktası ekleme](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Traffic Manager profilini kullanın
1.  Portalın arama çubuğunda arama **Traffic Manager profili** sonuçlarında traffic manager profili tıklayın ve önceki bölümde oluşturduğunuz adı, görüntülenen.
2. **Genel Bakış**'a tıklayın.
3. **Traffic Manager profili** penceresinde yeni oluşturduğunuz Traffic Manager profilinin DNS adı görüntülenir. Tüm istemciler tarafından bu kullanılabilir (örneğin, bir web tarayıcısı kullanarak giderek) yönlendirme türüne göre belirlenen doğru uç noktaya yönlendirilir.  Coğrafi yönlendirme, söz konusu olduğunda Traffic Manager, gelen isteğin kaynak IP adresinde arar ve kendisinden kaynaklanan bölgeyi belirler. Bu bölge için bir uç nokta eşlendiğinde, trafik için orada yönlendirilir. Bu bölge için bir uç nokta eşlenmemiş Traffic Manager NODATA sorgu yanıtı döndürür.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [coğrafi trafik yönlendirme yöntemini](traffic-manager-routing-methods.md#geographic).
- Bilgi edinmek için nasıl [Traffic Manager ayarlarını sınama](traffic-manager-testing-settings.md).
