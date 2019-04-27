---
title: Azure Data Factory bağlı Hizmetleri Parametreleştirme | Microsoft Docs
description: Azure Data Factory bağlı Hizmetleri Parametreleştirme ve çalışma zamanında dinamik değerler geçirmek hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/18/2018
author: gauravmalhot
ms.author: gamal
manager: craigg
ms.openlocfilehash: 0239c53f98fba201b6d70e1e2212eea36134e30d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60635559"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Azure Data Factory bağlı Hizmetleri Parametreleştirme

Artık, bağlı hizmet Parametreleştirme ve çalışma zamanında dinamik değerler geçirin. Örneğin, Azure SQL veritabanı aynı sunucuda farklı veritabanlarına bağlanmak istiyorsanız, artık bağlantılı hizmet tanımı içinde veritabanı adını parametreleştirebilirsiniz. Bu, her veritabanı için bağlı hizmet Azure SQL veritabanı sunucusunda oluşturmak zorunda kalmasını önler. Örneğin, diğer özellikleri de - bağlı hizmet tanımında parametreleştirebilirsiniz *kullanıcı adı.*

Data Factory kullanıcı Arabiriminde Azure portalı veya bir programlama arabirimi, bağlı hizmetler parametre haline getirmek için kullanabilirsiniz.

> [!TIP]
> Parolalar veya gizli dizileri Parametreleştirme değil tavsiye ederiz. Azure anahtar Kasası'nda, bunun yerine tüm bağlantı dizeleri Store ve Parametreleştirme *gizli dizi adı*.

Yedi dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Desteklenen veri depolar

Şu anda aşağıdaki veri depolarını için Azure portalında Data Factory kullanıcı arabiriminde bağlı hizmet Parametreleştirme desteklenir. Diğer tüm veri depoları için bağlı hizmet seçerek parametreleştirebilirsiniz **kod** simgesine **bağlantıları** sekmesi ve JSON Düzenleyicisi'ni kullanarak.
- Azure SQL Veritabanı
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
