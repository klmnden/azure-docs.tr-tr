---
title: Azure şifrelemeye genel bakış | Microsoft Docs
description: Azure'da çeşitli şifreleme seçenekleri hakkında bilgi edinin
services: security
documentationcenter: na
author: Barclayn
manager: barbkess
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: barclayn
ms.openlocfilehash: 272cc843ab90eade06525f665d3cf2decf74a26f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60444525"
---
# <a name="azure-encryption-overview"></a>Azure şifrelemeye genel bakış

Bu makalede, şifreleme Microsoft Azure'da nasıl kullanıldığına genel bir bakış sağlar. Şifreleme, bekleme sırasında şifreleme, şifreleme uçuş yanı sıra, Azure Key Vault ile anahtar yönetimi dahil olmak üzere genel alanlarını kapsar. Her bölümde daha ayrıntılı bilgi için bağlantılar içerir.

## <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi

Bekleyen veriler herhangi bir sayısal biçimde fiziksel medyada kalıcı depolama alanında bulunan bilgileri içerir. Medya dosyaları manyetik veya optik medya, arşivlenmiş veriler ve veri yedeklemeleri içerebilir. Microsoft Azure, çeşitli dosya, disk, blob ve tablo depolama gibi farklı ihtiyaçları karşılamak için veri depolama çözümleri sunar. Microsoft ayrıca korumak için şifreleme sağlar [Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md), [Azure Cosmos DB](../cosmos-db/introduction.md)ve Azure Data Lake.

Bekleyen verileri şifreleme çeşitli yazılım olarak hizmet (SaaS), platform olarak hizmet (PaaS) ve altyapı (ıaas) bulut modelleri olarak hizmetler için kullanılabilir. Bu makalede özetler ve Azure şifreleme seçenekleri kullanmanıza yardımcı olacak kaynaklar sunulmaktadır.

Bekleyen verileri Azure'da nasıl şifrelenir daha ayrıntılı bir açıklaması için bkz: [Azure veri şifreleme bekleyen](azure-security-encryption-atrest.md).

## <a name="azure-encryption-models"></a>Azure şifreleme modeli

Azure, hizmet tarafından yönetilen anahtarlar, anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar veya müşteri tarafından yönetilen anahtarlar müşteri tarafından denetlenen donanımda kullanan sunucu tarafı şifreleme dahil olmak üzere çeşitli şifreleme modelleri destekler. İstemci tarafı şifreleme ile yönetin ve şirket anahtarları depolamak veya başka bir programda konumunun güvenliğini sağlayın.

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

İstemci tarafı şifreleme Azure dışında gerçekleştirilir. Aşağıdakileri içerir:

- Müşteri'nin veri merkezinde çalışan bir uygulama tarafından veya bir hizmet uygulaması tarafından şifrelenmiş veriler.
- Azure tarafından alındığında zaten şifrelenmiş veriler.

Bulut hizmeti sağlayıcıları, istemci tarafı şifreleme ile şifreleme anahtarlarının erişiminiz yoksa ve bu verilerin şifresini çözemez. Anahtarların tam denetimi elinizde.

### <a name="server-side-encryption"></a>Sunucu tarafı şifrelemesi

Üç sunucu tarafı şifrelemesi model gereksinimlerinize göre seçebileceğiniz farklı anahtar yönetimi özellikleri sağlar:

- **Hizmet tarafından yönetilen anahtarlar**: Denetim ve kullanışlı bir birleşimini düşük yüklerle sağlar.

- **Müşteri tarafından yönetilen anahtarlar**: Verir Getir bilgisayarınızı kendi anahtarlar (BYOK) desteği de dahil olmak üzere anahtarlarını denetlemek veya yenilerini oluşturmanızı sağlar.

- **Hizmet tarafından yönetilen anahtarlar müşteri tarafından denetlenen donanımda**: Microsoft denetim dışında özel deponuzda anahtarları yönetmenizi sağlar. Bu özellik, ana bilgisayarınızı kendi anahtarını Taşı (HYOK) denir. Ancak, yapılandırma karmaşıktır ve çoğu Azure hizmeti bu modeli desteklemez.

### <a name="azure-disk-encryption"></a>Azure disk şifrelemesi

Windows ve Linux sanal makinelerini kullanarak koruyabilirsiniz [Azure disk şifrelemesi](azure-security-disk-encryption.md), kullanan [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) teknoloji ve Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) hem korumak için işletim sistemi diskleri ve veri diskleri ile tam birim şifreleme.

Şifreleme anahtarlarını ve gizli anahtarları içinde korunması, [Azure Key Vault aboneliklerinde](../key-vault/key-vault-whatis.md). Azure Backup hizmetini kullanarak, yedekleme ve geri yükleme şifrelenmiş anahtar şifreleme anahtarı (KEK) yapılandırması kullanan sanal makineleri (VM'ler).

### <a name="azure-storage-service-encryption"></a>Azure Depolama Hizmeti Şifrelemesi

Hem sunucu tarafı ve istemci tarafı senaryolarda Azure Blob Depolama ve Azure dosya paylaşımlarını bölgesinde, bekleyen veri şifrelenebilir.

[Azure depolama hizmeti şifrelemesi (SSE)](../storage/common/storage-service-encryption.md) depolandığını ve onu aldığınızda, otomatik olarak verilerin şifresini çözer önce verileri otomatik olarak şifreleyebilirsiniz. İşlem, kullanıcılara tamamen saydamdır. Depolama hizmeti şifrelemesi, 256 bit kullanır [Gelişmiş Şifreleme Standardı (AES) şifrelemesiyle](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), en güçlü blok şifreleme özelliklerinden biri olan. AES şifreleme, şifre çözme ve anahtar yönetimi şeffaf bir şekilde işler.

### <a name="client-side-encryption-of-azure-blobs"></a>Azure BLOB'ları istemci tarafı şifreleme

İstemci tarafı gerçekleştirebileceğiniz çeşitli şekillerde şifreleme Azure blobları.

Azure depolama karşıya önce istemci uygulamalar içinde verilerin şifrelenmesi için .NET NuGet paketi için Azure depolama istemci Kitaplığı'nı kullanabilirsiniz.

Daha fazla bilgi edinin ve Azure depolama istemci kitaplığı için .NET NuGet paketi indirmek için bkz: [Windows Azure depolama 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage).

İstemci tarafı şifreleme anahtar kasası ile kullandığınızda, verilerinizi, bir kerelik simetrik içerik şifreleme anahtarını (Azure depolama istemci SDK'sı tarafından oluşturulan CEK) kullanılarak şifrelenir. CEK simetrik anahtar veya asimetrik anahtar çifti olabilen anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir. Key Vault'ta depolamak ya da yerel olarak yönetin. Şifrelenmiş veriler, ardından Azure depolama alanına yüklenir.

Key Vault ile istemci tarafı şifreleme hakkında daha fazla bilgi edinmek ve nasıl yapılır yönergeleri ile çalışmaya başlama hakkında bilgi için bkz: [Öğreticisi: Şifreleme ve anahtar Kasası'nı kullanarak Azure Depolama'daki blobları şifre çözme](../storage/storage-encrypt-decrypt-blobs-key-vault.md).

Son olarak, Java için Azure depolama istemci kitaplığı Azure Storage'a veri karşıya yüklemeden önce istemci tarafı şifreleme gerçekleştirmek ve istemciye indirirken verilerin şifresini çözmek için kullanabilirsiniz. Bu kitaplık ayrıca ile tümleştirmeyi destekler [Key Vault](https://azure.microsoft.com/services/key-vault/) depolama hesabı anahtarı yönetimi için.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Azure SQL veritabanı ile bekleyen verilerin şifrelenmesi

[Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md) Azure'da yer alan ve ilişkisel veri, JSON, uzamsal ve XML gibi yapıları destekleyen çok amaçlı ilişkisel veritabanı hizmetidir. SQL veritabanı saydam veri şifrelemesi (TDE) özelliği üzerinden sunucu tarafı şifrelemesi hem istemci tarafı şifreleme aracılığıyla Always Encrypted özelliğini destekler.

#### <a name="transparent-data-encryption"></a>Saydam Veri Şifrelemesi

[TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) şifrelemek için kullanılan [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md), ve [Azure SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) bir veritabanı şifreleme anahtarı (DEK) kullanarak gerçek zamanlı veri dosyaları , Kurtarma sırasında depolanan kullanılabilirlik veritabanı önyükleme kaydındaki.

TDE, AES ve Üçlü Veri Şifreleme Standardı (3DES) kullanarak veri ve günlük dosyaları koruyan şifreleme algoritmaları. Veritabanı dosyasının şifreleme sayfa düzeyinde gerçekleştirilir. Şifrelenmiş bir veritabanı sayfaları için yazılmadan önce şifrelenir disk ve belleğe okuma, şifresi çözülür. TDE, artık yeni oluşturulan Azure SQL veritabanları üzerinde varsayılan olarak etkindir.

#### <a name="always-encrypted-feature"></a>Her zaman şifreli özelliği

İle [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) Özelliği Azure SQL'de Azure SQL veritabanı'nda depolama önce istemci uygulamalar içinde verilerin şifreleyebilirsiniz. Ayrıca, şirket içi veritabanı yönetim üçüncü taraflara etkinleştirmek ve kullanıcıların kendi verilerini görüntüleyebilir ve yönetmek, ancak ona erişimi olmaması gereken kişiler arasında ayrılığı koruma.

#### <a name="cell-level-or-column-level-encryption"></a>Hücre düzeyinde veya sütun düzeyinde şifreleme

Azure SQL veritabanı ile Transact-SQL kullanarak bir veri sütununu simetrik şifreleme uygulayabilirsiniz. Bu yaklaşım adlı [hücre düzeyinde şifreleme veya sütun düzeyinde şifrelemeyi (CLE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data), belirli sütunları veya hatta belirli veri hücrelerini farklı şifreleme anahtarlarıyla şifrelemenizi için kullanabilirsiniz. Bunun yapılması daha sayfalarda verileri şifreler TDE, daha ayrıntılı şifreleme özelliği sağlar.

CLE simetrik ya da asimetrik anahtarlar, bir sertifika 3DES kullanılarak bir parola veya ortak anahtar kullanarak verileri şifrelemek için kullanabileceğiniz yerleşik işlevleri vardır.

### <a name="cosmos-db-database-encryption"></a>Cosmos DB veritabanı şifrelemesi

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) Microsoft'un Global olarak dağıtılmış, çok modelli veritabanıdır. Cosmos DB'de (katı hal sürücüsü) geçici olmayan depolama alanında depolanan kullanıcı verileri varsayılan olarak şifrelenir. Açıp kapatmak için denetim yoktur. Bekleme sırasında şifreleme birkaç güvenli anahtar depolama sistemleri, şifreli ağları ve şifreleme API'leri dahil güvenlik teknolojileri kullanılarak uygulanır. Şifreleme anahtarları, Microsoft tarafından yönetilir ve Microsoft iç yönergelerine göre döndürülür.

### <a name="at-rest-encryption-in-data-lake"></a>Veri Gölü'nde bekleyen şifreleme

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) her tür verinin tek bir yerde bir resmi gereksinim tanımı veya şema önce toplanan bir Kurumsal Çapta deposudur. Data Lake Store destekler "üzerinde varsayılan olarak," saydam hesabınızı oluşturma sırasında ayarlanır ve bekleyen verilerin şifrelenmesi. Varsayılan olarak, Azure Data Lake Store anahtarları sizin yerinize yönetir, ancak bunları yönetmek için seçeneğiniz vardır.

Üç tür anahtarlar, şifreleme ve verilerin şifresini çözme kullanılır: ana şifreleme anahtarı (MEK), veri şifreleme anahtarı (DEK) ve blok şifreleme anahtarı (BEK). MEK'i kalıcı medyada depolanır, DEK şifrelemek için kullanılır ve BEK, DEK'ten ve veri bloğundan türetilir. Kendi anahtarlarınızı yönetiyorsanız, mek'i döndürebilirsiniz.

## <a name="encryption-of-data-in-transit"></a>Aktarımdaki verileri şifreleme

Azure, tek bir konumdan diğerine taşınırken veri gizliliğini korumak için birçok mekanizma sunar.

### <a name="tlsssl-encryption-in-azure"></a>Azure'da TLS/SSL şifreleme

Microsoft'un kullandığı [Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokolü bulut Hizmetleri ve müşteriler arasında seyahat ederken verileri korumak için. Microsoft veri merkezleri ile Azure hizmetlerine bağlanan istemci sistemleri TLS bağlantı anlaşması. TLS, güçlü kimlik doğrulaması, ileti gizliliği ve bütünlük (ileti oynama, durdurma ve sahtekarlığı saptama etkinleştirme), birlikte çalışabilirlik, algoritma esnekliği ve dağıtım ve kullanım kolaylığı sağlar.

[Kusursuz iletme gizliliği](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) benzersiz anahtarlara göre müşterilerin istemci sistemleri ve Microsoft bulut hizmetleri arasında bağlantılar korur. Bağlantıları da RSA tabanlı 2.048 bit şifreleme anahtar uzunluklarını kullanın. Bu birleşim, birisi için Aktarım verileri kesme ve erişim zorlaştırır.

### <a name="azure-storage-transactions"></a>Azure depolama işlemleri

Azure portalı üzerinden Azure depolama ile etkileşimde bulunmak, tüm işlemler HTTPS üzerinden gerçekleşir. Azure depolama ile etkileşimde bulunmak için HTTPS üzerinden depolama REST API de kullanabilirsiniz. Depolama hesabı için gerekli olan güvenli aktarım etkinleştirerek depolama hesaplarında erişim nesneleri için REST API'lerini çağırdığınızda HTTPS kullanılmasını zorunlu kılabilir.

Paylaşılan erişim imzaları ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), paylaşılan erişim imzaları'nı kullandığınızda yalnızca HTTPS protokolü kullanılabilir belirtmek için bir seçenek Azure depolama nesneleri erişimi devretmek için kullanılabilir. Bu yaklaşım, SAS belirteçleri bağlantılarla gönderen herkes doğru protokolü kullanmasını sağlar.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption), Azure dosya paylaşımları, desteklediği şifreleme ve onu erişmek için kullanılan Windows Server 2012 R2, Windows 8, Windows 8.1 ve Windows 10 kullanılabilir. Bölgeler arası erişim sağlar ve hatta masaüstünde erişim.

Azure depolama Örneğinize gönderilmeden önce istemci tarafı şifreleme verileri şifreler; böylece ağ üzerinden geçen olarak şifrelenir.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Azure sanal ağları üzerinden SMB şifrelemesi 

Kullanarak [SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) Windows Server 2012 veya sonraki sürümünü çalıştıran VM'ler veri aktarımları, Azure sanal ağları üzerinden Aktarımdaki verileri şifreleyerek güvenli hale getirebilirsiniz. Verileri şifreleyerek kurcalama ve gizlice saldırılarına karşı korumaya yardımcı olur. Yöneticiler, tüm sunucunun veya yalnızca belirli paylaşımları için SMB şifrelemesi etkinleştirebilirsiniz.

SMB şifrelemesi paylaşımı veya sunucu için açıldıktan sonra varsayılan olarak, yalnızca SMB 3.0 istemciler şifrelenmiş paylaşımlara erişim izni.

## <a name="in-transit-encryption-in-vms"></a>Vm'lerde aktarım sırasında şifreleme

Aktarım için gelen ve Windows çalıştıran sanal makineler arasında verileri, bağlantının doğasına bağlı olarak çeşitli yollarla şifrelenir.

### <a name="rdp-sessions"></a>RDP oturumları

Bağlanmak ve bir VM'ye kullanarak oturum açma [Uzak Masaüstü Protokolü (RDP)](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) yüklü bir RDP istemcisi ile bir Mac veya Windows istemci bilgisayarı. RDP oturumları ağ üzerinden Aktarım verileri tarafından TLS korunabilir.

Uzak Masaüstü, azure'da bir Linux VM'ye bağlanmak için de kullanabilirsiniz.

### <a name="secure-access-to-linux-vms-with-ssh"></a>SSH ile Linux Vm'leri güvenli erişim

Uzaktan Yönetimi için kullandığınız [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) (Azure'da çalışan Linux Vm'lerine bağlanmak için SSH). SSH güvenli olmayan bağlantılara güvenli oturum açma işlemleri izin veren bir şifreli bağlantı protokolüdür. Bu Azure'da barındırılan Linux VM'ler için varsayılan bağlantı protokolüdür. Kimlik doğrulaması için SSH anahtarlarını kullanarak oturum açmak parola gereksinimini ortadan kaldırır. SSH kimlik doğrulaması için bir ortak/özel anahtar çifti (asimetrik şifreleme) kullanır.

## <a name="azure-vpn-encryption"></a>Azure VPN şifreleme

Ağ üzerinden gönderilen verilerin gizliliği korumak için güvenli bir tünel oluşturan bir sanal özel ağ üzerinden Azure'a bağlanabilirsiniz.

### <a name="azure-vpn-gateways"></a>Azure VPN ağ geçitleri

Kullanabileceğiniz bir [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) ortak bir bağlantı üzerinden sanal ağınız ile şirket içi konumunuz arasında şifrelenmiş trafik göndermek için veya sanal ağlar arasında trafik göndermek için.

Siteden siteye VPN'ler kullanım [IPSec](https://en.wikipedia.org/wiki/IPsec) aktarım şifreleme için. Azure VPN ağ geçitleri, bir dizi varsayılan tekliflerini kullanın. Özel IPSec/IKE ilke belirli şifreleme algoritmaları ve anahtar güçleriyle kullanmak için Azure VPN ağ geçitleri yapılandırabileceğiniz yerine Azure varsayılan ilkesini ayarlar.

### <a name="point-to-site-vpns"></a>Noktadan siteye VPN'ler

Noktadan siteye VPN'lerde tek tek istemci bilgisayarları bir Azure sanal ağı erişmesine. [Güvenli Yuva Tünel Protokolü (SSTP)](https://technet.microsoft.com/library/2007.06.cableguy.aspx) VPN tüneli oluşturmak için kullanılır. Güvenlik duvarları (tünel bir HTTPS bağlantısı olarak görünür) çapraz geçiş yapabilirsiniz. Noktadan siteye bağlantı için kendi iç ortak anahtar altyapısı (PKI) kök sertifika yetkilisi (CA) kullanabilirsiniz.

Bir sanal ağa noktadan siteye VPN bağlantısı sertifika kimlik doğrulaması veya PowerShell ile Azure portalını kullanarak yapılandırabilirsiniz.

Noktadan siteye VPN bağlantıları için Azure sanal ağları hakkında daha fazla bilgi için bkz:

[Sertifika kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: Azure portalı](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

[Sertifika kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpns"></a>Siteden siteye VPN'ler 

Şirket içi ağınızı bir IPSec/IKE (Ikev1 veya Ikev2) VPN tüneli üzerinden Azure sanal ağına bağlanmak için siteden siteye VPN ağ geçidi Bağlantısı'nı kullanabilirsiniz. Bu tür bir bağlantı, kendisine atanmış bir dönük genel IP adresine sahip bir şirket içi VPN cihazı gerektirir.

Azure portal, PowerShell veya Azure CLI kullanarak bir sanal ağ için siteden siteye VPN bağlantısı yapılandırabilirsiniz.

Daha fazla bilgi için bkz.

[Azure portalını kullanarak siteden siteye bağlantı oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[PowerShell'de bir siteden siteye bağlantı oluşturma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[CLI kullanarak siteden siteye VPN bağlantısı ile sanal ağ oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-data-lake"></a>Veri Gölü'nde aktarım sırasında şifreleme

Data Lake Store'da aktarımdaki (diğer adıyla hareket halindeki) veriler de her zaman şifrelenir. İçinde kalıcı medyaya depolama önce veri şifrelemeye ek olarak, veriler de her zaman Aktarımdaki HTTPS kullanılarak korunmaktadır. HTTPS, Data Lake Store REST arabirimleri için desteklenen tek protokoldür.

Veri Gölü'nde Aktarımdaki verileri şifreleme hakkında daha fazla bilgi için bkz: [Data Lake Store içinde verilerin şifrelenmesi](../data-lake-store/data-lake-store-encryption.md).

## <a name="key-management-with-key-vault"></a>Key Vault ile anahtar yönetimi

Şifreleme, uygun koruması ve Yönetimi anahtarların, yararsız olarak işlenir. Anahtar kasası yönetimi ve bulut Hizmetleri tarafından kullanılan şifreleme anahtarlarına erişimi denetlemek için Microsoft tarafından önerilen çözümdür. Anahtarlarına erişmek için izinler Hizmetleri veya Azure Active Directory hesaplarını aracılığıyla kullanıcılara atanabilir.

Key Vault, yapılandırma, düzeltme eki uygulama ve donanım güvenlik modülleri (HSM'ler) ve anahtar yönetim yazılımı korumak için gereken kuruluşlar üzerinizden alır. Key Vault kullanırken, denetimi korumak. Microsoft hiçbir zaman anahtarlarınızı görür ve uygulamaların bunlara doğrudan erişiminiz yok. Ayrıca, içeri aktarma veya anahtarlar, Hsm'lerde oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure güvenliğine genel bakış](security-get-started-overview.md)
- [Azure ağ güvenliğine genel bakış](security-network-overview.md)
- [Azure veritabanı güvenliğine genel bakış](azure-database-security-overview.md)
- [Azure sanal makineleri güvenliğine genel bakış](security-virtual-machines-overview.md)
- [Bekleme sırasında veri şifrelemesi](azure-security-encryption-atrest.md)
- [Veri güvenliği ve şifreleme için en iyi uygulamalar](azure-security-data-encryption-best-practices.md)
