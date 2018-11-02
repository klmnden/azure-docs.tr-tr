---
title: İstemci VM - Azure SQL veritabanı yönetilen örneği bağlayın | Microsoft Docs
description: Azure SQL veritabanı yönetilen SQL Server Management Studio kullanarak bir Azure sanal makine örneğine bağlanın.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, srbozovi, bonova
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 97362cb91c16f91d637283df7a583f685124a21b
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913679"
---
# <a name="quickstart-configure-azure-vm-to-connect-to-an-azure-sql-database-managed-instance"></a>Hızlı Başlangıç: Azure VM, Azure SQL veritabanı yönetilen örneğine bağlanmak için yapılandırın

Bu hızlı başlangıçlar, bir Azure SQL veritabanı yönetilen SQL Server Management Studio (SSMS) kullanarak örneğine bağlanmak için bir Azure sanal makinesi yapılandırma işlemi gösterilmektedir. Noktadan siteye bağlantısı kullanarak bir şirket içi istemci bilgisayarından bağlanmak gösteren bir hızlı başlangıç için bkz: [noktadan siteye bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md) 

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıçta, başlangıç noktası olarak bu hızlı başlangıçta oluşturulan kaynakları kullanır: [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="create-a-new-subnet-in-the-managed-instance-vnet"></a>Yönetilen örnek sanal ağda yeni bir alt ağ oluşturma

Aşağıdaki adımlar yönetilen örnek yönetilen örneğine bağlanmak için sanal ağ içindeki bir Azure sanal makinesi için yeni bir alt ağ oluşturun. Yönetilen örnek alt yönetilen örnekler için ayrılmıştır ve bu alt ağdaki diğer kaynakları (örneğin Azure sanal makineler) oluşturulamıyor. 

1. Yönetilen örnek, oluşturduğunuz kaynak grubunu açın [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md) hızlı başlangıç, yönetilen örneği için sanal ağ'a tıklayın ve ardından **alt ağlar**.

   ![Yönetilen Örnek kaynakları](./media/sql-database-managed-instance-configure-vm/resources.png)

2. Tıklayın **+** yanındaki oturum **alt** yeni bir alt ağ oluşturmak için.

   ![Yönetilen örnek alt ağlar](./media/sql-database-managed-instance-configure-vm/subnets.png)

3. Formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ---------------- | ----------------- | ----------- | 
   | **Ad** | Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   | **Adres aralığı (CIDR bloğu)** | Geçerli bir aralık | Bu Hızlı Başlangıç için iyi varsayılan değerdir.|
   | **Ağ güvenlik grubu** | None | Bu Hızlı Başlangıç için iyi varsayılan değerdir.|
   | **Yol tablosu** | None | Bu Hızlı Başlangıç için iyi varsayılan değerdir.|
   | ** Hizmet uç noktalarını ** | 0 adet seçildi | Bu Hızlı Başlangıç için iyi varsayılan değerdir.|
   | **Alt ağ temsilci seçme** | None | Bu Hızlı Başlangıç için iyi varsayılan değerdir.|
 
   ![İstemci sanal makine için yeni bir yönetilen örnek alt](./media/sql-database-managed-instance-configure-vm/new-subnet.png)

4. Tıklayın **Tamam** yönetilen örnek sanal ağda bu ek alt ağ oluşturmak için.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Sanal ağdaki yeni alt ağ içinde sanal makine oluşturma

Aşağıdaki adımlar yönetilen örneğine bağlanmak için yeni bir alt ağ içinde bir sanal makine oluşturma işlemini gösterir. 

## <a name="prepare-the-azure-virtual-machine"></a>Azure sanal makinesi hazırlama

SQL yönetilen örneği, özel sanal ağınızdaki yerleştirilir olduğundan, yönetilen örneğe bağlanma ve sorgu yürütmek için SQL Server Management Studio veya Azure Data Studio gibi bazı yüklü SQL istemci aracı ile Azure VM oluşturmak gerekir. Bu hızlı başlangıçta, SQL Server Management Studio kullanılmaktadır.

Tüm gerekli araçları ile bir istemci sanal makine oluşturmak için en kolay yolu, Azure Resource Manager şablonları kullanmaktır.

1. İstemci sanal makine oluşturma ve SQL Server Management Studio'yu yüklemek için aşağıdaki düğmeye tıklayın (, başka bir tarayıcı sekmesinde Azure portalında oturum, emin olun):

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ---------------- | ----------------- | ----------- |
   | **Abonelik** | Geçerli bir abonelik | Yeni kaynaklar oluşturma iznine sahip olduğunuz bir abonelik olmalıdır |
   | **Kaynak Grubu** |Belirtilen kaynak grubu [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md) hızlı başlangıç.|Bu sanal ağın bulunduğu kaynak grubu olması gerekir.|
   | **Konum** | Kaynak grubu konumu | Bu değer, seçili kaynak grubuna göre doldurulur | 
   | **Sanal makine adı**  | Geçerli bir ad | Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetici kullanıcı adı**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Ayrılmış bir sunucu düzeyi rolü olduğundan "serveradmin" kullanmayın.| 
   |**Parola**|Geçerli bir parola|Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   | **Sanal makine boyutu** | Herhangi bir geçerli boyut | Bu şablon varsayılan ** Standard_B2s Bu Hızlı Başlangıç için yeterli. |
   | **Konum**|[resourceGroup () .location].| Bu değeri değiştirmeyin |
   | **Sanal ağ adı**|Daha önce seçtiğiniz konum|Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   | **Alt ağ adı**|Önceki yordamda oluşturduğunuz alt ağ adı| Yönetilen örnek oluşturduğunuz alt ağ seçin değil|
   | **yapıtları konumu** | [dağıtım.properties.templateLink.uri ()]  Bu değeri değiştirmeyin |
   | **yapıtları konumu Sas belirteci** | Boş bırakın | Bu değeri değiştirmeyin |

   ![İstemci sanal makinesi oluşturma](./media/sql-database-managed-instance-configure-vm/create-client-sql-vm.png)

   Önerilen sanal ağ adına ve varsayılan alt ağda kullanılan [yönetilen örneğinizi oluşturma](sql-database-managed-instance-get-started.md), son iki parametreyi değiştirmek gerekmez. Aksi halde ağ ortamını ayarlarken girdiğiniz değerlere bu değerleri değiştirmeniz gerekir.

3. Seçin **yukarıda belirtilen hüküm ve koşulları kabul ediyorum** onay kutusu.
4. Tıklayın **satın alma** ağınızda Azure VM dağıtmak için.
5. Dağıtım durumunu görüntülemek için **Bildirimler** simgesine tıklayın.
   
   Azure sanal makinesi oluşturulana kadar devam etmeyin. 

## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Aşağıdaki adımlarda, uzak masaüstü bağlantısı kullanarak yeni oluşturduğunuz sanal makineye bağlanma işlemi gösterilmektedir.

1. Dağıtım tamamlandıktan sonra sanal makine kaynağına gidin.

    ![VM](./media/sql-database-managed-instance-configure-vm/vm.png)  

2. **Bağlan**'a tıklayın. 
   
   Genel IP adresi ve bağlantı noktası numarasını sanal makine ile bir Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) formu görüntülenir. 

   ![RDP formu](./media/sql-database-managed-instance-configure-vm/rdp.png)  

3. Tıklayın **RDP dosyasını indir**.
 
   > [!NOTE]
   > Ayrıca, sanal Makinenize bağlanmak için SSH kullanabilirsiniz.

4. Kapat **sanal makineye bağlanma** formu.
5. VM'nize bağlanmak için indirilen RDP dosyasını açın. 
6. İstenirse **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

6. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

7. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

Sunucu Yöneticisi panosunda sanal makinenizle bağlantı kurulur.

## <a name="use-ssms-to-connect-to-the-managed-instance"></a>SSMS, yönetilen örneği'ne bağlanın

1. Sanal makineler'de SQL Server Management Studio (SSMS) açın.
 
   SSMS başlatıldı ilk olarak, yapılandırmayı tamamlamak gereken açmak için birkaç dakika sürer.
2. İçinde **sunucuya Bağlan** iletişim kutusunda, tam girin **ana bilgisayar adı** yönetilen örneğinizin **sunucu adı** kutusunda **SQL Server Kimlik doğrulaması**, kullanıcı adı ve parola sağlayın ve ardından **Connect**.

    ![ssms bağlanma](./media/sql-database-managed-instance-configure-vm/ssms-connect.png)  

Bağlandıktan sonra Veritabanları düğümündeki sistem ve kullanıcı veritabanlarınızı ve Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ile XEvent Profil Oluşturucu düğümlerindeki çeşitli nesneleri görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Noktadan siteye bağlantısı kullanarak bir şirket içi istemci bilgisayarından bağlanmak gösteren bir hızlı başlangıç için bkz: [noktadan siteye bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md).
- Uygulamaların bağlantı seçeneklerine genel bir bakış için bkz: [Uygulamalarınızı Yönetilen Örneğe bağlama](sql-database-managed-instance-connect-app.md).
- Mevcut bir SQL Server veritabanını şirket içinden Yönetilen bir örneğe geri yüklerken, veritabanı yedekleme dosyasından geri yükleme işlemini [Geçiş için Azure Veritabanı Geçiş Hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) veya [T-SQL RESTORE komutu](sql-database-managed-instance-get-started-restore.md) ile yapabilirsiniz.
