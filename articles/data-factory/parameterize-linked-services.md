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
ms.date: 08/31/2018
ms.author: douglasl
ms.openlocfilehash: 7bc603ceee1f85e6f00cdac9a6ac098abb39e217
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48251127"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Azure Data Factory bağlı Hizmetleri Parametreleştirme

Artık, bağlı hizmet Parametreleştirme ve çalışma zamanında dinamik değerler geçirin. Örneğin, Azure SQL veritabanı aynı sunucuda farklı veritabanlarına bağlanmak istiyorsanız, artık bağlantılı hizmet tanımı içinde veritabanı adını parametreleştirebilirsiniz. Bu, her veritabanı için bağlı hizmet Azure SQL veritabanı sunucusunda oluşturmak zorunda kalmasını önler. Örneğin, diğer özellikleri de - bağlı hizmet tanımında parametreleştirebilirsiniz *kullanıcı adı.*

Data Factory kullanıcı Arabiriminde Azure portalı veya bir programlama arabirimi, bağlı hizmetler parametre haline getirmek için kullanabilirsiniz.

> [!TIP]
> Parolalar veya gizli dizileri Parametreleştirme değil tavsiye ederiz. Azure anahtar Kasası'nda, bunun yerine tüm bağlantı dizeleri Store ve Parametreleştirme *gizli dizi adı*.

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
