---
title: "Gerçek kullanıcı ölçümler için Azure Traffic Manager web sayfalarıyla | Microsoft Docs"
description: "Trafik Yöneticisi için gerçek kullanıcı ölçümleri göndermek için web sayfalarınıza ayarlayın"
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 7f4088cf4470b1f9fa22c4ec83a9f92657032734
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Web sayfalarını kullanarak Azure trafik Yöneticisi için gerçek kullanıcı ölçümleri gönderme

>[!NOTE]
>Trafik Yöneticisi'nde gerçek kullanıcı ölçümleri özellik genel önizlemede ve genel kullanılabilirlik özellikleri sürüm gibi aynı düzeyde kullanılabilirlik ve güvenilirlik olmayabilir. Özellik desteklenmiyor, yetenekleri kısıtlı ve tüm Azure konumlarda kullanılamayabilir. Kullanılabilirlik ve bu özellik durumunu en güncel bildirimleri için denetleme [Azure trafik Yöneticisi'ni güncelleştirir](https://azure.microsoft.com/updates/?product=traffic-manager) sayfası.

Gerçek kullanıcı ölçümleri (RUM) anahtarı alma ve web sayfası için oluşturulan kodu katıştırma trafik Yöneticisi için gerçek kullanıcı ölçümleri göndermek için web sayfalarınıza yapılandırabilirsiniz.

## <a name="obtain-a-real-user-measurements-key"></a>Gerçek kullanıcı ölçümleri anahtarı edinme

Alın ve trafik Yöneticisi için istemci uygulamasından gönderebilmek ölçümleri adlı benzersiz bir dize kullanarak hizmeti tarafından tanımlanan **gerçek kullanıcı ölçümleri (RUM) anahtarı**. Azure portal, bir REST API kullanarak RUM bir anahtarı elde edebilirsiniz veya PowerShell veya Azure CLI kullanarak.

Azure portalını kullanarak RUM anahtarı edinmek için:
1. Bir tarayıcı üzerinden Azure portalında oturum açın. Zaten bir hesabınız yoksa, ücretsiz bir aylık deneme için kaydolabilirsiniz.
2. Değiştirmek istediğiniz trafik Yöneticisi profili adı portal'ın arama çubuğunda aramak ve sonuçları trafik Yöneticisi profili ardından, görüntülenen.
3. Trafik Yöneticisi profili dikey tıklayın **gerçek kullanıcı ölçümleri** altında **ayarları**.
4. Tıklatın **oluşturmak anahtar** yeni RUM anahtarı oluşturmak için.
 
  ![Gerçek kullanıcı ölçümleri anahtarı oluşturma](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Şekil 1: Gerçek kullanıcı ölçümleri anahtar oluşturma**

5. Dikey şimdi RUM oluşturulan anahtarı ve HTML sayfanıza gömülü olması gereken bir JavaScript kod parçacığı görüntüler.
 
    ![Gerçek kullanıcı ölçümleri anahtarı için JavaScript kodu](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Şekil 2: Gerçek kullanıcı ölçümleri anahtarı ve ölçüm JavaScript**
 
6.  Tıklatın **kopyalama** düğmesi JavaScript kodunu kopyalayın. 

>[!IMPORTANT]
> Düzgün çalışması için oluşturulan JavaScript gerçek kullanıcı ölçümleri özelliği için kullanın. Bu komut dosyası veya gerçek kullanıcı ölçümleri tarafından kullanılan komut değişiklikleri beklenmeyen davranışlara neden olabilir.

## <a name="embed-the-code-to-an-html-web-page"></a>Bir HTML web sayfası için kod ekleme

RUM anahtarı aldıktan sonra bu kopyalanan JavaScript son kullanıcılarınızın ziyaret HTML sayfasına katıştırmak için sonraki adım olacaktır. HTML düzenleme birçok yolla yapılabilir ve farklı araçlar ve iş akışlarını kullanma. Bu örnekte, bu komut dosyası eklemek için bir HTML sayfasında güncelleştirme gösterilmiştir. HTML kaynak yönetimi iş akışınıza uyarlamak için bu kılavuzu kullanabilirsiniz.

1.  Bir metin düzenleyicide HTML sayfasını açın
2.  HTML gövde bölümüne önceki adımda kopyaladığınız JavaScript kodu yapıştırın (kopyalanan kod satırı 8 &9;bkz. Şekil 3).
 
    ![JavaScript kodu için gerçek kullanıcı ölçümleri web sayfasına ekleme](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **Şekil 3: Basit HTML katıştırılmış gerçek kullanıcı ölçümleri JavaScript ile**

3.  HTML dosyasını kaydedin
4. Bu sayfa bir web tarayıcısı işlenmeden sonraki sefer başvurulan JavaScript indirilir ve ölçüm ve operations raporlama betiği çalıştırır.


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinmek [gerçek kullanıcı ölçümleri](traffic-manager-rum-overview.md)
- Bilgi [trafik Yöneticisi nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinmek [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [bir Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

