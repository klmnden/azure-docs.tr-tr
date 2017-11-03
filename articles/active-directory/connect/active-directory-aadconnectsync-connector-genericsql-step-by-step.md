---
title: "Genel SQL bağlayıcı adım adım | Microsoft Docs"
description: "Bu makalede bir basit ik sistemi genel SQL Bağlayıcısı'nı kullanarak adım adım taramasını değil."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 28c1cc60-24fd-4d0d-a36d-b4aba6de86e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 3fdc1b405b95180d031aa4ad45b406f7fc149d8f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="generic-sql-connector-step-by-step"></a>Adım adım Genel SQL Bağlayıcısı
Bu konu hakkında adım adım bir kılavuzdur. Bir Basit örnek HR veritabanı oluşturur ve bazı kullanıcı ve grup üyeliklerini içeri aktarmak için kullanın.

## <a name="prepare-the-sample-database"></a>Örnek veritabanını hazırlama
İçinde SQL komut dosyasını çalıştırın, SQL Server çalıştıran bir sunucuda bulunan [ek A](#appendix-a). Bu komut dosyası GSQLDEMO adı ile örnek bir veritabanı oluşturur. Oluşturulan veritabanı için nesne modeli Bu resim gibi görünür:  
![Nesne modeli](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/objectmodel.png)

Ayrıca veritabanına bağlanmak için kullanmak istediğiniz bir kullanıcı oluşturun. Bu kılavuzda, kullanıcı FABRIKAM\SQLUser çağrılır ve etki alanında bulunan.

## <a name="create-the-odbc-connection-file"></a>ODBC bağlantı dosyası oluşturma
Genel SQL bağlayıcı uzak sunucuya bağlanmak için ODBC kullanma. İlk olarak kimliğinizi ODBC bağlantı bilgilerini bir dosya oluşturmak gerekiyor.

1. ODBC Yönetimi yardımcı programı, sunucunuzda başlatın:  
   ![ODBC](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc.png)
2. Sekmeyi seçin **dosya DSN**. Tıklatın **Ekle...** .  
   ![ODBC1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc1.png)
3. Out-of-box sürücü works ince, bu nedenle seçin ve **sonraki >**.  
   ![ODBC2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc2.png)
4. Dosyaya gibi bir ad verin **GenericSQL**.  
   ![ODBC3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc3.png)
5. **Son**'a tıklayın.  
   ![ODBC4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc4.png)
6. Bağlantıyı yapılandırmak için süre. Veri kaynağı iyi bir açıklama girin ve SQL Server çalıştıran sunucunun adını belirtin.  
   ![ODBC5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc5.png)
7. SQL ile kimlik doğrulaması yapmayı seçin. Bu durumda, Windows kimlik doğrulaması kullanın.  
   ![ODBC6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc6.png)
8. Örnek veritabanı adını sağlayın **GSQLDEMO**.  
   ![ODBC7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc7.png)
9. Bu ekranda her şeyi varsayılan devam eder. **Son**'a tıklayın.  
   ![ODBC8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc8.png)
10. Her şeyi çalıştığını beklendiği gibi doğrulamak için tıklatın **Test veri kaynağı**.  
    ![ODBC9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc9.png)
11. Test başarılı olduğundan emin olun.  
    ![ODBC10](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc10.png)
12. ODBC yapılandırma dosyası artık dosya DSN görünür olması gerekir.  
    ![ODBC11](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/odbc11.png)

Dosyanın gerekir ve bağlayıcı oluşturmaya başlamak şimdi sahibiz.

## <a name="create-the-generic-sql-connector"></a>Genel SQL bağlayıcısı oluşturun
1. Eşitleme Hizmeti Yöneticisi Arabiriminde seçin **Bağlayıcılar** ve **oluşturma**. Seçin **Genel SQL (Microsoft)** ve açıklayıcı bir ad verin.  
   ![Connector1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector1.png)
2. Önceki bölümde oluşturduğunuz DSN dosyasını bulun ve sunucuya yükleyin. Veritabanına bağlanmak için kimlik bilgilerini sağlayın.  
   ![Connector2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector2.png)
3. Bu kılavuzda, biz bize kolaylaşır ve iki nesne türlerini olduğunu söylemek **kullanıcı** ve **grup**.
   ![Connector3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector3.png)
4. Öznitelikleri bulmak amacıyla tablosuna kendisini bakarak özniteliklerle algılamak için bağlayıcı istiyoruz. Bu yana **kullanıcılar** ayrılmış bir sözcük içinde köşeli ayraçlar [] bunu sağlamak ihtiyacımız SQL'de.  
   ![Connector4](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector4.png)
5. Bağlayıcı öznitelik ve DN özniteliği tanımlamak için süre. İçin **kullanıcılar**, iki öznitelik kullanıcı adı ve EmployeeID birleşimi kullanırız. İçin **grup**, GroupName kullanıyoruz (değil gerçekçi gerçekçi, ancak bu kılavuz çalışır).
   ![Connector5](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector5.png)
6. Tüm öznitelik türlerini, bir SQL veritabanında algılanabilir. Başvuru özniteliği türü özellikle yapılamıyor. Grup nesnesi türü için biz OwnerId ve Memberıd başvurmak için değiştirmeniz gerekir.  
   ![Connector6](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector6.png)
7. Başvuru özniteliği önceki adımda nesne türü gerektiği seçtik bu değerleri başvuru öznitelikleridir. Örneğimizde, kullanıcı nesnesi yazın.  
   ![Connector7](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector7.png)
8. Genel Parametreler sayfasında seçin **Filigran** delta stratejisi olarak. Ayrıca tarih/saat biçiminde yazın **yyyy-aa-gg ss: dd:**.
   ![Connector8](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector8.png)
9. Üzerinde **yapılandırma bölümleri ve hiyerarşileri** sayfasında, her iki nesne türlerini seçin.
   ![Connector9](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/connector9.png)
10. Üzerinde **nesne türlerini Seç** ve **öznitelikleri Seç**, nesne türleri ve tüm öznitelikleri seçin. Üzerinde **yapılandırma bağlayıcılarını** sayfasında, **son**.

## <a name="create-run-profiles"></a>Çalıştırma profillerini oluşturma
1. Eşitleme Hizmeti Yöneticisi Arabiriminde seçin **Bağlayıcılar**, ve **çalıştırma profillerini Yapılandır**. Tıklatın **yeni bir profil**. Biz başlayın **tam içeri aktarma**.  
   ![Runprofile1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile1.png)
2. Seçin **tam içeri aktarma (yalnızca aşama)**.  
   ![Runprofile2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile2.png)
3. Bölümü seçin **nesne = kullanıcı**.  
   ![Runprofile3](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile3.png)
4. Seçin **tablo** ve türü **[kullanıcılar]**. Birden çok değerli nesne türü bölümüne gidin ve aşağıdaki resimde olduğu gibi veri girin. Seçin **son** adım kaydetmek için.  
   ![Runprofile4a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4a.png)  
   ![Runprofile4b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile4b.png)  
5. Seçin **yeni adım**. Bu süre, select **nesne grubu =**. Son sayfasında, aşağıdaki resimde olduğu gibi yapılandırmasını kullanın. **Son**'a tıklayın.  
   ![Runprofile5a](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5a.png)  
   ![Runprofile5b](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/runprofile5b.png)  
6. İsteğe bağlı: istiyorsanız, ek çalıştırma profillerini yapılandırabilirsiniz. Bu kılavuzda, yalnızca tam içeri aktarma kullanılır.
7. Tıklatın **Tamam** çalıştırma profillerini değiştirme işlemini tamamlamak için.

## <a name="add-some-test-data-and-test-the-import"></a>Bazı test verilerini ve içeri aktarma test ekleyin
Örnek veritabanınızdaki bazı test verilerini doldurun. Hazır olduğunuzda seçin **çalıştırmak** ve **tam alma**.

İşte, iki telefon numarası olan bir kullanıcı ve Grup bazı üyeleri ile.  
![cs1](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs1.png)  
![CS2](./media/active-directory-aadconnectsync-connector-genericsql-step-by-step/cs2.png)  

## <a name="appendix-a"></a>Ek A
**Örnek veritabanı oluşturmak için SQL komut dosyası**

```SQL
---Creating the Database---------
Create Database GSQLDEMO
Go
-------Using the Database-----------
Use [GSQLDEMO]
Go
-------------------------------------
USE [GSQLDEMO]
GO
/****** Object:  Table [dbo].[GroupMembers]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GroupMembers](
    [MemberID] [int] NOT NULL,
    [Group_ID] [int] NOT NULL
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[GROUPS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[GROUPS](
    [GroupID] [int] NOT NULL,
    [GROUPNAME] [nvarchar](200) NOT NULL,
    [DESCRIPTION] [nvarchar](200) NULL,
    [WATERMARK] [datetime] NULL,
    [OwnerID] [int] NULL,
PRIMARY KEY CLUSTERED
(
    [GroupID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Table [dbo].[USERPHONE]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[USERPHONE](
    [USER_ID] [int] NULL,
    [Phone] [varchar](20) NULL
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [dbo].[USERS]   ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[USERS](
    [USERID] [int] NOT NULL,
    [USERNAME] [nvarchar](200) NOT NULL,
    [FirstName] [nvarchar](100) NULL,
    [LastName] [nvarchar](100) NULL,
    [DisplayName] [nvarchar](100) NULL,
    [ACCOUNTDISABLED] [bit] NULL,
    [EMPLOYEEID] [int] NOT NULL,
    [WATERMARK] [datetime] NULL,
PRIMARY KEY CLUSTERED
(
    [USERID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_GROUPS] FOREIGN KEY([Group_ID])
REFERENCES [dbo].[GROUPS] ([GroupID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_GROUPS]
GO
ALTER TABLE [dbo].[GroupMembers]  WITH CHECK ADD  CONSTRAINT [FK_GroupMembers_USERS] FOREIGN KEY([MemberID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GroupMembers] CHECK CONSTRAINT [FK_GroupMembers_USERS]
GO
ALTER TABLE [dbo].[GROUPS]  WITH CHECK ADD  CONSTRAINT [FK_GROUPS_USERS] FOREIGN KEY([OwnerID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[GROUPS] CHECK CONSTRAINT [FK_GROUPS_USERS]
GO
ALTER TABLE [dbo].[USERPHONE]  WITH CHECK ADD  CONSTRAINT [FK_USERPHONE_USER] FOREIGN KEY([USER_ID])
REFERENCES [dbo].[USERS] ([USERID])
GO
ALTER TABLE [dbo].[USERPHONE] CHECK CONSTRAINT [FK_USERPHONE_USER]
GO
```
