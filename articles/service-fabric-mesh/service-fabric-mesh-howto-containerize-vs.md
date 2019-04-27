---
title: Service Fabric Mesh için mevcut bir .NET uygulamasını kapsayıcılı hale getirme | Microsoft Docs
description: Mevcut bir .NET uygulaması için Kafes desteği eklendi
services: service-fabric-mesh
keywords: Service fabric kafes kapsayıcılı hale getirme
author: dkkapur
ms.author: dekapur
ms.date: 11/08/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: cb4e327e1c8c0a653cb94233f568b4847494c439
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60419467"
---
# <a name="containerize-an-existing-net-app-for-service-fabric-mesh"></a>Service Fabric Mesh için mevcut bir .NET uygulamasını kapsayıcılı hale getirme

Bu makalede, var olan bir .NET uygulaması için Service Fabric Mesh kapsayıcı düzenleme desteği ekleme gösterir.

Visual Studio 2017'de tam .NET framework için ASP.NET ve konsol projeleri kapsayıcı desteği ekleyebilirsiniz.

> [!NOTE]
> .NET **çekirdek** projeleri şu anda desteklenmez.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturabilirsiniz.

* Emin olun [geliştirme ortamınızı ayarlama](service-fabric-mesh-howto-setup-developer-environment-sdk.md). Bu, Service Fabric çalışma zamanını, SDK, Docker, Visual Studio 2017, yükleme ve yerel küme oluşturma içerir.

## <a name="open-an-existing-net-app"></a>Mevcut bir .NET uygulamasını açın.

Kapsayıcı düzenleme desteği eklemek istediğiniz uygulamayı açın.

Bir örnek deneyin istiyorsanız, kullanabileceğiniz [elektronik mağaza](https://github.com/MikkelHegn/ContainersSFLab) kod örneği. Kendi proje için şu adımları uygulayabilir, bu makalenin geri kalanında bu projeye kullanıyoruz varsayar.

Bir kopyasını alın **elektronik mağaza** proje:

```git
git clone https://github.com/MikkelHegn/ContainersSFLab.git
```

Bu, Visual Studio 2017 açık exe'yi **ContainersSFLab\eShopLegacyWebFormsSolution\eShopLegacyWebForms.sln**.

## <a name="add-container-support"></a>Kapsayıcı desteği eklendi
 
Kapsayıcı düzenleme desteği, Service Fabric Mesh araçları kullanarak aşağıdaki gibi mevcut bir ASP.NET veya konsol projeye ekleyin:

Visual Studio Çözüm Gezgini'nde proje adına sağ tıklayın (örnekte **eShopLegacyWebForms**) ve ardından **Ekle** > **kapsayıcı Düzenleyicisi desteği** .
**Kapsayıcı Düzenleyicisi desteği Ekle** iletişim kutusu görüntülenir.

![Visual Studio kapsayıcı orchestrator iletişim Ekle](./media/service-fabric-mesh-howto-containerize-vs/add-container-orchestration-support.png)

Seçin **Service Fabric Mesh** açılır listeden ve ardından **Tamam**.

Aracı'nı, ardından Docker yüklü olduğu, bir Dockerfile projenize ekler ve projeniz için bir docker görüntüsü aşağı çeker doğrular.  
Bir Service Fabric Mesh uygulaması projesi çözümünüze eklenir. İçerdiği, ağ profilleri ve yapılandırma dosyalarını Yayımla. Projenin adı 'sonuna kadar Örneğin, birleştirilmiş uygulama', proje adıyla aynıdır **eShopLegacyWebFormsApplication**. 

Yeni Mesh projede iki görürsünüz klasörleri farkında olmalıdır:
- **Uygulama kaynaklarını** ağı gibi ek ağ kaynakları tanımlayan YAML dosyaları içerir.
- **Hizmet kaynaklarının** dağıttığınızda uygulamanızı nasıl çalışması gerektiğini tanımlayan bir service.yaml dosyası içerir.

Uygulamanız için kapsayıcı düzenleme desteği eklendikten sonra basabilirsiniz **F5** yerel kümenizde Service Fabric Mesh .NET uygulamanızda hata ayıklama. Service Fabric Mesh küme üzerinde çalışan Elektronik Mağaza ASP.NET uygulaması şu şekildedir: 

![Elektronik Mağaza uygulaması](./media/service-fabric-mesh-howto-containerize-vs/eshop-running.png)

Azure Service Fabric Mesh için artık uygulama yayımlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh için bir uygulama yayımlama bakın: [Öğretici-bir Service Fabric Mesh uygulaması dağıtma](service-fabric-mesh-tutorial-deploy-service-fabric-mesh-app.md)