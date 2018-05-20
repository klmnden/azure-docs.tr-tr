---
title: Visual Studio'da Azure bildirimleri Fabric uygulamalarınızı yönetin | Microsoft Docs
description: Oluşturmak, geliştirmek, paketi, dağıtmak ve Azure Service Fabric uygulamaları ve Hizmetleri hata ayıklamak için Visual Studio'yu kullanın.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2018
ms.author: mikhegn
ms.openlocfilehash: 25c7f0e8d6ebc31121e29870026a735495ef7900
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Yazma ve Service Fabric uygulamaları yönetme basitleştirmek için Visual Studio kullanma
Azure Service Fabric uygulamaları ve Hizmetleri Visual Studio aracılığıyla yönetebilirsiniz. Seçtiğiniz sonra [geliştirme ortamınızı ayarlama](service-fabric-get-started.md), Service Fabric uygulamaları oluşturmak, hizmetleri veya paket, kayıt eklemek ve yerel geliştirme kümenizdeki uygulamaları dağıtmak için Visual Studio'yu kullanabilirsiniz.

## <a name="deploy-your-service-fabric-application"></a>Service Fabric uygulamanızı dağıtma
Varsayılan olarak, bir uygulama dağıtımı aşağıdaki adımları basit bir işlemde birleştirir:

1. Uygulama paketi oluşturma
2. Uygulama paketi görüntü deposuna karşıya yükleme
3. Uygulama türünü kaydetme
4. Herhangi bir kaldırma uygulama örnekleri çalıştırma
5. Uygulama örneğini oluşturma

Visual Studio'da tuşuna basarak **F5** uygulamanızı dağıtır ve tüm uygulama örneklerine hata ayıklayıcısını ekleyin. Kullanabileceğiniz **Ctrl + F5** hata ayıklama veya bir uygulamayı dağıtmak için yerel veya uzak bir küme için yayımlama profili kullanarak yayımlayabilirsiniz.

### <a name="application-debug-mode"></a>Uygulama hata ayıklama modu
Visual Studio sağlamak adlı bir özellik **uygulama hata ayıklama modu**, uygulama dağıtımı hata ayıklama bir parçası olarak işlemek için görsel stüdyoları istediğiniz kontrol eder.

#### <a name="to-set-the-application-debug-mode-property"></a>Uygulama hata ayıklama modu özelliğini ayarlamak için
1. Service Fabric uygulaması projenin (*.sfproj) üzerinde kısayol menüsünü seçin **özellikleri** (veya basın **F4** anahtar).
2. İçinde **özellikleri** penceresindeki ayarlayın **uygulama hata ayıklama modu** özelliği.

![Uygulama hata ayıklama modu özelliğini ayarlayın][debugmodeproperty]

#### <a name="application-debug-modes"></a>Uygulama hata ayıklama modu

1. **Uygulamayı yenileyin** hızlı bir şekilde değiştirin ve statik web dosyaları ayıklarken düzenlemeyi destekler ve kod hatalarını ayıklamak bu modu sağlar. Bu modda, yalnızca yerel geliştirme kümenizi [1-düğümü] modundaysa çalışır. Varsayılan uygulama hata ayıklama modu budur. (/ service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).
2. **Uygulamayı kaldırmak** uygulamanın hata ayıklama oturumu sona erdiğinde kaldırılmasına neden olur.
3. **Yükseltme otomatik** uygulama hata ayıklama oturumu sona erdiğinde çalışmaya devam eder. Sonraki hata ayıklama oturumu dağıtım yükseltme olarak kabul eder. Yükseltme işlemi önceki bir hata ayıklama oturumunda girdiğiniz herhangi bir veriyi korur.
4. **Uygulama tutmak** hata ayıklama oturumu sona erdiğinde uygulama kümede çalışan tutar. Sonraki hata ayıklama oturumu başlangıcında, uygulama kaldırılır.

İçin **otomatik yükseltme** veriler Service Fabric uygulama yükseltme özelliklerini uygulayarak korunur. Uygulamalar ve gerçek bir ortamda bir yükseltme nasıl yerine getirmeyebilir yükseltme hakkında daha fazla bilgi için bkz: [Service Fabric uygulama yükseltme](service-fabric-application-upgrade.md).

## <a name="add-a-service-to-your-service-fabric-application"></a>Service Fabric uygulamanızı bir hizmet Ekle
Uygulamanızı işlevselliğini genişletmek için yeni hizmetler ekleyebilirsiniz. Hizmet, uygulama paketinde bulunduğundan emin olun, hizmeti aracılığıyla eklemek **yeni Fabric hizmeti...**  menü öğesi.

![Yeni bir Service Fabric hizmeti ekleyin][newservice]

Uygulamanıza eklemek için Service Fabric proje türü seçin ve hizmet için bir ad belirtin.  Bkz: [hizmetiniz için bir çerçeve seçme](service-fabric-choose-framework.md) kullanmak için hangi hizmet türü karar vermenize yardımcı olacak.

![Uygulamanıza eklemek için Service Fabric hizmeti proje türü seçin][addserviceproject]

Yeni hizmet, çözüm ve mevcut uygulama paketi eklenir. Hizmet başvuruları ve varsayılan hizmet örneği oluşturulabilir ve başladığı sonraki saat uygulamayı dağıtmak hizmetin neden uygulama bildirimine eklenir.

![Yeni hizmet uygulama bildiriminizi eklenir][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Service Fabric uygulamanızı paketleme
Bir kümeye uygulama ve Hizmetleri dağıtmak için bir uygulama paketi oluşturmanız gerekir.  Paket uygulama bildirimi, hizmet bildirimlerini ve diğer gerekli dosyaları belirli bir düzende düzenler.  Visual Studio ayarlayan ve 'pkg' dizini için uygulama projenin klasöründe paketini yönetir.  Tıklatarak **paket** gelen **uygulama** bağlam menüsü oluşturur veya uygulama paketini güncelleştirir.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Uygulamalar ve uygulama türleri bulut Gezgini'ni kullanarak kaldırma
Temel küme yönetimi işlemleri çalıştırmanızı sağlayan bulut Gezgini'ni kullanarak Visual Studio içinden gerçekleştirebilirsiniz **Görünüm** menüsü. Örneğin, uygulamaları silin ve yerel veya uzak kümelerde uygulama türleri sağlamasını.

![Bir uygulamayı kaldırma][removeapplication]

> [!TIP]
> Bkz: için daha zengin bir küme yönetim işlevleri, [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulama modeli](service-fabric-application-model.md)
* [Service Fabric uygulama dağıtımı](service-fabric-deploy-remove-applications.md)
* [Birden çok ortamlar için uygulama parametreleri yönetme](service-fabric-manage-multiple-environment-app-configuration.md)
* [Service Fabric uygulamanızı hata ayıklama](service-fabric-debugging-your-application.md)
* [Service Fabric Explorer kullanarak kümenizi Görselleştirme](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png