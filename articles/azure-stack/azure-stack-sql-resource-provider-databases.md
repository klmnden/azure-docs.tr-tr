---
title: Veritabanlarını SQL bağdaştırıcısı RP Azure yığında tarafından sağlanan kullanma | Microsoft Docs
description: Oluşturma ve SQL bağdaştırıcısı kaynak sağlayıcısı kullanılarak sağlanan SQL veritabanlarını yönetme
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: b9f92b4d85e17bc848d82be413df1d0dad7c8548
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294947"
---
# <a name="create-sql-databases"></a>SQL veritabanı oluşturma
Veritabanlarının Self Servis Kullanıcı Portalı üzerinden sağlanır. Bir Azure yığın kullanıcı SQL veritabanı hizmetinin içeren bir teklif sahip bir abonelik gerekiyor.

1. Oturum [Azure yığın](azure-stack-poc.md) kullanıcı portalı (hizmet yöneticileri aynı zamanda de Yönetim Portalı'nı kullanın).

2. Tıklatın **+ yeni** &gt; **veri + depolama "** &gt; **SQL Server veritabanı** &gt; **eklemek**.

3. Dahil olmak üzere veritabanı ayrıntılarla formu doldurun bir **veritabanı adı**, **en büyük boyutu**ve diğer parametreler gerektiği gibi değiştirin. Veritabanınız için bir SKU çekme istenir. Barındırma sunucuları eklendikçe bir SKU atanmış oldukları. Veritabanları, SKU yapmak sunucuları bulundurma bu havuzda oluşturulur.

  ![Yeni veritabanı](./media/azure-stack-sql-rp-deploy/newsqldb.png)

  >[!NOTE]
  > Veritabanı boyutu en az 64 MB olmalıdır. Ayarları kullanarak artırılabilir.

4. Oturum açma ayarlarını doldurun: **veritabanı oturum açmayı**, ve **parola**. Bu ayarlar yalnızca bu veritabanına erişim için oluşturulan SQL kimlik doğrulaması kimlik bilgisi ' dir. Oturum açma kullanıcı adı genel olarak benzersiz olmalıdır. Yeni bir oturum açma ayarı oluşturabilir veya varolan bir tanesini seçin. Aynı SKU kullanarak diğer veritabanları için oturum açma ayarlarını yeniden kullanabilirsiniz.

    ![Yeni bir veritabanı oturum açmayı oluşturma](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Form gönderme ve dağıtım tamamlanmasını bekleyin.

    Sonuçta elde edilen dikey penceresinde, "Bağlantı dizesi" alanı dikkat edin. SQL Server erişmesi (örneğin, bir web uygulaması) herhangi bir uygulamanın bu dizeyi Azure yığınında kullanabilirsiniz.

    ![Bağlantı dizesi alma](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-alwayson-databases"></a>SQL AlwaysOn veritabanlarını Sil
SQL AlwaysOn veritabanı kaynak Sağlayıcısı'ndan silindiğinde, bu birincil ve AlwaysOn Kullanılabilirlik grubu başarıyla silindi ancak veritabanı geri yükleme durumunda her yinelemede tasarım, SQL tarafından AG yerleştirir ve veritabanı tetiklenen sürece bırakma değil. Bir veritabanı bırakılmaz, ikincil çoğaltmaları gider durumu değil eşitleniyor. Yeni bir veritabanı ile aynı RP aracılığıyla AG yeniden ekleme hala çalışmaktadır.

## <a name="verify-sql-alwayson-databases"></a>SQL AlwaysOn veritabanlarını doğrulayın
AlwaysOn veritabanlarını göstermek eşitlenmiş olarak ve tüm örneklerini ve kullanılabilirlik grubundaki kullanılabilir. Yük devretme işleminden sonra veritabanı sorunsuzca bağlanmanız gerekir. Bir veritabanı eşitliyor doğrulamak için SQL Server Management Studio'yu kullanabilirsiniz:

![AlwaysOn doğrulayın](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)


## <a name="next-steps"></a>Sonraki adımlar

[SQL Server Kaynak sağlayıcısı koru](azure-stack-sql-resource-provider-maintain.md)
