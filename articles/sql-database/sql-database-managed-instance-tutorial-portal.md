---
title: 'Azure portalı: SQL Veritabanı Yönetilen Örneği Oluşturma | Microsoft Docs'
description: Sanal ağ içinde bir Azure SQL Veritabanı Yönetilen Örneği oluşturun ve SSMS kullanarak Wide World Importers veritabanını geri yükleyin.
keywords: sql veritabanı öğreticisi, sql veritabanı yönetilen örneği oluşturma
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 03/14/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: 774a761465cfd886b85378a35dd43ac656a7ee48
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2018
ms.locfileid: "31004486"
---
# <a name="create-an-azure-sql-database-managed-instance-in-the-azure-portal"></a>Azure portalında Azure SQL Veritabanı Yönetilen Örneği oluşturma

Bu öğreticide, bir sanal ağın (VNet) ayrılmış bir alt ağında Azure portalını kullanarak Azure SQL Veritabanı Yönetilen Örneği (önizleme) oluşturma işlemi gösterilmektedir. Daha sonra, aynı sanal ağ içindeki bir sanal makine üzerinde SQL Server Management Studio kullanarak Yönetilen Örneğe bağlanma ve sonra Azure blob depolamada depolanmış bir veritabanının yedeğini Yönetilen Örneğe geri yükleme işlemleri gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!IMPORTANT]
> Yönetilen Örneğin şu anda kullanılabilir olduğu bölgelerin listesi için bkz. [Azure SQL Veritabanı Yönetilen Örneği ile tam yönetilen hizmete veritabanlarınızı geçirme](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/#create/Microsoft.SQLManagedInstance)’da oturum açın.

## <a name="whitelist-your-subscription"></a>Aboneliğinizi izin verilenler listesine ekleme

Yönetilen Örnek başlangıçta aboneliğinizin güvenilir listeye eklenmesini gerektiren geçitli bir genel önizleme olarak yayınlanır. Aboneliğiniz henüz izin verilenler listesinde değilse, sunulması ve önizleme koşullarını kabul edip izin verilenler listesine ekleme isteği göndermek için aşağıdaki adımları kullanın.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yönetilen Örnek**’i bulup **Azure SQL Veritabanı Yönetilen Örneği (önizleme)** öğesini seçin.
3. **Oluştur**’a tıklayın.

   ![yönetilen örnek oluşturma](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Aboneliğinizi seçin, **Önizleme koşulları**’na tıklayın ve ardından istenen bilgileri sağlayın.

   ![yönetilen örnek önizleme koşulları](./media/sql-database-managed-instance-tutorial/preview-terms.png)

5. İşlem tamamlandığında **Tamam**’a tıklayın.

   ![yönetilen örnek önizleme koşulları](./media/sql-database-managed-instance-tutorial/preview-approval-pending.png)

   > [!NOTE]
   > Önizleme onayını beklerken devam edip bu öğreticinin sonraki birkaç bölümünü tamamlayabilirsiniz.

## <a name="configure-a-virtual-network-vnet"></a>Sanal ağ yapılandırma (VNet)

Aşağıdaki adımlarda, Yönetilen Örneğiniz tarafından kullanılacak yeni bir [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md) sanal ağı (VNet) oluşturma işlemi gösterilmektedir. Sanal ağ yapılandırması hakkında daha fazla bilgi için bkz. [Yönetilen Örnek Sanal Ağ Yapılandırması](sql-database-managed-instance-vnet-configuration.md).

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Sanal Ağ**’ı bulup tıklayın, dağıtım modu olarak **Resource Manager**’ın seçili olduğunu doğrulayın ve ardından **Oluştur**’a tıklayın.

   ![sanal ağ oluşturma](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Sanal ağ formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Adres alanı**|10.14.0.0/24 gibi geçerli bir adres aralığı|CIDR gösteriminde sanal ağın adres adı.|
   |**Abonelik**|Aboneliğiniz|Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions).|
   |**Kaynak Grubu**|Geçerli bir kaynak grubu (yeni veya var olan)|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Konum**|Geçerli bir konum| Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   |**Alt ağ adı**|mi_subnet gibi geçerli bir alt ağ adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Alt ağ adres aralığı**|10.14.0.0/28 gibi geçerli bir alt ağ adresi. Aynı sanal ağda test / istemci uygulamalarını barındırmaya yönelik bir alt ağ ya da şirket içi veya diğer sanal ağlardan bağlanmak için ağ geçidi alt ağları gibi başka alt ağlar oluşturmaya yer bırakmak amacıyla, adres alanının kendisinden daha küçük bir alt ağ adres alanı kullanın.|CIDR gösteriminde alt ağın adres aralığı. Bu aralık, sanal ağın adres aralığı kapsamında olmalıdır|
   |**Hizmet uç noktaları**|Devre dışı|Bu alt ağ için bir veya daha fazla hizmet uç noktası etkinleştirin|
   ||||

   ![sanal ağ oluşturma formu](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. **Oluştur**’a tıklayın.

## <a name="create-new-route-table-and-a-route"></a>Yeni yol tablosu ve yol oluşturma

Aşağıdaki adımlarda 0.0.0.0/0 Sonraki Atlama İnternet yolu oluşturma işlemi gösterilmektedir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yol tablosu**’nu bulup tıklayın ve ardından Yol tablosu sayfasında **Oluştur**’a tıklayın. 

   ![yol tablosu oluşturma](./media/sql-database-managed-instance-tutorial/route-table-create.png)

3. Yol tablosu formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Abonelik**|Aboneliğiniz|Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions).|
   |**Kaynak Grubu**|Önceki yordamda oluşturduğunuz kaynak grubunu seçin|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Konum**|Önceki yordamda belirttiğiniz konumu seçin| Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   |**BCP yol yaymayı devre dışı bırakma**|Devre dışı||
   ||||

   ![yol tablosu oluşturma formu](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

4. **Oluştur**’a tıklayın.
5. Yol tablosu oluşturulduktan sonra yeni oluşturulan yol tablosunu açın.

   ![yol tablosu](./media/sql-database-managed-instance-tutorial/route-table.png)

6. **Yollar** ve ardından **Ekle**'ye tıklayın.

   ![yol tablosu ekleme](./media/sql-database-managed-instance-tutorial/route-table-add.png)

7.  Aşağıdaki tabloda verilen bilgileri kullanarak **0.0.0.0/0 Sonraki Atlama İnternet yolu**’nu **tek** yol olarak ekleyin:

    | Ayar| Önerilen değer | Açıklama |
    | ------ | --------------- | ----------- |
    |**Yol adı**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
    |**Adres ön eki**|0.0.0.0/0|Bu yolun uygulandığı CIDR gösterimindeki hedef IP adresi.|
    |**Sonraki atlama türü**|Internet|Sonraki atlama bu yol için eşleşen paketleri işler|
    |||

    ![yol](./media/sql-database-managed-instance-tutorial/route.png)

8. **Tamam**’a tıklayın.

## <a name="to-apply-the-route-table-to-the-managed-instance-subnet"></a>Yol tablosunu Yönetilen Örnek alt ağına uygulamak için

Aşağıdaki adımlar Yönetilen Örnek alt ağında yeni bir yol tablosu ayarlama işlemini göstermektedir.

1. Yönetilen Örnek alt ağında yol tablosu ayarlamak için daha önce oluşturduğunuz sanal ağı açın.
2. **Alt Ağlar**’a ve ardından Yönetilen Örnek alt ağına (aşağıdaki ekran görüntüsünde **mi_subnet**) tıklayın.

    ![alt ağ](./media/sql-database-managed-instance-tutorial/subnet.png)

11. **Yol tablosu**’na tıklayın ve ardından **myMI_route_table** öğesini seçin.

    ![yol tablosu ayarlama](./media/sql-database-managed-instance-tutorial/set-route-table.png)

12. **Kaydet**’e tıklayın

    ![yol tablosu ayarlama-kaydetme](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)

## <a name="create-a-managed-instance"></a>Yönetilen Örnek oluşturma

Aşağıdaki adımlar, önizlemeniz onaylandıktan sonra Yönetilen Örneğinizi oluşturma işlemini gösterir.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Yönetilen Örnek**’i bulup **Azure SQL Veritabanı Yönetilen Örneği (önizleme)** öğesini seçin.
3. **Oluştur**’a tıklayın.

   ![yönetilen örnek oluşturma](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Aboneliğinizi seçin ve önizleme koşullarında **Kabul Edildi** ifadesinin gösterildiğini doğrulayın.

   ![yönetilen örnek önizlemesi kabul edildi](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

4. Yönetilen Örnek formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Yönetilen örnek adı**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).| 
   |**Parola**|Geçerli bir parola|Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Kaynak Grubu**|Önceden oluşturduğunuz kaynak grubu||
   |**Konum**|Daha önce seçtiğiniz konum|Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|Daha önce oluşturduğunuz sanal ağ|

   ![yönetilen örnek oluşturma formu](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

5. İşlem ve depolama kaynaklarını boyutlandırmaya ek olarak fiyatlandırma katmanı seçeneklerini gözden geçirmek için **Fiyatlandırma katmanı**’na tıklayın. Varsayılan olarak, örneğiniz ücretsiz 32 GB depolama alanı alır ancak bu boyut uygulamalarınız için yeterli olmayabilir.
6. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın. 
   ![yönetilen örnek oluşturma formu](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

7. Tamamlandığında, seçiminizi kaydetmek için **Uygula**’ya tıklayın.  
8. Yönetilen Örneği dağıtmak için **Oluştur**’a tıklayın.
9. Dağıtım durumunu görüntülemek için **Bildirimler** simgesine tıklayın.
 
   ![dağıtım ilerleme durumu](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

9. Dağıtımın ilerleme durumunu daha ayrıntılı izlemek üzere Yönetilen Örnek penceresini açmak için **Dağıtım sürüyor**’a tıklayın.
 
   ![dağıtım ilerleme durumu 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Dağıtım gerçekleşirken sonraki yordama geçin.

> [!IMPORTANT]
> Bir alt ağdaki ilk örnek için dağıtım süresi genellikle sonraki örneklerden çok daha uzundur, bazen tamamlanması 24 saatten uzun sürer. Dağıtım işlemini beklediğinizden daha uzun sürdüğü için iptal etmeyin. İlk örneğinizi dağıtma süresinin uzunluğu geçici bir durumdur. Genel önizlemenin başlamasından kısa süre sonra dağıtım süresinde önemli bir azalma olmasını bekleyebilirsiniz.

## <a name="create-a-new-subnet-in-the-vnet-for-a-virtual-machine"></a>Bir sanal makine için sanal ağda yeni bir alt ağ oluşturma

Aşağıdaki adımlarda, SQL Server Management Studio yükleyip Yönetilen Örneğinize bağladığınız bir sanal makine için sanal ağda ikinci bir alt ağ oluşturma işlemi gösterilmektedir.

1. Sanal ağ kaynağınızı açın.
 
   ![Sanal ağ](./media/sql-database-managed-instance-tutorial/vnet.png)

2. **Alt ağlar**’a ve ardından **+Alt ağ**’a tıklayın.
 
   ![alt ağ ekleme](./media/sql-database-managed-instance-tutorial/add-subnet.png)

3. Alt ağ formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Adres aralığı (CIDR bloğu)**|Sanal ağ içindeki geçerli bir adres aralığı (varsayılanı kullanın)||
   |**Ağ güvenlik grubu**|None||
   |**Yol tablosu**|None||
   |**Hizmet uç noktaları**|None||

   ![vm alt ağ ayrıntıları](./media/sql-database-managed-instance-tutorial/vm-subnet-details.png)

4. **Tamam**’a tıklayın.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Sanal ağdaki yeni alt ağ içinde sanal makine oluşturma

Aşağıdaki adımlar, Yönetilen Örneği oluşturulduğu sanal ağın içinde bir sanal makine oluşturma işlemini göstermektedir. 

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **İşlem**'i ve sonra **Windows Server 2016 Datacenter** ya da **Windows 10**’u seçin. Öğreticinin bu bölümünde Windows Server kullanılır. Windows 10’un yapılandırılması da önemli ölçüde benzerdir. 

   ![işlem](./media/sql-database-managed-instance-tutorial/compute.png)

3. Sanal makine formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   | **VM disk türü**|SSD|SSD, fiyat ile performans arasında en iyi dengeyi sağlar.|   
   |**Kullanıcı adı**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).| 
   |**Parola**|Geçerli bir parola|Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.| 
   |**Abonelik**|Aboneliğiniz|Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions).|
   |**Kaynak Grubu**|Önceden oluşturduğunuz kaynak grubu||
   |**Konum**|Daha önce seçtiğiniz konum||
   |**Zaten bir Windows lisansım var**|Hayır|Etkin Yazılım Güvencesi (SA) olan Windows lisanslarına sahipseniz işlem maliyetinden tasarruf etmek için Azure Hibrit Avantajı'nı kullanın|
   ||||

   ![sanal makine oluşturma formu](./media/sql-database-managed-instance-tutorial/virtual-machine-create-form.png)

3. **Tamam**’a tıklayın.
4. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Bu öğretici için yalnızca küçük bir sanal makine gerekir.

    ![VM boyutları](./media/sql-database-managed-instance-tutorial/virtual-machine-size.png)  

5. **Seç**'e tıklayın.
6. **Ayarlar** formunda **Alt ağ**’a tıklayın ve sonra **vm_subnet**’i seçin. Yönetilen Örneğin hazırlandığı alt ağı değil, aynı sanal ağ içindeki farklı bir alt ağı seçin.

    ![VM ayarları](./media/sql-database-managed-instance-tutorial/virtual-machine-settings.png)  

7. **Tamam**’a tıklayın.
8. Özet sayfasında teklif ayrıntılarını gözden geçirin ve sonra **Oluştur**’a tıklayarak sanal makine dağıtımını başlatın.
 
## <a name="connect-to-virtual-machine"></a>Sanal makineye bağlanma

Aşağıdaki adımlarda, uzak masaüstü bağlantısı kullanarak yeni oluşturduğunuz sanal makineye bağlanma işlemi gösterilmektedir.

1. Dağıtım tamamlandıktan sonra sanal makine kaynağına gidin.

    ![VM](./media/sql-database-managed-instance-tutorial/vm.png)  

2. Sanal makine özelliklerinde **Bağlan** düğmesine tıklayın. Uzak Masaüstü Protokolü dosyasını (.rdp dosyası) oluşturulup indirilir.
3. VM'nize bağlanmak için indirilen RDP dosyasını açın. 
4. İstenirse **Bağlan**’a tıklayın. Mac bilgisayarlarda, Mac App Store’dan bu [Uzak Masaüstü İstemcisi](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) gibi bir RDP istemcisi indirmeniz gerekir.

5. Sanal makine oluştururken belirttiğiniz kullanıcı adı ile parolayı girin ve **Tamam**’a tıklayın.

6. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

Sunucu Yöneticisi panosunda sanal makinenizle bağlantı kurulur.

> [!IMPORTANT]
> Yönetilen Örneğiniz başarıyla hazırlanana kadar devam etmeyin. Hazırlandıktan sonra, Yönetilen Örneğinize ait **Genel Bakış** sekmesinde bulunan **Yönetilen Örnek** alanından örneğinizin ana bilgisayar adını alın. Ad şuna benzer bir değerdir: **drfadfadsfd.tr23.westus1-a.worker.database.windows.net**.

## <a name="install-ssms-and-connect-to-the-managed-instance"></a>SSMS yükleme ve Yönetilen Sunucuya bağlama

Aşağıdaki adımlar, SSMS indirip yükleme ve sonra Yönetilen Örneğinize bağlama adımlarını göstermektedir.

1. Sunucu Yöneticisi'nde sol bölmedeki **Yerel Sunucu**’ya tıklayın.

    ![sunucu yöneticisi özellikleri](./media/sql-database-managed-instance-tutorial/server-manager-properties.png)  

2. **Özellikler** bölmesinde **Açık**’a tıklayarak IE Artırılmış Güvenlik Yapılandırmasını değiştirin.
3. **Internet Explorer Artırılmış Güvenlik Yapılandırması** iletişim kutusunun Yöneticiler bölümünde **Kapalı**’ya ve sonra **Tamam**’a tıklayın.

    ![internet explorer artırılmış güvenlik yapılandırması](./media/sql-database-managed-instance-tutorial/internet-explorer-security-configuration.png)  
4. Görev çubuğundan **Internet Explorer**’ı açın.
5. **Önerilen güvenlik ve uyumluluk ayarlarını kullan**’ı seçin ve sonra **Tamam**’a tıklayarak Internet Explorer 11 kurulumunu tamamlayın.
6. URL adres kutusuna https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms adresini girin ve **Enter** tuşuna basın. 
7. SQL Server Management Studio’nun en son sürümünü indirin ve sorulduğunda **Çalıştır**’a tıklayın.
8. Sorulduğunda başlamak için **Yükle**’ye tıklayın.
9. Yükleme tamamlandığında **Kapat**'a tıklayın.
10. SSMS’i açın.
11. **Sunucuya Bağlan** iletişim kutusunda, **Sunucu adı** kutusuna Yönetilen Örneğinizin **ana bilgisayar adını* girin, **SQL Server Kimlik Doğrulaması**’nı seçin, kullanıcı adı ve parolanızı sağlayın ve sonra **Bağlan**’a tıklayın.

    ![ssms bağlanma](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Bağlandıktan sonra Veritabanları düğümündeki sistem ve kullanıcı veritabanlarınızı ve Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ile XEvent Profil Oluşturucu düğümlerindeki çeşitli nesneleri görüntüleyebilirsiniz.

## <a name="download-the-wide-world-importers---standard-backup-file"></a>Wide World Importers - Standart yedekleme dosyasını indirme

Wide World Importers - Standart yedekleme dosyasını indirmek için aşağıdaki adımları kullanın.

Internet Explorer’ı kullanarak URL adres kutusuna https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak adresini girin ve sonra istendiğinde bu dosyayı **İndirilenler** klasörüne kaydetmek için **Kaydet**’e tıklayın.

## <a name="create-azure-storage-account-and-upload-backup-file"></a>Azure depolama hesabı oluşturma ve yedekleme dosyasını karşıya yükleme

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın.
2. **Depolama**’yı bulup **Depolama Hesabı**’na tıklayarak depolama hesabı formunu açın.

   ![depolama hesabı](./media/sql-database-managed-instance-tutorial/storage-account.png)

3. Depolama hesabı formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Dağıtım modeli**|Kaynak modeli||
   |**Hesap türü**|Blob depolama||
   |**Performans**|Standart veya premium|Manyetik sürücüler veya SSD’ler|
   |**Çoğaltma**|Yerel olarak yedekli depolama||
   |**Erişim katmanı (varsayılan)|Seyrek veya sık||
   |**Güvenli aktarım gerekir**|Devre dışı||
   |**Abonelik**|Aboneliğiniz|Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions).|
   |**Kaynak grubu**|Önceden oluşturduğunuz kaynak grubu|| 
   |**Konum**|Daha önce seçtiğiniz konum||
   |**Sanal ağlar**|Devre dışı||

4. **Oluştur**’a tıklayın.

   ![depolama hesabı ayrıntıları](./media/sql-database-managed-instance-tutorial/storage-account-details.png)

5. Depolama hesabı dağıtımı başarılı olduktan sonra yeni depolama hesabınızı açın.
6. **Ayarlar** altında **Paylaşılan Erişim İmzası**’na tıklayarak Paylaşılan erişim imzası (SAS) formunu açın.

   ![sas formu](./media/sql-database-managed-instance-tutorial/sas-form.png)

7. SAS formunda varsayılan değerleri istenen şekilde değiştirin. Sona erme tarihi/saatinin varsayılan olarak yalnızca 8 saat olduğuna dikkat edin.
8. **SAS Oluştur**’a tıklayın.

   ![sas formu tamamlandı](./media/sql-database-managed-instance-tutorial/sas-generate.png)

9. **SAS belirtecini** ve **Blob sunucusu SAS URL’sini** kopyalayıp kaydedin.
10. **Ayarlar** altında **Kapsayıcılar**'a tıklayın.

    ![containers](./media/sql-database-managed-instance-tutorial/containers.png)

11. **+Kapsayıcı**’ya tıklayarak yedekleme dosyanızın tutulacağı kapsayıcıyı oluşturun.
12. Kapsayıcı formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

    | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Ad**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Genel erişim düzeyi**|Özel (anonim erişim yok)||

    ![kapsayıcı ayrıntısı](./media/sql-database-managed-instance-tutorial/container-detail.png)

13. **Tamam**’a tıklayın.
14. Kapsayıcı oluşturulduktan sonra kapsayıcıyı açın.

    ![kapsayıcı](./media/sql-database-managed-instance-tutorial/container.png)

15. **Kapsayıcı özellikleri**’ne tıklayın ve ardından kapsayıcıya URL'yi kopyalayın.

    ![kapsayıcı URL’si](./media/sql-database-managed-instance-tutorial/container-url.png)

16. **Karşıya Yükle**’ye tıklayarak **Blobu karşıya yükle** formunu açın.

    ![karşıya yükle](./media/sql-database-managed-instance-tutorial/upload.png)

17. İndirilenler klasörünüze göz atın ve **AdventureWorks2016.bak** dosyasını seçin.

    ![karşıya yükle](./media/sql-database-managed-instance-tutorial/upload-bak.png)

18. **Karşıya Yükle**'ye tıklayın.
19. Karşıya yükleme işlemi tamamlanana kadar devam etmeyin.

    ![karşıya yükleme tamamlandı](./media/sql-database-managed-instance-tutorial/upload-complete.png)


## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Wide World Importers veritabanını bir yedekleme dosyasından geri yükleme

SSMS ile Wide World Importers veritabanını yedekleme dosyasından Yönetilen Örneğinize geri yüklemek için aşağıdaki adımları kullanın.

1. SSMS içinde yeni bir sorgu penceresi açın.
2. Aşağıdaki betiği kullanarak bir SAS kimlik bilgisi oluşturun; depolama hesabı kapsayıcısının URL’sini ve SAS anahtarını belirtilen şekilde sağlayın.

   ```
CREATE CREDENTIAL [https://<storage_account_name>.blob.core.windows.net/<container>] 
WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
, SECRET = '<shared_access_signature_key_with_removed_first_?_symbol>' 
   ```

    ![kimlik bilgisi](./media/sql-database-managed-instance-tutorial/credential.png)

3. Aşağıdaki betiği kullanarak SAS kimlik bilgisini ve yedekleme geçerliliğini denetleyin; yedekleme dosyasını içeren kapsayıcının URL’sini sağlayın:

   ```
   RESTORE FILELISTONLY FROM URL = 
   'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'
   ```

    ![dosya listesi](./media/sql-database-managed-instance-tutorial/file-list.png)

4. Aşağıdaki betiği kullanarak Adventure Works 2012 veritabanını bir yedekleme dosyasından geri yükleyin; yedekleme dosyasını içeren kapsayıcının URL’sini sağlayın:

   ```
   RESTORE DATABASE [Wide World Importers] FROM URL =
  'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'`
   ```

    ![geri yükleme yürütülüyor](./media/sql-database-managed-instance-tutorial/restore-executing.png)

5. Geri yükleme işleminizin durumunu izlemek için aşağıdaki sorguyu yeni bir sorgu oturumunda çalıştırın:

   ```
SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
, dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
FROM sys.dm_exec_requests r 
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

    ![geri yükleme tamamlanma yüzdesi](./media/sql-database-managed-instance-tutorial/restore-percent-complete.png)

6. Geri yükleme tamamlandığında Nesne Gezgini içinde görüntüleyin. 

    ![geri yükleme tamamlandı](./media/sql-database-managed-instance-tutorial/restore-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen Örnek hakkında daha fazla bilgi için bkz. [Yönetilen Örnek nedir?](sql-database-managed-instance.md).
- Sanal ağ yapılandırması hakkında daha fazla bilgi için bkz. [Yönetilen Örnek Sanal Ağ Yapılandırması](sql-database-managed-instance-vnet-configuration.md).
- Uygulamaları bağlama hakkında daha fazla bilgi için bkz. [Uygulamaları bağlama](sql-database-managed-instance-connect-app.md).
- Azure Veritabanı Geçiş Hizmeti’ni (DMS) geçiş için kullanmaya ilişkin bir öğretici için bkz. [DMS kullanarak Yönetilen Örnek geçişi](../dms/tutorial-sql-server-to-managed-instance.md).
