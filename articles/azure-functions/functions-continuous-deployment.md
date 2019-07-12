---
title: Azure işlevleri için sürekli dağıtım | Microsoft Docs
description: Azure App Service'e sürekli dağıtımı özelliklerinin, işlevlerinizin yayımlamak için kullanın.
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
ms.openlocfilehash: dd1605aa2f5fc9e0b4bc790bae2a1c20cb3ea048
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594554"
---
# <a name="continuous-deployment-for-azure-functions"></a>Azure İşlevleri için sürekli dağıtım

Azure işlevleri kullanarak kodunuzu sürekli olarak dağıtmak için kullanabileceğiniz [kaynak denetimi tümleştirmesi](functions-deployment-technologies.md#source-control). Kaynak denetimi tümleştirmesi içinde Azure'a dağıtılacak kod güncelleştirmesi tetikleyen bir iş akışı sağlar. Azure işlevleri'ne yeni, gözden geçirerek başlayın [Azure işlevlerine genel bakış](functions-overview.md).

Sürekli dağıtım, birden çok tümleştirme burada projeleri için iyi bir seçenek ve sık sık katkıda. Sürekli dağıtım kullandığınızda, ekiplerin kolayca işbirliği sağlar, kodunuz için bir tek gerçeklik kaynağı korur. Aşağıdaki kaynak kodu konumlardan Azure işlevleri'nde sürekli dağıtım yapılandırabilirsiniz:

* [Azure depoları](https://azure.microsoft.com/services/devops/repos/)
* [GitHub](https://github.com)
* [Bitbucket](https://bitbucket.org/)

İşlev uygulaması Azure işlevleri için dağıtım birimidir. Bir işlev uygulamasında tüm işlevleri aynı anda dağıtılır. Sürekli dağıtım etkinleştirdikten sonra Azure portalında işlev kodunu erişimi yapılandırılmış *salt okunur* gerçeklik kaynağı başka bir yerde olarak ayarlandığından.

## <a name="requirements-for-continuous-deployment"></a>Sürekli dağıtım için gereksinimleri

Başarılı olması sürekli dağıtım için dizin yapısını beklediği Azure işlevleri temel klasör yapısı ile uyumlu olması gerekir.

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="credentials"></a>Sürekli dağıtım ayarlama

Var olan bir işlev uygulaması için sürekli dağıtımı yapılandırmak için aşağıdaki adımları tamamlayın. Bir GitHub deposu ile tümleştirme adımları gösterir, ancak Azure depoları veya diğer kaynak kodu depoları için benzer adımları uygulayın.

1. İşlev uygulamanıza [Azure portalında](https://portal.azure.com)seçin **Platform özellikleri** > **Dağıtım Merkezi**.

    ![Dağıtım Merkezi'ni açın](./media/functions-continuous-deployment/platform-features.png)

2. İçinde **Dağıtım Merkezi**seçin **GitHub**ve ardından **Authorize**. GitHub zaten yetki verdiğiniz, seçin **devam**. 

    ![Azure App Service dağıtım merkezi](./media/functions-continuous-deployment/github.png)

3. Github'da seçin **yetkilendirmek AzureAppService** düğmesi. 

    ![Azure App Service'ı Yetkilendir](./media/functions-continuous-deployment/authorize.png)
    
    İçinde **Dağıtım Merkezi** Azure portalında **devam**.

4. Aşağıdaki yapı sağlayıcılardan birini seçin:

    * **App Service derleme hizmeti**: Bir derleme ihtiyacınız kalmadığında, veya genel derleme gerekiyorsa en iyi.
    * **Azure işlem hatları (Önizleme)** : Derleme üzerinde daha fazla denetime olduğunda en iyisidir. Bu sağlayıcı şu anda Önizleme aşamasındadır.

    ![Bir oluşturma sağlayıcısını seçin](./media/functions-continuous-deployment/build.png)

5. Belirtilen kaynak denetim seçeneği belirli bilgileri yapılandırın. GitHub için girin veya seçmeniz gerekir değerlerini **kuruluş**, **depo**, ve **dal**. Değerleri, kodunuzun konumu temel alır. Ardından, **devam**.

    ![GitHub'ı yapılandırma](./media/functions-continuous-deployment/github-specifics.png)

6. Tüm ayrıntılarını gözden geçirin ve ardından **son** dağıtım yapılandırmayı tamamlamak için.

    ![Özet](./media/functions-continuous-deployment/summary.png)

İşlem tamamlandığında, tüm belirtilen kaynak koddan uygulamanıza dağıtılır. Bu noktada, dağıtım kaynağı değişiklikleri Azure işlev uygulamanızda bu değişiklikleri, bir dağıtım tetikleyin.

## <a name="deployment-scenarios"></a>Dağıtım senaryoları

<a name="existing"></a>

### <a name="move-existing-functions-to-continuous-deployment"></a>Mevcut işlevleri için sürekli dağıtım Taşı

İşlevleri önceden yazılmış [Azure portalında](https://portal.azure.com) ve sürekli dağıtım için geçiş yapmadan önce uygulamanızın içeriğini indirmek istediğiniz Git **genel bakış** işlev uygulamanızın sekmesi. Seçin **uygulama içeriği karşıdan** düğmesi.

![Uygulama içeriğini indir](./media/functions-continuous-deployment/download.png)

> [!NOTE]
> Sürekli tümleştirmeyi yapılandırdıktan sonra kaynak dosyalarınızda işlevler portalına artık düzenleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
