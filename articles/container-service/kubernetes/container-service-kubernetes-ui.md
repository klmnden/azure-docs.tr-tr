---
title: Azure Kubernetes küme web kullanıcı Arabirimi ile yönetme
description: Azure kapsayıcı Hizmeti'nde Kubernetes web kullanıcı Arabirimi kullanma
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 0680551d3a87c942574a4eac70fa380cc1e9b5d9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="using-the-kubernetes-web-ui-with-azure-container-service"></a>Azure kapsayıcı hizmeti ile Kubernetes web kullanıcı arabirimini kullanma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu kılavuz, sahibi olduğunuzu varsayar [Azure kapsayıcı hizmeti kullanarak Kubernetes küme oluşturulan](container-service-kubernetes-walkthrough.md).


Ayrıca, Azure CLI 2.0 sahibi olduğunuzu varsayar ve `kubectl` araçları yüklü.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```console
$ az --version
```

Sahip değilseniz `az` yüklü, aracı yönergeler vardır [burada](https://github.com/azure/azure-cli#installation).

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```console
$ kubectl version
```

Sahip değilseniz `kubectl` yüklü çalıştırabilirsiniz:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Genel Bakış

### <a name="connect-to-the-web-ui"></a>Web kullanıcı Arabirimi Bağlan
Kubernetes web kullanıcı arabirimini çalıştırarak başlatabilirsiniz:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Bu, yerel makinenize Kubernetes web kullanıcı Arabirimi bağlanma güvenli bir proxy iletişim kurabilecek şekilde yapılandırılmış bir web tarayıcısı açmanız gerekir.

### <a name="create-and-expose-a-service"></a>Oluşturma ve bir hizmet kullanıma sunma
1. Kubernetes web kullanıcı Arabirimi,'ı **oluşturma** üst sağ penceresinde düğmesini.

    ![Kubernetes kullanıcı Arabirimi oluşturma](./media/container-service-kubernetes-ui/create.png)

    Uygulamanızı oluşturma başlayabileceğiniz bir iletişim kutusu açılır.

2. Adını verin `hello-nginx`. Kullanım [ `nginx` Docker kapsayıcıdan](https://hub.docker.com/_/nginx/) ve bu web Hizmeti'nin, üç çoğaltmaları dağıtın.

    ![Kubernetes Pod Oluştur iletişim kutusu](./media/container-service-kubernetes-ui/nginx.png)

3. Altında **hizmet**seçin **dış** ve 80 numaralı bağlantı noktasını girin.

    Bu ayar yük-üç çoğaltmaları trafiği dengeler.

    ![Kubernetes hizmeti oluşturma iletişim kutusu](./media/container-service-kubernetes-ui/service.png)

4. Tıklatın **dağıtma** Bu kapsayıcılar ve hizmet dağıtmak için.

    ![Kubernetes dağıtma](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Kapsayıcılarınızı görüntüleyin
Tıklattıktan sonra **dağıtma**, dağıttığı gibi UI hizmetiniz bir görünümünü gösterir:

![Kubernetes durumu](./media/container-service-kubernetes-ui/status.png)

UI sol taraftaki altında bir daire her Kubernetes nesnesinde durumunu görebilirsiniz **pod'ları**. Kısmen dolu daire ise, ardından nesne hala dağıtıyor. Bir nesne tam olarak dağıtıldığında, yeşil bir onay işareti görüntüler:

![Dağıtılan Kubernetes](./media/container-service-kubernetes-ui/deployed.png)

Her şeyi çalışmaya başladıktan sonra çalışan web hizmeti hakkındaki ayrıntıları görmek için pod'ları birini tıklatın.

![Kubernetes pod'ları](./media/container-service-kubernetes-ui/pods.png)

İçinde **pod'ları** görünümü, bu kapsayıcıları tarafından kullanılan CPU ve bellek kaynakları yanı sıra pod kapsayıcılarında hakkında bilgileri görebilirsiniz:

![Kubernetes kaynakları](./media/container-service-kubernetes-ui/resources.png)

Kaynakları görmüyorsanız, yaymak izleme verilerini birkaç dakika beklemeniz gerekebilir.

Günlükleri, kapsayıcı için görmek için tıklatın **görüntülemek günlükleri**.

![Kubernetes günlükleri](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Hizmetinizi görüntüleme
Kapsayıcılarınızı çalıştırmanın yanı sıra, bir dış Kubernetes UI oluşturdu `Service` kümenizi kapsayıcılarında trafiği getirmek üzere bir yük dengeleyici sağlar.

Sol gezinti bölmesinde **Hizmetleri** (olması gerektiğini tek) tüm hizmetleri görüntülemek için.

![Kubernetes Hizmetleri](./media/container-service-kubernetes-ui/service-deployed.png)

Bu görünümde hizmetinize ayrılan dış uç noktası (IP adresi) görmeniz gerekir.
Bu IP adresi tıklarsanız, yük dengeleyicinin ardında çalışan, Nginx kapsayıcısı görmeniz gerekir.

![nginx görünümü](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Hizmetinizi yeniden boyutlandırma
Kullanıcı Arabiriminde nesnelerinizi görüntülemeye ek olarak, düzenleme ve Kubernetes API nesneleri güncelleştirme.

Önce tıklatın **dağıtımları** hizmetiniz için dağıtım görmek için sol gezinti bölmesindeki.

Bu görünümde olduktan sonra çoğaltma kümesinde tıklayın ve ardından **Düzenle** üst gezinti çubuğunda:

![Kubernetes Düzenle](./media/container-service-kubernetes-ui/edit.png)

Düzen `spec.replicas` olmasını alan `2`, tıklatıp **güncelleştirme**.

Bu iki, pod'ları birini silerek bırakmak için yineleme sayısı neden olur.

 

