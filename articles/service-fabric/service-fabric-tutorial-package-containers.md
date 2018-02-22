---
title: "Service Fabric kapsayıcıları uygulaması paketleme ve dağıtma | Microsoft Belgeleri"
description: "Yeoman kullanarak Azure Service Fabric uygulaması tanımı oluşturmayı ve uygulamayı paketlemeyi öğrenin."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker, Kapsayıcılar, Mikro Hizmetler, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: eb838903802de5a04084a60924fc52d988180c11
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="package-and-deploy-containers-as-a-service-fabric-application"></a>Kapsayıcıları Service Fabric uygulaması olarak paketleme ve dağıtma

Bu öğretici, bir dizinin ikinci bölümüdür. Bu öğreticide, bir şablon oluşturma aracı (Yeoman) kullanılarak bir Service Fabric uygulaması tanımı oluşturulmaktadır. Daha sonra bu uygulama Service Fabric’e kapsayıcı dağıtmak için kullanılabilir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 

> [!div class="checklist"]
> * Yeoman’ı yükleme  
> * Yeoman’ı kullanarak bir uygulama paketi oluşturma
> * Uygulama paketinin ayarlarını kapsayıcılarla kullanıma uygun şekilde yapılandırma
> * Uygulama oluşturma  
> * Uygulamayı dağıtma ve çalıştırma 
> * Uygulamayı temizleme

## <a name="prerequisites"></a>Ön koşullar

- Bu öğretici serisinin [1. Bölümünde](service-fabric-tutorial-create-container-images.md) oluşturulup Azure Container Registry’ye gönderilen kapsayıcı görüntüleri kullanılır.
- Linux geliştirme ortamı [ayarlanmıştır](service-fabric-tutorial-create-container-images.md).

## <a name="install-yeoman"></a>Yeoman’ı yükleme
Service Fabric, Yeoman şablon oluşturucu kullanarak terminalden uygulama oluşturmaya yardımcı olacak yapı iskelesi araçları sağlar. Yeoman şablon oluşturucunuzun olduğundan emin olmak için aşağıdaki adımları izleyin. 

1. Makinenize nodejs ve NPM’yi yükleyin. Mac OSX kullanıcılarının Homebrew paket yöneticisi'ni kullanması gerekir

    ```bash
    sudo apt-get install npm && sudo apt install nodejs-legacy
    ```
2. NPM’den makinenize Yeoman şablon oluşturucuyu yükleyin 

    ```bash
    sudo npm install -g yo
    ```
3. Service Fabric Yeoman kapsayıcı oluşturucuyu yükleyin

    ```bash 
    sudo npm install -g generator-azuresfcontainer
    ```

## <a name="package-a-docker-image-container-with-yeoman"></a>Yeoman ile Docker görüntü kapsayıcısını paketleme

1. Kopyalanan deponun “container-tutorial” dizininde Service Fabric kapsayıcı uygulamasını oluşturmak için aşağıdaki komutu çalıştırın.

    ```bash
    yo azuresfcontainer
    ```
2. Uygulamanıza bir ad vermek için lütfen “TestContainer” yazın
3. Uygulama hizmetinize bir ad vermek için lütfen “azurevotefront” yazın.
4. Ön uç deposu için ACR’deki kapsayıcı görüntüsü yolu sağlayın. Örnek: “\<acrName>.azurecr.io/azure-vote-front:v1”. \<acrName> alanı, önceki öğreticide kullanılan değerle aynı olmalıdır.
5. Komutlar bölümünü boş bırakmak için Enter tuşuna basın.
6. Örnek sayısı olarak 1 belirtin.

Aşağıda yo komutunu çalıştırmanın girişi ve çıktısı gösterilmektedir:

```bash
? Name your application TestContainer
? Name of the application service: azurevotefront
? Input the Image Name: <acrName>.azurecr.io/azure-vote-front:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/TestContainer/ApplicationManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/ServiceManifest.xml
   create TestContainer/TestContainer/azurevotefrontPkg/config/Settings.xml
   create TestContainer/TestContainer/azurevotefrontPkg/code/Dummy.txt
   create TestContainer/install.sh
   create TestContainer/uninstall.sh
```

Yeoman kullanılarak oluşturulmuş olan bir uygulamaya başka bir kapsayıcı hizmeti eklemek için aşağıdaki adımları uygulayın:

1. Dizini bir düzey değiştirerek **TestContainer** dizinine gidin. Örnek: *./TestContainer*
2. `yo azuresfcontainer:AddService` öğesini çalıştırın 
3. Hizmete “azurevoteback” adını verin
4. Redis için kapsayıcı görüntüsü yolunu sağlayın: “alpine:redis”
5. Komutlar bölümünü boş bırakmak için Enter tuşuna basın
6. "1" örnek sayısı belirtin.

Kullanılan hizmeti eklemeye yönelik girişlerin tümü aşağıda gösterilmektedir:

```bash
? Name of the application service: azurevoteback
? Input the Image Name: alpine:redis
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/azurevotebackPkg/ServiceManifest.xml
   create TestContainer/azurevotebackPkg/config/Settings.xml
   create TestContainer/azurevotebackPkg/code/Dummy.txt
```

Bu öğreticinin geri kalanında **TestContainer** dizininde çalışacağız. Örneğin, *./TestContainer/TestContainer*. Bu dizinde aşağıdakiler bulunmalıdır.
```bash
$ ls
ApplicationManifest.xml azurevotefrontPkg azurevotebackPkg
```

## <a name="configure-the-application-manifest-with-credentials-for-azure-container-registry"></a>Uygulama bildirimini Azure Container Registry’ye ait kimlik bilgileriyle yapılandırın
Service Fabric’in kapsayıcı görüntülerini Azure Container Registry’den çekebilmesi için **ApplicationManifest.xml** dosyasında kimlik bilgilerini sağlamamız gerekir. 

ACR örneğinizde oturum açın. İşlemi tamamlamak için **az acr login** komutunu kullanın. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz adı sağlayın.

```bash
az acr login --name <acrName>
```

Komut tamamlandığında bir **Oturum Başarıyla Açıldı** iletisi döndürür.

Ardından, kapsayıcı kayıt defterinizin parolasını almak için aşağıdaki komutu çalıştırın. Bu parola, kapsayıcı görüntülerini çekerken ACR’de kimlik doğrulamak için Service Fabric tarafından kullanılır.

```bash
az acr credential show -n <acrName> --query passwords[0].value
```

**ApplicationManifest.xml** dosyasında ön uç hizmetinin **ServiceManifestImport** öğesinin altına kod parçacığını ekleyin. **AccountName** alanına kendi **acrName** değerinizi ve **Password** alanına bir önceki komuttan döndürülen parolayı girin. Bu belgenin sonunda tam bir **ApplicationManifest.xml** verilmiştir. 

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="<acrName>" Password="<password>" PasswordEncrypted="false"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>İletişim ve kapsayıcı bağlantı noktalarıyla konak bağlantı noktalarını eşlemeyi yapılandırın

### <a name="configure-communication-port"></a>İletişim bağlantı noktasını yapılandırma

İstemcilerin hizmetinizle iletişim kurabilmesi için bir HTTP uç noktası yapılandırın. *./TestContainer/azurevotefrontPkg/ServiceManifest.xml* dosyasını açın ve **ServiceManifest** öğesinde bir uç nokta kaynağı belirtin.  Protokolü, bağlantı noktasını ve adını ekleyin. Bu öğreticide hizmet, bağlantı noktası 80’i dinlemektedir. Aşağıdaki kod parçacığı, kaynakta *ServiceManifest* etiketinin altına yerleştirilir.
  
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
  </Endpoints>
</Resources>

```
  
Benzer şekilde, arka uç hizmeti için Hizmet Bildirimi’ni (Service Manifest) değiştirin. *./TestContainer/azurevotebackPkg/ServiceManifest.xml* dosyasını açın ve **ServiceManifest** öğesinde bir uç nokta kaynağı belirtin. Bu öğreticide, redis için varsayılan değer olan 6379 kullanılmaktadır. Aşağıdaki kod parçacığı, kaynakta *ServiceManifest* etiketinin altına yerleştirilir.

```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
  </Endpoints>
</Resources>
```
**UriScheme** değerinin sağlanması, kapsayıcı uç noktasını bulunabilirlik için Service Fabric Adlandırma hizmetine otomatik olarak kaydeder. Bu makalenin sonunda örnek olarak arka uç hizmetine yönelik tam bir ServiceManifest.xml örnek dosyası verilmiştir. 

### <a name="map-container-ports-to-a-service"></a>Kapsayıcı bağlantı noktalarını bir hizmete eşleme
Kümedeki kapsayıcıları kullanıma sunmak için “ApplicationManifest.xml” dosyasında bir bağlantı noktası bağlaması oluşturulması da gerekir. **PortBinding** ilkesi **ServiceManifest.xml** dosyalarında tanımladığımız **Uç Noktalarına (Endpoints)** başvurur. Bu uç noktalarına gelen istekler, burada açılan ve bağlanan kapsayıcı bağlantı noktalarına eşlenir. 80 ve 6379 numaralı bağlantı noktalarını uç noktalarına bağlamak için **ApplicationManifest.xml** dosyasına aşağıdaki kodu ekleyin. Bu belgenin sonunda tam bir **ApplicationManifest.xml** verilmiştir. 
  
```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
</ContainerHostPolicies>
```

```xml
<ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
</ContainerHostPolicies>
```

### <a name="add-a-dns-name-to-the-backend-service"></a>Arka uç hizmetine bir DNS adı ekleme
  
Service Fabric’in bu DNS adını arka uç hizmetine atayabilmesi için söz konusu adın **ApplicationManifest.xml** dosyasında belirtilmesi gerekir. Aşağıda gösterildiği gibi **Service** öğesine **ServiceDnsName** özniteliğini ekleyin: 
  
```xml
<Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
  <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
    <SingletonPartition/>
  </StatelessService>
</Service>
```

Ön uç hizmeti, Redis örneğinin DNS adını öğrenmek için bir ortam değişkenini okur. Bu ortam değişkeni, Docker görüntüsünü oluşturmak için kullanılan Dockerfile’da zaten tanımlanmıştır ve burada herhangi bir işlem yapılmasına gerek yoktur.
  
```Dockerfile
ENV REDIS redisbackend.testapp
```
  
Aşağıdaki kod parçacığı, ön uç Python kodunun Dockerfile’da belirtilen ortam değişkenini nasıl aldığını göstermektedir. Burada herhangi bir işlem yapılmasına gerek yoktur. 

```python
# Get DNS Name
redis_server = os.environ['REDIS'] 

# Connect to the Redis store
r = redis.StrictRedis(host=redis_server, port=6379, db=0)
```

Öğreticinin bu noktasında Service Package uygulamasına yönelik şablon bir kümeye dağıtılmak için hazırdır. Sıradaki öğreticide bu uygulama bir Service Fabric kümesine dağıtılıp burada çalıştırılmaktadır.

## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluşturma
Uygulamayı Azure'daki bir kümeye dağıtmak için kendi kümenizi oluşturun.

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleridir. Service Fabric ekibi tarafından çalıştırılan bu kümelere herkes uygulama dağıtarak platform hakkında bilgi alabilir. Bir Grup Kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric). 

Güvenli toplu kümede yönetimi işlemleri gerçekleştirmek için Service Fabric Explorer, CLI veya Powershell’i kullanabilirsiniz. Service Fabric Explorer’ı kullanmak için, PFX dosyasını Toplu Küme Web sitesinden indirmeniz ve sertifikayı sertifika deponuza (Windows veya Mac) veya tarayıcının kendisine (Ubuntu) aktarmanız gerekir. Toplu kümeden gelen otomatik olarak imzalanan sertifikaların parolası yoktur. 

Powershell veya CLI ile yönetim işlemleri gerçekleştirmek için PFX (Powershell) veya PEM (CLI) gerekir. PFX dosyasını PEM dosyasına dönüştürmek için lütfen aşağıdaki komutu çalıştırın:  

```bash
openssl pkcs12 -in party-cluster-1277863181-client-cert.pfx -out party-cluster-1277863181-client-cert.pem -nodes -passin pass:
```

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da Service Fabric kümesi oluşturma](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

## <a name="build-and-deploy-the-application-to-the-cluster"></a>Uygulamayı oluşturup kümeye dağıtma
Uygulamayı, Service Fabric CLI’yi kullanarak Azure kümesine dağıtabilirsiniz. Service Fabric CLI makinenizde yüklü değilse [buradaki](service-fabric-get-started-linux.md#set-up-the-service-fabric-cli) yönergeleri uygulayarak yükleyin. 

Azure’daki Service Fabric kümesine bağlanın. Yer tutucu uç noktayı kendi uç noktanız ile değiştirin. Uç nokta, aşağıdaki gibi tam bir URL olmalıdır.

```bash
sfctl cluster select --endpoint https://linh1x87d1d.westus.cloudapp.azure.com:19080 --pem party-cluster-1277863181-client-cert.pem --no-verify
```

**TestContainer** dizininde sağlanan yükleme betiğini kullanarak uygulama paketini kümenin görüntü deposuna kopyalayın, uygulama türünü kaydedin ve uygulamanın bir örneğini oluşturun.

```bash
./install.sh
```

Bir tarayıcı penceresi açın ve http://lin4hjim3l4.westus.cloudapp.azure.com:19080/Explorer adresindeki Service Fabric Explorer’a gidin. Uygulamalar düğümünü genişletin ve uygulamanızın türü için bir giriş ve örnek için başka bir giriş olduğuna dikkat edin.

![Service Fabric Explorer][sfx]

Çalışan uygulamaya bağlanmak için bir web tarayıcısı açın ve kümenin URL’sine gidin. Örnek: http://lin0823ryf2he.cloudapp.azure.com:80. Web kullanıcı arabiriminde Voting (Oylama) uygulamasını görmeniz gerekir.

![votingapp][votingapp]

## <a name="clean-up"></a>Temizleme
Kümeden uygulama örneğini silmek ve uygulama türünün kaydını silmek için şablonda sağlanan kaldırma betiğini kullanın. Bu komutun örneği temizlemesi zaman alır ve “install.sh” komutu bu betiğin hemen ardından çalıştırılamaz. 

```bash
./uninstall.sh
```

## <a name="examples-of-completed-manifests"></a>Tamamlanan bildirimlerin örnekleri

### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<ApplicationManifest ApplicationTypeName="TestContainerType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotefrontPkg" ServiceManifestVersion="1.0.0"/>
    <Policies> 
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="azurevotefrontTypeEndpoint"/>
    </ContainerHostPolicies>
        </Policies>
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="azurevotebackPkg" ServiceManifestVersion="1.0.0"/>
      <Policies> 
        <ContainerHostPolicies CodePackageRef="Code">
          <PortBinding ContainerPort="6379" EndpointRef="azurevotebackTypeEndpoint"/>
        </ContainerHostPolicies>
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="azurevotefront">
      <StatelessService ServiceTypeName="azurevotefrontType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
    <Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
      <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
        <SingletonPartition/>
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

### <a name="front-end-servicemanifestxml"></a>Ön uç ServiceManifest.xml 
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotefrontPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotefrontType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>acrName.azurecr.io/azure-vote-front:v1</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>

 </ServiceManifest>
```

### <a name="redis-servicemanifestxml"></a>Redis ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="azurevotebackPkg" Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" >

   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="azurevotebackType" UseImplicitHost="true">
   </StatelessServiceType>
   </ServiceTypes>
   
   <CodePackage Name="code" Version="1.0.0">
      <EntryPoint>
         <ContainerHost>
            <ImageName>alpine:redis</ImageName>
            <Commands></Commands>
         </ContainerHost>
      </EntryPoint>
      <EnvironmentVariables> 
      </EnvironmentVariables> 
   </CodePackage>
     <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="azurevotebackTypeEndpoint" Port="6379" Protocol="tcp"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>
```
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Yeoman kullanılarak birden çok kapsayıcı bir Service Fabric uygulaması şeklinde paketlendi. Ardından, bu uygulama bir Service Fabric kümesine dağıtılıp burada çalıştırıldı. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Yeoman’ı yükleme  
> * Yeoman’ı kullanarak bir uygulama paketi oluşturma
> * Uygulama paketinin ayarlarını kapsayıcılarla kullanıma uygun şekilde yapılandırma
> * Uygulama oluşturma  
> * Uygulamayı dağıtma ve çalıştırma 
> * Uygulamayı temizleme

Service Fabric’te uygulamaya yönelik yük devretme ve ölçeklendirme hakkında bilgi edinmek için sıradaki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Uygulamalar için yük devretme ve ölçeklendirme hakkında bilgi edinin](service-fabric-tutorial-containers-failover.md)

[votingapp]: ./media/service-fabric-tutorial-deploy-run-containers/votingapp.png
[sfx]: ./media/service-fabric-tutorial-deploy-run-containers/containerspackagetutorialsfx.png


