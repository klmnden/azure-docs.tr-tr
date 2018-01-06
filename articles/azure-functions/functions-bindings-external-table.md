---
title: "Azure işlevleri (Deneysel) için dış tablo bağlama"
description: "Dış tablo bağlamaları Azure işlevlerini kullanma"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 8a4358fa67e45d0b7a2df1519d649099b5ef5850
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="external-table-binding-for-azure-functions-experimental"></a>Azure işlevleri (Deneysel) için dış tablo bağlama

Bu makalede, Sharepoint ve Azure işlevlerinde Dynamics gibi SaaS sağlayıcıları tablo verilerle çalışmak açıklanmaktadır. Giriş ve dış tablolar için bağlamaları çıktı Azure işlevleri destekler.

> [!IMPORTANT]
> Dış tablo bağlama Deneysel ve hiçbir zaman genellikle kullanılabilir (GA) durumuna ulaşmasını. Yalnızca Azure'da dahil 1.x işlevler ve Azure işlevleri eklemek için herhangi bir plan yok 2.x. SaaS sağlayıcıları veri erişimi gerektiren senaryolar için kullanmayı [işlevlerini çağırma logic apps](functions-twitter-email.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API bağlantıları

Tablo bağlamaları üçüncü taraf SaaS sağlayıcıları ile kimlik doğrulaması için harici API bağlantıları yararlanın. 

Bir bağlama atarken yeni bir API bağlantı oluşturabilir veya var olan bir API bağlantısını aynı kaynak grubunda kullanabilirsiniz.

### <a name="available-api-connections-tables"></a>Kullanılabilir API bağlantıları (tablolar)

|Bağlayıcı|Tetikleyici|Girdi|Çıktı|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 işlemleri için](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Google E-Tablolar](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Mali Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Veritabanı](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Ortak veri hizmeti](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x

> [!NOTE]
> Dış tablo bağlantıları da kullanılabilir [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="creating-an-api-connection-step-by-step"></a>Bir API bağlantı oluşturma: adım adım

1. Azure portal sayfasında işlevi uygulamanız için artı işaretini seçin (**+**) bir işlev oluşturmak için.

1. İçinde **senaryo** kutusunda **Experimental**.

1. Seçin **dış tablo**.

1. Bir dil seçin.

2. Altında **dış tablo bağlantı**, varolan bir bağlantı seçin veya seçin **yeni**.

1. Yeni bir bağlantı için ayarları yapılandırın ve seçin **Authorize**.

1. Seçin **oluşturma** işlevi oluşturmak için.

1. Seçin **tümleştirmek > Dış tablo**.

1. Bağlantıyı hedef tablo kullanacak şekilde yapılandırın. Bu ayarlar, SaaS sağlayıcılar arasında farklılık gösterir. Örnekler aşağıdaki bölümde dahil edilir.

## <a name="example"></a>Örnek

Bu örnek kimliği, Soyadı ve FirstName sütunlarla "Başvurun" adlı bir tablo bağlanır. Kod tablosundaki kişi varlıkları listeler ve adları ve soyadları günlüğe kaydeder.

Burada *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```

C# betik kod aşağıdaki gibidir:

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retrieve table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

### <a name="sql-server-data-source"></a>SQL Server veri kaynağı

SQL Server'ın bu örneği kullanmak için bir tablo oluşturmak için bir komut şöyledir. `dataSetName`"varsayılan."

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets-data-source"></a>Google sayfaları veri kaynağı

Bu örnekte Google belgeleri ile kullanmak üzere bir tablo oluşturmak için bir elektronik tablo adında bir çalışma ile oluşturmak `Contact`. Bağlayıcı, elektronik tablo görünen adı kullanamazsınız. Örneğin dataSetName kullanılacak iç adını (kalın) gereksinimlerini: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  sütun adlarını eklemek `Id`, `LastName`, `FirstName` ilk satırın üzerinde veri sonra doldurma sonraki satır.

### <a name="salesforce"></a>Salesforce

Salesforce ile bu örneği kullanmak için `dataSetName` "varsayılan."

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|----------------------|
|**türü** | ayarlanmalıdır `apiHubTable`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | ayarlanmalıdır `in`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | İşlev kodu olay öğesinde temsil eden değişken adı. | 
|**bağlantı**| API bağlantı dizesi depolar uygulama ayarı tanımlar. Bir API bağlantısı tümleştir UI eklediğinizde, uygulama ayarı otomatik olarak oluşturulur.|
|**dataSetName**|Okumak için tabloyu içeren veri kümesinin adı.|
|**tableName**|Tablonun adı|
|**Entityıd**|Tablo bağlamaları için boş olması gerekir.

Veri kümeleri tablo Bağlayıcısı sağlar ve her bir veri kümesi tabloları içerir. "Varsayılan" varsayılan veri kümesinin adıdır Bir veri kümesi ve bir tablo çeşitli SaaS sağlayıcıları başlıklarını aşağıda listelenmiştir:

|Bağlayıcı|Veri kümesi|Tablo|
|:-----|:---|:---| 
|**SharePoint**|Site|SharePoint listesi
|**SQL**|Database|Tablo 
|**Google sayfası**|Elektronik tablo|Çalışma sayfası 
|**Excel**|Excel dosyası|Sayfa 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
