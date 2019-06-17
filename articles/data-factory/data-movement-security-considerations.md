---
title: Azure Data factory'de güvenlik konuları | Microsoft Docs
description: Verilerinizin güvenliğini sağlamak için Azure Data factory'deki veri taşıma hizmetleri kullanan temel bir güvenlik altyapısı açıklar.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: abnarain
ms.openlocfilehash: 635b45fe7f0108795c34f51081fa374c604036b2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66153259"
---
#  <a name="security-considerations-for-data-movement-in-azure-data-factory"></a>Azure Data factory'de veri taşımayı için güvenlik konuları
> [!div class="op_single_selector" title1="Data Factory hizmetinin kullandığınız sürümü seçin:"]
>
> * [Sürüm 1](v1/data-factory-data-movement-security-considerations.md)
> * [Geçerli sürüm](data-movement-security-considerations.md)

Bu makalede, verilerinizin güvenliğini sağlamak için Azure Data factory'deki veri taşıma hizmetleri kullanan temel bir güvenlik altyapısı açıklanır. Veri Fabrikası yönetim kaynakları, Azure güvenlik altyapıyla oluşturulmuş ve Azure tarafından sunulan tüm olası güvenlik önlemleri kullanın.

Bir Data Factory çözümünde bir veya daha fazla [işlem hattı](concepts-pipelines-activities.md) oluşturursunuz. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. Bu komut zincirleri, data factory oluşturulduğu bölgede yer alır. 

Data Factory yalnızca birkaç bölgelerinde kullanılabilir olsa da, veri taşıma hizmetidir [kullanılabilir genel](concepts-integration-runtime.md#integration-runtime-location) veri uyumluluk sağlamak için verimlilik ve daha düşük ağ çıkış maliyetlerini. 

Azure Data Factory, bağlı hizmet kimlik bilgilerini sertifikalar kullanılarak şifrelenmiş bulut veri depoları için dışında herhangi bir veri depolamaz. Data Factory ile veri arasında taşımayı düzenlemek için veri odaklı iş akışları oluşturma [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)ve kullanarak verilerin işlenmesini [işlem Hizmetleri](compute-linked-services.md) de başka bölgelerde veya bir Şirket içi ortamı. Ayrıca, izleme ve SDK'ları ve Azure İzleyicisi'ni kullanarak iş akışlarını yönetme.

Veri Fabrikası için yetkilendirildi:

| **[CSA STAR sertifika](https://www.microsoft.com/trustcenter/compliance/csa-star-certification)** |
| :----------------------------------------------------------- |
| **[ISO 20000-1:2011](https://www.microsoft.com/trustcenter/Compliance/ISO-20000-1)** |
| **[ISO 22301:2012](https://www.microsoft.com/trustcenter/compliance/iso-22301)** |
| **[ISO 27001: 2013](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001)** |
| **[ISO 27017:2015](https://www.microsoft.com/trustcenter/compliance/iso-iec-27017)** |
| **[ISO 27018:2014](https://www.microsoft.com/trustcenter/compliance/iso-iec-27018)** |
| **[ISO 9001:2015](https://www.microsoft.com/trustcenter/compliance/iso-9001)** |
| **[SOC 1, 2, 3](https://www.microsoft.com/trustcenter/compliance/soc)** |
| **[HIPAA BAA](https://www.microsoft.com/trustcenter/compliance/hipaa)** |

Azure uyumluluk ve Azure'nın kendi altyapısını nasıl korur ilgileniyorsanız ziyaret [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/default.aspx). Tüm Azure uyumluluk teklifleri denetimi - en son listesi için https://aka.ms/AzureCompliance.

Bu makalede, biz aşağıdaki iki veri taşıma senaryolarda güvenlik konuları gözden geçirin: 

- **Bulut senaryosu**: Bu senaryoda, hem kaynak hem de hedef internet üzerinden genel olarak erişilebilir. Bunlar, Azure depolama, Azure SQL veri ambarı, Azure SQL veritabanı, Azure Data Lake Store, Amazon S3, Amazon Redshift, Salesforce gibi SaaS hizmetlerine ve FTP ve OData gibi web protokolleri gibi yönetilen bulut depolama hizmetleri içerir. Desteklenen veri kaynaklarının tam bir listesi [desteklenen veri depoları ve biçimler](copy-activity-overview.md#supported-data-stores-and-formats).
- **Karma senaryo**: Bu senaryoda, kaynak veya hedef bir güvenlik duvarı ardında veya bir şirket içi kurumsal ağ içinde olur. Veya veri deposu içinde özel bir ağ veya sanal ağ (genellikle kaynak) ve genel olarak erişilebilir değil. Sanal makinelerde barındırılan veritabanı sunucuları, ayrıca bu senaryoya ayrılır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cloud-scenarios"></a>Bulut senaryoları

### <a name="securing-data-store-credentials"></a>Güvenliğini sağlama veri deposu kimlik bilgileri

- **Şifrelenmiş kimlik bilgileriyle bir Azure Data Factory yönetilen deposunda Store**. Data Factory ile Microsoft tarafından yönetilen sertifikaları şifreleyerek veri deposu kimlik bilgilerinizi korumaya yardımcı olur. Bu sertifikalar, (kod sertifika yenileme ve kimlik bilgilerini geçişini içerir) her iki yıl döndürülür. Şifrelenmiş kimlik bilgileri güvenli bir şekilde Azure Data Factory Yönetim Hizmetleri tarafından yönetilen bir Azure depolama hesabında depolanır. Azure depolama güvenliği hakkında daha fazla bilgi için bkz. [Azure depolama güvenliğine genel bakış](../security/security-storage-overview.md).
- **Azure anahtar Kasası'nda kimlik bilgileri Store**. Veri deposunun kimlik bilgisi olarak da depolayabilirsiniz [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/). Veri fabrikası, bir etkinlik yürütülmesi sırasında kimlik bilgisi alır. Daha fazla bilgi için [Store kimlik bilgilerini Azure Key vault'ta](store-credentials-in-key-vault.md).

### <a name="data-encryption-in-transit"></a>Aktarımdaki verileri şifreleme
Bulut veri deposu, HTTPS veya TLS destekliyorsa, Data factory'deki veri taşıma hizmetleri arasında tüm veri aktarımı ve bulut veri deposu olan güvenli kanal HTTPS veya TLS.

> [!NOTE]
> Veri aktarım için ve veritabanından durumdayken Azure SQL veritabanı ve Azure SQL veri ambarı yönelik tüm bağlantılar şifreleme (SSL/TLS) gerektirir. Bir işlem hattı JSON'ı kullanarak geliştirme, şifreleme özelliği ekleyin ve değerini **true** bağlantı dizesindeki. Azure depolama için kullanabileceğiniz **HTTPS** bağlantı dizesindeki.

> [!NOTE]
> Etkinleştirmek için Oracle veri taşırken aktarım sırasında şifreleme izleyin birini seçenekleri aşağıda:
> 1. Oracle server, Oracle Gelişmiş Güvenlik (OAS) için gidin ve başvuru Üçlü DES şifrelemesi (3DES) ve Gelişmiş Şifreleme Standardı (AES), destekleyen şifreleme ayarlarını yapılandır [burada](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759) Ayrıntılar için. ADF içinde OAS Oracle bağlantı kurarken yapılandırma bir kullanılacak şifreleme yöntemini otomatik olarak belirler.
> 2. ADF içinde EncryptionMethod ekleyebilirsiniz = 1 bağlantı dizesinde (bağlantılı hizmetteki). Bu şifreleme yöntemi olarak SSL/TLS kullanır. Bunu kullanmak için şifreleme çakışmayı önlemek için SSL olmayan şifreleme ayarlarını OAS Oracle sunucu tarafında devre dışı bırakmak gerekir.

> [!NOTE]
> Kullanılan TLS 1.2 sürümüdür.

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi
Bekleyen verilerin şifrelenmesi destek bazı veriler depolanır. Bu veri depoları için veri şifreleme mekanizması etkinleştirmenizi öneririz. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı
Azure SQL veri ambarı'nda saydam veri şifrelemesi (TDE), kötü amaçlı etkinlik tehditlerine karşı gerçek zamanlı şifreleme ve şifre çözme, bekleyen veri gerçekleştirerek koruma yardımcı olur. Bu davranış, istemci için saydamdır. Daha fazla bilgi için [güvenli bir veritabanında SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Veritabanı
Azure SQL veritabanı saydam veri şifrelemesi (TDE) yardımcı olan kötü amaçlı etkinlik tehditlerine karşı gerçek zamanlı şifreleme ve şifre çözme verileri uygulamada değişiklik gerektirmeden gerçekleştirerek koruma da destekler. Bu davranış, istemci için saydamdır. Daha fazla bilgi için [SQL veritabanı ve veri ambarı için saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake Store ayrıca hesapta depolanan veriler için şifreleme sağlar. Etkin olduğunda, Data Lake Store, otomatik olarak devam ettirmeden önce verileri şifreler ve verilere erişen istemci saydam yapmadan önce alma, şifresini çözer. Daha fazla bilgi için [Azure Data Lake Store güvenlik](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Depolama ve Azure tablo depolama
Depolama hizmeti şifrelemesi (otomatik olarak kalıcı depolama için önce verilerinizi şifreler ve şifresini çözer alma önce SSE), Azure Blob Depolama ve Azure tablo depolaması desteği. Daha fazla bilgi için [bekleyen veriler için Azure depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3, bekleyen verilerin istemci ve sunucu şifreleme destekler. Daha fazla bilgi için [veri şifreleme kullanarak koruma](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift, bekleyen veriler için küme şifrelemesini destekler. Daha fazla bilgi için [Amazon Redshift veritabanına şifreleme](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). 

#### <a name="salesforce"></a>Salesforce
Salesforce Shield Platform şifreleme tüm dosyaları, ekler ve özel alanları veren şifrelemesini destekler. Daha fazla bilgi için [Web sunucusu OAuth kimlik doğrulaması akışı anlama](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios"></a>Karma senaryolar
Karma senaryolar şirket içinde barındırılan tümleştirme çalışma zamanı bir şirket içi ağa (Azure) bir sanal ağ içinde ya da sanal özel bulut (Amazon) içinde yüklü olması gerekir. Şirket içinde barındırılan tümleştirme çalışma zamanı yerel veri depolarını erişebilir olması gerekir. Şirket içinde barındırılan tümleştirme çalışma zamanı hakkında daha fazla bilgi için bkz. [oluşturmak ve yapılandırmak nasıl Integration runtime şirket içinde barındırılan](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime). 

![Şirket içinde barındırılan tümleştirme çalışma zamanı kanallar](media/data-movement-security-considerations/data-management-gateway-channels.png)

Komut kanalı Data factory'deki veri taşıma hizmetleri ve şirket içinde barındırılan tümleştirme çalışma zamanı arasında iletişime olanak sağlar. İletişim için etkinlik ilgili bilgiler içerir. Veri kanalı, şirket içi veri depoları ile bulut veri depoları arasında veri aktarmak için kullanılır.    

### <a name="on-premises-data-store-credentials"></a>Şirket içi veri deposu kimlik bilgileri
Kimlik bilgileri, şirket içi veri depoları için her zaman şifrelenir ve depolanır. Ya da şirket içinde barındırılan tümleştirme çalışma zamanı makinesinde yerel olarak depolanan veya (bulut depolama kimlik bilgileri yalnızca gibi) Azure Data Factory yönetilen depolama alanında depolanır. 

- **Kimlik bilgileri yerel olarak Store**. Şifreleme ve kimlik bilgilerini şirket içinde barındırılan tümleştirme çalışma zamanını yerel olarak depolamak istiyorsanız, adımları [Azure Data factory'de şirket içi veri depoları için kimlik bilgilerini şifrele](encrypt-credentials-self-hosted-integration-runtime.md). Bu seçenek tüm bağlayıcıları destekler. Şirket içinde barındırılan tümleştirme çalışma zamanı kullanan Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) hassas veri ve kimlik bilgilerini şifrelemek için. 

   Kullanım **yeni AzDataFactoryV2LinkedServiceEncryptedCredential** bağlı hizmet kimlik bilgilerini ve bağlı hizmet hassas ayrıntılarında şifrelemek için cmdlet'i. Ardından döndürülen JSON kullanabilirsiniz (ile **EncryptedCredential** öğesinde bağlantı dizesi) kullanarak bir bağlı hizmetini oluşturmak için **kümesi AzDataFactoryV2LinkedService** cmdlet'i.  

- **Azure Data Factory yönetilen depolama Store**. Doğrudan kullanırsanız **kümesi AzDataFactoryV2LinkedService** cmdlet ile bağlantı dizeleri ve satır içi olarak JSON kimlik bilgileri, bağlı hizmet şifrelenir ve Azure Data Factory yönetilen depolanan. Hassas bilgilerin hala sertifikası tarafından şifrelenir ve bu sertifikalar Microsoft yönetir.



#### <a name="ports-used-when-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde bağlı hizmet şifrelerken kullanılan bağlantı noktaları
Varsayılan olarak, makineye şirket içinde barındırılan tümleştirme çalışma zamanı ile güvenli iletişim için bağlantı noktası 8050 PowerShell kullanır. Gerekirse, bu bağlantı noktası değiştirilebilir.  

![Ağ geçidi için HTTPS bağlantı noktası](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Tüm veri aktarımları sırasında Azure hizmetleriyle iletişim adam-de-adam saldırıları önlemek için TCP üzerinden HTTPS ve TLS güvenli kanal yoluyla olan.

Ayrıca [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) veya [Azure ExpressRoute](../expressroute/expressroute-introduction.md) daha fazla şirket içi ağınız ile Azure arasındaki iletişim kanalını güvenli hale getirmek için.

Azure sanal ağı, buluttaki ağınızın mantıksal bir gösterimidir. Sanal ağınıza (siteden siteye) IPSec VPN veya ExpressRoute (özel eşdüzey hizmet sağlama) ayarlayarak, bir şirket içi ağ bağlanabilirsiniz.    

Aşağıdaki tabloda özetlenmiştir ağ ve şirket içinde barındırılan tümleştirme çalışma zamanı yapılandırma önerileri karma veri taşıma için konumları kaynak ve hedef farklı kombinasyonlarına dayalı.

| Kaynak      | Hedef                              | Ağ yapılandırması                    | Tümleştirme çalışma zamanı kurulumu                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Şirket içi | Sanal makineler ve sanal ağlara dağıtılan bulut Hizmetleri | IPSec VPN (noktadan siteye veya siteden siteye) | Sanal ağdaki bir Azure sanal makinesinde, şirket içinde barındırılan tümleştirme çalışma zamanının yüklenmesi gerekir.  |
| Şirket içi | Sanal makineler ve sanal ağlara dağıtılan bulut Hizmetleri | ExpressRoute (özel eşdüzey hizmet sağlama)           | Sanal ağdaki bir Azure sanal makinesinde, şirket içinde barındırılan tümleştirme çalışma zamanının yüklenmesi gerekir.  |
| Şirket içi | Genel bir uç nokta içeren azure tabanlı Hizmetleri | ExpressRoute (Microsoft eşdüzey hizmet sağlama)            | Şirket içinde barındırılan tümleştirme çalışma zamanının yüklü şirket içi olabilir veya bir Azure sanal makinesinde. |

Aşağıdaki resimlerde ExpressRoute ve (Azure sanal ağı ile) IPSec VPN kullanarak şirket içi veritabanı ve Azure hizmetleri arasında veri taşıma için şirket içinde barındırılan tümleştirme çalışma zamanı kullanımını gösterir:

**ExpressRoute**

![ExpressRoute ağ geçidi ile kullanma](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN**

![IPSec VPN ağ geçidi ile](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a> Güvenlik duvarı yapılandırmaları ve IP adreslerini beyaz listeye ekleme

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Şirket içi/özel ağ için güvenlik duvarı gereksinimleri  
Bir kuruluşta, kuruluşun merkezi yönlendiricisinde kurumsal bir güvenlik duvarı çalıştırır. Windows Güvenlik Duvarı, şirket içinde barındırılan tümleştirme çalışma zamanının yüklü olduğu yerel makinede bir arka plan programı gibi çalışır. 

Aşağıdaki tabloda, güvenlik duvarları için giden bağlantı noktası ve etki alanı gereksinimleri verilmiştir:

| Etki alanı adları                  | Giden bağlantı noktaları | Açıklama                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Şirket içinde barındırılan tümleştirme çalışma zamanı tarafından Data factory'de veri hareketini hizmetlerine bağlanmak için gereklidir. |
| `*.frontend.clouddatahub.net` | 443            | Şirket içinde barındırılan tümleştirme çalışma zamanı tarafından Data Factory hizmetine bağlanmak için gereklidir. |
| `download.microsoft.com`    | 443            | Güncelleştirmeleri karşıdan yüklemek için şirket içinde barındırılan tümleştirme çalışma zamanı tarafından gerekli. Otomatik güncelleştirme devre dışı bıraktıysanız bu atlayabilirsiniz. |
| `*.core.windows.net`          | 443            | Kullandığınızda Azure depolama hesabına bağlanmak için şirket içinde barındırılan tümleştirme çalışma zamanı tarafından kullanılan [kopyalama aşamalı](copy-activity-performance.md#staged-copy) özelliği. |
| `*.database.windows.net`      | 1433           | (İsteğe bağlı) Veya Azure SQL veritabanı veya Azure SQL veri ambarı kopyalayın gereklidir. 1433 numaralı bağlantı noktasını açmaya gerek kalmadan Azure SQL veritabanı veya Azure SQL veri ambarı veri kopyalamak için hazırlanmış kopya özelliğini kullanın. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | (İsteğe bağlı) Veya Azure Data Lake Store için kopyalayın gereklidir. |

> [!NOTE] 
> Bağlantı noktaları veya ilgili veri kaynakları tarafından gerektiği gibi kurumsal bir güvenlik duvarı düzeyinde beyaz listeye ekleme etki alanlarını yönetmek zorunda kalabilirsiniz. Bu tablo yalnızca örnek olarak Azure SQL veritabanı, Azure SQL veri ambarı ve Azure Data Lake Store kullanır.   

Aşağıdaki tabloda, Windows Güvenlik Duvarı gelen bağlantı noktası gereksinimleri verilmiştir:

| Gelen bağlantı noktaları | Açıklama                              |
| ------------- | ---------------------------------------- |
| 8060 (TCP)    | PowerShell şifreleme cmdlet tarafından açıklandığı gibi gerekli [Azure Data factory'de şirket içi veri depoları için kimlik bilgilerini şifrele](encrypt-credentials-self-hosted-integration-runtime.md)ve güvenli bir şekilde şirket içi veri depoları için kimlik bilgilerini ayarlamak için kimlik bilgileri Yöneticisi uygulaması Şirket içinde barındırılan tümleştirme çalışma zamanı üzerinde. |

![Ağ geçidi bağlantı noktası gereksinimleri](media/data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-and-whitelisting-in-data-stores"></a>IP yapılandırmaları ve veri depolarında beyaz listeye ekleme
Bulut veri depoları, ayrıca, beyaz listeye eklemeniz deposuna erişilirken bir makinenin IP adresi gerektirir. Şirket içinde barındırılan tümleştirme çalışma zamanı makinenin IP adresi izin verilenler listesinde veya Güvenlik Duvarı'nda uygun şekilde yapılandırılmış emin olun.

Bu, beyaz liste IP adresi şirket içinde barındırılan tümleştirme çalışma zamanı makinenin aşağıdaki bulut veri depoları gerektirir. Varsayılan olarak, bu veri depoları bazı beyaz listeye ekleme gerekmeyebilir. 

- [Azure SQL Veritabanı](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Veri Ambarı](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**Şirket içinde barındırılan tümleştirme çalışma zamanı, farklı veri fabrikaları arasında paylaşılabilir?**

Evet. Diğer ayrıntıları [burada](https://azure.microsoft.com/blog/sharing-a-self-hosted-integration-runtime-infrastructure-with-multiple-data-factories/) bulabilirsiniz.

**Çalışmak şirket içinde barındırılan tümleştirme çalışma zamanı için bağlantı noktası gereksinimleri nelerdir?**

Şirket içinde barındırılan tümleştirme çalışma zamanı, İnternet'e erişmek için HTTP tabanlı bağlantılar oluşturur. Şirket içinde barındırılan tümleştirme çalışma zamanı bu bağlantıyı kurmak 443 giden bağlantı noktaları açılmalıdır. Gelen istekler noktasının 8050 yalnızca makine düzeyinde (Kurumsal güvenlik duvarınız düzeyinde değil) için kimlik bilgileri Yöneticisi uygulamasını açın. Azure SQL veritabanı veya Azure SQL veri ambarı kaynak veya hedef kullanılıyorsa, 1433 numaralı bağlantı noktasını da açmanız gerekir. Daha fazla bilgi için [güvenlik duvarı yapılandırmaları ve IP adreslerini beyaz listeye ekleme](#firewall-configurations-and-whitelisting-ip-address-of-gateway) bölümü. 


## <a name="next-steps"></a>Sonraki adımlar
Azure Data Factory kopyalama etkinliği performansı hakkında daha fazla bilgi için bkz: [kopyalama etkinliği performansı ve ayarlama Kılavuzu](copy-activity-performance.md).

 
