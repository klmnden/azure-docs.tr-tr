---
title: Azure yığında SQL bağdaştırıcısı kaynak sağlayıcısı tarafından sağlanan veritabanlarını kullanma | Microsoft Docs
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
ms.date: 06/18/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 56d21b76268f94f4254985a6924c4ca2d778a9cd
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300831"
---
# <a name="create-sql-databases"></a>SQL veritabanı oluşturma

Oluşturun ve veritabanlarının Self Servis Kullanıcı Portalı'nda yönetin. Bir Azure yığın kullanıcı bir SQL veritabanı hizmetinin içeren bir teklif aboneliğiyle gerekir.

1. Oturum [Azure yığın](azure-stack-poc.md) Kullanıcı Portalı.

2. Seçin **+ yeni** &gt; **veri + depolama** &gt; **SQL Server veritabanı** &gt; **eklemek**.

3. Altında **Create Database**, gerekli bilgileri girin **veritabanı adı** ve **MB cinsinden en büyük boyutu**.

   >[!NOTE]
   >Veritabanı boyutu en az 64 artıran MB olmalıdır veritabanı dağıttıktan sonra.

   Ortamınız için gerektiği gibi diğer ayarları yapılandırın.

4. Altında **Create Database**seçin **SKU**. Altında **bir SKU seçin**, veritabanınız için SKU seçin.

   ![Veritabanı Oluştur](./media/azure-stack-sql-rp-deploy/newsqldb.png)

   >[!NOTE]
   >Barındırma sunucuları Azure yığınına eklendikçe bir SKU atanmış oldukları. Veritabanlarını barındıran bir SKU sunucu havuzundaki oluşturulur.

5. Seçin **oturum açma**.
6. Altında **bir oturum açma seçin**, var olan bir oturum seçin veya seçin **+ yeni bir oturum açma oluşturmak**.
7. Altında **yeni oturum açma**, için bir ad girin **veritabanı oturum açmayı** ve **parola**.

   >[!NOTE]
   >Bu ayarlar yalnızca bu veritabanına erişim için oluşturulan SQL kimlik doğrulaması kimlik bilgisi ' dir. Oturum açma kullanıcı adı genel olarak benzersiz olmalıdır. Aynı SKU kullanan diğer veritabanları için oturum açma ayarlarını yeniden kullanabilirsiniz.

   ![Yeni bir veritabanı oturum açmayı oluşturma](./media/azure-stack-sql-rp-deploy/create-new-login.png)

8. Seçin **Tamam** veritabanı dağıtmayı tamamlamak için.

Altında **Essentials**, veritabanı dağıtıldıktan sonra gösterilen, not edin **bağlantı dizesi**. Bu dize SQL Server veritabanına erişmek için gereken herhangi bir uygulamada kullanabilirsiniz.

![Bağlantı dizesi alma](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="sql-always-on-databases"></a>SQL Always On veritabanları

Tasarım gereği, her zaman açık veritabanları farklı değerinden bir tek başına sunucu ortamında işlenir. Daha fazla bilgi için bkz: [Tanıtımı SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).

### <a name="verify-sql-always-on-databases"></a>SQL Always On veritabanları doğrulayın

Aşağıdaki ekran görüntüsünde SQL Always On veritabanı durumunu bakmak için SQL Server Management Studio nasıl kullanabileceğinizi gösterir.

![AlwaysOn veritabanı durumu](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

Her zaman açık veritabanlarının Synchronızed olarak ve kullanılabilir tüm SQL örneklerinde Göster ve kullanılabilirlik grupları görünür gerekir. Önceki ekran görüntüsünde newdb1 veritabanı örnektir ve durumunun **newdb1 (eşitlendi)**.

### <a name="delete-an-alwayson-database"></a>AlwaysOn veritabanını silin

Kaynak Sağlayıcısı'ndan bir SQL AlwaysOn veritabanı sildiğinizde, SQL veritabanı birincil çoğaltmadan ve kullanılabilirlik grubundan siler.

SQL veritabanı geri yükleme durumuna diğer çoğaltmaları koyar ve veritabanı tetiklenen sürece bırakma değil. Veritabanı bırakılmakta değil, ikincil çoğaltmaları Not Synchronızıng bir duruma gidin.

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server Kaynak sağlayıcısı koru](azure-stack-sql-resource-provider-maintain.md)
