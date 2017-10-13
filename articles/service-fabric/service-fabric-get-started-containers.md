---
title: "Azure Service Fabric kapsayıcı uygulaması oluşturma | Microsoft Docs"
description: "Azure Service Fabric üzerinde ilk Windows kapsayıcı uygulamanızı oluşturun.  Python uygulamasıyla bir Docker görüntüsü oluşturun, görüntüyü bir kapsayıcı kayıt defterine iletin, bir Service Fabric kapsayıcı uygulaması oluşturup dağıtın."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/18/2017
ms.author: ryanwi
ms.openlocfilehash: 025bde02b3f342ec3399d51819d1fa8a91f11374
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-service-fabric-container-application-on-windows"></a>Windows üzerinde ilk Service Fabric kapsayıcı uygulamanızı oluşturma
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started-containers.md)
> * [Linux](service-fabric-get-started-containers-linux.md)

Bir Service Fabric kümesindeki Windows kapsayıcısında mevcut olan bir uygulamayı çalıştırmak için uygulamanızda herhangi bir değişiklik yapılması gerekmez. Bu makalede, Python [Flask](http://flask.pocoo.org/) web uygulaması içeren bir Docker görüntüsü oluşturma ve bunu Service Fabric kümesine dağıtma işlemlerinde size yol gösterilir.  Ayrıca, kapsayıcıya alınmış uygulamanızı [Azure Container Registry](/azure/container-registry/) aracılığıyla paylaşırsınız.  Bu makale Docker hakkında temel bir anlayışınızın olduğunu varsayar. [Docker’a Genel Bakış](https://docs.docker.com/engine/understanding-docker/) makalesini okuyarak Docker hakkında bilgi edinebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Şunları çalıştıran bir geliştirme bilgisayarı:
* Visual Studio 2015 veya Visual Studio 2017.
* [Service Fabric SDK’sı ve araçları](service-fabric-get-started.md).
*  Windows için Docker.  [Windows için Docker CE edinme (dengeli)](https://store.docker.com/editions/community/docker-ce-desktop-windows?tab=description). Docker’ı yükleyip başlattıktan sonra tepsi simgesine sağ tıklayıp **Windows kapsayıcılarına geç** öğesini seçin. Bu işlem, Windows temelinde Docker görüntülerini çalıştırmak için gereklidir.

Kapsayıcı içeren Windows Server 2016 üzerinde üç veya daha fazla düğüme sahip bir Windows kümesi - [Küme oluşturun](service-fabric-cluster-creation-via-portal.md) veya [Service Fabric’i ücretsiz deneyin](https://aka.ms/tryservicefabric).

Azure Container Registry’deki bir kayıt defteri - Azure aboneliğinizde [Kapsayıcı kayıt defteri oluşturun](../container-registry/container-registry-get-started-portal.md).

## <a name="define-the-docker-container"></a>Docker kapsayıcısını tanımlama
Docker Hub’ında bulunan [Python görüntüsünü](https://hub.docker.com/_/python/) temel alan bir görüntü oluşturun.

Docker kapsayıcınızı bir Dockerfile içinde tanımlayın. Dockerfile, kapsayıcınızın içindeki ortamı ayarlama, çalıştırmak istediğiniz uygulamayı yükleme ve bağlantı noktalarını eşleme yönergelerini içerir. Dockerfile, görüntüyü oluşturan `docker build` komutunun girdisidir.

Boş bir dizin oluşturun ve *Dockerfile* dosyasını oluşturun (dosya uzantısı kullanmayın). Aşağıdakini *Dockerfile* dosyasına ekleyin ve değişikliklerinizi kaydedin:

```
# Use an official Python runtime as a base image
FROM python:2.7-windowsservercore

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

Daha fazla bilgi için [Dockerfile başvurusunu](https://docs.docker.com/engine/reference/builder/) okuyun.

## <a name="create-a-simple-web-application"></a>Basit web uygulaması oluşturma
Bağlantı noktası 80 üzerinden dinleyen ve "Merhaba Dünya!" başlığını döndüren bir Flask web uygulaması oluşturun.  Aynı dizinde, *requirements.txt* dosyasını oluşturun.  Aşağıdakini dosyaya ekleyin ve değişikliklerinizi kaydedin:
```
Flask
```

Ayrıca, *app.py* dosyasını da oluşturun ve aşağıdakini ekleyin:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():

    return 'Hello World!'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

<a id="Build-Containers"></a>
## <a name="build-the-image"></a>Görüntü oluşturma
Web uygulamanızı çalıştıran görüntüyü oluşturmak için `docker build` komutunu çalıştırın. PowerShell penceresini açın ve Dockerfile dosyasını içeren dizine gidin. Şu komutu çalıştırın:

```
docker build -t helloworldapp .
```

Bu komut Dockerfile içindeki yönergeleri kullanarak yeni görüntüyü oluşturur ve "helloworldapp" olarak adlandırır (-t etiketi). Görüntü oluşturulduğunda, temel görüntü Docker Hub’ından alınır ve uygulamanızı temel görüntünün üstüne ekleyen yeni bir görüntü oluşturulur.  

Oluşturma komutu tamamlandıktan sonra, yeni görüntü üzerindeki bilgileri görmek için `docker images` komutunu çalıştırın:

```
$ docker images

REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
helloworldapp                 latest              8ce25f5d6a79        2 minutes ago       10.4 GB
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma
Görüntüyü kapsayıcı kayıt defterine göndermeden önce yerel olarak doğrulayın.  

Uygulamayı çalıştırın:

```
docker run -d --name my-web-site helloworldapp
```

*name*, çalışan kapsayıcıya bir ad verir (kapsayıcı kimliği yerine).

Kapsayıcı başladıktan sonra çalışan kapsayıcınıza bir tarayıcıdan bağlanabilmek için IP adresini bulun:
```
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" my-web-site
```

Çalışan kapsayıcıya bağlanın.  Döndürülen IP adresine işaret eden bir web tarayıcısı açın (örneğin, "http://172.31.194.61"). "Hello World!" başlığının tarayıcıda gösterildiğini görürsünüz.

Kapsayıcınızı durdurmak için şu komutu çalıştırın:

```
docker stop my-web-site
```

Kapsayıcıyı geliştirme makinenizden silin:

```
docker rm my-web-site
```

<a id="Push-Containers"></a>
## <a name="push-the-image-to-the-container-registry"></a>Görüntüyü kapsayıcı kayıt defterine gönderme
Kapsayıcının geliştirme makinenizde çalıştığını doğruladıktan sonra, görüntüyü Azure Container Registry içindeki kayıt defterinize gönderin.

Kapsayıcı kayıt defterinizde [kayıt defteri kimlik bilgileriniz](../container-registry/container-registry-authentication.md) ile oturum açmak için ``docker login`` komutunu çalıştırın.

Aşağıdaki örnekte, bir Azure Active Directory [hizmet sorumlusunun](../active-directory/active-directory-application-objects.md) kimliği ve parolası geçirilmiştir. Örneğin, bir otomasyon senaryosu için kayıt defterinize bir hizmet sorumlusu atamış olabilirsiniz. İsterseniz, kayıt defteri kullanıcı kimliğiniz ve parolanızı kullanarak oturum açabilirsiniz.

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

Aşağıdaki komut, görüntünün kayıt defterinize ait tam yolu içeren bir etiketini veya diğer adını oluşturur. Bu örnek, kayıt defterinin kökünde dağınıklığı önlemek için ```samples``` ad alanına görüntüyü yerleştirir.

```
docker tag helloworldapp myregistry.azurecr.io/samples/helloworldapp
```

Görüntüyü kapsayıcı kayıt defterinize gönderin:

```
docker push myregistry.azurecr.io/samples/helloworldapp
```

## <a name="create-the-containerized-service-in-visual-studio"></a>Visual Studio’da kapsayıcıya alınmış hizmet oluşturma
Service Fabric SDK’sı ve araçları, kapsayıcıya alınmış uygulamalar oluşturmanıza yardımcı olan bir hizmet şablonu sağlar.

1. Visual Studio’yu çalıştırın.  **Dosya** > **Yeni** > **Proje**’yi seçin.
2. **Service Fabric uygulaması**’nı seçin, "MyFirstContainer" olarak adlandırın ve **Tamam**’a tıklayın.
3. **Hizmet şablonları** listesinden **Konuk Kapsayıcı**’yı seçin.
4. **Görüntü Adı** alanına, kapsayıcı deponuza gönderdiğiniz görüntünün dizini olan "myregistry.azurecr.io/samples/helloworldapp" değerini girin.
5. Hizmetinize bir ad verin ve **Tamam**’a tıklayın.

## <a name="configure-communication"></a>İletişimi yapılandırma
Kapsayıcıya alınmış hizmetin iletişim sağlayabilmesi için bir uç nokta gerekir. ServiceManifest.xml dosyasına protokol, bağlantı noktası ve tür bilgileriyle bir `Endpoint` öğesi ekleyin. Bu makalede, kapsayıcıya alınmış hizmet 8081 numaralı bağlantı noktasında dinler.  Bu örnekte, 8081 numaralı sabit bağlantı noktası kullanılır.  Hiçbir bağlantı noktası belirtilmemişse, uygulama bağlantı noktası aralığından rastgele bir bağlantı noktası seçilir.  

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
  </Endpoints>
</Resources>
```

Uç nokta tanımlandığında, Service Fabric uç noktayı Adlandırma hizmetinde yayımlar.  Kümede çalışan diğer hizmetler bu kapsayıcıyı çözümleyebilir.  Ayrıca, [ters proxy](service-fabric-reverseproxy.md)’yi kullanarak kapsayıcıdan kapsayıcıya iletişim kurabilirsiniz.  İletişim, ters proxy’nin HTTP dinleme bağlantı noktasını ve iletişim kurmak istediğiniz hizmetlerin adlarının ortam değişkenleri olarak sağlanmasıyla gerçekleştirilir.

## <a name="configure-and-set-environment-variables"></a>Ortam değişkenlerini yapılandırma ve ayarlama
Ortam değişkenleri, hizmet bildirimindeki her kod paketi için belirtilebilir. Bu özellik, kapsayıcı olarak mı dağıtıldıklarına yoksa konuk yürütülebilir dosyası olarak mı işlendiklerine bakılmaksızın tüm hizmetlerde sağlanır. Ortam değişkeni değerlerini, uygulama bildiriminde geçersiz kılabilir veya dağıtım sırasında uygulama parametresi olarak belirtebilirsiniz.

Aşağıdaki hizmet bildirimi XML kod parçacığı, kod paketi için ortam değişkenlerinin nasıl belirtileceğini gösteren bir örnektir:
```xml
<CodePackage Name="Code" Version="1.0.0">
  ...
  <EnvironmentVariables>
    <EnvironmentVariable Name="HttpGatewayPort" Value=""/>    
  </EnvironmentVariables>
</CodePackage>
```

Bu ortam değişkenleri, uygulama bildiriminde geçersiz kılınabilir:

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
    <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
  </EnvironmentOverrides>
  ...
</ServiceManifestImport>
```

## <a name="configure-container-port-to-host-port-mapping-and-container-to-container-discovery"></a>Kapsayıcı bağlantı noktasından konak bağlantı noktasına eşlemeyi ve kapsayıcıdan kapsayıcıya keşfi yapılandırma
Kapsayıcıyla iletişim kurmak için kullanılan konak bağlantı noktasını yapılandırın. Bağlantı noktası bağlama, hizmetin kapsayıcı içinde dinlediği bağlantı noktasını konaktaki bağlantı noktasıyla eşler. ApplicationManifest.xml dosyasının `ContainerHostPolicies` öğesine bir `PortBinding` öğesi ekleyin.  Bu makalede `ContainerPort` değeri 80 (Dockerfile içinde belirtildiği gibi kapsayıcı 80 numaralı bağlantı noktasını gösterir) ve `EndpointRef` değeri "Guest1TypeEndpoint" (hizmet bildiriminde daha önce tanımlanmış olan uç nokta) olarak verilir.  8081 numaralı bağlantı noktasında hizmete gelen istekler, kapsayıcı üzerindeki 80 numaralı bağlantı noktasıyla eşlenir.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-container-registry-authentication"></a>Kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırma
ApplicationManifest.xml dosyasının `ContainerHostPolicies` öğesine bir `RepositoryCredentials` öğesi ekleyerek kapsayıcı kayıt defteri kimlik doğrulamasını yapılandırın. Hizmetin depodan kapsayıcı görüntüsünü indirmesini sağlayan myregistry.azurecr.io kapsayıcı kayıt defterine hesap ve parola ekleyin.

```xml
<Policies>
    <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="=P==/==/=8=/=+u4lyOB=+=nWzEeRfF=" PasswordEncrypted="false"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
    </ContainerHostPolicies>
</Policies>
```

Kümenin tüm düğümlerine dağıtılan bir şifreleme sertifikası kullanarak depo parolasını şifrelemenizi öneririz. Service Fabric hizmet paketi kümeye dağıttığında, şifre metninin şifresini çözmek için şifreleme sertifikası kullanılır.  Parolanın şifre metni Invoke-ServiceFabricEncryptText cmdlet’i kullanılarak oluşturulur ve bu metin ApplicationManifest.xml dosyasına eklenir.

Aşağıdaki betik, otomatik olarak imzalanan yeni bir sertifika oluşturup bunu PFX dosyasına aktarır.  Sertifika, var olan bir anahtar kasasının içine aktarılır ve ardından Service Fabric kümesine dağıtılır.

```powershell
# Variables.
$certpwd = ConvertTo-SecureString -String "Pa$$word321!" -Force -AsPlainText
$filepath = "C:\MyCertificates\dataenciphermentcert.pfx"
$subjectname = "dataencipherment"
$vaultname = "mykeyvault"
$certificateName = "dataenciphermentcert"
$groupname="myclustergroup"
$clustername = "mycluster"

$subscriptionId = "subscription ID"

Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Create a self signed cert, export to PFX file.
New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject $subjectname -Provider 'Microsoft Enhanced Cryptographic Provider v1.0' `
| Export-PfxCertificate -FilePath $filepath -Password $certpwd

# Import the certificate to an existing key vault.  The key vault must be enabled for deployment.
$cer = Import-AzureKeyVaultCertificate -VaultName $vaultName -Name $certificateName -FilePath $filepath -Password $certpwd

Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $groupname -EnabledForDeployment

# Add the certificate to all the VMs in the cluster.
Add-AzureRmServiceFabricApplicationCertificate -ResourceGroupName $groupname -Name $clustername -SecretIdentifier $cer.SecretId
```
[Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet’ini kullanarak parolayı şifreleyin.

```powershell
$text = "=P==/==/=8=/=+u4lyOB=+=nWzEeRfF="
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint $cer.Thumbprint -Text $text -StoreLocation Local -StoreName My
```

Parolayı, [Invoke-ServiceFabricEncryptText](/powershell/module/servicefabric/Invoke-ServiceFabricEncryptText?view=azureservicefabricps) cmdlet’i tarafından döndürülen şifre metniyle değiştirin ve `PasswordEncrypted` öğesini “true” olarak ayarlayın.

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="Code">
    <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
    <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
  </ContainerHostPolicies>
</Policies>
```

## <a name="configure-isolation-mode"></a>Yalıtım modunu yapılandırma
Windows, kapsayıcılar için iki yalıtım modunu destekler: İşlem ve Hyper-V. İşlem yalıtım moduyla, aynı konak makinesinde çalışan tüm kapsayıcılar çekirdeği konakla paylaşır. Hyper-V yalıtım moduyla, çekirdekler her Hyper-V kapsayıcısı ile kapsayıcı konağı arasında yalıtılır. Yalıtım modu, uygulama bildirimi dosyasında bulunan `ContainerHostPolicies` öğesinde belirtilir. Belirtilebilen yalıtım modları `process`, `hyperv` ve `default` modlarıdır. Varsayılan yalıtım modu, Windows Server konaklarında `process` ve Windows 10 konaklarında `hyperv` modudur. Aşağıdaki kod parçacığı uygulama bildirimi dosyasında yalıtım modunun nasıl belirtildiğini gösterir.

```xml
<ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
```

## <a name="configure-resource-governance"></a>Kaynak idaresini yapılandırma
[Kaynak idaresi](service-fabric-resource-governance.md) kapsayıcının konakta kullanabildiği kaynakları kısıtlar. Uygulama bildiriminde belirtilen `ResourceGovernancePolicy` öğesi, hizmet kod paketinin kaynak sınırlarını tanımlamak için kullanılır. Şu kaynaklar için kaynak sınırları ayarlanabilir: Memory, MemorySwap, CpuShares (CPU göreli ağırlığı), MemoryReservationInMB, BlkioWeight (BlockIO göreli ağırlığı).  Bu örnekte, Guest1Pkg hizmet paketi bulunduğu küme düğümlerinde bir çekirdek alır.  Bellek sınırları mutlaktır; dolayısıyla, kod paketi 1024 MB bellekle (aynı genel garantili ayırmayla) sınırlıdır. Kod paketleri (kapsayıcılar veya işlemler) bu sınırı aşan miktarda bellek ayıramazlar ve bunu denediklerinde yetersiz bellek özel durumu ortaya çıkar. Kaynak sınırı zorlamasının çalışması için, hizmet paketi içindeki tüm kod paketlerinin bellek sınırlarının belirtilmiş olması gerekir.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="Guest1Pkg" ServiceManifestVersion="1.0.0" />
  <Policies>
    <ServicePackageResourceGovernancePolicy CpuCores="1"/>
    <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
  </Policies>
</ServiceManifestImport>
```

## <a name="deploy-the-container-application"></a>Kapsayıcı uygulamasını dağıtma
Tüm değişikliklerinizi kaydedin ve uygulamayı derleyin. Uygulamanızı yayımlamak için Çözüm Gezgini’nde **MyFirstContainer**’a sağ tıklayın ve **Yayımla**’yı seçin.

**Bağlantı Uç Noktası**’nda kümenin yönetim uç noktasını girin.  Örneğin, "containercluster.westus2.cloudapp.azure.com:19000". İstemci bağlantı uç noktasını [Azure portalında](https://portal.azure.com) kümenizin Genel Bakış dikey penceresinde bulabilirsiniz.

**Yayımla**’ta tıklayın.

[Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), bir Service Fabric kümesindeki uygulama ve düğümleri inceleyip yönetmeye yönelik web tabanlı bir araçtır. Bir tarayıcı açıp http://containercluster.westus2.cloudapp.azure.com:19080/Explorer/ dizinine gidin ve uygulama geliştirme adımlarını izleyin.  Uygulama dağıtılır, ancak görüntü küme düğümlerine yüklenene kadar hatalı durumdadır (bu işlem, görüntü boyutuna bağlı olarak biraz zaman alabilir): ![Hata][1]

Uygulamanın ```Ready``` durumu ![Hazır][2] olduğunda uygulama hazırdır

Bir tarayıcı açıp http://containercluster.westus2.cloudapp.azure.com:8081 adresine gidin. "Hello World!" başlığının tarayıcıda gösterildiğini görürsünüz.

## <a name="clean-up"></a>Temizleme
Küme çalışırken size ücret yansımaya devam edebilir, bu nedenle [kümenizi silmeyi](service-fabric-get-started-azure-cluster.md#remove-the-cluster) düşünün.  [Taraf kümeleri](http://tryazureservicefabric.westus.cloudapp.azure.com/) birkaç saat sonra otomatik olarak silinir.

Görüntüyü kapsayıcı kayıt defterine gönderdikten sonra yerel görüntüyü geliştirme bilgisayarınızdan silebilirsiniz:

```
docker rmi helloworldapp
docker rmi myregistry.azurecr.io/samples/helloworldapp
```

## <a name="complete-example-service-fabric-application-and-service-manifests"></a>Tam Service Fabric uygulaması ve hizmet bildirimleri örneği
Bu makalede kullanılan tam hizmet ve uygulama bildirimleri aşağıda verilmiştir.

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
    <EnvironmentVariables>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
    </EnvironmentVariables>

  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to
           listen. Please note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="Guest1TypeEndpoint" UriScheme="http" Port="8081" Protocol="http"/>
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
    <EnvironmentOverrides CodePackageRef="FrontendService.Code">
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentOverrides>
    <ConfigOverrides />
    <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="myregistry" Password="MIIB6QYJKoZIhvcNAQcDoIIB2jCCAdYCAQAxggFRMIIBTQIBADA1MCExHzAdBgNVBAMMFnJ5YW53aWRhdGFlbmNpcGhlcm1lbnQCEFfyjOX/17S6RIoSjA6UZ1QwDQYJKoZIhvcNAQEHMAAEg
gEAS7oqxvoz8i6+8zULhDzFpBpOTLU+c2mhBdqXpkLwVfcmWUNA82rEWG57Vl1jZXe7J9BkW9ly4xhU8BbARkZHLEuKqg0saTrTHsMBQ6KMQDotSdU8m8Y2BR5Y100wRjvVx3y5+iNYuy/JmM
gSrNyyMQ/45HfMuVb5B4rwnuP8PAkXNT9VLbPeqAfxsMkYg+vGCDEtd8m+bX/7Xgp/kfwxymOuUCrq/YmSwe9QTG3pBri7Hq1K3zEpX4FH/7W2Zb4o3fBAQ+FuxH4nFjFNoYG29inL0bKEcTX
yNZNKrvhdM3n1Uk/8W2Hr62FQ33HgeFR1yxQjLsUu800PrYcR5tLfyTB8BgkqhkiG9w0BBwEwHQYJYIZIAWUDBAEqBBBybgM5NUV8BeetUbMR8mJhgFBrVSUsnp9B8RyebmtgU36dZiSObDsI
NtTvlzhk11LIlae/5kjPv95r3lw6DHmV4kXLwiCNlcWPYIWBGIuspwyG+28EWSrHmN7Dt2WqEWqeNQ==" PasswordEncrypted="true"/>
        <PortBinding ContainerPort="80" EndpointRef="Guest1TypeEndpoint"/>
      </ContainerHostPolicies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="Code" MemoryInMB="1024"  />
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

## <a name="configure-time-interval-before-container-is-force-terminated"></a>Kapsayıcı zorla sonlandırılmadan önceki zaman aralığını yapılandırın

Hizmet silme (veya başka bir düğüme taşıma) başladıktan sonra, çalışma zamanının kapsayıcı kaldırılmadan önce ne kadar bekleyeceğine ilişkin bir zaman aralığı yapılandırabilirsiniz. Zaman aralığını yapılandırma, kapsayıcıya `docker stop <time in seconds>` komutunu gönderir.   Daha ayrıntılı bilgi için bkz. [docker durdurma](https://docs.docker.com/engine/reference/commandline/stop/). Beklenecek zaman aralığı, `Hosting` bölümünde belirtilir. Aşağıdaki küme bildirimi kod parçacığı, bekleme aralığının nasıl ayarlandığını gösterir:

```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "ContainerDeactivationTimeout": "10",
          ...
          }
        ]
}
```
Varsayılan zaman aralığı 10 saniye olarak ayarlanır. Bu yapılandırma dinamik olduğundan, kümedeki yalnızca yapılandırmaya yönelik bir güncelleştirme zaman aşımını güncelleştirir. 


## <a name="configure-the-runtime-to-remove-unused-container-images"></a>Kullanılmayan kapsayıcı görüntülerini kaldırmak için çalışma zamanını yapılandırma

Service Fabric kümesini kullanılmayan kapsayıcı görüntülerini düğümden kaldıracak şekilde yapılandırabilirsiniz. Bu yapılandırma, düğümde çok fazla kapsayıcı görüntüsü varsa yeniden disk alanı elde edilmesine imkan tanır.  Bu özelliği etkinleştirmek için küme bildirimindeki `Hosting` bölümünü aşağıdaki kod parçacığında gösterildiği gibi güncelleştirin: 


```xml
{
        "name": "Hosting",
        "parameters": [
          {
            "PruneContainerImages": “True”,
            "ContainerImagesToSkip": "microsoft/windowsservercore|microsoft/nanoserver|…",
          ...
          }
        ]
} 
```

Silinmemesi gereken görüntüleri `ContainerImagesToSkip` parametresi altında belirtebilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric’te kapsayıcı](service-fabric-containers-overview.md) çalıştırma hakkında daha fazla bilgi edinin.
* [Kapsayıcı içinde .NET uygulaması dağıtma](service-fabric-host-app-in-a-container.md) öğreticisini okuyun.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
* GitHub’da [Service Fabric kapsayıcı kod örneklerine](https://github.com/Azure-Samples/service-fabric-dotnet-containers) bakın.

[1]: ./media/service-fabric-get-started-containers/MyFirstContainerError.png
[2]: ./media/service-fabric-get-started-containers/MyFirstContainerReady.png
