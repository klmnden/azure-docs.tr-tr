---
title: "Azure veri fabrikası'nda güvenlik değerlendirmeleri | Microsoft Docs"
description: "Verilerinizi güvenli hale getirmek için Azure Data Factory veri taşıma hizmetleri kullanan temel güvenlik altyapısı açıklar."
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 8bd5ae2aac23b18aeb3ef44692f448b50b7e3d44
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - veri taşıma için güvenlik konuları
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-data-movement-security-considerations.md)
> * [Sürüm 2 - Önizleme](data-movement-security-considerations.md)

Bu makalede, verilerinizin güvenliğini sağlamak için Azure Data Factory veri taşıma hizmetleri kullanan temel güvenlik altyapısı açıklanmaktadır. Azure veri fabrikası yönetim kaynakları Azure güvenlik altyapı üzerine kurulmuş ve Azure tarafından sunulan tüm olası güvenlik önlemleri kullanın.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory sürüm 1 için veri taşıma güvenlik konuları](v1/data-factory-data-movement-security-considerations.md).

Bir Data Factory çözümünde bir veya daha fazla [işlem hattı](concepts-pipelines-activities.md) oluşturursunuz. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bu ardışık düzen veri fabrikası oluşturulduğu bölgede yer alır. 

Data Factory yalnızca kullanılabilir olsa bile **Doğu ABD**, **Doğu ABD 2**, ve **Batı Avrupa** bölgeler (sürüm 2 Önizleme), veri taşıma Hizmeti'nde kullanılabilir olduğu[genel birçok bölgede](concepts-integration-runtime.md#azure-ir). Veri Taşıma hizmeti bu bölgeye henüz dağıtılmamışsa, Data Factory hizmetinin veri coğrafi bölgeye bırakmaz sağlar / bölge alternatif bir bölge kullanın hizmete açıkça toplamasını sürece. 

Azure Data Factory'nin kendisi sertifikalar kullanılarak şifrelenmiş bulut veri depoları için bağlı hizmet kimlik bilgilerini dışında herhangi bir veriyi depolamaz. Veri hareketini [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) arasında, verilerin işlenmesini de başka bölgelerde veya şirket içi bir ortamda [işlem hizmetleri](compute-linked-services.md) kullanarak düzenlemek için veri temelinde iş akışları oluşturmanızı sağlar. Ayrıca, izlemek ve SDK'ları ve Azure İzleyicisi'ni kullanarak da iş akışları yönetmenize olanak sağlar.

Azure Data Factory kullanarak veri taşıma süredir **sertifikalı** için:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA STAR](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Azure uyumluluk ve Azure kendi altyapısını nasıl korur ilgileniyorsanız, ziyaret [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

Bu makalede, aşağıdaki iki veri taşıma senaryolarda güvenlik konuları inceleyin: 

- **Bulut senaryosu**-Bu senaryoda, hem kaynak hem de hedef Internet üzerinden genel olarak erişilebilir. Bunlar Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon Redshift Salesforce gibi SaaS Hizmetleri ve FTP ve OData gibi web protokoller gibi yönetilen bulut depolama hizmetlerine içerir. Desteklenen veri kaynaklarının tam listesi bulabilirsiniz [burada](copy-activity-overview.md#supported-data-stores-and-formats).
- **Karma senaryo**- Bu senaryoda, kaynak veya hedef bir güvenlik duvarı ardında veya içinde bir şirket içi şirket ağı veya veri deposu özel bir ağda / sanal ağ (çoğunlukla kaynağı) ve genel olarak erişilebilir değil. Sanal makineler üzerinde barındırılan veritabanı sunucularını da bu senaryoya ayrılır.

## <a name="cloud-scenarios"></a>Bulut senaryoları
###<a name="securing-data-store-credentials"></a>Veri deposu kimlik güvenliğini sağlama
- Şifrelenmiş kimlik bilgileri, Azure Data Factory yönetilen deposunda saklar.

   Azure Data Factory ile veri deposu kimlik bilgilerinizi korur **şifreleme** kullanarak bunları **Microsoft tarafından yönetilen sertifikaları**. Bu sertifikalar Döndürülmüş her **iki yıllık** (içeren sertifikanın yenilenmesini ve kimlik bilgilerini geçişini). Bu şifrelenmiş kimlik bilgileri güvenli bir şekilde depolanır bir **Azure Storage, Azure Data Factory Yönetim Hizmetleri tarafından yönetilen**. Azure Storage güvenliği hakkında daha fazla bilgi için bkz [Azure depolama güvenliğine genel bakış](../security/security-storage-overview.md).
- Kimlik bilgilerini Azure Key Vault’ta depolama 

   Şimdi veri deposunun kimlik bilgisi depolamayı seçebilirsiniz [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/), bir etkinlik yürütme sırasında almak için Azure Data Factory olanak tanır. Daha fazla bilgi için bkz: [Azure anahtar kasası kimlik bilgisi deposu](store-credentials-in-key-vault.md).

   > [!NOTE]
   > Şu anda yalnızca [Dynamics Bağlayıcısı](connector-dynamics-crm-office-365.md) bu özelliğini destekler. 

### <a name="data-encryption-in-transit"></a>Aktarımdaki verileri şifreleme
Bulut veri deposu HTTPS veya TLS destekliyorsa, tüm veri aktarımlarını veri fabrikasında veri taşıma hizmetleri arasında ve bir bulut veri deposu olan güvenli kanal HTTPS veya TLS.

> [!NOTE]
> Tüm bağlantıları **Azure SQL veritabanı** ve **Azure SQL Data Warehouse** veri esnasında her zaman şifreleme (SSL/TLS) için ve veritabanından gerektirir. JSON kullanarak bir işlem hattı yazarken ekleme **şifreleme** özelliği ve ayarlamak **true** içinde **bağlantı dizesi**. İçin **Azure Storage**, kullanabileceğiniz **HTTPS** bağlantı dizesinde.

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi
Rest verileri destek şifrelenmesi bazı verileri depolar. Bu veri depoları için veri şifreleme mekanizması etkinleştirmenizi öneririz. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Azure SQL Data warehouse'da saydam veri şifreleme (TDE) kötü amaçlı etkinliği tehdide karşı gerçek zamanlı şifreleme ve şifre çözme REST verilerinizin gerçekleştirerek ile korumaya yardımcı olur. Bu davranış, istemci için saydamdır. Daha fazla bilgi için bkz: [SQL veri ambarı veritabanında güvenli](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL veritabanı, gerçek zamanlı şifreleme ve şifre çözme veri uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirerek kötü amaçlı etkinliği tehdide karşı koruma ile yardımcı olan, saydam veri şifreleme (TDE) da destekler. Bu davranış, istemci için saydamdır. Daha fazla bilgi için bkz: [saydam veri şifrelemesi ile Azure SQL veritabanı](/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store ayrıca hesapta depolanan veriler için şifreleme sağlar. Etkinleştirildiğinde, Data Lake deposu otomatik olarak devam ettirmeden önce verileri şifreler ve alma, veri erişen istemci saydam hale önce şifresini çözer. Daha fazla bilgi için bkz: [Azure Data Lake Store'da güvenlik](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Depolama ve Azure tablo depolaması
Azure Blob Depolama ve Azure Table storage depolama hizmeti şifreleme (otomatik olarak depolama birimine devam ettirmeden önce verilerinizi şifreler ve alma önce şifresini çözer SSE), destekler. Daha fazla bilgi için bkz: [bekleyen veri için Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 REST verilerin istemci ve sunucu şifrelenmesini destekler. Daha fazla bilgi için bkz: [koruma verileri kullanarak şifreleme](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Şu anda, veri fabrikası sanal özel bulut (VPC) içinde Amazon S3 desteklemiyor.

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift küme şifreleme bekleyen veri için destekler. Daha fazla bilgi için bkz: [Amazon Redshift veritabanı şifreleme](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Şu anda, veri fabrikası Amazon Redshift bir VPC içinde desteklemiyor. 

#### <a name="salesforce"></a>Salesforce
Salesforce Shield Platform şifreleme'de, tüm dosyaları ekler, özel alanlar şifrelemeye izin destekler. Daha fazla bilgi için bkz: [Web sunucusu OAuth kimlik doğrulama akışı anlama](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-self-hosted-integration-runtime"></a>Karma senaryolar (kullanma kendini barındıran tümleştirmesi çalışma zamanı)
Karma senaryolar kendini barındıran tümleştirmesi çalışma zamanı bir şirket ağında veya bir sanal ağ (Azure) veya bir sanal özel bulut (Amazon) içinde yüklü olmasını gerektirir. Kendini barındıran tümleştirmesi çalışma zamanı yerel veri depolarına erişebilmeleri gerekir. Kendini barındıran tümleştirmesi çalışma zamanı hakkında daha fazla bilgi için bkz: [tümleştirmesi çalışma zamanı'kendi kendini barındıran](create-self-hosted-integration-runtime.md). 

![kendini barındıran tümleştirme çalışma zamanı kanalları](media/data-movement-security-considerations/data-management-gateway-channels.png)

**Komut kanalı** veri fabrikasında veri taşıma hizmetleri ve kendi kendini barındıran tümleştirmesi çalışma zamanı arasında iletişime olanak sağlar. İletişim faaliyete ilgili bilgiler içerir. Veri kanalı, şirket içi veri depoları ve bulut veri depoları arasında veri aktarımı için kullanılır.    

### <a name="on-premises-data-store-credentials"></a>Şirket içi veri deposu kimlik
Şirket içi veri depoları için kimlik bilgileri her zaman şifrelenir ve depolanır. Ya da yerel olarak kendini barındıran tümleştirmesi çalışma zamanı makinede depolanabilir veya Azure Data Factory'de (yalnızca depolama kimlik bilgileri bulut gibi) depolama birimi yönetilen. 

1. Seçebileceğiniz **kimlik bilgileri yerel olarak depolamak**. Şifrelemek ve kimlik bilgileri yerel olarak kendini barındıran tümleştirmesi Çalışma Zamanı Modülü depolamak istiyorsanız, adımları [kendini barındıran tümleştirmesi çalışma zamanı kimlik bilgisi şifreleme](encrypt-credentials-self-hosted-integration-runtime.md). Bu seçenek tüm bağlayıcıları destekler. Kendini barındıran tümleştirmesi çalışma zamanı Windows kullanır [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) hassas verileri şifrelemek için bilgi kimlik bilgisi. 

   Kullanım **yeni AzureRmDataFactoryV2LinkedServiceEncryptedCredential** bağlantılı hizmet kimlik bilgilerini şifrelemek / bağlantılı hizmet önemli ayrıntılar şifrelemek için cmdlet. Daha sonra döndürülen JSON kullanabilirsiniz (ile **EncryptedCredential** öğesinde **connectionString**) ile bağlantılı bir hizmet oluşturmak için **Set-AzureRmDataFactoryV2LinkedSevrice**cmdlet'i.  

2. Kullanmıyorsanız, **yeni AzureRmDataFactoryV2LinkedServiceEncryptedCredential** cmdlet'ini olarak ve Yukarıdaki adımda açıklanan ve bunun yerine doğrudan kullanın **kümesi AzureRmDataFactoryV2LinkedSevrice** cmdlet'iyle bağlantı dizeleri / satır JSON içinde kimlik bilgileri sonra bağlantılı hizmeti **şifrelenmiş ve depolanan Azure Data Factory yönetilen depolama**. Hassas bilgiler hala sertifikası tarafından şifrelenir ve bu sertifikalar Microsoft tarafından yönetilir.



#### <a name="ports-used-during-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Kendini barındıran tümleştirmesi çalışma zamanı'bağlantılı hizmet şifrelerken kullanılan bağlantı noktaları
Varsayılan olarak, PowerShell bağlantı noktasını kullanır. **8050** güvenli iletişim için kendi kendini barındıran tümleştirmesi çalışma zamanı makinede. Gerekirse, bu bağlantı noktası değiştirilebilir.  

![Ağ geçidi için HTTPS bağlantı noktası](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
Tüm veri aktarımlarını güvenli kanal olan **HTTPS** ve **TLS üzerinden TCP** Azure Hizmetleri ile iletişim sırasında man-in--middle saldırılarını önlemek için.

Aynı zamanda [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) veya [hızlı rota](../expressroute/expressroute-introduction.md) daha fazla şirket içi ağınız ile Azure arasındaki iletişim kanalını güvenli hale getirmek için.

Sanal ağ, bulut ağınızdaki mantıksal bir gösterimidir. IPSec VPN (siteden siteye) veya hızlı rota (özel eşleme) ayarlayarak, Azure sanal ağı (VNet) için bir şirket içi ağ bağlanabilir     

Kendini barındıran tümleştirmesi çalışma zamanı yapılandırma önerileri kaynak ve hedef birleşimlerini üzerinde karma veri taşıma için konumları tabanlı ve ağ aşağıdaki tabloda özetlenmiştir.

| Kaynak      | Hedef                              | Ağ yapılandırması                    | Tümleştirme çalışma zamanı modülü kurulumu                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Şirket içi | Sanal makineler ve sanal ağlarda dağıtılan bulut Hizmetleri | IPSec VPN (noktadan siteye veya siteden siteye) | Kendini barındıran tümleştirmesi çalışma zamanı olabilir ya da şirket içi yüklü veya bir Azure sanal üzerinde (VM) içinde sanal makine |
| Şirket içi | Sanal makineler ve sanal ağlarda dağıtılan bulut Hizmetleri | ExpressRoute (özel eşleme)           | Kendini barındıran tümleştirmesi çalışma zamanı olabilir ya da şirket içi yüklü veya bir Azure VM VNet içinde |
| Şirket içi | Genel bir uç nokta sahip azure tabanlı Hizmetleri | ExpressRoute (ortak eşleme)            | Kendini barındıran tümleştirmesi çalışma zamanı içi yüklü olması gerekir |

Aşağıdaki görüntüleri bir şirket içi veritabanı ile hızlı rota ve IPSec VPN (ile sanal ağ) kullanarak Azure hizmetleri arasında verileri taşımak için kendi kendini barındıran Integration zamanının kullanım göster:

**Hızlı rota:**

![Expressroute ağ geçidi ile kullanma](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![IPSec VPN ağ geçidi ile](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-self-hosted-integration-runtime"></a>Güvenlik duvarı yapılandırmaları ve uygulamaları güvenilir listeye almayı IP adresi (kendi kendini barındıran tümleştirmesi çalışma zamanı)

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Şirket içi/özel ağ için güvenlik duvarı gereksinimleri  
Bir kuruluşta bir **Kurumsal Güvenlik Duvarı** kuruluşun merkezi yönlendirici üzerinde çalışır. Ve **Windows Güvenlik Duvarı** kendini barındıran tümleştirmesi çalışma zamanı yüklendiği yerel makine üzerinde bir arka plan programı gibi çalışır. 

Aşağıdaki tabloda verilmiştir **giden bağlantı noktası** ve etki alanı gereksinimleri için **Kurumsal Güvenlik Duvarı**.

| Etki alanı adları                  | Giden bağlantı noktaları | Açıklama                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443, 80        | Kendini barındıran tümleştirmesi çalışma zamanı tarafından veri fabrikasında veri taşıma hizmetleri bağlanmak için gereken |
| `*.core.windows.net`          | 443            | Kendini barındıran tümleştirmesi çalışma zamanı tarafından kullandığınızda Azure depolama hesabına bağlanmak için kullanılan [kopyalama hazırlanan](copy-activity-performance.md#staged-copy) özelliği. |
| `*.frontend.clouddatahub.net` | 443            | Azure Data Factory hizmetine bağlanmak için kendi kendini barındıran tümleştirmesi çalışma zamanı tarafından gerekli. |
| `*.database.windows.net`      | 1433           | (İsteğe bağlı) gerekli Hedefinizi Azure SQL veritabanı olduğunda / Azure SQL veri ambarı. Bağlantı noktası 1433 açmadan Azure SQL veritabanı/Azure SQL Data Warehouse için verileri kopyalamak için hazırlanmış kopyalama özelliğini kullanın. |
| `*.azuredatalakestore.net`    | 443            | (İsteğe bağlı), hedef Azure Data Lake deposu olduğunda gerekli |

> [!NOTE] 
> Bağlantı noktaları yönetmek zorunda kalabilirsiniz / uygulamaları güvenilir listeye almayı etki alanları Kurumsal güvenlik duvarında düzey gerektiği gibi ilgili veri kaynakları tarafından. Bu tablo yalnızca Azure SQL Database, Azure SQL Data Warehouse, Azure Data Lake Store örnek olarak kullanır.   

Aşağıdaki tabloda verilmiştir **gelen bağlantı noktası** gereksinimleri **Windows Güvenlik Duvarı**.

| Gelen bağlantı noktaları | Açıklama                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | PowerShell şifreleme cmdlet tarafından açıklandığı gibi gerekli [kendini barındıran tümleştirmesi çalışma zamanı kimlik bilgisi şifreleme](encrypt-credentials-self-hosted-integration-runtime.md)/ kimlik bilgisi Yöneticisi uygulaması güvenli bir şekilde şirket içi veri depoları için kimlik bilgileri kendini barındıran ayarlama Tümleştirme çalışma zamanı. |

![Ağ geçidi bağlantı noktası gereksinimleri](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurationswhitelisting-in-data-store"></a>IP yapılandırmaları/beyaz listeleri kullanarak veri depolama
Bazı veri depolarına bulutta ayrıca erişmesini makinenin IP adresinin uygulamaları güvenilir listeye almayı gerektirir. Kendini barındıran tümleştirmesi çalışma zamanı makinenin IP adresini Güvenilenler listesine / Güvenlik Duvarı'nda uygun şekilde yapılandırılmış emin olun.

Aşağıdaki bulut veri depolarına kendini barındıran tümleştirmesi çalışma zamanı makinenin IP adresinin uygulamaları güvenilir listeye almayı gerektirir. Varsayılan olarak, bu veri depolarına bazıları uygulamaları güvenilir listeye almayı IP adresinin gerektirmeyebilir. 

- [Azure SQL Veritabanı](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Veri Ambarı](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Soru:** kendini barındıran tümleştirmesi çalışma zamanı farklı veri fabrikaları arasında paylaşılabilir?
**Yanıt:** bu özellik bir henüz desteklemiyoruz. Etkin olarak üzerinde çalışıyoruz.

**Soru:** çalışmak kendi kendini barındıran tümleştirmesi çalışma zamanı için bağlantı noktası gereksinimleri nelerdir?
**Yanıt:** kendini barındıran tümleştirmesi çalışma zamanı Internet açmak için HTTP tabanlı bağlantılar sağlar. **Giden bağlantı noktası 443 ve 80** kendini barındıran tümleştirmesi çalışma zamanı Bu bağlantı kurmayı açılması gerekir. Açık **gelen bağlantı noktası 8050** yalnızca makine düzeyinde (düzeyinde Kurumsal güvenlik duvarı) için kimlik bilgisi Yöneticisi uygulaması. Azure SQL Database veya Azure SQL Data Warehouse kullanılıyorsa olarak kaynak / hedef, ardından açmanız **1433** de bağlantı noktası. Daha fazla bilgi için bkz: [güvenlik duvarı yapılandırmaları ve uygulamaları güvenilir listeye almayı IP adreslerini](#firewall-configurations-and-whitelisting-ip-address-of gateway) bölümü. 


## <a name="next-steps"></a>Sonraki adımlar
Kopya etkinliği performansının hakkında daha fazla bilgi için bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](copy-activity-performance.md).

 
