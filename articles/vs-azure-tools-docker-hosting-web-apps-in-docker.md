---
title: Azure kapsayıcı kayıt defteri (ACR) için bir ASP.NET Docker kapsayıcısı dağıtma | Microsoft Docs
description: Bir kapsayıcı kayıt defterine ASP.NET Core web uygulama dağıtmak için Docker için Visual Studio Araçları kullanmayı öğrenin
services: azure-container-service
documentationcenter: .net
author: mlearned
manager: douge
editor: ''
ms.assetid: e5e81c5e-dd18-4d5a-a24d-a932036e78b9
ms.service: azure-container-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: mlearned
ms.openlocfilehash: 4442c1d763f4ed21a5efeedbe957727254e2a0b8
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34658480"
---
# <a name="deploy-an-aspnet-container-to-a-container-registry-using-visual-studio"></a>Visual Studio kullanarak bir kapsayıcı kayıt defterine bir ASP.NET kapsayıcı dağıtma
## <a name="overview"></a>Genel Bakış
Docker ana uygulamalar ve hizmetler için kullanabileceğiniz bir sanal makineye, bazı şekillerde benzer bir basit kapsayıcı alt yapısıdır.
Bu öğretici, Kapsayıcılı uygulamanızı yayımlamak için Visual Studio kullanarak kılavuzluk bir [Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/en-us/services/container-registry).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/dotnet/?utm_source=acr-publish-doc&utm_medium=docs&utm_campaign=docs) oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için:

* En son sürümünü yüklemek [Visual Studio 2017](https://azure.microsoft.com/en-us/downloads/) "ASP.NET ve web geliştirme" iş yükü ile
* Yükleme [Windows için Docker](https://docs.docker.com/docker-for-windows/install/)

## <a name="1-create-an-aspnet-core-web-app"></a>1. ASP.NET Core web uygulaması oluşturma
Aşağıdaki adımlar Bu öğreticide kullanılan temel bir ASP.NET Core uygulama oluşturmada size yol.

[!INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-publish-your-container-to-azure-container-registry"></a>2. Azure kapsayıcı kayıt defterine, kapsayıcı yayımlama
1. Projenize sağ **Çözüm Gezgini** ve **Yayımla**.
2. Yayımlama hedefi iletişim kutusunda seçin **kapsayıcı kayıt defteri** sekmesi.
3. Seçin **yeni Azure kapsayıcı kayıt defteri** tıklatıp **Yayımla**.
4. İstenen değerlerinizi doldurun **yeni bir Azure kapsayıcı kayıt**.

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **DNS öneki** | Genel olarak benzersiz bir ad | Kapsayıcı kaydınız benzersiz olarak tanıtan adı. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Kapsayıcı kaydınız oluşturulacağı kaynak grubunun adı. Seçin **yeni** yeni bir kaynak grubu oluşturmak için.|
    | **[SKU](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-skus)** | Standart | Hizmet katmanı kapsayıcı kayıt defteri  |
    | **Kayıt defteri konumu** | Yakın bir konum | Bir konumdan seçin bir [bölge](https://azure.microsoft.com/regions/) yakın veya kapsayıcı kaydınız kullanacağı diğer hizmetler yakın. |
    ![Visual Studio'nun Oluştur Azure kapsayıcı kayıt defteri iletişim kutusu][0]
5. **Oluştur**'a tıklayın

Artık kapsayıcı kayıt defterinden herhangi bir ana bilgisayara çalıştırabilen Docker görüntüler, örneğin çekebilir [Azure kapsayıcı örnekleri](./container-instances/container-instances-tutorial-deploy-app.md).

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/vs-acr-provisioning-dialog.png
