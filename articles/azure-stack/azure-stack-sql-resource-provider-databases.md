---
title: "Veritabanlarını SQL bağdaştırıcısı RP Azure yığında tarafından sağlanan kullanma | Microsoft Docs"
description: "Oluşturma ve SQL bağdaştırıcısı kaynak sağlayıcısı kullanılarak sağlanan SQL veritabanlarını yönetme"
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 0cc08c37e879b00f8cd9a4046a4c81c55dab167c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-sql-databases"></a>SQL veritabanı oluşturma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Veritabanlarının Self Servis Kullanıcı Portalı deneyimi sağlanır. Bir kullanıcı veritabanı hizmeti içeren bir teklif sahip bir abonelik gerekiyor.

1. Oturum [Azure yığın](azure-stack-poc.md) kullanıcı portalı (hizmet yöneticileri aynı zamanda de Yönetim Portalı'nı kullanın).

2. Tıklatın **+ yeni** &gt; **veri + depolama "** &gt; **SQL Server veritabanı (Önizleme)** &gt; **Ekle**.

3. Dahil olmak üzere veritabanı ayrıntılarla formu doldurun bir **veritabanı adı**, **en büyük boyutu**ve diğer parametreler gerektiği gibi değiştirin. Veritabanınız için bir SKU çekme istenir. Barındırma sunucuları eklendikçe bir SKU atanmış oldukları. Veritabanları, SKU yapmak sunucuları bulundurma bu havuzda oluşturulur.

  ![Yeni veritabanı](./media/azure-stack-sql-rp-deploy/newsqldb.png)

  >[!NOTE]
  > Veritabanı boyutu en az 64 MB olmalıdır. Ayarları kullanarak artırılabilir.

4. Oturum açma ayarlarını doldurun: **veritabanı oturum açmayı**, ve **parola**. Bu ayarlar yalnızca bu veritabanına erişim için oluşturulan SQL kimlik doğrulaması kimlik bilgisi ' dir. Oturum açma kullanıcı adı genel olarak benzersiz olmalıdır. Yeni bir oturum açma ayarı oluşturabilir veya varolan bir tanesini seçin. Aynı SKU kullanarak diğer veritabanları için oturum açma ayarlarını yeniden kullanabilirsiniz.

    ![Yeni bir veritabanı oturum açmayı oluşturma](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Form gönderme ve dağıtım tamamlanmasını bekleyin.

    Sonuçta elde edilen dikey penceresinde, "Bağlantı dizesi" alanı dikkat edin. SQL Server erişmesi (örneğin, bir web uygulaması) herhangi bir uygulamanın bu dizeyi Azure yığınında kullanabilirsiniz.

    ![Bağlantı dizesi alma](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-databases"></a>SQL veritabanlarını Sil
Portalı'ndan

>[!NOTE]
>
>SQL AlwaysOn veritabanı RP silindiğinde, birincil ve AlwaysOn Kullanılabilirlik grubu başarıyla silindi ancak Tasarım SQL AG tarafından veritabanı geri yükleme durumunda her yinelemede yerleştirir ve veritabanı tetiklenen sürece bırakma değil. Bir veritabanı bırakılmaz, ikincil çoğaltmaları gider durumu değil eşitleniyor. Yeni bir veritabanı ile aynı RP aracılığıyla AG yeniden ekleme hala çalışmaktadır. Bkz: ![ikincil bir veritabanı kaldırılıyor](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server)

## <a name="manage-database-credentials"></a>Veritabanı kimlik bilgileri yönetme
Veritabanı kimlik bilgileri (oturum açma ayarları) güncelleştirebilirsiniz.

## <a name="verify-sql-alwayson-databases"></a>SQL AlwaysOn veritabanlarını doğrulayın
AlwaysOn veritabanlarını göstermek eşitlenmiş olarak ve tüm örneklerini ve kullanılabilirlik grubundaki kullanılabilir. Yük devretme işleminden sonra veritabanı sorunsuzca bağlanmanız gerekir. Bir veritabanı eşitliyor doğrulamak için SQL Server Management Studio'yu kullanabilirsiniz:

![AlwaysOn doğrulayın](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)
