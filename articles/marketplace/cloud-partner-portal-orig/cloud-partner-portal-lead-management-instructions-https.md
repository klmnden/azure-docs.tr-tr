---
title: HTTPS uç noktası | Microsoft Docs
description: Https için sağlama yönetimi yapılandırın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: pbutlerm
ms.openlocfilehash: fd13a7281c7e8702fd199364261ebcd458db0555
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811267"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Bir HTTPS uç noktası kullanarak müşteri adayı yönetimini yapılandırma

CRM sisteme yazılabilen Azure Market'in ve Appsource'un müşteri adayları işlemek için bir HTTPS uç noktası'nı kullanabilirsiniz. Bu makalede, Microsoft Flow Otomasyonu hizmetini kullanarak müşteri adayı yönetiminin nasıl yapılandırılacağına dair açıklanır.


## <a name="create-a-flow-using-microsoft-flow"></a>Microsoft Flow kullanarak akış oluşturma

1.  Açık [akış](https://flow.microsoft.com/) Web sayfası. Seçin **oturum** veya **ücretsiz olarak kaydolun** ücretsiz bir Flow hesabı oluşturmak için.

2.  Oturum açın ve seçin **Akışlarım** menü çubuğundaki.

    ![Akışlarım](./media/cloud-partner-portal-lead-management-instructions-https/image001.png)

3.  Seçin **boş akış Oluştur**.

    ![Boş akış oluştur](./media/cloud-partner-portal-lead-management-instructions-https/image003.png)


4.  Seçin **istek/yanıt** bağlayıcı ve istek tetikleyicisi sonra arayın. 

    ![Boş akış oluştur](./media/cloud-partner-portal-lead-management-instructions-https/image005.png)

5. Seçin **istek** tetikleyici.
    ![İstek tetikleyicisi](./media/cloud-partner-portal-lead-management-instructions-https/image007.png)


6.  Kopyalama **JSON örneği** bu makalenin sonunda **istek gövdesi JSON şeması**.

7.  Yeni adım ekleme ve CRM sistemine yeni bir kayıt oluşturmak için istediğiniz eylemi seçin. Sonraki ekran yakalama programları **Dynamics 365 - yeni kayıt oluştur** örnek olarak.

    ![Yeni kayıt oluşturma](./media/cloud-partner-portal-lead-management-instructions-https/image009.png)

8.  Bağlantı girişleri, bağlayıcı ve seçin için sağlama **müşteri adayları** varlık.

    ![Müşteri adaylarını seçin](./media/cloud-partner-portal-lead-management-instructions-https/image011.png)

9.  Akışları müşteri adayı bilgilerini sağlamak için bir form gösterir. Dinamik İçerik Ekle seçerek giriş isteği öğelerinden eşleyebilirsiniz.

    ![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/image013.png)

10.  Ardından seçin ve istediğiniz alanları eşleyin **Kaydet** akışınızı kaydetmek için.

11. Bir HTTP POST URL'si istekte oluşturulur. Bu URL'yi kopyalayın ve HTTPS uç noktası olarak kullanın.

    ![HTTP Post URL'si](./media/cloud-partner-portal-lead-management-instructions-https/image015.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Teklifinizin müşteri adayları HTTPS uç noktasına göndermesi için yapılandırın

Teklifiniz için müşteri adayı yönetim bilgilerini yapılandırırken, **HTTPS uç noktası** önceki adımda kopyaladığınız hedef sağlama ve HTTP POST URL'yi yapıştırın.  

![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/image017.png)

Müşteri adayı oluşturulduğunda, Microsoft müşteri adayları CRM sistemine yapılandırdığınız yönlendirilir akış gönderir.


## <a name="json-example"></a>JSON örneği

``` json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "definitions": {},
  "id": "http://example.com/example.json",
  "properties": {
    "ActionCode": {
      "id": "/properties/ActionCode",
      "type": "string"
    },
    "OfferTitle": {
      "id": "/properties/OfferTitle",
      "type": "string"
    },
    "LeadSource": {
      "id": "/properties/LeadSource",
      "type": "string"
    },
    "UserDetails": {
      "id": "/properties/UserDetails",
      "properties": {
        "Company": {
          "id": "/properties/UserDetails/properties/Company",
          "type": "string"
        },
        "Country": {
          "id": "/properties/UserDetails/properties/Country",
          "type": "string"
        },
        "Email": {
          "id": "/properties/UserDetails/properties/Email",
          "type": "string"
        },
        "FirstName": {
          "id": "/properties/UserDetails/properties/FirstName",
          "type": "string"
        },
        "LastName": {
          "id": "/properties/UserDetails/properties/LastName",
          "type": "string"
        },
        "Phone": {
          "id": "/properties/UserDetails/properties/Phone",
          "type": "string"
        },
        "Title": {
          "id": "/properties/UserDetails/properties/Title",
          "type": "string"
        }
      },
      "type": "object"
    }
  },
  "type": "object"
}
```
