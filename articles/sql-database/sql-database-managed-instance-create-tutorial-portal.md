---
title: 'Azure portalı: SQL Veritabanı Yönetilen Örneği Oluşturma | Microsoft Docs'
description: VNet’te Azure SQL Veritabanı Yönetilen Örneği oluşturun.
keywords: sql veritabanı öğreticisi, sql veritabanı yönetilen örneği oluşturma
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 05/09/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: a019b21c130bebfe27925e90d7f7843d92654e01
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41918556"
---
# <a name="create-an-azure-sql-database-managed-instance-in-the-azure-portal"></a>Azure portalında Azure SQL Veritabanı Yönetilen Örneği oluşturma

Bu öğreticide, bir sanal ağın (VNet) ayrılmış alt ağında Azure portalını kullanarak Azure SQL Veritabanı Yönetilen Örneği (önizleme) oluşturma ve sonra aynı sanal ağ üzerindeki bir sanal makinede SQL Server Management Studio kullanarak Yönetilen Örneğe bağlanma işlemi gösterilmektedir.

> [!div class="checklist"]
> * Aboneliğinizi izin verilenler listesine ekleme
> * Sanal ağ yapılandırma (VNet)
> * Yeni yol tablosu ve yol oluşturma
> * Rota tablosunu Yönetilen Örnek alt ağına uygulama
> * Yönetilen Örnek oluşturma
> * Bir sanal makine için sanal ağda yeni bir alt ağ oluşturma
> * Sanal ağdaki yeni alt ağ içinde sanal makine oluşturma
> * Sanal makineye bağlanma
> * SSMS yükleme ve Yönetilen Sunucuya bağlama

> [!Note]
> Bu öğreticide ağ, alt ağ, örnek ve sanal makine yapılandırma işlemlerini Azure portalı kullanarak gerçekleştirme adımları açıklanmaktadır. Bu adımlar daha uzun sürebilir. Örneğe erişmek için kullanılan ağın ve sanal makinenin "Azure'a Dağıt" düğmesiyle tek seferde yapıldığı daha kısa bir öğreticiye ihtiyacınız varsa [Kullanmaya başlama öğreticisine](sql-database-managed-instance-get-started.md) göz atabilirsiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!IMPORTANT]
> Yönetilen Örneğin şu anda kullanılabilir olduğu bölgelerin listesi için bkz. [Azure SQL Veritabanı Yönetilen Örneği ile tam yönetilen hizmete veritabanlarınızı geçirme](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

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
   |**BCP yol yaymayı devre dışı bırakma**|Etkin||
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

8. **Tamam** düğmesine tıklayın.

## <a name="apply-the-route-table-to-the-managed-instance-subnet"></a>Rota tablosunu Yönetilen Örnek alt ağına uygulama

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

4. Aboneliğinizi seçin ve önizleme koşullarında **Kabul Edildi** ifadesinin gösterildiğini doğrulayın.

   ![yönetilen örnek önizlemesi kabul edildi](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

5. Yönetilen Örnek formunu aşağıdaki tabloda verilen bilgileri kullanarak istenen bilgilerle doldurun:

   | Ayar| Önerilen değer | Açıklama |
   | ------ | --------------- | ----------- |
   |**Yönetilen örnek adı**|Geçerli bir ad|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Yönetilen örnek yöneticisi oturum açma**|Geçerli bir kullanıcı adı|Geçerli adlar için bkz. [Adlandırma kuralları ve kısıtlamalar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Ayrılmış bir sunucu düzeyi rolü olduğundan "serveradmin" kullanmayın.| 
   |**Parola**|Geçerli bir parola|Parola en az 16 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır.|
   |**Kaynak Grubu**|Önceden oluşturduğunuz kaynak grubu||
   |**Konum**|Daha önce seçtiğiniz konum|Bölgeler hakkında bilgi için bkz. [Azure Bölgeleri](https://azure.microsoft.com/regions/).|
   |**Sanal ağ**|Daha önce oluşturduğunuz sanal ağ|

   ![yönetilen örnek oluşturma formu](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

6. İşlem ve depolama kaynaklarını boyutlandırmaya ek olarak fiyatlandırma katmanı seçeneklerini gözden geçirmek için **Fiyatlandırma katmanı**’na tıklayın. Varsayılan olarak, örneğiniz ücretsiz 32 GB depolama alanı alır ancak bu boyut uygulamalarınız için yeterli olmayabilir.
7. Depolama miktarını ve sanal çekirdek sayısını belirtmek için kaydırıcıları veya metin çubuklarını kullanın. 
   ![yönetilen örnek fiyatlandırma katmanı](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

8. Tamamlandığında, seçiminizi kaydetmek için **Uygula**’ya tıklayın.  
9. Yönetilen Örneği dağıtmak için **Oluştur**’a tıklayın.
10. Dağıtım durumunu görüntülemek için **Bildirimler** simgesine tıklayın.
 
   ![dağıtım ilerleme durumu](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

11. Dağıtımın ilerleme durumunu daha ayrıntılı izlemek üzere Yönetilen Örnek penceresini açmak için **Dağıtım sürüyor**’a tıklayın.
 
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

4. **Tamam** düğmesine tıklayın.

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

4. **Tamam** düğmesine tıklayın.
5. VM için bir boyut seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Bu öğretici için yalnızca küçük bir sanal makine gerekir.

    ![VM boyutları](./media/sql-database-managed-instance-tutorial/virtual-machine-size.png)  

6. **Seç**'e tıklayın.
7. **Ayarlar** formunda **Alt ağ**’a tıklayın ve sonra **vm_subnet**’i seçin. Yönetilen Örneğin hazırlandığı alt ağı değil, aynı sanal ağ içindeki farklı bir alt ağı seçin.

    ![VM ayarları](./media/sql-database-managed-instance-tutorial/virtual-machine-settings.png)  

8. **Tamam** düğmesine tıklayın.
9. Özet sayfasında teklif ayrıntılarını gözden geçirin ve sonra **Oluştur**’a tıklayarak sanal makine dağıtımını başlatın.
 
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
11. **Sunucuya Bağlan** iletişim kutusunda, **Sunucu adı** kutusuna Yönetilen Örneğinizin **ana bilgisayar adını** girin, **SQL Server Kimlik Doğrulaması**’nı seçin, kullanıcı adı ve parolanızı sağlayın ve sonra **Bağlan**’a tıklayın.

    ![ssms bağlanma](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Bağlandıktan sonra Veritabanları düğümündeki sistem ve kullanıcı veritabanlarınızı ve Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ile XEvent Profil Oluşturucu düğümlerindeki çeşitli nesneleri görüntüleyebilirsiniz.

> [!NOTE]
> Mevcut bir SQL veritabanını, Yönetilen örneğe geri yüklemek için [Geçiş için Azure Veritabanı Geçiş Hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) kullanarak bir veritabanı yedekleme dosyasından geri yükleyebilir, [T-SQL RESTORE komutunu](sql-database-managed-instance-restore-from-backup-tutorial.md) kullanarak bir veritabanı yedekleme dosyasından geri yükleyebilir veya [BACPAC dosyasından içeri aktar](sql-database-import.md) işlemi yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal ağın (VNet) ayrılmış alt ağında Azure portalını kullanarak Azure SQL Veritabanı Yönetilen Örneği (önizleme) oluşturma ve sonra aynı sanal ağ üzerindeki bir sanal makinede SQL Server Management Studio kullanarak Yönetilen Örneğe bağlanma işlemini öğrendiniz.  Şunları öğrendiniz: 

> [!div class="checklist"]
> * Aboneliğinizi izin verilenler listesine ekleme
> * Sanal ağ yapılandırma (VNet)
> * Yeni yol tablosu ve yol oluşturma
> * Rota tablosunu Yönetilen Örnek alt ağına uygulama
> * Yönetilen Örnek oluşturma
> * Bir sanal makine için sanal ağda yeni bir alt ağ oluşturma
> * Sanal ağdaki yeni alt ağ içinde sanal makine oluşturma
> * Sanal makineye bağlanma
> * SSMS yükleme ve Yönetilen Sunucuya bağlama

Veritabanı yedeklemesinin Azure SQL Veritabanı Yönetilen Örneği’ne nasıl geri yükleneceğini öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
>[Veritabanı yedeklemesini Azure SQL Veritabanı Yönetilen Örneğine geri yükleme](sql-database-managed-instance-restore-from-backup-tutorial.md)
