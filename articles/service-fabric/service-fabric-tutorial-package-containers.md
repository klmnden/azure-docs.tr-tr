---
title: "Paket ve Service Fabric kapsayıcıları uygulama dağıtma | Microsoft Docs"
description: "Yeoman kullanarak Azure Service Fabric uygulama tanımı Oluştur öğrenin ve uygulama paketi."
services: service-fabric
documentationcenter: 
author: suhuruli
manager: timlt
editor: suhuruli
tags: servicefabric
keywords: "Docker, kapsayıcıları, mikro, Service Fabric, Azure"
ms.assetid: 
ms.service: service-fabric
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2017
ms.author: suhuruli
ms.custom: mvc
ms.openlocfilehash: 9b1498d76680185b45edf9ac7e1747bfa6794eec
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="package-and-deploy-containers-as-a-service-fabric-application"></a>Paket ve bir Service Fabric uygulaması olarak kapsayıcıları dağıtın

Bu öğretici iki serisinde bir parçasıdır. Bu öğreticide, bir şablon oluşturma Aracı (Yeoman) bir Service Fabric uygulaması tanımı oluşturmak için kullanılır. Bu uygulama daha sonra Service Fabric kapsayıcıları dağıtmak için kullanılabilir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 

> [!div class="checklist"]
> * Yeoman yükleyin  
> * Yeoman kullanarak bir uygulama paketi oluşturun.
> * Kapsayıcılar ile kullanmak için uygulama paketi ayarlarını yapılandırma
> * Uygulama oluşturma  
> * Dağıtma ve uygulamayı çalıştırma 
> * Uygulamayı oluşturan Temizle

## <a name="prerequisites"></a>Ön koşullar

- Kapsayıcı görüntüleri gönderilen kayıt defterine Azure kapsayıcı içinde oluşturulan [Kısım 1](service-fabric-tutorial-create-container-images.md) Bu öğretici serisi kullanılır.
- Linux geliştirme ortamıdır [ayarlanan](service-fabric-tutorial-create-container-images.md).

## <a name="install-yeoman"></a>Yeoman yükleyin
Service fabric şablon oluşturucu Yeoman kullanarak terminal durumundan uygulamaları oluşturmaya yardımcı olmak için yapı iskelesi araçları sağlar. Yeoman sahip emin olmak için aşağıdaki adımları izleyin şablon oluşturucu. 

1. Nodejs ve NPM makinenize yükleyin. Mac OSX kullanıcıların Paket Yöneticisi'ni Homebrew gerekir, Not

    ```bash
    sudo apt-get install npm && sudo apt install nodejs-legacy
    ```
2. Yeoman yükleme makinenizde NPM şablonu Oluşturucusu 

    ```bash
    sudo npm install -g yo
    ```
3. Service Fabric Yeoman kapsayıcı oluşturucuyu yükleme

    ```bash 
    sudo npm install -g generator-azuresfcontainer
    ```

## <a name="package-a-docker-image-container-with-yeoman"></a>Yeoman ile Docker görüntü kapsayıcısını paketleme

1. Service Fabric kapsayıcısı oluşturmak için uygulama, kopyalanan depo 'kapsayıcı-tutorial' dizinini aşağıdaki komutu çalıştırın.

    ```bash
    yo azuresfcontainer
    ```
2. Uygulamanızı "TestContainer" ve "azurevotefront" uygulama hizmeti adlandırın.
3. Ön uç depoyu için - örneğin 'test.azurecr.io/azure-vote-front:v1' ACR kapsayıcı görüntü yolu belirtin. 
4. Komutları bırakmak için Enter tuşuna basın bölümü boş.
5. Örnek sayısını 1 belirtin.

Giriş ve çıkış çalıştırma aşağıdaki gösterir yo komutu:

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

Yeoman kullanılarak zaten oluşturulmuş bir uygulamaya başka bir kapsayıcı hizmeti eklemek için aşağıdaki adımları uygulayın:

1. Değişiklik dizinine **TestContainer** dizini
2. `yo azuresfcontainer:AddService` öğesini çalıştırın 
3. 'Azurevoteback' hizmet adı
4. Arka uç depoyu için - örneğin 'test.azurecr.io/azure-vote-back:v1' ACR kapsayıcı görüntü yolu girin
5. Bölüm boş komutları bırakmak için Enter tuşuna basın
6. "1" örnek sayısı belirtin.

Kullanılan hizmet eklemek için girişleri tüm gösterilmektedir:

```bash
? Name of the application service: azurevoteback
? Input the Image Name: <acrName>.azurecr.io/azure-vote-back:v1
? Commands: 
? Number of instances of guest container application: 1
   create TestContainer/azurevotebackPkg/ServiceManifest.xml
   create TestContainer/azurevotebackPkg/config/Settings.xml
   create TestContainer/azurevotebackPkg/code/Dummy.txt
```

Bu öğretici kalanı için çalışıyoruz **TestContainer** dizin.

## <a name="configure-the-application-manifest-with-credentials-for-azure-container-registry"></a>Uygulama bildirimini kimlik bilgileriyle Azure kapsayıcı kayıt için yapılandırın.
Kimlik bilgilerini sağlamak ihtiyacımız Service Fabric'ın Azure kapsayıcı kayıt defterinden kapsayıcı görüntüleri çekmesini **ApplicationManifest.xml**. 


ACR Örneğiniz için oturum açın. Kullanım [az acr oturum açma](/cli/azure/acr#az_acr_login) işlemi tamamlamak için komutu. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz bir ad sağlayın.

```bash
az acr login --name <acrName>
```

Komut döndürür bir **başarıyla** tamamlandıktan sonra ileti.

Ardından, kapsayıcı kaydınız parola almak için aşağıdaki komutu çalıştırın. Bu parola, kapsayıcı görüntüleri çekmesini ACR ile kimlik doğrulaması için Service Fabric tarafından kullanılır.

```bash
az acr credential show -n <acrName> --query passwords[0].value
```

İçinde **ApplicationManifest.xml**, altında kod parçacığını ekleyin **ServiceManifestImport** hizmetlerinin her biri için öğesi. Ekle, **acrName** için **AccountName** alan ve önceki komuttan döndürülen parola için kullanıldığından **parola** alan. Tam **ApplicationManifest.xml** bu belgenin sonuna sağlanır. 

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="<acrName>" Password="<password>" PasswordEncrypted="false"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="configure-communication-and-container-port-to-host-port-mapping"></a>İletişim ve kapsayıcı bağlantı noktalarıyla konak bağlantı noktalarını eşlemeyi yapılandırın

### <a name="configure-communication-port"></a>İletişim bağlantı noktasını yapılandırın

İstemcilerin hizmetinizle iletişim kurabilmesi için bir HTTP uç noktası yapılandırın.  Açık *./TestContainer/azurevotefrontPkg/ServiceManifest.xml* dosyası ve bir uç nokta kaynağının bildirme **ServiceManifest** öğesi.  Protokolü, bağlantı noktasını ve adını ekleyin. Bu öğretici için hizmet bağlantı noktası 80 üzerinde dinler. 
  
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
  
Benzer şekilde, arka uç hizmetine Service Manifest değiştirin. Bu öğretici için 6379 redis varsayılan korunur.
```xml
<Resources>
  <Endpoints>
    <!-- This endpoint is used by the communication listener to obtain the port on which to 
            listen. Please note that if your service is partitioned, this port is shared with 
            replicas of different partitions that are placed in your code. -->
    <Endpoint Name="azurevotebackTypeEndpoint" UriScheme="http" Port="6379" Protocol="http"/>
  </Endpoints>
</Resources>
```
Sağlama **UriScheme**kapsayıcı uç nokta bulunabilirlik için Service Fabric adlandırma hizmetiyle otomatik olarak kaydeder. Arka uç hizmet için tam bir ServiceManifest.xml örnek dosya bu makalenin sonundaki örnek olarak sağlanır. 

### <a name="map-container-ports-to-a-service"></a>Bir hizmet eşlemesi kapsayıcı bağlantı noktaları
    
Küme kapsayıcılarında kullanıma sunmak için biz de 'ApplicationManifest.xml' bağlantı noktası bağlamasında oluşturmanız gerekir. **PortBinding** İlkesi başvuruları **uç noktaları** tanımlanmış biz **ServiceManifest.xml** dosyaları. Bu uç noktalar için gelen istekleri kapsayıcı bağlantı noktalarına açılır ve burada ilişkisindeki eşlenmiş. İçinde **ApplicationManifest.xml** dosya, uç noktaları için bağlantı noktası 80 ve 6379 bağlamak için aşağıdaki kodu ekleyin. Tam **ApplicationManifest.xml** bu belgenin sonuna kullanılabilir. 
  
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

### <a name="add-a-dns-name-to-the-backend-service"></a>Bir DNS adı arka uç hizmetine ekleyin
  
Service Fabric'ın bu DNS adına arka uç hizmetine atamak ad içinde belirtilmesi gerekiyor **ApplicationManifest.xml**. Ekleme **ServiceDnsName** özniteliğini **hizmet** gösterildiği gibi öğe: 
  
```xml
<Service Name="azurevoteback" ServiceDnsName="redisbackend.testapp">
  <StatelessService ServiceTypeName="azurevotebackType" InstanceCount="1">
    <SingletonPartition/>
  </StatelessService>
</Service>
```

Ön uç hizmeti Redis örneğinin DNS adını bilmek bir ortam değişkeni okur. Ortam değişkeni Dockerfile gösterildiği gibi tanımlanır:
  
```Dockerfile
ENV REDIS redisbackend.testapp
```
  
Ön uç kullandığı işler python komut dosyası bu DNS adı çözümlemek ve gösterildiği gibi arka uç redis deposuna bağlanmak için:

```python
# Get DNS Name
redis_server = os.environ['REDIS'] 

# Connect to the Redis store
r = redis.StrictRedis(host=redis_server, port=6379, db=0)
```

Bu noktada öğreticide, şablon bir hizmet paketi uygulama için dağıtım bir küme için kullanılabilir. Sonraki Öğreticide bu uygulamayı dağıtılır ve bir Service Fabric kümesi çalıştırılmıştır.

## <a name="create-a-service-fabric-cluster"></a>Service Fabric kümesi oluştur
Uygulamayı Azure'daki bir kümeye dağıtmak için kendi kümenizi veya bir Grup Kümesi kullanın.

Grup kümeleri Azure üzerinde barındırılan ücretsiz ve sınırlı süreli Service Fabric kümeleridir. Burada herkes uygulamaları dağıtabilir ve platform hakkında bilgi edinin Service Fabric ekibi tarafından korunur. Bir Grup Kümesine erişmek için [yönergeleri takip edin](http://aka.ms/tryservicefabric). 

Kendi kümenizi oluşturma hakkında daha fazla bilgi için bkz. [Azure'da ilk Service Fabric kümenizi oluşturma](service-fabric-get-started-azure-cluster.md).

## <a name="build-and-deploy-the-application-to-the-cluster"></a>Derleme ve uygulamayı kümeye dağıtma
Service Fabric CLI kullanarak Azure küme uygulama dağıtabilirsiniz. Service Fabric CLI makinenize yüklü değilse, yönergeleri izleyin [burada](service-fabric-get-started-linux.md#set-up-the-service-fabric-cli) yükleyin. 

Azure’daki Service Fabric kümesine bağlanın.

```bash
sfctl cluster select --endpoint http://lin4hjim3l4.westus.cloudapp.azure.com:19080
```

Sağlanan yükleme komut dosyası kullanma **TestContainer** uygulama paketi kümenin görüntü deposuna kopyalama, uygulama türünü kaydetme ve uygulama örneğini oluşturmak için dizin.

```bash
./install.sh
```

Bir tarayıcı açın ve http://lin4hjim3l4.westus.cloudapp.azure.com:19080/Explorer'a Service Fabric Gezgini'ne gidin. Uygulamaları düğümünü genişletin ve uygulama türü için bir giriş ve başka bir örnek için olduğuna dikkat edin.

![Service Fabric Explorer][sfx]

Çalışan bir uygulamaya bağlanmak için bir web tarayıcısı açın ve küme url - örneğin http://lin0823ryf2he.cloudapp.azure.com:80 gidin. Web kullanıcı Arabirimi oylama uygulamada görmeniz gerekir.

![votingapp][votingapp]

## <a name="clean-up"></a>Temizleme
Kümeden uygulama örneğini silmek ve uygulama türünün kaydını silmek için şablonda sağlanan kaldırma betiğini kullanın. Bu komut örneği temizlemek için biraz zaman alır ve bu betiği hemen sonra 'install.sh' komutu çalıştırılamıyor. 

```bash
./uninstall.sh
```

## <a name="examples-of-completed-manifests"></a>Tamamlanan bildirimleri örnekleri

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
          <RepositoryCredentials AccountName="myaccountname" Password="<password>" PasswordEncrypted="false"/>
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
            <ImageName>my.azurecr.io/azure-vote-front:v1</ImageName>
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
      <Endpoint Name="azurevotefrontTypeEndpoint" UriScheme="http" Port="8080" Protocol="http"/>
    </Endpoints>
  </Resources>

 </ServiceManifest>
```

### <a name="redis-servicemanifestxml"></a>ServiceManifest.xml redis
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
            <ImageName>my.azurecr.io/azure-vote-back:v1</ImageName>
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
      <Endpoint Name="azurevotebackTypeEndpoint" UriScheme="http" Port="6379" Protocol="http"/>
    </Endpoints>
  </Resources>
 </ServiceManifest>
```
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, birden çok kapsayıcı Yeoman kullanarak bir Service Fabric uygulamasına paket. Bu uygulama daha sonra dağıtılan ve Service Fabric kümesi üzerinde çalıştırın. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Yeoman yükleyin  
> * Yeoman kullanarak bir uygulama paketi oluşturun.
> * Kapsayıcılar ile kullanmak için uygulama paketi ayarlarını yapılandırma
> * Uygulama oluşturma  
> * Dağıtma ve uygulamayı çalıştırma 
> * Uygulamayı oluşturan Temizle

Yük devretme ve Service Fabric uygulamanın ölçeklendirme hakkında bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Yük devretme ve ölçeklendirme uygulamaları hakkında bilgi edinin](service-fabric-tutorial-containers-failover.md)

[votingapp]: ./media/service-fabric-tutorial-deploy-run-containers/votingapp.png
[sfx]: ./media/service-fabric-tutorial-deploy-run-containers/containerspackagetutorialsfx.png


