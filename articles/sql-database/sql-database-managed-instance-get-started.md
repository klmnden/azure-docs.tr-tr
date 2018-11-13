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
ms.date: 11/01/2018
ms.openlocfilehash: 3eadc2d233fd1716716c323f4c7087ee8363c67c
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912331"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Hızlı Başlangıç: Azure SQL Veritabanı Yönetilen Örneği oluşturma

Bu hızlı başlangıç, Azure portalda bir Azure SQL Veritabanı [Yönetilen Örneği](sql-database-managed-instance.md) oluşturma adımlarını vermektedir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-managed-instance"></a>Yönetilen Örnek oluşturma

Aşağıdaki adımlar Yönetilen Örneğin nasıl oluşturulacağını gösterir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yönetilen Örneği** bulun, ardından **Azure SQL Yönetilen Örneği**'ni seçin.
3. **Oluştur**’a tıklayın.

   ![Yönetilen örnek oluşturma](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Yönetilen Örnek formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   | **Abonelik** | Aboneliğiniz | İçinde yeni kaynaklar oluşturma izniniz olan bir abonelik |
   |**Yönetilen örnek adı**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Ayrılmış bir sunucu düzeyi rolü olduğundan "serveradmin" kullanmayın.| 
   |**Parola**|Geçerli bir parola|Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Kaynak Grubu**|Yeni veya mevcut bir kaynak grubu|Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Konum**|Yönetilen Örneği oluşturmak istediğiniz konum|Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|**Yeni sanal ağ oluştur**'u ya da bu formda daha önce sağladığınız kaynak grubunda önceden oluşturduğunuz bir sanal ağı seçin| Özel ayarları olan bir Yönetilen Örnek için bir ağ yapılandırmaya ilişkin olarak Github'da [SQL Yönetilen Örnek sanal ağ ortamı şablonu yapılandırma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment)'ya bakın. Bir Yönetilen Örneğin ağ ortamını yapılandırma gereksinimleri hakkında bilgi için bkz: [Azure SQL Veritabanı Yönetilen Örneği için bir VNet yapılandırma](sql-database-managed-instance-vnet-configuration.md) |

   ![yönetilen örnek formu](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. İşlem ve depolama kaynaklarını boyutlandırmaya ek olarak fiyatlandırma katmanı seçeneklerini gözden geçirmek için **Fiyatlandırma katmanı**’na tıklayın. 32 GB bellek ve 16 sanal çekirdekli Genel Amaçlı fiyatlandırma katmanı varsayılan değerdir.
6. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın. 
7. Tamamlandığında, seçiminizi kaydetmek için **Uygula**’ya tıklayın.  
8. Yönetilen Örneği dağıtmak için **Oluştur**’a tıklayın.
9. Dağıtım durumunu görüntülemek için **Bildirimler** simgesine tıklayın.

    ![yönetilen örnek dağıtım ilerleme durumu](./media/sql-database-managed-instance-get-started/deployment-progress.png)

10. Dağıtımın ilerleme durumunu daha ayrıntılı izlemek üzere Yönetilen Örnek penceresini açmak için **Dağıtım sürüyor**’a tıklayın. 

> [!IMPORTANT]
> Bir alt ağdaki ilk örnek için dağıtım süresi genellikle sonraki örneklerden çok daha uzundur. Dağıtım işlemini beklediğinizden daha uzun sürdüğü için iptal etmeyin. İkinci Yönetilen Örneğin alt ağda oluşturulması yalnızca birkaç dakika sürer.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Kaynakları gözden geçirme ve tam sunucu adınızı alma

Dağıtım başarıyla tamamlandıktan sonra oluşturulan kaynakları gözden geçirin ve sonraki hızlı başlangıçlarda kullanmak üzere tam sunucu adını alın.

1. Yönetilen Örneğinizin kaynak grubunu açın ve [Yönetilen Örnek oluşturma](sql-database-managed-instance-get-started.md) hızlı başlangıcında sizin iççin oluşturulan kaynaklarını görüntüleyin.

   ![Yönetilen Örnek kaynakları](./media/sql-database-managed-instance-get-started/resources.png)Yönetilen Örnek kaynağınızı Azure portalda açın.

2. Yönetilen Örneğinize tıklayın.
3. **Genel bakış** sekmesinde **Konak** özelliğini bulun ve Yönetilen Örneğin tam konak adresini kopyalayın.


   ![Yönetilen Örnek kaynakları](./media/sql-database-managed-instance-get-started/host-name.png)

   Ad şuna benzer: **makine_adiniz.neu15011648751ff.database.windows.net**.

## <a name="next-steps"></a>Sonraki adımlar

- Bir Yönetilen Örneğe bağlanma hakkında bilgi edinmek için bkz:
  - Uygulamaların bağlantı seçeneklerine genel bir bakış için bkz: [Uygulamalarınızı Yönetilen Örneğe bağlama](sql-database-managed-instance-connect-app.md).
  - Bir Azure sanal makinesinden bir Yönetilen Örneğe bağlanmayı gösteren bir hızlı başlangıç için bkz: [Azure sanal makine bağlantısı yapılandırma](sql-database-managed-instance-configure-vm.md).
  - Şirket içi bir istemci bilgisayardan noktadan siteye bağlantı kullanarak bir Yönetilen Örneğe bağlanmayı gösteren bir hızlı başlangıç için bkz: [Siteden noktaya bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md).
- Mevcut bir SQL Server veritabanını şirket içinden Yönetilen bir örneğe geri yüklerken, veritabanı yedekleme dosyasından geri yükleme işlemini [Geçiş için Azure Veritabanı Geçiş Hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) veya [T-SQL RESTORE komutu](sql-database-managed-instance-get-started-restore.md) ile yapabilirsiniz.
- Sorun gidermeye yönelik yerleşik makine zekasını da içeren gelişmiş Yönetilen Örnek veritabanı performansı izleme seçenekleri için bkz. [Azure SQL Veritabanı'nı Azure SQL Analytics ile izleme](../log-analytics/log-analytics-azure-sql.md)
