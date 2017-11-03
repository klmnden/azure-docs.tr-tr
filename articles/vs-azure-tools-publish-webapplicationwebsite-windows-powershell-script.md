---
title: "Yayımlama WebApplicationWebSite (Windows PowerShell komut dosyası) | Microsoft Docs"
description: "Bir web projesini Azure Web sitesine yayımlamak öğrenin. Bu komut dosyası yoksa gerekli kaynakları Azure aboneliğinizde oluşturur."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: 07d21b7ce6cd8aee1cff704d316e7a2ca8c00437
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
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
| Diğer adlar |Yok |
| Gerekli mi? |TRUE |
| Konumu |Adlı |
| Varsayılan değer |Yok |
| Ardışık Düzen giriş kabul edilsin mi? |False |
| Joker karakterler kabul edilsin mi? |False |

## <a name="subscriptionname"></a>varlığıyla subscriptionName
Web sitesi oluşturmak istediğiniz Azure aboneliği adı.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |Yok |
| Gerekli mi? |False |
| Konumu |Adlı |
| Varsayılan değer |Yok |
| Ardışık Düzen giriş kabul edilsin mi? |False |
| Joker karakterler kabul edilsin mi? |False |

## <a name="webdeploypackage"></a>WebDeployPackage
Web sitesine yayımlamak için web dağıtım paketi yolu. Visual Studio'da Web'i Yayımla Sihirbazı'nı kullanarak bu paketi oluşturabilirsiniz. Daha fazla bilgi için bkz: [Azure Cloud Services ve ASP.NET kullanmaya başlama](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |Yok |
| Gerekli mi? |False |
| Konumu |Adlı |
| Varsayılan değer |Yok |
| Ardışık Düzen giriş kabul edilsin mi? |False |
| Joker karakterler kabul edilsin mi? |False |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
Kullanıcı adı ve parola Azure SQL veritabanı için.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |Yok |
| Gerekli mi? |False |
| Konumu |Adlı |
| Varsayılan değer |Yok |
| Ardışık Düzen giriş kabul edilsin mi? |False |
| Joker karakterler kabul edilsin mi? |False |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
TRUE ise, yazdırma betikten çıkış akışına iletileri.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |Yok |
| Gerekli mi? |False |
| Konumu |Adlı |
| Varsayılan değer |False |
| Ardışık Düzen giriş kabul edilsin mi? |False |
| Joker karakterler kabul edilsin mi? |False |

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

