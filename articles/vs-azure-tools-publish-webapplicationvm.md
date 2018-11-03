---
title: Yayımlama WebApplicationVM | Microsoft Docs
description: Bir sanal makine bir web uygulamasına dağıtmayı öğrenin. Bu betik, mevcut olmaması durumunda Azure aboneliğinizde gerekli kaynakları oluşturur.
services: visual-studio-online
author: ghogen
manager: douge
assetId: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: f2b6a4ce611169c6e03479eb71446d4ac059d83e
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50969443"
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Yayımlama WebApplicationVM (Windows PowerShell komut dosyası)
Bir sanal makine için bir web uygulaması dağıtır. Mevcut olmaması durumunda betik, Azure aboneliğinizde gerekli kaynakları oluşturur.

```
Publish-WebApplicationVM
–Configuration <configuration>
-SubscriptionName <subscriptionName>
-WebDeployPackage <packageName>
-VMPassword @{Name = "name"; Password = "password")
-DatabaseServerPassword @{Name = "name"; Password = "password"}
-SendHostMessagesToOutput
-Verbose
```

### <a name="configuration"></a>Yapılandırma
Dağıtım ayrıntılarını açıklayan JSON yapılandırma dosyası yolu.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |true |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="subscriptionname"></a>Abonelik adı
Sanal makineyi oluşturmak istediğiniz Azure aboneliği adı.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |Abonelik ilk abonelik kullanır |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="webdeploypackage"></a>WebDeployPackage
Sanal makineye yayımlamak için web dağıtım paketi yolu. Bu paket, Visual Studio'da Web'i Yayımla Sihirbazı'nı kullanarak oluşturabilirsiniz. Bkz: [nasıl yapılır: Visual Studio'da bir Web dağıtım paketi oluşturma](https://msdn.microsoft.com/library/dd465323.aspx).

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="allowuntrusted"></a>AllowUntrusted
TRUE ise bir güvenilir kök yetkilisi tarafından imzalanmadığını sertifikaların kullanılmasına izin verin.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |false |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="vmpassword"></a>VMPassword
Sanal makine hesabı için kimlik bilgileri. Örnek: - VMPassword @{Name = "Yönetici"; Parola = "password"}

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
Azure SQL veritabanı için kimlik bilgileri. Örnek: - DatabaseServerPassword @{Name = "Yönetici"; Parola = "password"}

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
TRUE ise, yazdırma komut dosyasından çıkış akışına iletileri.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |false |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="remarks"></a>Açıklamalar
Geliştirme ve Test ortamları oluşturmak için komut dosyası kullanmayı tam bir açıklaması için bkz [yayımlamak için geliştirme ve Test ortamları için Windows PowerShell betiklerini kullanarak](vs-azure-tools-publishing-using-powershell-scripts.md).

JSON yapılandırma dosyası, dağıtılacak nedir ayrıntılarını belirtir. Bu ad, benzeşim grubu, VHD görüntüsünü ve boyutunu sanal makine gibi bir proje oluşturduğunuzda, belirttiğiniz bilgileri içerir. Ayrıca veritabanlarını sağlamak için sanal makine üzerindeki uç noktaları varsa içerir ve web dağıtım parametreleri. Aşağıdaki kod, örnek bir JSON yapılandırma dosyası gösterir:

```
{
    "environmentSettings": {
        "cloudService": {
            "name": "myvmname",
            "affinityGroup": "",
            "location": "West US",
            "virtualNetwork": "",
            "subnet": "",
            "availabilitySet": "",
            "virtualMachine": {
                "name": "myvmname",
                "vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201404.01-en.us-127GB.vhd",
                "size": "Small",
                "user": "vmuser1",
                "password": "",
                "enableWebDeployExtension": true,
                "endpoints": [
                    {
                        "name": "Http",
                        "protocol": "TCP",
                        "publicPort": "80",
                        "privatePort": "80"
                    },
                    {
                        "name": "Https",
                        "protocol": "TCP",
                        "publicPort": "443",
                        "privatePort": "443"
                    },
                    {
                        "name": "WebDeploy",
                        "protocol": "TCP",
                        "publicPort": "8172",
                        "privatePort": "8172"
                    },
                    {
                        "name": "Remote Desktop",
                        "protocol": "TCP",
                        "publicPort": "3389",
                        "privatePort": "3389"
                    },
                    {
                        "name": "Powershell",
                        "protocol": "TCP",
                        "publicPort": "5986",
                        "privatePort": "5986"
                    }
                ]
            }
        },
        "databases": [
            {
                "connectionStringName": "",
                "databaseName": "",
                "serverName": "",
                "user": "",
                "password": ""
            }
        ],
        "webDeployParameters": {
            "iisWebApplicationName": "Default Web Site"
        }
    }
}
```

Ne sağlanan değiştirmek için JSON yapılandırma dosyasını düzenleyebilirsiniz. Bir sanal makine ve bulut hizmeti gerekli, ancak veritabanı bölümü isteğe bağlıdır.

