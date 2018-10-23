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
ms.openlocfilehash: 7bac2b291bec2ceda118c877cde997a2fa9f9f37
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49322983"
---
1. [Azure Portal](http://portal.azure.com) oturum açın.

1. Sol gezinti bölmesinden **Kaynak oluştur**'a tıklayın. *digital twins* terimini aratın ve **Digital Twins (önizleme)** hizmetini seçin. Dağıtım işlemini başlatmak için **Oluştur**’a tıklayın.

    ![Digital Twins oluşturma](./media/create-digital-twins-portal/create-digital-twins.png)

1. **Digital Twins** bölmesine şu bilgileri girin:
   * **Kaynak Adı**: Digital Twins örneğiniz için benzersiz bir ad oluşturun.
   * **Abonelik**: Bu Digital Twins örneğini oluşturmak için kullanmak istediğiniz aboneliği seçin. 
   * **Kaynak grubu**: Digital Twins örneği için bir [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) seçin veya oluşturun.
   * **Konum**: Cihazlarınıza en yakın konumu seçin.

    ![Digital Twins oluşturma](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. Digital Twins bilgilerinizi gözden geçirin, ardından **Oluştur**’a tıklayın. Digital Twins örneğinizin oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.

1. Digital Twins örneğinizin **Genel Bakış** bölmesini açın. **Yönetim API'si** bölümündeki bağlantıyı not edin.

    1. **Yönetim API'si** URL'si şu biçime sahiptir: **_https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger_**. Bu URL, örneğiniz için geçerli olan Azure Digital Twins REST API belgesini açar. Bu API belgelerini okumayı ve kullanmayı öğrenmek için bkz. [Azure Digital Twins Swagger'ı kullanma](../articles/digital-twins/how-to-use-swagger.md).

    1. **Yönetim API'si** URL'sini şu şekilde değiştirin: **_https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/_**. Uygulamanız, değiştirilen URL'yi örneğinize erişmek için temel URL olarak kullanır. Değiştirdiğiniz URL'yi geçici bir dosyaya kopyalayın. Bir sonraki bölümde bu bilgiye ihtiyacınız olacak.

    ![Yönetim API’leri](./media/create-digital-twins-portal/digital-twins-management-api.png)