---
title: "Docker görüntüsünü özel Azure kayıt defterine itme | Microsoft Docs"
description: "Docker CLI’yı kullanarak Azure’da özel bir kapsayıcı kayıt defterine Docker görüntüleri itme ve kapsayıcıdan görüntü çekme"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 64fbe43f-fdde-4c17-a39a-d04f2d6d90a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 0433e22dc7722ef9c8edfaf949dbd9a9d8645e67
ms.openlocfilehash: e778c09991da73cd5064532119d92bff36f0a00e
ms.lasthandoff: 03/02/2017

---
# <a name="push-your-first-image-to-a-private-docker-container-registry-using-the-docker-cli"></a>Docker CLI’yı kullanarak özel bir Dockler kapsayıcı kayıt defterine ilk görüntünüzü itme
Azure kapsayıcısı kayıt defteri, [Docker Hub](https://hub.docker.com/)’ın genel Docker görüntülerini depolama yöntemine benzer şekilde özel [Docker](http://hub.docker.com) kapsayıcı görüntülerini depolar ve yönetir. Kapsayıcı kayıt defterinizde [oturum açma](https://docs.docker.com/engine/reference/commandline/login/), [itme](https://docs.docker.com/engine/reference/commandline/push/), [çekme](https://docs.docker.com/engine/reference/commandline/pull/) ve diğer işlemleri gerçekleştirmek için [Docker Komut Satırı Arabirimi](https://docs.docker.com/engine/reference/commandline/cli/)’ni (Docker CLI) kullanırsınız. 

Arka plan ve kavramlar için, bkz: [genel bakış](container-registry-intro.md)


> [!NOTE]
> Container Kayıt Defteri şu an önizleme aşamasındadır.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
* **Azure kapsayıcısı kayıt defteri** -Azure aboneliğinizde bir kapsayıcı kayıt defteri oluşturun. Örneğin, [Azure portalını](container-registry-get-started-portal.md) veya [Azure CLI 2.0](container-registry-get-started-azure-cli.md)’ı kullanın.
* **Docker CLI** - Yerel bilgisayarınızı bir Docker konağı olarak ayarlamak ve Docker CLI komutlarına erişmek için [Docker Engine](https://docs.docker.com/engine/installation/)’i yükleyin.

## <a name="log-in-to-a-registry"></a>Kayıt defterinde oturum açma
Kapsayıcı kayıt defterinizde [kayıt defteri kimlik bilgileriniz](container-registry-authentication.md) ile oturum açmak için `docker login` komutunu çalıştırın.

Aşağıdaki örnekte, bir Azure Active Directory [hizmet sorumlusunun](../active-directory/active-directory-application-objects.md) kimliği ve parolası geçirilmiştir. Örneğin, bir otomasyon senaryosu için kayıt defterinize bir hizmet sorumlusu atamış olabilirsiniz. 

```
docker login myregistry-contoso.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

> [!TIP]
> Tam kayıt defteri adını (tamamı küçük harf) belirttiğinizden emin olun. Bu örnekte bu değer `myregistry-contoso.azurecr.io`’dur.

## <a name="steps-to-pull-and-push-an-image"></a>Görüntü çekme ve itme adımları
Aşağıdaki örnekte, genel Docker Hub kayıt defterinden bir Nginx görüntüsü indirilir, bu Nginx özel Azure kapsayıcısı kayıt defteriniz için etiketlenir, kayıt defterinize itilir ve yeniden çekilir.

**1. Nginx için Docker resmi görüntüsünü çekme**

Önce genel Nginx görüntüsünü yerel bilgisayarınıza çekin.

```
docker pull nginx
```
**2. Nginx kapsayıcısını başlatma**

Aşağıdaki komut yerel Nginx kapsayıcısını etkileşimli olarak (Nginx’ten çıkış görebilmeniz için) ve 8080 bağlantı noktasında dinleyecek şekilde başlatır. Durdurulduğunda çalışmakta olan kapsayıcıyı kaldırır.

```
docker run -it --rm -p 8080:80 nginx
```

Çalışmakta olan kapsayıcıyı görmek için [http://localhost:8080](http://localhost:8080) konumuna gidin. Aşağıdakine benzeyen bir ekran görürsünüz.

![Yerel bilgisayarda Nginx](./media/container-registry-get-started-docker-cli/nginx.png)

Çalışmakta olan kapsayıcıyı durdurmak için [CTRL]+[C] tuşlarına basın.

**3. Kayıt defterinizde görüntünün bir diğer adını oluşturma**

Aşağıdaki komut, görüntünün kayıt defterinize yönelik tam ada sahip bir diğer adını oluşturur. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için `samples` ad alanını belirtir.

```
docker tag nginx myregistry-exp.azurecr.io/samples/nginx
```  

**4. Görüntüyü kayıt defterinize itme**

```
docker push myregistry-contoso.azurecr.io/samples/nginx
``` 

**5. Görüntüyü kayıt defterinizden çekme**

```
docker pull myregistry-contoso.azurecr.io/samples/nginx
``` 

**6. Kayıt defterinizden Nginx kapsayıcısını başlatma**

```
docker run -it --rm -p 8080:80 myregistry-exp.azurecr.io/samples/nginx
```

Çalışmakta olan kapsayıcıyı görmek için [http://localhost:8080](http://localhost:8080) konumuna gidin.

Çalışmakta olan kapsayıcıyı durdurmak için [CTRL]+[C] tuşlarına basın.

**6. Görüntüyü kaldırma**

```
docker rmi myregistry-contoso.azurecr.io/samples/nginx
```



## <a name="next-steps"></a>Sonraki adımlar
Temel bilgileri de öğrendiğinize göre artık kayıt defterinizi kullanmaya başlamaya hazırsınız demektir! Örneğin, bir [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/) kümesine kapsayıcı görüntüleri dağıtmaya başlayın.




