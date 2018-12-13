---
title: Azure Service Fabric Mesh depolama seçeneklerini belirtin. | Microsoft Docs
description: Güvenilir bir şekilde durumunu Service Fabric Mesh uygulamalarda çalışan Azure Service Fabric Mesh depolama hakkında bilgi edinin.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 11/27/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: ecdb36af786d96a5b343d11cd689642d59528445
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52888545"
---
# <a name="state-management-with-service-fabric"></a>Service Fabric ile durum yönetimi

Service Fabric, birçok farklı seçenek için durumu depolama alanını destekler. Kavramsal genel bakış durum yönetimi düzenleri ve Service Fabric için bkz: [Service Fabric Kavramları: durum](/azure/service-fabric/service-fabric-concepts-state). Hizmetlerinizi içinde veya Service Fabric Mesh dışında çalışan tüm aynı kavramlar geçerlidir. 

Service Fabric Mesh ile kolayca yeni bir uygulama dağıtın ve Azure'da barındırılan mevcut bir veri deposuna bağlanın. Herhangi bir uzak veritabanı kullanmanın yanı sıra, hizmet yerel veya uzak depolama olup istediği bağlı olarak verileri depolamak için birkaç seçenek vardır. 

## <a name="volumes"></a>Birimler

Kapsayıcılar genellikle olun geçici diskler kullanır. Yeni, geçici bir diskle almak ve bir kapsayıcı kilitlendiğinde bilgileri kaybedersiniz geçici diskler ancak kısa ömürlü, değildir. Ayrıca, diğer kapsayıcılarla geçici diskler hakkında bilgi paylaşmak zor olabilir. Birimler durumu kalıcı hale getirmek için kullanabileceğiniz kapsayıcı örneklerinizin içinde oluşturulmuş dizinleri listelenmiştir. Birimleri, genel amaçlı dosya depolama sağlar ve normal bir disk g/ç dosya API'lerini kullanarak dosya okuma/yazma olanak sağlar. Birim kaynağına nasıl bağlandığını bir dizin ve kullanmak için hangi yedekleme depolama açıklar. Azure dosya depolama veya Service Fabric birim disk verilerini saklamak için seçebilirsiniz.

![Birimler][image3]

### <a name="service-fabric-reliable-volume"></a>Service Fabric güvenilir birim

Service Fabric güvenilir birim kapsayıcısına yerel bir birimi bağlamak için kullanılan bir Docker birim sürücüsüdür. Okuma ve yazma işlemleri yerel işlemler ve hızlı olan. Veriler varsayılan olarak, yüksek oranda kullanılabilir hale getirme ikincil düğüm için çıkış çoğaltılır. Yük devretme hızlı çalışır. Bir kapsayıcı kilitlendiğinde verilerinizin bir kopyasının zaten bir düğüme devreder. Bir örnek için bkz [Service Fabric güvenilir toplu bir uygulamayla dağıtma.](https://github.com/Azure-Samples/service-fabric-mesh/tree/2018-09-01-preview/templates/counter)

### <a name="azure-files-volume"></a>Azure dosya birimi

Azure dosyaları birim kapsayıcısına bir Azure dosya paylaşımını bağlayabilmeniz için kullanılan bir Docker birim sürücüsü değil. Depolama ağ depolama kullanan yeniden, bu nedenle okuyan ve yazan azure dosyaları ağ üzerinden gerçekleşir. Service Fabric güvenilir birimine göre Azure dosya depolama daha az yüksek performanslı bağlıdır, ancak daha ucuz ve tam olarak güvenilir veri seçeneğini sağlar. Bir örnek için bkz. [Azure dosya birimi bir uygulamayla dağıtma](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md).

## <a name="next-steps"></a>Sonraki adımlar

Uygulama modeli hakkında daha fazla bilgi için bkz: [Service Fabric kaynakları](service-fabric-mesh-service-fabric-resources.md)

[image3]: ./media/service-fabric-mesh-storing-state/volumes.png
