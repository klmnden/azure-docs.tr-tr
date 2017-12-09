---
title: "Windows Azure Service Fabric kümesi sertifikaları kullanarak güvenli hale getirme | Microsoft Docs"
description: "Tek başına veya şirket içi bir Azure Service Fabric kümesindeki yanı sıra istemciler ve küme arasındaki iletişimin güvenliğini sağlamak."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: fe0ed74c-9af5-44e9-8d62-faf1849af68c
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: dd09a4df42c1022c2a9f96daf69591bbfc777d79
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-x509-certificates"></a>Windows tek başına kümede X.509 sertifikaları kullanarak güvenli hale getirme
Bu makalede, tek başına Windows kümenizi çeşitli düğümleri arasındaki iletişimin güvenliğini sağlamak açıklar. Ayrıca, bu kümeye X.509 sertifikalarını kullanarak bağlanan istemcilerin kimliğini doğrulamak nasıl açıklanır. Kimlik doğrulaması, yalnızca yetkili kullanıcılar küme ve dağıtılan uygulamalar erişim ve yönetim görevlerini gerçekleştirme sağlar. Küme oluşturulduğunda sertifika güvenliği kümede etkinleştirilmelidir.  

Düğümü düğümü güvenlik, istemci düğümü güvenlik ve rol tabanlı erişim denetimi gibi küme güvenlik üzerinde daha fazla bilgi için bkz: [küme güvenlik senaryoları](service-fabric-cluster-security.md).

## <a name="which-certificates-do-you-need"></a>Hangi sertifikaların gerekiyor?
İle başlamak [Windows Server için Service Fabric paketini karşıdan](service-fabric-cluster-creation-for-windows-server.md#download-the-service-fabric-for-windows-server-package) kümenizdeki düğümlerin birinde. İndirilen paketteki bir ClusterConfig.X509.MultiMachine.json dosyasını bulun. Dosyasını açın ve güvenlik özellikler bölümü altında bölümü gözden geçirin:

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
                "CertificateIssuerThumbprint": "[Thumbprint]",
                "IsAdmin": true
            }
        ],
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

Bu bölümde, tek başına Windows kümeniz güvenli hale getirmek için gereken sertifikaları açıklanmaktadır. Bir küme sertifika belirtirseniz, ClusterCredentialType için değerini _X509_. Dış bağlantılar için bir sunucu sertifikası belirtirseniz, ServerCredentialType kümesine _X509_. Zorunlu değil, ancak bu sertifikalar düzgün güvenli bir küme için her ikisini de sahip öneririz. Bu değerleri ayarlamak, *X509*, karşılık gelen sertifikaları belirtmeniz gerekir veya Service Fabric bir özel durum oluşturur. Bazı senaryolarda, yalnızca belirtmek isteyebileceğiniz _ClientCertificateThumbprints_ veya _ReverseProxyCertificate_. Bu senaryolarda ayarlamanız gerekmez _ClusterCredentialType_ veya _ServerCredentialType_ için _X509_.


> [!NOTE]
> A [parmak izi](https://en.wikipedia.org/wiki/Public_key_fingerprint) bir sertifika birincil kimliğidir. Oluşturduğunuz sertifika parmak izi bulmak için bkz: [bir bir sertifikanın parmak izini alma](https://msdn.microsoft.com/library/ms734695.aspx).
> 
> 

Aşağıdaki tabloda, Küme kurulumu gereken sertifikaları listelenmektedir:

| **CertificateInformation ayarı** | **Açıklama** |
| --- | --- |
| ClusterCertificate |Bir test ortamı için önerilir. Bu sertifika, bir küme düğümlerinde arasındaki iletişimin güvenliğini sağlamak için gereklidir. İki farklı sertifikaları, birincil ve ikincil bir yükseltme için kullanabilirsiniz. Parmak izi bölüm ve ikincil ThumbprintSecondary değişkenlerine sertifikanın parmak izi birincil ayarlayın. |
| ClusterCertificateCommonNames |Bir üretim ortamı için önerilir. Bu sertifika, bir küme düğümlerinde arasındaki iletişimin güvenliğini sağlamak için gereklidir. Bir veya iki küme sertifika ortak adları kullanabilirsiniz. Bu sertifika verenin parmak izi için CertificateIssuerThumbprint karşılık gelir. Aynı ortak ada sahip birden fazla sertifika kullandıysanız, birden çok sertifikayı verenin parmak izleri belirtebilirsiniz.|
| ServerCertificate |Bir test ortamı için önerilir. Bu kümeye bağlanmaya çalıştığında bu sertifikayı istemciye sunulur. Kolaylık olması için ClusterCertificate ve ServerCertificate için aynı sertifika kullanmayı seçebilirsiniz. İki farklı sunucu sertifikaları, birincil ve ikincil bir yükseltme için kullanabilirsiniz. Parmak izi bölüm ve ikincil ThumbprintSecondary değişkenlerine sertifikanın parmak izi birincil ayarlayın. |
| ServerCertificateCommonNames |Bir üretim ortamı için önerilir. Bu kümeye bağlanmaya çalıştığında bu sertifikayı istemciye sunulur. Bu sertifika verenin parmak izi için CertificateIssuerThumbprint karşılık gelir. Aynı ortak ada sahip birden fazla sertifika kullandıysanız, birden çok sertifikayı verenin parmak izleri belirtebilirsiniz. Kolaylık olması için ClusterCertificateCommonNames ve ServerCertificateCommonNames için aynı sertifika kullanmayı seçebilirsiniz. Bir veya iki sunucu sertifika ortak adları kullanabilirsiniz. |
| ClientCertificateThumbprints |Bu sertifikalar kümesini kimliği doğrulanmış istemcilerde yükleyin. Bir dizi farklı istemci sertifikaları, küme erişmesine izin vermek istediğiniz makinelerde yüklü olabilir. Her sertifikanın parmak izi CertificateThumbprint değişkeninde ayarlayın. IsAdmin ayarlarsanız *doğru*, istemcinin yüklü bu sertifikayla yönetici küme yönetimi etkinliklerini yapabilirsiniz. IsAdmin ise *yanlış*, bu sertifika ile istemci kullanıcı erişim haklarını, salt okunur genellikle yalnızca izin verilen eylemleri gerçekleştirebilirsiniz. Rolleri hakkında daha fazla bilgi için bkz: [rol tabanlı erişim denetimi (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac). |
| ClientCertificateCommonNames |İlk istemci sertifikasının ortak adı için CertificateCommonName ayarlayın. Bu sertifika verenin parmak izini CertificateIssuerThumbprint olur. Ortak adları ve veren hakkında daha fazla bilgi için bkz: [iş sertifikalarla](https://msdn.microsoft.com/library/ms731899.aspx). |
| ReverseProxyCertificate |Bir test ortamı için önerilir. Bu isteğe bağlı sertifika olabilir, güvenli hale getirmek istiyorsanız belirtilen, [ters proxy](service-fabric-reverseproxy.md). Bu sertifika kullanıyorsanız bu reverseProxyEndpointPort nodeTypes ayarlandığından emin olun. |
| ReverseProxyCertificateCommonNames |Bir üretim ortamı için önerilir. Bu isteğe bağlı sertifika olabilir, güvenli hale getirmek istiyorsanız belirtilen, [ters proxy](service-fabric-reverseproxy.md). Bu sertifika kullanıyorsanız bu reverseProxyEndpointPort nodeTypes ayarlandığından emin olun. |

Burada küme, sunucu ve istemci sertifikalarını sağlanmış olan bir örnek küme yapılandırma İşte. Sunucu/küme/reverseProxy sertifikaları için parmak izi ve ortak ad birlikte aynı sertifika türü için yapılandırılamaz.

 ```JSON
 {
    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",
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
        }
        "security": {
            "metadata": "The Credential type X509 indicates this cluster is secured by using X509 certificates. The thumbprint format is d5 ec 42 3b 79 cb e5 07 fd 83 59 3c 56 b9 d5 31 24 25 42 64.",
            "ClusterCredentialType": "X509",
            "ServerCredentialType": "X509",
            "CertificateInformation": {
                "ClusterCertificateCommonNames": {
                  "CommonNames": [
                    {
                      "CertificateCommonName": "myClusterCertCommonName",
                      "CertificateIssuerThumbprint": "7c fc 91 97 13 66 8d 9f a8 ee 71 2b a2 f4 37 62 00 03 49 0d"
                    }
                  ],
                  "X509StoreName": "My"
                },
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

## <a name="certificate-rollover"></a>Sertifika aktarma
Sertifika ortak adı yerine parmak izi kullandığınızda, sertifika geçişine küme yapılandırması yükseltme gerektirmez. Verenin parmak izi yükseltmeler için yeni parmak izi listesi eski listesiyle kesiştiğinden emin olun. İlk yeni verenin parmak izleri config yükseltmeye yapmak zorunda ve ardından yeni bir sertifika (küme/sunucu sertifikası ve veren sertifikaları) deposuna yükleyin. Eski sertifikayı sertifika deposuna yeni sertifikayı yükledikten sonra en az iki saat için tutun.

## <a name="acquire-the-x509-certificates"></a>X.509 sertifikaları alma
Küme içindeki iletişimin güvenliğini sağlamak için önce Küme düğümlerinizi X.509 sertifikalarını edinmeniz gerekir. Ek olarak, yetkili makineler/kullanıcıların bu kümeye bağlantı sınırlamak için edinilir ve istemci makineleri için sertifikalar gerekir.

Üretim iş yükleri çalıştıran kümeleri kullanan bir [sertifika yetkilisi (CA)](https://en.wikipedia.org/wiki/Certificate_authority)-küme güvenli hale getirmek için X.509 sertifikası imzalanmış. Bu sertifikaları edinme hakkında daha fazla bilgi için bkz: [bir sertifika edinme](http://msdn.microsoft.com/library/aa702761.aspx).

Test amaçları için kullandığınız kümeler için otomatik olarak imzalanan bir sertifika kullanmayı da tercih edebilirsiniz.

## <a name="optional-create-a-self-signed-certificate"></a>İsteğe bağlı: otomatik olarak imzalanan bir sertifika oluşturun
Doğru bir şekilde güvenli hale getirilebilir otomatik olarak imzalanan bir sertifika oluşturmak için bir yol, C:\Program Files\Microsoft SDKs\Service Fabric\ClusterSetup\Secure directory Service Fabric SDK klasöründe CertSetup.ps1 komut dosyası kullanmaktır. Sertifikayı varsayılan adını değiştirmek için bu dosyayı düzenleyin. (CN değeri Ara ServiceFabricDevClusterCert =.) Bu komut dosyası olarak çalıştıracak `.\CertSetup.ps1 -Install`.

Şimdi sertifika korumalı bir parola ile bir .pfx dosyasına dışarı aktarın. İlk olarak, sertifikanın parmak izini edinin. 
1. Gelen **Başlat** çalıştırmak menüsünde **bilgisayar sertifikalarını yönetmek**. 

2. Git **yerel Bilgisayar\Kişisel** klasörü ve oluşturduğunuz bulma sertifika. 

3. Açmak için seçin sertifikayı çift tıklatın **ayrıntıları** sekmesini tıklatın ve doğru aşağı kaydırın **parmak izi** alan. 

4. Boşlukları kaldırın ve parmak izi değerini aşağıdaki PowerShell komutunu kopyalayabilir. 

5. Değişiklik `String` koruyun ve PowerShell içinde aşağıdaki çalıştırmak için uygun bir güvenli parola değerine:

   ```powershell   
   $pswd = ConvertTo-SecureString -String "1234" -Force –AsPlainText
   Get-ChildItem -Path cert:\localMachine\my\<Thumbprint> | Export-PfxCertificate -FilePath C:\mypfx.pfx -Password $pswd
   ```

6. Makinede yüklü bir sertifika ayrıntılarını görmek için aşağıdaki PowerShell komutunu çalıştırın:

   ```powershell
   $cert = Get-Item Cert:\LocalMachine\My\<Thumbprint>
   Write-Host $cert.ToString($true)
   ```

Bir Azure aboneliğiniz varsa, bunun yerine, bölümdeki adımları [sertifikalar, anahtar Kasası'na eklemek](service-fabric-cluster-creation-via-arm.md#add-certificates-to-your-key-vault).

## <a name="install-the-certificates"></a>Sertifika Yükleme
Sertifikaları aldıktan sonra küme düğümlerinde yükleyebilirsiniz. Düğümleriniz en son Windows PowerShell gerek 3.x yüklü. Bu adımları her düğümde Küme ve sunucu sertifikaları ve tüm ikincil sertifikaları için yineleyin.

1. .Pfx dosyasını veya dosyaların düğüme kopyalayın.

2. Bir yönetici olarak bir PowerShell penceresi açın ve aşağıdaki komutları yazın. Değiştir *$pswd* bu sertifikayı oluşturmak için kullanılan parola ile. Değiştir *$PfxFilePath* .pfx tam yoluna sahip bu düğüme kopyalanır.
   
    ```powershell
    $pswd = "1234"
    $PfxFilePath ="C:\mypfx.pfx"
    Import-PfxCertificate -Exportable -CertStoreLocation Cert:\LocalMachine\My -FilePath $PfxFilePath -Password (ConvertTo-SecureString -String $pswd -AsPlainText -Force)
    ```
3. Şimdi ağ hizmeti hesabı altında çalışır, Service Fabric işlem, aşağıdaki komut dosyası çalıştırarak kullanabilmesi için bu sertifikaya erişim denetimini ayarlayın. Sertifikanın parmak izini verin ve **ağ hizmeti** hizmet hesabı. Sertifikada açarak sertifika ACL'lerin doğru olduğundan emin olun **Başlat** > **bilgisayar sertifikalarını yönetmek** ve bakarak **tüm görevler**  >  **Özel anahtarları Yönet**.
   
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
4. Her sunucu sertifikası için önceki adımları yineleyin. Küme erişmesine izin vermek istediğiniz makinelere istemci sertifikalarını yüklemek için aşağıdaki adımları da kullanabilirsiniz.

## <a name="create-the-secure-cluster"></a>Güvenli küme oluşturma
ClusterConfig.X509.MultiMachine.json dosyasının güvenlik bölümü yapılandırdıktan sonra geçebilirsiniz [küme oluşturmak](service-fabric-cluster-creation-for-windows-server.md#create-the-cluster) düğümleri yapılandırmak ve tek başına küme oluşturmak için bölüm. Küme oluştururken ClusterConfig.X509.MultiMachine.json dosyası kullanmayı unutmayın. Örneğin, komutunuzu aşağıdakine benzeyebilir:

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

Güvenli tek başına Windows başarıyla çalışan küme ve buna bağlanmak için kimliği doğrulanmış istemcilerini ayarladıktan sonra bölümdeki adımları [PowerShell kullanarak bir kümeye Bağlan](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-powershell) bağlanmak için. Örneğin:

```powershell
$ConnectArgs = @{  ConnectionEndpoint = '10.7.0.5:19000';  X509Credential = $True;  StoreLocation = 'LocalMachine';  StoreName = "MY";  ServerCertThumbprint = "057b9544a6f2733e0c8d3a60013a58948213f551";  FindType = 'FindByThumbprint';  FindValue = "057b9544a6f2733e0c8d3a60013a58948213f551"   }
Connect-ServiceFabricCluster $ConnectArgs
```

Ardından, bu küme ile çalışmak için diğer PowerShell komutları çalıştırabilirsiniz. Örneğin, çalıştırabilirsiniz [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode.md?view=azureservicefabricps) güvenli bu kümede düğümler listesi göstermek için.


Küme kaldırmak için Service Fabric paketi indirdiğiniz küme düğümünde bağlanmak, bir komut satırı açın ve paket klasörüne gidin. Şimdi aşağıdaki komutu çalıştırın:

```powershell
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.X509.MultiMachine.json
```

> [!NOTE]
> Yanlış sertifika yapılandırması, Küme dağıtımı sırasında yaklaşan engelleyebilir. Kendi kendine güvenlik sorunları tanılamak için Olay Görüntüleyicisi'ni Grup Ara **uygulama ve hizmet günlükleri** > **Microsoft Service Fabric**.
> 
> 

