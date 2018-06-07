---
title: Azure Data factory'de veri taşımayı ilgili güvenlik konuları | Microsoft Docs
description: Azure Data factory'de veri taşımayı güvenliğini sağlama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: cad363309b6086197ced1a5d1c1793995db11228
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34621634"
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - veri taşıma için güvenlik konuları

> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [Data Factory sürüm 2 için veri taşıma güvenlik konuları](../data-movement-security-considerations.md).

## <a name="introduction"></a>Giriş
Bu makalede, verilerinizin güvenliğini sağlamak için Azure Data Factory veri taşıma hizmetleri kullanan temel güvenlik altyapısı açıklanmaktadır. Azure veri fabrikası yönetim kaynakları Azure güvenlik altyapı üzerine kurulmuş ve Azure tarafından sunulan tüm olası güvenlik önlemleri kullanın.

Bir Data Factory çözümünde bir veya daha fazla [işlem hattı](data-factory-create-pipelines.md) oluşturursunuz. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bu ardışık düzen veri fabrikası oluşturulduğu bölgede yer alır. 

Data Factory yalnızca kullanılabilir olsa bile **Batı ABD**, **Doğu ABD**, ve **Kuzey Avrupa** bölgeler, veri taşıma Hizmeti'nde kullanılabilir [içinde genel olarak birkaç bölgeleri](data-factory-data-movement-activities.md#global). Data Factory Hizmeti'ne sağlar veri coğrafi bölgeye bırakmaz / bölge hizmetini veri taşıma hizmeti bu bölgeye henüz dağıtılmamışsa, alternatif bir bölge kullanacak biçimde açıkça toplamasını sürece. 

Azure Data Factory'nin kendisi sertifikalar kullanılarak şifrelenmiş bulut veri depoları için bağlı hizmet kimlik bilgilerini dışında herhangi bir veriyi depolamaz. Veri hareketini [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](data-factory-compute-linked-services.md) kullanarak düzenlemek için veri temelinde iş akışları oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak [iş akışlarını izlemenizi ve yönetmenizi](data-factory-monitor-manage-pipelines.md) de sağlar.

Azure Data Factory kullanarak veri taşıma süredir **sertifikalı** için:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA YILDIZ](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Azure uyumluluk ve Azure kendi altyapısını nasıl korur ilgileniyorsanız, ziyaret [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

Bu makalede, aşağıdaki iki veri taşıma senaryolarda güvenlik konuları inceleyin: 

- **Bulut senaryosu**-Bu senaryoda, hem kaynak hem de hedef Internet üzerinden genel olarak erişilebilir. Bunlar Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon Redshift Salesforce gibi SaaS Hizmetleri ve FTP ve OData gibi web protokoller gibi yönetilen bulut depolama hizmetlerine içerir. Desteklenen veri kaynaklarının tam listesi bulabilirsiniz [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Karma senaryo**- Bu senaryoda, kaynak veya hedef bir güvenlik duvarı ardında veya içinde bir şirket içi şirket ağı veya veri deposu özel bir ağda / sanal ağ (çoğunlukla kaynağı) ve genel olarak erişilebilir değil. Sanal makineler üzerinde barındırılan veritabanı sunucularını da bu senaryoya ayrılır.

## <a name="cloud-scenarios"></a>Bulut senaryoları
### <a name="securing-data-store-credentials"></a>Veri deposu kimlik güvenliğini sağlama
Azure Data Factory ile veri deposu kimlik bilgilerinizi korur **şifreleme** kullanarak bunları **Microsoft tarafından yönetilen sertifikaları**. Bu sertifikalar Döndürülmüş her **iki yıllık** (içeren sertifikanın yenilenmesini ve kimlik bilgilerini geçişini). Bu şifrelenmiş kimlik bilgileri güvenli bir şekilde depolanır bir **Azure Storage, Azure Data Factory Yönetim Hizmetleri tarafından yönetilen**. Azure Storage güvenliği hakkında daha fazla bilgi için bkz [Azure depolama güvenliğine genel bakış](../../security/security-storage-overview.md).

### <a name="data-encryption-in-transit"></a>Aktarımdaki verileri şifreleme
Bulut veri deposu HTTPS veya TLS destekliyorsa, tüm veri aktarımlarını veri fabrikasında veri taşıma hizmetleri arasında ve bir bulut veri deposu olan güvenli kanal HTTPS veya TLS.

> [!NOTE]
> Tüm bağlantıları **Azure SQL veritabanı** ve **Azure SQL Data Warehouse** veri esnasında her zaman şifreleme (SSL/TLS) için ve veritabanından gerektirir. Bir JSON Düzenleyicisi'ni kullanarak bir işlem hattı yazarken ekleme **şifreleme** özelliği ve ayarlamak **true** içinde **bağlantı dizesi**. Kullandığınızda [Kopyalama Sihirbazı'nı](data-factory-azure-copy-wizard.md), sihirbazın, bu özellik varsayılan olarak ayarlanır. İçin **Azure Storage**, kullanabileceğiniz **HTTPS** bağlantı dizesinde.

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi
Rest verileri destek şifrelenmesi bazı verileri depolar. Bu veri depoları için veri şifreleme mekanizması etkinleştirmenizi öneririz. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Azure SQL Data warehouse'da saydam veri şifreleme (TDE) kötü amaçlı etkinliği tehdide karşı gerçek zamanlı şifreleme ve şifre çözme REST verilerinizin gerçekleştirerek ile korumaya yardımcı olur. Bu davranış, istemci için saydamdır. Daha fazla bilgi için bkz: [SQL veri ambarı veritabanında güvenli](../../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL veritabanı, gerçek zamanlı şifreleme ve şifre çözme veri uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirerek kötü amaçlı etkinliği tehdide karşı koruma ile yardımcı olan, saydam veri şifreleme (TDE) da destekler. Bu davranış, istemci için saydamdır. Daha fazla bilgi için bkz: [saydam veri şifrelemesi ile Azure SQL veritabanı](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store ayrıca hesapta depolanan veriler için şifreleme sağlar. Etkinleştirildiğinde, Data Lake deposu otomatik olarak devam ettirmeden önce verileri şifreler ve alma, veri erişen istemci saydam hale önce şifresini çözer. Daha fazla bilgi için bkz: [Azure Data Lake Store'da güvenlik](../../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Depolama ve Azure tablo depolaması
Azure Blob Depolama ve Azure Table storage depolama hizmeti şifreleme (otomatik olarak depolama birimine devam ettirmeden önce verilerinizi şifreler ve alma önce şifresini çözer SSE), destekler. Daha fazla bilgi için bkz: [bekleyen veri için Azure depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 REST verilerin istemci ve sunucu şifrelenmesini destekler. Daha fazla bilgi için bkz: [koruma verileri kullanarak şifreleme](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Şu anda, veri fabrikası sanal özel bulut (VPC) içinde Amazon S3 desteklemiyor.

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift küme şifreleme bekleyen veri için destekler. Daha fazla bilgi için bkz: [Amazon Redshift veritabanı şifreleme](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Şu anda, veri fabrikası Amazon Redshift bir VPC içinde desteklemiyor. 

#### <a name="salesforce"></a>Salesforce
Salesforce Shield Platform şifreleme'de, tüm dosyaları ekler, özel alanlar şifrelemeye izin destekler. Daha fazla bilgi için bkz: [Web sunucusu OAuth kimlik doğrulama akışı anlama](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Karma senaryolar (veri yönetimi ağ geçidi kullanarak)
Karma senaryolar veri yönetimi ağ geçidi bir şirket ağında veya bir sanal ağ (Azure) veya bir sanal özel bulut (Amazon) içinde yüklü olmasını gerektirir. Ağ geçidi yerel veri depolarına erişebilmeleri gerekir. Ağ geçidi hakkında daha fazla bilgi için bkz: [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md). 

![Veri Yönetimi ağ geçidi kanalları](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

**Komut kanalı** veri fabrikasında veri taşıma hizmetleri ve veri yönetimi ağ geçidi arasında iletişime olanak sağlar. İletişim faaliyete ilgili bilgiler içerir. Veri kanalı, şirket içi veri depoları ve bulut veri depoları arasında veri aktarımı için kullanılır.    

### <a name="on-premises-data-store-credentials"></a>Şirket içi veri deposu kimlik
Şirket içi veri depoları için kimlik bilgileri yerel olarak depolanır (bulutta olmayan). Üç farklı yolla ayarlanabilir. 

- Kullanarak **düz metin** (daha az güvenli) Azure portalından HTTPS üzerinden / Kopyalama Sihirbazı. Kimlik bilgilerini şirket içi ağ geçidi için düz metin olarak geçirilir.
- Kullanarak **JavaScript şifreleme Kopyalama Sihirbazı'nı kitaplıktan**.
- Kullanarak **tıklatın-kimlik bilgileri Yöneticisi uygulamasının temel sonra**. Tıklatın-sonra uygulama ağ geçidi erişimi ve veri deposu için kimlik bilgilerini ayarlar şirket içi makinede yürütür. Bu seçenek ve bir sonraki en güvenli seçeneklerdir. Kimlik bilgisi Yöneticisi uygulamasının varsayılan olarak, güvenli iletişim için ağ geçidi ile makinede 8050 bağlantı noktasını kullanır.  
- Kullanım [yeni AzureRmDataFactoryEncryptValue](/powershell/module/azurerm.datafactories/New-AzureRmDataFactoryEncryptValue) kimlik bilgilerini şifrelemek için PowerShell cmdlet. Cmdlet sertifikayı kullanan bu ağ geçidi kimlik bilgilerini şifrelemek için kullanmak üzere yapılandırılmış. Bu cmdlet tarafından döndürülen şifreli kimlik bilgilerini kullanın ve ekleyin **EncryptedCredential** öğesinin **connectionString** ile kullandığınız JSON dosyasında [ AzureRmDataFactoryLinkedService yeni](/powershell/module/azurerm.datafactories/new-azurermdatafactorylinkedservice) cmdlet'ini veya JSON parçacığı Portal'daki Data Factory Düzenleyici. Bu seçenek ve tıklatın-uygulama olduktan sonra en güvenli seçenekleri. 

#### <a name="javascript-cryptography-library-based-encryption"></a>JavaScript şifreleme kitaplığı tabanlı şifreleme
Veri deposu kimlik bilgilerini kullanarak şifreleyebilirsiniz [JavaScript şifreleme Kitaplığı](https://www.microsoft.com/download/details.aspx?id=52439) gelen [Kopyalama Sihirbazı'nı](data-factory-copy-wizard.md). Bu seçeneği belirlediğinizde, kopyalama Sihirbazı'nı ağ geçidi ortak anahtarını alır ve veri deposu kimlik bilgilerini şifrelemek için kullanır. Kimlik bilgileri ağ geçidi makinesi tarafından şifresi çözülen ve Windows tarafından korunan [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx).

**Desteklenen tarayıcılar:** IE8, IE9, ıe10, IE11, Microsoft Edge ve en son Firefox, Chrome, Opera, Safari tarayıcı. 

#### <a name="click-once-credentials-manager-app"></a>Tıklatın-bir kez kimlik bilgileri Yöneticisi uygulama
Öğesini başlatın-ardışık düzen yazarken kimlik bilgisi Yöneticisi uygulamasından Azure portal/Kopyalama Sihirbazı'nı temel sonra. Bu uygulama kimlik bilgileri kablo üzerinden düz metin olarak aktarılmayan sağlar. Varsayılan olarak, bağlantı noktasını kullanır **8050** güvenli iletişim için ağ geçidi ile makinede. Gerekirse, bu bağlantı noktası değiştirilebilir.  
  
![Ağ geçidi için HTTPS bağlantı noktası](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Şu anda veri yönetimi ağ geçidi tek bir kullanır **sertifika**. Bu sertifika, ağ geçidi yükleme işlemi sırasında oluşturulan (veri yönetimi ağ geçidi Kasım 2016 sonrasında oluşturulan ve sürüm 2.4.xxxx.x uygulanır veya üstü). Bu sertifika, kendi SSL/TLS sertifika ile değiştirebilir. Bu sertifika tıklatın tarafından kullanılan-kez Yöneticisi uygulaması veri deposu kimlik bilgilerini ayarlamak için ağ geçidi makinesi güvenli bir şekilde bağlanmak için kimlik bilgisi. Verilerini depolayan depolama kimlik bilgileri güvenli bir şekilde şirket içi Windows kullanarak [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) makinede ağ geçidi ile. 

> [!NOTE]
> Kasım 2016 önce veya sürüm 2.3.xxxx.x yüklenmiş olan eski ağ geçitleri şifrelenir ve bulutta depolanan kimlik bilgileri kullanmaya devam edin. Ağ geçidi son sürümüne yükseltme olsa bile, kimlik bilgileri bir şirket içi makineye geçirilmez    
  
| Ağ geçidi sürümü (sırasında oluşturma) | Depolanan kimlik bilgileri | Kimlik bilgisi şifreleme / güvenlik | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | Bulutta | Sertifika (kimlik bilgisi Yöneticisi uygulama tarafından kullanılan olandan farklı) kullanılarak şifrelenir | 
| > = 2.4.xxxx.x | Şirket içinde | DPAPI güvenli | 
  

### <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
Tüm veri aktarımlarını güvenli kanal olan **HTTPS** ve **TLS üzerinden TCP** Azure Hizmetleri ile iletişim sırasında man-in--middle saldırılarını önlemek için.
 
Aynı zamanda [IPSec VPN](../../vpn-gateway/vpn-gateway-about-vpn-devices.md) veya [hızlı rota](../../expressroute/expressroute-introduction.md) daha fazla şirket içi ağınız ile Azure arasındaki iletişim kanalını güvenli hale getirmek için.

Sanal ağ, bulut ağınızdaki mantıksal bir gösterimidir. IPSec VPN (siteden siteye) veya hızlı rota (özel eşleme) ayarlayarak, Azure sanal ağı (VNet) için bir şirket içi ağ bağlanabilir     

Aşağıdaki tabloda, karma veri taşıma için kaynak ve hedef konumların farklı birleşimlerine göre ağ ve ağ geçidi yapılandırması önerileri özetler.

| Kaynak | Hedef | Ağ yapılandırması | Ağ geçidi Kurulumu |
| ------ | ----------- | --------------------- | ------------- | 
| Şirket içi | Sanal makineler ve sanal ağlarda dağıtılan bulut Hizmetleri | IPSec VPN (noktadan siteye veya siteden siteye) | Ağ geçidi olabilir ya da şirket içi yüklü veya bir Azure sanal üzerinde (VM) içinde sanal makine | 
| Şirket içi | Sanal makineler ve sanal ağlarda dağıtılan bulut Hizmetleri | ExpressRoute (özel eşleme) | Ağ geçidi olabilir ya da şirket içi yüklü veya bir Azure VM VNet içinde | 
| Şirket içi | Genel bir uç nokta sahip azure tabanlı Hizmetleri | ExpressRoute (ortak eşleme) | Ağ geçidi içi yüklü olması gerekir | 

Aşağıdaki görüntüleri, şirket içi veritabanı ile hızlı rota ve IPSec VPN (ile sanal ağ) kullanarak Azure hizmetleri arasında veri taşımak için veri yönetimi ağ geçidi kullanımını göster:

**Hızlı rota:**
 
![Expressroute ağ geçidi ile kullanma](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![IPSec VPN ağ geçidi ile](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Güvenlik duvarı yapılandırmaları ve ağ geçidi uygulamaları güvenilir listeye almayı IP adresi

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Şirket içi/özel ağ için güvenlik duvarı gereksinimleri  
Bir kuruluşta bir **Kurumsal Güvenlik Duvarı** kuruluşun merkezi yönlendirici üzerinde çalışır. Ve **Windows Güvenlik Duvarı** ağ geçidinin yüklü yerel makine üzerinde bir arka plan programı gibi çalışır. 

Aşağıdaki tabloda verilmiştir **giden bağlantı noktası** ve etki alanı gereksinimleri için **Kurumsal Güvenlik Duvarı**.

| Etki alanı adları | Giden bağlantı noktaları | Açıklama |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Ağ Geçidi tarafından veri fabrikasında veri taşıma hizmetleri bağlanmak için gereken |
| `*.core.windows.net` | 443 | Ağ Geçidi tarafından kullandığınızda Azure depolama hesabına bağlanmak için kullanılan [kopyalama hazırlanan](data-factory-copy-activity-performance.md#staged-copy) özelliği. | 
| `*.frontend.clouddatahub.net` | 443 | Azure Data Factory hizmetine bağlanmak için ağ geçidi tarafından gerekli. | 
| `*.database.windows.net` | 1433   | (İsteğe bağlı) gerekli Hedefinizi Azure SQL veritabanı olduğunda / Azure SQL veri ambarı. Bağlantı noktası 1433 açmadan Azure SQL veritabanı/Azure SQL Data Warehouse için verileri kopyalamak için hazırlanmış kopyalama özelliğini kullanın. | 
| `*.azuredatalakestore.net` | 443 | (İsteğe bağlı), hedef Azure Data Lake deposu olduğunda gerekli | 

> [!NOTE] 
> Bağlantı noktaları yönetmek zorunda kalabilirsiniz / uygulamaları güvenilir listeye almayı etki alanları Kurumsal güvenlik duvarında düzey gerektiği gibi ilgili veri kaynakları tarafından. Bu tablo yalnızca Azure SQL Database, Azure SQL Data Warehouse, Azure Data Lake Store örnek olarak kullanır.   

Aşağıdaki tabloda verilmiştir **gelen bağlantı noktası** gereksinimleri **windows güvenlik duvarı**.

| Gelen bağlantı noktaları | Açıklama | 
| ------------- | ----------- | 
| 8050 (TCP) | Ağ geçidi şirket içi veri depoları için kimlik bilgilerini güvenli bir şekilde ayarlamak için kimlik bilgisi Yöneticisi uygulama tarafından gerekli. | 

![Ağ geçidi bağlantı noktası gereksinimleri](media\data-factory-data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-whitelisting-in-data-store"></a>IP yapılandırmaları / uygulamaları güvenilir listeye almayı veri deposu
Bazı veri depolarına bulutta ayrıca erişmesini makinenin IP adresinin uygulamaları güvenilir listeye almayı gerektirir. Ağ geçidi makinenin IP adresini Güvenilenler listesine / Güvenlik Duvarı'nda uygun şekilde yapılandırılmış emin olun.

Aşağıdaki bulut veri depoları ağ geçidi makinenin IP adresinin uygulamaları güvenilir listeye almayı gerektirir. Varsayılan olarak, bu veri depolarına bazıları uygulamaları güvenilir listeye almayı IP adresinin gerektirmeyebilir. 

- [Azure SQL Veritabanı](../../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Veri Ambarı](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Soru:** ağ geçidi farklı veri fabrikaları arasında paylaşılabilir?
**Yanıt:** bu özellik bir henüz desteklemiyoruz. Etkin olarak üzerinde çalışıyoruz.

**Soru:** çalışmak ağ geçidi için bağlantı noktası gereksinimleri nelerdir?
**Yanıt:** ağ geçidi Internet açmak için HTTP tabanlı bağlantılar sağlar. **Giden bağlantı noktası 443 ve 80** bu bağlantıyı kurmak ağ geçidi için açılması gerekir. Açık **gelen bağlantı noktası 8050** yalnızca makine düzeyinde (düzeyinde Kurumsal güvenlik duvarı) için kimlik bilgisi Yöneticisi uygulaması. Azure SQL Database veya Azure SQL Data Warehouse kullanılıyorsa olarak kaynak / hedef, ardından açmanız **1433** de bağlantı noktası. Daha fazla bilgi için bkz: [güvenlik duvarı yapılandırmaları ve uygulamaları güvenilir listeye almayı IP adreslerini](#firewall-configurations-and-whitelisting-ip-address-of gateway) bölümü. 

**Soru:** ağ geçidi için sertifika gereksinimleri nelerdir?
**Yanıt:** geçerli ağ geçidi veri deposu kimlik güvenli bir şekilde ayarlamak için kimlik bilgisi Yöneticisi uygulama tarafından kullanılan bir sertifika gerektirir. Bu sertifikayı oluşturulur ve ağ geçidi kurulum tarafından yapılandırılan otomatik olarak imzalanan bir sertifikadır. Kendi TLS kullanabilir / SSL sertifika yerine. Daha fazla bilgi için bkz: [tıklatın-manager uygulamasını bir kez kimlik bilgisi](#click-once-credentials-manager-app) bölümü. 

## <a name="next-steps"></a>Sonraki adımlar
Kopya etkinliği performansının hakkında daha fazla bilgi için bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

 
