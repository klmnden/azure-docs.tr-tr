---
title: include dosyası
description: include dosyası
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 06/28/2019
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: 324f41055cf333081f308a3ff533ff7df6b33038
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67479260"
---
>[!NOTE]
>Yönergeler için bu bölümde [yeni Azure AD uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-register-app). Hala eski yerel uygulama kayıt varsa, bu desteklenir mi sürece kullanabilir. Ayrıca, herhangi bir nedenden dolayı uygulama işlese yeni yolu kurulumunuzda çalışmıyorsa yerel eski bir AAD uygulaması oluşturmak deneyebilirsiniz. Okuma [ile Azure Active Directory eski Azure dijital İkizlerini uygulamanızı kaydetmeniz](../articles/digital-twins/how-to-use-legacy-aad.md) daha fazla yönerge için. 

1. İçinde [Azure portalında](https://portal.azure.com)açın **Azure Active Directory** sol bölmeden ve ardından açık **uygulama kayıtları** bölmesi. Seçin **yeni kayıt** düğmesi.

    ![Kayıtlı uygulama](./media/digital-twins-permissions/aad-app-register.png)

1. Bu uygulama kaydında için kolay bir ad verin **adı** kutusu. Altında **yeniden yönlendirme URI'si (isteğe bağlı)** bölümünde, seçin **genel istemci (Mobil ve Masaüstü)** sol taraftaki açılan girin `https://microsoft.com` metin kutusuna sağ. **Kaydol**’u seçin.

    ![Bölmesinde oluşturma](./media/digital-twins-permissions/aad-app-reg-create.png)

1. Emin olmak için [uygulama olarak kaydedilmiş bir *yerel uygulama*](https://docs.microsoft.com/azure/active-directory/develop/scenario-desktop-app-registration)açın **kimlik doğrulaması** bölmesi için uygulama kaydı ve bu bölmedeki aşağı kaydırın. İçinde **varsayılan istemci türü** bölümünde, seçin **Evet** için **uygulama genel bir istemci kabul**. 

    ![Varsayılan yerel](./media/digital-twins-permissions/aad-app-default-native.png)

1.  Açık **genel bakış** kayıtlı uygulamanızın bölmesi ve aşağıdaki varlık değerleri geçici bir dosyaya kopyalayın. Aşağıdaki bölümlerde örnek Uygulamanızı yapılandırmak için bu değerleri kullanacaksınız.

    - **Uygulama (istemci) kimliği**
    - **(Kiracı) dizin kimliği**

    ![Azure Active Directory Uygulama Kimliği](./media/digital-twins-permissions/aad-app-reg-app-id.png)

1. Açık **API izinleri** uygulama kaydınızı bölmesi. Seçin **bir izin eklemek** düğmesi. İçinde **istek API izinleri** bölmesinde **Kuruluşum kullandığı API'leri** sekmesine ve ardından arama **Azure akıllı alanları**. Seçin **Azure akıllı hizmet alanları** API.

    ![Arama API’si](./media/digital-twins-permissions/aad-app-search-api.png)

1. Seçili API olarak gösterilir **Azure dijital İkizlerini** aynı **istek API izinleri** bölmesi. Seçin **okuma (1)** açılan menü ve ardından **Read.Write** onay kutusu. Seçin **izinleri eklemek** düğmesi.

    ![API izinleri ekleme](./media/digital-twins-permissions/aad-app-req-permissions.png)

1. Kuruluşunuzun ayarlara bağlı olarak, yönetici bu API'ye erişim için ek adımlar gerekebilir. Daha fazla bilgi için yöneticinize başvurun. Yönetici erişimi onaylandıktan sonra **yönetici onayı gerekli** sütununda **API izinleri** bölmesinde Apı'leriniz için aşağıdakine benzer gösterir:

    ![API izinleri ekleme](./media/digital-twins-permissions/aad-app-admin-consent.png)

