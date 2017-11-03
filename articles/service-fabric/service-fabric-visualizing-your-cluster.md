---
title: "Service Fabric Explorer kullanarak kümenizi Görselleştirme | Microsoft Docs"
description: "Service Fabric Explorer inceleme ve bulut uygulamaları ve Microsoft Azure Service Fabric kümesindeki düğümler yönetmek için bir web tabanlı araçtır."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: c875b993-b4eb-494b-94b5-e02f5eddbd6a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2017
ms.author: ryanwi
ms.openlocfilehash: 965ffc0f8cec26cccbe6e6459731afc234111f4d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="visualize-your-cluster-with-service-fabric-explorer"></a>Service Fabric Explorer ile kümenizi görselleştirme
Service Fabric Explorer inceleme ve uygulamaları ve bir Azure Service Fabric kümesindeki düğümler yönetmek için bir web tabanlı araçtır. Her zaman kümenizi nerede çalıştığına bakmaksızın kullanılabilir olması için Service Fabric Explorer doğrudan küme içinde barındırılır.

## <a name="video-tutorial"></a>Video öğretici

Service Fabric Explorer kullanmayı öğrenmek için aşağıdaki Microsoft Virtual Academy videoyu izleyin:

[<center><img src="./media/service-fabric-visualizing-your-cluster/SfxVideo.png" WIDTH="360" HEIGHT="244"></center>](https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=bBTFg46yC_9806218965)

## <a name="connect-to-service-fabric-explorer"></a>Service Fabric Gezgini'ne Bağlan
Yönergeleri izlediyseniz [geliştirme ortamınızı hazırlama](service-fabric-get-started.md), http://localhost: 19080/Explorer giderek yerel kümenizde Service Fabric Explorer başlatabilirsiniz.

## <a name="understand-the-service-fabric-explorer-layout"></a>Service Fabric Explorer düzeni anlama
Soldaki ağaç kullanarak Service Fabric Explorer gidebilirsiniz. Ağaç kökünde küme Panosu uygulama ve düğüm durumu özetini dahil olmak üzere kümenizi genel bir bakış sağlar.

![Service Fabric Explorer küme Panosu][sfx-cluster-dashboard]

### <a name="view-the-clusters-layout"></a>Kümenin düzeni görüntüleyin
Bir Service Fabric kümesindeki düğümler hata etki alanı arasında iki boyutlu bir kılavuz yerleştirilir ve etki alanlarını yükseltme. Bu yerleştirme uygulamalarınızı donanım hataları ve uygulama yükseltmelerini varlığında kullanılabilir kalmasını sağlar. Nasıl geçerli Küme Küme eşlemesi kullanarak düzenlendiğini görüntüleyebilirsiniz.

![Service Fabric Explorer küme eşleme][sfx-cluster-map]

### <a name="view-applications-and-services"></a>Görünüm uygulamaları ve Hizmetleri
Küme iki alt ağaçları içerir: biri uygulamaları ve diğer düğümleri için.

Service Fabric'ın mantıksal hiyerarşide gezinmek için uygulama görünümü kullanabilirsiniz: uygulamaları, hizmetleri, bölümler ve çoğaltmalar.

Uygulama aşağıdaki örnekte **Uygulamam** iki hizmetinden, oluşan **Mystatefulservice'ten** ve **WebService**. Bu yana **Mystatefulservice'ten** durum bilgisi olan bir birincil ve iki ikincil çoğaltma sahip bir bölüm içerir. Bunun aksine, WebSvcService durum bilgisiz ve tek bir örnek içerir.

![Service Fabric Explorer uygulama görünümü][sfx-application-tree]

Her ağacı düzeyinde ana bölmede öğe hakkında bilgileri gösterir. Örneğin, belirli bir hizmet için sürüm ve sistem durumunu görebilirsiniz.

![Service Fabric Explorer essentials bölmesi][sfx-service-essentials]

### <a name="view-the-clusters-nodes"></a>Küme düğümlerini görüntüleme
Düğüm görünümü, kümenin fiziksel düzenini gösterir. Belirli bir düğümde, hangi uygulamalara kod dağıtıldığını denetleyebilirsiniz. Daha açık belirtmek gerekirse, çoğaltmalar var. çalışmakta görebilirsiniz.

## <a name="actions"></a>Eylemler
Service Fabric Explorer düğümleri, uygulamaları ve Hizmetleri, küme içindeki eylemleri çağırmak için hızlı bir yol sunar.

Örneğin, bir uygulama örneği silmek için soldaki ağaç uygulamayı seçin ve ardından **Eylemler** > **uygulama Sil**.

![Bir Service Fabric Explorer'da uygulama silme][sfx-delete-application]

> [!TIP]
> Her öğe yanındaki üç nokta işaretine tıklayarak aynı eylemleri gerçekleştirebilirsiniz.
>
>

Aşağıdaki tabloda her bir varlık için kullanılabilir eylemleri listeler:

| **Varlık** | **Eylem** | **Açıklama** |
| --- | --- | --- |
| Uygulama türü |Sağlamayı kaldırma türü |Uygulama paketi kümenin görüntü deposundan kaldırır. Bu türdeki tüm uygulamaları önce kaldırılmasını gerektirir. |
| Uygulama |Uygulamayı Silme |Tüm hizmetler ve durumlarına (varsa) dahil olmak üzere uygulama silin. |
| Hizmet |Hizmet silme |Hizmet ve durumuna (varsa) silin. |
| Node |Etkinleştir |Düğüm etkinleştirin. |
| Node | (Ara) devre dışı bırakma | Geçerli durumunda düğümü duraklatır. Hizmetleri çalışmaya devam eder, ancak bir kesinti veya veri tutarsızlığı önlemek için gerekli olmadıkça Service Fabric proaktif olarak herhangi bir şey üzerine veya onu devre dışı taşımaz. Bu eylem, genellikle belirli bir düğümde İnceleme sırasında taşımayın emin olmak için hata ayıklama hizmetlerini etkinleştirmek için kullanılır. | |
| Node | (Yeniden) devre dışı bırakma | Güvenli bir şekilde tüm bellek içi Hizmetleri bir düğümü kapalı ve kalıcı Hizmetleri kapatın taşıyın. Genellikle kullanılmaz ana bilgisayar işlemlerini ya da makinenin yeniden başlatılması gerekir. | |
| Node | (Verileri Kaldır) devre dışı bırakma | Güvenli bir şekilde yeterli yedek çoğaltmaları oluşturduktan sonra düğüm üzerinde çalışan tüm hizmetleri kapatın. Genellikle bir düğümü kullanılır (veya en az depolama alanı) dışında Komisyonu kalıcı olarak kullanıldığını. | |
| Node | Düğüm durumu Kaldır | Bir düğümün çoğaltmaları bilgisi kümeden kaldırın. Genellikle, zaten başarısız olan bir düğümün kurtarılamaz kabul edilir olduğunda kullanılır. | |
| Node | Yeniden Başlatma | Bir düğüm düğümü yeniden başlatarak arızanın benzetimini gerçekleştirin. Daha fazla bilgi [burada](/powershell/module/servicefabric/restart-servicefabricnode?view=azureservicefabricps) | |

Birçok Eylemler bozucu olduğundan, işlem tamamlanmadan önce maksadınızı onaylamak için istenebilir.

> [!TIP]
> Service Fabric Explorer gerçekleştirilebilir her eylem otomasyonunu sağlamak için PowerShell veya bir REST API'si gerçekleştirilebilir.
>
>

Service Fabric Explorer, verilen uygulama türü ve sürümü için uygulama örnekleri oluşturmak için de kullanabilirsiniz. Ağaç görünümünde uygulama türünü seçin ve ardından **oluşturma uygulama örneği** gibi sağ bölmede sürüm yanındaki bağlantı.

![Service Fabric Explorer'da uygulama örneğini oluşturma][sfx-create-app-instance]

> [!NOTE]
> Service Fabric Explorer ile oluşturulan uygulama örnekleri şu anda parametreli olamaz. Bunlar, varsayılan parametre değerleri kullanılarak oluşturulur.
>
>

## <a name="connect-to-a-remote-service-fabric-cluster"></a>Uzak bir Service Fabric kümeye bağlanın
Kümenin endpoint biliyor ve yeterli izinlere sahip herhangi bir tarayıcıdan Service Fabric Explorer erişebilir. Service Fabric Explorer kümede çalışan başka bir hizmete olmasıdır.

### <a name="discover-the-service-fabric-explorer-endpoint-for-a-remote-cluster"></a>Uzak bir küme için Service Fabric Explorer uç noktası bulunamadı
Belirli bir küme için Service Fabric Explorer ulaşmak için tarayıcınızı noktası:

http://&lt;küme endpoint bilgisayarınızı&gt;: 19080/Explorer

Azure kümeleri için tam URL de Azure Portalı'nın küme essentials bölmesinde kullanılabilir.

### <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma
Service Fabric kümenize, sertifikalar veya Azure Active Directory (AAD) kullanarak, istemci erişimi denetleyebilirsiniz.

Service Fabric Gezgini'ne güvenli bir kümede bağlanmaya çalışırsanız, ardından kümenin yapılandırmasına bağlı olarak, bir istemci sertifikası sunan veya AAD kullanarak oturum gerekecek.

## <a name="next-steps"></a>Sonraki adımlar
* [Test Edilebilirlik genel bakış](service-fabric-testability-overview.md)
* [Visual Studio'da, Service Fabric uygulamaları yönetme](service-fabric-manage-application-in-visual-studio.md)
* [PowerShell kullanarak Service Fabric uygulama dağıtımı](service-fabric-deploy-remove-applications.md)

<!--Image references-->
[sfx-cluster-dashboard]: ./media/service-fabric-visualizing-your-cluster/SfxClusterDashboard.png
[sfx-cluster-map]: ./media/service-fabric-visualizing-your-cluster/SfxClusterMap.png
[sfx-application-tree]: ./media/service-fabric-visualizing-your-cluster/SfxApplicationTree.png
[sfx-service-essentials]: ./media/service-fabric-visualizing-your-cluster/SfxServiceEssentials.png
[sfx-delete-application]: ./media/service-fabric-visualizing-your-cluster/SfxDeleteApplication.png
[sfx-create-app-instance]: ./media/service-fabric-visualizing-your-cluster/SfxCreateAppInstance.png
