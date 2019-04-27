---
title: Güvenli bir şekilde bir Azure Service Fabric kümesine bağlanın | Microsoft Docs
description: Bir Service Fabric kümesinde istemci erişimi kimlik doğrulaması yapmayı ve istemcilerle bir küme arasındaki iletişimin güvenliğini sağlamak nasıl açıklar.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/29/2019
ms.author: aljo
ms.openlocfilehash: 42c8fa15c6b1e7c98ae47180bec5cc61236a7c44
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60881394"
---
# <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma

İstemci bir Service Fabric küme düğümüne bağlandığında istemci sertifika güvenliği veya Azure Active Directory (AAD) kullanılarak kimliği doğrulanmış ve güvenli iletişimi olabilir. Bu kimlik doğrulaması yalnızca yetkili kullanıcıların kümesine erişebilirsiniz ve dağıtılan uygulamalar ve yönetim görevlerini gerçekleştirme sağlar.  Kümeyi oluştururken sertifika veya AAD güvenlik daha önce kümede etkinleştirilmiş olmalıdır.  Küme güvenliği senaryoları hakkında daha fazla bilgi için bkz. [küme güvenlik](service-fabric-cluster-security.md). Sertifika ile güvenli bir kümeye bağlanıyorsanız [istemci sertifika ayarlama](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) bilgisayarda kümeye bağlanır. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric CLI (sfctl) kullanarak güvenli bir kümeye bağlanma

Service Fabric CLI (sfctl) kullanarak güvenli bir kümeye bağlanmak için birkaç farklı yolu vardır. Kimlik doğrulaması için bir istemci sertifikası kullanıyorsanız sertifika bilgilerinin küme düğümlerine dağıtılmış olan bir sertifikayla eşleşmesi gerekir. Sertifikanızı sertifika yetkilileri (CA'lar) varsa, güvenilen CA'lar ayrıca belirtmeniz gerekir.

Kullanarak kümeye bağlanabilir `sfctl cluster select` komutu.

İstemci sertifikaları bir sertifika ve anahtar çifti olarak veya tek bir PFX dosyası olarak iki farklı fashions belirtilebilir. Parola korumalı PEM dosyaları için otomatik olarak parolayı girmeniz istenir. Önce istemci sertifikasını bir PFX dosyası olarak aldıysanız, PFX dosyasını aşağıdaki komutu kullanarak bir PEM dosyasına dönüştürmek. 

```bash
openssl pkcs12 -in your-cert-file.pfx -out your-cert-file.pem -nodes -passin pass:your-pfx-password
```

.Pfx dosyanızı parola korumalı değilse, - passin geçişini kullanın: son parametresi için.

Pem dosyası olarak istemci sertifikasını belirtmek için dosya yolu belirtin. `--pem` bağımsız değişken. Örneğin:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Pem dosyaları herhangi bir komutu çalıştırmadan önce parolası ister parola korumalı.

Bir sertifika belirtmek için anahtar çifti kullanımı `--cert` ve `--key` ilgili her dosya için dosya yollarını belirtmek için bağımsız değişkenler.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Bazen test veya geliştirme kümelerini korumak için kullanılan sertifikaları, sertifika doğrulama başarısız. Sertifika doğrulamasını atlamak için belirtin `--no-verify` seçeneği. Örneğin:

> [!WARNING]
> Kullanmayın `no-verify` seçeneği için Service Fabric kümelerini üretim bağlanırken.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Ayrıca, dizinleri güvenilir CA sertifikaları ya da tek tek sertifikaları için yol belirtebilirsiniz. Bu yolları belirtmek için kullanın `--ca` bağımsız değişken. Örneğin:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

Bağlandıktan sonra yapabilmelisiniz [diğer sfctl komutlarının çalıştırılmasını](service-fabric-cli.md) kümeyle etkileşim kurmak için.

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a>PowerShell kullanarak bir kümeye bağlanma
İlk PowerShell aracılığıyla bir kümede bir işlem gerçekleştirmeden önce kümeye bağlantı kurun. Küme bağlantısı verilen PowerShell oturumunda sonraki tüm komutlar için kullanılır.

### <a name="connect-to-an-unsecure-cluster"></a>Güvenli olmayan bir kümeye bağlanma

Güvenli olmayan bir kümeye bağlanmak için küme uç nokta adresine sağlamak **Connect-ServiceFabricCluster** komutu:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Azure Active Directory kullanarak güvenli bir kümeye bağlanma

Küme Yöneticisi erişim yetkisi vermek için Azure Active Directory kullanan güvenli bir kümeye bağlanmak için küme sertifikası parmak izi ve makul *AzureActiveDirectory* bayrağı.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Bir istemci sertifikası ile güvenli bir kümeye bağlanma
Yönetici erişim yetkisi vermek için istemci sertifikalarını kullanan güvenli bir kümeye bağlanmak için aşağıdaki PowerShell komutunu çalıştırın. 

#### <a name="connect-using-certificate-common-name"></a>Sertifika ortak adını kullanarak bağlanma
Küme sertifikasını ortak ad ve küme yönetimi için izinleri verilmiş olan istemci sertifikası ortak adı sağlayın. Sertifika ayrıntıları, küme düğümlerinde bir sertifikayla eşleşmesi gerekir.

```powershell
Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCommonName <certificate common name>  `
    -FindType FindBySubjectName `
    -FindValue <certificate common name> `
    -StoreLocation CurrentUser `
    -StoreName My 
```
*ServerCommonName* küme düğümlerinde yüklü sunucu sertifikasının ortak addır. *FindValue* ortak yönetici istemci sertifikası adıdır. Parametreleri doldurulduğunda, komutu aşağıdaki örnekteki gibi görünür:
```powershell
$ClusterName= "sf-commonnametest-scus.southcentralus.cloudapp.azure.com:19000"
$certCN = "sfrpe2eetest.southcentralus.cloudapp.azure.com"

Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCommonName $certCN  `
    -FindType FindBySubjectName `
    -FindValue $certCN `
    -StoreLocation CurrentUser `
    -StoreName My 
```

#### <a name="connect-using-certificate-thumbprint"></a>Sertifika parmak izini kullanarak bağlan
Küme sertifikası parmak izi ve küme yönetimi için izinleri verilmiş olan istemci sertifikası parmak izi sağlayın. Sertifika ayrıntıları, küme düğümlerinde bir sertifikayla eşleşmesi gerekir.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `  
          -KeepAliveIntervalInSec 10 `  
          -X509Credential -ServerCertThumbprint <Certificate Thumbprint> `  
          -FindType FindByThumbprint -FindValue <Certificate Thumbprint> `  
          -StoreLocation CurrentUser -StoreName My
```

*ServerCertThumbprint* küme düğümlerinde yüklü sunucu sertifikasının parmak izi. *FindValue* yönetici istemci sertifikasının parmak izi.  Parametreleri doldurulduğunda, komutu aşağıdaki örnekteki gibi görünür:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint clustername.westus.cloudapp.azure.com:19000 `  
          -KeepAliveIntervalInSec 10 `  
          -X509Credential -ServerCertThumbprint A8136758F4AB8962AF2BF3F27921BE1DF67F4326 `  
          -FindType FindByThumbprint -FindValue 71DE04467C9ED0544D021098BCD44C71E183414E `  
          -StoreLocation CurrentUser -StoreName My 
```

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a>Windows Active Directory kullanarak güvenli bir kümeye bağlanma
AD güvenlik kullanarak tek başına kümenizi dağıtılırsa, ' % s'anahtarı "WindowsCredential" ekleyerek kümeye bağlanın.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a>FabricClient API'leri kullanarak bir kümeye bağlanma
Service Fabric SDK'sı sağlar [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) küme yönetimi için sınıf. FabricClient API'leri kullanmak için Microsoft.ServiceFabric NuGet paketini alın.

### <a name="connect-to-an-unsecure-cluster"></a>Güvenli olmayan bir kümeye bağlanma

Uzaktan güvenli olmayan bir kümeye bağlanmak için FabricClient örneği oluşturun ve küme adresini sağlayın:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

Bir küme içinde çalışan kod için güvenilir bir hizmet, örneğin, bir FabricClient oluşturun. *olmadan* küme adresini belirtin. Yerel Yönetim ağ geçidi kodu şu anda, üzerinde çalıştığı düğüm üzerinde fazladan ağ atlama önleme FabricClient bağlanır.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Bir istemci sertifikası ile güvenli bir kümeye bağlanma

Kümedeki düğümler geçerli sertifikaların ortak adı olması gerekir veya SAN DNS adı görünür [RemoteCommonNames özelliği](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials) ayarlamak [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient). Bu işlem aşağıdaki istemci ve küme düğümleri arasında karşılıklı kimlik doğrulaması sağlar.

```csharp
using System.Fabric;
using System.Security.Cryptography.X509Certificates;

string clientCertThumb = "71DE04467C9ED0544D021098BCD44C71E183414E";
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string CommonName = "www.clustername.westus.azure.com";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var xc = GetCredentials(clientCertThumb, serverCertThumb, CommonName);
var fc = new FabricClient(xc, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

static X509Credentials GetCredentials(string clientCertThumb, string serverCertThumb, string name)
{
    X509Credentials xc = new X509Credentials();
    xc.StoreLocation = StoreLocation.CurrentUser;
    xc.StoreName = "My";
    xc.FindType = X509FindType.FindByThumbprint;
    xc.FindValue = clientCertThumb;
    xc.RemoteCommonNames.Add(name);
    xc.RemoteCertThumbprints.Add(serverCertThumb);
    xc.ProtectionLevel = ProtectionLevel.EncryptAndSign;
    return xc;
}
```

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a>Etkileşimli olarak Azure Active Directory kullanarak güvenli bir kümeye bağlanma

Aşağıdaki örnek, Azure Active Directory istemci kimliği ve sunucu sertifikası sunucu kimliği için kullanır.

Bir iletişim penceresi otomatik olarak etkileşimli oturum açma için kümeye bağlandıktan sonra açılır.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}
```

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a>Etkileşimli olmayan Azure Active Directory kullanarak güvenli bir kümeye bağlanma

Aşağıdaki örnek, Microsoft.IdentityModel.Clients.activedirectory, sürüm kullanır: 2.19.208020213.

AAD belirteç edinme hakkında daha fazla bilgi için bkz. [Microsoft.IdentityModel.Clients.activedirectory](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).

```csharp
string tenantId = "C15CFCEA-02C1-40DC-8466-FBD0EE0B05D2";
string clientApplicationId = "118473C2-7619-46E3-A8E4-6DA8D5F56E12";
string webApplicationId = "53E6948C-0897-4DA6-B26A-EE2A38A690B4";

string token = GetAccessToken(
    tenantId,
    webApplicationId,
    clientApplicationId,
    "urn:ietf:wg:oauth:2.0:oob");

string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);
claimsCredentials.LocalClaims = token;

var fc = new FabricClient(claimsCredentials, connection);

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(
    string tenantId,
    string resource,
    string clientId,
    string redirectUri)
{
    string authorityFormat = @"https://login.microsoftonline.com/{0}";
    string authority = string.Format(CultureInfo.InvariantCulture, authorityFormat, tenantId);
    var authContext = new AuthenticationContext(authority);

    var authResult = authContext.AcquireToken(
        resource,
        clientId,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Önceki meta veri bilgisi olmadan Azure Active Directory kullanarak güvenli bir kümeye bağlanma

Aşağıdaki örnek, etkileşimli olmayan belirteç edinme işlemi kullanır, ancak aynı yaklaşımı özel etkileşimli belirteç edinme deneyim oluşturmak için kullanılabilir. Belirteç edinme işlemi için gereken Azure Active Directory meta veriler, küme yapılandırmasından okunur.

```csharp
string serverCertThumb = "A8136758F4AB8962AF2BF3F27921BE1DF67F4326";
string connection = "clustername.westus.cloudapp.azure.com:19000";

var claimsCredentials = new ClaimsCredentials();
claimsCredentials.ServerThumbprints.Add(serverCertThumb);

var fc = new FabricClient(claimsCredentials, connection);

fc.ClaimsRetrieval += (o, e) =>
{
    return GetAccessToken(e.AzureActiveDirectoryMetadata);
};

try
{
    var ret = fc.ClusterManager.GetClusterManifestAsync().Result;
    Console.WriteLine(ret.ToString());
}
catch (Exception e)
{
    Console.WriteLine("Connect failed: {0}", e.Message);
}

...

static string GetAccessToken(AzureActiveDirectoryMetadata aad)
{
    var authContext = new AuthenticationContext(aad.Authority);

    var authResult = authContext.AcquireToken(
        aad.ClusterApplication,
        aad.ClientApplication,
        new UserCredential("TestAdmin@clustenametenant.onmicrosoft.com", "TestPassword"));
    return authResult.AccessToken;
}

```

<a id="connectsecureclustersfx"></a>

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a>Service Fabric Explorer kullanarak güvenli bir kümeye bağlanma
Ulaşmak için [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) belirli bir küme için tarayıcınızı noktası:

`http://<your-cluster-endpoint>:19080/Explorer`

Tam URL'si Azure portal'ın küme temel bileşenler bölmesinde kullanılabilir.

Windows veya OS X bir tarayıcı kullanarak güvenli bir kümeye bağlanmak için istemci sertifikasını içeri aktarın ve tarayıcı sizin için kümeye bağlanmak için kullanılacak sertifikayı ister.  Linux makinelerinde, sertifika (her tarayıcıda farklı mekanizmalar vardır) gelişmiş tarayıcı ayarlarınızı kullanarak içe aktarılması ve diskte sertifika konuma işaret gerekecektir. Okuma [istemci sertifika ayarlama](#connectsecureclustersetupclientcert) daha fazla bilgi için.

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Azure Active Directory kullanarak güvenli bir kümeye bağlanma

AAD ile güvenli bir kümeye bağlanmak için tarayıcınızı noktası:

`https://<your-cluster-endpoint>:19080/Explorer`

Otomatik olarak sorulur AAD oturum açmak için.

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Bir istemci sertifikası ile güvenli bir kümeye bağlanma

Sertifikaları güvenli bir kümeye bağlanmak için tarayıcınızı noktası:

`https://<your-cluster-endpoint>:19080/Explorer`

Otomatik olarak sorulur bir istemci sertifikası seçin.

<a id="connectsecureclustersetupclientcert"></a>

## <a name="set-up-a-client-certificate-on-the-remote-computer"></a>Uzak bilgisayardaki bir istemci sertifikası ayarlama

Bir küme ve sunucu sertifikası ve istemci erişimi için başka bir kümenin güvenliğini sağlamak için en az iki sertifika kullanılmalıdır.  Ayrıca ek ikincil sertifika ve istemci erişim sertifikası kullanmanızı öneririz.  Bir istemci sertifikası güvenlik kullanarak bir küme düğümü arasındaki iletişimin güvenliğini sağlamak için önce alma ve istemci sertifikasını yükleme gerekir. Kişisel (My) deposuna, yerel bilgisayar ya da geçerli kullanıcı sertifika yüklenebilir.  Böylece istemci, küme kimlik doğrulaması yapabilir sunucu sertifikasının parmak izini de gerekir.

* Windows'da: PFX dosyasına çift tıklayın ve kişisel deponuza sertifikayı yüklemek için istemleri takip edin `Certificates - Current User\Personal\Certificates`. Alternatif olarak, PowerShell komutunu kullanabilirsiniz:

    ```powershell
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
            -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
            -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
    ```

    Otomatik olarak imzalanan sertifika ise, güvenli bir kümeye bağlanmak için bu sertifika kullanabilmeniz için önce makinenizin "güvenilir kişiler" depoya almanız gerekir.

    ```powershell
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
    -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
    -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
    ```

* Mac'te: PFX dosyasına çift tıklayın ve Anahtarlığınıza sertifikayı yüklemek için istemleri takip edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric kümesini yükseltme işlemi ve sizden beklentileri](service-fabric-cluster-upgrade.md)
* [Visual Studio'da Service Fabric uygulamalarınızı yönetme](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric sistem durumu modeli giriş](service-fabric-health-introduction.md)
* [Uygulama güvenliği ve farklı çalıştır](service-fabric-application-runas-security.md)
* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)
