---
title: "Azure Service Fabric kapsayıcı uygulaması oluşturma | Microsoft Docs"
description: "Azure Service Fabric üzerinde ilk kapsayıcı uygulamanızı oluşturun.  Uygulamanızla bir Docker görüntüsü oluşturun, görüntüyü bir kapsayıcı kayıt defterine iletin, bir Service Fabric kapsayıcı uygulaması oluşturup dağıtın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: acb68b274228aa647dc7be5d36b2b077bd213c1b
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---

# <a name="create-your-first-service-fabric-container-app"></a>İlk Service Fabric kapsayıcı uygulamanızı oluşturma
Bir Service Fabric kümesindeki Windows kapsayıcısında mevcut olan bir uygulamayı çalıştırmak için uygulamanızda herhangi bir değişiklik yapılması gerekmez. Bu hızlı başlangıçta, web uygulaması içeren bir Docker görüntüsü oluşturma, yeni görüntüyü Azure Container Registry’ye iletme, bir Service Fabric kapsayıcı uygulaması oluşturma ve kapsayıcı uygulamasını bir Service Fabric kümesine dağıtma işlemi açıklanmaktadır.  Bu makale Docker hakkında temel bir anlayışınızın olduğunu varsayar. [Docker’a Genel Bakış](https://docs.docker.com/engine/understanding-docker/) makalesini okuyarak Docker hakkında bilgi edinebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Şunları çalıştıran bir geliştirme bilgisayarı:
* Visual Studio 2015 veya Visual Studio 2017.
* [Service Fabric SDK’sı ve araçları](service-fabric-get-started.md).
*  Windows için Docker.  [Windows için Docker CE edinme (dengeli)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Docker’ı yükleyip başlattıktan sonra tepsi simgesine sağ tıklayıp **Windows kapsayıcılarına geç** öğesini seçin. Bu işlem, Windows temelinde Docker görüntülerini çalıştırmak için gereklidir.

Kapsayıcı içeren Windows Server 2016 üzerinde üç veya daha fazla düğüme sahip bir Windows kümesi - [Küme oluşturun](service-fabric-get-started-azure-cluster.md) veya [Service Fabric’i ücretsiz deneyin](http://tryazureservicefabric.westus.cloudapp.azure.com/). 

Azure Container Registry’deki bir kayıt defteri - Azure aboneliğinizde [Kapsayıcı kayıt defteri oluşturun](../container-registry/container-registry-get-started-portal.md). 

## <a name="create-a-simple-web-app"></a>Basit web uygulaması oluşturma
Bir Docker görüntüsüne yüklemeniz gereken tüm varlıkları tek bir yerde toplayın. Bu hızlı başlangıç için, geliştirme bilgisayarınızda bir "Hello World" web uygulaması oluşturun.

1. *c:\temp\helloworldapp* gibi bir dizin oluşturun.
2. *c:\temp\helloworldapp\content* adlı bir alt dizin oluşturun.
3. *c:\temp\helloworldapp\content* dizininde *index.html* adlı bir dosya oluşturun.
4. *index.html* dosyasını düzenleyip aşağıdaki satırı ekleyin:
    ```
    <h1>Hello World!</h1>
    ```
5. Değişikliklerinizi *index.html* dosyasına kaydedin.

## <a name="build-the-docker-image"></a>Docker görüntüsü oluşturma
Docker Hub’ında bulunan [microsft/iis image](https://hub.docker.com/r/microsoft/iis/) görüntüsünü temel alan bir görüntü oluşturun. Microsoft/iis görüntüsü Windows Server Core temel işletim sistemi görüntüsünden türetilir ve Internet Information Services (IIS) içerir.  Bu görüntünün kapsayıcınızda çalıştırılması IIS ve yüklü web sitelerini otomatik olarak başlatır.

Docker görüntünüzü bir Dockerfile içinde tanımlayın. Dockerfile, görüntüyü oluşturmaya ve çalıştırmak istediğiniz uygulamayı yüklemeye ilişkin yönergeler içerir. Dockerfile, görüntüyü oluşturan ```docker build``` komutunun girdisidir. 

1. *c:\temp\helloworldapp* dizinin *Dockerfile* (uzantı olmadan) adlı bir dosya oluşturun ve aşağıdakileri ekleyin:

    ```
    # The `FROM` instruction specifies the base image. You are
    # extending the `microsoft/iis` image.
    FROM microsoft/iis

    # Create a directory to hold the web app in the container.
    RUN mkdir C:\site

    # Create a new IIS site.
    RUN powershell -NoProfile -Command \
        Import-module IISAdministration; \
        New-IISSite -Name "Site" -PhysicalPath C:\site -BindingInformation "*:8000:"

    # Opens port 8000 on the container.
    EXPOSE 8000

    # The final instruction copies the web app you created earlier into the container.
    ADD content/ /site
    ```

    Bu Dockerfile içinde bir ```ENTRYPOINT``` komutu yoktur. Gerekli değildir. Windows Server’ı IIS ile çalıştırırken, IIS işlemi temel görüntüde başlamak üzere yapılandırılmış giriş noktasıdır.

    Daha fazla bilgi için [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) okuyun.

2. Web uygulamanızı çalıştıran görüntüyü oluşturmak için ```docker build``` komutunu çalıştırın. Bir PowerShell penceresi açıp *c:\temp\helloworldapp* dizinine gidin. Şu komutu çalıştırın:

    ```
    docker build -t helloworldapp .
    ```
    Bu komut Dockerfile içindeki yönergeleri kullanarak yeni görüntüyü oluşturur ve "helloworldapp" olarak adlandırır (-t etiketi). Görüntü oluşturulduğunda, temel görüntü Docker Hub’ından alınır ve uygulamanızı temel görüntünün üstüne ekleyen yeni bir görüntü oluşturulur.  [Microsft/iis görüntüsü](https://hub.docker.com/r/microsoft/iis/) ve işletim sistemi temel görüntüleri 10,5 GB boyutundadır ve geliştirme bilgisayarınıza indirilip ayıklanması vakit alır.  İşlem sırasında öğle yemeğine çıkabilir veya kahve molası verebilirsiniz.  Temel işletim sistemi görüntüsünü geliştirme bilgisayarınıza daha önce aldıysanız indirme işlemi daha kısa sürer.

3. Oluşturma komutu tamamlandıktan sonra, yeni görüntü üzerindeki bilgileri görmek için `docker images` komutunu çalıştırın:

    ```
    docker images
    
    REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
    helloworldapp              latest              86838648aab6        2 minutes ago       10.1 GB
    ```

## <a name="verify-the-image-runs-locally"></a>Görüntünün yerel olarak çalıştığını doğrulama
Görüntüyü kapsayıcı kayıt defterine göndermeden önce yerel olarak doğrulayın.  

1. Kapsayıcıyı ```docker run``` ile başlatın:

    ```
    docker run -d -p 8000:8000 --name my-web-site helloworldapp
    ```

    *name*, çalışan kapsayıcıya bir ad verir (kapsayıcı kimliği yerine).

2. Kapsayıcı başladıktan sonra çalışan kapsayıcınıza bir tarayıcıdan bağlanabilmek için IP adresini bulun:
    ```
    docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
    ```

3. Çalışan kapsayıcıya bağlanın.  8000 numaralı bağlantı noktasında döndürülen IP adresini işaret eden bir web tarayıcısı açın (örneğin, "http://172.31.194.61:8000"). "Hello World!" başlığının tarayıcıda gösterildiğini görürsünüz.

4. Kapsayıcınızı durdurmak için şu komutu çalıştırın:

    ```
    docker stop my-web-site
    ```

5. Kapsayıcıyı geliştirme makinenizden silin:

    ```
    docker rm my-web-site
    ```

## <a name="push-the-image-to-the-container-registry"></a>Görüntüyü kapsayıcı kayıt defterine gönderme
Kapsayıcının geliştirme makinenizde çalıştığını doğruladıktan sonra, görüntüyü Azure Container Registry içindeki kayıt defterinize gönderin.

1. Kapsayıcı kayıt defterinizde [kayıt defteri kimlik bilgileriniz](../container-registry/container-registry-authentication.md) ile oturum açmak için ``docker login`` komutunu çalıştırın.

    Aşağıdaki örnekte, bir Azure Active Directory [hizmet sorumlusunun](../active-directory/active-directory-application-objects.md) kimliği ve parolası geçirilmiştir. Örneğin, bir otomasyon senaryosu için kayıt defterinize bir hizmet sorumlusu atamış olabilirsiniz.

    ```
    docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
    ```

2. Aşağıdaki komut, görüntünün kayıt defterinize ait tam yolu içeren bir etiketini veya diğer adını oluşturur. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için ```samples``` ad alanına görüntüyü yerleştirir.

    ```
    docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
    ```

3.  Görüntüyü kapsayıcı kayıt defterinize gönderin:

    ```
    docker push myregistry.azurecr.io/samples/helloworldapp
    ```

## <a name="create-and-package-the-containerized-service-in-visual-studio"></a>Visual Studio’da kapsayıcı hizmeti oluşturma ve paketleme
Service Fabric SDK’sı ve araçları, bir kapsayıcıyı Service Fabric kümesine dağıtmanıza yardımcı olan bir hizmet şablonu sağlar.

1. Visual Studio’yu çalıştırın.  **Dosya** > **Yeni** > **Proje**’yi seçin.
2. **Service Fabric uygulaması**’nı seçin, "MyFirstContainer" olarak adlandırın ve **Tamam**’a tıklayın.
3. **Hizmet şablonları** listesinden **Konuk Kapsayıcı**’yı seçin.
4. **Görüntü Adı** alanına, kapsayıcı deponuza gönderdiğiniz görüntünün dizini olan "myregistry.azurecr.io/samples/helloworldapp" değerini girin. 
5. Hizmetinize bir ad verin ve **Tamam**’a tıklayın.
6. Kapsayıcı hizmetiniz iletişim için bir uç noktaya gereksinim duyuyorsa, ServiceManifest.xml dosyasındaki ```Endpoint``` içine artık protokol, bağlantı noktası ve tür ekleyebilirsiniz. Bu hızlı başlangıçta kapsayıcı hizmeti 80 numaralı bağlantı noktasında dinler: 

    ```xml
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    ```
    ```UriScheme``` değerinin sağlanması, kapsayıcı uç noktasını bulunabilirlik için Service Fabric Adlandırma hizmetine otomatik olarak kaydeder. Bu makalenin sonunda tam bir ServiceManifest.xml örnek dosyası verilmiştir. 
7. ApplicationManifest.xml dosyasının ```ContainerHostPolicies``` kısmında bir ```PortBinding``` ilkesi belirterek kapsayıcı bağlantı noktasından ana bilgisayar bağlantı noktasına eşleme yapılandırın.  Bu hızlı başlangıçta ```ContainerPort``` değeri 8000 (Dockerfile içinde belirtildiği gibi kapsayıcı 8000 numaralı bağlantı noktasını gösterir) ve ```EndpointRef``` değeri "Guest1TypeEndpoint"’tir (hizmet bildiriminde tanımlanan uç nokta).  80 numaralı bağlantı noktasında hizmete gelen istekler, kapsayıcı üzerindeki 8000 numaralı bağlantı noktasıyla eşlenir.  Kapsayıcınızın özel bir depoda kimlik doğrulaması yapması gerekiyorsa, ```RepositoryCredentials``` öğesini ekleyin.  Bu hızlı başlangıç için, myregistry.azurecr.io kapsayıcı kayıt defterine ait hesap adı ve parola ekleyin. 

    ```xml
    <Policies>
        <ContainerHostPolicies CodePackageRef="Code">
            <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
            <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
        </ContainerHostPolicies>
    </Policies>
    ```

    Bu makalenin sonunda tam bir ApplicationManifest.xml örnek dosyası verilmiştir.
8. Uygulamayı kümenize yayımlayabilmeniz için küme bağlantısı uç noktasını yapılandırın.  İstemci bağlantı uç noktasını [Azure portalında](https://portal.azure.com) kümenizin Genel Bakış dikey penceresinde bulabilirsiniz. Çözüm Gezgini'nde **MyFirstContainer**->**PublishProfiles** altındaki *Cloud.xml* dosyasını açın.  Küme adı ve bağlantı noktasını **ClusterConnectionParameters**’a ekleyin.  Örneğin:
    ```xml
    <ClusterConnectionParameters ConnectionEndpoint="containercluster.westus2.cloudapp.azure.com:19000" />
    ```
    
9. Tüm dosyaları kaydedin ve projenizi oluşturun.  

10. Uygulamanızı paketlemek için Çözüm Gezgini’nde **MyFirstContainer**’a sağ tıklayın ve **Paketle**’yi seçin. 

## <a name="deploy-the-container-app"></a>Kapsayıcı uygulamasını dağıtma
1. Uygulamanızı yayımlamak için Çözüm Gezgini’nde **MyFirstContainer**’a sağ tıklayın ve **Yayımla**’yı seçin.

2. [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), bir Service Fabric kümesindeki uygulama ve düğümleri inceleyip yönetmeye yönelik web tabanlı bir araçtır. Bir tarayıcı açıp http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ dizinine gidin ve uygulama geliştirme adımlarını izleyin.  Uygulama dağıtılır, ancak görüntü küme düğümlerine yüklenene kadar hatalı durumdadır (bu işlem, görüntü boyutuna bağlı olarak biraz zaman alabilir):  ![Hata][1]

3. Uygulamanın ```Ready``` durumu  ![Hazır][2] olduğunda uygulama hazırdır

4. Bir tarayıcı açıp http://containercluster.westus2.cloudapp.azure.com adresine gidin. "Hello World!" başlığının tarayıcıda gösterildiğini görürsünüz.

## <a name="clean-up"></a>Temizleme
Küme çalışırken size ücret yansımaya devam edebilir, bu nedenle [kümenizi silmeyi](service-fabric-get-started-azure-cluster.md#remove-the-cluster) düşünün.  [Taraf kümeleri](http://tryazureservicefabric.westus.cloudapp.azure.com/) birkaç saat sonra otomatik olarak silinir.

Görüntüyü kapsayıcı kayıt defterine gönderdikten sonra yerel görüntüyü geliştirme bilgisayarınızdan silebilirsiniz:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Tam Service Fabric uygulaması ve hizmet bildirimleri örneği
Bu hızlı başlangıçta kullanılan tam hizmet ve uygulama bildirimleri aşağıda verilmiştir.

### <a name="servicemanifestxml"></a>ServiceManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Guest1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="Guest1Type" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
      <ContainerHost>
        <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container: -->
    <!--
    <EnvironmentVariables>
      <EnvironmentVariable Name="VariableName" Value="VariableValue"/>
    </EnvironmentVariables>
    -->
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="80" Protocol="http"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```
### <a name="applicationmanifestxml"></a>ApplicationManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="MyFirstContainerType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Guest1_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="8000" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Guest1">
      <StatelessService ServiceTypeName="Guest1Type" InstanceCount="[Guest1_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
* GitHub’da [Service Fabric kapsayıcı kod örneklerine](https://github.com/Azure-Samples/service-fabric-dotnet-containers) bakın.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png

