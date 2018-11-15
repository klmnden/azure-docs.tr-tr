---
title: include dosyası
description: include dosyası
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 09/17/2018
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: e5e015c1e12e68f54ae7fd32082faed1abfebe74
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51626407"
---
1. [Azure Portal](http://portal.azure.com) oturum açın.

1. Sol bölmeden **kaynak Oluştur**. **digital twins** terimini aratın ve **Digital Twins (önizleme)** hizmetini seçin. Seçin **Oluştur** dağıtım işlemini başlatmak için.

   ![Seçimleri, yeni bir dijital İkizlerini örneği oluşturma](./media/create-digital-twins-portal/create-digital-twins.png)

1. **Digital Twins** bölmesine şu bilgileri girin:
   * **Kaynak Adı**: Digital Twins örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu Digital Twins örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: Digital Twins örneği için bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) seçin veya oluşturun.
   * **Konum**: Cihazlarınıza en yakın konumu seçin.

    ![Girilen bilgileri ile dijital İkizlerini bölmesi](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Dijital İkizlerini bilgilerinizi gözden geçirin ve ardından **Oluştur**. Dijital İkizlerini örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Digital Twins örneğinizin **Genel Bakış** bölmesini açın. Altındaki bağlantıyı Not **yönetim API**.

   **Yönetim API** URL olarak biçimlendirileceğini: _https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger_. Bu URL, örneğiniz için geçerli olan Azure Digital Twins REST API belgesini açar. Bu API belgelerini okumayı ve kullanmayı öğrenmek için bkz. [Azure Digital Twins Swagger'ı kullanma](../articles/digital-twins/how-to-use-swagger.md).

    Değiştirme **yönetim API** URL şu biçimi: _https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/_. Uygulamanız, değiştirilen URL'yi örneğinize erişmek için temel URL olarak kullanır. Değiştirdiğiniz URL'yi geçici bir dosyaya kopyalayın. Sonraki bölümde bu anahtar gerekir.

    ![Yönetim API'si](./media/create-digital-twins-portal/digital-twins-management-api.png)