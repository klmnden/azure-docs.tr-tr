---
title: Yayımlama WebApplicationWebSite (Windows PowerShell Betiği) | Microsoft Docs
description: Bir Azure Web sitesine bir web projesi yayımlamayı öğrenin. Bu betik, mevcut olmaması durumunda Azure aboneliğinizde gerekli kaynakları oluşturur.
services: visual-studio-online
author: ghogen
manager: douge
assetId: 63cfaa2d-f04d-40dc-8677-345385c278d5
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/11/2016
ms.author: ghogen
ms.openlocfilehash: ea8e36aabb75839a9c301f45a82241e3a859d42a
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056097"
---
# <a name="publish-webapplicationwebsite-windows-powershell-script"></a>Yayımlama WebApplicationWebSite (Windows PowerShell komut dosyası)
## <a name="syntax"></a>Sözdizimi
Bir Azure Web sitesine bir web projesi yayımlar. Mevcut olmaması durumunda betik, Azure aboneliğinizde gerekli kaynakları oluşturur.

    Publish-WebApplicationWebSite
    –Configuration <configuration>
    -SubscriptionName <subscriptionName>
    -WebDeployPackage <packageName>
    -DatabaseServerPassword @{Name = "name"; Password = "password"}
    -SendHostMessagesToOutput
    -Verbose


## <a name="configuration"></a>Yapılandırma
Dağıtım ayrıntılarını açıklayan JSON yapılandırma dosyası yolu.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |true |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="subscriptionname"></a>Abonelik adı
Web sitesi oluşturmak istediğiniz Azure aboneliğini adı.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="webdeploypackage"></a>WebDeployPackage
Web sitesine yayımlamak için web dağıtım paketi yolu. Bu paket, Visual Studio'da Web'i Yayımla Sihirbazı'nı kullanarak oluşturabilirsiniz. Daha fazla bilgi için [Azure Cloud Services ve ASP.NET kullanmaya başlama](http://go.microsoft.com/fwlink/p/?LinkID=623089).

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="databaseserverpassword"></a>DatabaseServerPassword
Kullanıcı adı ve Azure SQL veritabanı için parola.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |yok |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="sendhostmessagestooutput"></a>SendHostMessagesToOutput
TRUE ise, yazdırma komut dosyasından çıkış akışına iletileri.

| Parametre | Varsayılan değer |
| --- | --- |
| Diğer adlar |yok |
| Gerekli mi? |false |
| Konum |adlı |
| Varsayılan değer |false |
| Ardışık giriş yapılabilir mi? |false |
| Joker karakterler kabul edilsin mi? |false |

## <a name="remarks"></a>Açıklamalar
Geliştirme ve Test ortamları oluşturmak için komut dosyası kullanmayı tam bir açıklaması için bkz [yayımlamak için geliştirme ve Test ortamları için Windows PowerShell betiklerini kullanarak](vs-azure-tools-publishing-using-powershell-scripts.md).

JSON yapılandırma dosyası, dağıtılacak nedir ayrıntılarını belirtir. Bu Web sitesi için kullanıcı adı ve adı gibi bir proje oluştururken belirttiğiniz bilgileri içerir. Ayrıca veritabanını sağlama, varsa içerir. Aşağıdaki kod, örnek bir JSON yapılandırma dosyası gösterir:

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

Ne dağıtılır değiştirmek için JSON yapılandırma dosyasını düzenleyebilirsiniz. Bir Web Bölümü gereklidir, ancak veritabanı bölümü isteğe bağlıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [Yayımla-WebApplicationVM (Windows PowerShell komut dosyası)](vs-azure-tools-publish-webapplicationvm.md)

