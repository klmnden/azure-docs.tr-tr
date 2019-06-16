---
title: Azure işlevleri için sürekli dağıtım | Microsoft Docs
description: Azure App Service'in sürekli dağıtım özellikleri, işlevlerinizin yayımlamak için kullanın.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/25/2016
ms.author: glenga
ms.openlocfilehash: d4d2f24a0a7b1f01627ed2cea4a5732ca0e001c9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67068355"
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure İşlevleri için sürekli dağıtım

Azure işlevleri, kodunuzu ile sürekli olarak dağıtmanızı sağlar [kaynak denetimi tümleştirmesi](functions-deployment-technologies.md#source-control). Bu bir iş akışı tetikleyici kod güncelleştirmeleri nerede sağlar Azure'a dağıtılacak. Azure işlevleri'ne yeniyseniz, kullanmaya başlama [Azure işlevlerine genel bakış](functions-overview.md).

Sürekli dağıtım, birden çok burada tümleştiriyorsanız projeler için mükemmel bir seçenektir ve sık sık katkıda. Ayrıca, tek bir işlev kodu doğru kaynağı korumak sağlar. Aşağıdaki kaynak kodu konumlardan Azure işlevleri'nde sürekli dağıtım yapılandırabilirsiniz:

* [Azure depoları](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

İşlev uygulaması Azure işlevleri için dağıtım birimidir. Başka bir deyişle, bir işlev uygulamasında tüm işlevleri aynı anda dağıtılır. Sürekli dağıtım etkinleştirildikten sonra Azure portalında işlev kodunu erişimi yapılandırılmış *salt okunur*, gerçekte kaynağı başka bir yerde olacak şekilde ayarlanmış olduğundan.

## <a name="requirements-for-continuous-deployment"></a>Sürekli dağıtım için gereksinimleri

Başarılı olması sürekli dağıtım için dizin yapısını uyumlu olması gerekir, Azure işlevleri bekliyor aşağıdaki temel klasör yapısı:

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Sürekli dağıtım ayarlama

Var olan bir işlev uygulaması için sürekli dağıtımını yapılandırmak için bu yordamı kullanın. Bir GitHub deposu ile tümleştirme adımları gösterir, ancak Azure depoları veya diğer kaynak kodu depoları için benzer adımları uygulayın.

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com)seçin **Platform özellikleri** > **Dağıtım Merkezi**.

    ![Dağıtım Merkezi açma](./media/functions-continuous-deployment/platform-features.png)

2. Üzerinde **Dağıtım Merkezi**seçin **GitHub** için **kaynak denetimi** > **Authorize**.

    ![Dağıtım Merkezi](./media/functions-continuous-deployment/github.png)

3. Seçin **AzureAppService yetkilendirmek** > **devam**.

    ![Yetkilendirme](./media/functions-continuous-deployment/authorize.png)

4. Aşağıdaki yapı sağlayıcılardan birini seçin:

    * **App Service derleme hizmeti** - en iyi bir yapı ihtiyacınız kalmadığında, veya genel derleme gerekiyorsa.
    * **Azure işlem hatları (Önizleme)** - derleme hakkında daha fazla denetime ihtiyacınız olduğunda en iyi. Bu sağlayıcı şu anda Önizleme aşamasındadır.

    ![Bir oluşturma sağlayıcısını seçme](./media/functions-continuous-deployment/build.png)

5. Belirtilen kaynak denetim seçeneği belirli bilgileri yapılandırın. GitHub için sağlamanız gereken **kuruluş**, **depo**, ve **dal** kodunuzu nerede yaşıyor. Ardından, **devam**.

    ![GitHub'ı yapılandırma](./media/functions-continuous-deployment/github-specifics.png)

6. Son olarak, tüm ayrıntılarını gözden geçirin ve seçin **son** dağıtım yapılandırmayı tamamlamak için.

    ![Özet](./media/functions-continuous-deployment/summary.png)

İşlem tamamlandığında, tüm belirtilen kaynak koddan uygulamanıza dağıtılır. Bu noktada, dağıtım kaynağı değişiklikleri Azure işlev uygulamanızda bu değişiklikleri, bir dağıtım tetikleyin.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Mevcut işlevleri için sürekli dağıtım Taşı

İşlevleri önceden yazılmış [Azure portalında](https://portal.azure.com) ve uygulamanızın içeriğini sürekli dağıtım geçmeden önce indirmek istiyorsanız, gitmek **genel bakış** işlevinizin sekmesi Uygulama seçeneğine tıklayıp **uygulama içeriği karşıdan** düğmesi.

![Uygulama içeriği indiriliyor](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Sürekli tümleştirmeyi yapılandırdıktan sonra kaynak dosyalarınızda işlevler portalına artık düzenleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
