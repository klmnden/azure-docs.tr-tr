---
title: Azure Service Fabric görüntü deposu bağlantı dizesi | Microsoft Docs
description: Görüntü deposu bağlantı dizesi anlama
services: service-fabric
documentationcenter: .net
author: alexwun
manager: chackdan
editor: ''
ms.assetid: 00f8059d-9d53-4cb8-b44a-b25149de3030
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2018
ms.author: alexwun
ms.openlocfilehash: 4a56b48c0041e963b89312c59335b45cabacc1bb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60720204"
---
# <a name="understand-the-imagestoreconnectionstring-setting"></a>Imagestoreconnectionstring ayarını anlama

Bazı belgelerimize size kısaca "ImageStoreConnectionString" parametre varlığı gerçekten anlamını açıklayan olmadan bahsedebilirsiniz. Ve bir makale gibi aracılığıyla olduktan sonra [PowerShell kullanarak dağıtma ve Kaldır uygulamaları][10], tüm yaptığınız gibi görünüyor kopyala/yapıştır hedef kümenin küme bildiriminde gösterildiği gibi bir değerdir. Ayar küme yapılandırılabilir olması gerekir, ancak aracılığıyla bir küme oluşturduğunuzda [Azure portalında][11], bu ayarı yapılandırma seçeneği yoktur ve her zaman "fabric: ImageStore" olur. Bu ayarın amacı nedir?

![Küme bildirimi][img_cm]

Service Fabric dahili Microsoft tüketim için bir platform olarak birçok farklı ekip tarafından başlatılan bazı yönlerini üst düzeyde özelleştirilebilir - "Görüntü Store" gibi bir yönü için. Esas olarak, görüntü Store takılabilir uygulama paketlerini depolamak için deposudur. Uygulamanız, kümedeki bir düğüme dağıtıldığında, o düğümde görüntü Store ', uygulama paketinin içeriği indirir. Imagestoreconnectionstring belirli bir küme için doğru görüntü Store bulmak için istemcileri ve düğümler için gerekli tüm bilgileri içeren bir ayardır.

Şu anda görüntü Store sağlayıcıları üç olası türü vardır ve bunların karşılık gelen bağlantı dizeleri aşağıdaki gibidir:

1. Görüntü Store hizmeti: "fabric: ImageStore"

2. Dosya sistemi: "file:[file sistem yolu]"

3. Azure Depolama: "xstore:DefaultEndpointsProtocol = https; AccountName = […]; AccountKey = […]; Kapsayıcı = […] "

Üretim ortamında kullanılan sağlayıcı türü görüntü Store, Service Fabric Explorer'ın görebileceğiniz bir durum bilgisi olan kalıcı sistemi hizmetidir hizmetidir. 

![Görüntü Store hizmeti][img_is]

Görüntü Store küme içindeki bir sistem hizmetinde barındırma paket deposu için dış bağımlılıkları ortadan kaldırır ve depolama konumu üzerinde daha fazla denetim sağlıyor. Gelecekteki geliştirmeleri görüntü Store etrafında ilk değilse özel görüntü Store sağlayıcısı hedef olasılığı düşüktür. İstemci zaten hedef kümeye bağlı olduğundan, görüntü Store hizmet sağlayıcı için bağlantı dizesini herhangi bir benzersiz bilgi yok. İstemci, yalnızca sistem hizmetini hedefleyen protokollerin kullanılması gerektiğini bilmeniz gerekir.

Dosya sistemi sağlayıcısı yerine görüntü Store hizmeti için yerel bir hazır kümeleri geliştirme sırasında küme biraz daha hızlı önyükleme için kullanılır. Fark genellikle daha küçüktür, ancak çoğu yeni başlayanlar için kullanışlı bir iyileştirme geliştirme sırasında olduğu. Diğer depolama sağlayıcısı türleri de olan bir yerel hazır bir küme dağıtmak mümkün mü, ancak genellikle bu yana geliştirme/test iş akışı sağlayıcısı bakılmaksızın aynı kalır. Bunu yapmak için bir neden yoktur. Azure depolama sağlayıcısının yalnızca görüntü Store hizmet sağlayıcısı sunulmadan önce dağıtılan kümeleri için eski destek eski bulunmaktadır.

Ayrıca, dosya sistemi sağlayıcısı veya Azure depolama sağlayıcısının birden çok küme arasındaki bir görüntü Store paylaşım yöntemi olarak kullanılmalıdır - her küme görüntüye çakışan veri yazabilen gibi bu küme yapılandırması veri bozulmasına neden olur Store. Sağlanan uygulama paketlerini birden fazla küme arasında paylaşmak için kullanmak [sfpkg] [ 12] bunun yerine, hangi dosyaların herhangi bir dış depolama indirme URI'si ile karşıya yüklenebilir.

Bu nedenle Imagestoreconnectionstring yapılandırılabilir olsa da, yalnızca varsayılan ayarı kullanın. Visual Studio aracılığıyla azure'a yayımlarken, parametre otomatik olarak sizin için uygun şekilde ayarlanır. Azure'da barındırılan kümelerine programlamalı dağıtım için bağlantı dizesi her zaman "fabric: ImageStore" dir. Şüpheye düştüğünüzde değeri her zaman tarafından gibi küme bildiriminin alarak doğrulanabilir rağmen [PowerShell](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricclustermanifest), [.NET](https://msdn.microsoft.com/library/azure/mt161375.aspx), veya [REST](https://docs.microsoft.com/rest/api/servicefabric/get-a-cluster-manifest). Hem şirket içinde test ve üretim kümeleri her zaman da görüntü Store hizmeti sağlayıcısı kullanacak şekilde yapılandırılmalıdır.

### <a name="next-steps"></a>Sonraki adımlar
[Dağıtma ve PowerShell kullanarak uygulamaları kaldırma][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-package-apps.md#create-an-sfpkg
