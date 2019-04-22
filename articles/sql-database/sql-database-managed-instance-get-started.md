---
title: 'Azure portalı: SQL yönetilen örnek oluşturma | Microsoft Docs'
description: SQL yönetilen örneği, ağ ortamı ve erişim için istemci VM oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 04/10/2019
ms.openlocfilehash: d94e00c8a475e29ddd671004b8137ba4e6efd107
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59495046"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Hızlı Başlangıç: Bir Azure SQL veritabanı yönetilen örneği oluşturma

Bu hızlı başlangıçta, Azure SQL veritabanı oluşturma hakkında bilgi vermektedir [yönetilen örnek](sql-database-managed-instance.md) Azure portalında.

> [!IMPORTANT]
> Kısıtlamalar için bkz: [desteklenen bölgeler](sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen abonelik türleri](sql-database-managed-instance-resource-limits.md#supported-subscription-types).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-managed-instance"></a>Yönetilen örnek oluşturma

Aşağıdaki adımlar yönetilen örnek oluşturma işlemini göstermektedir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
2. Bulun **yönetilen örnek** seçip **Azure SQL yönetilen örneği**.
3. **Oluştur**’u seçin.

   ![Yönetilen örnek oluşturma](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Doldurun **SQL yönetilen örneği** formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz | İçinde yeni kaynaklar oluşturma izniniz olan bir abonelik |
   |**Yönetilen örnek adı**|Geçerli bir ad|Geçerli adlar için bkz. [adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Ayrılmış bir sunucu düzeyindeki rolüdür gibi "serveradmin" kullanmayın.|
   |**Parola**|Geçerli bir parola|Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Saat dilimi**|Yönetilen örneğiniz tarafından gözlenecek saat dilimi|Daha fazla bilgi için [saat dilimleri](sql-database-managed-instance-timezone.md)|
   |**Harmanlama**|Yönetilen Örneğiniz için kullanmak istediğiniz harmanlama|SQL Server veritabanlarını geçirme varsa, kaynak bir harmanlama kullanarak denetleyin `SELECT SERVERPROPERTY(N'Collation')` ve bu değeri kullanın. Harmanlamalar hakkında daha fazla bilgi için bkz. [sunucu düzeyinde harmanlamaları](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|
   |**Konum**|Yönetilen örnek oluşturmak istediğiniz konumu|Bölgeler hakkında daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|Şunlardan birini seçin **yeni sanal ağ oluştur** veya geçerli sanal ağ ve alt ağ.| Ağ/alt ağ kullanılamıyorsa, zorunluluktur olması [ağ gereksinimlerini karşılamak için değişiklik](sql-database-managed-instance-configure-vnet-subnet.md) önce yeni yönetilen örnek için hedef olarak seçin. Yönetilen örnek için ağ ortamını yapılandırma gereksinimleriyle ilgili daha fazla bilgi için bkz: [yönetilen örnek için bir VNet yapılandırma](sql-database-managed-instance-connectivity-architecture.md). |
   |**Bağlantı türü**|Proxy ve yeniden yönlendirme arasında bağlantı türünü seçin|Bağlantı türleri hakkında daha fazla bilgi için bkz. [Azure SQL bağlantı İlkesi](sql-database-connectivity-architecture.md#connection-policy).|
   |**Kaynak grubu**|Yeni veya mevcut bir kaynak grubu|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|

   ![yönetilen örnek formu](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Yönetilen örnek bir örnek yük devretme grubu ikincil kullanmak için kullanıma al'ı seçin ve DnsAzurePartner yönetilen örnek belirtin. Bu özellik Önizleme aşamasındadır ve eşlik eden ekran görüntüsünde gösterilen değil.
6. Seçin **fiyatlandırma katmanı** işlem ve depolama kaynaklarını boyutlandırmaya ek olarak için fiyatlandırma katmanı seçeneklerini gözden geçirin. 32 GB bellek ve 16 sanal çekirdekli Genel Amaçlı fiyatlandırma katmanı varsayılan değerdir.
7. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın.
8. İşlem tamamlandığında seçin **Uygula** seçiminizi kaydetmek için.  
9. Seçin **Oluştur** yönetilen örnek dağıtmak için.
10. Seçin **bildirimleri** dağıtım durumu görüntülenecek simge.

    ![yönetilen örnek dağıtım ilerleme durumu](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Seçin **dağıtım sürüyor** daha da dağıtımın ilerleme durumunu izlemek üzere yönetilen örnek penceresini açmak için.

> [!IMPORTANT]
> Bir alt ağdaki ilk örnek için dağıtım sonraki durumda genellikle daha uzun zamandır. Dağıtım işlemi beklediğinizden daha uzun sürdüğü için iptal etme. İkinci bir yönetilen örnek alt ağında oluşturma, yalnızca birkaç dakika sürer.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Kaynakları gözden geçirin ve tam sunucu adınızı alma

Dağıtım başarıyla tamamlandıktan sonra oluşturulan kaynakları gözden geçirin ve sonraki hızlı başlangıçlarda kullanmak üzere tam sunucu adını alın.

1. Yönetilen Örneğiniz için kaynak grubunu açın ve içinde sizin için oluşturulan kaynaklarını görüntüleme [yönetilen örnek oluşturma](#create-a-managed-instance) hızlı başlangıç.

   ![Yönetilen örnek kaynakları](./media/sql-database-managed-instance-get-started/resources.png)

2. Kullanıcı tanımlı yol UDR gözden geçirmek için rota tablosu seçin) sizin için oluşturulan tablo.

   ![Rota tablosu](./media/sql-database-managed-instance-get-started/route-table.png)

3. Rota tablosunda gelen ve yönetilen örnek sanal ağ içinde trafiği yönlendirmek girişlerini gözden geçirin. Oluşturma veya yol tablonuz el ile yapılandırma, bu girişleri rota tablosunda oluşturduğunuzdan emin olması gerekir.

   ![Yerel mı alt ağ için giriş](./media/sql-database-managed-instance-get-started/udr.png)

4. Bir kaynak grubuna geri dönün ve güvenlik kurallarını gözden geçirmek için ağ güvenlik grubunu seçin.

   ![Ağ güvenlik grubu](./media/sql-database-managed-instance-get-started/network-security-group.png)

5. Gelen ve giden güvenlik kuralları gözden geçirin.

   ![Güvenlik kuralları](./media/sql-database-managed-instance-get-started/security-rules.png)

6. Bir kaynak grubuna geri dönün ve yönetilen örneğinizi seçin.

   ![Yönetilen örnek](./media/sql-database-managed-instance-get-started/managed-instance.png)

7. Üzerinde **genel bakış** sekmesinde, bulmak **konak** tam ana bilgisayar adresi sonraki hızlı kullanmak için yönetilen örnek için özellik ve kopyalayın.

   ![Ana bilgisayar adı](./media/sql-database-managed-instance-get-started/host-name.png)

   Ad benzer **your_machine_name.a1b2c3d4e5f6.database.windows.net**.

## <a name="next-steps"></a>Sonraki adımlar

- Bir yönetilen örneğe bağlanma hakkında bilgi edinmek için bkz:
  - Bağlantı seçenekleri uygulamalar için genel bakış için bkz: [uygulamalarınızı yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md).
  - Azure sanal makinesinden bir yönetilen örneğe bağlanma gösteren Hızlı Başlangıç için bkz: [bir Azure sanal makine bağlantısı yapılandırma](sql-database-managed-instance-configure-vm.md).
  - Noktadan siteye bağlantısı kullanarak bir şirket içi istemci bilgisayardan bir yönetilen örneğe bağlanma gösteren Hızlı Başlangıç için bkz: [noktadan siteye bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md).
- Mevcut bir SQL Server veritabanını şirket içinden Yönetilen bir örneğe geri yüklerken, veritabanı yedekleme dosyasından geri yükleme işlemini [Geçiş için Azure Veritabanı Geçiş Hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) veya [T-SQL RESTORE komutu](sql-database-managed-instance-get-started-restore.md) ile yapabilirsiniz.
- Gelişmiş sorun giderme yerleşik zekaya sahip yönetilen örnek veritabanı performansını izleme için bkz: [kullanarak Azure SQL Analytics İzleyici Azure SQL veritabanı](../azure-monitor/insights/azure-sql.md)
