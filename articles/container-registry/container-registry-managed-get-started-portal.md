---
title: "Özel Docker kayıt defteri oluşturma - Azure portalı | Microsoft Docs"
description: "Azure portalı ile özel Docker kapsayıcısı kayıt defterleri oluşturmaya ve yönetmeye başlayın"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: 560aee42b0c5a61c37c594d7937f833ab7183d49
ms.contentlocale: tr-tr
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-portal"></a>Azure portalını kullanarak yönetilen kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Bu kılavuzda, Azure portalını kullanarak yönetilen bir Azure Container Registry örneği oluşturma hakkındaki ayrıntılar yer alır.

Yönetilen Azure kapsayıcı kayıt defterleri önizleme aşamasındadır ve tüm bölgelerde kullanılamaz.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

http://portal.azure.com sayfasından Azure portalda oturum açın.

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

1. Azure portalının sol üst köşesinde bulunan **Yeni** düğmesine tıklayın.

2. Markette **Azure kapsayıcı kayıt defteri** ifadesini arayın ve seçin.

3. **Oluştur**’a tıklayın; ACR oluşturma dikey penceresi açılır.

    ![Container kayıt defteri ayarları](./media/container-registry-get-started-portal/managed-container-registry-settings.png)

4. **Azure Kapsayıcı Kayıt Defteri** dikey penceresinde aşağıdaki bilgileri girin. İşiniz bittiğinde **Oluştur**’a tıklayın.

    a. **Kayıt defteri adı**: Oluşturduğunuz kayıt defterinize özel olan, genel olarak benzersiz bir üst düzey etki alanı adı. Bu örnekte kayıt defterinin adı *myAzureContainerRegistry1* olarak verilmiştir, ama bunu kendi bulduğunuz bir adla değiştirin. Ad yalnızca harf ve sayı içerebilir.

    b. **Kaynak grubu**: Mevcut bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md#resource-groups) seçin ya da yeni bir grup için ad yazın.

    c. **Konum**: Hizmetin [kullanılabilir](https://azure.microsoft.com/regions/services/) olduğu bir Azure veri merkezi bölgesi seçin, örneğin **Orta Güney ABD**.

    d. **Yönetici kullanıcı**: İsterseniz bir yönetici kullanıcıya kayıt defterine erişim izni verin. Kayıt defterini oluşturduktan sonra bu ayarı değiştirebilirsiniz.

    e. **Yönetilen kayıt defteri kullan**: ACR’nin kayıt defteri depolamasını otomatik olarak yönetmesi, web kancaları ve AAD kimlik doğrulaması kullanması için evet’i seçin.

    f. **Fiyatlandırma Katmanı**: Fiyatlandırma katmanını seçin; daha fazla bilgi için buradaki ACR fiyatlandırma bölümüne bakın.

## <a name="log-in-to-acr-instance"></a>ACR örneğinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. 

Bunu yapmak için Azure CLI 2.0’ı kullanın. İlk olarak, gerekirse [az login](/cli/azure/#login) komutunu kullanıp Azure’da oturum açın. 

```azurecli
az login
```

Ardından Azure Container Registry’de oturum açmak için [az acr login](/cli/azure/acr#login) komutunu kullanın.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

## <a name="use-azure-container-registry"></a>Azure Container Registry’yi kullanma

### <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Bir depodaki görüntüleri ve etiketleri sorgulamak için `az acr` CLI komutlarını kullanın.

> [!NOTE]
> Container Kayıt Defteri şu anda görüntü ve etiket sorgulamak için `docker search` komutunu desteklememektedir.

### <a name="list-repositories"></a>Depoları listeleme

Aşağıdaki örnekte, bir kayıt defterindeki depolar JSON (JavaScript Nesne Gösterimi) biçiminde listelenmiştir:

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Etiketleri listeleme

Aşağıdaki örnekte, **samples/nginx** deposundaki etiketler JSON biçiminde listelenmiştir:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure portalını kullanarak bir yönetilen Azure Container Registry örneği oluşturdunuz.

> [!div class="nextstepaction"]
> [Docker CLI’yı kullanarak ilk görüntünüzü itme](container-registry-get-started-docker-cli.md)
