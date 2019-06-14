---
title: (KULLANIM DIŞI) Azure Kubernetes kümesi web kullanıcı Arabirimi ile yönetme
description: Azure Container Service'te Kubernetes web kullanıcı arabirimini kullanarak
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: c3a79b2e4fab807613a54d2792f5f5b97570293b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60309778"
---
# <a name="deprecated-using-the-kubernetes-web-ui-with-azure-container-service"></a>(KULLANIM DIŞI) Azure Container Service ile Kubernetes web kullanıcı arabirimini kullanarak

> [!TIP]
> Bu makale, güncelleştirilmiş sürümü kullanan Azure Kubernetes Service için bkz: [Kubernetes web panosuna Azure Kubernetes Service (AKS) erişme](../../aks/kubernetes-dashboard.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu izlenecek yol, sahibi olduğunuzu varsayar [Azure Container Service kullanan bir Kubernetes kümesi oluşturuldu](container-service-kubernetes-walkthrough.md).


Ayrıca, Azure CLI'yı sahibi olduğunuzu varsayar ve `kubectl` araçlarının yüklü.

Varsa, test edebilirsiniz `az` çalıştırarak yüklü aracı:

```console
$ az --version
```

Öğeniz yoksa `az` yüklü aracı yönergeler sunulmaktadır [burada](https://github.com/azure/azure-cli#installation).

Varsa, test edebilirsiniz `kubectl` çalıştırarak yüklü aracı:

```console
$ kubectl version
```

Öğeniz yoksa `kubectl` yüklü çalıştırabilirsiniz:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Genel Bakış

### <a name="connect-to-the-web-ui"></a>Web kullanıcı Arabirimine bağlanma
Kubernetes web kullanıcı arabirimini çalıştırarak başlatabilirsiniz:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Bu, yerel makinenize Kubernetes web kullanıcı Arabirimine bağlanma güvenli bir proxy konuşmak üzere yapılandırılmış bir web tarayıcısı açmanız gerekir.

### <a name="create-and-expose-a-service"></a>Oluşturma ve bir hizmeti göstermesi
1. Kubernetes web kullanıcı Arabirimi,'ı **Oluştur** üst sağ pencerede düğmesi.

    ![Kubernetes Create UI](./media/container-service-kubernetes-ui/create.png)

    Uygulamanızı oluşturmaya başlayabileceğiniz bir iletişim kutusu açılır.

2. Ad verin `hello-nginx`. Kullanım [ `nginx` Docker kapsayıcısından](https://hub.docker.com/_/nginx/) ve bu web hizmetinin üç kopyaya dağıtın.

    ![Kubernetes Pod Oluştur iletişim kutusu](./media/container-service-kubernetes-ui/nginx.png)

3. Altında **hizmet**seçin **dış** ve 80 numaralı bağlantı noktasını girin.

    Bu ayar Yük Dengeleme trafiği üç kopyaya gerçekleştirir.

    ![Kubernetes hizmeti oluşturma iletişim kutusu](./media/container-service-kubernetes-ui/service.png)

4. Tıklayın **Dağıt** Bu kapsayıcıları ve Hizmetleri dağıtmak için.

    ![Kubernetes dağıtma](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Kapsayıcılarınızı görüntüleyin
Tıkladıktan sonra **Dağıt**, bunu dağıtan bir görünümünü hizmetinizin kullanıcı arabirimini gösterir:

![Kubernetes durumu](./media/container-service-kubernetes-ui/status.png)

Kullanıcı Arabirimi, sol tarafındaki altında bir daire içinde her bir Kubernetes nesnenin durumu görebilirsiniz **pod'ların**. Kısmen dolu bir daire ise, ardından nesnesi hala dağıtıyor. Bir nesne tam olarak dağıtıldığında, yeşil bir onay işareti görüntüler:

![Dağıtılan Kubernetes](./media/container-service-kubernetes-ui/deployed.png)

Her şey çalışır duruma geçtikten sonra çalışan web hizmeti ile ilgili ayrıntıları görmek için pod birine tıklayın.

![Kubernetes pod'ları](./media/container-service-kubernetes-ui/pods.png)

İçinde **pod'ların** görünümü, bu kapsayıcıları tarafından kullanılan CPU ve bellek kaynaklarının yanı sıra pod kapsayıcıları hakkında daha fazla bilgi görebilirsiniz:

![Kubernetes kaynakları](./media/container-service-kubernetes-ui/resources.png)

Kaynaklarını görmüyorsanız, izleme verilerini yayılması için birkaç dakika beklemeniz gerekebilir.

Kapsayıcı için günlükleri görmek için tıklayın **günlükleri görüntüleyebilir**.

![Kubernetes günlükleri](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Hizmetinizi görüntüleme
Kapsayıcılarınızı çalıştırmanın yanı sıra, bir dış Kubernetes kullanıcı arabirimini oluşturdu `Service` kümenizde kapsayıcıları trafik için yük dengeleyici sağlar.

Sol gezinti bölmesinden **Hizmetleri** (bulunmamalıdır tek) tüm hizmetleri görüntülemek için.

![Kubernetes Hizmetleri](./media/container-service-kubernetes-ui/service-deployed.png)

Bu görünümde, hizmetinize ayrılmış bir dış uç noktası (IP adresi) görmeniz gerekir.
Bu IP adresine tıklayın, yük dengeleyicinin ardında çalışan Ngınx kapsayıcınızı görmeniz gerekir.

![ngınx görüntüle](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Hizmetinizi yeniden boyutlandırma
Kullanıcı Arabiriminde nesnelerinizi görüntülemenin yanı sıra düzenleyin ve Kubernetes API nesneleri güncelleştirin.

İlk olarak, tıklayın **dağıtımları** hizmetiniz için dağıtım görmek için sol gezinti bölmesinde.

Bu görünümde olduktan sonra üzerinde çoğaltma kümesine tıklayın ve ardından **Düzenle** üst gezinti çubuğundaki:

![Kubernetes Edit](./media/container-service-kubernetes-ui/edit.png)

Düzen `spec.replicas` olmasını alan `2`, tıklatıp **güncelleştirme**.

Bu iki podlarınız birini silerek bırakmak için yineleme sayısı neden olur.

 

