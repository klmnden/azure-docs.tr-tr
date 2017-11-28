---
title: "Koşullu erişim - Azure SQL veritabanı ve veri ambarı | Microsoft belge"
description: "Azure SQL veritabanı ve veri ambarı için koşullu erişim yapılandırma hakkında bilgi edinin."
services: sql-database
author: BYHAM
manager: jhubbard
ms.custom: security
ms.service: sql-database
ms.topic: article
ms.date: 06/07/2017
ms.author: rickbyh
ms.workload: Inactive
ms.openlocfilehash: 16bd403fecbd24d7cea774bba8535f102501861f
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2017
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Azure SQL veritabanı ve veri ambarı ile koşullu erişim (MFA)  

SQL Database ve SQL Data Warehouse Microsoft koşullu erişimi destekler. Aşağıdaki adımlar SQL veritabanına bir koşullu erişim politikasını yapılandırmak nasıl gösterir.  

## <a name="prerequisites"></a>Ön koşullar  
- SQL Database veya SQL Data Warehouse Azure Active Directory kimlik doğrulamayı destekleyecek şekilde yapılandırmanız gerekir. Belirli adımlar için bkz: [yapılandırma ve SQL Database veya SQL Data Warehouse ile Azure Active Directory kimlik doğrulamasını yönetmek](sql-database-aad-authentication-configure.md).  
- Çok faktörlü kimlik doğrulaması etkinleştirildiğinde, en son SSMS gibi desteklenen aracı ile bağlanmalıdır. Daha fazla bilgi için bkz: [SQL Server Management Studio Azure SQL veritabanını yapılandırma çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>Azure SQL DB/DW için CA'yı yapılandırma  
1.  Portalı, select oturum açma **Azure Active Directory**ve ardından **koşullu erişim**. Daha fazla bilgi için bkz: [Azure Active Directory koşullu erişim teknik başvuru](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
  ![koşullu erişim dikey penceresi](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2.  İçinde **koşullu erişim ilkeleri** dikey penceresinde tıklatın **yeni ilke**, bir ad sağlayın ve ardından **kurallar**.  
3.  Altında **atamaları**seçin **kullanıcılar ve gruplar**, denetleme **kullanıcıları ve grupları seçin**ve ardından kullanıcıyı veya grubu koşullu erişim için seçin. Tıklatın **seçin**ve ardından **Bitti** seçiminizi kabul etmek için.  
  ![Kullanıcıları ve grupları seçin](./media/sql-database-conditional-access/select-users-and-groups.png)  

4.  Seçin **bulut uygulamaları**, tıklatın **uygulamaları**. Tüm uygulamalar için koşullu erişim kullanılabilir bakın. Seçin **Azure SQL veritabanı**, alt tıklatın **seçin**ve ardından **Bitti**.  
  ![SQL veritabanı seçin](./media/sql-database-conditional-access/select-sql-database.png)  
  Bilgisayarınızda bulamazsanız **Azure SQL veritabanı** aşağıdaki üçüncü ekran içinde listelenen, aşağıdaki adımları tamamlayın:   
  - Azure SQL DB/DW örneğinizi SSMS ile bir AAD yönetici hesabı kullanarak oturum açın.  
  - Yürütme `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
  - AAD'ye oturum açabilir ve Azure SQL veritabanı ve veri ambarı, AAD uygulamalarda listelendiğini doğrulayın.  

5.  Seçin **erişim denetimleri**seçin **Grant**ve ardından uygulamak istediğiniz ilke denetleyin. Bu örnekte, biz seçin **çok faktörlü kimlik doğrulaması gerektiren**.  
  ![erişim izni ver seçin](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Özet  
Azure SQL DB/Azure AD Premium kullanarak DW için bağlanmasına izin vererek seçilen uygulama (Azure SQL veritabanı) artık seçili koşullu erişim ilkesi zorunlu kılar **gerekli çok faktörlü kimlik doğrulaması.**  
Çok faktörlü kimlik doğrulaması ile ilgili Azure SQL veritabanı ve veri ambarı hakkında sorular için başvurun MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Sonraki adımlar  

Bir öğretici için bkz: [Azure SQL veritabanınıza güvenli](sql-database-security-tutorial.md).
