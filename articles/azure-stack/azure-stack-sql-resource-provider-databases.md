---
title: Azure Stack'te SQL bağdaştırıcısı kaynak sağlayıcısı tarafından sağlanan veritabanları kullanılarak | Microsoft Docs
description: Nasıl SQL bağdaştırıcısı kaynak sağlayıcısı kullanılarak sağlanan SQL veritabanlarını oluşturma ve yönetme
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
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.lastreviewed: 10/16/2018
ms.openlocfilehash: bbb4f72e029a442362b792f3cdb4731e9563687b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244836"
---
# <a name="create-sql-databases"></a>SQL veritabanı oluşturma

Oluşturun ve Kullanıcı Portalı'nda, Self Servis veritabanlarını yönetme. Azure Stack kullanıcı SQL veritabanı hizmeti içeren bir teklif olan bir abonelik gerekir.

1. Oturum [Azure Stack](azure-stack-poc.md) Kullanıcı Portalı.

2. Seçin **+ yeni** &gt; **veri + depolama** &gt; **SQL Server veritabanı** &gt; **ekleme**.

3. Altında **Create Database**, gerekli bilgileri girin **veritabanı adı** ve **MB cinsinden en büyük boyutu**.

   >[!NOTE]
   >Veritabanı boyutu en az 64 hangi artırabilir miyim MB olmalıdır veritabanını dağıttıktan sonra.

   Ortamınız için gerektiği gibi diğer ayarları yapılandırın.

4. Altında **Create Database**seçin **SKU**. Altında **bir SKU seçin**, veritabanınız için SKU'ları seçin.

   ![Veritabanı Oluştur](./media/azure-stack-sql-rp-deploy/newsqldb.png)

   >[!NOTE]
   >Azure Stack için barındırma sunucuları eklendikçe, bir SKU atanmış oldukları. Bir SKU sunucuları bulundurma havuzdaki veritabanları oluşturulur.

5. Seçin **oturum açma**.
6. Altında **bir oturum açma seçin**, var olan bir oturum seçin ya da seçin **+ yeni bir oturum açma Oluştur**.
7. Altında **yeni oturum açma**, için bir ad girin **veritabanı oturum açma** ve **parola**.

   >[!NOTE]
   >Bu ayarlar, erişim yalnızca bu veritabanı için oluşturulan SQL kimlik doğrulaması kimlik bilgisi yöneliktir. Oturum açma kullanıcı adı genel olarak benzersiz olmalıdır. Oturum açma ayarları aynı SKU kullanan diğer veritabanları için yeniden kullanabilirsiniz.

   ![Yeni bir veritabanı oturumu oluştur](./media/azure-stack-sql-rp-deploy/create-new-login.png)

8. Seçin **Tamam** veritabanı dağıtmayı tamamlamak için.

Altında **Essentials**veritabanı dağıtıldıktan sonra gösterilen, Not **bağlantı dizesi**. Bu dize SQL Server veritabanına erişmek için gereken herhangi bir uygulamada kullanabilirsiniz.

![Bağlantı dizesi alma](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="sql-always-on-databases"></a>SQL Always On veritabanları

Tasarım gereği, veritabanları, Always On farklı değerinden bir tek başına sunucu ortamında işlenir. Daha fazla bilgi için [Karşınızda SQL Server Always On kullanılabilirlik grupları'Azure sanal makinelerinde](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview).

### <a name="verify-sql-always-on-databases"></a>SQL Always On veritabanları doğrulayın

Aşağıdaki ekran görüntüsü yakalamayı SQL Always On veritabanı durumunu görmek için SQL Server Management Studio nasıl kullanabileceğinizi gösterir.

![AlwaysOn veritabanı durumu](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)

Always On veritabanları ve kullanılabilir tüm SQL örneklerinde Synchronızed olarak göster ve kullanılabilirlik gruplarının gerekir. Önceki ekran görüntüsünde newdb1 veritabanı örnektir ve durumunun **newdb1 (eşzamanlı)**.

### <a name="delete-an-alwayson-database"></a>AlwaysOn veritabanını silme

Kaynak Sağlayıcısı'ndan bir SQL AlwaysOn veritabanı sildiğinizde, SQL veritabanı birincil çoğaltmadan ve kullanılabilirlik grubundan siler.

SQL veritabanı geri yükleme durumuna başka çoğaltmalarda koyar ve veritabanını tetiklenen sürece bırakma değil. Veritabanı kaldırılmadıysa, ikincil çoğaltmalar bir Not Synchronızıng durumuna gidin.

## <a name="next-steps"></a>Sonraki adımlar

[SQL Server Kaynak sağlayıcısı koru](azure-stack-sql-resource-provider-maintain.md)
