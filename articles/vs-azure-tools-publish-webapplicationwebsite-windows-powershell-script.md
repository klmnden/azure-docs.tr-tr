---
title: Yayımlama WebApplicationWebSite (Windows PowerShell komut dosyası) | Microsoft Docs
description: Bir web projesini Azure Web sitesine yayımlamak öğrenin. Bu komut dosyası yoksa gerekli kaynakları Azure aboneliğinizde oluşturur.
services: visual-studio-online
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: aaa1f679b0368b0ca93305fe867a63f3971a788c
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Yayımlama WebApplicationWebSite (Windows PowerShell komut dosyası)
## <a name="syntax"></a>Sözdizimi
Bir web projesini Azure Web sitesine yayımlar. Komut dosyası yoksa gerekli kaynakları Azure aboneliğinizde oluşturur.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Yapılandırma
Dağıtım ayrıntılarını açıklayan JSON yapılandırma dosyasının yolu.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |true |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="subscriptionname"></a>Abonelik adı
Web sitesi oluşturmak istediğiniz Azure aboneliği adı.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="webdeploypackage"></a>WebDeployPackage
Web sitesine yayımlamak için web dağıtım paketi yolu. Visual Studio'da Web'i Yayımla Sihirbazı'nı kullanarak bu paketi oluşturabilirsiniz. Daha fazla bilgi için bkz: [Azure Cloud Services ve ASP.NET kullanmaya başlama](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
Kullanıcı adı ve parola Azure SQL veritabanı için.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |yok |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
TRUE ise, yazdırma betikten çıkış akışına iletileri.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |Adlı |
| Varsayılan değer |false |
| Ardışık Düzen giriş kabul edilsin mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="remarks"></a>Açıklamalar
Geliştirme ve Test ortamları, komut dosyası oluşturmak için nasıl kullanılacağını tam bir açıklaması için bkz: [geliştirme ve Test ortamları için yayımlamak için Windows PowerShell betiklerini kullanarak](vs-azure-tools-publishing-using-powershell-scripts.md).

JSON yapılandırma dosyası dağıtılacak nedir ayrıntılarını belirtir. Proje adı ve Web sitesi için kullanıcı adı gibi oluşturduğunuzda belirtilen bilgileri içerir. Ayrıca veritabanını hazırlamak, varsa içerir. Aşağıdaki kod örnek bir JSON yapılandırma dosyası gösterir:

    {
        "environmentSettings": {
            "webSite": {
                "name": "WebApplication10554",
                "location": "West US"
            },
            "databases": [
                {
                    "connectionStringName": "DefaultConnection",
                    "databaseName": "WebApplication10554_db",
                    "serverName": "iss00brc88",
                    "user": "sqluser2",
                    "password": "",
                    "edition": "",
                    "size": "",
                    "collation": "",
                    "location": "West US"
                }
            ]
        }
    }

Ne dağıtılan değiştirmek için JSON yapılandırma dosyasını düzenleyebilirsiniz. Bir Web Bölümü gereklidir, ancak veritabanı bölümü isteğe bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [Yayımla-WebApplicationVM (Windows PowerShell komut dosyası)](vs-azure-tools-publish-webapplicationvm.md)

