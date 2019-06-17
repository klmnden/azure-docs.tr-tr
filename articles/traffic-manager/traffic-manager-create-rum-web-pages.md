---
title: Gerçek kullanıcı ölçümleri için web sayfaları ile Azure Traffic Manager | Microsoft Docs
description: Traffic Manager gerçek kullanıcı ölçümleri göndermek için web sayfalarınıza ayarlayın
services: traffic-manager
documentationcenter: traffic-manager
author: asudbring
manager: twooley
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: allensu
ms.custom: ''
ms.openlocfilehash: 2d044457df80f16a6e8073e7f3253a611f74d8a8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071227"
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Web pages kullanılarak Azure Traffic Manager gerçek kullanıcı ölçümleri gönderme

Gerçek kullanıcı ölçümleri (RUM) anahtarı alma ve web sayfası için oluşturulan kod ekleme, Traffic Manager gerçek kullanıcı ölçümleri göndermek için web sayfalarınıza yapılandırabilirsiniz.

## <a name="obtain-a-real-user-measurements-key"></a>Bir gerçek kullanıcı ölçümleri anahtarı edinme

Olması ve Traffic Manager, istemci uygulamasından gönderme ölçüleri adlı benzersiz bir dize kullanarak hizmet tarafından tanımlanan **gerçek kullanıcı ölçümleri (RUM) anahtar**. Azure portalı, bir REST API kullanarak ÇALIŞTIRMA bir anahtar alabilirsiniz veya PowerShell veya Azure CLI kullanarak.

Azure portalını kullanarak RUM anahtarı almak için:
1. Azure portalında bir tarayıcıdan oturum açın. Zaten bir hesabınız yoksa, ücretsiz bir aylık deneme için kaydolabilirsiniz.
2. Portalın arama çubuğunda, değiştirmek istediğiniz Traffic Manager profil adı için arama yapın ve sonra sonuçları Traffic Manager profili seçin, görüntülenen.
3. Traffic Manager profili dikey penceresinde **gerçek kullanıcı ölçümleri** altında **ayarları**.
4. Tıklayın **anahtar oluştur** RUM yeni bir anahtar oluşturmak için.
 
   ![Gerçek kullanıcı ölçümleri anahtarı oluşturma](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Şekil 1: Gerçek kullanıcı ölçümleri anahtarı oluşturma**

5. Dikey pencere artık RUM oluşturulan anahtarı ve HTML sayfanıza eklenmesi gereken JavaScript kod parçacığını görüntüler.
 
    ![Gerçek kullanıcı ölçümleri anahtarı için JavaScript kodu](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Şekil 2: Gerçek kullanıcı ölçümleri anahtarı ve ölçüm JavaScript'i**
 
6. Tıklayın **kopyalama** düğmesine tıklayarak JavaScript kodu kopyalayın. 

>[!IMPORTANT]
> Oluşturulan JavaScript için gerçek kullanıcı ölçümleri özelliği düzgün şekilde çalışabilmesi için kullanın. Bu betik veya gerçek kullanıcı ölçümleri tarafından kullanılan betikler değişiklikleri beklenmeyen davranışa neden olabilir.

## <a name="embed-the-code-to-an-html-web-page"></a>Bir HTML web sayfası için kod ekleme

ÇALIŞTIRMA anahtarı aldıktan sonra sonraki adım, bu kopyalanan JavaScript ekleme, son kullanıcılarınızın ziyaret ettiği bir HTML sayfasına sağlamaktır. HTML düzenleme birçok şekilde yapılabilir ve farklı araçlar ve iş akışlarını kullanma. Bu örnekte, bu komut dosyası eklemek için bir HTML sayfası gösterilmektedir. HTML kaynak yönetimi akışınızı uyarlamak için bu kılavuzu kullanabilirsiniz.

1.  HTML sayfası bir metin düzenleyicisinde açın.
2.  HTML gövde bölümüne önceki adımda kopyaladığınız JavaScript kodu yapıştırın (kopyalanan kod satırı 8 & 9; Şekil 3).
 
    ![Web sayfasına ekleme JavaScript kodu için gerçek kullanıcı ölçümleri](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **Şekil 3: Katıştırılmış gerçek kullanıcı ölçümleri JavaScript ile basit bir HTML**

3.  İnternet'e bağlı bir Web sunucusu üzerinde konak ve HTML dosyasını kaydedin. 
4. Bu sayfa bir web tarayıcısı üzerinde işlenen, sonraki açışınızda, başvurulan JavaScript indirilir ve ölçüm ve raporlama işlemlerinin betiği çalıştırır.


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [gerçek kullanıcı ölçümleri](traffic-manager-rum-overview.md)
- Bilgi [Traffic Manager nasıl çalışır?](traffic-manager-overview.md)
- Daha fazla bilgi edinin [trafik yönlendirme yöntemlerini](traffic-manager-routing-methods.md) Traffic Manager tarafından desteklenen
- Bilgi edinmek için nasıl [Traffic Manager profili oluşturma](traffic-manager-create-profile.md)

