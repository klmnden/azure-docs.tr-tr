---
title: Bir Azure Service Fabric kümesindeki Windows sertifikaları kullanılarak güvenli hale getirme | Microsoft Docs
description: Tek başına veya şirket içi bir Azure Service Fabric kümesindeki yanı sıra küme istemciler arasındaki iletişimi güvenli hale getirin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: ee2ce03fccc3e6556f9d261687edb050c8cfa1cc
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661454"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-x509-certificates"></a>X.509 sertifikaları kullanarak Windows üzerinde tek başına küme güvenliğini sağlama
Bu makalede, çeşitli, tek başına Windows küme düğümleri arasındaki iletişimin güvenliğini sağlamak açıklar. Ayrıca, bu kümeye X.509 sertifikaları kullanarak bağlanan istemcilerin kimliğini doğrulamak nasıl açıklar. Kimlik doğrulaması, yalnızca yetkili kullanıcıların küme ve dağıtılan uygulamalar erişim ve yönetim görevlerini gerçekleştirme sağlar. Küme oluşturulduğunda, sertifika güvenliği kümede etkinleştirilmelidir.  

Düğümden düğüme güvenlik istemci düğümü güvenlik ve rol tabanlı erişim denetimi gibi küme güvenliği hakkında daha fazla bilgi için bkz. [küme güvenliği senaryoları](service-fabric-cluster-security.md).

## <a name="which-certificates-do-you-need"></a>Hangi sertifikaların ihtiyacınız var?
İle başlamak [Windows Server için Service Fabric paketini karşıdan](service-fabric-cluster-creation-for-windows-server.md#download-the-service-fabric-for-windows-server-package) küme düğümlerinden biri için. İndirilen paketteki ClusterConfig.X509.MultiMachine.json dosyasını bulun. Dosyasını açın ve Özellikler bölümü altında güvenlik bölümü gözden geçirin:

```JSON
"security": {
    "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
    "ClusterCredentialType": "X509",
    "ServerCredentialType": "X509",
    "CertificateInformation": {
        "ClusterCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },        
        "ClusterCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ClusterCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ServerCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ServerCertificateCommonNames": {
            "CommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]"
            }
            ],
            "X509StoreName": "My"
        },
        "ServerCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames" : "Root"
            }
        ],
        "ClientCertificateThumbprints": [
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": false
            },
            {
                "CertificateThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateCommonNames": [
            {
                "CertificateCommonName": "[CertificateCommonName]",
                "CertificateIssuerThumbprint": "[Thumbprint1,Thumbprint2,Thumbprint3,...]",
                "IsAdmin": true
            }
        ],
        "ClientCertificateIssuerStores": [
            {
                "IssuerCommonName": "[IssuerCommonName]",
                "X509StoreNames": "Root"
            }
        ]
        "ReverseProxyCertificate": {
            "Thumbprint": "[Thumbprint]",
            "ThumbprintSecondary": "[Thumbprint]",
            "X509StoreName": "My"
        },
        "ReverseProxyCertificateCommonNames": {
            "CommonNames": [
                {
                "CertificateCommonName": "[CertificateCommonName]"
                }
            ],
            "X509StoreName": "My"
        }
    }
},
```

Bu bölümde, tek başına Windows kümenizin güvenliğini sağlamak için gereken sertifikaları açıklanmaktadır. Bir küme sertifikası belirtirseniz ClusterCredentialType için değerini _X509_. Dış bağlantıları için bir sunucu sertifikası belirtirseniz ServerCredentialType kümesine _X509_. Zorunlu olsa da, bu sertifikaların düzgün bir şekilde güvenli bir küme için her ikisi de sahip olmasını öneririz. Bu değerleri ayarlamak, *X509*, karşılık gelen sertifikaları da belirtmeniz gerekir veya Service Fabric, bir özel durum oluşturur. Bazı senaryolarda, yalnızca belirtmek isteyebilirsiniz _ClientCertificateThumbprints_ veya _ReverseProxyCertificate_. Bu senaryolarda ayarlamanız gerekmez _ClusterCredentialType_ veya _ServerCredentialType_ için _X509_.


> [!NOTE]
> A [parmak izi](https://en.wikipedia.org/wiki/Public_key_fingerprint) bir sertifika birincil kimliğidir. Oluşturduğunuz sertifikaların parmak izi bulmak için bkz: [bir sertifikanın parmak izini alma](https://msdn.microsoft.com/library/ms734695.aspx).
> 
> 

Aşağıdaki tabloda, küme kurulumunuzu gereken sertifikaları listelenmektedir:

| **CertificateInformation ayarı** | **Açıklama** |
| --- | --- |
| ClusterCertificate |Bir test ortamı için önerilir. Bu sertifika, bir kümedeki düğümlerden arasındaki iletişimin güvenliğini sağlamak için gereklidir. İki farklı sertifikaları, birincil ve ikincil bir yükseltme için kullanabilirsiniz. Parmak izi bölümü ve ikincil ThumbprintSecondary değişkenlerine birincil sertifikanın parmak izini ayarlayın. |
| ClusterCertificateCommonNames |Bir üretim ortamı için önerilir. Bu sertifika, bir kümedeki düğümlerden arasındaki iletişimin güvenliğini sağlamak için gereklidir. Bir veya iki küme sertifikasını ortak adlarını kullanabilirsiniz. Bu sertifika verenin parmak izi için CertificateIssuerThumbprint'karşılık gelir. Ortak aynı ada sahip birden fazla sertifika kullandıysanız, birden çok verenin parmak izleri belirtebilirsiniz.|
| ClusterCertificateIssuerStores |Bir üretim ortamı için önerilir. Bu sertifika için küme sertifikası verenin karşılık gelir. Ortak ad ve karşılık gelen deposu adı altında ClusterCertificateCommonNames verenin parmak izini belirtmek yerine bu bölümünde, dağıtımcı sağlayabilirsiniz.  Bu, geçiş kümesi veren sertifikaları kolaylaştırır. Birden çok verenler, birden fazla küme sertifikası kullanılır belirtilebilir. Boş bir IssuerCommonName beyaz tüm sertifikaların karşılık gelen depolarında X509StoreNames altında belirtilmiş.|
| ServerCertificate |Bir test ortamı için önerilir. Bu sertifika, bu kümeye bağlanmayı denediğinde istemciye sunulur. Kolaylık olması için aynı sertifikayı ClusterCertificate ve ServerCertificate kullanmayı seçebilirsiniz. İki farklı sunucu sertifikaları, birincil ve ikincil bir yükseltme için kullanabilirsiniz. Parmak izi bölümü ve ikincil ThumbprintSecondary değişkenlerine birincil sertifikanın parmak izini ayarlayın. |
| ServerCertificateCommonNames |Bir üretim ortamı için önerilir. Bu sertifika, bu kümeye bağlanmayı denediğinde istemciye sunulur. Bu sertifika verenin parmak izi için CertificateIssuerThumbprint'karşılık gelir. Ortak aynı ada sahip birden fazla sertifika kullandıysanız, birden çok verenin parmak izleri belirtebilirsiniz. Kolaylık olması için aynı sertifikayı ClusterCertificateCommonNames ve ServerCertificateCommonNames kullanmayı seçebilirsiniz. Bir veya iki sunucu sertifika ortak adları kullanabilirsiniz. |
| ServerCertificateIssuerStores |Bir üretim ortamı için önerilir. Bu sertifika için sunucu sertifikasını veren karşılık gelir. Ortak ad ve karşılık gelen deposu adı altında ServerCertificateCommonNames verenin parmak izini belirtmek yerine bu bölümünde, dağıtımcı sağlayabilirsiniz.  Bu, geçiş işlemini sertifikaları veren kolaylaştırır. Birden çok verenler olabilir belirtilen birden fazla sunucu sertifikası kullanılır. Boş bir IssuerCommonName beyaz tüm sertifikaların karşılık gelen depolarında X509StoreNames altında belirtilmiş.|
| ClientCertificateThumbprints |Bu sertifikalar kümesini yetkili istemcilere yükleyin. Bir dizi farklı istemci sertifikaları, küme erişimine izin vermek istediğiniz makinelerde yüklü olabilir. Her bir sertifikanın parmak izini CertificateThumbprint değişkeninde ayarlayın. IsAdmin ayarlamanız *true*, istemci bilgisayarda yüklü sertifika ile yönetici yönetimi etkinlikleri küme üzerinde yapabilir. IsAdmin ise *false*, bu sertifika ile istemci kullanıcı erişim haklarını, salt okunur genellikle yalnızca izin verilen eylemleri gerçekleştirebilirsiniz. Roller hakkında daha fazla bilgi için bkz. [rol tabanlı erişim denetimi (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac). |
| ClientCertificateCommonNames |İlk istemci sertifikası ortak adı için CertificateCommonName ayarlayın. Bu sertifika verenin parmak izi CertificateIssuerThumbprint olur. Yaygın olarak kullanılan adları ve veren hakkında daha fazla bilgi için bkz: [sertifikalarla çalışma](https://msdn.microsoft.com/library/ms731899.aspx). |
| ClientCertificateIssuerStores |Bir üretim ortamı için önerilir. Bu sertifika istemci sertifikası (hem yönetim hem de yönetici olmayan rol) yayınlayanla karşılık gelir. Ortak ad ve karşılık gelen deposu adı altında ClientCertificateCommonNames verenin parmak izini belirtmek yerine bu bölümünde, dağıtımcı sağlayabilirsiniz.  Bu, geçiş işlemini istemci veren sertifikaları kolaylaştırır. Birden çok verenler olabilir belirtilen birden fazla istemci sertifikası kullanılır. Boş bir IssuerCommonName beyaz tüm sertifikaların karşılık gelen depolarında X509StoreNames altında belirtilmiş.|
| ReverseProxyCertificate |Bir test ortamı için önerilir. Bu isteğe bağlı bir sertifika olabilir, güvenli istiyorsanız belirtilen, [ters proxy](service-fabric-reverseproxy.md). Bu sertifika kullanıyorsanız bu reverseProxyEndpointPort NodeType ayarlandığından emin olun. |
| ReverseProxyCertificateCommonNames |Bir üretim ortamı için önerilir. Bu isteğe bağlı bir sertifika olabilir, güvenli istiyorsanız belirtilen, [ters proxy](service-fabric-reverseproxy.md). Bu sertifika kullanıyorsanız bu reverseProxyEndpointPort NodeType ayarlandığından emin olun. |

Burada küme, sunucu ve istemci sertifikalarını sağlanmış olan örnek bir küme yapılandırma aşağıdadır. Küme/sunucu/reverseProxy sertifikaları için parmak izi ve ortak adı birlikte aynı sertifika türü için yapılandırılamaz.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "10-2017",
    "nodes": [{
        "nodeName": "vm0",
        "metadata": "Replace the localhost below with valid IP address or FQDN",
        "iPAddress": "10.7.0.5",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "iPAddress": "10.7.0.4",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "10.7.0.6",
        "metadata": "Replace the localhost with valid IP address or FQDN",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],
    "properties": {
        "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
        },
        "security": {
            "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClusterCertificateIssuerStores": [
                    {
                        "IssuerCommonName": "ClusterIssuer1",
                        "X509StoreNames" : "Root"
                    },
                    {
                        "IssuerCommonName": "ClusterIssuer2",
                        "X509StoreNames" : "Root"
                    }
                ],
                "ServerCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myServerCertCommonName",
                      "CertificateIssuerThumbprint": "7c fc 91 97 13 16 8d ff a8 ee 71 2b a2 f4 62 62 00 03 49 0d"
                    }
                  ],
                  "X509StoreName": "My"
                },
                "ClientCertificateThumbprints": [{
                    "CertificateThumbprint": "c4 c18 8e aa a8 58 77 98 65 f8 61 4a 0d da 4c 13 c5 a1 37 6e",
                    "IsAdmin": false
                }, {
                    "CertificateThumbprint": "71 de 04 46 7c 9e d0 54 4d 02 10 98 bc d4 4c 71 e1 83 41 4e",
                    "IsAdmin": true
                }]
            }
        },
        "reliabilityLevel": "Bronze",
        "nodeTypes": [{
            "name": "NodeType0",
            "clientConnectionEndpointPort": "19000",
            "clusterConnectionEndpointPort": "19001",
            "leaseDriverEndpointPort": "19002",
            "serviceConnectionEndpointPort": "19003",
            "httpGatewayEndpointPort": "19080",
            "applicationPorts": {
                "startPort": "20001",
                "endPort": "20031"
            },
            "ephemeralPorts": {
                "startPort": "20032",
                "endPort": "20062"
            },
            "isPrimary": true
        }
         ],
        "fabricSettings": [{
            "name": "Setup",
            "parameters": [{
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
            }, {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
            }]
        }]
    }
}
 ```

## <a name="certificate-rollover"></a>Sertifika geçişi
Parmak izi yerine bir sertifika ortak adına kullandığınızda, sertifika geçişi bir küme yapılandırma yükseltmesi yapılması gerekmez. Verenin parmak izi yükseltmeler için yeni parmak izi listesi eski bir liste ile kesişip emin olun. Yeni sertifikayı verenin parmak izleri ile bir yapılandırma yükseltme yapmak öncelikle olması ve ardından yeni bir sertifika (küme/sunucu sertifikası ve veren sertifikaları) deposuna yükleyin. Eski sertifikayı sertifika deposuna yeni sertifikayı yükledikten sonra en az iki saat için saklayın.
Veren depoları kullanıyorsanız, yapılandırma yükseltme, veren sertifika geçişine gerçekleştirilmesi gerekiyor. Yeni sertifikayı bir ikinci sona erme tarihi ile ilgili sertifika deposuna yükleyin. ve birkaç saat sonra eski sertifikayı kaldırın.

## <a name="acquire-the-x509-certificates"></a>X.509 sertifikaları alma
Küme içindeki iletişimin güvenliğini sağlamak için önce küme düğümleri için X.509 sertifikaları edinmeniz gerekir. Buna ek olarak, yetkili makineleri/kullanıcıların bu kümeye bağlantı sınırlamak için edinilir ve istemci makineleri için sertifikalar gerekir.

Üretim iş yükleri çalıştıran kümeleri kullanan bir [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority)-otomatik olarak imzalanan X.509 Sertifika, kümenizin güvenliğini sağlamak için. Bu sertifikaları edinme hakkında daha fazla bilgi için bkz. [bir sertifikanın nasıl alınacağı](https://msdn.microsoft.com/library/aa702761.aspx).

Test amaçları için kullandığınız kümeler için otomatik olarak imzalanan bir sertifika kullanmayı seçebilirsiniz.

## <a name="optional-create-a-self-signed-certificate"></a>İsteğe bağlı: Otomatik olarak imzalanan sertifika oluşturma
Doğru şekilde güvenli hale getirilebilir otomatik olarak imzalanan bir sertifika oluşturma yöntemlerinden biri, dizin C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure Service Fabric SDK'sı klasöründe CertSetup.ps1 betik kullanmaktır. Varsayılan Sertifika adını değiştirmek için bu dosyayı düzenleyin. (CN değeri Ara ServiceFabricDevClusterCert =.) Bu betik olarak çalıştırmak `.\CertSetup.ps1 -Install`.

Artık sertifika korumalı bir parola ile bir .pfx dosyasına dışarı aktarın. İlk olarak, sertifikanın parmak izini alın. 
1. Gelen **Başlat** menüsü çalıştırma **bilgisayar sertifikalarını yönetme**. 

2. Git **yerel Bilgisayar\Kişisel** klasörünü açın ve oluşturduğunuz bulma sertifika. 

3. Açmak için seçmek için sertifikayı çift tıklatın **ayrıntıları** sekmesini ve ekranı aşağı kaydırarak **parmak izi** alan. 

4. Boşlukları Kaldır ve aşağıdaki PowerShell komutunu parmak izi değerini kopyalayın. 

5. Değişiklik `String` değeri için uygun güvenli bir parola, korumak ve PowerShell'de aşağıdaki komutu çalıştırın:

   ```powershell   
   $pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
   Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
   ```

6. Makinede yüklü bir sertifika ayrıntılarını görmek için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   $cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
   Write-Host $cert.ToString($true)
   ```

Bir Azure aboneliğiniz varsa, alternatif olarak, adımları [Azure Resource Manager'ı kullanarak bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-arm.md).

## <a name="install-the-certificates"></a>Sertifikaları yükleme
Sertifika aldıktan sonra küme düğümleri üzerinde yükleyebilirsiniz. Düğümlerinizi en son Windows PowerShell gerek 3.x yüklüdür. Küme ve sunucu sertifikaları ve ikincil sertifikalar için her düğümde aşağıdaki adımları yineleyin.

1. .Pfx dosyasını veya dosyaların düğüme kopyalayın.

2. Yönetici olarak bir PowerShell penceresi açın ve aşağıdaki komutları girin. Değiştirin *$pswd* bu sertifikayı oluşturmak için kullanılan parola. Değiştirin *$PfxFilePath* .pfx tam yoluyla bu düğüme kopyalar.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Şimdi ağ hizmeti hesabı altında çalışan, Service Fabric işlem, aşağıdaki betiği çalıştırarak kullanabilmesi için bu sertifikaya erişim denetimini ayarlayın. Sertifikanın parmak izi sağlayın ve **ağ hizmeti** hizmet hesabı. Sertifikada açarak sertifika ACL'lerin doğru olduğunu kontrol edebilirsiniz **Başlat** > **bilgisayar sertifikalarını yönetme** bakarak **tüm görevler**  >  **Özel anahtarları Yönet**.
   
    ```powershell
    param
    (
    [Parameter(Position=1, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$pfxThumbPrint,
   
    [Parameter(Position=2, Mandatory=$true)]
    [ValidateNotNullOrEmpty()]
    [string]$serviceAccount
    )
   
    $cert = Get-ChildItem -Path cert:\LocalMachine\My | Where-Object -FilterScript { $PSItem.ThumbPrint -eq $pfxThumbPrint; }
   
    # Specify the user, the permissions, and the permission type
    $permission = "$($serviceAccount)","FullControl","Allow"
    $accessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $permission
   
    # Location of the machine-related keys
    $keyPath = Join-Path -Path $env:ProgramData -ChildPath "\Microsoft\Crypto\RSA\MachineKeys"
    $keyName = $cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName
    $keyFullPath = Join-Path -Path $keyPath -ChildPath $keyName
   
    # Get the current ACL of the private key
    $acl = (Get-Item $keyFullPath).GetAccessControl('Access')
   
    # Add the new ACE to the ACL of the private key
    $acl.SetAccessRule($accessRule)
   
    # Write back the new ACL
    Set-Acl -Path $keyFullPath -AclObject $acl -ErrorAction Stop
   
    # Observe the access rights currently assigned to this certificate
    get-acl $keyFullPath| fl
    ```
4. Her bir sunucu sertifikası için önceki adımı yineleyin. Ayrıca, küme erişimine izin vermek istediğiniz makinelerde istemci sertifikaları yüklemek için aşağıdaki adımları kullanabilirsiniz.

## <a name="create-the-secure-cluster"></a>Güvenli küme oluşturma
ClusterConfig.X509.MultiMachine.json dosyasının güvenlik bölümünü yapılandırdıktan sonra devam edebilirsiniz [küme oluşturma](service-fabric-cluster-creation-for-windows-server.md#create-the-cluster) düğümleri yapılandırmak ve tek başına küme oluşturma için bölümü. Kümeyi oluştururken ClusterConfig.X509.MultiMachine.json dosyası kullanmayı unutmayın. Örneğin, komutu aşağıdaki gibi görünebilir:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Güvenli tek başına Windows Küme başarıyla çalıştığını ve bağlanmak için kimlik doğrulamasından geçen istemcilerin ayarladıktan sonra bu bölümdeki adımları [PowerShell kullanarak bir kümeye Bağlan](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-powershell) bağlanacağım. Örneğin:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Ardından, bu bir küme ile çalışmanıza için diğer PowerShell komutlarını çalıştırabilirsiniz. Örneğin, çalıştırabileceğiniz [Get-ServiceFabricNode](https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) güvenli bu kümede düğümlerin listesini göstermek için.


Kümeyi kaldırmak için Service Fabric paketini indirdiğiniz kümedeki düğüme bağlanmak, bir komut satırı açın ve paket klasörüne gidin. Artık aşağıdaki komutu çalıştırın:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Yanlış sertifika yapılandırması, Küme dağıtımı sırasında yakında engelleyebilir. Kendi kendine güvenlik sorunlarını tanılamak için Olay Görüntüleyicisi Grup Ara **uygulama ve hizmet günlükleri** > **Microsoft Service Fabric**.
> 
> 

