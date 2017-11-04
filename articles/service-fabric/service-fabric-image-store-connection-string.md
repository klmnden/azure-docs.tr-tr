---
title: "Azure Service Fabric görüntü Mağazası'ndan dize | Microsoft Docs"
description: "Görüntü deposu bağlantı dizesi anlama"
services: service-fabric
documentationcenter: .net
author: alexwun
manager: timlt
editor: 
ms.assetid: 00f8059d-9d53-4cb8-b44a-b25149de3030
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/02/2017
ms.author: alexwun
ms.openlocfilehash: 49003c16c262180afcdba22c5557c91297cb2840
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="understand-the-imagestoreconnectionstring-setting"></a>ImageStoreConnectionString ayarı anlama

Bazı Belgelerimizi biz kısaca "ImageStoreConnectionString" parametresi varlığını gerçekten anlamını açıklayan olmadan Bahsediyor. Ve bir makale gibi aracılığıyla olduktan sonra [PowerShell kullanarak uygulamaları dağıtma ve Kaldır][10], bunu tüm gibi görünüyor hedef kümenin küme bildiriminde göründüğü gibi kopyala/yapıştır değerdir. Ayarı küme yapılandırılabilir olması gerekir, ancak aracılığıyla bir küme oluştururken [Azure portal][11], bu ayarı yapılandırmak için bir seçenek yoktur ve her zaman "fabric: görüntü" olur. Bu ayarın amacı nedir?

![Küme bildirimi][img_cm]

Service Fabric dahili Microsoft tüketim için bir platform olarak birçok farklı ekip tarafından başlatılan bazı yönlerini büyük ölçüde özelleştirilebilir - "Image Store" gibi bir yönü için. Esas olarak, görüntü deposu takılabilir uygulama paketleri depolamak için deposudur. Kümedeki bir düğüm için uygulamanızı dağıtıldığında, bu düğüm görüntü deposundan uygulama paketinizi içeriğini indirir. ImageStoreConnectionString verilmiş bir küme için doğru Image Store bulmak için istemcileri ve düğümler için gerekli tüm bilgileri içeren bir ayardır.

Şu anda görüntü deposu sağlayıcıları üç olası tür vardır ve bunların karşılık gelen bağlantı dizeleri aşağıdaki gibidir:

1. Image Store hizmeti: "fabric: görüntü"

2. Dosya sistemi: "file:[file sistem path]"

3. Azure Storage: "xstore:DefaultEndpointsProtocol = https; AccountName = [...]; AccountKey = [...]; Kapsayıcı = [...] "

Üretimde kullanılan sağlayıcı türü görüntü deposu, Service Fabric Explorer'dan gördüğünüz bir durum bilgisi olan kalıcı sistem hizmeti olduğu hizmetidir. 

![Image Store hizmeti][img_is]

Bir sistem hizmeti küme içindeki görüntü deposunda barındırma paket deposu için dış bağımlılıklar ortadan kaldırır ve bize depolama yerleşim üzerinde daha fazla denetim sağlar. Image Store geçici gelecekteki geliştirmeleri ilk değilse yalnızca Image Store sağlayıcısını hedeflemek olasıdır. İstemci zaten hedef kümeye bağlı olduğundan, görüntü deposu hizmet sağlayıcı için bağlantı dizesini herhangi bir benzersiz bilgi sahip değil. İstemci, yalnızca sistem hizmeti hedefleme protokolleri kullanılması gereken bilmesi gerekir.

Dosya sistemi sağlayıcısı görüntü Deposu hizmetini yerine yerel bir kutusunu kümeleri için geliştirme sırasında küme biraz daha hızlı bootstrap için kullanılır. Fark genellikle küçük, ancak çoğu terimleri için yararlı bir iyileştirme geliştirme sırasında gelir. Diğer türleriyle depolama sağlayıcısı da yerel bir çalıştırma küme dağıtmak mümkündür, ancak genellikle geliştirme ve test iş akışı sağlayıcısı bakılmaksızın aynı kalır olduğundan Bunu yapmak için bir neden yoktur. Bu kullanım dışında dosya sistemi ve Azure depolama sağlayıcıları için eski destek yalnızca mevcut.

ImageStoreConnectionString yapılandırılabilir olsa da, bu nedenle, genellikle yalnızca varsayılan ayarı kullanın. Azure üzerinden yayımlarken [Visual Studio][12], parametre otomatik olarak sizin için uygun şekilde ayarlanır. Azure üzerinde barındırılan kümeleri için programlı dağıtımı için bağlantı dizesi her zaman "fabric: görüntü" dir. Şüpheli zaman değeri her zaman küme bildirimi tarafından alarak doğrulanabilir olsa [PowerShell](https://docs.microsoft.com/powershell/servicefabric/vlatest/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), veya [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest). Her iki şirket içi test ve üretim kümelerine her zaman görüntü deposu hizmet sağlayıcısı de kullanmak için yapılandırılmış olması gerekir.

### <a name="next-steps"></a>Sonraki adımlar
[Dağıtma ve PowerShell kullanarak uygulamaları kaldırma][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-publish-app-remote-cluster.md
