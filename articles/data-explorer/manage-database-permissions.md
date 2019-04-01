---
title: Azure Veri Gezgini veritabanı izinlerini yönetme
description: Bu makale, veritabanlarını ve tabloları Azure veri Gezgini'nde için rol tabanlı erişim denetimlerini açıklar.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 3f5f174f5f5e8aa122ab9c280cc812dd64b0b425
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756482"
---
# <a name="manage-azure-data-explorer-database-permissions"></a>Azure Veri Gezgini veritabanı izinlerini yönetme

Azure Veri Gezgini sağlar, veritabanlarını ve tabloları, erişimi denetlemek kullanarak bir *rol tabanlı erişim denetimi* modeli. Bu modelde, *sorumluları* (kullanıcılar, gruplar ve uygulamalar) eşleştirilmiş *rolleri*. İlkeleri atanmış oldukları rollerine göre kaynaklara erişebilir.

Bu makalede, kullanılabilir roller ve ilkeleri Azure portalı ve Azure Veri Gezgini yönetim komutları kullanarak roller atama açıklanmaktadır.

## <a name="roles-and-permissions"></a>Roller ve izinler

Azure Veri Gezgini, aşağıdaki roller vardır:

|Rol                       |İzinler                                                                        |
|---------------------------|-----------------------------------------------------------------------------------|
|Veritabanı Yöneticisi             |Belirli bir veritabanı kapsamında her şeyi yapabilirsiniz.|
|Veritabanı kullanıcısı              |Tüm verileri ve meta verileri veritabanı'nda okuyabilirsiniz. Veritabanında tablolar (Bu tablo için tablo yönetici olma) ve İşlevler ayrıca oluşturabilirler.|
|Veritabanı Görüntüleyicisi            |Tüm verileri ve meta verileri veritabanı'nda okuyabilirsiniz.|
|Veritabanı çıkışlara          |Tüm var olan veritabanı tabloları veri alma, ancak verileri sorgulamak değil.|
|Veritabanı İzleyicisi           |Veritabanı ve onun alt varlıklar bağlamında '.show...' komutları yürütebilir.|
|Tablo yönetici                |Belirli bir tablonun kapsamdaki herhangi bir şey yapabilirsiniz. |
|Tablo çıkışlara             |Kapsamı belirli bir tablonun verileri alma, ancak verileri sorgulamak değil.|

## <a name="manage-permissions-in-the-azure-portal"></a>Azure portalında izinleri Yönet

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Azure Veri Gezgini kümenize gidin.

1. İçinde **genel bakış** bölümünde, izinlerini yönetmek istediğiniz veritabanını seçin.

    ![veritabanı seçin](media/manage-database-permissions/select-database.png)

1. Seçin **izinleri** ardından **ekleme**.

    ![Veritabanı izinleri](media/manage-database-permissions/database-permissions.png)

1. Altında **veritabanı izinleri eklemek**, sorumlu, ardından atamak istediğiniz rolü seçin **seçin sorumluları**.

    ![Veritabanı izinleri ekleme](media/manage-database-permissions/add-permission.png)

1. Sorumlusu arayın, ardından, **seçin**.

    ![Azure portalında izinleri Yönet](media/manage-database-permissions/new-principals.png)

1. **Kaydet**’i seçin.

    ![Azure portalında izinleri Yönet](media/manage-database-permissions/save-permission.png)

## <a name="manage-permissions-with-management-commands"></a>Yönetim komutları izinlerle yönetme

1. Oturum açma için [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com), zaten mevcut değilse, kümenize ekleyin.

1. Sol bölmede, uygun veritabanını seçin.

1. Kullanım `.add` sorumluları rollere atamak için komut: `.add database databasename rolename ('aaduser | aadgroup=user@domain.com')`. Veritabanı kullanıcı rolüne kullanıcı eklemek için veritabanı adı ve kullanıcı değiştirerek aşağıdaki komutu çalıştırın.

    ```Kusto
    .add database <TestDatabase> users ('aaduser=<user@contoso.com>')
    ```

    Komut çıktısı, veritabanında mevcut kullanıcıların listesini ve atandıkları rollerini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

[Sorgu yazma](write-queries.md)
