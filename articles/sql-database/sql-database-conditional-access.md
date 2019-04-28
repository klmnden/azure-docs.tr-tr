---
title: Koşullu erişim - Azure SQL veritabanı ve veri ambarı | Microsoft belgeleri
description: Azure SQL veritabanı ve veri ambarı için koşullu erişimi yapılandırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sql-data-warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
manager: craigg
ms.date: 03/29/2019
ms.openlocfilehash: 79d15a46affb2a6b7159ba080d4235073c59919c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61413709"
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Azure SQL veritabanı ve veri ambarı ile koşullu erişim (MFA)  

Azure [SQL veritabanı](sql-database-technical-overview.md), [yönetilen örneği](sql-database-managed-instance.md), ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Microsoft koşullu erişimi destekler. 

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

Aşağıdaki adımlarda, SQL veritabanı, bir koşullu erişim ilkesini zorlamak için yapılandırma gösterilmektedir.  

## <a name="prerequisites"></a>Önkoşullar  
- SQL veritabanınız veya SQL veri ambarı Azure Active Directory kimlik doğrulamasını destekleyecek şekilde yapılandırmanız gerekir. Belirli adımlar için bkz: [yapılandırma ve SQL veritabanı veya SQL veri ambarı ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md).  
- Çok faktörlü kimlik doğrulaması etkinleştirildiğinde, en son SSMS gibi desteklenen aracı ile bağlanmalıdır. Daha fazla bilgi için [SQL Server Management Studio için Azure SQL veritabanını Yapılandır multi-Factor authentication](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Azure SQL DB/DW için CA'yı yapılandırma  
1. Portalında, select oturum **Azure Active Directory**ve ardından **koşullu erişim**. Daha fazla bilgi için [Azure Active Directory koşullu erişim teknik başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
   ![koşullu erişim dikey penceresi](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2. İçinde **koşullu erişim ilkeleri** dikey penceresinde tıklayın **yeni ilke**bir ad belirtin ve ardından **yapılandırma kuralları**.  
3. Altında **atamaları**seçin **kullanıcılar ve gruplar**, kontrol **kullanıcıları ve grupları seçin**ve ardından kullanıcı veya grup için koşullu erişim'i seçin. Tıklayın **seçin**ve ardından **Bitti** seçiminizi kabul etmek için.  
   ![Kullanıcıları ve grupları seçin](./media/sql-database-conditional-access/select-users-and-groups.png)  

4. Seçin **bulut uygulamaları**, tıklayın **uygulamaları Seç**. Tüm uygulamalar için koşullu erişim kullanılabilir görürsünüz. Seçin **Azure SQL veritabanı**, alt tıklatın **seçin**ve ardından **Bitti**.  
   ![SQL veritabanı seçin](./media/sql-database-conditional-access/select-sql-database.png)  
   Aradığınız bulamıyorsanız **Azure SQL veritabanı** üçüncü aşağıdaki ekran görüntüsünde listelenen, aşağıdaki adımları tamamlayın:   
   - Azure SQL DB/DW örneğinizin SSMS ile bir AAD yönetici hesabı kullanarak oturum açın.  
   - Yürütme `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
   - AAD için oturum açın ve Azure SQL veritabanı ve veri ambarı, AAD uygulamalarda listelendiğini doğrulayın.  

5. Seçin **erişim denetimleri**seçin **Grant**ve ardından uygulamak istediğiniz ilke olup olmadığını denetleyin. Bu örnekte, seçiyoruz **çok faktörlü kimlik doğrulaması gerektiren**.  
   ![erişim verme seçin](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Özet  
Azure SQL DB/Azure AD Premium kullanarak DW'ye bağlanmasına izin verme (Azure SQL veritabanı) seçili uygulama artık seçili koşullu erişim ilkesi zorunlu kılar **gerekli çok faktörlü kimlik doğrulaması.**  
Çok faktörlü kimlik doğrulaması ile ilgili Azure SQL veritabanı ve veri ambarı hakkında sorular için başvurun MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Sonraki adımlar  

Bir öğretici için bkz. [Azure SQL veritabanınızın güvenliğini sağlama](sql-database-security-tutorial.md).
