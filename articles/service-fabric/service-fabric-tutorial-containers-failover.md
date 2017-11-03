---
title: "Yük devretme ve bir Azure Service Fabric kapsayıcıları uygulama ölçeklendirme | Microsoft Docs"
description: "Yük devretme Azure Service Fabric kapsayıcıları uygulamada nasıl işlendiğini öğrenin.  Ayrıca bir kümede çalışan hizmetler ve kapsayıcıları ölçeklendirmek öğrenin."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker, kapsayıcıları, mikro, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 21dd9dfbc90c26236c43e2c334305ca97f63d361
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="demonstrate-fail-over-and-scaling-of-container-services-with-service-fabric"></a>Başarısız üstünde ve Service Fabric ile kapsayıcı hizmetlerin ölçeklendirme gösterme

Bu öğretici üç serinin bir parçasıdır. Bu öğreticide, yük devretme Service Fabric kapsayıcı uygulamalarda nasıl işlendiğini öğrenin. Ayrıca, kapsayıcı ölçeklendirmek nasıl öğrenin. Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * Service Fabric kümesi kapsayıcı yük hakkında bilgi edinin  
> * Web ön uç kapsayıcıları bir yaptığınız ölçeklendirme

## <a name="prerequisites"></a>Ön koşullar
Uygulamadan [Kısım 2](service-fabric-tutorial-package-containers.md) etkin bir Service Fabric kümede çalışıyor.

## <a name="fail-over-a-container-in-a-cluster"></a>Kümedeki bir kapsayıcıya yük devretme
Service Fabric, kapsayıcı örneklerini otomatik olarak taşır. kümedeki diğer düğümlere hata gerçekleşeceğini emin olur. El ile de kapsayıcıların bir düğüm boşaltma ve uygun şekilde kümedeki diğer düğümlere taşıyabilirsiniz. Hizmetlerinizi ölçeklendirmek için kullanabileceğiniz birden fazla yöntem vardır. Bu örnekte Service Fabric Explorer'ı kullanacağız.

Ön uç kapsayıcısında yük devretmek için aşağıdaki adımları uygulayın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Tıklayın **fabric: / TestContainer/azurevotefront** ağaç görünümünde düğümünü ve (bir GUID ile temsil edilen) bölüm düğümünü genişletin. Düğüm adı düğümleri gösterir treeview içindeki dikkat edin, kapsayıcı şu anda - örneğin çalışıyor`_nodetype_1`
3. Genişletme **düğümleri** treeview düğümüne. Kapsayıcı çalıştığı düğüm yanındaki üç nokta (üç nokta) tıklayın.
1. İlgili düğümü yeniden başlatmak için **Yeniden Başlat**'ı seçin ve yeniden başlatma eylemini onaylayın. Yeniden başlatma durumunda kapsayıcıdan kümedeki başka bir düğüme yük devretme gerçekleştirilir.

![noderestart][noderestart]

Ön uç kapsayıcıları çalıştıran burada nasıl düğüm adı belirten şimdi kümedeki başka bir düğüme değişikliklerini dikkat edin. Birkaç dakika sonra uygulamayı yeniden bulun ve şimdi farklı bir düğüm üzerinde çalışan uygulama bkz yapabiliyor olmanız gerekir.

## <a name="scale-containers-and-services-in-a-cluster"></a>Ölçek kapsayıcıları ve bir Küme Hizmetleri
Service Fabric kapsayıcıları Hizmetleri üzerindeki yükü için uygun hale getirmek için bir küme içinde genişletilebilir. Kümede çalışan örneği sayısı değiştirerek bir kapsayıcı ölçeklendirin.

Ön uç web ölçeklendirmek için aşağıdaki adımları uygulayın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `http://lin4hjim3l4.westus.cloudapp.azure.com:19080`.
2. Üç nokta (üç nokta) tıklayın **fabric: / TestContainer/azurevotefront** ağaç düğümünde görüntüleyin ve seçin **ölçek hizmet**.

![sfxscale][sfxscale]

Şimdi Web ön uç örnek sayısını ölçeklendirmek seçebilirsiniz.

3. Rakamı **2** olarak değiştirin ve **Hizmeti Ölçeklendir**'e tıklayın.
4. Tıklayın **fabric: / TestContainer/azurevotefront** ağaç düğümünde görüntülemek ve (bir GUID ile temsil edilen) bölüm düğümünü genişletin.

![sfxscaledone][sfxscaledone]

Hizmetin artık iki örneği olduğunu görebilirsiniz. Ağaç görünümünde örnekleri çalıştıracağınız hangi düğümlerin bakın.

Bu basit yönetim görevi sayesinde ön uç hizmetinizde kullanıcı yükünü işleyecek kaynak sayısını iki katına çıkarmış olduk. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığını anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir uygulama ölçeklendirme yanı sıra kapsayıcı yük devretme gösterilen. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Service Fabric kümesi kapsayıcı yük hakkında bilgi edinin  
> * Web ön uç kapsayıcıları bir yaptığınız ölçeklendirme

Bu öğretici serisinde öğrenilen nasıl yapılır: 
> [!div class="checklist"]
> * Kapsayıcı görüntüleri oluşturma
> * Azure kapsayıcı kayıt defterine kapsayıcı görüntüleri bildirme
> * Yeoman kullanarak Service Fabric paketi kapsayıcıları
> * Derleme ve Service Fabric uygulaması ile kapsayıcıları çalıştırma
> * Service Fabric yük devretme ve ölçeklendirme nasıl işlenir

[noderestart]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialnoderestart.png
[sfxscale]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscale.png
[sfxscaledone]: ./media/service-fabric-tutorial-containers-failover/containersfailovertutorialscaledone.png
