---
title: Azure şifreleme genel bakış | Microsoft Docs
description: Azure içindeki çeşitli şifreleme seçenekleri hakkında bilgi edinin
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: 00c8b30b5351b7a6e4388b186fab70e3a3357ef4
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="azure-encryption-overview"></a>Azure şifreleme genel bakış

Bu makalede, şifreleme Microsoft Azure'da nasıl kullanıldığını genel bir bakış sağlar. Şifreleme, bekleyen şifreleme, şifreleme uçuş ve Azure anahtar kasası anahtar yönetimi dahil olmak üzere temel alanları kapsamaktadır. Her bölüm daha ayrıntılı bilgilere bağlantılar içerir.

## <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi

Rest verileri dijital biçimi fiziksel medyada kalıcı depolama alanında bulunan bilgileri içerir. Medya dosyaları manyetik veya optik medya, arşivlenen verileri ve veri yedeklemeleri dahil edebilirsiniz. Microsoft Azure dosya, disk, blob ve tablo depolama da dahil olmak üzere farklı gereksinimlerini karşılamak için veri depolama çözümleri, çeşitli sunar. Microsoft ayrıca korumak için şifreleme sağlar [Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md), [Azure Cosmos DB](../cosmos-db/introduction.md)ve Azure Data Lake.

Bekleyen verileri şifreleme arasında hizmet (SaaS), bir hizmet (PaaS) ve altyapı olarak platform olarak yazılım hizmet (Iaas) bulut modelleri hizmetler için kullanılabilir değil. Bu makalede özetler ve Azure şifreleme seçenekleri kullanmanıza yardımcı olacak kaynaklar sağlar.

Rest verileri Azure'da nasıl şifrelenir daha ayrıntılı bilgi için bkz: [Azure veri şifreleme çalışmıyorken-](azure-security-encryption-atrest.md).

## <a name="azure-encryption-models"></a>Azure şifreleme modelleri

Azure hizmet tarafından yönetilen anahtarlar, anahtar kasası anahtarlardan müşteri tarafından yönetilen veya müşteri tarafından yönetilen anahtarları müşteri tarafından denetlenen donanımda kullanan sunucu tarafı şifreleme dahil olmak üzere çeşitli şifreleme modellerini destekler. İstemci tarafı şifreleme ile yönetin ve anahtarları şirket içi depolamak veya başka bir programda konumu güvenli.

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

İstemci tarafı şifreleme Azure dışında gerçekleştirilir. Aşağıdakileri içerir:

- Müşteri'nin veri merkezinde çalışan bir uygulama tarafından veya bir hizmet uygulaması tarafından şifrelenmiş veriler.
- Azure tarafından alındığında zaten şifrelenmiş veriler.

İstemci tarafı şifreleme ile bulut hizmet sağlayıcılarının şifreleme anahtarları erişiminiz yok ve bu verilerin şifresini çözemez. Anahtarların tam denetimi korumak.

### <a name="server-side-encryption"></a>Sunucu tarafı şifrelemesi

Üç sunucu tarafı şifreleme modeli gereksinimlerinize göre seçebileceğiniz farklı anahtar yönetimi özellikleri sağlar:

- **Hizmet tarafından yönetilen anahtarları**: denetim ve kullanışlı bir bileşimini düşük yükü ile sağlar.

- **Müşteri tarafından yönetilen anahtarları**: denetim Getir bilgisayarınızı kendi anahtarları (BYOK) desteği dahil olmak üzere anahtarlar üzerinden veya yenilerini oluşturmanızı sağlar sağlar.

- **Müşteri tarafından denetlenen donanım hizmet tarafından yönetilen anahtarlarında**: Microsoft denetimi dışında özel deponuz anahtarlarında yönetmenizi sağlar. Bu özellik, ana bilgisayarınızı kendi anahtarını Taşı (HYOK) denir. Ancak, yapılandırma karmaşıktır ve çoğu Azure Hizmetleri bu modeli desteklemez.

### <a name="azure-disk-encryption"></a>Azure disk şifrelemesi

Windows ve Linux sanal makineleri kullanarak koruyabilirsiniz [Azure disk şifrelemesi](azure-security-disk-encryption.md), kullanan [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) teknolojisi ve Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) hem korumak için işletim sistemi ve tam birim şifrelemesi ile veri disklerle.

Şifreleme anahtarları ve gizli anahtarları içinde korunması, [Azure anahtar kasası abonelik](../key-vault/key-vault-whatis.md). Azure Yedekleme hizmetini kullanarak, yedekleme ve şifrelenmiş anahtar şifreleme anahtarı (KEK) yapılandırma kullanan sanal makineleri (VM'ler) geri yükleyebilirsiniz.

### <a name="azure-storage-service-encryption"></a>Azure Depolama Hizmeti Şifrelemesi

Azure Blob Depolama ve Azure dosya paylaşımları, kalan verileri sunucu tarafı ve istemci tarafı senaryolarda şifrelenebilir.

[Azure Storage hizmeti şifreleme (SSE)](../storage/common/storage-service-encryption.md) verilerin depolandığı ve onu aldığınızda, otomatik olarak verilerin şifresini çözer önce otomatik olarak şifreleyebilirsiniz. İşlem kullanıcılara tamamen saydamdır. Depolama hizmeti şifrelemesi kullanır 256 bit [Gelişmiş Şifreleme Standardı (AES) şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok şifrelemeler birini olduğu. AES şifreleme, şifre çözme ve anahtar yönetimi şeffaf bir şekilde işler.

### <a name="client-side-encryption-of-azure-blobs"></a>İstemci tarafı şifreleme Azure BLOB

İstemci-tarafı gerçekleştirebilirsiniz şifreleme Azure BLOB'ların çeşitli şekillerde.

.NET NuGet paketi için Azure Storage istemci kitaplığı Azure depolama alanına karşıya yüklemeden önce istemci uygulamalar içinde verileri şifrelemek için kullanabilirsiniz.

Daha fazla bilgi edinmek ve Azure Storage istemci kitaplığı .NET NuGet paketini karşıdan yüklemek için bkz: [Windows Azure depolama 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage).

İstemci tarafı şifreleme anahtar kasası ile kullandığınızda, verilerinizi bir kerelik simetrik içerik şifreleme anahtarı (Azure Storage istemci SDK'sı tarafından oluşturulan CEK) kullanılarak şifrelenir. CEK, bir simetrik anahtar veya asimetrik anahtar çifti olabilen anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir. Yerel olarak yönetin veya anahtar kasasına depolar. Şifrelenmiş veriler daha sonra Azure depolama alanına yüklenir.

Anahtar kasası ile istemci tarafı şifreleme hakkında daha fazla bilgi edinmek ve nasıl yapılır yönergeleri ile kullanmaya başlamak için bkz: [Öğreticisi: şifrelemek ve anahtar kasası kullanarak Azure Storage blobları şifresini](../storage/storage-encrypt-decrypt-blobs-key-vault.md).

Son olarak, ayrıca Java için Azure Storage istemci kitaplığı Azure depolama alanına veri karşıya yüklemeden önce istemci tarafı şifreleme gerçekleştirmek ve istemciye yüklediğinizde verilerin şifresini çözmek için kullanabilirsiniz. Bu kitaplığı ile tümleştirme de destekler [anahtar kasası](https://azure.microsoft.com/services/key-vault/) depolama hesabı anahtarı yönetimi için.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Azure SQL Database rest verileri şifreleme

[Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md) bir ilişkisel veri, uzamsal, JSON ve XML gibi yapıları destekleyen Azure genel amaçlı ilişkisel veritabanı hizmetidir. SQL veritabanı saydam veri şifreleme (TDE) özelliği aracılığıyla sunucu tarafı şifreleme ve istemci tarafı şifreleme her zaman şifreli özelliği aracılığıyla destekler.

#### <a name="transparent-data-encryption"></a>Saydam Veri Şifrelemesi

[TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) şifrelemek için kullanılan [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md), ve [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) gerçek zamanlı bir veritabanı şifreleme anahtarı (DEK) kullanarak, veri dosyaları , Kurtarma sırasında depolanan kullanılabilirlik veritabanı önyükleme kaydındaki.

TDE, AES ve Üçlü Veri Şifreleme Standardı (3DES) kullanarak veri ve günlük dosyaları koruyan şifreleme algoritmaları. Veritabanı dosyasının şifreleme sayfa düzeyinde gerçekleştirilir. Şifrelenmiş bir veritabanı sayfalarında için yazılmadan önce şifrelenir disk ve belleğe okurken, şifresi çözülür. TDE, artık yeni oluşturulan Azure SQL veritabanı üzerinde varsayılan olarak etkindir.

#### <a name="always-encrypted-feature"></a>Her zaman şifreli özelliği

İle [her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) Özelliği Azure SQL Azure SQL veritabanında depolamak önce istemci uygulamalar içinde verilerin şifreleyebilirsiniz. Ayrıca, üçüncü taraflar için şirket içi veritabanı yönetim temsilcisi seçmeyi etkinleştirmek ve kendi ve verilerini görüntüleyebilir ve yönetebilirsiniz ancak erişimi olmaması gereken olanlar arasında ayrım korumak.

#### <a name="cell-level-or-column-level-encryption"></a>Hücre düzeyi veya sütun düzeyi şifreleme

Azure SQL veritabanı ile Transact-SQL kullanarak bir veri sütunu için simetrik şifreleme uygulayabilirsiniz. Bu yaklaşım adlı [hücre düzeyi şifreleme veya sütun düzeyi şifreleme (Temizle)](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data), çünkü belirli sütunları veya hatta belirli hücreleri verilerin farklı şifreleme anahtarlarını şifrelemek için kullanabilirsiniz. Bunun yapılması sayfaları, verileri şifreler TDE'den daha ayrıntılı şifreleme özelliği sağlar.

Temizle simetrik ya da asimetrik anahtarlar, bir sertifika 3DES kullanılarak bir parola veya ortak anahtar kullanarak verileri şifrelemek için kullanabileceğiniz yerleşik işlevi vardır.

### <a name="cosmos-db-database-encryption"></a>Cosmos DB veritabanı şifreleme

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) Microsoft'un Genel dağıtılmış birden çok model veritabanıdır. Cosmos DB'de geçici olmayan depolama (katı hal sürücüsü) depolanan kullanıcı verileri varsayılan olarak şifrelenir. Açmak veya kapatmak için denetim yoktur. Bekleyen şifreleme, güvenli anahtar depolama sistemleri, şifrelenmiş ağlar ve şifreleme API'leri dahil güvenlik teknolojileri çeşitli kullanılarak uygulanır. Şifreleme anahtarları, Microsoft tarafından yönetilir ve Microsoft iç yönergelerine göre döndürülür.

### <a name="at-rest-encryption-in-data-lake"></a>Data Lake çalışmıyorken şifreleme

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) herhangi resmi gereksinimleri veya şema tanımını önce tek bir yerde toplanan verileri her tür bir kuruluş çapında deposudur. Data Lake Store destekler "üzerinde varsayılan olarak," hesabınızı oluşturma sırasında ayarlanan bekleyen verilerin saydam şifreleme. Varsayılan olarak, Azure Data Lake Store anahtarları yönetiliyorsa, ancak kendiniz yönetmeye seçeneğiniz vardır.

Üç tür anahtarlarını, şifreleme ve verilerin şifresini çözmek kullanılır: ana şifreleme anahtarı (MEK), veri şifreleme anahtarı (DEK) ve blok şifreleme anahtarı (BEK). MEK kalıcı medyada depolanır, DEK şifrelemek için kullanılır ve BEK DEK ve veri bloğu türetilir. Kendi anahtarları yönetiyorsanız, MEK döndürebilirsiniz.

## <a name="encryption-of-data-in-transit"></a>Aktarımdaki verileri şifreleme

Azure bir konumdan diğerine hareket ederken verileri gizliliğini korumak için birçok mekanizma sunar.

### <a name="tlsssl-encryption-in-azure"></a>TLS/SSL şifreleme Azure

Microsoft kullanır [Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokolünü müşteriler ve bulut hizmetleri arasında seyahat halindeyken verileri korumak için. Microsoft veri merkezlerine Azure hizmetlerine bağlanan istemci sistemleri TLS bağlantıyla anlaşmaları. TLS, güçlü kimlik doğrulaması, ileti gizliliği ve bütünlük (ileti oynama, kişiler tarafından ele ve sahtekarlığı saptama etkinleştirme), birlikte çalışabilirlik, algoritma esnekliği ve dağıtım ve kullanım kolaylığı sağlar.

[Kusursuz iletme gizliliği](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) benzersiz anahtarlara göre müşterilerin istemci sistemleri ve Microsoft bulut hizmetleri arasında bağlantılar korur. Bağlantıları da RSA tabanlı 2.048 bit şifreleme anahtar uzunluklarını kullanın. Bu birleşim birisi için yolda ıntercept ve erişim verilere zorlaştırır.

### <a name="azure-storage-transactions"></a>Azure depolama işlemleri

Azure Portalı aracılığıyla Azure Storage ile etkileşim kurduklarında, tüm işlemleri HTTPS üzerinden gerçekleşir. Azure Storage ile etkileşim kurmak için HTTPS üzerinden depolama REST API de kullanabilirsiniz. Depolama hesabı için gerekli olan güvenli aktarımı etkinleştirerek erişim nesnelere REST API'leri depolama hesaplarında çağırdığınızda HTTPS kullanılmasını zorunlu kılabilir.

Paylaşılan erişim imzası ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), Azure Storage nesnelere erişimi temsilci, paylaşılan erişim imzaları kullandığınızda yalnızca HTTPS protokolünü kullanılabileceğini belirtmek için bir seçenek eklemek için kullanılabilir. Bu yaklaşım, SAS belirteci bağlantılarıyla gönderen herkes uygun protokolünü kullanan sağlar.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption), Azure dosya paylaşımları, desteklediği şifreleme ve onu erişmek için kullanılan Windows Server 2012 R2, Windows 8, Windows 8.1 ve Windows 10 kullanılabilir. Bölgeler arası erişim sağlar ve hatta masaüstünde erişim.

Azure Storage örneğinizi gönderilmeden önce istemci tarafı şifreleme böylece ağ üzerinden geçen gibi şifreli verileri şifreler.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Azure sanal ağlar üzerinden SMB şifrelemesi 

Kullanarak [SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) Windows Server 2012 veya sonraki sürümünü çalıştıran VM'ler veri aktarımlarını güvenli Azure sanal ağlar üzerinden Aktarımdaki verileri şifreleyerek yapabilirsiniz. Verileri şifreleyerek izinsiz ve gizli dinleme saldırılarına karşı korumaya yardımcı olmak. Yöneticiler, tüm sunucunun veya yalnızca belirli paylaşımları için SMB şifrelemesi etkinleştirebilirsiniz.

Bir paylaşımı veya sunucu için SMB şifrelemesi açık kaldıktan sonra varsayılan olarak, yalnızca SMB 3.0 istemciler şifrelenmiş paylaşımlara erişim izin verilir.

## <a name="in-transit-encryption-in-vms"></a>Sanal makineleri aktarım sırasında şifreleme

Aktarım için gelen ve Windows çalıştıran VM'ler arasında verileri çeşitli yollarla bağlantı doğasına bağlı olarak şifrelenir.

### <a name="rdp-sessions"></a>RDP oturumları

Bağlanabilir ve bir VM kullanarak oturum [Uzak Masaüstü Protokolü (RDP)](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) bir Windows istemci bilgisayarı veya bir RDP istemcisinin yüklü Mac. RDP oturumları ağ üzerinden Aktarım verileri TLS tarafından korunabilir.

Uzak Masaüstü, azure'da bir Linux VM bağlanmak için de kullanabilirsiniz.

### <a name="secure-access-to-linux-vms-with-ssh"></a>SSH ile Linux VM'ler güvenli erişim

Uzaktan Yönetimi için kullandığınız [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) Linux Azure üzerinde çalışan Vm'leri bağlanmak için (SSH). SSH güvenli olmayan bağlantıları üzerinden güvenli oturum açma işlemlerine izin veren bir şifreli bir bağlantı protokolüdür. Bu Azure'da barındırılan Linux VM'ler için varsayılan bağlantı protokolüdür. Kimlik doğrulaması için SSH anahtarları kullanarak oturum açmak parola gereksinimini ortadan kaldırır. SSH kimlik doğrulaması için bir ortak/özel anahtar çifti (asimetrik şifreleme) kullanır.

## <a name="azure-vpn-encryption"></a>Azure VPN şifreleme

Ağ üzerinden gönderilen verilerin gizliliği korumak için güvenli bir tünel oluşturur bir sanal özel ağ üzerinden Azure bağlanabilirsiniz.

### <a name="azure-vpn-gateways"></a>Azure VPN ağ geçitleri

Kullanabileceğiniz bir [Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) şifrelenmiş trafik sanal ağınız ile şirket içi konumunuz arasında ortak bir bağlantı üzerinden göndermek için veya sanal ağlar arasında trafiği göndermek için.

Siteden siteye VPN kullanım [IPSec](https://en.wikipedia.org/wiki/IPsec) aktarım şifreleme için. Azure VPN ağ geçitleri varsayılan teklifleri kümesi kullanır. Özel bir IPSec/IKE ilkesini belirli şifreleme algoritmaları ve anahtar gücü ile kullanmak için Azure VPN ağ geçitleri yapılandırmak yerine Azure varsayılan ilkeyi ayarlar.

### <a name="point-to-site-vpns"></a>Noktadan siteye VPN

Noktadan siteye VPN'lerde tek bir istemci bir Azure sanal ağı bilgisayarlar erişime izin verin. [Güvenli Yuva Tünel Protokolü (SSTP)](https://technet.microsoft.com/library/2007.06.cableguy.aspx) VPN tüneli oluşturmak için kullanılır. Güvenlik duvarları (tünel bir HTTPS bağlantısı olarak görünür) geçebilir. Noktadan siteye bağlanabilirlik için kendi iç ortak anahtar altyapısı (PKI) kök sertifika yetkilisi (CA) kullanabilirsiniz.

Sertifika kimlik doğrulaması veya PowerShell ile Azure portalını kullanarak bir sanal ağa noktadan siteye VPN bağlantısı yapılandırabilirsiniz.

Noktadan siteye VPN bağlantıları için Azure sanal ağlar hakkında daha fazla bilgi için bkz:

[Sertifika kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: Azure portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) 

[Sertifika kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpns"></a>Siteden siteye VPN 

Şirket içi ağınızı Azure sanal ağını IPSec/IKE (Ikev1 veya Ikev2) VPN tüneli bağlamak için siteden siteye VPN ağ geçidi bağlantısı kullanabilirsiniz. Bu tür bir bağlantı atanmış dışa dönük ortak IP adresine sahip bir şirket içi VPN cihazı gerektirir.

Azure portal, PowerShell veya Azure CLI kullanarak bir sanal ağ için siteden siteye VPN bağlantısı yapılandırabilirsiniz.

Daha fazla bilgi için bkz.

[Azure portalında bir siteden siteye bağlantı oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[PowerShell içinde bir siteden siteye bağlantı oluşturma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[CLI kullanarak bir sanal ağ ile bir siteden siteye VPN bağlantısı oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-data-lake"></a>Data Lake aktarım sırasında şifreleme

Data Lake Store'da aktarımdaki (diğer adıyla hareket halindeki) veriler de her zaman şifrelenir. Kalıcı ortamında depolama önce veri şifreleme ek olarak, veriler ayrıca her zaman aktarım sırasında HTTPS kullanılarak korunmaktadır. HTTPS, Data Lake Store REST arabirimleri için desteklenen tek protokoldür.

Data Lake Aktarımdaki verileri şifreleme hakkında daha fazla bilgi için bkz: [Data Lake Store'da verilerin şifrelenmesi](../data-lake-store/data-lake-store-encryption.md).

## <a name="key-management-with-key-vault"></a>Anahtar kasası ile anahtar yönetimi

Uygun koruması ve Yönetimi anahtarların şifreleme gereksiz işlenir. Anahtar kasası yönetmek ve bulut Hizmetleri tarafından kullanılan şifreleme anahtarlarının erişimi denetlemek için Microsoft tarafından önerilen çözümdür. Erişim anahtarları için izinleri, hizmetleri veya Azure Active Directory hesaplarını aracılığıyla kullanıcılara atanabilir.

Anahtar kasası yapılandırmak, düzeltme eki ve donanım güvenlik modülleri (HSM'ler) ve anahtar yönetimi yazılımı korumak için gereken kuruluşlar kurtarır. Anahtar kasası kullandığınızda, denetimi korumak. Microsoft hiçbir zaman anahtarlarınızı görür ve bunları doğrudan erişim uygulamanız yok. Ayrıca, alma veya HSM'ler içinde anahtarları oluştur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure güvenliğine genel bakış](security-get-started-overview.md)
- [Azure ağ güvenliğine genel bakış](security-network-overview.md)
- [Azure veritabanı güvenliğine genel bakış](azure-database-security-overview.md)
- [Azure sanal makineleri güvenliğine genel bakış](security-virtual-machines-overview.md)
- [Bekleme sırasında veri şifrelemesi](azure-security-encryption-atrest.md)
- [Veri güvenliği ve şifreleme için en iyi uygulamalar](azure-security-data-encryption-best-practices.md)
