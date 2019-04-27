---
title: Visual Studio'da Azure Service Fabric uygulamalarınızı yönetmek | Microsoft Docs
description: Oluşturma, geliştirme, paketlemeyi, dağıtmayı ve Azure Service Fabric uygulamalarınızı ve hizmetlerinizi hata ayıklama için Visual Studio'yu kullanın.
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: chackdan
editor: ''
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.custom: vs-azure
ms.workload: azure-vs
ms.date: 03/26/2018
ms.author: mikhegn
ms.openlocfilehash: 4744858869e10094389be58ddd3960cb8cc2773a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60720095"
---
# <a name="use-visual-studio-to-simplify-writing-and-managing-your-service-fabric-applications"></a>Service Fabric uygulamalarınızı yönetmek ve yazma işlemlerini kolaylaştırmak için Visual Studio'yu kullanın.
Azure Service Fabric uygulamalarınızı ve hizmetlerinizi Visual Studio aracılığıyla yönetebilirsiniz. Kaydederler [geliştirme ortamınızı ayarlama](service-fabric-get-started.md), Service Fabric uygulamaları oluşturmanıza, hizmetler veya paket, kayıt ekleyin ve uygulamaları, yerel geliştirme kümenizin dağıtmak için Visual Studio'yu kullanabilirsiniz.

## <a name="deploy-your-service-fabric-application"></a>Service Fabric uygulamanızı dağıtma
Varsayılan olarak, uygulama dağıtma, aşağıdaki adımları basit bir işlemde birleştirir:

1. Uygulama paketi oluşturma
2. Görüntü deposu için uygulama paketi yükleniyor
3. Uygulama türü kaydetme
4. Tüm kaldırma çalışan uygulama örnekleri
5. Bir uygulama örneği oluşturma

Visual Studio'da tuşuna basarak **F5** uygulamanızı dağıtır ve tüm uygulama örnekleri için hata ayıklayıcının. Kullanabileceğiniz **Ctrl + F5** hata ayıklama veya bir uygulamayı dağıtmak için bir yerel veya uzak kümeye yayımlama profilini kullanarak yayımlayabilirsiniz.

### <a name="application-debug-mode"></a>Uygulama hata ayıklama modu
Visual Studio sağlamak adlı bir özellik **uygulama hata ayıklama modu**, uygulama dağıtımı hata ayıklama işleminin parçası olarak işlemek için Visual Studios nasıl istediğinizi kontrol eder.

#### <a name="to-set-the-application-debug-mode-property"></a>Uygulama hata ayıklama modu özelliğini ayarlamak için
1. Service Fabric uygulama projenin (*.sfproj) üzerinde kısayol menüsünü seçin **özellikleri** (veya basın **F4** anahtar).
2. İçinde **özellikleri** penceresinde **uygulama hata ayıklama modu** özelliği.

![Uygulama hata ayıklama modu özelliğini ayarlayın][debugmodeproperty]

#### <a name="application-debug-modes"></a>Uygulama hata ayıklama modu

1. **Aktualizovat Aplikaci** bu modu hızlı bir şekilde değiştirin ve hata ayıklama sırasında statik web dosyaları düzenleme destekler ve kod hatalarını ayıklama olanak tanır. Bu mod yalnızca, yerel geliştirme kümenizin 1 düğümlü moddaysa çalışır. Varsayılan uygulama hata ayıklama modu budur.
2. **Uygulamayı kaldırma** uygulama hata ayıklama oturumu sona erdiğinde kaldırılmasına neden olur.
3. **Otomatik yükseltme** uygulama hata ayıklama oturumu sona erdiğinde çalışmaya devam eder. Sonraki hata ayıklama oturumunda dağıtım yükseltme olarak değerlendirir. Yükseltme işlemi önceki bir hata ayıklama oturumunda girdiğiniz tüm veriler korunur.
4. **Uygulamanın** hata ayıklama oturumu sona erdiğinde, uygulamanın kümede çalışmaya devam eder. Sonraki hata ayıklama oturumunun başlangıcında, uygulama kaldırılacak.

İçin **otomatik yükseltme** veriler, Service Fabric uygulaması yükseltme yeteneklerini uygulayarak korunur. Uygulamalar ve nasıl gerçek bir ortamda bir yükseltme gerçekleştirmek isteyebileceğiniz yükseltme hakkında daha fazla bilgi için bkz. [Service Fabric uygulaması yükseltme](service-fabric-application-upgrade.md).

## <a name="add-a-service-to-your-service-fabric-application"></a>Service Fabric uygulamanızı hizmet ekleme
Yeni hizmetler işlevselliği genişletmek için uygulamanıza ekleyebilirsiniz. Hizmet, uygulama paketinde yer aldığından emin olun, hizmeti aracılığıyla eklemek **yeni Fabric hizmeti...**  menü öğesi.

![Yeni Service Fabric hizmet ekleme][newservice]

Uygulamanıza eklemek için Service Fabric projesi türünü seçin ve hizmet için bir ad belirtin.  Bkz: [hizmetiniz için bir çerçeve seçme](service-fabric-choose-framework.md) kullanmak için hangi hizmet türü karar vermenize yardımcı olacak.

![Uygulamanıza eklemek için bir Service Fabric hizmeti proje türü seçin][addserviceproject]

Yeni hizmet, çözüm ve mevcut uygulama paketi eklenir. Hizmet başvuruları ve varsayılan hizmet örneği, oluşturulması ve başlatılması sonraki sefer uygulamayı dağıtmak hizmetin neden uygulama bildirimine eklenir.

![Yeni hizmet, uygulama bildirimine eklenir][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Service Fabric uygulamanızı paketleme
Uygulama ve hizmetlerinin bir kümeye dağıtmak için bir uygulama paketini oluşturmak gerekir.  Paket, uygulama bildirimi, hizmet bildirimleri ve diğer gerekli dosyaları belirli bir düzende düzenler.  Visual Studio, ayarlar ve paket uygulama proje klasöründe 'pkg' dizini yönetir.  Tıklayarak **paket** gelen **uygulama** bağlam menüsü oluşturur veya uygulama paketini güncelleştirir.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Uygulamalar ve uygulama türleri bulut Gezgini'ni kullanarak kaldırma
Temel küme yönetimi işlemleri başlatmak Cloud Explorer'ı kullanarak Visual Studio içinden gerçekleştirebilir **görünümü** menüsü. Örneğin, uygulamaları silin ve uygulama türlerinde yerel veya uzak kümeleri sağlama.

![Uygulamayı kaldırma][removeapplication]

> [!TIP]
> Görmek için daha zengin bir küme yönetimi işlevselliği, [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md).
>
>

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric uygulama modeli](service-fabric-application-model.md)
* [Service Fabric uygulama dağıtımı](service-fabric-deploy-remove-applications.md)
* [Birden çok ortam için uygulama parametrelerini yönetme](service-fabric-manage-multiple-environment-app-configuration.md)
* [Service Fabric uygulamanızı hata ayıklama](service-fabric-debugging-your-application.md)
* [Service Fabric Explorer kullanarak kümenizi Görselleştirme](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png
