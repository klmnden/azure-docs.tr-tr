---
title: "Azure şifreleme genel bakış | Microsoft Docs"
description: "Azure içindeki çeşitli şifreleme seçenekleri hakkında bilgi edinin"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 08/18/2017
ms.author: barclayn
ms.openlocfilehash: 752568747ab96bc0a9c7fc5f24ff28c3bce4e09f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-encryption-overview"></a>Azure şifreleme genel bakış

Bu makalede, şifreleme Microsoft Azure'da nasıl kullanıldığını genel bir bakış sağlar. Şifreleme, bekleyen şifreleme, şifreleme uçuş ve anahtar kasası ile anahtar yönetimi dahil olmak üzere temel alanları kapsamaktadır. Her bölüm daha ayrıntılı bilgi için bağlantılar içerir.

## <a name="encryption-of-data-at-rest"></a>Bekleyen verilerin şifrelenmesi

Rest verileri dijital biçimi fiziksel medyada kalıcı depolama alanında bulunan bilgileri içerir. Bu, manyetik veya optik medya, arşivlenen verileri ve veri yedeklemeleri dosyalarını içerir. Microsoft Azure dosya, disk, blob ve tablo depolama da dahil olmak üzere farklı gereksinimlerini karşılamak için veri depolama çözümleri, çeşitli sunar. Microsoft ayrıca korumak için şifreleme sağlar [Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md), [CosmosDB](../cosmos-db/introduction.md)ve Azure Data Lake.

Bekleyen verileri şifreleme kullanılabilir hizmetler için Azure hizmet olarak yazılım-a-arasında (SaaS)-olarak-hizmet Platform (PaaS) ve altyapı olarak-hizmet (Iaas) bulut modeller. Bu belge özetler ve Azure'nın şifreleme seçenekleri kullanmanıza yardımcı olacak kaynaklar sağlar.

Rest verileri Azure'da nasıl şifrelenir, tartışma ayrıntılı için başlıklı belgesine bakın [Azure veri şifreleme çalışmıyorken-](azure-security-encryption-atrest.md)

## <a name="azure-encryption-models"></a>Azure şifreleme modelleri

Azure hizmet tarafından yönetilen tuşlarını kullanarak, müşteri tarafından yönetilen anahtarları Azure anahtar kasası veya müşteri tarafından yönetilen anahtarları müşteri tarafından denetlenen donanımda kullanarak sunucu tarafı şifreleme dahil olmak üzere çeşitli şifreleme modellerini destekler. İstemci tarafı şifreleme, yönetme ve anahtarları şirket içi depolamak veya başka bir programda konumu güvenli olanak sağlar.

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

İstemci tarafı şifreleme Azure dışında gerçekleştirilir. İstemci tarafı şifreleme içerir:

- Müşteri'nin veri merkezinde çalışan bir uygulama tarafından veya bir hizmet uygulaması tarafından şifrelenmiş verileri
- Azure tarafından alındığında zaten şifrelenmiş veriler.

İstemci tarafı şifreleme ile bulut hizmeti sağlayıcısı şifreleme anahtarları için erişime sahip değil ve bu verilerin şifresini çözemez. Anahtarların tam denetimi korumak.

### <a name="server-side-encryption"></a>Sunucu tarafı şifreleme

Üç sunucu tarafı şifreleme modeli gereksinimlerinizi seçilebilir farklı anahtar yönetimi özellikleri sunar.

- **Hizmet tarafından yönetilen anahtarları** denetim ve kullanışlı bir bileşimini düşük yükü ile sağlayın

- **Müşteri tarafından yönetilen anahtarları** kendi anahtarları (BYOK) getirmek veya yenilerini oluşturmak için özelliği de dahil olmak üzere anahtarları üzerinde denetim sağlar.

- **Ccustomer controlledhardware hizmeti yönetilen anahtarlarında** anahtarları, Microsoft'un denetimi dışında olan özel deponuzun yönetmenizi sağlar. Bu ana bilgisayarınızı kendi anahtarını Taşı (HYOK) denir. Ancak, yapılandırma karmaşıktır ve çoğu Azure Hizmetleri bu modeli desteklemez.

### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

Windows ve Linux sanal makineleri kullanarak korunabilir [Azure Disk şifrelemesi](azure-security-disk-encryption.md), kullanan [Windows BitLocker](https://technet.microsoft.com/library/cc766295(v=ws.10).aspx) teknolojisi ve Linux [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) işletim sistemi disklerinde ve veri diskleri tam birim şifreleme ile korumak için.

Şifreleme anahtarları ve gizli anahtarları içinde korunması, [Azure anahtar kasası](../key-vault/key-vault-whatis.md) abonelik. Yedekleme ve Azure Yedekleme hizmetini kullanarak KEK yapılandırmayla şifrelenmiş şifrelenmiş VM'ler geri yükleyebilirsiniz.

### <a name="azure-storage-service-encryption"></a>Azure depolama hizmeti şifrelemesi

Veri deposunda kalan Azure (hem Blob hem de dosya), sunucu tarafı ve istemci tarafı senaryolarda şifrelenebilir.

[Azure depolama hizmeti şifrelemesi](../storage/storage-service-encryption.md) (SSE) otomatik olarak şifreleme veri önce depolanır ve, işlem tamamen saydam kullanıcıların veri aldığınızda, otomatik olarak çözer. Depolama hizmeti şifrelemesi kullanır 256 bit [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), güçlü blok birini olduğu kullanılabilir şifrelemeleri ve şifreleme, şifre çözme ve anahtar yönetimi saydam bir şekilde işler.

### <a name="client-side-encryption-of-azure-blobs"></a>İstemci tarafı şifreleme Azure BLOB

İstemci tarafı şifreleme Azure BLOB farklı şekillerde gerçekleştirilebilir.

.NET NuGet paketi için Azure Storage istemci kitaplığı Azure Storage'a yüklemeden önce istemci uygulamalar içinde verileri şifrelemek için kullanabilirsiniz.

Daha fazla bilgi edinmek ve Azure Storage istemci kitaplığı .NET NuGet paketini karşıdan yüklemek için adlı belgeye bakın [Windows Azure depolama 8.3.0](https://www.nuget.org/packages/WindowsAzure.Storage)

İstemci tarafı şifreleme Azure anahtar kasası ile kullandığınızda, verilerinizi bir kerelik simetrik içerik şifreleme anahtarı (Azure Storage istemci SDK'sı tarafından oluşturulan CEK) kullanılarak şifrelenir. CEK, bir simetrik anahtar veya asimetrik anahtar çifti olabilen anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir. Yerel olarak yönetin veya Azure anahtar kasası depolar. Şifrelenmiş veriler Azure depolama hizmetine sonra yüklenir.

Azure anahtar kasası ile istemci tarafı şifreleme hakkında daha fazla bilgi edinmek ve nasıl yapılır yönergeleri ile çalışmaya başlama için adlı belgeye bakın [Öğreticisi: şifrelemek ve şifresini çözmek Azure anahtar kasası kullanılarak Microsoft Azure Storage blobları](../storage/storage-encrypt-decrypt-blobs-key-vault.md)

Son olarak, ayrıca Java için Azure Storage istemci kitaplığı verilerini Azure Storage'a yüklemeden önce istemci tarafı şifreleme gerçekleştirmek ve istemciye indirirken verilerin şifresini çözmek için kullanabilirsiniz. Bu kitaplığı ile tümleştirme de destekler [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) depolama hesabı anahtarı yönetimi için.

### <a name="encryption-of-data-at-rest-with-azure-sql-database"></a>Azure SQL Database rest verileri şifreleme

[Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md) bir ilişkisel veri, uzamsal, JSON ve XML gibi yapıları destekleyen Microsoft Azure genel amaçlı ilişkisel veritabanı hizmetidir. Azure SQL saydam veri şifreleme (TDE) özelliği aracılığıyla sunucu tarafı şifreleme ve istemci tarafı şifreleme her zaman şifreli özelliği aracılığıyla destekler.

#### <a name="transparent-data-encryption"></a>Saydam veri şifrelemesi

[TDE saydam veri şifreleme](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) şifrelemek için kullanılan [SQL Server](https://www.microsoft.com/sql-server/sql-server-2016), [Azure SQL veritabanı](../sql-database/sql-database-technical-overview.md), ve [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) veritabanında depolanan bir veritabanı şifreleme anahtarı (DEK) kullanarak gerçek zamanlı veri dosyalarını önyükleme kullanılabilirlik kaydı sırasında kurtarma.

TDE, AES ve 3DES şifreleme algoritmaları kullanarak veri ve günlük dosyalarını korur. Veritabanı dosyasının şifreleme sayfa düzeyinde gerçekleştirilir; şifrelenmiş bir veritabanı sayfalarında için yazılmadan önce şifrelenir disk ve belleğe okurken, şifresi çözülür. TDE, artık yeni oluşturulan Azure SQL veritabanı üzerinde varsayılan olarak etkindir.

#### <a name="always-encrypted"></a>Always encrypted

[Her zaman şifreli](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) Azure SQL özelliği, Azure SQL veritabanında depolamak önce istemci uygulamalar içinde verilerin şifrelemenizi sağlar ve üçüncü taraflara şirket içi veritabanı yönetim temsilcisi seçmeyi etkinleştirmek ve kullanıcılar kendi ve verileri ve yönetme ancak erişimi olmaması gereken olanlar görüntüleyebilir arasında birbirinden ayırmaya olanak sağlar.

#### <a name="cellcolumn-level-encryption"></a>Hücre/sütun düzeyinde şifreleme

Azure SQL veritabanı Transact-SQL kullanarak verileri bir sütun için simetrik şifreleme uygulayabilmenizi sağlar. Bu adlı [düzeyinde şifreleme veya sütun düzeyinde şifreleme hücre](https://docs.microsoft.com/sql/relational-databases/security/encryption/encrypt-a-column-of-data) (Temizle), çünkü belirli sütunları veya hatta belirli hücreleri verilerin farklı şifreleme anahtarlarını şifrelemek için kullanabilirsiniz. Bu sayfa verileri şifreler TDE'den daha ayrıntılı şifreleme özelliği sağlar.

Temizle simetrik ya da asimetrik anahtarlar, bir sertifikanın ortak anahtarının veya 3DES kullanılarak bir parola kullanarak verileri şifrelemek için kullanabileceğiniz yerleşik işlevi vardır.

### <a name="cosmos-db-database-encryption"></a>Cosmos DB veritabanı şifreleme

[Azure Cosmos DB](../cosmos-db/database-encryption-at-rest.md) Microsoft'un Genel dağıtılmış birden çok model veritabanıdır. Cosmos DB'de geçici olmayan depolama (katı hal sürücüsü) depolanan kullanıcı verileri varsayılan olarak şifrelenir; açmak veya kapatmak için denetim yoktur. Bekleyen şifreleme, güvenli anahtar depolama sistemleri, şifrelenmiş ağlar ve şifreleme API'leri dahil güvenlik teknolojileri çeşitli kullanılarak uygulanır. Şifreleme anahtarları, Microsoft tarafından yönetilir ve Microsoft'un iç yönergelerine göre döndürülür.

### <a name="at-rest-encryption-in-azure-data-lake"></a>Azure Data Lake çalışmıyorken şifreleme

[Azure Data Lake](../data-lake-store/data-lake-store-encryption.md) herhangi resmi gereksinimleri veya şema tanımını önce tek bir yerde toplanan verileri her tür bir kuruluş çapında deposudur. Azure Data Lake Store destekler "üzerinde varsayılan olarak," hesabınızı oluşturma sırasında ayarlanan bekleyen verilerin saydam şifreleme. Varsayılan olarak, Data Lake Store anahtarları yönetiliyorsa, ancak kendiniz yönetmeye seçeneğiniz vardır.

Üç tür anahtarlarını, şifreleme ve verilerin şifresini çözmek kullanılır: ana şifreleme anahtarı (MEK), veri şifreleme anahtarı (DEK) ve blok şifreleme anahtarı (BEK). MEK kalıcı medyada depolanır, DEK şifrelemek için kullanılır ve BEK DEK ve veri bloğu türetilir. Kendi anahtarları yönetiyorsanız, MEK döndürebilirsiniz.

## <a name="encryption-of-data-in-transit"></a>Aktarımdaki verileri şifreleme

Azure bir konumdan diğerine hareket ederken verileri gizliliğini korumak için birçok mekanizma sunar.

### <a name="tlsssl-encryption-in-azure"></a>TLS/SSL şifreleme Azure

Microsoft kullanır [Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) protokolünü müşteriler ve bulut hizmetleri arasında seyahat halindeyken verileri korumak için. Microsoft'un veri merkezlerinin Azure hizmetlerine bağlanan istemci sistemleri TLS bağlantıyla anlaşmaları. TLS, güçlü kimlik doğrulaması, ileti gizliliği ve bütünlük (ileti oynama, kişiler tarafından ele ve sahtekarlığı saptama etkinleştirme), birlikte çalışabilirlik, algoritma esnekliği, dağıtım ve kullanım kolaylığı sağlar.

[Kusursuz iletme gizliliği](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) olan müşterilerin istemci sistemleri ve Microsoft'un bulut hizmetleri arasında bağlantılar benzersiz anahtarlara göre korur. Bağlantıları da RSA tabanlı 2.048 bit şifreleme anahtar uzunluklarını kullanın. Bu birleşim birisi için aktarım sırasında olan ıntercept ve verilere zorlaştırır.

### <a name="azure-storage-transactions"></a>Azure depolama işlemleri

Azure Portalı aracılığıyla Azure Storage ile etkileşim kurduklarında, tüm işlemleri HTTPS üzerinden gerçekleşir. Azure Storage ile etkileşim kurmak için HTTPS üzerinden depolama REST API de kullanabilirsiniz. Erişim nesnelere REST API'leri güvenli aktarımı için depolama hesabı gerekli etkinleştirerek depolama hesaplarında çağrılırken HTTPS kullanılmasını zorunlu kılabilir.

Paylaşılan erişim imzası ([SAS](../storage/storage-dotnet-shared-access-signature-part-1.md)), Azure Storage nesnelere erişimi temsilci, paylaşılan erişim imzaları kullanırken yalnızca HTTPS protokolünü kullanılabileceğini belirtmek için bir seçenek eklemek için kullanılabilir. Bu herkes SAS belirteci bağlantılarıyla dışarı gönderme uygun protokolünü kullanan sağlar.

[SMB 3.0](https://technet.microsoft.com/library/dn551363(v=ws.11).aspx#BKMK_SMBEncryption) Azure dosya paylaşımları destekler şifreleme ve cihazın Windows Server 2012 R2, Windows 8, Windows 8.1 ve Windows 10, çapraz bölge erişimine kullanılabilir erişmek ve hatta masaüstünde erişmek için kullanılır.

Azure depolama birimine gönderilmeden önce istemci tarafı şifreleme böylece ağ üzerinden geçen gibi şifreli verileri şifreler.

### <a name="smb-encryption-over-azure-virtual-networks"></a>Azure sanal ağlar üzerinden SMB şifrelemesi 

[SMB 3.0](https://support.microsoft.com/help/2709568/new-smb-3-0-features-in-the-windows-server-2012-file-server) Azure Vm'lerde Windows Server 2012 çalıştıran ve yukarıdaki veri aktarımlarını güvenli Azure sanal izinsiz ve gizli dinleme saldırılarına karşı korumak için ağları üzerinden Aktarımdaki verileri şifreleyerek yapma yeteneği sağlar. Yöneticiler, tüm sunucu ya da yalnızca belirli paylaşımları için SMB şifrelemesi etkinleştirebilirsiniz.

SMB şifrelemesi bir paylaşımı veya sunucu için açıldıktan sonra varsayılan olarak, yalnızca SMB 3 istemcilere şifreli paylaşımlara erişim izin verilir.

## <a name="in-transit-encryption-in-azure-virtual-machines"></a>Azure sanal makinelerde aktarım sırasında şifreleme

Aktarım için gelen ve Azure Windows çalıştıran VM'ler arasında verileri çeşitli yollarla bağlantı doğasına bağlı olarak şifrelenir.

### <a name="rdp-sessions"></a>RDP oturumları

Bağlanma ve oturum kullanarak bir Azure VM [Uzak Masaüstü Protokolü](https://msdn.microsoft.com/library/aa383015(v=vs.85).aspx) (RDP) bir Windows istemci bilgisayarı veya bir RDP istemcisinin yüklü Mac. RDP oturumları ağ üzerinden Aktarım verileri TLS tarafından korunabilir.

Uzak Masaüstü, azure'da bir Linux VM bağlanmak için de kullanabilirsiniz.

### <a name="secure-access-to-linux-vms-with-ssh"></a>SSH ile Linux VM'ler güvenli erişim

Kullanabileceğiniz [Secure Shell](../virtual-machines/linux/ssh-from-windows.md) uzaktan yönetim için Azure'da çalışan Linux VM'ler bağlanmak için (SSH). SSH güvenli olmayan bağlantıları üzerinden güvenli oturum açma bilgileri veren bir şifreli bir bağlantı protokolüdür. Bu Azure'da barındırılan Linux VM'ler için varsayılan bağlantı protokolüdür. Kimlik doğrulaması için SSH anahtarları kullanarak oturum açmak parola gereksinimini ortadan kaldırır. SSH kimlik doğrulaması için bir ortak/özel anahtar çifti (asimetrik şifreleme) kullanır.

## <a name="azure-vpn-encryption"></a>Azure VPN şifreleme

Ağ üzerinden gönderilen verilerin gizliliği korumak için güvenli bir tünel oluşturur bir sanal özel ağ üzerinden Azure bağlanabilirsiniz.

### <a name="azure-vpn-gateway"></a>Azure VPN Gateway

[Azure VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md) şifrelenmiş trafik sanal ağınız ile şirket içi konumunuz arasında ortak bir bağlantı üzerinden göndermek için veya sanal ağlar arasında trafiği göndermek için kullanılabilir.

Siteden siteye VPN kullanan [IPSec](https://en.wikipedia.org/wiki/IPsec) aktarım şifreleme için. Azure VPN ağ geçitleri varsayılan teklifleri kümesi kullanır. Özel bir IPSec/IKE ilkesini belirli şifreleme algoritmaları ve anahtar gücü ile kullanmak için Azure VPN ağ geçitleri yapılandırmak yerine Azure varsayılan ilkeyi ayarlar.

### <a name="point-to-site-vpn"></a>Noktadan siteye VPN

Noktadan siteye VPN'lerde tek bir istemci bir Azure sanal ağı bilgisayarlar erişime izin verin. [Güvenli Yuva Tünel Protokolü](https://technet.microsoft.com/library/2007.06.cableguy.aspx) (SSTP) VPN tüneli oluşturmak için kullanılır ve (tünel bir HTTPS bağlantısı olarak görünür) güvenlik duvarlarından geçebilir. Noktadan siteye bağlanabilirlik için kendi iç PKI kök CA'yı kullanabilirsiniz.

Sertifika kimlik doğrulaması veya PowerShell ile Azure portalını kullanarak bir sanal ağa noktadan siteye VPN bağlantısı yapılandırabilirsiniz.

Noktadan siteye VPN bağlantıları için Azure sanal ağlar hakkında daha fazla bilgi için bkz: [sertifika kimlik doğrulaması kullanan bir sanal ağa noktadan siteye bağlantı yapılandırma: Azure portal](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md) ve

[Sertifika kimlik doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırma: PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### <a name="site-to-site-vpn"></a>Siteden siteye VPN 

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır. Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir.

Azure portalı, PowerShell veya Azure komut satırı arabirimi (CLI) kullanarak bir sanal ağ için siteden siteye VPN bağlantısı yapılandırabilirsiniz.

Daha fazla bilgi için bu okuyun:

[Azure portalında bir siteden siteye bağlantı oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)

[Siteden siteye bağlantı oluşturma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

[CLI kullanarak siteden siteye VPN bağlantısı olan bir sanal ağ oluşturma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="in-transit-encryption-in-azure-data-lake"></a>Azure Data Lake aktarım sırasında şifreleme

Data Lake Store'da aktarımdaki (diğer adıyla hareket halindeki) veriler de her zaman şifrelenir. Kalıcı medyaya depolama önce veri şifrelemeye ek olarak, aktarımdaki veriler de her zaman HTTPS kullanılarak korunmaktadır. HTTPS, Data Lake Store REST arabirimleri için desteklenen tek protokoldür.

Azure Data Lake Aktarımdaki verileri şifreleme hakkında daha fazla bilgi için başlıklı belge bkz [Azure Data Lake Store'da verilerin şifrelenmesi.](../data-lake-store/data-lake-store-encryption.md)

## <a name="key-management-with-azure-key-vault"></a>Azure anahtar kasası anahtar yönetimi

Uygun koruması ve Yönetimi anahtarların şifreleme gereksiz işlenir. Azure anahtar kasası yönetmek ve bulut Hizmetleri tarafından kullanılan şifreleme anahtarlarının erişimi denetlemek için Microsoft'un önerilen çözümdür. Erişim anahtarları için izinleri, hizmetleri veya Azure Active Directory hesaplarını aracılığıyla kullanıcılara atanabilir.

Azure anahtar kasası yapılandırmak, düzeltme eki ve donanım güvenlik modülleri (HSM'ler) ve anahtar yönetimi yazılımı korumak için gereken kuruluşlar kurtarır. Azure anahtar kasası ile Microsoft anahtarlarınızı hiçbir zaman görür ve bunları doğrudan erişim uygulamanız yok; denetimi korumak. Ayrıca, alma veya HSM'ler içinde anahtarları oluştur.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure güvenliğine genel bakış](security-get-started-overview.md)
- [Azure ağ güvenliğine genel bakış](security-network-overview.md)
- [Azure veritabanı güvenliğine genel bakış](azure-database-security-overview.md)
- [Azure sanal makineleri güvenliğine genel bakış](security-virtual-machines-overview.md)
- [Bekleme sırasında veri şifrelemesi](azure-security-encryption-atrest.md)
- [Veri güvenliği ve şifreleme için en iyi uygulamalar](azure-security-data-encryption-best-practices.md)
