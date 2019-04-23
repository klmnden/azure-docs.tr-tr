---
title: -Azure anahtar kasası tümleştirme veya kendi anahtarını getir (BYOK) - TDE Azure SQL veritabanı | Microsoft Docs
description: SQL veritabanı ve veri ambarı için bilgisayarınızı kendi anahtarını (BYOK) desteği için saydam veri şifrelemesi (TDE) Azure anahtar kasası ile taşıyın. BYOK genel bakış, avantajlar, nasıl çalıştığını, ilgili önemli noktalar ve öneriler ile TDE.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 04/19/2019
ms.openlocfilehash: 6ad4cf251ad09adb7e1f11ebd42d7eab0d6a9183
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60006480"
---
# <a name="azure-sql-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault-bring-your-own-key-support"></a>Azure SQL saydam veri şifrelemesi ile Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar: Destek kendi anahtarını Getir

[Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) veritabanı şifreleme anahtarı (DEK) TDE koruyucusu olarak adlandırılan bir müşteri tarafından yönetilen asimetrik anahtarla şifrelemek için Azure Key Vault ile tümleştirme sağlar. Bu ayrıca genellikle için saydam veri şifrelemesi Getir bilgisayarınızı kendi anahtarını (BYOK olarak) destekleyecek şekilde adlandırılır.  BYOK senaryoda TDE koruyucusuna bir müşteriye ait içinde depolanır ve yönetilen [Azure anahtar kasası](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault), Azure'un bulut tabanlı dış anahtar yönetimi sistemi. TDE koruyucusuna olabilir [oluşturulan](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) anahtar kasası tarafından veya [aktarılan](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) üzerinde bir şirket içi HSM CİHAZDAN anahtar kasası. Bir veritabanının önyükleme sayfası üzerinde depolanan TDE DEK şifrelenir ve tarafından Azure anahtar, hiçbir zaman ayrılmaz Kasası'nda depolanan olan TDE koruyucusuna şifresi çözülür.  SQL veritabanı DEK şifrelemek ve şifresini çözmek için müşteriye ait anahtar kasasındaki izinleri verilmiş olması gerekir. Mantıksal SQL sunucusu için anahtar kasası izinlerini iptal erişilemez bir veritabanı ve tüm veriler şifrelenir. Azure SQL veritabanı için TDE koruyucusu mantıksal SQL sunucusu düzeyinde ayarlanır ve bu sunucuyla ilişkili tüm veritabanları tarafından devralınır. İçin [Azure SQL yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-howto-managed-instance)TDE koruyucusuna örnek düzeyinde ayarlanır ve tümü tarafından devralınır *şifrelenmiş* bu örneğindeki veritabanları. Terim *sunucu* hem sunucusu ve örneği bu belge boyunca farklı belirtilmedikçe ifade eder.

> [!NOTE]
> Saydam veri şifrelemesi ile Azure SQL veritabanı yönetilen örneği için Azure anahtar kasası tümleştirme (kendi anahtarını Getir) önizleme aşamasındadır.

Azure Key Vault tümleştirmesi sayesinde TDE ile kullanıcılar anahtar devirlerini, anahtar kasası izinlerini, anahtar yedeklemelerini de dahil olmak üzere önemli yönetim görevlerinin denetlemek ve Azure anahtar kasası işlevini kullanarak tüm TDE koruyucusu üzerinde denetim/raporlamayı etkinleştirmek. Key Vault anahtar Merkezi Yönetimi sağlayan, sıkı bir şekilde izlenen donanım güvenlik modülleri (HSM'ler) yararlanır ve güvenlik ilkeleriyle uyumluluğunu karşılamanıza yardımcı olmak üzere anahtar yönetimi ve veri arasında görev ayrımı sağlar.  

Azure Key Vault tümleştirmesi sayesinde TDE aşağıdaki avantajları sağlar:

- Saydamlık ve ayrıntılı denetim TDE koruyucusuna Self yönetme olanağı artırdık.
- Dilediğiniz zaman veritabanı erişilemeyen işlemek için izinlerini iptal etme olanağı
- TDE koruyucusu (birlikte, diğer anahtarlar ve gizli dizileri diğer Azure hizmetlerini kullandı) Merkezi Yönetimi tarafından bunları anahtar Kasası'nda barındırma
- Görev ayrımı desteklemek için kuruluş içindeki anahtar ve veri yönetim sorumluluk ayrımı
- Daha fazla güven Microsoft bakın veya herhangi bir şifreleme anahtarı ayıklamak için Key Vault tasarlandığından kendi istemcilerden.
- Anahtar döndürme desteği

> [!IMPORTANT]
> Kullananlar için Key Vault kullanmaya başlamak isteyen, hizmet tarafından yönetilen TDE, TDE Key vault'taki TDE koruyucusu geçiş işlemi sırasında etkin durumda kalır. Kapalı kalma süresi ya da veritabanı dosyalarını yeniden şifrelenmesi yoktur. Bir hizmetle yönetilen anahtarı için bir Key Vault anahtar geçişi, yalnızca yeniden şifreleme hızlı ve çevrimiçi bir işlem olan veritabanı şifreleme anahtarı (DEK) gerektirir.

## <a name="how-does-tde-with-azure-key-vault-integration-support-work"></a>Azure Key Vault tümleştirmesi sayesinde TDE iş nasıl destekler

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

![Anahtar kasası için sunucu kimlik doğrulaması](./media/transparent-data-encryption-byok-azure-sql/tde-byok-server-authentication-flow.PNG)

TDE varsayılan TDE koruyucusu Key vault'tan kullanmak için yapılandırıldığında, sunucu DEK her TDE etkin veritabanının anahtar Kasası'na kaydırma anahtar isteği gönderir. Key Vault kullanıcı veritabanında depolanan şifrelenmiş veritabanı şifreleme anahtarını döndürür.  

> [!IMPORTANT]
> Dikkat etmeniz önemlidir **TDE koruyucusu, Azure Key Vault'ta depolandıktan sonra hiçbir zaman Azure anahtar kasası ayrılmaz**. Sunucu yalnızca TDE koruyucusu anahtar malzemesi içinde Key Vault için anahtar işlemi istek gönderebilirsiniz ve **hiçbir zaman erişmez veya TDE koruyucusuna önbelleğe alır**. Anahtar kasası Yöneticisi herhangi bir noktada sunucunun Key Vault izinlerini iptal etme hakkına sahiptir, bu sunucuya tüm bağlantıları durumda kesilir.

## <a name="guidelines-for-configuring-tde-with-azure-key-vault"></a>Azure anahtar kasası ile TDE yapılandırma yönergeleri

### <a name="general-guidelines"></a>Genel Yönergeler

- Azure anahtar kasası ve Azure SQL veritabanı/yönetilen örnek seçeceğiz aynı kiracıda olmasını sağlamak.  Kiracılar arası anahtar kasası ve sunucu etkileşimleri **desteklenmez**.
- Hangi abonelikler gerekli kaynaklar için kullanılan karar verme – sunucusunu daha sonra farklı abonelikler arasında taşıma BYOKs ile TDE, yeni bir kurulum gerektirir. Daha fazla bilgi edinin [kaynaklar taşınıyor](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- Azure anahtar kasası ile TDE yapılandırırken anahtar kasasında yinelenen sarmalama/kaydırma işlemleri tarafından yerleştirilen yük dikkate almak önemlidir. Örneğin, kasa beklenmediğini karşı çoğu anahtar işlemleri sunucu veritabanlarında olduğu gibi bir SQL veritabanı sunucusu ile ilişkili tüm veritabanlarını aynı TDE koruyucusuna kullandığından, o sunucunun bir yük devretme tetikler. Deneyimimizi üzerinde temel alır ve belgelenen [anahtar kasası hizmet sınırları](https://docs.microsoft.com/azure/key-vault/key-vault-service-limits), en fazla 500 standart ilişkilendirme öneririz / genel amaçlı veya 200 Premium / iş açısından kritik veritabanları bir Azure anahtar sağlamak için kasası ile tek bir abonelikte vault'taki TDE koruyucusuna erişilirken sürekli olarak yüksek kullanılabilirlik.
- Önerilen: Şirket içi TDE koruyucusuna bir kopyasını tutun.  Bu, bir HSM cihazını TDE koruyucusu yerel olarak oluşturmak için ve biri sonucunda TDE koruyucusuna yerel bir kopyasını depolamak için bir anahtar emanet sistemi gerektirir.  Bilgi [bir anahtar yerel HSM'NİZDEN Azure anahtar Kasası'na aktarma](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys).

### <a name="guidelines-for-configuring-azure-key-vault"></a>Azure anahtar Kasası'nı yapılandırma yönergeleri

- Key vault ile oluşturma [geçici silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete) veri kaybı durumunda yanlışlıkla anahtarı – veya anahtar kasası – silinmeye karşı korunmasına yönelik etkin.  Kullanmalısınız ["geçici silme" özelliğini etkinleştirmek için PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) anahtar kasasındaki (Bu seçenek kullanılamaz AKV portaldan henüz – Azure SQL tarafından ancak gerekli):  
  - Kurtarılamaz veya temizleneceği sürece geçici silinen kaynakları belirli bir süre süreyi 90 gün saklanır.
  - **Kurtarmak** ve **Temizleme** eylemleri bir anahtar kasası erişim ilkesini ilişkili kendi izinlere sahiptir.
- Anahtar kasasındaki kimlerin kritik bu kaynağı silmek ve yetkilendirilmemiş veya yanlışlıkla silinmesini engellemek için bir kaynak kilidi ayarlayın.  [Kaynak kilitleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources)

- SQL veritabanı sunucusu, Azure Active Directory (Azure AD) kimliğini kullanarak anahtar kasası erişim.  Portal kullanıcı arabirimini kullanarak, Azure AD kimlik otomatik olarak oluşturulur ve sunucuya anahtar kasası erişim izni verilir.  BYOK ile TDE yapılandırmak için PowerShell kullanarak Azure AD kimlik oluşturulmalı ve tamamlanma doğrulanmalıdır. Bkz: [BYOK ile TDE yapılandırma](transparent-data-encryption-byok-azure-sql-configure.md) ve [yönetilen örneği için BYOK ile TDE yapılandırma](https://aka.ms/sqlmibyoktdepowershell) PowerShell kullanırken ayrıntılı adım adım yönergeler için.

  > [!NOTE]
  > Varsa Azure AD kimlik **olan yanlışlıkla silinmiş veya sunucunun izinler iptal** anahtar kasasının erişim ilkesi kullanarak sunucu anahtar kasası erişimi kaybeder ve şifrelenmiş TDE veritabanlarıdır erişilemez 24 saat içinde.

- Güvenlik duvarları ve sanal ağlar ile Azure Key Vault kullanırken aşağıdakileri yapılandırmanız gerekir: 
  - Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını geçmesine izin ver: Evet seçti

  > [!NOTE]
  > TDE şifrelenmiş güvenlik duvarı atlama olamaz çünkü SQL veritabanları anahtar kasası erişimi kaybedersiniz, veritabanlarını 24 saat içinde erişilebilir durumda değil.

- Denetim ve tüm şifreleme anahtarlarını raporlama etkinleştir: Key Vault, diğer güvenlik bilgileri ve Olay yönetimi (SIEM) araçları eklenmek üzere kolay günlüklerini sunar. Operations Management Suite (OMS) [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-key-vault) zaten tümleşik bir hizmet örneğidir.
- Şifreli veritabanlarına yüksek kullanılabilirlik sağlamak için farklı bölgelerde bulunan iki Azure anahtar kasaları her SQL veritabanı sunucusunu yapılandırın.

### <a name="guidelines-for-configuring-the-tde-protector-asymmetric-key"></a>(Asimetrik anahtar) TDE koruyucusuna yapılandırma yönergeleri

- Şifreleme anahtarınızın yerel bir HSM cihazda yerel olarak oluşturun. Azure anahtar Kasası'nda storable, bu nedenle bu asimetrik bir RSA 2048 anahtarı olduğundan emin olun.
- Anahtarı bir anahtar emanet sistemde tutmak.  
- Şifreleme anahtarı dosyasını (.pfx, .byok veya .backup) Azure anahtar Kasası'na içeri aktarın.

   > [!NOTE]
   > Bu anahtar kalacakları ancak özel anahtar, anahtar kasası hiçbir zaman bırakabilirsiniz test amacıyla, Azure Key Vault, bir anahtar oluşturmak mümkün olmasıdır.  Her zaman yedeklenir ve tutmak anahtarının zarar üretim verileri şifrelemek için kullanılan anahtarları (yanlışlıkla silinmesine anahtar kasası, sona erme vb.) kalıcı veri kaybına neden olur.

- Bir anahtar olmadan bir sona erme tarihi – kullanın ve kullanılmakta olan bir anahtar üzerinde bir sona erme tarihi ayarlamanız gerekmez: **anahtarın süresi dolduktan sonra şifreli veritabanlarına kendi TDE koruyucusuna erişimini kaybeder ve 24 saat içinde erişilemeyen**.
- Anahtar etkinleştirilir ve gerçekleştirmek için izinlere sahip olun *alma*, *anahtarı sarmalama*, ve *anahtarı kaydırma* operations.
- Anahtar Azure anahtar Kasası'nda ilk kez kullanmadan önce bir Azure Key Vault anahtarı yedekleme oluşturun. Daha fazla bilgi edinin [yedekleme AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey) komutu.
- Anahtar herhangi bir değişiklik yapıldığında, yeni bir yedekleme oluşturun (örneğin, ACL'ler, Ekle etiketler ekleyin, anahtar öznitelik ekleyin).
- **Önceki sürümlerini** anahtarı anahtar kasasındaki anahtarları döndürürken, bu nedenle daha eski bir veritabanı yedeklerini geri yükleyebilirsiniz. Ne zaman TDE koruyucusuna değiştirildiğinde bir veritabanı için veritabanı eski yedeklemeler **güncelleştirilmez** son TDE koruyucusuna kullanılacak.  Her yedekleme, geri yükleme sırasında birlikte oluşturulduğu TDE koruyucusuna gerekir. Anahtar devirlerini yönergeleri izleyerek gerçekleştirilebilir [saydam veri şifrelemesi koruyucu PowerShell kullanarak döndürme](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Tüm daha önce kullanılan anahtarlar geri hizmet tarafından yönetilen anahtarlar için değiştirdikten sonra Azure anahtar Kasası'nda tutun.  Bu, Azure Key Vault'ta depolanan TDE koruyucusu ile veritabanı yedeklerini geri sağlar.  TDE koruyucusu Azure anahtar kasası ile oluşturulan tüm depolanan yedeklemeler, hizmet tarafından yönetilen anahtarlarla oluşturulmuş kadar tutulması gerekir.  
- Kurtarılabilir yedek kopyalarını kullanarak bu anahtarların [yedekleme AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey).
- Veri kaybı olmadan bir güvenlik olayı sırasında riskli olabilecek bir anahtar kaldırmak için adımları izleyin. [riskli olabilecek bir anahtarı Kaldır](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md).

## <a name="high-availability-geo-replication-and-backup--restore"></a>Yüksek kullanılabilirlik, coğrafi çoğaltma ve yedekleme / geri yükle

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Azure anahtar kasası ile yüksek kullanılabilirlik yapılandırma veritabanı ve SQL veritabanı sunucu yapılandırmasına bağlıdır ve iki farklı durumlar için önerilen yapılandırmaları şunlardır.  İlk tek başına veritabanı veya SQL veritabanı sunucusu yapılandırılan coğrafi artıklık ile bir durumdur.  İkinci bir veritabanı veya SQL veritabanı sunucusu yük devretme grupları veya coğrafi yedeklilik ile yapılandırılmış Burada, her coğrafi olarak yedekli kopyalar için coğrafi yük devretme iş emin olmak için yük devretme grubu içinde yerel bir Azure Key Vault olduğunu güvence altına gereken bir durumdur.

Bir veritabanı ve SQL veritabanı sunucusu yok yapılandırılan coğrafi yedeklilik, yüksek kullanılabilirlik gerekiyorsa bu durumda, aynı anahtar malzemesi ile iki farklı bölgelerde iki farklı anahtar kasalarını kullanılacak sunucuyu yapılandırmak için önerilir. Bu bir TDE koruyucusu SQL veritabanı sunucusu ile aynı bölgede birlikte bulunan bir birincil anahtar kasasını kullanarak oluşturarak gerçekleştirilebilir ve birincil sunucunun ikinci bir anahtar kasasına erişim sahip olacak şekilde, anahtarı bir anahtar kasası farklı bir Azure bölgesinde içerisine kopyalanıyor gerekir anahtar kasası, veritabanının hazır ve çalışır durumda sırasında kesinti karşılaşırsınız. Anahtarı şifreli biçimde birincil anahtar kasasından almak ve ardından geri yükleme-AzKeyVaultKey cmdlet'ini kullanın ve bir anahtar kasası ikinci bir bölgede belirtmek için yedekleme AzKeyVaultKey cmdlet'ini kullanın.

![Tek sunuculu HA ve coğrafi-dr](./media/transparent-data-encryption-byok-azure-sql/SingleServer_HA_Config.PNG)

## <a name="how-to-configure-geo-dr-with-azure-key-vault"></a>Azure anahtar kasası ile coğrafi-DR yapılandırma

Şifrelenmiş veritabanları için TDE koruyucusu, yüksek kullanılabilirliği sürdürmek için istenen ya da mevcut SQL veritabanı yük devretme grupları veya etkin coğrafi çoğaltma örnekleri temel alan yedekli Azure anahtar kasaları yapılandırmak için gereklidir.  Aynı Azure bölgesinde sunucusuyla birlikte bulunan ayrı bir anahtar kasası, her coğrafi olarak çoğaltılmış bir sunucu gerektirir. Birincil veritabanı bir bölgede kesinti nedeniyle erişilemez duruma gelir ve bir yük devretme tetiklenir, ikincil veritabanı ikincil anahtar kasasını kullanarak konuşturabilirsiniz kuramıyor.

Geo-Replicated Azure SQL veritabanları için aşağıdaki Azure Key Vault yapılandırması gereklidir:

- Bir bölge ve bir ikincil veritabanı key Vault bölgede bir anahtar kasası ile bir birincil veritabanı.
- En az bir ikincil gerekli, en fazla dört ikinciller desteklenir.
- (Zincirleme) ikincil veritabanı, ikincil veritabanı desteklenmez.

Aşağıdaki bölümde, ayrıntılı Kurulum ve yapılandırma adımları üzerinden geçer.

### <a name="azure-key-vault-configuration-steps"></a>Azure anahtar kasası yapılandırma adımları

- Yükleme [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)
- Kullanarak iki farklı bölgelerde iki Azure anahtar kasaları oluşturma ["geçici silme" özelliğini etkinleştirmek için PowerShell](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) (Bu seçenek kullanılamaz AKV portalından henüz – ancak SQL tarafından gerekli) anahtar kasası üzerinde.
- Hem Azure anahtar kasaları Azure Coğrafyada çalışmak için yedekleme ve geri yükleme anahtarları için sırada bulunan iki bölgede bulunması gerekir.  SQL Geo-DR gereksinimlerini karşılamak için izleyin, farklı bölgelerde bulunması için iki anahtar kasalarını gerekiyorsa [BYOK işlem](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) bir şirket içi HSM'NİZDEN içeri aktarılacak anahtarlar sağlar.
- İlk anahtar Kasası'nda yeni bir anahtar oluşturun:  
  - RSA/RSA-HSA 2048 anahtarı
  - Hiçbir sona erme tarihleri
  - Anahtar etkin ve anahtar işlemleri sarmalamadan çıkarma get gerçekleştirmek ve anahtarı sarmalama için izinlere sahip
- Birincil anahtarı yedeklemek ve anahtar ikinci anahtar Kasası'na geri yükleyin.  Bkz: [BackupAzureKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/backup-azkeyvaultkey) ve [geri yükleme-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/restore-azkeyvaultkey).

### <a name="azure-sql-database-configuration-steps"></a>Azure SQL veritabanı yapılandırma adımları

Aşağıdaki yapılandırma adımlarını yeni bir SQL dağıtımı başlatılıyor veya zaten var olan bir SQL Geo-DR dağıtım ile çalışıyorsanız, farklı.  Biz öncelikle yeni bir dağıtım için yapılandırma adımlarını özetler ve daha sonra kurulan bir coğrafi-DR bağlantısı zaten var. mevcut bir dağıtımı için Azure Key Vault'ta depolanan TDE koruyucusu atama açıklar.

**Yeni bir dağıtım adımları**:

- Önceden oluşturulmuş bir anahtar kasası ile aynı iki bölgede, iki SQL veritabanı sunucusu oluşturun.
- SQL veritabanı sunucusu TDE bölmesinde seçin ve her SQL veritabanı sunucusu için:  
  - Aynı bölgede AKV seçin
  - TDE koruyucusu olarak kullanılacak anahtarı seçin – her sunucunun TDE koruyucusu, yerel kopyayı kullanır.
  - Portalda bunu oluşturacak bir [AppID](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) – anahtar kasasına erişmek için SQL veritabanı sunucusu izinleri atamak için kullanılan SQL veritabanı sunucusu için bu kimlik silmeyin. Azure anahtar Kasası'nda izinleri yerine anahtar kasasına erişmek için SQL veritabanı sunucusu izinleri atamak için kullanılan SQL veritabanı sunucusu için kaldırma tarafından erişim iptal edilebilir.
- Birincil veritabanı oluşturun.
- İzleyin [etkin coğrafi çoğaltma Kılavuzu](sql-database-geo-replication-overview.md) senaryoyu tamamlamak için bu adım, ikincil veritabanı oluşturur.

![Yük devretme grupları ve coğrafi-dr](./media/transparent-data-encryption-byok-azure-sql/Geo_DR_Config.PNG)

> [!NOTE]
> Aynı TDE koruyucusu veritabanları arasındaki coğrafi-bağlantı kurmak için devam etmeden önce her iki anahtar Kasası'nda mevcut olduğundan emin olmak önemlidir.

**Mevcut bir SQL DB ile coğrafi-DR dağıtım adımları**:

SQL veritabanı sunucusu zaten var ve birincil ve ikincil veritabanları zaten atanmış olduğundan, Azure Key Vault yapılandırma adımları aşağıdaki sırayla gerçekleştirilmesi gerekir:

- İkincil veritabanını barındıran SQL veritabanı sunucusuyla başlayın:
  - Aynı bölgede yer alan anahtar kasası atama
  - TDE koruyucusuna atayın
- Artık birincil veritabanını barındıran SQL veritabanı sunucusuna gidin:
  - Aynı TDE koruyucusu kullanılan için ikincil bir veritabanı seçin

![Yük devretme grupları ve coğrafi-dr](./media/transparent-data-encryption-byok-azure-sql/geo_DR_ex_config.PNG)

>[!NOTE]
>Anahtar kasası sunucuya atarken, ikincil sunucu ile başlamanız gerekir.  Anahtar kasası birincil sunucuya ve sonucunda TDE koruyucusuna güncelleştirme ikinci adım Ata içinde Geo-DR bağlantı çoğaltılmış veritabanı tarafından kullanılan TDE koruyucusuna bu noktada her iki sunucuda da kullanılabilir olduğu için çalışmaya devam eder.

SQL veritabanı coğrafi-DR senaryosu için Azure Key vault'taki anahtarları yönetilen müşteri ile TDE etkinleştiriliyor önce oluşturmak ve SQL Database coğrafi çoğaltma için kullanılan aynı bölgede aynı içeriğe sahip iki Azure anahtar kasaları korumak önemlidir.  "Özdeş içeriğe" özellikle her iki sunucuyu tüm veritabanları tarafından TDE koruyucusu kullanımı erişimi, her iki anahtar kasalarını aynı TDE Protector(s) kopyalarını içermelidir anlamına gelir.  Bundan sonra hangi anahtar döndürme sonra TDE koruyucusu aynı kopyasını içermelidir anlamına gelir, günlük dosyaları için kullanılan anahtarları eski sürümlerini korumak her iki anahtar kasalarını eşitlenmiş olarak tutmak gereklidir veya yedeklemeler, TDE koruyucusu, aynı anahtar özellikler ve anahtarı olması gerekir kasaları SQL aynı erişim izinlerini sürdürmeniz gerekir.  

Bağlantısındaki [etkin coğrafi Çoğaltmaya genel bakış](sql-database-geo-replication-overview.md) sınamak ve bir yük devretmeyi tetiklemek için her iki anahtar kasalarına SQL için erişim izinlerini doğrulamak için düzenli aralıklarla yapılması gerektiğini tutulur.

### <a name="backup-and-restore"></a>Yedekleme ve Geri Yükleme

Bir veritabanı TDE kullanarak Key vault'tan bir anahtar ile şifrelenmiş sonra oluşturulan tüm yedeklemeleri de aynı TDE koruyucusu ile şifrelenir.

Key vault'tan TDE koruyucusu ile şifrelenmiş bir yedeklemeyi geri yüklemek için anahtar malzemesi özgün anahtar adı altında hala özgün kasadaki olduğundan emin olun. Ne zaman TDE koruyucusuna değiştirildiğinde bir veritabanı için veritabanı eski yedeklemeler **değil** son TDE koruyucusu kullanacak şekilde güncelleştirildi. Bu nedenle, veritabanı yedeklerini geri yüklenebilmesi için anahtar Kasası'nda tüm eski sürümlerini biri sonucunda TDE koruyucusuna tutmanızı öneririz.

Bir yedekleme geri yüklemek için gerekebilecek bir anahtar artık özgün, anahtar Kasası'nda ise, aşağıdaki hata iletisini döndürülür: "Hedef sunucusuna `<Servername>` < zaman damgası #1 > oluşturduğunuz tüm AKV URI'ler için erişimi yok ve < zaman damgası #2 >. Lütfen tüm AKV bir URI'leri geri yükledikten sonra işlemi yeniden deneyin."

Bunu azaltmak için çalıştırma [Get-AzSqlServerKeyVaultKey](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) cmdlet'ini (bir kullanıcı tarafından Silindiklerini sürece) sunucuya eklenmiş olan bir anahtar Kasası'ndaki anahtarları listesini döndürür. Tüm yedeklemeler geri emin olmak için hedef sunucu yedekleme için tüm bu anahtarları erişim olduğundan emin olun.

```powershell
Get-AzSqlServerKeyVaultKey `
  -ServerName <LogicalServerName> `
  -ResourceGroup <SQLDatabaseResourceGroupName>
```

SQL veritabanı için yedekleme kurtarma hakkında daha fazla bilgi edinmek için [bir Azure SQL veritabanını kurtarma](sql-database-recovery-using-backups.md). SQL veri ambarı için yedekleme kurtarma hakkında daha fazla bilgi edinmek için [bir Azure SQL veri ambarı kurtarmak](../sql-data-warehouse/backup-and-restore.md).

Sağlayabildiği için günlük dosyaları desteklenir: Dosyaları TDE koruyucusuna döndürülmüş ve yeni TDE koruyucusu kullanarak artık veritabanı bile özgün TDE Şifreleyici ile şifrelenmiş kalır günlüğünü desteklenir.  Geri yükleme sırasında veritabanını geri yüklemek için her iki anahtar da gerekecektir.  Azure Key Vault'ta depolanan bir TDE koruyucusu günlük dosyası kullanıyorsanız, hizmetle yönetilen TDE sırada kullanmak için veritabanı değiştirilmiş olsa bile bu anahtarı geri yükleme sırasında gerekli olacaktır.
