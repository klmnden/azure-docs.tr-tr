---
title: Azure Data Factory bağlı Hizmetleri Parametreleştirme | Microsoft Docs
description: Azure Data Factory bağlı Hizmetleri Parametreleştirme ve çalışma zamanında dinamik değerler geçirmek hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: douglasl
ms.openlocfilehash: 99efd29165f381b9038758c3384774a65da91501
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49649423"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Azure Data Factory bağlı Hizmetleri Parametreleştirme

Artık, bağlı hizmet Parametreleştirme ve çalışma zamanında dinamik değerler geçirin. Örneğin, Azure SQL veritabanı aynı sunucuda farklı veritabanlarına bağlanmak istiyorsanız, artık bağlantılı hizmet tanımı içinde veritabanı adını parametreleştirebilirsiniz. Bu, her veritabanı için bağlı hizmet Azure SQL veritabanı sunucusunda oluşturmak zorunda kalmasını önler. Örneğin, diğer özellikleri de - bağlı hizmet tanımında parametreleştirebilirsiniz *kullanıcı adı.*

Data Factory kullanıcı Arabiriminde Azure portalı veya bir programlama arabirimi, bağlı hizmetler parametre haline getirmek için kullanabilirsiniz.

> [!TIP]
> Parolalar veya gizli dizileri Parametreleştirme değil tavsiye ederiz. Azure anahtar Kasası'nda, bunun yerine tüm bağlantı dizeleri Store ve Parametreleştirme *gizli dizi adı*.

Yedi dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Desteklenen veri depolar

Şu anda aşağıdaki veri depolarını için Azure portalında Data Factory kullanıcı arabiriminde bağlı hizmet Parametreleştirme desteklenir. Diğer tüm veri depoları için bağlı hizmet seçerek parametreleştirebilirsiniz **kod** işlem hattı sekmesine ve JSON Düzenleyicisi'ni kullanarak simgesi.
- Azure SQL Database
- Azure SQL Veri Ambarı
- SQL Server
- Oracle
- Cosmos DB
- Amazon Redshift
- MySQL
- MySQL için Azure Veritabanı

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

![Bağlantılı hizmet tanımına dinamik içerik Ekle](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Yeni parametre oluşturma](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "value": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
                "type": "SecureString"
            }
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
