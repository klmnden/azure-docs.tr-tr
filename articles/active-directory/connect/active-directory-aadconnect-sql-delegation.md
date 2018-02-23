---
title: "Temsilci SQL yönetici izinleri kullanarak Azure AD Connect'i yükleme | Microsoft Docs"
description: "Bu konuda, bir güncelleştirme için yalnızca SQL dbo izinleri olan bir hesabı kullanarak yüklemeye olanak sağlar. Azure AD Connect açıklanmaktadır."
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.reviewer: jparsons
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: billmath
ms.openlocfilehash: c2d77c37f2f65c9a7db1fd5c4010fc43bcbc7ebf
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>Temsilci SQL yönetici izinleri kullanarak Azure AD Connect'i yükleme
En son Azure AD Connect yapı önce SQL, gerekli yapılandırmaları dağıtırken yönetim temsilcisi desteklenmiyordu.  Azure AD Connect'i yüklemek isteyen kullanıcılara SQL server üzerinde Sistem Yöneticisi (SA) izinlerine sahip gerekirdi.

Azure AD Connect'in en son sürümle birlikte, bant dışı SQL Yöneticisi tarafından gerçekleştirilen ve veritabanı sahibi hakları olan Azure AD Connect Yöneticisi tarafından yüklenen veritabanı can şimdi sağlama.

## <a name="before-you-begin"></a>Başlamadan önce
Bu özelliği kullanmak için birkaç taşıma bölümü vardır ve her biri farklı bir yönetici, kuruluşunuzda gerektirebilir hayata geçirmek gerekir.  Bireysel rolleri ve Azure AD Connect bu özellik ile dağıtmanın kendi ilgili görevleri aşağıdaki tabloda özetlenmiştir.

|Rol|Açıklama|
|-----|-----|
|Etki alanı veya orman AD Yöneticisi|Eşitleme hizmeti çalıştırmak için Azure AD Connect tarafından kullanılan etki alanı düzeyi hizmet hesabı oluşturur.  Hizmet hesapları hakkında daha fazla bilgi için bkz: [hesapları ve izinleri](active-directory-aadconnect-accounts-permissions.md).
|SQL yönetici|ADSync veritabanı oluşturur ve oturum açma + dbo verir Azure AD Connect yönetici ve etki alanında/ormanda yönetici tarafından oluşturulan hizmet hesabı için erişim|
Azure AD Connect Yöneticisi|Azure AD Connect yükler ve özel yükleme sırasında hizmet hesabı belirtir.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>SQL kullanarak Azure AD Connect'i yüklemek için adımları izinlere temsilci
Bant dışı veritabanını sağlamak ve veritabanı sahibinin izinleriyle Azure AD Connect'i yüklemek için aşağıdaki adımları kullanın.

>[!NOTE]
>Gerekli olmamasına karşın, olan **tavsiye** latin1_general_cı_as harmanlama veritabanı oluşturulurken seçilir.


1.  ADSync veritabanı ile büyük küçük harfe duyarlı harmanlama sırası oluşturmak SQL yöneticisine sahip **(latin1_general_cı_as)**.  Veritabanı olarak adlandırılmalıdır **ADSync**.  Azure AD Connect yüklendiğinde kurtarma modeli, uyumluluk düzeyi ve kapsama türü doğru değerlerin güncelleştirilir.  Aksi takdirde harmanlama sırası SQL yönetici tarafından doğru şekilde ayarlanması gerekir ancak Azure AD Connect yükleme engeller.  SA kurtarmak için gereken silin ve veritabanını yeniden oluşturun.</br>
![Harmanlama](media/active-directory-aadconnect-sql-delegation/sql1.png)
2.  Azure AD Connect yönetici ve etki alanı hizmet hesabı aşağıdaki izinleri verin:
    - SQL Oturum Açma 
    - **Owner(dbo) veritabanı** hakları.  </br>
![İzinler](media/active-directory-aadconnect-sql-delegation/sql3.png)
3.  Azure AD Connect yüklenirken kullanılması SQL server ve örnek adı gösteren Azure AD Connect yöneticisine bir e-posta gönderin.

## <a name="additional-information"></a>Ek bilgiler
Veritabanı sağlandıktan sonra Azure AD Connect yönetici yükleyin ve şirket içi eşitlemesi sırasında kolaylık yapılandırın.  

Her senaryo ilgili Azure AD Connect yeni yüklemeler destekleyen ek olarak, bu özellik ayrıca temsilci sağlar **/UseExistingDatabase** bayrağı.  Var olan bir veritabanı Azure AD Connect'i yükleme hakkında daha fazla bilgi için bkz: [var olan bir ADSync veritabanı kullanarak Azure AD Connect'i yükleme](active-directory-aadconnect-existing-database.md)


## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](active-directory-aadconnect-get-started-express.md)
- [Azure AD Connect özel yüklemesi](active-directory-aadconnect-get-started-custom.md)
- [Var olan bir ADSync veritabanı kullanarak Azure AD Connect'i yükleme](active-directory-aadconnect-existing-database.md)  