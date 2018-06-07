---
title: Azure veri fabrikası'nda güvenlik değerlendirmeleri | Microsoft Docs
description: Verilerinizi güvenli hale getirmek için Azure Data Factory veri taşıma hizmetleri kullanan temel güvenlik altyapısı açıklar.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: abnarain
ms.openlocfilehash: 80cec0bc8136142f30ea7b957de819379b1bb139
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619142"
---
#  <a name="security-considerations-for-data-movement-in-azure-data-factory"></a>Azure Data factory'de veri taşımayı ilgili güvenlik konuları
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-data-movement-security-considerations.md)
> * [Sürüm 2 - Önizleme](data-movement-security-considerations.md)

Bu makalede, verilerinizin güvenliğini sağlamak için Azure Data Factory veri taşıma hizmetleri kullanan temel güvenlik altyapısı açıklanmaktadır. Veri Fabrikası yönetim kaynakları Azure güvenlik altyapı üzerine kurulmuş ve Azure tarafından sunulan tüm olası güvenlik önlemleri kullanın.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory sürüm 1 için veri taşıma güvenlik konuları](v1/data-factory-data-movement-security-considerations.md).

Bir Data Factory çözümünde bir veya daha fazla [işlem hattı](concepts-pipelines-activities.md) oluşturursunuz. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bu ardışık düzen veri fabrikası oluşturulduğu bölgede yer alır. 

Data Factory yalnızca Doğu ABD, Doğu ABD 2 ve Batı Avrupa bölgeler (sürüm 2 Önizleme) kullanılabilir olsa bile, veri taşıma Hizmeti'nde kullanılabilir [genel birçok bölgede](concepts-integration-runtime.md#azure-ir). Veri Taşıma hizmeti bu bölgeye henüz dağıtılmamışsa, alternatif bir bölge kullanın hizmete açıkça toplamasını sürece veri bir coğrafi konuma veya bölge oluşturmaz, Data Factory hizmeti sağlar. 

Azure Data Factory bağlantılı hizmeti kimlik bilgileri sertifikalar kullanılarak şifrelenmiş bulut veri depoları için dışında herhangi bir veriyi depolamaz. Data Factory ile arasında veri hareketini düzenlemek için veri temelli iş akışlarını oluşturma [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)ve kullanarak verilerin işlenmesini [işlem Hizmetleri](compute-linked-services.md) de başka bölgelerde veya içinde bir Şirket içi ortamı. Ayrıca, izlemek ve SDK'ları ve Azure İzleyicisi'ni kullanarak iş akışlarını yönetme.

Veri Fabrikası kullanarak veri taşıma için onaylanmıştır:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA YILDIZ](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Azure uyumluluk ve Azure kendi altyapısını nasıl korur düşünüyorsanız ziyaret [Microsoft Trust Center](https://www.microsoft.com/trustcenter).

Bu makalede, aşağıdaki iki veri taşıma senaryolarda güvenlik konuları inceleyin: 

- **Bulut senaryosu**: Bu senaryoda, hem kaynak hem de, hedef Internet üzerinden genel olarak erişilebilir. Bunlar Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon Redshift Salesforce gibi SaaS Hizmetleri ve FTP ve OData gibi web protokoller gibi yönetilen bulut depolama hizmetleri içerir. Desteklenen veri kaynaklarının tam listesi Bul [desteklenen veri depoları ve biçimleri](copy-activity-overview.md#supported-data-stores-and-formats).
- **Karma senaryo**: Bu senaryoda, kaynak ya da hedefiniz bir güvenlik duvarının arkasında veya şirket içi kurumsal ağ içinde değil. Veya veri deposu özel bir ağ veya sanal ağ (çoğunlukla kaynağı) ve genel olarak erişilebilir değil. Sanal makineler üzerinde barındırılan veritabanı sunucularını da bu senaryoya ayrılır.

## <a name="cloud-scenarios"></a>Bulut senaryoları

### <a name="securing-data-store-credentials"></a>Veri deposu kimlik güvenliğini sağlama

- **Şifrelenmiş kimlik bilgileri bir Azure Data Factory yönetilen deposunda depola**. Veri Fabrikası Microsoft tarafından yönetilen sertifikaları ile şifreleyerek veri deposu kimlik bilgilerinizi korumaya yardımcı olur. Bu sertifikaları (içeren sertifika yenileme ve kimlik bilgilerini geçişini) her iki yıllık döndürülür. Şifrelenmiş kimlik bilgileri güvenli bir şekilde Azure Data Factory Yönetim Hizmetleri tarafından yönetilen bir Azure depolama hesabında depolanır. Azure Storage güvenliği hakkında daha fazla bilgi için bkz: [Azure Storage güvenliğine genel bakış](../security/security-storage-overview.md).
- **Azure anahtar kasası kimlik bilgilerini saklamak**. Veri deposunun kimlik bilgisi de depolayabilir [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/). Veri Fabrikası bir etkinlik yürütme sırasında kimlik bilgisi alır. Daha fazla bilgi için bkz: [Azure anahtar kasası kimlik bilgisi deposu](store-credentials-in-key-vault.md).

### <a name="data-encryption-in-transit"></a>Aktarımdaki verileri şifreleme
Bulut veri deposu HTTPS veya TLS destekliyorsa, tüm veri aktarımlarını veri fabrikasında veri taşıma hizmetleri arasında ve bir bulut veri deposu olan güvenli kanal HTTPS veya TLS.

> [!NOTE]
> Veri aktarım için ve veritabanından olsa da Azure SQL Database ve Azure SQL Data Warehouse için tüm bağlantılar şifreleme (SSL/TLS) gerektirir. JSON kullanarak bir ardışık düzen geliştirme, şifreleme özelliğini ekler ve ayarlamak **true** bağlantı dizesinde. Azure Storage için kullandığınız **HTTPS** bağlantı dizesinde.

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi
Rest verileri destek şifrelenmesi bazı verileri depolar. Bu veri depoları için veri şifreleme mekanizması etkinleştirmenizi öneririz. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Azure SQL Data warehouse'da saydam veri şifreleme (TDE) gerçek zamanlı şifreleme ve şifre çözme REST verilerinizin gerçekleştirerek kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olur. Bu davranış, istemci için saydamdır. Daha fazla bilgi için bkz: [SQL veri ambarı veritabanında güvenli](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL veritabanı, gerçek zamanlı şifreleme ve şifre çözme veri uygulamasında yapılacak değişiklikler gerek kalmadan gerçekleştirerek kötü amaçlı etkinliği tehdide karşı korunmasına yardımcı olan saydam veri şifreleme (TDE) da destekler. Bu davranış, istemci için saydamdır. Daha fazla bilgi için bkz: [saydam veri şifreleme SQL veritabanı ve veri ambarı için](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake Store ayrıca hesapta depolanan veriler için şifreleme sağlar. Etkinleştirildiğinde, Data Lake Store, otomatik olarak devam ettirmeden önce verileri şifreler ve verilere erişen istemci saydam hale alma önce şifresini çözer. Daha fazla bilgi için bkz: [Azure Data Lake Store'da güvenlik](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Depolama ve Azure tablo depolaması
Depolama hizmeti şifreleme (otomatik olarak depolama birimine devam ettirmeden önce verilerinizi şifreler ve alma önce şifresini çözer SSE), Azure Blob Depolama ve Azure Table depolama destekler. Daha fazla bilgi için bkz: [bekleyen veri için Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 REST verilerin istemci ve sunucu şifrelenmesini destekler. Daha fazla bilgi için bkz: [koruma verileri kullanarak şifreleme](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift küme şifreleme bekleyen veri için destekler. Daha fazla bilgi için bkz: [Amazon Redshift veritabanı şifreleme](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). 

#### <a name="salesforce"></a>Salesforce
Salesforce Shield Platform şifreleme'de, tüm dosyaları, ekler ve özel alanlar şifrelenmesini sağlar destekler. Daha fazla bilgi için bkz: [Web sunucusu OAuth kimlik doğrulama akışı anlama](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios"></a>Karma senaryolar
Karma senaryolar kendini barındıran tümleştirmesi çalışma zamanı bir şirket ağındaki bir sanal ağ (Azure) içinde ya da sanal özel bulut (Amazon) içinde yüklü olmasını gerektirir. Kendini barındıran tümleştirmesi çalışma zamanı yerel veri depolarına erişebilmeleri gerekir. Kendini barındıran tümleştirmesi çalışma zamanı hakkında daha fazla bilgi için bkz: [oluşturmak ve yapılandırmak nasıl tümleştirmesi çalışma zamanı kendi kendini barındıran](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime). 

![kendini barındıran tümleştirme çalışma zamanı kanalları](media/data-movement-security-considerations/data-management-gateway-channels.png)

Komut kanalı veri fabrikasında veri taşıma hizmetleri kendini barındıran tümleştirmesi çalışma zamanı arasında iletişimi sağlar. İletişim faaliyete ilgili bilgiler içerir. Veri kanalı, şirket içi veri depoları ve bulut veri depoları arasında veri aktarımı için kullanılır.    

### <a name="on-premises-data-store-credentials"></a>Şirket içi veri deposu kimlik
Şirket içi veri depoları için kimlik bilgileri her zaman şifrelenir ve depolanır. Ya da kendi kendini barındıran tümleştirmesi çalışma zamanı makinede yerel olarak depolanan veya (yalnızca depolama kimlik bilgileri bulut gibi) Azure Data Factory yönetilen depolama alanına depolanır. 

- **Kimlik bilgileri yerel olarak depolamak**. Şifrelemek ve kimlik bilgileri yerel olarak kendini barındıran tümleştirmesi Çalışma Zamanı Modülü depolamak istiyorsanız, adımları [şirket içi veri depolarında Azure veri fabrikası için kimlik bilgilerini şifrelemek](encrypt-credentials-self-hosted-integration-runtime.md). Bu seçenek tüm bağlayıcıları destekler. Kendini barındıran tümleştirmesi çalışma zamanı Windows kullanır [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) hassas verileri ve kimlik bilgilerini şifrelemek için. 

   Kullanım **yeni AzureRmDataFactoryV2LinkedServiceEncryptedCredential** bağlantılı hizmeti kimlik bilgileri ve bağlantılı hizmet önemli ayrıntılar şifrelemek için cmdlet. Daha sonra döndürülen JSON kullanabilirsiniz (ile **EncryptedCredential** bağlantı dizesi öğesinde) kullanarak bağlantılı bir hizmet oluşturmak için **kümesi AzureRmDataFactoryV2LinkedService** cmdlet'i.  

- **Azure Data Factory yönetilen depolama deposunda**. Doğrudan kullanırsanız **kümesi AzureRmDataFactoryV2LinkedService** bağlantı cmdlet'iyle dizeleri ve satır JSON içinde kimlik bilgileri, bağlantılı hizmet şifrelenir ve Azure Data Factory yönetilen depolama alanında depolanır. Hassas bilgileri hala sertifikası tarafından şifrelenir ve Microsoft bu sertifikaları yönetir.



#### <a name="ports-used-when-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Kendini barındıran tümleştirmesi çalışma zamanı bağlantılı hizmette şifrelerken kullanılan bağlantı noktaları
Varsayılan olarak, kendi kendini barındıran tümleştirmesi çalışma zamanı makinede güvenli iletişim için bağlantı noktası 8050 PowerShell kullanır. Gerekirse, bu bağlantı noktası değiştirilebilir.  

![Ağ geçidi için HTTPS bağlantı noktası](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Aktarımdaki şifreleme
Tüm veri aktarımlarını güvenli kanal, Azure Hizmetleri ile iletişim sırasında man-in--middle saldırılarını önlemek için TCP üzerinden HTTPS ve TLS markalarıdır.

Aynı zamanda [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) veya [Azure ExpressRoute](../expressroute/expressroute-introduction.md) daha fazla şirket içi ağınız ile Azure arasındaki iletişim kanalını güvenli hale getirmek için.

Azure sanal ağı ağınızı buluttaki mantıksal bir gösterimidir. IPSec VPN (siteden siteye) ya da (özel eşleme) ExpressRoute ayarlayarak sanal ağınıza bir şirket ağına bağlanabilir.    

Kendini barındıran tümleştirmesi çalışma zamanı yapılandırma önerileri kaynak ve hedef birleşimlerini üzerinde karma veri taşıma için konumları tabanlı ve ağ aşağıdaki tabloda özetlenmiştir.

| Kaynak      | Hedef                              | Ağ yapılandırması                    | Tümleştirme çalışma zamanı kurulumu                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Şirket içi | Sanal makineler ve sanal ağlarda dağıtılan bulut Hizmetleri | IPSec VPN (noktadan siteye veya siteden siteye) | Kendini barındıran tümleştirmesi çalışma zamanı olabilir ya da şirket içi yüklü veya bir Azure sanal makinesi bir sanal ağ içinde. |
| Şirket içi | Sanal makineler ve sanal ağlarda dağıtılan bulut Hizmetleri | ExpressRoute (özel eşleme)           | Kendini barındıran tümleştirmesi çalışma zamanı olabilir ya da şirket içi yüklü veya bir Azure sanal makinesi bir sanal ağ içinde. |
| Şirket içi | Genel bir uç nokta sahip azure tabanlı Hizmetleri | ExpressRoute (ortak eşleme)            | Kendini barındıran tümleştirmesi çalışma zamanı içi yüklü olması gerekir. |

Aşağıdaki görüntüleri ExpressRoute ve IPSec VPN (ile Azure Virtual Network) kullanarak bir şirket içi veritabanına ve Azure hizmetleri arasında veri taşımak için kullanım kendini barındıran Integration zamanının göster:

**ExpressRoute**

![ExpressRoute ağ geçidi ile kullanma](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN**

![IPSec VPN ağ geçidi ile](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a> Güvenlik duvarı yapılandırmaları ve uygulamaları güvenilir listeye almayı IP adresleri

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Şirket içi/özel ağ için güvenlik duvarı gereksinimleri  
Kuruluş, kurumsal bir güvenlik duvarı kuruluşun merkezi yönlendirici üzerinde çalışır. Windows Güvenlik Duvarı kendini barındıran tümleştirmesi çalışma zamanı yüklendiği yerel makine üzerinde bir arka plan programı gibi çalışır. 

Aşağıdaki tabloda, güvenlik duvarları için giden bağlantı noktası ve etki alanı gereksinimleri verilmiştir:

| Etki alanı adları                  | Giden bağlantı noktaları | Açıklama                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Kendini barındıran tümleştirmesi çalışma zamanı tarafından veri fabrikasında veri taşıma hizmetleri bağlanmak için gerekli. |
| `*.core.windows.net`          | 443            | Kendini barındıran tümleştirmesi çalışma zamanı tarafından kullandığınızda Azure depolama hesabına bağlanmak için kullanılan [kopyalama hazırlanan](copy-activity-performance.md#staged-copy) özelliği. |
| `*.frontend.clouddatahub.net` | 443            | Kendini barındıran tümleştirmesi çalışma zamanı tarafından veri fabrikası hizmetine bağlanmak için gerekli. |
| `*.database.windows.net`      | 1433           | (İsteğe bağlı) Veya Azure SQL veritabanına veya Azure SQL Data Warehouse kopyalayın gereklidir. Bağlantı noktası 1433 açmadan Azure SQL Database veya Azure SQL Data Warehouse veri kopyalamak için hazırlanmış kopyalama özelliğini kullanın. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | (İsteğe bağlı) Veya Azure Data Lake Store'a kopyalayın gereklidir. |

> [!NOTE] 
> Bağlantı noktaları veya ilgili veri kaynakları tarafından gerekli olarak kurumsal güvenlik duvarı düzeyinde uygulamaları güvenilir listeye almayı etki alanlarını yönetmek zorunda kalabilirsiniz. Bu tablo yalnızca Azure SQL Database, Azure SQL Data Warehouse ve Azure Data Lake Store örnek olarak kullanır.   

Aşağıdaki tabloda, Windows Güvenlik Duvarı gelen bağlantı noktası gereksinimleri verilmiştir:

| Gelen bağlantı noktaları | Açıklama                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | PowerShell şifreleme cmdlet tarafından açıklandığı gibi gerekli [şirket içi veri depolarında Azure veri fabrikası için kimlik bilgilerini şifrelemek](encrypt-credentials-self-hosted-integration-runtime.md)ve güvenli bir şekilde şirket içi veri depoları için kimlik bilgilerini ayarlamak için kimlik bilgisi Yöneticisi uygulaması tarafından kendini barındıran tümleştirmesi çalışma zamanı '. |

![Ağ geçidi bağlantı noktası gereksinimleri](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-and-whitelisting-in-data-stores"></a>IP yapılandırmaları ve uygulamaları güvenilir listeye almayı veri depolarında
Bazı veri depolarına bulutta, ayrıca bu, beyaz liste deposuna erişilirken makinenin IP adresi gerektirir. Kendini barındıran tümleştirmesi çalışma zamanı makinenin IP adresini Güvenilenler listesine veya Güvenlik Duvarı'nda uygun şekilde yapılandırdığınızdan emin olun.

Bu, beyaz liste kendini barındıran tümleştirmesi çalışma zamanı makinenin IP adresini aşağıdaki bulut veri depoları gerektirir. Varsayılan olarak, bu veri depolarına bazıları uygulamaları güvenilir listeye almayı gerektirmeyebilir. 

- [Azure SQL Veritabanı](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Veri Ambarı](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Kendini barındıran tümleştirmesi çalışma zamanı farklı veri fabrikaları arasında paylaşılabilir?**

Bu özellik henüz desteklemiyoruz. Etkin olarak üzerinde çalışıyoruz.

**Çalışmak kendi kendini barındıran tümleştirmesi çalışma zamanı için bağlantı noktası gereksinimleri nelerdir?**

Kendini barındıran tümleştirmesi çalışma zamanı Internet'e erişmek için HTTP tabanlı bağlantılar sağlar. Giden bağlantı noktası 443 ve 80 bu bağlantıyı kurmak kendi kendini barındıran tümleştirmesi çalışma zamanı için açık olması gerekir. Yalnızca makine düzeyinde (Kurumsal güvenlik duvarı düzeyinde değil) kimlik bilgisi Yöneticisi uygulama için gelen istekler noktasının 8050 açın. Azure SQL Database veya Azure SQL Data Warehouse kaynak veya hedef olarak kullanılıyorsa, 1433 numaralı bağlantı noktasını da açmanız gerekir. Daha fazla bilgi için bkz: [güvenlik duvarı yapılandırmaları ve uygulamaları güvenilir listeye almayı IP adreslerini](#firewall-configurations-and-whitelisting-ip-address-of-gateway) bölümü. 


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği performansı hakkında daha fazla bilgi için bkz: [kopyalama etkinliği performans ve ayarlama Kılavuzu](copy-activity-performance.md).

 
