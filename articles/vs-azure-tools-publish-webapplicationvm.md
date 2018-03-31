---
title: Yayımlama WebApplicationVM | Microsoft Docs
description: Bir sanal makine bir web uygulamasına dağıtmayı öğrenin. Bu komut dosyası yoksa gerekli kaynakları Azure aboneliğinizde oluşturur.
services: visual-studio-online
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: de4cec95-f73f-44d9-babd-9f47f2633cdb
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: 49778b00dc9b1f6a8a11de5e3575599957b753fe
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="publish-webapplicationvm-windows-powershell-script"></a>Yayımlama WebApplicationVM (Windows PowerShell komut dosyası)
Bir sanal makine bir web uygulamasına dağıtır. Komut dosyası yoksa gerekli kaynakları Azure aboneliğinizde oluşturur.

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
Dağıtım ayrıntılarını açıklayan JSON yapılandırma dosyasının yolu.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |true |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="subscriptionname"></a>Abonelik adı
Sanal makineyi oluşturmak istediğiniz Azure aboneliği adı.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |İlk aboneliğe abonelik dosyasındaki kullanır |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="webdeploypackage"></a>WebDeployPackage
Sanal makineye yayımlamak için web dağıtım paketi yolu. Visual Studio'da Web'i Yayımla Sihirbazı'nı kullanarak bu paketi oluşturabilirsiniz. Bkz: [nasıl yapılır: Visual Studio'da bir Web dağıtım paketi oluşturma](https://msdn.microsoft.com/library/dd465323.aspx).

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="allowuntrusted"></a>AllowUntrusted
TRUE ise, bir güvenilen kök yetkilisi tarafından imzalanmadığını sertifikaların kullanılmasına izin verin.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |false |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="vmpassword"></a>VMPassword
Sanal makine hesabı için kimlik bilgileri. Örnek: - VMPassword @{Name = "Yönetici"; Parola = "parola"}

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="databaseserverpassword"></a>DatabaseServerPassword
Azure SQL veritabanı için kimlik bilgileri. Örnek: - DatabaseServerPassword @{Name = "Yönetici"; Parola = "parola"}

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

### <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
TRUE ise, yazdırma betikten çıkış akışına iletileri.

| Diğer adlar | yok |
| --- | --- |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |false |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="remarks"></a>Açıklamalar
Geliştirme ve Test ortamları, komut dosyası oluşturmak için nasıl kullanılacağını tam bir açıklaması için bkz: [geliştirme ve Test ortamları için yayımlamak için Windows PowerShell betiklerini kullanarak](vs-azure-tools-publishing-using-powershell-scripts.md).

JSON yapılandırma dosyası dağıtılacak nedir ayrıntılarını belirtir. Ad, benzeşim grubu, VHD görüntüsü ve boyutunu sanal makine gibi bir proje oluşturduğunuzda, belirttiğiniz bilgileri içerir. Ayrıca sanal makinede sağlamak için veritabanlarını uç noktaları varsa ve dağıtım parametreleri web. Aşağıdaki kod örnek bir JSON yapılandırma dosyası gösterir:

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

Ne sağlandığında değiştirmek için JSON yapılandırma dosyasını düzenleyebilirsiniz. Bir sanal makine ve bulut hizmeti gerekiyor, ancak veritabanı bölümü isteğe bağlıdır.

