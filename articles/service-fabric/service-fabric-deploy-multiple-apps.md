---
title: Azure Service Fabric için MongoDB kullanan bir Node.js uygulaması dağıtma | Microsoft Docs
description: Bir Azure Service Fabric kümesine dağıtmak için birden fazla konuk tarafından yürütülebilir uygulama paketini nasıl yönergeleri
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: chackdan
editor: ''
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 7fb4c68d10478a7c8af62262b3fa4633eaac9d2b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660417"
---
# <a name="deploy-multiple-guest-executables"></a>Konuk tarafından yürütülebilir birden çok uygulama dağıtma
Bu makalede, paketleyin ve birden fazla Konuk yürütülebilir dosyaları Azure Service Fabric'e dağıtma gösterilmektedir. Oluşturmak ve tek bir Service Fabric paket dağıtımı için okuma nasıl için [Konuk yürütülebilir dosyası, Service Fabric'e dağıtma](service-fabric-deploy-existing-app.md).

Bu izlenecek yolda veri deposu olarak MongoDB kullanan bir Node.js ön ucuna sahip bir uygulamanın nasıl dağıtılacağı gösterir, ancak başka bir uygulama üzerinde bağımlılıkları olan herhangi bir uygulama adımları uygulayabilirsiniz.   

Visual Studio, birden fazla Konuk yürütülebilir dosyaları içeren uygulama paketini oluşturmak için kullanabilirsiniz. Bkz: [var olan bir uygulamayı paketlemek için Visual Studio kullanarak](service-fabric-deploy-existing-app.md). İlk Konuk yürütülebilir dosyası ekledikten sonra uygulama projesine sağ tıklayın ve seçin **Ekle -> Yeni Service Fabric hizmeti** ikinci Konuk yürütülebilir projeyi çözüme eklemek için. Not: Visual Studio projesini kaynak bağlamak isterseniz, Visual Studio çözümü uygulama paketinizi değişiklikleri kaynak güncel olduğundan emin. 

## <a name="samples"></a>Örnekler
* [Paketleme ve dağıtma Konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a>El ile birden fazla konuk tarafından yürütülebilir uygulama paketi
Alternatif olarak el ile Konuk yürütülebilir dosyası paketleyebilirsiniz. El ile paketleme için bu makalede bulunabilir Service Fabric paketleme aracı kullanır. [ http://aka.ms/servicefabricpacktool ](https://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Node.js uygulaması paketleme
Bu makalede, Node.js, Service Fabric kümesindeki düğümlere yüklenmedi varsayılır. Sonuç olarak node.js uygulamanızı paketleme önce kök dizinine Node.exe eklemeniz gerekir. Dizin yapısı (Express web çerçevesini ve Jade şablon altyapısı kullanılarak) Node.js uygulaması, aşağıdaki gibi görünmelidir:

```
|-- NodeApplication
    |-- bin
        |-- www
    |-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
    |-- public
        |-- images
        |-- etc.
    |-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

Sonraki adım olarak, bir Node.js uygulaması için uygulama paketi oluşturun. Aşağıdaki kod, Node.js uygulaması içeren bir Service Fabric uygulama paketi oluşturur.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Kullanılmayan parametreleri açıklaması aşağıda verilmiştir:

* **/ source** paketlenmesi gereken uygulamanın dizinine işaret eder.
* **/ target** , paketin oluşturulacağı dizin tanımlar. Bu dizin, kaynak dizinden farklı olmak zorundadır.
* **yüklemede** var olan uygulamayı uygulama adını tanımlar. Bu bildirimde hizmet adını ve Service Fabric uygulama adı için dönüşür anlamak önemlidir.
* **/ exe** Service Fabric, bu durumda başlatmak için gereken yürütülebilir tanımlar `node.exe`.
* **/ma** yürütülebilir dosyayı başlatmak için kullanılan bağımsız değişkeni tanımlar. Node.js yüklü değil gibi Service Fabric yürüterek Node.js web sunucusunu başlatmak gereken `node.exe bin/www`.  `/ma:'bin/www'` paketleme aracını kullanmak için bildiren `bin/www` node.exe için bağımsız değişken olarak.
* **/ Apptype özelliğine** Service Fabric uygulama türü adını tanımlar.

/ Target parametresinde belirtilen dizine göz atarsanız, aşağıda gösterildiği gibi bir araç tam olarak işlevsel bir Service Fabric paket oluşturdu görebilirsiniz:

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Oluşturulan ServiceManifest.xml şimdi nasıl Node.js web sunucusunun başlatılması gerekir, aşağıdaki kod parçacığında gösterildiği gibi açıklayan bir bölümü vardır:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
Aşağıda gösterildiği gibi ServiceManifest.xml dosyasındaki uç nokta bilgileri güncelleştirmeye gerek duyduğunuz şekilde bu örnekte Node.js web sunucusu bağlantı noktası 3000 dinler.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>MongoDB Uygulama paketleme
Node.js uygulaması paketlediğinizden, devam edin ve MongoDB paket. Daha önce belirtildiği gibi artık Git aracılığıyla adımlar Node.js ve MongoDB özgü değildir. Aslında, bir Service Fabric uygulaması olarak birlikte paketlenmesi gereken tüm uygulamalara uygulanır.  

MongoDB paketlemek için Mongod.exe ve Mongo.exe paketini emin olmanız gerekir. Her iki ikili dosyaları bulunur `bin` MongoDB yükleme dizininin dizin. Dizin yapısı aşağıdaki gibi görünür.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric gereken MongoDB benzer bir komut aşağıdaki başlatmak kullanmanız gereken şekilde `/ma` MongoDB paketlenirken parametresi.

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> MongoDB veri dizini düğümünün yerel dizinde koyarsanız veri olması durumunda bir düğümde hata oluştuktan korunuyor değil. Dayanıklı depolama kullanabilir veya bir MongoDB çoğaltma veri kaybını önlemek için uygulama.  
>
>

PowerShell veya komut kabuğu, biz paketleme aracı aşağıdaki parametrelerle çalıştırın:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

MongoDB, Service Fabric uygulama paketini eklemek için uygulamayı içeren aynı dizinde / target parametre noktalarına yanı sıra Node.js uygulaması bildirim emin olmanız gerekir. ApplicationType adın aynısını kullanarak emin olmanız gerekir.

Şimdi dizinine göz atın ve hangi aracın oluşturduğu inceleyin.

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Gördüğünüz gibi aracı yeni bir klasör, MongoDB, MongoDB ikili dosyaları içeren dizine eklenir. Açarsanız `ApplicationManifest.xml` dosyasını, paket şimdi Node.js uygulama ve MongoDB içerdiğini görebilirsiniz. Aşağıdaki kod, uygulama bildiriminin içeriği gösterir.

```xml
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Uygulama yayımlama
PowerShell komut dosyalarını kullanarak yerel bir Service Fabric kümesine uygulama yayımlamak için son adımdır bakın:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Uygulamayı yerel kümeye başarıyla yayımlandığında, Node.js uygulaması--örneğin http hizmet bildiriminde girdiğimiz bağlantı noktası üzerinde Node.js uygulaması erişebilirsiniz:\//localhost:3000.

Bu öğreticide, bir Service Fabric uygulaması olarak iki mevcut uygulamaları kolayca paketlenecek öğrendiniz. Ayrıca bazı Service Fabric özellikleri, yüksek kullanılabilirlik ve sistem durumu sistem tümleştirmesi gibi yararlanabilir, böylece Service Fabric'e dağıtma gerçekleştirmeyi öğrendiniz.


## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a>Daha fazla Konuk yürütülebilir dosyaları, Linux üzerinde Yeoman kullanarak var olan bir uygulamaya ekleme

`yo` kullanılarak oluşturulmuş bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın: 
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. Çalıştırma `yo azuresfguest:AddService` ve gerekli ayrıntıları belirtin.

## <a name="next-steps"></a>Sonraki adımlar
* Kapsayıcılar ile dağıtma hakkında bilgi edinin [Service Fabric ve kapsayıcılar genel bakış](service-fabric-containers-overview.md)
* [Paketleme ve dağıtma Konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-containers)
