---
title: Güvenli bir şekilde bir Azure Service Fabric kümesine bağlanma | Microsoft Docs
description: Service Fabric kümesi istemci erişimi kimlik doğrulaması ve istemcileri ile bir küme arasındaki iletişimin güvenliğini açıklar.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 759a539e-e5e6-4055-bff5-d38804656e10
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/10/2018
ms.author: ryanwi
ms.openlocfilehash: 0ce01b62fde690934d97fdefb7720e1be5512f4a
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="connect-to-a-secure-cluster"></a>Güvenli bir kümeye bağlanma

Bir istemci bir Service Fabric küme düğümüne bağlandığında, istemci kimliği doğrulanmış ve güvenli iletişim sertifika güvenliği veya Azure Active Directory (AAD) kullanılarak oluşturulmuş olabilir. Bu kimlik doğrulaması yalnızca yetkili kullanıcılar küme erişebilir ve uygulamaları dağıtılan ve yönetim görevlerini gerçekleştirme sağlar.  Küme oluştururken sertifika veya AAD güvenlik daha önce kümede etkinleştirilmiş olmalıdır.  Küme güvenlik senaryoları hakkında daha fazla bilgi için bkz: [küme güvenlik](service-fabric-cluster-security.md). Sertifikalar ile güvenli bir kümeye bağlanıyorsanız [istemci sertifika ayarlama](service-fabric-connect-to-secure-cluster.md#connectsecureclustersetupclientcert) bilgisayarda kümeye bağlanır. 

<a id="connectsecureclustercli"></a> 

## <a name="connect-to-a-secure-cluster-using-azure-service-fabric-cli-sfctl"></a>Azure Service Fabric CLI (sfctl) kullanarak güvenli bir kümeye bağlanın

Service Fabric CLI (sfctl) kullanarak güvenli bir kümeye bağlanmak için birkaç farklı yolu vardır. Kimlik doğrulaması için bir istemci sertifikası kullanıyorsanız sertifika bilgilerinin küme düğümlerine dağıtılmış olan bir sertifikayla eşleşmesi gerekir. Sertifikanızı sertifika yetkilileri (CA) varsa, güvenilen CA'lar ayrıca belirtmeniz gerekir.

Kullanarak bir küme bağlanabilir `sfctl cluster select` komutu.

İstemci sertifikaları, bir sertifika ve anahtar çifti olarak ya da tek pem dosyası olarak iki farklı fashions belirtilebilir. Parola korumalı için `pem` istenir otomatik olarak parola girmesini dosyaları,.

Pem dosyası olarak istemci sertifikasını belirtmek için dosya yolu belirtin `--pem` bağımsız değişkeni. Örneğin:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

Pem dosyaları herhangi bir komut çalıştırılmadan önce parola istemeyeceğini parola korumalı.

Bir sertifika belirtmek için anahtar çifti kullanımı `--cert` ve `--key` ilgili her dosya için dosya yolları belirtmek için bağımsız değişkenler.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --cert ./client.crt --key ./keyfile.key
```

Bazen test veya geliştirme kümesi güvenliğini sağlamak için kullanılan sertifikalar sertifika doğrulaması başarısız. Sertifika doğrulama atlamak üzere belirtin `--no-verify` seçeneği. Örneğin:

> [!WARNING]
> Kullanmayın `no-verify` Service Fabric kümeleri üretim bağlanırken seçeneği.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --no-verify
```

Ayrıca, güvenilir CA sertifikaları ya da tek tek sertifikaları dizinler için yol belirtebilirsiniz. Bu yolları belirtmek için kullanın `--ca` bağımsız değişkeni. Örneğin:

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem --ca ./trusted_ca
```

Bağlandıktan sonra şunları yapabilirsiniz [diğer sfctl komutlarının çalıştırılmasını](service-fabric-cli.md) kümeyle etkileşim kurmak için.

<a id="connectsecurecluster"></a>

## <a name="connect-to-a-cluster-using-powershell"></a>PowerShell kullanarak bir kümeye bağlanın
PowerShell aracılığıyla bir kümede işlemleri gerçekleştirmeden önce ilk küme bağlantı kurun. Küme bağlantısı verilen PowerShell oturumunda izleyen tüm komutlar için kullanılır.

### <a name="connect-to-an-unsecure-cluster"></a>Güvenli olmayan bir kümeye bağlanın

Güvenli olmayan bir kümeye bağlanmak için küme uç nokta adresine sağlamak **Connect-ServiceFabricCluster** komutu:

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 
```

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak güvenli bir kümeye bağlanın

Küme Yöneticisi erişim yetkisi vermek için Azure Active Directory kullanan güvenli bir kümeye bağlanmak için küme sertifika parmak izini verin ve kullanmak *AzureActiveDirectory* bayrağı.  

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
-ServerCertThumbprint <Server Certificate Thumbprint> `
-AzureActiveDirectory
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Bir istemci sertifikası ile güvenli bir kümeye bağlanın
Yönetici erişim yetkisi vermek için istemci sertifikalarını kullanan güvenli bir kümeye bağlanmak için aşağıdaki PowerShell komutunu çalıştırın. 

#### <a name="connect-using-certificate-common-name"></a>Sertifika ortak adı kullanarak bağlan
Küme sertifika ortak adı ve küme yönetimi için izinleri verildi istemci sertifikası ortak adı sağlayın. Sertifika ayrıntılarını küme düğümlerinde bir sertifika ile eşleşmesi gerekir.

```powershell
Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCommonName <certificate common name>  `
    -FindType FindBySubjectName `
    -FindValue <certificate common name> `
    -StoreLocation CurrentUser `
    -StoreName My 
```
*ServerCommonName* küme düğümlerinde yüklü Sunucu sertifikanın ortak adı. *FindValue* ortak yönetici istemci sertifikası adıdır. Parametreleri doldurulduğunda, komut aşağıdaki gibi görünür:
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

### <a name="connect-to-a-secure-cluster-using-windows-active-directory"></a>Windows Active Directory kullanarak güvenli bir kümeye bağlanın
Tek başına kümenizi AD güvenlik kullanarak dağıtılmışsa, anahtar "WindowsCredential" ekleyerek kümeye bağlanın.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <Cluster FQDN>:19000 `
          -WindowsCredential
```

<a id="connectsecureclusterfabricclient"></a>

## <a name="connect-to-a-cluster-using-the-fabricclient-apis"></a>FabricClient API'lerini kullanarak bir kümeye bağlanın
Service Fabric SDK sağlar [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) küme yönetimi için sınıf. FabricClient API'ları kullanmak için Microsoft.ServiceFabric NuGet paketi alın.

### <a name="connect-to-an-unsecure-cluster"></a>Güvenli olmayan bir kümeye bağlanın

Uzak güvenli olmayan bir kümeye bağlanmak için bir FabricClient örneği oluşturun ve küme adresini sağlayın:

```csharp
FabricClient fabricClient = new FabricClient("clustername.westus.cloudapp.azure.com:19000");
```

Bir küme içinde çalışan için kodu, örneğin, güvenilir bir hizmetinde bir FabricClient oluşturmak *olmadan* küme adresini belirtme. Kod, üzerinde çalışmakta olan düğüm üzerinde yerel yönetim ağ geçidi ek ağ atlama önleme FabricClient bağlanır.

```csharp
FabricClient fabricClient = new FabricClient();
```

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Bir istemci sertifikası ile güvenli bir kümeye bağlanın

Kümedeki düğümler geçerli sertifikaların ortak adı olması gerekir veya SAN DNS adıyla görünür [RemoteCommonNames özelliği](https://docs.microsoft.com/dotnet/api/system.fabric.x509credentials#System_Fabric_X509Credentials_RemoteCommonNames) ayarlamak [FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient). Bu işlem aşağıdaki istemci ve küme düğümleri arasında karşılıklı kimlik doğrulamasını etkinleştirir.

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

### <a name="connect-to-a-secure-cluster-interactively-using-azure-active-directory"></a>Etkileşimli olarak Azure Active Directory'yi kullanarak güvenli bir kümeye bağlanın

Aşağıdaki örnek, Azure Active Directory istemci kimliği ve sunucu sertifikası sunucu kimliği için kullanır.

Bir iletişim kutusu penceresinin otomatik olarak etkileşimli oturum açma için kümeye bağlandıktan sonra açılır.

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

### <a name="connect-to-a-secure-cluster-non-interactively-using-azure-active-directory"></a>Etkileşimsiz Azure Active Directory'yi kullanarak güvenli bir kümeye bağlanın

Aşağıdaki örnek Microsoft.IdentityModel.Clients.activedirectory tarafından üzerinde sürüm kullanır: 2.19.208020213.

AAD belirteci edinme hakkında daha fazla bilgi için bkz: [Microsoft.IdentityModel.Clients.activedirectory tarafından](https://msdn.microsoft.com/library/microsoft.identitymodel.clients.activedirectory.aspx).

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

### <a name="connect-to-a-secure-cluster-without-prior-metadata-knowledge-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak önceki meta veri bilgisi olmadan güvenli bir kümeye bağlanın

Aşağıdaki örnek, etkileşimli olmayan belirteç edinme kullanır, ancak aynı yaklaşımı özel etkileşimli belirteç edinme deneyim oluşturmak için kullanılabilir. Belirteç alımı için gereken Azure Active Directory meta veri kümesi yapılandırmasından okuyun.

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

## <a name="connect-to-a-secure-cluster-using-service-fabric-explorer"></a>Service Fabric Explorer kullanarak güvenli bir kümeye bağlanın
Ulaşmaya [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) verilmiş bir küme için tarayıcınızı noktası:

`http://<your-cluster-endpoint>:19080/Explorer`

Tam URL Azure Portalı'nın küme essentials bölmesinde kullanılabilir.

Windows veya OS X bir tarayıcı kullanarak güvenli bir kümeye bağlanma, istemci sertifikasını içeri aktarabilirsiniz, ve tarayıcı, kümeye bağlanmak için kullanılacak sertifika ister.  Linux makinelerde sertifika (her tarayıcı farklı mekanizmalar vardır) gelişmiş tarayıcı ayarlarınızı kullanarak içe aktarılması ve diskte tehe sertifika konuma noktası gerekir.

### <a name="connect-to-a-secure-cluster-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak güvenli bir kümeye bağlanın

AAD ile güvenli bir kümeye bağlanmak için tarayıcınızı noktası:

`https://<your-cluster-endpoint>:19080/Explorer`

Otomatik olarak sorulur AAD oturum oturum.

### <a name="connect-to-a-secure-cluster-using-a-client-certificate"></a>Bir istemci sertifikası ile güvenli bir kümeye bağlanın

Sertifikaları güvenli bir kümeye bağlanmak için tarayıcınızı noktası:

`https://<your-cluster-endpoint>:19080/Explorer`

Otomatik olarak sorulur bir istemci sertifikası seçin.

<a id="connectsecureclustersetupclientcert"></a>
## <a name="set-up-a-client-certificate-on-the-remote-computer"></a>Uzak bilgisayarda bir istemci sertifikası ayarlama
Bir küme ve sunucu sertifikası ve istemci erişimi için başka bir küme güvenliğini sağlamak için en az iki sertifika kullanılmalıdır.  Ayrıca ek ikincil sertifikalar ve istemci erişim sertifikalarını kullanmanızı öneririz.  İstemci ve sertifika güvenliği kullanarak bir küme düğümünü arasındaki iletişimin güvenliğini sağlamak için önce istemci sertifikası edinin ve yükleyin gerekir. Kişisel (My) deposuna yerel bilgisayar veya geçerli kullanıcı sertifika yüklenebilir.  İstemci küme doğrulanabilmesi Ayrıca sunucu sertifikasının parmak izi gerekir.

İstemci sertifikası kümeye erişmek bilgisayarda ayarlamak için aşağıdaki PowerShell cmdlet'ini çalıştırın.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\My `
        -FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
        -Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

Kendinden imzalı bir sertifika ise, güvenli bir kümeye bağlanmak için bu sertifikayı kullanmadan önce makinenizin "güvenilir kişiler" deposuna içeri aktarmanız gerekir.

```powershell
Import-PfxCertificate -Exportable -CertStoreLocation Cert:\CurrentUser\TrustedPeople `
-FilePath C:\docDemo\certs\DocDemoClusterCert.pfx `
-Password (ConvertTo-SecureString -String test -AsPlainText -Force)
```

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric kümesi yükseltme işlemi ve sizden beklentileri](service-fabric-cluster-upgrade.md)
* [Visual Studio'da, Service Fabric uygulamaları yönetme](service-fabric-manage-application-in-visual-studio.md)
* [Service Fabric sistem durumu modeli giriş](service-fabric-health-introduction.md)
* [Uygulama güvenliği ve farklı çalıştır](service-fabric-application-runas-security.md)
* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)
