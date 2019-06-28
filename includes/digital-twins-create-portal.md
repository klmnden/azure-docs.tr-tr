---
title: include dosyası
description: include dosyası
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 6/26/2019
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: 9771e312269eb78e0dc4535a61e9287b5b169d7c
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459038"
---
1. [Azure Portal](http://portal.azure.com) oturum açın.

1. Sol bölmeden **kaynak Oluştur**. Arama **dijital ikizlerini**seçip **dijital İkizlerini**. Seçin **Oluştur** dağıtım işlemini başlatmak için.

   ![Seçimleri, yeni bir dijital İkizlerini örneği oluşturma](./media/create-digital-twins-portal/create-digital-twins.png)

1. **Digital Twins** bölmesine şu bilgileri girin:
   * **Kaynak adı**: Dijital İkizlerini Örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu dijital İkizlerini örneği oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: Seçin veya oluşturun bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) dijital İkizlerini örneği.
   * **Konum**: Cihazlarınıza en yakın konumu seçin.

     ![Girilen bilgileri ile dijital İkizlerini bölmesi](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Dijital İkizlerini bilgilerinizi gözden geçirin ve ardından **Oluştur**. Dijital İkizlerini örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Digital Twins örneğinizin **Genel Bakış** bölmesini açın. Altındaki bağlantıyı Not **yönetim API**.

   **Yönetim API** URL olarak biçimlendirileceğini `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger`. Bu URL, örneğiniz için geçerli olan Azure Digital Twins REST API belgesini açar. Bu API belgelerini okumayı ve kullanmayı öğrenmek için bkz. [Azure Digital Twins Swagger'ı kullanma](../articles/digital-twins/how-to-use-swagger.md).

    Değiştirme **yönetim API** URL bu biçime `https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/`. Uygulamanız, değiştirilen URL'yi örneğinize erişmek için temel URL olarak kullanır. Değiştirdiğiniz URL'yi geçici bir dosyaya kopyalayın. Sonraki bölümde bu anahtar gerekir.

    ![Yönetim API'si](./media/create-digital-twins-portal/digital-twins-management-api.png)