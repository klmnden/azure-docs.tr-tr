---
title: 'Azure portal: SQL Yönetilen Örneği Oluşturma | Microsoft Docs'
description: Erişim için SQL Yönetilen Örneği, ağ ortamı ve istemci VM oluşturun.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: Carlrab
manager: craigg
ms.date: 11/28/2018
ms.openlocfilehash: d5be25abc634200e0c0afed6946b38fd163fb78e
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890509"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Hızlı Başlangıç: Azure SQL Veritabanı Yönetilen Örneği oluşturma

Bu hızlı başlangıç, Azure portalda bir Azure SQL Veritabanı [Yönetilen Örneği](sql-database-managed-instance.md) oluşturma adımlarını vermektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-managed-instance"></a>Yönetilen Örnek oluşturma

Aşağıdaki adımlar Yönetilen Örneğin nasıl oluşturulacağını gösterir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
2. **Yönetilen Örneği** bulun, ardından **Azure SQL Yönetilen Örneği**'ni seçin.
3. **Oluştur**’u seçin.

   ![Yönetilen örnek oluşturma](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Doldurun **yönetilen örneği** formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz | İçinde yeni kaynaklar oluşturma izniniz olan bir abonelik |
   |**Yönetilen örnek adı**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Ayrılmış bir sunucu düzeyindeki rolüdür gibi "serveradmin" kullanmayın.|
   |**Parola**|Geçerli bir parola|Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Konum**|Yönetilen Örneği oluşturmak istediğiniz konum|Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|Şunlardan birini seçin **yeni sanal ağ oluştur** veya daha önce bu formda sağladığınız kaynak grubu içinde daha önce oluşturduğunuz bir sanal ağ.| Özel ayarları olan bir Yönetilen Örnek için bir ağ yapılandırmaya ilişkin olarak Github'da [SQL Yönetilen Örnek sanal ağ ortamı şablonu yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment)'ya bakın. Yönetilen örneği için ağ ortamını yapılandırma gereksinimleriyle ilgili daha fazla bilgi için bkz: [Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma](sql-database-managed-instance-vnet-configuration.md). |
   |**Kaynak grubu**|Yeni veya mevcut bir kaynak grubu|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|

   ![yönetilen örnek formu](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Yönetilen örnek bir örnek yük devretme grubu ikincil kullanmak için kullanıma al'ı seçin ve DnsAzurePartner yönetilen örnek belirtin. Bu özellik Önizleme aşamasındadır ve eşlik eden ekran görüntüsünde gösterilen değil.
6. Seçin **fiyatlandırma katmanı** işlem ve depolama kaynaklarını boyutlandırmaya ek olarak için fiyatlandırma katmanı seçeneklerini gözden geçirin. 32 GB bellek ve 16 sanal çekirdekli Genel Amaçlı fiyatlandırma katmanı varsayılan değerdir.
7. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın.
8. İşlem tamamlandığında seçin **Uygula** seçiminizi kaydetmek için.  
9. Seçin **Oluştur** yönetilen örneği dağıtmak için.
10. Seçin **bildirimleri** dağıtım durumu görüntülenecek simge.

    ![yönetilen örnek dağıtım ilerleme durumu](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Seçin **dağıtım sürüyor** daha da dağıtımın ilerleme durumunu izlemek üzere yönetilen örnek penceresini açmak için.

> [!IMPORTANT]
> Bir alt ağdaki ilk örnek için dağıtım sonraki durumda genellikle daha uzun zamandır. Dağıtım işlemi beklediğinizden daha uzun sürdüğü için iptal etme. İkinci Yönetilen Örneğin alt ağda oluşturulması yalnızca birkaç dakika sürer.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Kaynakları gözden geçirin ve tam sunucu adınızı alma

Dağıtım başarıyla tamamlandıktan sonra oluşturulan kaynakları gözden geçirin ve sonraki hızlı başlangıçlarda kullanmak üzere tam sunucu adını alın.

1. Yönetilen Örneğinizin kaynak grubunu açın ve [Yönetilen Örnek oluşturma](#create-a-managed-instance) hızlı başlangıcında sizin iççin oluşturulan kaynaklarını görüntüleyin.

2. Yönetilen örneğinizi seçin.

   ![Yönetilen Örnek kaynakları](./media/sql-database-managed-instance-get-started/resources.png)

3. Üzerinde **genel bakış** sekmesinde, bulmak **konak** tam ana bilgisayar için yönetilen örneğe yönelik özellik ve kopyalama.

   ![Yönetilen Örnek kaynakları](./media/sql-database-managed-instance-get-started/host-name.png)

   Ad benzer **your_machine_name.a1b2c3d4e5f6.database.windows.net**.

## <a name="next-steps"></a>Sonraki adımlar

- Bir Yönetilen Örneğe bağlanma hakkında bilgi edinmek için bkz:
  - Uygulamaların bağlantı seçeneklerine genel bir bakış için bkz: [Uygulamalarınızı Yönetilen Örneğe bağlama](sql-database-managed-instance-connect-app.md).
  - Bir Azure sanal makinesinden bir Yönetilen Örneğe bağlanmayı gösteren bir hızlı başlangıç için bkz: [Azure sanal makine bağlantısı yapılandırma](sql-database-managed-instance-configure-vm.md).
  - Şirket içi bir istemci bilgisayardan noktadan siteye bağlantı kullanarak bir Yönetilen Örneğe bağlanmayı gösteren bir hızlı başlangıç için bkz: [Siteden noktaya bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md).
- Mevcut bir SQL Server veritabanını şirket içinden Yönetilen bir örneğe geri yüklerken, veritabanı yedekleme dosyasından geri yükleme işlemini [Geçiş için Azure Veritabanı Geçiş Hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) veya [T-SQL RESTORE komutu](sql-database-managed-instance-get-started-restore.md) ile yapabilirsiniz.
- Gelişmiş sorun giderme yerleşik zekaya sahip yönetilen örnek veritabanı performansını izleme için bkz: [kullanarak Azure SQL Analytics İzleyici Azure SQL veritabanı](../azure-monitor/insights/azure-sql.md)
