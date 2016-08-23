<properties
   pageTitle="Geliştirme ortamınızı ayarlama | Microsoft Azure"
   description="Çalışma zamanını, SDK'yı ve araçları yükleyip yerel bir geliştirme kümesi oluşturun. Bu kurulumu tamamladıktan sonra uygulama derlemek için hazır hale gelirsiniz."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/15/2016"
   ms.author="ryanwi"/>

# Geliştirme ortamınızı hazırlama
 Geliştirme makinenizde [Azure Service Fabric uygulamaları][1] derlemek ve çalıştırmak için çalışma süresini, SDK'yı ve araçları yüklemeniz gerekir. Ayrıca, SDK'da bulunan Windows PowerShell betiklerinin çalıştırılmasını da etkinleştirmeniz gerekir.

## Önkoşullar
### Desteklenen işletim sistemi sürümleri
Geliştirme için şu işletim sistemi sürümleri desteklenir:

- Windows 7
- Windows 8/Windows 8.1
- Windows Server 2012 R2
- Windows 10

>[AZURE.NOTE] Windows 7 varsayılan olarak yalnızca Windows PowerShell 2.0 içerir. Service Fabric PowerShell cmdlet'lerini kullanmak için PowerShell 3.0 sürümünü veya sonraki bir sürümü yüklemeniz gerekir. Microsoft Yükleme Merkezi'nden [Windows PowerShell 5.0'ı indirebilirsiniz][powershell5-download].

## Çalışma zamanını, SDK'yı ve araçları yükleme

Web Platformu Yükleyicisi, Service Fabric geliştirmesi için üç yapılandırma seçeneği sunar:

- [Visual Studio 2015 Güncelleştirme 2 için Service Fabric çalışma zamanı, SDK ve araçları yükleme][full-bundle-vs2015]
- [Visual Studio "15" için Service Fabric çalışma zamanını, SDK'yı ve araçları yükleyin][full-bundle-dev15]
- [Yalnızca Service Fabric çalışma zamanını ve SDK'yı yükleyin (Visual Studio araçları olmadan) ][core-sdk]


## PowerShell betik yürütmesini etkinleştirme

Service Fabric, yerel geliştirme merkezi oluşturmak ve Visual Studio'dan uygulamaları dağıtmak için Windows PowerShell betiklerini kullanır. Varsayılan olarak, Windows bu betiklerin çalışmasını engeller. Betikleri etkinleştirmek için PowerShell yürütme ilkenizi değiştirmeniz gerekir. PowerShell'i yönetici olarak açın ve şu komutu girin:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## Sonraki adımlar
Artık geliştirme ortamınız ayarlandığına göre uygulama derlemeye ve çalıştırmaya başlayabilirsiniz.

- [Visual Studio'da ilk Service Fabric uygulamanızı oluşturma](service-fabric-create-your-first-application-in-visual-studio.md)
- [Yerel kümenizde uygulamaları dağıtmayı ve yönetmeyi öğrenin](service-fabric-get-started-with-a-local-cluster.md)
- [Programlama modelleri hakkında bilgi edinin: Reliable Services ve Reliable Actors](service-fabric-choose-framework.md)
- [GitHub'da Service Fabric kod örneklerine bakın](https://aka.ms/servicefabricsamples)
- [Service Fabric Explorer kullanarak kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)
- [Platforma yönelik daha geniş kapsamlı bilgi edinmek için Service Fabric öğrenme yolunu uygulayın](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Service Fabric kampanya sayfası"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015-2_1 "VS 2015 WebPI bağlantısı"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15-2_1 "Dev15 WebPI bağlantısı"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ServiceFabricSDK_2_1 "Core SDK WebPI bağlantısı"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395



<!--HONumber=Aug16_HO1-->


