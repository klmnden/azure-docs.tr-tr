---
title: SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme | Microsoft Docs
description: Bu konuda, bir güncelleştirme yüklemesi için yalnızca SQL dbo izinleri olan bir hesap kullanarak sağlayan Azure AD Connect açıklanmaktadır.
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: jparsons
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
origin.date: 02/26/2018
ms.date: 04/09/2019
ms.subservice: hybrid
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6269d00c9a6a8f827a4e31044d9d20efb0f8471b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60243533"
---
# <a name="install-azure-ad-connect-using-sql-delegated-administrator-permissions"></a>SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme
En son Azure AD Connect derleme önce SQL, gerekli yapılandırmaları dağıtırken yönetim temsilcisi seçme desteklenmiyor.  Azure AD Connect'i yüklemek isteyen kullanıcılar, SQL Server'da Sistem Yöneticisi (SA) izinlerine sahip gerekmiyor.

Azure AD Connect'in en son sürümüyle bant dışı SQL Yöneticisi tarafından gerçekleştirilen ve ardından veritabanı sahibi haklarıyla Azure AD Connect Yöneticisi tarafından yüklenen veritabanı sağlama, artık.

## <a name="before-you-begin"></a>Başlamadan önce
Bu özelliği kullanmak için birden fazla hareketli parça vardır ve her biri farklı bir yönetici, kuruluşunuzdaki gerektirebilir gerekir.  Bireysel rolleri ve Azure AD Connect ile bu özelliği dağıtmak, ilgili görevlerini, aşağıdaki tabloda özetlenmiştir.

|Rol|Açıklama|
|-----|-----|
|Etki alanı veya orman AD Yöneticisi|Eşitleme hizmeti çalıştırmak için Azure AD Connect tarafından kullanılan etki alanı düzeyinde hizmet hesabı oluşturur.  Hizmet hesapları hakkında daha fazla bilgi için bkz. [hesapları ve izinleri](reference-connect-accounts-permissions.md).
|SQL yönetici|Ad eşitleme veritabanını oluşturur ve oturum açma + dbo verir erişim için Azure AD Connect yönetici ve etki alanına/ormana yönetici tarafından oluşturulan hizmet hesabı|
Azure AD Connect Yöneticisi|Azure AD Connect yükler ve özel bir yükleme sırasında hizmet hesabı belirtir.

## <a name="steps-for-installing-azure-ad-connect-using-sql-delegated-permissions"></a>SQL kullanarak Azure AD Connect'i yüklemek için adımları temsilci izinleri
Bant dışı veritabanını sağlamak ve veritabanı sahibi izinleriyle Azure AD Connect'i yüklemek için aşağıdaki adımları kullanın.

>[!NOTE]
>Gerekli olmamasına rağmen olduğu **kesinlikle önerilir** latin1_general_cı_as harmanlama veritabanı oluşturulurken seçilir.


1. SQL ile bir büyük/küçük harfe duyarlı olmayan harmanlama sırası ad eşitleme veritabanını oluşturmak yöneticisinin **(latin1_general_cı_as)**.  Veritabanı adlandırılmalıdır **ADSync**.  Azure AD Connect yüklendikten sonra kurtarma modeli, uyumluluk düzeyi ve içerik türü için doğru değerleri güncelleştirilir.  Aksi takdirde harmanlama sırası SQL Yöneticisi tarafından doğru şekilde ayarlanması gerekir ancak Azure AD Connect yüklemesini engeller.  SA, kurtarılır silmeniz ve veritabanı oluşturmanız gerekir.
 
   ![Harmanlama](./media/how-to-connect-install-sql-delegation/sql4.png)
2. Azure AD Connect Yöneticisi ve etki alanı hizmet hesabı aşağıdaki izinleri verin:
   - SQL Oturum Açma 
   - **Veritabanı Owner(dbo)** hakları.
 
   ![İzinler](./media/how-to-connect-install-sql-delegation/sql3a.png)

   >[!NOTE]
   >Azure AD Connect ile iç içe geçmiş üyelikler oturumları desteklemez.  Başka bir deyişle, Azure AD Connect yönetici hesabı ve etki alanı hizmet hesabı dbo hakları bahşedilir bir oturum açma bağlanmalıdır.  Yalnızca dbo haklarına sahip bir oturum açma için atanmış bir gruba üye olamaz.

3. Azure AD Connect yükleme sırasında kullanılacak SQL sunucusunu ve örnek adı gösteren Azure AD Connect Yöneticisi için bir e-posta gönderin.

## <a name="additional-information"></a>Ek bilgiler
Veritabanı oluşturulduktan sonra Azure AD Connect Yöneticisi yükleyebilir ve şirket içi eşitleme sırasında kolaylık yapılandırın.

SQL yönetici ADSync veritabanı önceki bir Azure AD Connect yedeklemeden geri olması durumunda, yeni Azure AD Connect sunucusu, var olan bir veritabanını kullanarak yüklemek gerekir. Mevcut bir veritabanı ile Azure AD Connect yükleme hakkında daha fazla bilgi için bkz. [var olan bir ad eşitleme veritabanını kullanarak Azure AD Connect'i yükleme](how-to-connect-install-existing-database.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](how-to-connect-install-express.md)
- [Azure AD Connect özel yüklemesi](how-to-connect-install-custom.md)
- [Azure AD Connect'i mevcut bir AD Eşitleme veritabanını kullanarak yükleme](how-to-connect-install-existing-database.md)  

<!-- Update_Description: wording update -->