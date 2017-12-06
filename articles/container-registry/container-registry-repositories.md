---
title: "Azure kapsayıcı kayıt defteri depoları"
description: "Azure kapsayıcı kayıt defteri depoları için Docker görüntüleri kullanma"
services: container-registry
author: cristy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: 3321dd1d8bbd1b8232c26491edd8c374df16b813
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-repositories"></a>Azure kapsayıcı kayıt defteri depoları

Azure kapsayıcı kayıt defteri, kapsayıcı görüntüleri depolarına olanak sağlar. Depoları görüntüleri depolayarak Yalıtılmış ortamlarda görüntü (veya görüntüleri sürümü) grupları olabilir. Kayıt defterine görüntüleri bastığınızda bu depoları belirtebilirsiniz.


## <a name="prerequisites"></a>Ön koşullar
* **Azure kapsayıcısı kayıt defteri** -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalını](container-registry-get-started-portal.md) veya [Azure CLI 2.0](container-registry-get-started-azure-cli.md)’ı kullanın.
* **Docker CLI** - Yerel bilgisayarınızı bir Docker konağı olarak ayarlamak ve Docker CLI komutlarına erişmek için [Docker Engine](https://docs.docker.com/engine/installation/)’i yükleyin.
* **Bir görüntü çekme** - görüntüyü ortak Docker hub'a kayıt defterinden çekme etiketlemek ve kayıt defterine itme. Nasıl hakkında yönergeler için anında iletme ve çekme görüntüleri bkz [Azure özel kayıt defteri itme Docker görüntüye](container-registry-get-started-docker-cli.md).


## <a name="viewing-repositories-in-the-portal"></a>Portalda depoları görüntüleme

Kapsayıcı kaydınız görüntüleri gönderilen sonra Azure portalında görüntüleri barındırma depoları listesini görebilirsiniz.

' Ndaki adımları izlediyseniz [Azure özel kayıt defteri anında Docker görüntüye](container-registry-get-started-docker-cli.md) makale, kapsayıcı kayıt defterinizde Nginx görüntü şimdi olmalıdır. Yönergeleri bir parçası olarak, görüntü için bir ad belirttiğinizden. Aşağıdaki örnekte, "örneklerini" depoya NGinx görüntü komutu iter:

```
docker push myregistry.azurecr.io/samples/nginx
```
 Azure Container Kayıt Defteri, çok düzeyli depo ad alanlarını destekler. Bu özellik, belirli bir uygulamayla veya uygulama koleksiyonuyla ilişkili görüntü koleksiyonlarını belirli geliştirme veya işlem ekipleri halinde gruplandırmanıza imkan tanır. Daha fazla bilgi için kapsayıcı kayıt defterleri depoları hakkında bkz [özel Docker kapsayıcısı kayıt defterleri azure'da](container-registry-intro.md).

Kapsayıcı kayıt defteri depoları görüntülemek için:

1. Azure portalında oturum açma
2. Üzerinde **Azure kapsayıcı kayıt defteri** dikey penceresinde, incelemek istediğiniz kayıt defteri seçin
3. Kayıt defteri dikey penceresinde tıklayın **depoları** tüm depoları ve resimlerinin listesini görmek için
4. (İsteğe bağlı) Etiketleri görmek için belirli bir görüntü seçin

![Portalda depoları](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a>Sonraki adımlar
Temel bilgileri de öğrendiğinize göre artık kayıt defterinizi kullanmaya başlamaya hazırsınız demektir! Örneğin, bir [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) kümesine kapsayıcı görüntüleri dağıtmaya başlayın.
