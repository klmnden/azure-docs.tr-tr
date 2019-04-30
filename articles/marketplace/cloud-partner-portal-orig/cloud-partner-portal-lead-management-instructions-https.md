---
title: HTTPS uç noktası | Microsoft Docs
description: Bir HTTPS uç noktası için sağlama yönetimi yapılandırın.
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
ms.date: 12/24/2018
ms.author: pbutlerm
ms.openlocfilehash: cfcd154b2f44c9e8acf12a9666abc9ce95fb3c26
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62124075"
---
# <a name="configure-lead-management-using-an-https-endpoint"></a>Bir HTTPS uç noktası kullanarak müşteri adayı yönetimini yapılandırma

AppSource müşteri adayları ve Azure Marketi'nde işlemek için bir HTTPS uç noktası'nı kullanabilirsiniz. Bu müşteri adayları yazılabilir, bir müşteri ilişkileri yönetimi (CRM) sistemine yazılması veya gönderilen bir e-posta bildirimi. Bu makalede sağlama Yönetimi'ni kullanarak yapılandırmak nasıl [Microsoft Flow](https://powerapps.microsoft.com/automate-processes/) Otomasyon hizmeti.

## <a name="create-a-flow-using-microsoft-flow"></a>Microsoft Flow kullanarak akış oluşturma

1. Açık [akış](https://flow.microsoft.com/) Web sayfası. Seçin **oturum** veya **ücretsiz olarak kaydolun** ücretsiz bir Flow hesabı oluşturmak için.

2. Oturum açın ve seçin **Akışlarım** menü çubuğundaki.

    ![Akışlarım](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows.png)

3. Seçin **+ boş akış Oluştur**.

    ![Boş akış oluştur](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank.png)

4. Seçin **boş akış Oluştur**.

    ![Boş akış oluştur](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-create-fromblank2.png)

5. İçinde **bağlayıcı ve tetikleyicide arama** alanında, "request" istek Bağlayıcısı'nı bulmak için yazın.
6. Altında **Tetikleyicileri**seçin **olduğunda bir HTTP isteği alındığında**. 

    ![HTTP isteği alındı tetikleyicisini seçin](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-pick-request-trigger.png)

7. Yapılandırmak için aşağıdaki adımlardan birini kullanın **istek gövdesi JSON şeması**:

   - Kopyalama [JSON şeması](#json-schema) bu makalenin sonunda **istek gövdesi JSON şeması** metin kutusu.
   - **Şema oluşturmak için örnek yük kullanma** öğesini seçin. İçinde **girin veya yapıştırın örnek JSON yükü** metin kutusu, yapıştırma seçeneğiyle [JSON örneği](#json-example). Seçin **Bitti** şema oluşturun.

   >[!Note]
   >Bu noktada akışa bir CRM sistemine bağlanmak veya bir e-posta bildirimini yapılandırın.

### <a name="to-connect-to-a-crm-system"></a>Bir CRM sistemine bağlanmak için

1. Seçin **+ yeni adım**.
2. CRM sistemine yeni bir kayıt oluşturmak için istediğiniz eylemi seçin. Aşağıdaki ekran yakalama programları **Dynamics 365 - yeni kayıt oluştur** örnek olarak.

    ![Yeni kayıt oluşturma](./media/cloud-partner-portal-lead-management-instructions-https/https-image009.png)

3. Sağlamak **kuruluş adı** Bağlayıcınız için bağlantı girişleri olmasıdır. Seçin **müşteri adayları** gelen **varlık adı** açılır liste.

    ![Müşteri adaylarını seçin](./media/cloud-partner-portal-lead-management-instructions-https/https-image011.png)

4. Akışı, müşteri adayı bilgilerini sağlamak için bir form gösterilmektedir. Dinamik İçerik Ekle seçerek giriş isteği öğelerinden eşleyebilirsiniz. Aşağıdaki ekran yakalama programları **OfferTitle** örnek olarak.

    ![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/https-image013.png)

5. Ardından seçin ve istediğiniz alanları eşleyin **Kaydet** akışınızı kaydetmek için.

6. Bir HTTP POST URL'si istekte oluşturulur. Bu URL'yi kopyalayın ve HTTPS uç noktası olarak kullanın.

    ![HTTP Post URL'si](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

### <a name="to-set-up-email-notification"></a>E-posta bildirimi ayarlamak için

1. Seçin **+ yeni adım**.
2. Altında **eylem seçin**seçin **eylemleri**.
3. **Eylemler** altında **E-posta gönder**’i seçin.

    ![Bir e-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-add-email-action.png)

4. İçinde **bir e-posta**, aşağıdaki gerekli alanları yapılandırın:

   - **İçin** -en az bir geçerli e-posta adresi girin.
   - **Konu** -Flow size gibi dinamik içerik ekleme seçeneğiniz **LeadSource** aşağıdaki ekran görüntüsünde.

     ![Dinamik içerik kullanarak bir e-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-dynamic-content.png)

   - **Gövde** - dinamik içerik listesinden, e-postanın gövdesinde istediğiniz bilgileri ekleyin. Örneğin, LastName, FirstName, e-posta, ve şirket.

   E-posta bildirimini kurma işlemini tamamladığınızda, aşağıdaki ekran görüntüsünde örnekteki gibi görünecektir.

   ![Bir e-posta eylemi ekleme](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-configure-email-action.png)

5. Seçin **Kaydet** akışınız tamamlanması.
6. Bir HTTP POST URL'si istekte oluşturulur. Bu URL'yi kopyalayın ve HTTPS uç noktası olarak kullanın.

    ![HTTP Post URL'si](./media/cloud-partner-portal-lead-management-instructions-https/https-myflows-get-post-url.png)

## <a name="configure-your-offer-to-send-leads-to-the-https-endpoint"></a>Teklifinizin müşteri adayları HTTPS uç noktasına göndermesi için yapılandırın

Teklifiniz için müşteri adayı yönetim bilgilerini yapılandırırken, **HTTPS uç noktası** için **hedef yol** ve HTTP POST önceki adımda kopyaladığınız URL'yi yapıştırın.  

![Dinamik içerik ekle](./media/cloud-partner-portal-lead-management-instructions-https/https-image017.png)

Müşteri adayı oluşturulduğunda, Microsoft müşteri adayları, yapılandırdığınız CRM Sistem ya da e-posta adresine yönlendirilir akış gönderir.

## <a name="json-schema-and-example"></a>JSON şema ve örnek

JSON test örneği aşağıdaki şemayı kullanır:

### <a name="json-schema"></a>JSON şeması

``` json
{
  "$schema": "https://json-schema.org/draft-04/schema#",
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

Kopyalayın ve düzenleyin, MS Flow testinde kullanmak için aşağıdaki JSON örneği.

### <a name="json-example"></a>JSON örneği

```json
{
"OfferTitle": "Test Microsoft",
"LeadSource": "Test run through MS Flow",
"UserDetails": {
"Company": "Contoso",
"Country": "USA",
"Email": "someone@contoso.com",
"FirstName": "Some",
"LastName": "One",
"Phone": "16175555555",
"Title": "Esquire"
}
}
```

## <a name="next-steps"></a>Sonraki adımlar

Zaten yapmadıysanız, müşteri yapılandırma [müşteri adayları](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-get-customer-leads) bulut iş ortağı Portalı'nda.
