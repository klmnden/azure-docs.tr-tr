---
title: 'Azure portalı: SQL veritabanı yönetilen örneği oluşturma | Microsoft Docs'
description: SQL veritabanı yönetilen örneği, ağ ortamı ve erişim için istemci VM oluşturun.
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
ms.date: 04/26/2019
ms.openlocfilehash: f4f9ecec3876fa84abf420a2ef9b147132e7fe2a
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925175"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Hızlı Başlangıç: Bir Azure SQL veritabanı yönetilen örneği oluşturma

Bu hızlı başlangıçta, Azure SQL veritabanı oluşturma hakkında bilgi vermektedir [yönetilen örnek](sql-database-managed-instance.md) Azure portalında.

> [!IMPORTANT]
> Kısıtlamalar için bkz: [desteklenen bölgeler](sql-database-managed-instance-resource-limits.md#supported-regions) ve [desteklenen abonelik türleri](sql-database-managed-instance-resource-limits.md#supported-subscription-types).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın. 

## <a name="create-a-managed-instance"></a>Yönetilen örnek oluşturma

Aşağıdaki adımlar yönetilen örnek oluşturma işlemini göstermektedir.

1. Seçin **kaynak Oluştur** Azure portal'ın sol üst köşedeki.
2. Bulun **yönetilen örnek**ve ardından **Azure SQL yönetilen örneği**.
3. **Oluştur**’u seçin.

   ![Yönetilen örnek oluşturma](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Doldurun **SQL yönetilen örneği** formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle.

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz yok. | Yeni kaynaklar oluşturma izni veren bir abonelik. |
   |**Yönetilen örnek adı**|Geçerli bir ad.|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir kullanıcı adı.|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Ayrılmış bir sunucu düzeyindeki rolüdür olduğundan "serveradmin" kullanmayın.|
   |**Parola**|Geçerli bir parola.|Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Saat dilimi**|Yönetilen örneğiniz tarafından gözlenecek saat dilimi.|Daha fazla bilgi için [saat dilimlerini](sql-database-managed-instance-timezone.md).|
   |**Harmanlama**|Yönetilen Örneğiniz için kullanmak istediğiniz harmanlama.|SQL Server veritabanları geçiş işlemi gerçekleştirirseniz, kaynak harmanlama kullanarak kontrol `SELECT SERVERPROPERTY(N'Collation')` ve bu değeri kullanın. Harmanlamalar hakkında daha fazla bilgi için bkz. [ayarlama veya değiştirme Sunucu harmanlaması](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|
   |**Konum**|Yönetilen örnek'ı oluşturmak istediğiniz konum.|Bölgeler hakkında daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|Şunlardan birini seçin **yeni sanal ağ oluştur** veya geçerli sanal ağ ve alt ağ.| Bir ağ veya alt ağ kullanılamıyorsa, olmalıdır [ağ gereksinimlerini karşılamak için değişiklik](sql-database-managed-instance-configure-vnet-subnet.md) önce yeni yönetilen örnek için hedef olarak seçin. Yönetilen örnek için ağ ortamını yapılandırma gereksinimleri hakkında daha fazla bilgi için bkz: [yönetilen örnek için sanal ağ yapılandırma](sql-database-managed-instance-connectivity-architecture.md). |
   |**Genel bir uç noktayı etkinleştirme**   |Genel bir uç nokta etkinleştirmek için bu seçeneği işaretleyin   |Yönetilen örnek genel veri uç noktası aracılığıyla erişilebilir olması için **etkinleştirme genel uç nokta** denetlenmesi gerekiyor.| 
   |**Konumlardan erişime izin ver**   |Seçeneklerden birini seçin: <ul> <li>**Azure Hizmetleri**</li> <li>**Internet**</li> <li>**Erişim yok**</li></ul>   |Genel bir uç nokta yapılandırma güvenlik grubuyla Portal deneyimi sağlar. </br> </br> Üzerinde kendi senaryonuza bağlı olarak, aşağıdaki seçeneklerden birini seçin: </br> <ul> <li>Azure Hizmetleri - Power BI veya diğer çok kiracılı bir hizmet bağlanırken önerilir. </li> <li> İnternet'e - yönetilen bir örneği oluşturan hızla çalıştırın istediğinizde, test amacıyla kullanın. Üretim ortamında kullanım için önerilmez. </li> <li> Erişim yok - Bu seçenek bir reddetme güvenlik kuralı oluşturur. Yönetilen örnek genel uç noktası üzerinden erişilebilir hale getirmek için bu kural değiştirmeniz gerekir. </li> </ul> </br> Ortak uç nokta güvenliği hakkında daha fazla bilgi için bkz. [kullanarak Azure SQL veritabanı yönetilen örnek genel uç noktası ile güvenli bir şekilde](sql-database-managed-instance-public-endpoint-securely.md).|
   |**Bağlantı türü**|Bir ara sunucu arasında bir yeniden yönlendirme bağlantı türü seçin.|Bağlantı türleri hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı bağlantı İlkesi](sql-database-connectivity-architecture.md#connection-policy).|
   |**Kaynak grubu**|Yeni veya mevcut bir kaynak grubu.|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|

   ![Yönetilen örnek form](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Yönetilen örnek bir örnek yük devretme grubu ikincil kullanmak için kullanıma al'ı seçin ve DnsAzurePartner yönetilen örnek belirtin. Bu özellik Önizleme aşamasındadır ve aşağıdaki ekran görüntüsünde gösterilen değil.
6. Seçin **fiyatlandırma katmanı** boyutu işlem ve depolama kaynakları ve fiyatlandırma katmanı seçeneklerini gözden geçirin. 32 GB bellek ve 16 sanal çekirdekli Genel Amaçlı fiyatlandırma katmanı varsayılan değerdir.
7. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın.
8. İşlemi tamamladığınızda, seçin **Uygula** seçiminizi kaydetmek için. 
9. Seçin **Oluştur** yönetilen örnek dağıtmak için.
10. Seçin **bildirimleri** dağıtım durumunu görüntülemek için simge.

    ![Yönetilen örnek dağıtım ilerleme durumu](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Seçin **dağıtım sürüyor** daha da dağıtımın ilerleme durumunu izlemek üzere yönetilen örnek penceresini açmak için. 

> [!IMPORTANT]
> Bir alt ağdaki ilk örnek için dağıtım sonraki durumda genellikle daha uzun zamandır. Dağıtım işlemi beklediğinizden daha uzun sürdüğü için iptal etme. İkinci bir yönetilen örnek alt ağında oluşturma, yalnızca birkaç dakika sürer.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Kaynakları gözden geçirin ve tam sunucu adınızı alma

Dağıtım başarılı olduktan sonra oluşturulan kaynakları gözden geçirin ve sonraki hızlı başlangıçlarda kullanmak için tam sunucu adını alma.

1. Yönetilen Örneğiniz için kaynak grubunu açın. Uygulamasında oluşturulan kaynaklarını görüntüleme [yönetilen örnek oluşturma](#create-a-managed-instance) hızlı başlangıç.

   ![Yönetilen örnek kaynakları](./media/sql-database-managed-instance-get-started/resources.png)

2. Sizin için oluşturulan kullanıcı tanımlı yol (UDR) tabloyu gözden geçirmek için rota tablosunu seçin.

   ![Rota tablosu](./media/sql-database-managed-instance-get-started/route-table.png)

3. Rota tablosunda gelen ve yönetilen örnek sanal ağ içinde trafiği yönlendirmek girişlerini gözden geçirin. Oluşturursanız veya yol tablonuz el ile yapılandırın, bu girişleri rota tablosunda oluşturduğunuzdan emin olması gerekir.

   ![Yerel bir yönetilen örnek alt ağı için giriş](./media/sql-database-managed-instance-get-started/udr.png)

4. Kaynak grubunu dönün ve güvenlik kurallarını gözden geçirmek için ağ güvenlik grubunu seçin.

   ![Ağ güvenlik grubu](./media/sql-database-managed-instance-get-started/network-security-group.png)

5. Gelen ve giden güvenlik kuralları gözden geçirin. 

   ![Güvenlik kuralları](./media/sql-database-managed-instance-get-started/security-rules.png)

6. Kaynak grubunu dönün ve yönetilen örneğinizi seçin.

   ![Yönetilen örnek](./media/sql-database-managed-instance-get-started/managed-instance.png)

7. Üzerinde **genel bakış** sekmesinde, bulmak **konak** özelliği. Sonraki hızlı başlangıçta kullanmak için yönetilen örnek için tam bir ana bilgisayar adresi kopyalayın.

   ![Ana bilgisayar adı](./media/sql-database-managed-instance-get-started/host-name.png)

   Ad benzer **your_machine_name.a1b2c3d4e5f6.database.windows.net**.

## <a name="next-steps"></a>Sonraki adımlar

- Bir yönetilen örneğe bağlanma hakkında bilgi edinmek için:
  - Bağlantı seçenekleri uygulamalar için genel bakış için bkz: [uygulamalarınızı yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md).
  - Azure sanal makinesinden bir yönetilen örneğe bağlanma gösteren Hızlı Başlangıç için bkz: [bir Azure sanal makine bağlantısı yapılandırma](sql-database-managed-instance-configure-vm.md).
  - Yönetilen örnek için bir şirket içi istemci bilgisayardan bir noktadan siteye bağlantısı kullanarak bağlanmak nasıl oluşturulduğunu gösteren Hızlı Başlangıç için bkz: [noktadan siteye bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md).
- Mevcut SQL Server veritabanını şirket içinden bir yönetilen örneğine geri yüklemek için: 
    - Kullanım [geçiş için Azure veritabanı geçiş hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) bir veritabanı yedekleme dosyasından geri yüklemek için. 
    - Kullanım [T-SQL RESTORE komutunu](sql-database-managed-instance-get-started-restore.md) bir veritabanı yedekleme dosyasından geri yüklemek için.
- Gelişmiş sorun giderme yerleşik zekaya sahip yönetilen örnek veritabanı performansını izleme için bkz: [Azure SQL Analytics kullanarak Azure SQL veritabanını izleme](../azure-monitor/insights/azure-sql.md).
