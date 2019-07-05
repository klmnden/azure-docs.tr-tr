---
title: include dosyası
description: include dosyası
services: digital-twins
author: alinamstanciu
ms.service: digital-twins
ms.topic: include
ms.date: 06/26/2019
ms.author: alinast
ms.custom: include file
ms.openlocfilehash: 9a5b3b04287a8b732d01bd8fe4610e073332da0d
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478972"
---
1. İçinde [Azure portalında](https://portal.azure.com)açın **Azure Active Directory** sol bölmeden ve ardından açık **özellikleri** bölmesi. **Dizin Kimliğini** geçici bir dosyaya kopyalayın. Sonraki bölümde örnek bir uygulama yapılandırmak için bu değeri kullanacaksınız.

    ![Azure Active Directory dizin kimliği](./media/digital-twins-permissions-legacy/aad-app-reg-tenant.png)

1. İçinde [Azure portalında](https://portal.azure.com)açın **Azure Active Directory** sol bölmeden ve ardından açık **uygulama kayıtları (eski)** bölmesi. Seçin **yeni uygulama kaydı** düğmesi.

1. Bu uygulama kaydında için kolay bir ad verin **adı** kutusu. Seçin **uygulama türü** olarak **yerel**, ve **yeniden yönlendirme URI'si** olarak `https://microsoft.com`. **Oluştur**’u seçin.

    ![Bölmesinde oluşturma](./media/digital-twins-permissions-legacy/aad-app-reg-create.png)

1.  Kayıtlı uygulama açın ve değerini kopyalayın **uygulama kimliği** geçici bir dosya alanı. Bu değer, Azure Active Directory uygulamanızı tanımlar. Uygulama kimliği, örnek uygulamanızı aşağıdaki bölümlerde yapılandırmak için kullanırsınız.

    ![Azure Active Directory Uygulama Kimliği](./media/digital-twins-permissions-legacy/aad-app-reg-app-id.png)

1. Uygulamanızı uygulama kayıt bölmesini açın. Seçin **ayarları** > **gerekli izinler**ve ardından:

   a. Seçin **Ekle** açmak için üst sol taraftaki **API erişimi Ekle** bölmesi.

   b. Seçin **bir API seçin** araması **Azure dijital İkizlerini**. Arama sonucunda API görüntülenmezse **Azure Smart Spaces** araması yapın.

   c. Seçin **Azure dijital İkizlerini (Azure akıllı alanları Service)** seçenek ve **seçin**.

   d. Seçin **izinleri seçin**. Seçin **okuma/yazma erişimi** temsilci izinleri kutusunu işaretleyin ve **seçin**.

   e. Seçin **Bitti** içinde **API erişimi Ekle** bölmesi.

   f. İçinde **gerekli izinler** bölmesinde **izinleri verin** düğmesine ve görüntülenen bildirim kabul edin. Bu API için izin verilmezse, yöneticinize başvurun.

      ![Gerekli izinler bölmesinde](./media/digital-twins-permissions-legacy/aad-app-req-permissions.png)

 