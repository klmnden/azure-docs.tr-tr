---
title: Kiracı uygulamalarını görüntüleme - Azure Active Directory | Microsoft Docs
description: Azure Active Directory (Azure AD) kiracınızdaki uygulamaları görüntülemek için Azure portalı kullanın.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/25/2018
ms.author: barbkess
ms.reviewer: arvinh
ms.custom: it-pro
ms.openlocfilehash: bedd83426ecb24681fcfa292a049b8d4a3271d6a
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39266553"
---
# <a name="view-your-azure-active-directory-tenant-applications"></a>Azure Active Directory kiracınızdaki uygulamaları görüntüleme

Bu hızlı başlangıçta Azure Active Directory (Azure AD) kiracınızdaki uygulamaları görüntülemek için Azure portal kullanılmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Sonuçları görmek için Azure AD kiracınızda en az bir uygulamanız olması gerekir. Uygulama eklemek için [Uygulama ekleme](add-application-portal.md) hızlı başlangıcına bakın.

[Azure portalda](https://portal.azure.com) Azure AD kiracınızın genel yönetici, bulut uygulaması yöneticisi veya uygulama yöneticisi hesabıyla oturum açın.

## <a name="find-the-list-of-tenant-applications"></a>Kiracı uygulamaları listesini bulma

Azure AD kiracınızdaki uygulamaları Azure portalın **Kurumsal uygulamalar** bölümünden görüntüleyebilirsiniz.

Kiracı uygulamalarınızı bulmak için:

1. **[Azure portalda](https://portal.azure.com)** sol taraftaki gezinti panelinden **Azure Active Directory**’ye tıklayın. 

2. Azure Active Directory dikey penceresinde **Kurumsal uygulamalar**’a tıklayın. 

3. **Uygulama Türü** açılan menüsünden **Tüm Uygulamalar**’ı seçin ve **Uygula**’ya tıklayın. Kiracınızdaki uygulamalardan rastgele seçilenler burada görünür.

    ![Kurumsal uygulamalar](media/view-applications-portal/open-enterprise-apps.png)
   
4. Daha fazla uygulama görüntülemek için listenin en altındaki **Daha fazla göster**'e tıklayın. Kiracınızdaki uygulama sayısına bağlı olarak listeyi kaydırmak yerine [belirli bir uygulamayı aramak](#search-for-a-tenant-application) daha kolay olabilir.

## <a name="select-viewing-options"></a>Görüntüleme seçeneklerini belirleme

Bu bölümde aradığınız uygulamaya uygun seçenekleri belirleyin.

1. Uygulamaları **Uygulama Türü**, **Uygulama Durumu** ve **Uygulama Görünürlüğü** seçeneklerine göre görüntüleyebilirsiniz. 

    ![Arama seçenekleri](media/view-applications-portal/search-options.png)

2. **Uygulama Türü** bölümünde aşağıdaki seçeneklerden birini belirleyin:

    - **Kurumsal Uygulamalar** seçeneği Microsoft harici uygulamaları gösterir.
    - **Microsoft Uygulamaları** seçeneği Microsoft uygulamalarını gösterir.
    - **Tüm Uygulamalar** seçeneği hem Microsoft harici uygulamaları hem de Microsoft uygulamalarını gösterir.

3. **Uygulama Durumu** bölümünde **Tümü**, **Devre dışı** veya **Etkin** seçeneğini belirleyin. **Tümü** seçeneği hem devre dışı hem de etkin uygulamaları gösterir.

4. **Uygulama Görünürlüğü** bölümünde **Tümü** veya **Gizli** seçeneğini belirleyin. **Gizli** seçeneği kiracıda bulunan ancak kullanıcılara görünür olmayan uygulamaları gösterir.

5. İstediğiniz seçenekleri belirledikten sonra **Uygula**'ya tıklayın.
 

## <a name="search-for-a-tenant-application"></a>Belirli bir kiracı uygulamasını arama

Belirli bir uygulamayı aramak için:

1. **Uygulama Türü** menüsünden **Tüm uygulamalar**'ı seçin ve **Uygula**'ya tıklayın.

2. Bulmak istediğiniz uygulamanın adını girin. Uygulama Azure AD kiracınıza eklenmişse arama sonuçlarında görünür. Bu örnek, GitHub uygulamasının kiracı uygulamalarına eklenmediğini göstermektedir.

    ![Uygulama arama](media/view-applications-portal/search-for-tenant-application.png)

3. Uygulama adının ilk birkaç harfini girmeyi deneyin.  Bu örnek **Sales** ile başlayan tüm uygulamaları gösterir.

    ![Ön ek ile arama](media/view-applications-portal/search-by-prefix.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure AD kiracınızdaki uygulamaları görüntülemeyi ve uygulama listesini uygulama türüne, durumuna ve görünürlüğüne göre filtrelemeyi öğrendiniz. Ayrıca belirli bir uygulamayı aramayı da öğrendiniz.

Aradığınız uygulamayı bulduğunuza göre [Kiracınıza daha fazla uygulama ekle](add-application-portal.md) bölümüne geçebilir veya uygulamaya tıklayarak özelliklerini ve yapılandırma seçeneklerini görüntüleyebilir veya düzenleyebilirsiniz. Örneğin çoklu oturum açmayı yapılandırabilirsiniz. 

> [!div class="nextstepaction"]
> [Çoklu oturum açmayı yapılandırma](configure-single-sign-on-portal.md)


