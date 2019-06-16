---
title: Azure Service Fabric kapsayıcı uygulaması bildirim örnekleri | Microsoft Docs
description: Uygulama yapılandırma ve hizmet Service Fabric uygulaması için bildirim ayarları hakkında bilgi edinin.
services: service-fabric
documentationcenter: na
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: xml
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: 85a3066095cfc30da19b06d26f41bdc156f85832
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60718232"
---
# <a name="service-fabric-application-and-service-manifest-examples"></a>Service Fabric uygulaması ve hizmet örnekleri listesi
Bu bölümde, uygulama ve hizmet bildirimleri örnekleri içerir. Bu örnekler, önemli senaryoları göstermek için ancak mevcut olan farklı ayarlar ve bunları nasıl kullanacağınızı gösterecek şekilde değildir. 

Aşağıda gösterilen özelliklerin bir dizini ve örnek manifest(s) oldukları bir parçası.

|Özellik|Bildirimi|
|---|---|
|[Kaynak idaresi](service-fabric-resource-governance.md)|[Reliable Services uygulaması bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest), [kapsayıcılı bir uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Bir hizmet bir yerel yönetici farklı çalıştır hesabı](service-fabric-application-runas-security.md)|[Reliable Services uygulaması bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Tüm hizmet kod paketleri için varsayılan ilke geçerlidir](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages)|[Reliable Services uygulaması bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Kullanıcı ve grup ilkeleri oluşturma](service-fabric-application-runas-security.md)|[Reliable Services uygulaması bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|bir veri paketi hizmet örnekleri arasında paylaşma|[Reliable Services uygulaması bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Hizmet uç noktaları geçersiz kıl](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)|[Reliable Services uygulaması bildirimi](service-fabric-manifest-example-reliable-services-app.md#application-manifest)|
|[Hizmetin başlatılması sırasında bir betik çalıştırma](service-fabric-run-script-at-service-startup.md)|[VotingWeb hizmeti bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Bir HTTPS uç noktası tanımlama](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest)|[VotingWeb hizmeti bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingweb-service-manifest)|
|[Yapılandırma paketi bildirme](service-fabric-application-and-service-manifests.md)|[VotingData hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Bir veri paketi bildirme](service-fabric-application-and-service-manifests.md)|[VotingData hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|[Ortam değişkenlerini geçersiz kılar](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Kapsayıcı uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Kapsayıcı bağlantı noktasından konak eşlemeyi yapılandırma](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery)| [Kapsayıcı uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırma](service-fabric-get-started-containers.md#configure-container-registry-authentication)|[Kapsayıcı uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Yalıtım modunu ayarlama](service-fabric-get-started-containers.md#configure-isolation-mode)|[Kapsayıcı uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[İşletim sistemi derleme özgü kapsayıcı görüntüleri belirtme](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)|[Kapsayıcı uygulama bildirimi](service-fabric-manifest-example-container-app.md#application-manifest)|
|[Ortam değişkenlerini ayarlama](service-fabric-get-started-containers.md#configure-and-set-environment-variables)|[Kapsayıcı FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [kapsayıcı BackEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|
|[Bir uç nokta yapılandırma](service-fabric-get-started-containers.md#configure-communication)|[Kapsayıcı FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest), [kapsayıcı BackEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#backendservice-service-manifest), [VotingData hizmet bildirimi](service-fabric-manifest-example-reliable-services-app.md#votingdata-service-manifest)|
|kapsayıcıya komutları geçirin|[Kapsayıcı FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Sertifika bir kapsayıcıya alma](service-fabric-securing-containers.md)|[Kapsayıcı FrontEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#frontendservice-service-manifest)|
|[Birim sürücüsü yapılandırma](service-fabric-containers-volume-logging-drivers.md)|[Kapsayıcı BackEndService hizmet bildirimi](service-fabric-manifest-example-container-app.md#backendservice-service-manifest)|

