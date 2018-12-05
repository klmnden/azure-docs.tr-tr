---
title: Azure Service Fabric Mesh projeleri için Visual Studio performansını iyileştirme | Microsoft Docs
description: Azure Service Fabric Örgü uygulamalar için Visual Studio performansını iyileştirme
services: service-fabric-mesh
keywords: hata ayıklama performansını iyileştirme
author: tylermsft
ms.author: twhitney
ms.date: 11/29/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: 72e900e6e48d18a721be7d2991428f81a81d1303
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52893532"
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