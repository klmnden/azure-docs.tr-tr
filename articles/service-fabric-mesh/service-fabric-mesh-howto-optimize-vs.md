---
title: Azure Service Fabric Mesh projeleri için Visual Studio performansını iyileştirme | Microsoft Docs
description: Azure Service Fabric Örgü uygulamalar için Visual Studio performansını iyileştirme
services: service-fabric-mesh
keywords: hata ayıklama performansını iyileştirme
author: dkkapur
ms.author: dekapur
ms.date: 11/29/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: chakdan
ms.openlocfilehash: c7c9f9e72ae7ec2807c8fea08a8cc8d3e8a9e382
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804816"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Service Fabric Mesh projeleri için Visual Studio performansını iyileştirme

Bu makalede, ilk çalıştırmayı (F5) hata ayıklama çok daha hızlı olması, Service Fabric Mesh projeleri için Visual Studio performansını iyileştirmek nasıl gösterir.  

## <a name="change-visual-studio-settings"></a>Visual Studio ayarlarını değiştirme
 
Visual Studio'da altında **Araçları** > **seçenekleri**  > **Service Fabric kafes Araçları** > **genel**, aşağıdaki ayarları ayarlayabilirsiniz:

- **Projede açık gerekli Docker görüntülerini çekme** , ilk proje yüklenirken görüntü indirme işlemi başlatılıyor (F5) daha hızlı çalışmasını hata ayıklamasını sağlar.  
- **Açık olan proje üzerinde uygulaması dağıtma** , ilk proje açıldıktan sonra dağıtım işlemi başlatarak (F5) daha hızlı çalışmasını hata ayıklama yapabilirsiniz.  
- **Uygulamayı kapat projede kaldırmak** Mesh uygulaması proje kapatıldığında kaldırarak uygulamaya ayrılan kaynakları (CPU, RAM) geri kazanır.  

Visual Studio Service Fabric araçları çıktı penceresinde bir ileti gördüğünüzde 'görüntülerini çekme', 'ısınıyor' veya 'uygulama kaldırarak', yukarıdaki ayarların bağlamında olduğundan. Bu ayarları devre dışı kapatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Okumak [Mesh uygulaması Öğreticisi hata ayıklama](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)