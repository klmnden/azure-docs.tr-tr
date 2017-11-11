---
title: "Azure kişisel verileri koruma şifrelemeli REST | Microsoft Docs"
description: "Bu makalede, kişisel verileri korumak için Azure kullanmanıza yardımcı olacak bir dizi bir parçasıdır"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 2bb8370d23d9450fb8154f21c27817666fd7852c
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a>Azure şifreleme teknolojilerini: rest şifrelemeli kişisel verileri korumak

Bu makalede anlamak ve durağan verilerin güvenliğini sağlamak için Azure şifreleme teknolojilerini kullanmanıza yardımcı olur.

Kalan verileri şifrelemesini bir en iyi uygulama olarak hassas veya kişisel verileri korumak ve uyumluluk ve veri gizlilik gereksinimlerini karşılamak için gereklidir.
Bekleyen şifreleme saldırgan şifrelenmemiş erişimini engellemek için tasarlanmıştır, diskteki verileri sağlayarak veriler şifrelenir.

## <a name="scenario"></a>Senaryo 

Amerika Birleşik Devletleri'nde yönetim büyük seyahat şirket, İngiliz Adaları arasında yanı sıra Akdeniz ve Baltık seas masraflarını sunmaya işlemlerini genişletmektedir. Bu çalışmalarını desteklemek için daha küçük birkaç seyahat satırlarını İtalya, Almanya, Danimarka ve İngiltere göre aldı

Şirket Microsoft Azure bulutta şirket verilerini depolamak için kullanır. Bu müşteri ve/veya çalışan bilgiler gibi şunlar olabilir:

- adresleri
- Telefon numaraları
- Vergi kimlik numaraları
- Kredi kartı bilgileri

Şirket verileri ihtiyaç bu Departmanlar için erişilebilir olmasını sağlarken müşteri ve çalışan verilerin gizliliği korumanız gerekir. (bordro ve ayırmaları Departmanlar gibi)

Seyahat satır de geçerli ve geçmiş müşterilerle ilişkilerini izlemek için kişisel bilgi içeren büyük bir veritabanı ödül ve bağlılık programı üyelerinin tutar.

### <a name="problem-statement"></a>Sorun bildirimi

Şirket verileri (bordro ve ayırmaları Departmanlar gibi) ihtiyaç bu Departmanlar için erişilebilir olmasını sağlarken müşterilerin ve çalışanların kişisel verilerin gizliliği korumanız gerekir. Bu kişisel verileri şirket denetimli veri merkezi dışında depolanır ve şirketin fiziksel denetimi altında değil.

### <a name="company-goal"></a>Şirket hedefi

Çok katmanlı savunma güvenlik stratejisinin bir parçası, kişisel verileri içeren tüm veri kaynakları, bulut depolama alanında bulunan dahil olmak üzere şifrelendiğinden emin olmak için bir şirket hedeftir. Yetkisiz kişilerin kişisel verilere erişirse, okunamaz kılacak bir biçiminde olmalıdır. Şifreleme uygulama kolay veya – kullanıcılar ve Yöneticiler için saydam olmalıdır.

## <a name="solutions"></a>Çözümler

Birden çok Araçlar ve teknolojiler kalan kişisel verileri şifreleyerek korumanıza yardımcı olmak için Azure hizmetleri sağlar.

### <a name="azure-key-vault"></a>Azure Key Vault

[Azure anahtar kasası](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) Azure Hizmetleri, kalan verileri şifrelemek için kullanılan anahtarları için güvenli depolama sağlar ve önerilen anahtar depolama ve yönetimi çözümüdür. Şifreleme anahtar yönetimi, depolanan verilerin güvenliğini sağlamak için gereklidir.

#### <a name="how-do-i-use-azure-key-vault-to-protect-keys-that-encrypt-personal-data"></a>Kişisel veri şifreleme anahtarlarınızı korumak için Azure anahtar kasası nasıl kullanırım?

Azure anahtar kasası kullanmak için bir Azure hesabı için bir abonelik gerekiyor. Ayrıca Azure PowerShell yüklü gerekir. Aşağıdakileri yapmak için PowerShell cmdlet'leri kullanılarak adımları içerir:

1. Aboneliklerinize bağlanma

2. Bir anahtar kasası oluşturma

3. Anahtar kasasına bir anahtar veya gizli anahtar ekleme

4. Anahtar kasası Azure Active Directory ile kullanacağınız uygulamaları kaydetme

5. Anahtar veya gizli kullanmak için uygulamalar yetkilendirmek

Bir anahtar kasası oluşturmak için New-AzureRmKeyVault PowerShell cmdlet'ini kullanın. Kasa adı, kaynak grubu adı ve coğrafi konum atar. Diğer cmdlet'ler anahtarlarında yönetirken kasa adını kullanacaksınız. REST API'si aracılığıyla kasası kullanan uygulamaların URI kasası kullanır.

Azure anahtar kasası yazılımla korunan anahtardan sizin için sağlayabilir veya mevcut bir anahtarı içeri aktarabilirsiniz bir. PFX dosyası. Ayrıca, kasaya parolaları (Parolalar) depolayabilirsiniz.

Ayrıca, yerel HSM'NİZDE bir anahtar oluşturmak ve bunu Hsm'lerine anahtar kasası hizmetindeki HSM sınırını terk anahtar olmadan aktarabilirsiniz.

Azure anahtar kasası kullanma hakkında ayrıntılı yönergeleri için adımlarda [Azure anahtar kasası ile çalışmaya başlama.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)

Azure anahtar kasası ile kullanılan PowerShell cmdlet'leri listesi için bkz: [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

### <a name="azure-disk-encryption-for-windows"></a>Windows için Azure Disk şifrelemesi

[Azure Disk şifrelemesi for Windows ve Linux Iaas VM'ler](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) Azure sanal makinelerde rest kişisel verilerinizi korur ve Azure anahtar kasası ile tümleşir. Azure Disk şifrelemesi kullanan [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows ve [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux işletim sistemi ve veri diskleri şifrelemek için de. Azure Disk şifrelemesi, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 ve Windows 8 ve Windows 10 istemcileri desteklenir.

#### <a name="how-do-i-use-azure-disk-encryption-to-protect-personal-data"></a>Azure Disk şifrelemesi kişisel verilerinizi korumak için nasıl kullanırım?

Azure Disk Şifrelemesi'ni kullanmak için bir Azure hesabı için bir aboneliği gerekir. İçin Azure Disk şifrelemesi Windows ve Linux VM'ler etkinleştirmek için aşağıdakileri yapın:

1. Azure Disk şifrelemesi Resource Manager şablonu, PowerShell veya komut satırı arabirimi (CLI) disk şifrelemesi etkinleştirmek ve şifreleme yapılandırmasını belirtmek için kullanın. 

2. Anahtar Kasası'nı şifreleme malzeme okumak için Azure platformu izni verin.

3. Şifreleme anahtar malzemesini anahtar kasanızı yazmak için bir Azure Active Directory (AAD) uygulama kimliği sağlayın.

Azure VM ve anahtar kasası yapılandırmasını güncelleştirin ve şifrelenmiş VM'nizi ayarlayın.

Azure Disk şifrelemesi desteklemek için anahtar kasanızı ayarladığınızda, ek güvenlik için ve şifrelenmiş sanal makinelerinin desteklemek için bir anahtar şifreleme anahtarı (KEK) ekleyebilirsiniz.

![](media/protect-personal-data-at-rest/create-key.png)

Ayrıntılı yönergeler belirli dağıtım senaryoları ve kullanıcı deneyimleri için dahil edilmiştir [için Azure Disk şifrelemesi Windows ve Linux Iaas VM'ler.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)

### <a name="azure-storage-service-encryption"></a>Azure Depolama Hizmeti Şifrelemesi

[Azure Storage hizmeti şifreleme (SSE) bekleyen veri için](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) korumak ve Kuruluş güvenliği ve uyumluluğu taahhüt karşılamak için verilerinizi korumaya yardımcı olur. Azure depolama otomatik olarak depolama birimine devam ettirmeden önce 256 bit AES şifreleme kullanarak, verilerinizi şifreler ve alma önce şifresini çözer. Bu hizmet, Azure BLOB'ları ve dosyaları için kullanılabilir.

#### <a name="how-do-i-use-storage-service-encryption-to-protect-personal-data"></a>Depolama hizmeti şifrelemesi kişisel verilerinizi korumak için nasıl kullanırım?

Depolama hizmeti şifrelemesi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında oturum açın.

2. Bir depolama hesabı seçin.

3. Ayarları'nda, Blob hizmeti bölümü altında şifreleme seçin.

4. Dosya hizmeti bölümü altında şifreleme seçin.

Şifreleme ayarı tıkladıktan sonra etkinleştirme veya depolama hizmeti şifrelemesi devre dışı bırakabilirsiniz.

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

Yeni veri şifrelenir. Bu depolama hesabı var olan dosyalardaki veriler şifrelenmemiş kalır.

Veri şifreleme etkinleştirdikten sonra aşağıdaki yöntemlerden birini kullanarak depolama hesabına kopyalayın:

1. BLOB'ları veya dosyaları kopyalama [AzCopy komut satırı yardımcı programı](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).

2. [SMB kullanan bir dosya paylaşımını bağlama](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) dosyaları kopyalamak için Robocopy gibi bir yardımcı programını kullanabilirsiniz.

3. Depolama hesaplarını kullanarak blob veya dosya verileri için ve blob depolamadan veya arasında kopyalamak [depolama istemcisi kitaplıklarını .NET gibi](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).

4.  Kullanım bir [Depolama Gezgini](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) BLOB Depolama hesabınıza şifreleme ile karşıya yüklemek için.

### <a name="transparent-data-encryption"></a>Saydam Veri Şifrelemesi

Saydam veri şifreleme (TDE), SQL Azure veritabanı ve sunucu düzeyi verileri şifrelemek bir özelliktir. TDE, şimdi tüm yeni oluşturulan veritabanı üzerinde varsayılan olarak etkindir. TDE, gerçek zamanlı I/O şifreleme ve şifre çözme veri ve günlük dosyalarının gerçekleştirir.

#### <a name="how-do-i-use-tde-to-protect-personal-data"></a>TDE kişisel verilerinizi korumak için nasıl kullanırım?

REST API kullanarak veya PowerShell kullanarak Azure portalı üzerinden TDE yapılandırabilirsiniz. Azure Portalı'nı kullanarak var olan bir veritabanı üzerinde TDE etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında ziyaret <https://portal.azure.com> ve Azure yönetici veya katkıda hesabınız ile oturum açın.

2. Sol başlıktaki Gözat'ı tıklatın ve SQL veritabanları'ye tıklayın.

3. Sol bölmede seçili SQL veritabanları ile kullanıcı veritabanınız'ı tıklatın.

4. Veritabanı dikey penceresinde tüm ayarlar'ı tıklatın.

5. Ayarlar dikey penceresinde Saydam veri şifreleme bölümü saydam veri şifreleme dikey penceresini açmak için tıklatın.

6. Veri şifreleme dikey için veri şifreleme düğmesi taşıyın ve sonra (sayfanın en üstünde) ayarı uygulamak için Kaydet'i tıklatın. Şifreleme durumu saydam veri şifreleme ilerlemesini yaklaşık.

![Veri şifrelemeyi etkinleştirme](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

TDE etkinleştirmek yönergeler ve TDE korumalı veritabanları ve daha fazlasını çözme hakkında bilgi makalesinde bulunan [saydam veri şifrelemesi ile Azure SQL veritabanı.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)

## <a name="summary"></a>Özet

Şirket Azure bulutta depolanan kişisel verileri şifreleme kendi amacı gerçekleştirebilirsiniz. Tüm birimleri korumak için Azure Disk şifrelemesi kullanarak bunu. Bu hem işletim sistemi dosyalarını hem de kişisel olarak tanımlanabilir bilgileri ve diğer hassas verileri tutmak veri dosyaları içerebilir. Azure depolama hizmeti şifrelemesi, BLOB'lar ve dosyaları depolanan kişisel verilerinizi korumak için kullanılabilir. Azure SQL veritabanlarında depolanan veriler için saydam veri şifreleme yetkisiz Etkilenme kişisel bilgilerin kullanımına karşı koruma sağlar.

Azure verileri şifrelemek için kullanılan anahtarları korumak için şirket Azure anahtar kasası kullanabilirsiniz. Bu anahtar yönetimi işlemini kolaylaştırır ve şirket erişmek ve kişisel veri şifreleme anahtarları denetim kurmanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Disk şifrelemesi sorun giderme kılavuzu](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [Azure sanal Makine'yi şifreleme](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [Azure Data Lake Store'da verilerin şifrelenmesi](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [Bekleyen Azure Cosmos DB veritabanı şifreleme](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
