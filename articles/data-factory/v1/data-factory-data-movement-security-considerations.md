---
title: Azure Data factory'de veri taşımayı için güvenlik konuları | Microsoft Docs
description: Azure Data factory'de veri taşımayı güvenliğini sağlama hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 083770c24a6c8939f8d1ff9f0efd5d18aff9dcb0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60487077"
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - veri taşıma için güvenlik konuları

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data Factory veri taşıma güvenlik konuları](../data-movement-security-considerations.md).

## <a name="introduction"></a>Giriş
Bu makalede, verilerinizin güvenliğini sağlamak için Azure Data factory'deki veri taşıma hizmetleri kullanan temel bir güvenlik altyapısı açıklanır. Azure Data Factory yönetim kaynakları, Azure güvenlik altyapıyla oluşturulmuş ve Azure tarafından sunulan tüm olası güvenlik önlemleri kullanın.

Bir Data Factory çözümünde bir veya daha fazla [işlem hattı](data-factory-create-pipelines.md) oluşturursunuz. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bu komut zincirleri, data factory oluşturulduğu bölgede yer alır. 

Data Factory yalnızca kullanılabilir olsa bile **Batı ABD**, **Doğu ABD**, ve **Kuzey Avrupa** bölgeler, veri taşıma Hizmeti'nde kullanılabilir [içinde genel olarak çeşitli bölgeleri](data-factory-data-movement-activities.md#global). Data Factory hizmeti, verileri bir coğrafi bölgeye çıkmamasını sağlar / bölge sürece veri taşıma Hizmeti'nde bu bölgeye henüz dağıtılmamışsa, alternatif bir bölge kullanmaya hizmet açıkça söyleyin. 

Azure Data Factory'nin kendisi, sertifikalar kullanılarak şifrelenmiş bulut veri depoları için bağlı hizmet kimlik bilgilerini dışında herhangi bir veri depolamaz. Veri hareketini [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](data-factory-compute-linked-services.md) kullanarak düzenlemek için veri temelinde iş akışları oluşturmanızı sağlar. Hem programlama, hem de kullanıcı arabirimi mekanizmalarını kullanarak [iş akışlarını izlemenizi ve yönetmenizi](data-factory-monitor-manage-pipelines.md) de sağlar.

Azure Data Factory ile veri taşıma oluştu **sertifikalı** için:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA STAR](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Azure uyumluluk ve Azure'nın kendi altyapısını nasıl korur ilgileniyorsanız, ziyaret [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/default.aspx). 

Bu makalede, biz aşağıdaki iki veri taşıma senaryolarda güvenlik konuları gözden geçirin: 

- **Bulut senaryosu**-hem kaynak hem de hedef Bu senaryoda, internet üzerinden genel olarak erişilebilir. Bunlar, Azure depolama, Azure SQL veri ambarı, Azure SQL veritabanı, Azure Data Lake Store, Amazon S3, Amazon Redshift, Salesforce gibi SaaS hizmetlerine ve FTP ve OData gibi web protokolleri gibi yönetilen bulut depolama hizmetleri içerir. Desteklenen veri kaynaklarının tam listesini bulabilirsiniz [burada](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Karma senaryo**- Bu senaryoda, kaynak veya hedef bir güvenlik duvarı ardında kaldığı veya içinde bir şirket içi kurumsal ağ veya veri deposu olan özel bir ağda / sanal ağ (genellikle kaynak) ve genel olarak erişilebilir değil. Sanal makinelerde barındırılan veritabanı sunucuları, ayrıca bu senaryoya ayrılır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="cloud-scenarios"></a>Bulut senaryoları
### <a name="securing-data-store-credentials"></a>Güvenliğini sağlama veri deposu kimlik bilgileri
Azure Data Factory ile veri deposu kimlik bilgilerinizi korur **şifreleme** kullanarak bunları **Microsoft tarafından yönetilen sertifikaları**. Bu sertifikalar Döndürülmüş her **iki yıl** (içeren sertifikanın yenilenmesini ve geçiş kimlik bilgileri). Bu şifrelenmiş kimlik bilgileri güvenli bir şekilde depolanan bir **Azure depolama, Azure Data Factory Yönetim Hizmetleri tarafından yönetilen**. Azure depolama güvenliği hakkında daha fazla bilgi için [Azure depolama güvenliğine genel bakış](../../security/security-storage-overview.md).

### <a name="data-encryption-in-transit"></a>Aktarımdaki verileri şifreleme
Bulut veri deposu, HTTPS veya TLS destekliyorsa, Data factory'deki veri taşıma hizmetleri arasında tüm veri aktarımı ve bulut veri deposu olan güvenli kanal HTTPS veya TLS.

> [!NOTE]
> Tüm bağlantıları **Azure SQL veritabanı** ve **Azure SQL veri ambarı** veri esnasında her zaman şifreleme (SSL/TLS) için ve veritabanından gerektirir. JSON düzenleyicisini kullanarak bir işlem hattı yazma sırasında Ekle **şifreleme** özelliği ve değerini **true** içinde **bağlantı dizesi**. Kullanırken [Kopyalama Sihirbazı'nı](data-factory-azure-copy-wizard.md), sihirbaz bu özellik varsayılan olarak ayarlar. İçin **Azure depolama**, kullanabileceğiniz **HTTPS** bağlantı dizesindeki.

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi
Bekleyen verilerin şifrelenmesi destek bazı veriler depolanır. Bu veri depoları için veri şifreleme mekanizması etkinleştirmenizi öneririz. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Azure SQL veri ambarı'nda saydam veri şifrelemesi (TDE), gerçek zamanlı şifreleme ve şifre çözme, bekleyen veri gerçekleştirerek, kötü amaçlı etkinlik tehdide karşı koruma ile yardımcı olur. Bu davranış, istemci için saydamdır. Daha fazla bilgi için [güvenli bir veritabanında SQL veri ambarı](../../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Veritabanı
Azure SQL veritabanı ayrıca kesilmeyen kötü amaçlı etkinliklere karşı gerçek zamanlı şifreleme ve şifre çözme verileri uygulamada değişiklik gerektirmeden gerçekleştirerek ile korumaya yardımcı olan, saydam veri şifrelemesi (TDE) destekler. Bu davranış, istemci için saydamdır. Daha fazla bilgi için [Azure SQL veritabanı ile saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store ayrıca hesapta depolanan veriler için şifreleme sağlar. Etkin olduğunda, Data Lake store, otomatik olarak devam ettirmeden önce verileri şifreler ve alma, veri erişimi istemciye saydam hale önce şifresini çözer. Daha fazla bilgi için [Azure Data Lake Store güvenlik](../../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Depolama ve Azure tablo depolama
Azure Blob Depolama ve Azure tablo depolama, depolama hizmeti şifrelemesi (otomatik olarak kalıcı depolama için önce verilerinizi şifreler ve şifresini çözer alma önce SSE), destekler. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](../../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3, bekleyen verilerin istemci ve sunucu şifreleme destekler. Daha fazla bilgi için [veri şifreleme kullanarak koruma](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Şu anda, Data Factory, bir sanal özel bulut (VPC) içinde Amazon S3 desteklemez.

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift, bekleyen veriler için küme şifrelemesini destekler. Daha fazla bilgi için [Amazon Redshift veritabanına şifreleme](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Şu anda, Data Factory içinde bir VPC Amazon Redshift desteklemez. 

#### <a name="salesforce"></a>Salesforce
Salesforce Shield Platform şifreleme tüm dosyaları, ekleri, özel alanlar veren şifrelemesini destekler. Daha fazla bilgi için [Web sunucusu OAuth kimlik doğrulaması akışı anlama](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Karma senaryolar (veri yönetimi ağ geçidi kullanarak)
Karma senaryolar veri yönetimi ağ geçidi ile şirket içi ağ ya da bir sanal ağ (Azure) veya bir sanal özel bulut (Amazon) içinde yüklü olması gerekir. Ağ geçidi, yerel veri depolarını erişebilir olmalıdır. Ağ geçidi hakkında daha fazla bilgi için bkz. [veri yönetimi ağ geçidi](data-factory-data-management-gateway.md). 

![Veri Yönetimi ağ geçidi kanallar](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

**Komut kanalı** Data factory'deki veri taşıma hizmetleri ve veri yönetimi ağ geçidi arasında iletişime olanak sağlar. İletişim için etkinlik ilgili bilgiler içerir. Veri kanalı, şirket içi veri depoları ile bulut veri depoları arasında veri aktarmak için kullanılır.    

### <a name="on-premises-data-store-credentials"></a>Şirket içi veri deposu kimlik bilgileri
Kimlik bilgileri, şirket içi veri depoları için yerel olarak depolanır (bulutta değil). Üç farklı yolla ayarlanabilir. 

- Kullanarak **düz metin** (daha az güvenli) Azure portalından HTTPS üzerinden / Kopyalama Sihirbazı'nı. Kimlik bilgilerini düz metin olarak şirket içi ağ geçidine geçirilir.
- Kullanarak **JavaScript şifreleme Kopyalama Sihirbazı'nı kitaplıktan**.
- Kullanarak **tıklayın-kimlik bilgileri Yöneticisi uygulama tabanlı bir kez**. Tıklayarak-sonra uygulama ağ geçidine erişimi olan ve veri deposu için kimlik bilgilerini ayarlar şirket içi makinede yürütür. Bu seçenek ve bir sonraki en güvenli seçenekleri var. Kimlik bilgileri Yöneticisi uygulaması, varsayılan olarak, güvenli iletişim için ağ geçidi makinesine 8050 bağlantı noktasını kullanır.  
- Kullanım [yeni AzDataFactoryEncryptValue](/powershell/module/az.datafactory/New-azDataFactoryEncryptValue) kimlik bilgilerini şifrelemek için PowerShell cmdlet'i. Cmdlet sertifikayı kullanan kimlik bilgilerini şifrelemek üzere kullanmak için bu ağ geçidi yapılandırılmış. Bu cmdlet'i tarafından döndürülen şifreli kimlik bilgilerini kullanın ve eklemeniz **EncryptedCredential** öğesinin **connectionString** ile kullandığınız JSON dosyasındaki [ Yeni AzDataFactoryLinkedService](/powershell/module/az.datafactory/new-azdatafactorylinkedservice) cmdlet'ini veya JSON kod parçacığında Portal'daki Data Factory Düzenleyici. Bu seçenek ve tıklayarak-uygulama sonra en güvenli seçenekleri. 

#### <a name="javascript-cryptography-library-based-encryption"></a>JavaScript şifreleme kitaplık tabanlı şifreleme
Veri deposu kimlik bilgilerini kullanarak şifreleyebilirsiniz [JavaScript şifreleme Kitaplığı](https://www.microsoft.com/download/details.aspx?id=52439) gelen [Kopyalama Sihirbazı'nı](data-factory-copy-wizard.md). Bu seçeneği belirlediğinizde, kopyalama Sihirbazı'nı ağ geçidi ortak anahtarını alır ve veri deposu kimlik bilgilerini şifrelemek için kullanır. Kimlik bilgilerini ağ geçidi makinesi tarafından şifresi çözülür ve Windows tarafından korunan [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx).

**Desteklenen tarayıcılar:** IE8, IE9, ıe10, ıe11, Microsoft Edge ve en son Firefox, Chrome, Opera, Safari tarayıcı. 

#### <a name="click-once-credentials-manager-app"></a>Tıklayın-bir kez kimlik bilgileri Yöneticisi uygulaması
Tıklayarak başlatabilirsiniz-işlem hatları yazma kimlik bilgisi Yöneticisi Azure portalı/Kopyalama Sihirbazı'nı uygulamadan alarak sonra. Bu uygulama, kimlik bilgilerini düz metin olarak kablolu aktarılmıyor sağlar. Varsayılan olarak, bağlantı noktasını kullanır **8050** güvenli iletişim için ağ geçidi makinesine. Gerekirse, bu bağlantı noktası değiştirilebilir.  
  
![Ağ geçidi için HTTPS bağlantı noktası](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Şu anda tek bir veri yönetimi ağ geçidi kullanan **sertifika**. Bu sertifika, ağ geçidi yüklemesi sırasında oluşturulur (veri yönetimi ağ geçidi Kasım 2016'dan sonra oluşturulan ve sürüm 2.4.xxxx.x uygulanır veya üzeri). Bu sertifika, kendi SSL/TLS sertifikası ile değiştirebilirsiniz. Bu sertifikayı tıklatın tarafından kullanılan-bir kez Yöneticisi uygulama ağ geçidi makinesi, veri deposu kimlik bilgilerini ayarlamak için güvenli bir şekilde bağlanmak için kimlik bilgisi. Verileri depolayan depolama kimlik bilgileri güvenli bir şekilde şirket içi Windows kullanarak [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) ağ geçidi makinesine. 

> [!NOTE]
> Kasım 2016 tarihinden önce veya sürüm 2.3.xxxx.x yüklenmiş olan eski ağ geçidi şifrelenir ve bulut üzerinde depolanan kimlik bilgileri kullanmaya devam edin. Ağ geçidinin en son sürüme yükseltme bile bir şirket içi makine için kimlik bilgileri geçirilmez    
  
| Ağ geçidi sürümü (oluşturulurken) | Depolanan kimlik bilgileri | Kimlik bilgisi şifreleme / güvenlik | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | Bulutta | Sertifika (kimlik bilgileri Yöneticisi uygulaması tarafından kullanılan hesaptan farklı) kullanılarak şifrelenmiş | 
| > = 2.4.xxxx.x | Şirket içi | DPAPI ile güvenli | 
  

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Tüm veri aktarımları, güvenli kanal yoluyla olan **HTTPS** ve **TLS üzerinden TCP** Azure Hizmetleri ile iletişim sırasında adam-de-adam saldırıları önlemek için.
 
Ayrıca [IPSec VPN](../../vpn-gateway/vpn-gateway-about-vpn-devices.md) veya [Express Route](../../expressroute/expressroute-introduction.md) daha fazla şirket içi ağınız ile Azure arasındaki iletişim kanalını güvenli hale getirmek için.

Sanal ağ, buluttaki ağınızın mantıksal bir gösterimidir. (Siteden siteye) IPSec VPN veya Express Route (özel eşdüzey hizmet sağlama) ayarlayarak, Azure sanal ağı (VNet) için bir şirket içi ağa bağlanabilir     

Aşağıdaki tablo, karma veri taşıma için kaynak ve hedef konumların farklı birleşimlerini temel ağ ve ağ geçidi yapılandırması önerileri özetler.

| source | Hedef | Ağ yapılandırması | Ağ geçidi |
| ------ | ----------- | --------------------- | ------------- | 
| Şirket içi | Sanal makineler ve sanal ağlara dağıtılan bulut Hizmetleri | IPSec VPN (noktadan siteye veya siteden siteye) | Ağ geçidi olabilir ya da şirket içi yüklediyseniz veya bir Azure üzerinde sanal sanal ağda (VM) makine | 
| Şirket içi | Sanal makineler ve sanal ağlara dağıtılan bulut Hizmetleri | ExpressRoute (özel eşdüzey hizmet sağlama) | Ağ geçidi olabilir ya da şirket içi yüklü veya Azure VM'deki sanal ağ | 
| Şirket içi | Genel bir uç nokta içeren azure tabanlı Hizmetleri | ExpressRoute (ortak eşleme) | Ağ geçidi şirket yüklü olması gerekir | 

Aşağıdaki resimlerde, bir şirket içi veritabanı ve Express route ile IPSec VPN (sanal ağ için ile) Azure Hizmetleri arasındaki veri taşıma için veri yönetimi ağ geçidi kullanımı göster:

**Express route:**
 
![Express Route ağ geçidi ile kullanma](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![IPSec VPN ağ geçidi ile](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Güvenlik duvarı yapılandırmaları ve ağ geçidinin IP adresi izin verilenler listesine ekleme

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Şirket içi/özel ağ için güvenlik duvarı gereksinimleri  
Bir kuruluşta bir **Kurumsal güvenlik duvarınız** kuruluşun merkezi yönlendiricisi üzerinde çalışır. Ve **Windows Güvenlik Duvarı** ağ geçidinin yüklü olduğu yerel makinede bir arka plan programı gibi çalışır. 

Aşağıdaki tabloda verilmiştir **giden bağlantı noktası** ve etki alanı gereksinimleri **Kurumsal güvenlik duvarınız**.

| Etki alanı adları | Giden bağlantı noktaları | Açıklama |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Ağ Geçidi tarafından Data factory'deki veri taşıma hizmetleri bağlanmak için gereken |
| `*.core.windows.net` | 443 | Kullandığınızda, Azure depolama hesabına bağlanmak için ağ geçidi tarafından kullanılan [kopyalama aşamalı](data-factory-copy-activity-performance.md#staged-copy) özelliği. | 
| `*.frontend.clouddatahub.net` | 443 | Azure Data Factory hizmetine bağlanmak için ağ geçidi tarafından gerekli. | 
| `*.database.windows.net` | 1433   | (İsteğe bağlı) gerekli hedef Azure SQL veritabanı olduğunda / Azure SQL veri ambarı. 1433 numaralı bağlantı noktasını açmaya gerek kalmadan Azure SQL veritabanı/Azure SQL veri ambarı'na veri kopyalamak için hazırlanmış kopya özelliğini kullanın. | 
| `*.azuredatalakestore.net` | 443 | (İsteğe bağlı) Azure Data Lake store, hedef olduğunda gerekli | 

> [!NOTE] 
> Bağlantı noktalarını Yönet gerekebilir / şirket güvenlik duvarında beyaz listeye ekleme etki düzeyi gerektiği şekilde ilgili veri kaynakları tarafından. Bu tablo yalnızca örnek olarak Azure SQL veritabanı, Azure SQL veri ambarı, Azure Data Lake Store kullanır.   

Aşağıdaki tabloda verilmiştir **gelen bağlantı noktası** gereksinimleri **windows güvenlik duvarı**.

| Gelen bağlantı noktaları | Açıklama | 
| ------------- | ----------- | 
| 8050 (TCP) | Güvenli ağ geçidi üzerinde şirket içi veri depoları için kimlik bilgilerini ayarlamak için kimlik bilgileri Yöneticisi uygulamasını gerekli. | 

![Ağ geçidi bağlantı noktası gereksinimleri](media/data-factory-data-movement-security-considerations/gateway-port-requirements.png)

#### <a name="ip-configurations-whitelisting-in-data-store"></a>IP yapılandırması / beyaz listeye ekleme veri depolama
Bulut veri depoları, ayrıca beyaz listeye ekleme erişmesini makinenin IP adresi gerektirir. Ağ geçidi makinesinin IP adresi izin verilenler listesinde veya Güvenlik Duvarı'nda uygun şekilde yapılandırılmış emin olun.

Beyaz listeye ekleme IP adresinin ağ geçidi makinesinde aşağıdaki bulut veri depoları gerektirir. Varsayılan olarak, bu veri depoları bazı beyaz listeye ekleme IP adresinin gerektirmeyebilir. 

- [Azure SQL Veritabanı](../../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Veri Ambarı](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../../cosmos-db/firewall-support.md)
- [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Soru:** Ağ geçidi, farklı veri fabrikaları arasında paylaşılabilir?
**Yanıt:** Bu özellik henüz desteklemiyoruz. Üzerinde çalışmaya devam ediyoruz.

**Soru:** Ağ geçidinin çalışması bağlantı noktası gereksinimleri nelerdir?
**Yanıt:** Ağ geçidi, internet'i açmak için HTTP tabanlı bağlantılar oluşturur. **Giden bağlantı noktası 443 ve 80** ağ geçidi için bu bağlantıyı açık olması gerekir. Açık **gelen bağlantı noktası 8050** yalnızca makine düzeyinde (düzeyinde Kurumsal güvenlik duvarı) için kimlik bilgileri Yöneticisi uygulaması. Azure SQL veritabanı veya Azure SQL veri ambarı kullanılıyorsa farklı kaynak / hedef, sonra gerek açmak **1433** de bağlantı noktası. Daha fazla bilgi için [güvenlik duvarı yapılandırmaları ve IP adreslerini beyaz listeye ekleme](#firewall-configurations-and-whitelisting-ip-address-of gateway) bölümü. 

**Soru:** Ağ geçidi için sertifika gereksinimleri nelerdir?
**Yanıt:** Geçerli bir ağ geçidi, veri deposu kimlik bilgilerini güvenli bir şekilde ayarlamak için kimlik bilgileri Yöneticisi uygulaması tarafından kullanılan bir sertifika gerektirir. Bu sertifika oluşturulur ve ağ geçidi yapılandırılmış otomatik olarak imzalanan bir sertifikadır. Kullanabileceğiniz kendi TLS / SSL sertifikası yerine. Daha fazla bilgi için bkz. [tıklayın-bir kez Yöneticisi uygulaması kimlik bilgisi](#click-once-credentials-manager-app) bölümü. 

## <a name="next-steps"></a>Sonraki adımlar
Kopyalama etkinliği performansı hakkında daha fazla bilgi için bkz. [kopyalama etkinliği performansı ve ayarlama Kılavuzu](data-factory-copy-activity-performance.md).

 
