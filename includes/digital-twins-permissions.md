---
title: include dosyası
description: include dosyası
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 09/19/2018
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: 1887efd741f4779a5186707d60b27ca66fc3c06f
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51284008"
---
1. [Azure portalda](https://portal.azure.com) sol gezinti bölmesinden **Azure Active Directory**'yi seçin ve **Özellikler** bölmesini açın. **Dizin Kimliğini** geçici bir dosyaya kopyalayın. Bu değeri bir sonraki bölümde örnek uygulamayı yapılandırırken kullanacaksınız.

    ![Azure Active Directory dizin kimliği](./media/digital-twins-permissions/aad-app-reg-tenant.png)

1. **Uygulama kayıtları** bölmesini açıp **Yeni uygulama kaydı** düğmesine tıklayın.
    
    ![Azure Active Directory yeni uygulama kaydı](./media/digital-twins-permissions/aad-app-reg-start.png)

1. **Ad** alanına bu uygulama kaydı için bir kolay ad girin. **Uygulama türü** için **_Yerel_**, **Yönlendirme URI'si** için de **_https://microsoft.com_** değerini belirleyin. **Oluştur**’a tıklayın.

    ![Azure Active Directory uygulama kaydı oluşturma](./media/digital-twins-permissions/aad-app-reg-create.png)

1. Kaydettiğiniz uygulamayı açın ve **Uygulama Kimliği** değerini geçici bir dosyaya kopyalayın. Bu değer, Azure Active Directory uygulamanızı tanımlar. Uygulama Kimliği değerini sonraki bölümlerde örnek uygulamayı yapılandırırken kullanacaksınız.

    ![Azure Active Directory uygulama kimliği](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Uygulama kaydı bölmesini açın ve **Ayarlar** > **Gerekli izinler**'e tıklayın:
    - Sol üstteki **Ekle**'ye tıklayarak **API erişimi ekle** bölmesini açın.
    - **Bir API seçin**'e tıklayın ve **Azure Digital Twins** araması yapın. Arama sonucunda API görüntülenmezse **Azure Smart Spaces** araması yapın.
    - **Azure Digital Twins (Azure Smart Spaces Service)** seçeneğini belirleyin ve **Seç**'e tıklayın.
    - **İzinleri seçin**'e tıklayın. **Okuma/Yazma Erişimi** temsilci izinleri kutusunu işaretleyin ve **Seç**'e tıklayın.
    - **API erişimi ekle** bölmesinde **Bitti**'ye tıklayın.
    - **Gerekli izinler** bölmesinde **İzin ver** düğmesine tıklayın ve açılan bilgilendirmeyi kabul edin.

       ![Azure Active Directory uygulama kaydı API ekleme](./media/digital-twins-permissions/aad-app-req-permissions.png)
