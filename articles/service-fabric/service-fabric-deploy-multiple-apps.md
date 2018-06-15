---
title: Azure Service Fabric MongoDB kullanan bir Node.js uygulaması dağıtma | Microsoft Docs
description: Bir Azure Service Fabric kümesi dağıtmak için birden çok Konuk yürütülebilir dosyalar paketini konusunda gözden geçirme
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: ''
ms.assetid: b76bb756-c1ba-49f9-9666-e9807cf8f92f
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 9a7ab3881cd1058a60ff7d5f6e50c296f042e76e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206088"
---
# <a name="deploy-multiple-guest-executables"></a>Konuk tarafından yürütülebilir birden çok uygulama dağıtma
Bu makalede, paket ve birden çok Konuk yürütülebilir dosyalar için Azure Service Fabric dağıtma gösterilmektedir. Derleme ve tek bir Service Fabric paketi dağıtmak için okuma nasıl için [yürütülebilir Konuk dağıtmak için Service Fabric](service-fabric-deploy-existing-app.md).

Bu kılavuz, MongoDB veri deposu olarak kullanan bir Node.js ön ucuna sahip bir uygulamanın nasıl dağıtılacağı gösterilmektedir, ancak başka bir uygulama üzerinde bağımlılıklara sahip herhangi bir uygulama adımları uygulayabilirsiniz.   

Birden çok Konuk yürütülebilir dosyaları içeren uygulama paketini oluşturmak için Visual Studio'yu kullanabilirsiniz. Bkz: [var olan bir uygulama paketi için Visual Studio'yu kullanarak](service-fabric-deploy-existing-app.md). İlk Konuk yürütülebilir ekledikten sonra uygulama projesine sağ tıklayın ve seçin **Ekle -> Yeni Service Fabric hizmet** ikinci Konuk yürütülebilir proje çözüme eklemek için. Not: Visual Studio projesini kaynak bağlamayı seçerseniz, Visual Studio çözümü oluşturma uygulama paketinizi kaynağındaki değişikliklerle güncel olduğundan emin olun. 

## <a name="samples"></a>Örnekler
* [Paketleme ve dağıtma bir konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a>El ile birden çok Konuk yürütülebilir uygulama paketi
Bunun yerine el ile yürütülebilir Konuk paketleyebilirsiniz. El ile paketleme için bu makalede şu adresten edinilebilir Service Fabric paketleme aracı kullanan [ http://aka.ms/servicefabricpacktool ](http://aka.ms/servicefabricpacktool).

### <a name="packaging-the-nodejs-application"></a>Node.js Uygulama paketleme
Bu makale, Node.js Service Fabric kümesindeki düğümlere yüklü olmadığını varsayar. Sonuç olarak paketlemeden önce düğüm uygulamanızın kök dizinine Node.exe eklemeniz gerekir. (Express web çerçevesi ve Jade şablon motoru kullanarak) Node.js uygulaması dizin yapısını birine benzer görünmelidir:

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

Sonraki adım olarak, Node.js uygulaması için uygulama paketi oluşturun. Aşağıdaki kodu Node.js uygulaması içeren bir Service Fabric uygulama paketi oluşturur.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Kullanılmakta olan parametreleri açıklamasını aşağıdadır:

* **/ source** paketlenmesi uygulama dizinine işaret eder.
* **/ target** içinde paket oluşturulmalıdır dizin tanımlar. Bu dizin kaynak dizinden farklı olması gerekir.
* **Appname** var olan uygulamaya uygulama adını tanımlar. Bu hizmet adı bildiriminde ve değil Service Fabric uygulama adı dönüşür anlamak önemlidir.
* **/ exe** Service Fabric başlatın, bu durumda olması yürütülebilir tanımlar `node.exe`.
* **/ma** yürütülebilir başlatmak için kullanılan bağımsız değişken tanımlar. Node.js yüklenmemiş gibi Service Fabric yürüterek Node.js web sunucusu başlatmak gereken `node.exe bin/www`.  `/ma:'bin/www'` kullanılacak paketleme aracı söyler `bin/www` node.exe bağımsız değişkeni olarak.
* **/ Uygulama türü** Service Fabric uygulaması türü adını tanımlar.

/ Target parametresinde belirtilen dizine göz atarsanız, araç tümüyle işlevsel bir Service Fabric paketi aşağıda gösterildiği gibi oluşturdu görebilirsiniz:

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
Oluşturulan ServiceManifest.xml şimdi nasıl Node.js web sunucusu başlatılması gerekir, aşağıdaki kod parçacığında gösterildiği gibi açıklayan bir bölümü vardır:

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
Aşağıda gösterildiği gibi ServiceManifest.xml dosyasında uç nokta bilgileri güncelleştirmek gereken şekilde bu örnekte, bağlantı noktası 3000, Node.js web sunucusu dinler.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>MongoDB Uygulama paketleme
Node.js uygulaması paketlenmiş, devam edin ve MongoDB paket. Önceden belirtildiği gibi şimdi geçtikleri adımlar Node.js ve MongoDB özel değildir. Aslında, bir Service Fabric uygulaması olarak birlikte paketlenmesi yönelik tüm uygulamalar için geçerlidir.  

MongoDB paketlemek için Mongod.exe ve Mongo.exe paketi emin olmak istersiniz. Her iki ikili dosyaları bulunan `bin` , MongoDB yükleme dizininin dizin. Dizin yapısı birine benzer.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric gereken MongoDB, benzer bir komutla aşağıdaki başlatmak kullanmanız gereken şekilde `/ma` MongoDB paketleme, parametre.

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> Düğümün yerel dizin MongoDB veri dizini yerleştirirseniz veriler bir düğümü hatasına karşı korunmaz. Dayanıklı depolama kullanabilir veya veri kaybını önlemek için ayarlanmış bir MongoDB çoğaltma uygulamak.  
>
>

PowerShell ya da komut kabuğunu biz paketleme aracını aşağıdaki parametrelerle çalıştırın:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Service Fabric uygulama paketinizi MongoDB eklemek için uygulama zaten var. aynı dizine/target parametresi noktaları yanı sıra Node.js uygulama bildirimi emin olmanız gerekir. Ayrıca aynı ApplicationType adı kullandığınızdan emin olmanız gerekir.

Şirketinizdeki dizine gözatın ve aracı oluşturulmuş inceleyin.

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
Gördüğünüz gibi aracı yeni bir klasör, MongoDB, MongoDB ikili dosyaları içeren dizine eklendi. Açarsanız `ApplicationManifest.xml` dosya, paket artık Node.js uygulaması ve MongoDB içerdiğini görebilirsiniz. Aşağıdaki kod, uygulama bildirimi içeriğini gösterir.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
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
PowerShell komut dosyalarını kullanarak yerel Service Fabric kümesi için uygulama yayımlamak için son adımdır bakın:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Uygulamayı yerel kümeye başarıyla yayımlandığında Node.js uygulaması Node.js uygulaması--hizmeti bildiriminde örneğin girmiş olduğunuz bağlantı noktası üzerinde erişebilirsiniz http://localhost:3000.

Bu öğreticide, kolayca bir Service Fabric uygulaması olarak iki uygulamalarınız için nasıl gördünüz. Aynı zamanda yüksek kullanılabilirlik ve sistem durumu sistem tümleştirme gibi Service Fabric özelliklerinden bazıları yararlanabilir böylece Service Fabric dağıtmayı öğrendiniz.


## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a>Yeoman Linux'ta kullanarak var olan bir uygulamaya daha fazla Konuk yürütülebilir dosyaları ekleme

`yo` kullanılarak oluşturulmuş bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın: 
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. Çalıştırma `yo azuresfguest:AddService` ve gerekli ayrıntıları sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
* İle kapsayıcıları dağıtma hakkında bilgi edinin [Service Fabric ve kapsayıcıları genel bakış](service-fabric-containers-overview.md)
* [Paketleme ve dağıtma bir konuk yürütülebilir dosyası için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [İki Konuk yürütülebilir dosyalar (C# ve nodejs) iletişim REST kullanarak Adlandırma Hizmeti örnek](https://github.com/Azure-Samples/service-fabric-containers)
