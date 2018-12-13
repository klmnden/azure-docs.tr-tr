---
title: Azure Service Fabric Mesh örnekler bulun | Microsoft Docs
description: Service Fabric Mesh farklı örnek uygulamalar bulmayı öğrenin.
services: service-fabric-mesh
keywords: ''
author: v-vasuke
ms.author: v-vasuke
ms.date: 12/03/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: d31ee60977bb4ac11a93cfe1e72134c65baf839c
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53161254"
---
# <a name="find-service-fabric-mesh-samples"></a>Service Fabric kafes örnekleri bulun

Bu tabloda mevcut Service Fabric Mesh örnek uygulamalar özetlenmektedir. Bu örnek kaynak kodda, Service Fabric kaynak modeli kullanarak belirli bir senaryo elde etmek gösterilmektedir.

Şablonları doğrudan Azure'a dağıtma hakkında daha fazla bilgi için bkz. [örnek şablonu GitHub sayfası.](https://github.com/Azure-Samples/service-fabric-mesh/blob/master/templates/README.md)


|Örnek şablonu|Senaryo açıklaması|Kaynak kodu|Geliştirici Araçları|
|------------|--------------------|----------|----------------------|
| [Merhaba Dünya uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/helloworld) | Bir kapsayıcıda barındırılan statik bir Web sayfası. Linux için Windows IIS için ngınx kullanır | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/helloworld) | Gereksinim yok |
| [Azure dosya birimleri için sayacı uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter) | Azure dosyaları bağlayarak Store durumu birim kapsayıcı içinde temel. <br><br> **Not:** Bu şablon zaten sağlanmış olması için bir Azure dosyaları dosya paylaşımı gerektirir [yönergeleri](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share) | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio Araçları Mesh |
| [TodoListApp](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/todolistapp) | DNS tabanlı çözümü kullanan bir ön uç ve arka uç hizmetiyle uygulama oluşturma. Öğretici olarak kullanılan [burada](https://docs.microsoft.com/azure/service-fabric-mesh/service-fabric-mesh-tutorial-create-dotnetcore) | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/todolistapp) | Visual Studio Araçları Mesh |
| [Görsel nesneler uygulama](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/visualobjects) | Bir uygulama içinde ölçek ve yükseltme mikro hizmetler. | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/visualobjects) |  Visual Studio Araçları Mesh |
| [Oylama uygulaması](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/votingapp) | DNS tabanlı çözümü kullanan bir ön uç ve arka uç hizmetiyle uygulama oluşturma | [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/votingapp) | Windows sürümü için Araçlar, visual Studio kafes VS Code / dotnet CLI Linux sürümü için kullanılabilir |
| [Service Fabric güvenilir birimler için sayacı uygulama](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/templates/counter)| Service Fabric güvenilir Disk bağlayarak Store durumu birim kapsayıcı içinde temel.| [Kaynak kodu](https://github.com/Azure-Samples/service-fabric-mesh/tree/master/src/counter) | Visual Studio Araçları Mesh |
