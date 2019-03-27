---
title: Güvenliğini sağlama ve iş yüklerini yönetmek için en iyi uygulamalar için Azure geçişi | Microsoft Docs
description: Azure'a geçirdikten sonra çalıştırma, yönetme ve geçirilen iş yüklerinizi güvenliğini sağlamak için en iyi alın.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: raynew
ms.openlocfilehash: afcd180146bc349bda9375f10eb56f85f67ccb52
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58498746"
---
# <a name="best-practices-for-securing-and-managing-workloads-migrated-to-azure"></a>Güvenliğini sağlama ve iş yüklerini yönetmek için en iyi uygulamaları için Azure geçişi

Plan ve tasarım geçiş kendisi hakkında düşünmeye ek olarak, geçiş için geçiş sonrasında Azure güvenliği ve yönetimi, modelinizde dikkate almanız gerekir. Bu makalede, dağıtımınızı çalıştıran en üst düzeyde tutmaya devam eden görevlerin yanı sıra, geçişinden sonra Azure dağıtımınızın güvenliğini sağlamak için planlama ve en iyi yöntemleri açıklar. 

> [!IMPORTANT]
> En iyi yöntemler ve bu makalede açıklanan fikirlerini Azure platformunda dayalıdır ve Özellikler makalenin yazıldığı sırada hizmet. Özellikler ve yetenekler zamanla değişir.

## <a name="secure-migrated-workloads"></a>Geçişi yapılan iş yüklerinin güvenliğini sağlama

Geçişten sonra en kritik görev iç ve dış tehditlere karşı geçişi yapılan iş yüklerinin güvenliğini sağlamak için uygundur. Bu en iyi uygulamaları, bunu yapmak için Yardım:

- [Azure Güvenlik Merkezi ile iş](#best-practice-follow-azure-security-center-recommendations): İzleme, değerlendirmeler ve Azure Güvenlik Merkezi tarafından sağlanan öneriler ile çalışma hakkında bilgi edinin
- [Verilerinizi şifrelemek](#best-practice-encrypt-data): Azure'da verilerinizi şifrelemek için en iyi alın.
- [Kötü amaçlı yazılımdan koruma ' ayarlamak](#best-practice-protect-vms-with-antimalware): Vm'lerinizi kötü amaçlı yazılım ve kötü amaçlı saldırılara karşı koruyun.
- [Web uygulamalarının güvenliğini sağlama](#best-practice-secure-web-apps): Hassas bilgileri geçirilen web apps'te güvenli tutun.
- [Gözden geçirin, abonelikleri](#best-practice-review-subscriptions-and-resource-permissions): Azure Abonelikleriniz ve kaynak geçişten sonra erişebilecek kişileri doğrulayın.
- [İş günlükleri ile](#best-practice-review-audit-and-security-logs): Azure denetim ve güvenlik günlüklerini düzenli olarak gözden geçirin.
- [Diğer güvenlik özellikleri gözden](#best-practice-evaluate-other-security-features): Anlama ve Azure'un sunduğu Gelişmiş güvenlik özellikleri değerlendirin.

## <a name="best-practice-follow-azure-security-center-recommendations"></a>En iyi yöntem: Azure Güvenlik Merkezi önerilerini izleyin

Microsoft Azure Kiracı yöneticileri, iş yüklerini saldırılarına karşı güvenlik özelliklerini etkinleştirmek için gereken bilgileri olmasını sağlamak için sabit çalışır.  Azure Güvenlik Merkezi Birleşik güvenlik yönetimi sağlar. Güvenlik Merkezi'nden iş yüklerinde güvenlik ilkeleri uygulayın, tehdit kullanıma sunmayı denetlememize, sınırlama ve algılayabilir ve saldırılara karşılık vermek. Güvenlik Merkezi, kaynakları ve yapılandırmaları Azure kiracılar genelinde analiz eder ve dahil olmak üzere, güvenlik önerileri sağlar:

- **Merkezi ilke yönetimi** – Tüm hibrit bulut iş yüklerinizde güvenlik ilkelerini merkezi şekilde yöneterek şirket veya yasal güvenlik gereksinimleriyle uyumluluk sağlayın.
- **Sürekli güvenlik değerlendirmesi** – Olası güvenlik sorunlarını keşfetmek için makinelerin, ağların, depolama ve veri hizmetlerinin güvenlik durumunu izleyin.
- **Eyleme dönüştürülebilir öneriler** – Önceliği belirlenmiş ve eyleme dönüştürülebilir güvenlik önerileri sayesinde saldırganlar yararlanmadan önce güvenlik açıklarını düzeltin.
- **Önceliği belirlenmiş uyarılar ve olaylar** - Önceliği belirlenmiş güvenlik uyarıları ve olaylar sayesinde öncelikle en kritik tehditlere odaklanın.

Değerlendirmesi ve öneriler ek olarak, Güvenlik Merkezi birkaç belirli kaynaklar için etkin hale getirilebilir diğer güvenlik özellikleri sağlar.

- **Yalnızca zamanında (JIT) erişim**: Zamanında, denetimli erişim Azure vm'lerdeki yönetim bağlantı noktalarına ağ saldırı yüzeyinizi azaltmak.
    - VM RDP bağlantı noktası 3389 Internet'e açık olan VM'ler için sürekli kötü bir aktör etkinlik kullanıma sunar. Azure IP adresleri iyi bilinen ve bilgisayar korsanlarının sürekli olarak bunları için 3389 bağlantı noktalarını açma saldırıları araştırma. 
    - Yalnızca zaman kullanan ağ güvenlik grupları (Nsg'ler) ve gelen kuralları bu sınırı belirli bir bağlantı noktası açık olduğu süre miktarı.
    - İle tam zamanında etkinleştirildiğinde Güvenlik Merkezi bir kullanıcının bir VM için rol tabanlı erişim denetimi (RBAC) yazma erişim izinleri olduğunu denetler. Ayrıca, kullanıcıların sanal makinelerine nasıl bağlayabileceğini kurallarını belirtin. Tamam izinleri olan bir erişim isteği Onaylandı ve süre Seçili bağlantı noktalarına gelen trafiğe izin veren Nsg Güvenlik Merkezi'ni yapılandırır, belirtin. Süre dolduğunda Nsg'ler önceki durumuna geri dönüş.
- **Uyarlamalı uygulama denetimleri**: Dinamik uygulama beyaz listesini, yazılım ve hangi uygulamaların çalıştırılmasına denetleme tarafından kötü amaçlı yazılım VM'ler kapalı kullanmaya devam edin.
    - Uyarlamalı uygulama denetimleri için beyaz liste uygulamalara izin ver ve sahte kullanıcıların ve yöneticilerin Vm'lerinizde onaylanmamış veya güvenlik incelemesi yazılım uygulamalarını yüklenmesini önlemek.
    - Engelleyebilir veya kötü amaçlı uygulamalar çalıştırmak, istenmeyen veya kötü amaçlı uygulamalar önlemek ve kuruluşunuzun uygulama güvenlik ilkesi ile uyum sağlamak uyarı çalışır.
- **Dosya bütünlüğünü izleme**: Vm'lerde çalışan dosyalarının bütünlüğünü emin olun.
    - VM sorunlara neden yazılımı yüklemeniz gerekmez.  Bir sistem dosyasını değiştirerek VM hata veya performans düşüşüne neden olabilir.  İzleme sistem dosyaları ve değişiklikler için kayıt defteri ayarları inceler ve güncelleştirilmiş şey olduğunda size bildirir bütünlüğü dosyası.
    - Güvenlik Merkezi önerir, bir dosyaları izlemeniz gerekir.


**Daha fazla bilgi edinin:**

- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-intro) Azure Güvenlik Merkezi hakkında.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) hakkında tam zamanında VM erişimini.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application) Uyarlamalı uygulama denetimleri.
- [Başlama](https://docs.microsoft.com/azure/security-center/security-center-file-integrity-monitoring) dosya bütünlüğünü izleme ile.


## <a name="best-practice-encrypt-data"></a>En iyi yöntem: Verileri şifreleme 

Şifreleme, Azure güvenlik uygulamaları önemli bir parçasıdır. Düzeyleri şifrelemenin hiç etkin sağlamaya yardımcı olur yetkisiz taraflar aktarımda ve bekleme sırasında veriler dahil olmak üzere, hassas verilere erişmesini engellemeye. 

### <a name="encryption-for-iaas"></a>Iaas için şifreleme

- **Vm'leri**: VM'ler için Azure Disk şifrelemesi, Windows ve Linux Iaas sanal makine disklerini şifrelemek için kullanabilirsiniz.
    - Disk şifrelemesi işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için BitLocker'ı Windows ve Linux için DM-Crypt'ten yararlanır.
    - Azure tarafından oluşturulan bir şifreleme anahtarı kullanabilir veya Azure anahtar Kasası'nda korunması kendi şifreleme anahtarlarınızı sağlayabilirsiniz. 
    - Disk şifrelemesi ile bekleyen (diskte) Iaas VM verileri güvenli ve VM önyükleme sırasında. 
    - Şifrelenmiş olmayan Vm'leriniz varsa azure Güvenlik Merkezi sizi uyarır.
- **Depolama**: Azure Depolama'da depolanan rest veri koruyun.
    - Azure depolama hesaplarında depolanan veriler, FIPS 140-2 ile uyumlu olan Microsoft tarafından oluşturulan AES anahtarları kullanılarak şifrelenebilir veya kendi anahtarlarınızı kullanabilirsiniz.
    - Depolama hizmeti şifrelemesi için tüm yeni ve var olan depolama hesapları etkinleştirilir ve devre dışı bırakılamaz.


### <a name="encryption-for-paas"></a>PaaS için şifreleme

Iaas, kendi Vm'leri ve altyapı yönettiğiniz, içinde bir PaaS modeli platform ve altyapı yönetilir çekirdek uygulama mantığı ve yeteneklerini odaklanmanızı bırakarak sağlayıcısı. Çok sayıda farklı türde PaaS Hizmetleri ile her hizmetin güvenlik gerekçesiyle istemesi ayrı ayrı değerlendirilir. Örneğin, size Azure SQL veritabanı için şifreleme nasıl etkinleştirebileceğiniz görelim.

- **Her zaman şifreli**: Bekleyen verileri korumak için SQL Server Management Studio'da her zaman şifreli sihirbazını kullanın.
    - Tek tek sütun verileri şifrelemek için Always Encrypted anahtar oluşturun.
    - Her zaman şifreli anahtarları veritabanı meta verilerinde şifrelenmiş olarak depolanan veya Azure Key Vault gibi güvenilen anahtar depolarında depolanır.
    - Uygulama değişikliklerini büyük olasılıkla bu özelliği kullanmak için gerekli olacaktır.
- **Saydam veri şifrelemesi (TDE)**: Azure SQL veritabanı ile gerçek zamanlı şifreleme ve şifre çözme, veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları, bekleyen koruyun.
    - TDE uygulama katmanında bir değişiklik yapmadan gerçekleşmesi şifreleme etkinlikleri sağlar.
    - TDE, Microsoft tarafından sağlanan şifreleme anahtarlarını kullanabilir veya kendi anahtarını Getir desteğini kullanarak kendi anahtarlarınızı sağlayabilir.


**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview) Iaas Vm'leri için Azure Disk şifrelemesi.
- [Etkinleştirme](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-windows) Iaas Windows Vm'leri için şifreleme.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) bekleyen veriler için Azure depolama hizmeti şifrelemesi.
- [Okuma](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) Always Encrypted bir genel bakış.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql?view=sql-server-2017) TDE Azure SQL veritabanı için.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql) ile TDE kendi anahtarını getir.

## <a name="best-practice-protect-vms-with-antimalware"></a>En iyi yöntem: Kötü amaçlı yazılımdan koruma ile Vm'leri koruma

Özellikle, eski Azure geçirilen Vm'leri yüklü kötü amaçlı yazılımdan koruma düzeyini uygun olmayabilir.  Azure Vm'leri virüsler, casus yazılım ve diğer kötü amaçlı yazılımlardan korunmasına yardımcı olan ücretsiz uç nokta çözümü sağlar.
- Azure için Microsoft Antimalware bilinen kötü amaçlı veya istenmeyebilecek yazılım kendini yüklemeye çalışır olduğunda uyarılar oluşturur.
- İnsan müdahalesi olmadan arka planda çalışan tek bir aracı çözümüdür.
- Azure Güvenlik Merkezi'nde yoksa çalıştıran uç nokta koruması ve gerektiği gibi Microsoft Antimalware yükleme sanal makineleri kolayca belirleyebilirsiniz.

![Kötü amaçlı yazılımdan koruma VM'ler için](./media/migrate-best-practices-security-management/antimalware.png)
*VM'ler için kötü amaçlı yazılım*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/security/azure-security-antimalware) Microsoft kötü amaçlı yazılımdan koruma.

## <a name="best-practice-secure-web-apps"></a>En iyi yöntem: Web uygulamalarının güvenliğini sağlama

Geçirilen web uygulamaları sorunlarını birkaç yüz yüze gelmektedir:

- Çoğu eski web uygulaması yapılandırma dosyanız içinde hassas bilgileri eğilimindedir.  Gibi bilgileri içeren dosyaları, uygulamaları yedeklenir veya uygulama kod içine veya dışına kaynak denetimine iade edildiğinde güvenlik sorunları sunabilir.
- Bir VM'de bulunan web uygulamalarını geçirdiğinizde, ayrıca, büyük olasılıkla o makine bir şirket içi ağ ve güvenlik duvarı korumalı ortam internet'e yönelik bir ortam için taşınıyor. Koruma kaynakları şirket içi olarak aynı işi yapan bir çözümü ayarlama emin olmanız gerekir.


Azure çözümleri birkaç sağlar:

- **Azure anahtar kasası**: Web uygulaması geliştiricilerine hassas bilgileri bu dosyalara ilişkin sızmasına olmadığından emin olma adımları BUGÜN sürüyor. Dosyaları ayıklayın ve bir Azure anahtar kasasını yerleştirmek için bilgilerinin güvenliğini sağlamak için bir yöntem var.
    - Uygulama gizli anahtarlarının depolamayı merkezileştirmeyi için Key Vault'u kullanın ve denetleyebilirsiniz. Bu, güvenlik bilgileri uygulama dosyaların depolanacağı gereksinimini ortadan kaldırır.
    - Uygulamaları URI'ler, özel kod gerek kalmadan kullanarak kasadaki güvenlik erişim bilgileri kullanabilirsiniz.
    - Azure Key Vault, Azure güvenlik denetimlerini aracılığıyla erişimi kilitleme ve 'anahtarları' sorunsuz bir şekilde uygulamak için sağlar. Microsoft bakın veya veri ayıklayın.
- **App Service ortamı**: Geçiş uygulama ek koruma gerekiyorsa, bir App Service ortamı ve Web uygulaması güvenlik duvarı uygulama kaynaklarını korumak için eklemeyi düşünebilirsiniz.
    - Azure App Service ortamı, App Service uygulamaları gibi Windows ve Linux web uygulamaları, Docker kapsayıcıları, mobil uygulamalar ve işlevleri çalıştırmak için bir tam yalıtılmış ve ayrılmış ortam sağlar.
    - Çok büyük ölçekli uygulamalar için yararlıdır, yalıtım ve güvenli ağ erişimi veya yüksek bellek kullanımı gerektirir
- **Web uygulaması güvenlik duvarı**: Web apps için merkezi koruma sağlayan bir Application Gateway özelliğidir.
    - Web apps, arka uç kod değişikliği yapmaya gerek kalmadan korur.
    - Aynı zamanda bir application gateway arkasında birden fazla web uygulaması korur.
    - Web uygulaması güvenlik duvarı Azure İzleyicisi'ni kullanarak izlenebilir ve Azure Güvenlik Merkezi ile tümleşiktir.



![Web uygulamalarının güvenliğini sağlama](./media/migrate-best-practices-security-management/web-apps.png)
*Azure anahtar kasası*

**Daha fazla bilgi edinin:**

- [Genel bakışın](https://docs.microsoft.com/azure/key-vault/key-vault-overview) Azure anahtar kasası.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/application-gateway/waf-overview) Web uygulaması güvenlik duvarı.
- [Giriş yapın](https://docs.microsoft.com/azure/app-service/environment/intro) App Service ortamları için.
- [Bilgi nasıl](https://docs.microsoft.com/azure/key-vault/tutorial-web-application-keyvault) Key Vault'tan gizli anahtarları okumak için bir web uygulaması yapılandırırsınız.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/application-gateway/waf-overview) Web uygulaması güvenlik duvarı

## <a name="best-practice-review-subscriptions-and-resource-permissions"></a>En iyi yöntem: Gözden geçirme abonelikler ve kaynak izinleri

İş yüklerinizi geçirin ve bunları Azure'da çalıştırmak gibi iş yükü erişimle personeli hareket ettirin. Güvenlik ekibinizin düzenli aralıklarla Azure kiracısı ve kaynak grupları için erişim gözden geçirmeniz gerekir. Azure teklifleri için kimlik yönetimi ve erişim denetimi güvenliği, rol tabanlı erişim denetimi (RBAC) Azure kaynaklarına erişim izni vermenin dahil olmak üzere bir dizi sağlar.

- RBAC, güvenlik sorumluları için erişim izinleri atar. Güvenlik sorumluları temsil eden kullanıcılar, gruplar (kullanıcı kümesi), hizmet sorumluları (uygulamalar ve Hizmetleri tarafından kullanılan kimlik) ve yönetilen kimlikleri (otomatik olarak Azure tarafından yönetilen bir Azure Active Directory kimliği).
- RBAC rolleri, rol tarafından gerçekleştirilebilen işlemleri tanımlayan sahibi, katkıda bulunan ve okuyucu ve rol tanımları (izin koleksiyonu) gibi güvenlik ilkeleri atayabilirsiniz.
- RBAC, bir rol için bir sınır ayarlamak kapsamları da ayarlayabilirsiniz. Kapsam düzeyleri, bir yönetim grubuna, aboneliğe, kaynak grubuna ya da kaynak dahil olmak üzere bir dizi sırasında ayarlanabilir.
- Azure erişime sahip Yöneticiler yalnızca izin vermek istediğiniz kaynaklarına erişebilmesi olduğundan emin olun.  Azure'da önceden tanımlanmış rollerin yeterince ayrıntılı mevcut değilse, ayırın ve erişim izinleri sınırlandırmak için özel roller oluşturabilirsiniz.

![Erişim denetimi](./media/migrate-best-practices-security-management/subscription.png)
*erişim denetimi - IAM*

**Daha fazla bilgi edinin:**

- [Hakkında](https://docs.microsoft.com/azure/role-based-access-control/overview) RBAC.
- [Bilgi](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) RBAC ve Azure portalını kullanarak erişimi yönetmek için.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) özel roller.

## <a name="best-practice-review-audit-and-security-logs"></a>En iyi yöntem: Denetim ve güvenlik günlüklerini gözden geçirin

Azure Active Directory (AD), Azure İzleyici'de görüntülenen etkinlik günlükleri sağlar. Günlükler Azure kiralama, bunlar ortaya çıktığında ve bunları gerçekleştiren gerçekleştirilen işlemleri yakalar. 

- Denetim günlüklerini kiracıda görevleri geçmişini gösterir. Oturum açma etkinliği gerçekleştirilen görevleri show günlüğe kaydeder. 
- Erişim için güvenlik raporları, Azure AD lisansınıza bağlıdır. Ücretsiz ve temel riskli kullanıcılar ve oturum açma işlemlerinin listesini alın. Premium 1 ve Premium 2 sürümlerinde olay bilgilerini temel alın.
- Etkinlik günlükleri için bir uzun süreli saklama ve veri öngörüleri için uç nokta sayısı yönlendirebilirsiniz.
- Yaygın bir uygulama günlüklerini gözden geçirin veya otomatik olarak prosesler gözden geçirmek için güvenlik bilgileri ve Olay yönetimi (SIEM) araçlarınızı tümleştirin kolaylaştırır.  Premium 1 veya 2'yi kullanmıyorsanız, analiz çok fazla kendiniz veya SIEM sisteminizi kullanarak gerçekleştirmeniz gerekir.  Riskli oturum açma işlemleri ve olayları ve diğer kullanıcı saldırı desenlerini arayan analizi içerir.


![Kullanıcılar ve gruplar](./media/migrate-best-practices-security-management/azure-ad.png)
*Azure AD kullanıcıları ve grupları*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor) Azure İzleyici'de Azure AD etkinlik günlükleri.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs) denetim etkinlik raporları Azure AD portalında.

## <a name="best-practice-evaluate-other-security-features"></a>En iyi yöntem: Diğer güvenlik özellikleri değerlendirme

Azure, birçok gelişmiş güvenlik seçenekleri diğer güvenlik özellikleri sağlar. Bu en iyi uygulamaları bazıları, eklenti lisanslar ve premium seçeneklerini gerektirir.

- **Azure AD yönetim birimi (AU) uygulayan**: Personel desteklemek için yönetim görevleri için temsilci seçme yalnızca temel Azure erişim denetimi ile zor olabilir.  İdeal yaklaşım Kurumsal güvenlik için Azure AD'de grupları yönetmek için destek personeli erişimi verme olmayabilir.  AU'ı kullanarak Azure kaynakları, şirket içi kuruluş birimlerine (OU) benzer bir şekilde kapsayıcılarına ayırmak sağlar.  AU kullanmak için AU yönetici bir premium Azure AD Lisansınızın olması gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-administrative-units).
- **Kullanım ve çok faktörlü kimlik doğrulaması (MFA)**: Etkinleştirebilir ve yönetici hesaplarınızdan MFA zorlamak bir premium Azure AD lisansına sahip olması durumunda. Kimlik avı hesapları kimlik bilgileri tehlikede olduğunu, en yaygın yoludur.  Kötü bir aktör yönetici hesabı kimlik bilgilerine sahip sonra hiçbir bunları tüm kaynak grupları silme gibi değiştirebilen Eylemlerdeki durdurma yoktur. MFA e-posta, kimlik doğrulayıcı uygulamasını ve telefon metin mesajları dahil olmak üzere çeşitli ayarlayabilirsiniz. Yönetici olarak, az müdahale eden seçeneğini seçebilirsiniz. MFA threat analytics ile tümleşir ve rastgele bir MFA testini gerektirmek için koşullu erişim ilkeleri yanıtlayın. Daha fazla bilgi edinin [Güvenlik Kılavuzu](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication-security-best-practices), ve [nasıl ayarlandığı](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication-security-best-practices) MFA.
- **Koşullu erişim uygulamak**: En küçük ve orta ölçekli kuruluşlarda, Azure yöneticileri ve Destek ekibine muhtemelen tek bir coğrafi bölge bulunur. Bu durumda, çoğu oturum açma bilgileri aynı alanlarından gelecektir. Oldukça statik IP adresleri, bu konumlardan varsa, bu alanlar dışında yönetici oturum açma bilgilerinden görmeyeceksiniz mantıklıdır. Uzaktan kötü bir aktör yöneticinin kimlik bilgilerini tehlikeye atar bile bir durumda, uzak konumlardan veya sahte konumlardan rastgele IP adreslerinden oturum açma önlemek için MFA ile koşullu erişim gibi güvenlik özelliklerini uygulayabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) koşullu erişim hakkında ve [en iyi uygulamaları İnceleme](https://docs.microsoft.com/azure/active-directory/conditional-access/best-practices) Azure AD'de koşullu erişim için.
- **Kurumsal uygulama izinleri gözden**: Zaman içinde kuruluş üzerindeki etkilerine bilmeden Microsoft ve üçüncü taraf bağlantıları yöneticileri tıklayın. Bağlantılar, Azure uygulamaları için izinleri atayın ve Azure AD verilerini okumak üzere erişim ya da tüm Azure aboneliğinizi yönetmek için bile tam erişime izin verebilir onay ekranlar sunabilir. Düzenli olarak, Yöneticiler ve kullanıcılar Azure kaynaklarına erişimi allowed uygulamaları gözden geçirmeniz gerekir. Bu uygulamalar yalnızca gerekli izinlere sahip olduğunuzdan emin olmanız gerekir. Böylece bunlar, kuruluş verilerine erişim izni uygulamalar farkında ek olarak, üç aylık veya yıllık noktalı kullanıcılar uygulama sayfaları bağlantısını içeren e-posta gönderebilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/manage-apps/application-types) uygulama türleri hakkında ve [denetleme](https://docs.microsoft.com/azure/active-directory/manage-apps/remove-user-or-group-access-portal) Azure AD'de uygulama atamaları.



## <a name="managed-migrated-workloads"></a>Yönetilen geçirilen iş yükleri

Bu bölümde Azure yönetimi için bazı en iyi uygulamaları öneririz dahil olmak üzere:

- [Kaynakları yönetmek](#best-practice-name-resource-groups): Azure kaynak grupları ve akıllı gibi kaynakları için en iyi adlandırma, yanlışlıkla silinmesi, kaynak izinleri ve etkin kaynak etiketleme yönetme engelliyor.
- [Blueprint kullanın](#best-practice-implement-blueprints): Şemaları oluşturmak ve Dağıtım Ortamınızı yönetmek için kullanma hakkında hızlı bir genel bakış edinin.
- [Gözden geçirme mimarileri](#best-practice-review-azure-reference-architectures): Gözden geçirme gelen geçiş sonrası dağıtımlarınızı oluşturdukça öğrenmek için Azure mimarileri örnek.
- [Yönetim gruplarını ayarlama](#best-practice-manage-resources-with-management-groups): Birden fazla aboneliğiniz varsa, bunları yönetim gruplar halinde toplayın ve bu gruplara idare ayarlarını uygulayın.
- [Erişim ilkeleri ayarlama](#best-practice-deploy-azure-policy): Uyumluluk ilkeleri, Azure kaynaklarınıza uygulanır.
- [BCDR stratejinize uygulamak](#best-practice-implement-a-bcdr-strategy): Kesintiler meydana geldiğinde güvenli veri, ortam dayanıklı ve kaynakları kalmasını sağlamak için iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejisine ve çalışır durumda bir araya.
- [Vm'leri yönetme](#best-practice-use-managed-disks-and-availability-sets): Kullanılabilirlik grupları, dayanıklılık ve yüksek kullanılabilirlik grubu Vm'leri. Yönetilen diskler, sanal makine disk ve depolama yönetim kolaylığı için kullanın.
- [Kaynak kullanımını izlemek](#best-practice-monitor-resource-usage-and-performance): Azure kaynakları için tanılama günlüğünü etkinleştirme, uyarılar ve proaktif sorun giderme için playbook'ları oluşturun ve Azure panosuna dağıtım durumu ve durumu birleşik bir görünümünü kullanın.
- [Destek ve güncelleştirmeleri yönetme](#best-practice-manage-updates): Azure destek planınızı ve değişiklik yönetimi için yerinde Vm'leri güncel ve put işlemleri tutmak için en iyi uygulamaları alma nasıl uygulamak anlayın.


## <a name="best-practice-name-resource-groups"></a>En iyi yöntem: Kaynak grubu adı

Kaynak gruplarınızın anlamlı adlar, yöneticilerin sahip ve destek ekibi üyelerini kolayca tanımak gidin ve üretkenliğini ve verimliliğini önemli ölçüde artırır.
- Azure adlandırma kurallarına öneririz.
- Eşitliyorsanız, şirket içi AD DS AD Connect kullanarak Azure ad güvenlik grupları şirket içi Azure kaynak gruplarının adlarını adları eşleşen göz önünde bulundurun.

![Adlandırma](./media/migrate-best-practices-security-management/naming.png)
*kaynak grubu adlandırma*


**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) adlandırma kuralları

## <a name="best-practice-implement-delete-locks-for-resource-groups"></a>En iyi yöntem: Uygulama kaynak grupları kilitlerini Sil

Gereksinim duyduğunuz son yanlışlıkla silinmiş olduğundan kaybolması için bir kaynak grubu şeydir. Böylece bu gerçekleşmez silme kilitleri uygulamak öneririz.


![Kilitlerini Sil](./media/migrate-best-practices-security-management/locks.png)
*kilitlerini Sil*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-lock-resources) beklenmeyen değişiklikleri önlemek için kaynakları kilitleme.


## <a name="best-practice-understand-resource-access-permissions"></a>En iyi yöntem: Kaynak erişim izinleri anlama 

Abonelik sahibi aboneliğinizdeki tüm kaynak grupları ve kaynaklara erişimi vardır.
- Kişileri bu değerli atamaya tutumlu ekleyin. Bu tür izinler etkilerini anlamak, güvenli, kararlı ortamınızı sağlamadaki önemlidir.
- Kaynakları uygun kaynaklar gruplarında yerleştirin emin olun:
    - Benzer bir yaşam döngüsüne sahip kaynakları birlikte eşleştirin. İdeal olarak, bir kaynak grubunun tamamını silmek, ihtiyacınız olduğunda, bir kaynak taşıma gerekmez.
    - Bir işlev veya iş yükü destekleyen kaynaklar için basitleştirilmiş yönetim birlikte yerleştirilmelidir.

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://azure.microsoft.com/blog/organizing-subscriptions-and-resource-groups-within-the-enterprise/) abonelikler ve kaynak gruplarını düzenleme.

## <a name="best-practice-tag-resources-effectively"></a>En iyi yöntem: Kaynakları verimli etiketi

Genellikle, ilgili kaynakları için yalnızca bir kaynak grubu adı'nı kullanarak yeterli meta verileri iç faturalama veya abonelik içinde yönetim gibi mekanizmaların etkin bir uygulama için sağlamaz.
- En iyi uygulama, sorgulama ve üzerinde bildirilen yararlı meta verileri eklemek için Azure etiketleri kullanmanız gerekir. 
- Etiketleri tanımladığınız özelliklere sahip kaynakları mantıksal olarak düzenlemek için bir yol sağlar.  Etiketleri kaynak grupları ve kaynaklara doğrudan uygulanabilir.
- Etiketler, bir kaynak grubu veya tek tek kaynaklar üzerinde uygulanabilir. Kaynak grubu etiketleri grubundaki kaynaklar tarafından devralınmaz.
- Powershell veya Azure Otomasyonu veya etiket tek tek gruplar ve kaynakları kullanarak etiketleme otomatik hale getirebilirsiniz. -yaklaşım veya Self Servis bir etiketleme.  Bir istek geçerli yönetim sistemi değiştirirseniz, şirketinize özgü kaynak etiketlerinizi doldurmak için istek bilgileri kolayca kullanabilir.


![Etiketleme](./media/migrate-best-practices-security-management/tagging.png)
*etiketleme*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) etiketleme ve sınırlamalar etiketleyin.
- [Gözden geçirme](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags#powershell) PowerShell ve CLI örnekleri etiketleme ayarlama ve etiketler, bir kaynak grubundan kaynaklara uygulanacak.
- [Okuma](https://www.azurefieldnotes.com/2016/07/18/azure-resource-tagging-best-practices/) Azure en iyi etiketleme.


## <a name="best-practice-implement-blueprints"></a>En iyi yöntem: Blueprint uygulayın

Yalnızca şema mühendisleri ve mimarlara projenin tasarım parametreleri taslak izin verdiği ölçüde, bulut mimarları ve merkezi BT grupları gerçekleştiren ve bir kuruluş standartlarına uyar tekrarlanabilir Azure kaynakları tanımlamak Azure şemaları etkinleştirmek, desenler ve gereksinimleri. Azure bir blueprint'i geliştirme takımları kullanarak hızlı bir şekilde oluşturun ve kurumsal uyumluluk gereksinimlerini karşılayan ve ağ,, geliştirme ve teslim hızını artırmak için gibi yerleşik bileşenler kümesi olan yeni ortamlar oluşturun.

- Blueprint kaynak grubu, Azure Resource Manager şablonları ve ilke ve rol atamaları dağıtım düzenlemek için kullanın.
- Azure şemaları, Global olarak dağıtılmış bir Azure Cosmos DB'de depolanır. Şema nesneleri birden çok Azure bölgesinde çoğaltılır. Çoğaltma, düşük gecikme süreli, yüksek kullanılabilirlik ve tutarlı, blueprint kaynak dağıtan bölge ne olursa olsun blueprint erişimi sağlar.

**Daha fazla bilgi edinin:**

- [Okuma](https://docs.microsoft.com/azure/governance/blueprints/overview) şemaları hakkında.
- [Gözden geçirme](https://azure.microsoft.com/blog/customizing-azure-blueprints-to-accelerate-ai-in-healthcare/) sağlık hizmetleri yapay ZEKA hızlandırmak için kullanılan bir şema örnek.

## <a name="best-practice-review-azure-reference-architectures"></a>En iyi yöntem: Azure başvuru mimarileri gözden geçirin

Güvenli, ölçeklenebilir ve yönetilebilir iş yüklerini azure'da göz korkutucu olabilir.  Sürekli değişiklikler sayesinde, ideal bir ortam için farklı özelliklere sahip açık kalmasını sağlamak zor olabilir. Uzmanlardan başvuru sahip tasarlarken ve iş yüklerinizin geçişini yararlı olabilir.  Azure ve Azure iş ortakları, çeşitli ortamlar yönelik birkaç örnek başvuru mimarileri yerleşik sahiptir. Bu örnekler, öğrenin ve yapı fikirler sağlamak üzere tasarlanmıştır. 

Başvuru mimarileri, senaryoya göre düzenlenmiştir. Önerilen uygulamalar ve yönetimi, kullanılabilirlik, ölçeklenebilirlik ve güvenlik önerisi içerirler.
Azure App Service ortamı, App Service uygulamaları, Windows ve Linux web uygulamaları, Docker kapsayıcıları, mobil uygulamalar ve işlevleri çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar. App Service Azure'un gücünü uygulamanıza güvenlik, Yük Dengeleme, otomatik ölçeklendirme ve otomatik yönetim ekler. Ayrıca sürekli dağıtımı Azure DevOps ve GitHub, paket Yönetimi'ni Hazırlama ortamları, özel etki alanı ve SSL sertifikaları gibi DevOps özelliklerinden yararlanabilirsiniz. App Service, yalıtım ve güvenli ağ erişimi ve yüksek miktarda bellek ve diğer kaynakları ölçeklendirmeniz mi gerekiyor kullananlar ihtiyaç duyan uygulamalar için yararlıdır.
**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/reference-architectures/) Azure başvuru mimarileri.
- [Gözden geçirme](https://docs.microsoft.com/azure/architecture/example-scenario/) Azure örnek senaryolar.

## <a name="best-practice-manage-resources-with-management-groups"></a>En iyi yöntem: Yönetim gruplarıyla kaynakları yönetme

Kuruluşunuzda birden fazla aboneliğiniz varsa, erişim ilkeleri ve uyumluluk için bunları yönetmek gerekir. Azure yönetim grupları, aboneliklerin üzerinde bir kapsam düzeyi sunar.

- Yönetim grupları denir kapsayıcılarına abonelikleri düzenleme ve bunları idare koşullar geçerlidir.
- Bir yönetim grubundaki tüm abonelikler, Yönetim Grup koşulları otomatik olarak devralır.
- Yönetim grupları, sahip olduğunuz abonelik türüne ne olursa olsun büyük ölçekli, kurumsal düzeyde yönetimi sağlar.
- Örneğin, VM'ler oluşturulabilir bölgeleri sınırlayan bir yönetim grup ilkelerini uygulayabilirsiniz. Bu ilke, tüm Yönetim gruplarını, abonelikleri ve bu yönetim grubu kapsamındaki kaynaklar sonra uygulanır.
- Yönetim gruplarında ve Aboneliklerde, birleşik İlkesi ve erişim yönetimi için bir hiyerarşiye kaynaklarınızı düzenlemek için esnek bir yapısını oluşturabilirsiniz.

Aşağıdaki diyagramda, yönetim grupları kullanılarak idare amaçlı bir hiyerarşi oluşturma örneği gösterilmektedir.

![Yönetim grupları](./media/migrate-best-practices-security-management/management-groups.png)
*Yönetim grupları*

**Daha fazla bilgi edinin:**
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/governance/management-groups/index) ilgili kaynak yönetimi gruplar halinde düzenlemek.

## <a name="best-practice-deploy-azure-policy"></a>En iyi yöntem: Azure İlkesi dağıtma

Azure İlkesi, ilkelerinizi oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir.

- Bu kaynakları, Kurumsal standartlarınız ve hizmet düzeyi sözleşmeleri ile uyumlu kalmasını sağlar ilkeleri, kaynaklarınız üzerinden farklı kuralları ve etkileri uygular.
- Azure İlkesi kaynaklarınızın ilkelerinizle uyumlu olanlar için tarama değerlendirir.
- Örneğin, belirli bir SKU boyutu VM'ler için ortamınızda izin veren bir ilke oluşturabilirsiniz. Azure İlkesi bu ayar, kaynakların oluşturulup güncelleştirildiği ve mevcut kaynaklar taranırken değerlendirir. 
- Azure, atayabileceğiniz bazı yerleşik ilkeler sağlar veya kendi oluşturabilirsiniz.


![Azure İlkesi](./media/migrate-best-practices-security-management/policy.png)
*Azure İlkesi*

**Daha fazla bilgi edinin:**
- [Genel bakışın](https://docs.microsoft.com/azure/governance/policy/overview) Azure ilkesi.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage) uyumluluğu zorlamak için ilke oluşturma ve yönetme.


## <a name="best-practice-implement-a-bcdr-strategy"></a>En iyi yöntem: BCDR stratejinize uygulayın

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) için planlama, Azure'a geçiş için planlama sırasında tamamlaması kritik bir uygulamadır. Yasal koşullarını sözleşmeniz kasırgalar veya deprem gibi büyük bir zorla yükümlülüklerin mazur görün bir mücbir sebepli olaylar yan tümcesi içerir. Ancak, ayrıca yükümlülükler etrafında Hizmetleri çalıştırın ve olağanüstü durum strike zaman gerektiğinde, Kurtarma devam edeceğinden emin olmak için bir özelliği vardır. Bunu yapmak için yeteneğinizi değişiklik yapabilir veya şirketinizin geleceği Kes.

Genel anlamıyla BCDR stratejinize dikkate almanız gerekir:
- **Veri yedekleme**: Nasıl kesintiler meydana gelirse, kolayca kurtarabilirsiniz böylece verilerinizi güvende tutun.
- **Olağanüstü durum kurtarma**: Uygulamalarınızı, dayanıklı ve kesintiler durumunda kullanılabilir durumda tutmak nasıl. 

### <a name="azure-resiliency-features"></a>Azure'da dayanıklılık özellikleri
Azure platformu birçok dayanıklılık özelliği sağlar.

- **Bölge eşleştirme**: Azure veri yerleşimi sınırları içinde Bölgesel koruma sağlamak için bölge çiftleri. Azure bölge çiftleri arasındaki fiziksel yalıtımı sağlar, tek bir bölge çiftindeki geniş kapsamlı bir kesinti durumunda kurtarma önceliklendirir, her bölgede ayrı olarak sistem güncelleştirmelerini dağıtır ve arasında çoğaltmak için Azure coğrafi olarak yedekli depolama gibi özellikler sağlar Bölgesel çiftler.
- **Kullanılabilirlik alanları**: Kullanılabilirlik alanları, bir Azure bölgesi ile ayrı fiziksel bölgeler oluşturarak, tüm Azure veri merkezi arızasına karşı koruyun. Her bölge, bir ayırt edici bir güç kaynağına, ağ altyapısı ve soğutma mekanizması vardır.
- **Kullanılabilirlik kümeleri**: Kullanılabilirlik kümeleri, bir veri merkezi hatalarına karşı koruyun. Yüksek oranda kullanılabilir kalmasını sağlamak için kullanılabilirlik kümelerinde Vm'leri gruplar. Her kullanılabilirlik kümesi içinde Azure birden çok hata etki alanları ortak bir güç kaynağı ve ağ anahtarı ve bakımdan geçirilebilen ya da yeniden başlatılması, temel alınan donanım gruplamak güncelleştirme etki alanları ile donanımı temel birlikte bu gruba uygular, aynı anda. Bir örnek olarak, bir iş yükü Azure sanal makinelerde dağıldığında her uygulama katmanı için iki veya daha fazla sanal makine bir küme içine koyabilirsiniz. Örneğin, bir küme ve veri katmanı Vm'lerini başka bir ön uç sanal makineleri yerleştirebilirsiniz. Yalnızca bir güncelleme etki alanı olduğundan her kümesi zamanında yeniden ve Azure kümesindeki VM'lerin hata etki alanlarına yayılır, küme içindeki tüm VM'ler aynı anda başarısız olmak sağlar.

### <a name="set-up-bcdr"></a>BCDR ayarlayın

Azure'a geçiş sırasında Azure platformu bu yerleşik dayanıklılık özellikler sağlasa da, Azure özellikleri ve yüksek kullanılabilirlik sağlayan hizmetlerinden yararlanmak için Azure dağıtım tasarımınız gerektiğini anlamak önemlidir, Olağanüstü durum kurtarma ve yedekleme.

- BCDR çözümünüzü şirket hedeflerinize bağlı olacaktır ve Azure dağıtım stratejinizi etkiler. Bir hizmet (Iaas) platformu olarak bir altyapı olarak hizmet (PaaS) dağıtımlar için BCDR farklı zorlukları sunar.
- Bir kez yerine, BCDR çözümlerinizi düzenli olarak stratejinizi kurtarılabilir kalmasını denetlemek için test edilmelidir.


## <a name="best-practice-back-up-your-data"></a>En iyi yöntem: Verilerinizi yedekleme

Çoğu durumda, geçişten sonra bir şirket içi iş yükü kullanımdan kaldırıldığında ve verilerinizi yedeklemek için şirket içi stratejinizi genişletilmiş veya değiştirilmesi gerekir. Tüm veri merkezinizi Azure'a geçirme, tasarım ve Azure teknolojilerini kullanan bir tam yedekleme çözümü gerekir veya üçüncü taraf çözümleri tümleşik. 


### <a name="back-up-an-iaas-deployment"></a>Bir Iaas dağıtımı geri

Azure Iaas Vm'lerinde çalışan iş yükleri için bu yedekleme çözümleri göz önünde bulundurun:

- **Azure yedekleme**: Azure Windows ve Linux VM'ler için uygulamayla tutarlı yedeklemeler yapılmasını sağlar.
- **Depolama anlık görüntüleri**: Blob depolamanın anlık görüntüsünü.

#### <a name="azure-backup"></a>Azure Backup

Azure Backup yedekler, Azure Depolama'da depolanan veriler kurtarma noktaları oluşturur. Azure Backup, Azure sanal makine disklerini ve Azure dosyaları (Önizleme) yedekleyebilirsiniz. Azure dosyaları SMB erişilebilir bulut dosya paylaşımları sağlar.
   
Çeşitli şekillerde Vm'lerini yedeklemek için Azure Backup'ı kullanabilirsiniz.

- **VM ayarlarını doğrudan yedekten**: Doğrudan Azure portalındaki VM seçeneklerden Vm'leri Azure Backup ile yedekleyebilirsiniz. VM bir kez ve günlük yedekleme ve VM disk gerektiği gibi geri yükleyebilirsiniz. Azure yedekleme, uygulama durumunu algılayan veri anlık görüntüler (VSS) alır, aracı, VM'de yüklüdür.
- **Bir kurtarma Hizmetleri Kasası'nda doğrudan yedekleme**: Azure Backup kurtarma Hizmetleri kasasını dağıtarak, Iaas sanal makinelerini yedekleyebilirsiniz. Bu yedeklemeler yönetmek ve izlemek için tek bir konum sağlar ve ayrıntılı bir yedekleme ve geri yükleme seçenekleri sağlar. Yedekleme günde üç kez kadar dosya/klasör düzeyinde bulunur. Uygulama durumunu algılayan değildir ve Linux desteklenmiyor. Yedeklemek istediğiniz her VM'de Microsoft Azure kurtarma Hizmetleri (MARS) aracısı yüklemeniz gerekir.
- **Azure Backup sunucusu: Azure Backup sunucusu için VM koruma**: Azure Backup sunucusu Azure yedekleme ile ücretsiz olarak sağlanır. VM yerel Azure Backup Sunucusu'na depolama alanına yedeklenir. Ardından Azure Backup sunucusu Kasası'nda Azure'a yedekleyebilirsiniz. Uygulama durumunu algılayan, sık sık yedekleme ve bekletme üzerinde tam ayrıntı düzeyi ile yedeklemedir. Uygulama düzeyinde yedekleyebilirsiniz. Örneğin SQL Server veya SharePoint tarafından yedekleme.

Güvenlik için Azure Backup uçuşan verileri şifreler. AES 256 kullanılarak ve HTTPS üzerinden Azure'a gönderir. Yedeklenen verilerin bekleyen azure'da kullanılarak şifrelenir [depolama hizmeti şifrelemesi (SSE)](https://docs.microsoft.com/azure/storage/common/storage-service-encryption?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)ve veri iletilmesini ve depolanmasını.


![Azure yedekleme](./media/migrate-best-practices-security-management/iaas-backup.png)
*Azure yedekleme*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) farklı yedekleme türleri.
- [Yedekleme Altyapısı planlama](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction) Azure Vm'leri için.

#### <a name="storage-snapshots"></a>Depolama anlık görüntüleri

Azure Vm'leri, Azure Depolama'daki sayfa blobları olarak depolanır. 

- Anlık görüntüler, zaman içinde belirli bir noktada blob durumu yakalar.
- Azure VM diskleri için bir alternatif yedek yöntemi olarak depolama BLOB'ları anlık görüntüsünü alın ve bunları başka bir depolama hesabına kopyalayın. 
- Tüm bir bloba kopyalama veya bir artımlı anlık görüntü kopyalama kopyalanacak yalnızca delta değişiklikler kullanın ve depolama alanı azaltın.
- Ek bir önlem olarak geçici silme blob depolama hesaplarında etkinleştirebilirsiniz. Bu özellik etkinleştirildiğinde, silinen bir blob silinmek üzere işaretlenmiş ancak hemen temizlenir. Blob Ara dönemde geri yüklenebilir.

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) Azure blob depolama.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/storage/blobs/storage-blob-snapshots) blob anlık görüntüsü oluşturma.
- [Örnek senaryo gözden](https://azure.microsoft.com/blog/microsoft-azure-block-blob-storage-backup) blob depolama yedekleme.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) geçici silme.
- [Olağanüstü durum kurtarma ve Azure depolama zorlamalı yük devretme (Önizleme)](../storage/common/storage-disaster-recovery-guidance.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

#### <a name="third-party-backup"></a>Üçüncü taraf yedekleme

Ayrıca, Azure Vm'leri ve yerel depolama için depolama kapsayıcıları veya diğer bulut sağlayıcılarının yedeklemek için üçüncü taraf çözümleri kullanabilirsiniz. [Daha fazla bilgi edinin](https://azuremarketplace.microsoft.com/marketplace/apps?search=backup&page=1) Azure Marketi'nde yedekleme çözümleri hakkında. 


### <a name="back-up-a-paas-deployment"></a>Bir PaaS dağıtımı geri


Iaas, kendi Vm'leri ve altyapı yönettiğiniz, içinde bir PaaS modeli platform ve altyapı yönetilir çekirdek uygulama mantığı ve yeteneklerini odaklanmanızı bırakarak sağlayıcısı. Çok sayıda farklı türde PaaS Hizmetleri ile her hizmet yedekleme amaçları için ayrı ayrı değerlendirilir. İki ortak Azure PaaS hizmetlerinin - Azure SQL veritabanı ve Azure işlevleri inceleyeceğiz.

#### <a name="back-up-azure-sql-database"></a>Azure SQL veritabanını yedekleme

Azure SQL veritabanı bir tam olarak yönetilen bir PaaS veritabanı altyapısıdır. Birkaç sağlar iş sürekliliği özellikleri, backup otomatikleştirme dahil olmak üzere.

- SQL veritabanı, her 12 saatte bir haftalık tam veritabanı yedeklemeleri ve değişiklik yedekleri otomatik olarak gerçekleştirir. İşlem günlüğü yedeklemeleri, veritabanı veri kaybına karşı korumak için beş ila on dakikada alınır.
- Yedeklemeler, saydam ve ek ücret uygulanmaz.
- Yedeklemeleri RA-GRS depolama için coğrafi olarak yedeklilik depolanır ve eşleştirilmiş coğrafi bölgeye çoğaltılır.
- Yedekleme bekletme satın alma modele bağlıdır. DTU tabanlı hizmet katmanları temel katman için yedi güne ait diğer katmanlar için 35 gün boyunca gidin.
- Bir veritabanını bir-belirli bir noktaya için saklama süresi içinde geri yükleyebilirsiniz. Ayrıca, silinen veritabanını geri yükleyebilirsiniz geri yüklemek için farklı bir coğrafi bölgede ya da uzun vadeli bir bekletme ilkesi (LTR) veritabanı varsa, uzun vadeli bir yedekten geri yükleme.


![Azure SQL yedekleme](./media/migrate-best-practices-security-management/sql-backup.png)
*Azure SQL yedekleme*

**Daha fazla bilgi edinin:**
- [Otomatik yedeklemeler](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups) SQL veritabanı için.
- [Bir veritabanını kurtarma](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups) kullanılarak otomatik olarak yedekler.

 
#### <a name="back-up-azure-functions"></a>Azure işlevleri ' yedekleme

Azure işlevleri fazla veya az kod olarak yaptığından, onları kaynak denetimi GitHub veya Azure DevOps Hizmetleri gibi bir kod korumak için kullandığınız yöntemlerin aynısını kullanarak yedeklemelisiniz

**Daha fazla bilgi edinin:**

- [Veri koruma](/azure/devops/organizations/security/data-protection) Azure DevOps için.

## <a name="best-practice-set-up-disaster-recovery"></a>En iyi yöntem: Olağanüstü durum kurtarma işlemini ayarlama 

Verileri korumaya ek olarak, BCDR planlama uygulamaları ve iş yüklerini olağanüstü bir durum yaşandığında kullanılabilir durumda tutmak nasıl dikkate almanız gerekir. 

### <a name="set-up-disaster-recovery-for-iaas-apps"></a>Iaas uygulamalar için olağanüstü durum kurtarmayı ayarlama

Azure Iaas Vm'leri ve Azure depolama üzerinde çalışan iş yükleri için bu çözümleri göz önünde bulundurun:

- **Azure Site Recovery**: Azure sanal makinelerini ikincil bir bölgeye birincili çoğaltılmasını düzenler. Kesintiler meydana geldiğinde, birincil bölgeden ikincil veritabanına yük devretme ve kullanıcıların uygulamaları erişmeye devam edebilirsiniz. Normal olarak şeyler olduğunda birincil bölgeye geri başarısız.
- **Azure depolama**: Azure, derleme dayanıklılık ve yüksek kullanılabilirlik için farklı depolama türlerini sağlar:

#### <a name="azure-site-recovery"></a>Azure Site Recovery 

Azure Site Recovery, Azure Vm'leri çevrimiçi duruma getirilmeleri sağlamaya yönelik Birincil Azure hizmeti ve VM uygulamalar kesinti olduğunda kullanılabilir ' dir. Site Recovery Vm'leri, birincil sunucudan ikincil Azure bölgesine çoğaltır. Olağanüstü durumla karşılaştığınızda, Vm'leri birincil bölgeden yük devretme ve bunları normal olarak ikincil bölgede erişmeye devam etmek. İşlemler normal döndüğünüzde devredebilirsiniz Vm'leri, birincil bölgeye geri.


![Azure Site Recovery](./media/migrate-best-practices-security-management/site-recovery.png)
*Site kurtarma*

**Daha fazla bilgi edinin:**
- [Gözden geçirme](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-disaster-recovery-guidance) Azure Vm'leri için olağanüstü durum kurtarma senaryoları.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-replicate-after-migration) olağanüstü durum kurtarma ayarlama, geçişten sonra bir Azure sanal makinesi için ayarlayın.

#### <a name="azure-storage"></a>Azure Storage

Azure depolama, yerleşik dayanıklılık ve yüksek kullanılabilirlik için çoğaltılır.

-   **Coğrafi olarak yedekli depolama (GRS)**: % 99,99999999999999 değeri en az bir bölge çapında kesintiye karşı koruma (16 9) oranında dayanıklı olmasını belirli bir yıl boyunca nesnelerin.
    - Depolama verileri ile verileriniz birincil bölgenin eşleştirilir ikincil bölgeye çoğaltır.
    - Birincil bölgeye düşer ve Microsoft ikincil bölgeye yük devretme başlatır, verilerinize okuma erişimine sahip.
- **Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)**: Bir bölge çapında arızasına karşı korur.
    - Depolama verileri ikincil bölgeye çoğaltır.
    - Microsoft yük devretme olup olmadığını başlatır bağımsız olarak ikincil bölgede çoğaltılan veriler için okuma erişimi garanti. Burada da aynı bölgede iki veya daha fazla veri merkezleri, bir sorun olabilir, ancak verilerinizin coğrafi olarak ayrılmış bir bölgede hala kullanılabilir.
-   **Bölgesel olarak yedekli depolama (ZRS)**:  Veri Merkezi arızasına karşı korur.
    - ZRS, tek bir bölgede üç depolama kümeleri arasında verileri eşzamanlı olarak çoğaltır. Kümeleri ve fiziksel olarak ayrılmış ve her biri kendi kullanılabilirlik alanı'nda bulunan.
    - Olağanüstü durum oluşursa, depolama alanınızı hala kullanılabilir olacak. ZRS, görev açısından kritik iş yükleri için en düşük hedef olmalıdır.



**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/common/storage-redundancy) Azure depolama çoğaltma.


### <a name="set-up-disaster-recovery-for-paas-workloads"></a>PaaS iş yükleri için olağanüstü durum kurtarmayı ayarlama

Olağanüstü durum kurtarma seçeneklerini PaaS iş yükü örneklerimizde düşünelim.

#### <a name="disaster-recovery-of-azure-sql-server"></a>Azure SQL Server olağanüstü durum kurtarma

Birçok farklı seçenek, her veri kaybı, Kurtarma süresi ve maliyet etkiliyor.

Bölgesel kesintiler ve olağanüstü durumlara hataları karşı dayanıklılık sağlamak için yük devretme grupları ve etkin coğrafi çoğaltma kullanabilirsiniz

- **Etkin coğrafi çoğaltma**: Hızlı bir olağanüstü durum kurtarma için etkin coğrafi çoğaltma, bir veri merkezi kesintisi oluşur ya da birincil veritabanına bir bağlantı kurulamıyor dağıtın.
    - Coğrafi çoğaltma sürekli olarak aynı veya farklı bölgelerde en çok dört ikincil veritabanınızın okunabilir kopya oluşturur.
    - Bir kesinti ikincil bölgelerden birine yük devretme ve veritabanınızı çevrimiçi duruma getirin.
- **Otomatik Yük devretme grupları**: Birden fazla veritabanının saydam yük devirli etkin coğrafi çoğaltma otomatik yük devretme grupları genişletin.
    - Otomatik Yük devretme grubu, etkin coğrafi çoğaltma grubu düzeyinde veritabanı çoğaltma ve otomatik yük devretme ile güçlü bir Özet sağlar.
    - Bir veya daha fazla birincil veritabanlarını barındıran her sunucu ve otomatik yük devretme İlkesi işaret dinleyicileri birincil veritabanlarının salt okunur çoğaltmalar ikincil bir sunucu barındırma birincil bir sunucu içeren bir yük devretme grubu oluşturun.
    - Belirtilen dinleyici uç noktaları, yük devretme işleminden sonra SQL bağlantı dizesini değiştirin ihtiyacını ortadan kaldırıyor.
- **Coğrafi geri yükleme**: 
    -   Coğrafi geri yükleme, veritabanı farklı bir bölgeye kurtarmanıza olanak tanır. Azure tüm veritabanlarının otomatik yedekleme, arka planda ikincil bir bölgeye çoğaltılır. Bu her zaman veritabanını yedekleme dosyalarının ikincil bölge'de depolanan kopyasından geri yükler.
-   **Bölgesel olarak yedekli veritabanları** Azure kullanılabilirlik alanları için yerleşik destek sağlar.
    - Bölgesel olarak yedekli veritabanları, Azure SQL sunucusu için bir veri merkezi hata olması durumunda yüksek kullanılabilirlik geliştirin.
    - Bölge yedekliliği ile farklı kullanılabilirlik alanlarında bir bölge içinde yedekli veritabanı çoğaltmalarını yerleştirebilirsiniz. 



![Coğrafi çoğaltma](./media/migrate-best-practices-security-management/geo-replication.png)
*coğrafi çoğaltma*

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability) Azure SQL Server için yüksek kullanılabilirlik.
- [Okuma](https://azure.microsoft.com/blog/azure-sql-databases-disaster-recovery-101/) olağanüstü durum kurtarma için Azure SQL veritabanları 101.
- [Genel bakışın](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) etkin coğrafi çoğaltma ve yük devretme grupları.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery) olağanüstü durum kurtarma tasarımı.
- [En iyi uygulamaları alma](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) yük devretme grupları için.
- [En iyi uygulamaları alma](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-security-config) güvenlik coğrafi geri yükleme ya da yük devretme için.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-high-availability#zone-redundant-configuration) bölge artıklığı
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery-drills) SQL veritabanı için bir olağanüstü durum kurtarma tatbikatı gerçekleştirme.

### <a name="disaster-recovery-for-azure-functions"></a>Azure işlevleri için olağanüstü durum kurtarma

Azure bilgi işlem altyapısı başarısız olursa, bir Azure işlev uygulaması kullanılamaz duruma gelebilir.

- Bu tür kapalı kalma süresi olasılığını en aza indirmek için farklı bölgelere dağıtılan iki işlev uygulamaları kullanın.
- Azure Traffic Manager, birincil işlev uygulamasında sorunları algılar ve otomatik olarak ikincil bölgedeki işlev uygulaması için trafiği yönlendirmek üzere yapılandırılabilir
- Coğrafi olarak yedekli depolama ile traffic Manager, bölgesel başarısız olması durumunda birden çok bölgede aynı işleve sahip olmasını sağlar


![Traffic Manager](./media/migrate-best-practices-security-management/traffic-manager.png)
*Traffic Manager*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications) Azure uygulamaları için olağanüstü durum kurtarma.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-functions/durable/durable-functions-disaster-recovery-geo-distribution) olağanüstü durum kurtarma ve coğrafi dağıtım, kalıcı Azure işlevleri için.


## <a name="best-practice-use-managed-disks-and-availability-sets"></a>En iyi yöntem: Yönetilen diskler ve kullanılabilirlik kümeleri kullanın

Azure kullanılabilirlik kümeleri, mantıksal olarak Vm'leri gruplandırabilir ve diğer kaynaklardan kümesindeki Vm'leri ayrı tutmak için kullanır. Bir kullanılabilirlik kümesindeki VM'ler, yerel hatalarına karşı korumak için ayrı alt sistemler ile birden çok hata etki alanına yayılır ve böylece aynı anda bir kümedeki tüm Vm'leri yeniden başlatma da birden çok güncelleştirme etki alanlarına dağıtılır.

Azure yönetilen diskler, VM diskleriyle ilişkili depolama hesaplarını yöneterek Azure Iaas Vm'leri için disk yönetimini basitleştirir.

- Yönetilen diskler, mümkün olduğunda kullanmanızı öneririz. Yalnızca depolama kullanın ve diskin boyutu için gereksinim duyduğunuz ve Azure oluşturur ve disk sizin için arka planda yönetir istediğiniz türünü belirtmeniz gerekir.
- Mevcut yönetilen diskler dönüştürebilirsiniz.
- Yüksek dayanıklılık ve kullanılabilirlik için kullanılabilirlik kümesindeki sanal makineleri oluşturmanız gerekir. Planlanmış veya planlanmamış kesintiler meydana geldiğinde, kullanılabilirlik kümeleri, kümedeki sanal makineleriniz en az biri kullanılabilir olmaya devam emin olun.


![Yönetilen diskler](./media/migrate-best-practices-security-management/managed-disks.png)
*yönetilen diskler*

**Daha fazla bilgi edinin:**
- [Genel bakışın](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview) yönetilen diskler.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks) yönetilen diskleri dönüştürme.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) azure'da Windows sanal makinelerinin kullanılabilirliğini yönetme.


## <a name="best-practice-monitor-resource-usage-and-performance"></a>En iyi yöntem: Kaynak kullanımı izleme ve performans 

Büyük ölçeklendirme özellikleri için iş yüklerinizi Azure'a taşınmış. Ancak, iş yükünüz taşıma girişinizi gerek kalmadan ölçeklendirme, Azure otomatik olarak uygular anlamına gelmez. Örneğin:

- Pazarlama kuruluşunuz %300 daha fazla trafiği yönlendiren yeni bir TV tanıtım gönderir, bu site kullanılabilirlik sorunları neden olabilir. Yeni geçirilen iş yükünüz, atanan sınırları ve kilitlenme neden olabilir.
- Başka bir örnek, bir dağıtılmış hizmet engelleme (DDoS) saldırısının geçirilen iş yükünüze olabilir. Bu durumda ölçeklendirmek için ancak kaynak saldırıları kaynaklarınıza erişmesini önlemek için istemeyebilirsiniz.

Bu iki durumda, farklı çözünürlüklere sahip, ancak her iki sizin için kullanım ve performans izleme ile neler olduğunu Öngörüler gerekir.

- Azure İzleyici, bu ölçümleri yüzeyine bırakın ve uyarılar, otomatik ölçeklendirme, olay hub'ları, logic apps ve daha fazlası ile yanıt sağlamak yardımcı olabilir.
- Azure izlemenin yanı sıra, Azure günlükleri denetleme ve performans olayları için izlemek için üçüncü taraf SIEM uygulamanızı tümleştirebilirsiniz.


![Azure İzleyici](./media/migrate-best-practices-security-management/monitor.png)
*Azure İzleyici*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/overview) Azure İzleyici.
- [En iyi uygulamaları alma](https://docs.microsoft.com/azure/architecture/best-practices/monitoring) izleme ve tanılama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/architecture/best-practices/auto-scaling) otomatik ölçeklendirme.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/security-center/security-center-export-data-to-siem) Azure veri yönlendirmek için bir SIEM aracı.

## <a name="best-practice-enable-diagnostic-logging"></a>En iyi yöntem: Tanılama günlüğünü etkinleştirme

Azure kaynakları, ölçümleri ve telemetri verilerini günlüğe adil bir sayı oluşturur.

- Varsayılan olarak, tanılama günlüğünün etkin çoğu kaynak türü yok.
- Tanılama günlüğüne kaydetme kaynaklarınız genelinde etkinleştirerek, günlük verileri sorgulamak ve uyarılar ve üzerinde dayanan playbook'ları oluşturun.
- Tanılama günlüğünü etkinleştirme, her bir kaynağın belirli bir kategoriler kümesi olur. Bir veya daha fazla günlük kategorileri ve günlük verileri için bir konum seçin. Günlükleri bir depolama hesabına veya olay hub'ı veya Azure İzleyici günlüklerine gönderilebilir. 


![Tanılama günlüğüne kaydetme](./media/migrate-best-practices-security-management/diagnostics.png)
*tanılama günlüğüne kaydetme*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) toplama ve günlük verilerini kullanma.
- [Desteklenen özellikler öğrenin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-diagnostic-logs-schema) tanılama günlüğüne kaydedilecek.


## <a name="best-practice-set-up-alerts-and-playbooks"></a>En iyi yöntem: Playbook'ları ve uyarıları ayarlama

Azure kaynakları için etkin tanılama günlüğü ile özel uyarılar oluşturmak için günlük verileri kullanmaya başlayabilirsiniz.

- İzleme verilerinizi koşullar bulunduğunda uyarılar proaktif olarak size bildirir. Sistem kullanıcılar bunları fark önce ardından sorunlarını ele alabilirsiniz. Ölçüm değerleri, günlük arama sorguları, etkinlik günlüğü olayları, platform sistem durumu ve Web sitesi kullanılabilirlik gibi üzerinde uyarabilir.
- Uyarı tetiklendiğinde, Logic App Playbook'unu çalıştırabilirsiniz. Playbook otomatikleştirmek ve belirli bir uyarı için bir yanıt düzenlemek için yardımcı olur. Azure Logic Apps üzerinde playbook'ları temel alır. Playbook'lar oluşturabilir veya kendi oluşturmak için mantıksal uygulama şablonları kullanabilirsiniz.
- Basit bir örnek olarak, bir bağlantı noktası tarama bir ağ güvenlik grubu karşı gerçekleştiğinde tetikleyen bir uyarı oluşturabilirsiniz.  Çalıştıran ve tarama başlangıç IP adresi kilitler playbook ayarlayabilirsiniz.
- Başka bir örnek, bir bellek sızıntısı ile bir uygulama olabilir.  Bellek kullanımı belirli bir noktaya aldığında, bir playbook işlemi geri dönüştürebilirsiniz.


![Uyarılar](./media/migrate-best-practices-security-management/alerts.png)
*uyarıları*

**Daha fazla bilgi edinin:**
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-alerts) uyarıları.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/security-center/security-center-playbooks) Güvenlik Merkezi uyarılarını yanıtlama güvenlik playbook'ları.

## <a name="best-practice-use-the-azure-dashboard"></a>En iyi yöntem: Azure panosunu kullanma

Azure portalı oluşturun, yönetin ve basit web uygulamalarından karmaşık bulut uygulamalarına kadar her şeyi izlemek olanak tanıyan bir web tabanlı birleşik konsoludur. Özelleştirilebilir bir pano ve erişilebilirlik seçeneklerine sahiptir.
- Birden çok Pano oluşturma ve Azure aboneliklerinizi erişimi olan diğer kişilerle paylaşabilirsiniz.
- Bu paylaşılan modeliyle takımınız bunları bulut sistemleri yönetirken proaktif olarak sağlayan Azure ortamına görünürlük sahiptir.


![Azure Panosu](./media/migrate-best-practices-security-management/dashboard.png)
*Azure Panosu*

**Daha fazla bilgi edinin:**

- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) bir pano oluşturun.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-structure) Pano yapısı.


## <a name="best-practice-understand-support-plans"></a>En iyi yöntem: Destek planlarını anlayın

Belirli bir noktada, destek personeli veya Microsoft destek personeli ile işbirliği yapması gerekir. Olağanüstü durum kurtarma gibi senaryoları sırasında politikaları ve yordamları desteği için bir dizi olması önemlidir. Buna ek olarak, Yöneticiler ve Destek ekibinizin bu ilkeleri uygulama üzerinde eğitim verilmelidir.

- Bir Azure hizmet sorunu yükünüz etkiler olası Olay yöneticileri Microsoft için bir destek bileti en uygun ve etkili bir şekilde göndermek nasıl bilmeniz gerekir.
- Azure için sunulan çeşitli destek planları tanıyın. Bunlar yanıt süreleri Premier için geliştirici örnekleri, ayrılmış bir yanıt süresi 15 dakikadan kısa desteğiyle çeşitli.


![Destek planları](./media/migrate-best-practices-security-management/support.png)
*destek planları*

**Daha fazla bilgi edinin:**
- [Genel bakışın](https://azure.microsoft.com/support/options/) Azure destek planları.
- [Hakkında bilgi edinin](https://azure.microsoft.com/support/legal/sla/) hizmet düzeyi sözleşmeleri (SLA'lar).

## <a name="best-practice-manage-updates"></a>En iyi yöntem: Güncelleştirmeleri yönetme

Azure Vm'leri tutma güncelleştirildi en son işletim sistemi ve yazılım güncelleştirmeleri olan çok büyük bir işi. Güncelleştirmeleri son derece değerli ihtiyaç duydukları hangi güncelleştirmeleri ve anında iletme otomatik olarak bulmak için tüm sanal makineler ortaya çıkarma yeteneğinin.

- Azure Otomasyonu'nda güncelleştirme yönetimi, azure'da, şirket içinde ve diğer bulut sağlayıcılarının dağıtılan Windows ve Linux bilgisayarlarda çalışan makinelerin işletim sistemi güncelleştirmelerini yönetmek için kullanabilirsiniz. 
- Güncelleştirme yüklemesi yönetme ve güncelleştirme yönetimi tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızlı bir şekilde değerlendirmek için kullanın.
- Doğrudan bir Azure Otomasyonu hesabınızdan sanal makineler için güncelleştirme yönetimini etkinleştirebilirsiniz. Tek bir VM Azure portalındaki VM sayfasından da güncelleştirebilirsiniz.
- Ayrıca, Azure Vm'leri, System Center Configuration Manager ile kaydedilebilir. Ardından Configuration Manager iş yükünü Azure'a geçirme ve tek bir web arabiriminden raporlama ve yazılım güncelleştirmeleri yapmak.


![Sanal makine güncelleştirmeleri](./media/migrate-best-practices-security-management/updates.png)
*güncelleştirmeleri*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/automation/automation-update-management) güncelleştirme yönetimi azure'da.
- [Bilgi edinmek için nasıl](https://docs.microsoft.com/azure/automation/oms-solution-updatemgmt-sccmintegration) Configuration Manager güncelleştirme yönetimi ile tümleştirme.
- [Sık sorulan sorular](https://docs.microsoft.com/sccm/core/understand/configuration-manager-on-azure) azure'da Configuration Manager ile ilgili.


## <a name="implement-a-change-management-process"></a>Değişiklik Yönetimi işlemini uygulamasını

Tüm üretim sistemi olduğu gibi her tür değişiklik yapmadan ortamınızı etkileyebilir. Üretim sistemlerine değişiklik yapmak gönderilmesi için istekleri gerektiren bir değişim Yönetimi sürecinin geçirilen ortamınızdaki değerli bir ektir.

- Tanıma yöneticileri Yükselt ve personel desteklemek değişiklik yönetimi için en iyi uygulama çerçeveleri oluşturabilirsiniz.
- Azure Otomasyonu, yapılandırma yönetimi ile yardımcı ve geçirilen iş akışlarınızı için değişiklik izleme için kullanabilirsiniz.
- Değişiklik Yönetimi işlemini zorlamayı, Azure değişiklik bağlamak için denetim günlüklerini kullanabilirsiniz günlükleri büyük olasılıkla (veya etkinleştirmezsiniz) mevcut değişiklik istekleri. Böylece karşılık gelen bir değişiklik isteğinin yapılan bir değişikliği görürseniz, işlemde çıktığına araştırabilirsiniz.

Azure, Azure automation'da bir değişiklik izleme çözümü vardır:

- Çözüm, Windows ve Linux yazılım ve dosyalar, Windows kayıt defteri anahtarlarını, Windows Hizmetleri ve Linux Daemon'ları için değişiklikleri izler.
- İzlenen sunucular üzerinde değişiklikler, işleme için bulutta Azure İzleyici'hizmetine gönderilir.
- Mantıksal alınan verilere uygulanır ve bulut hizmeti olan verileri kaydeder.
- Değişiklik izleme panosunda, sunucu altyapınızda yapılan değişiklikler kolayca görebilirsiniz.


![Değişiklik Yönetimi](./media/migrate-best-practices-security-management/change.png)
*değiştirme Yönetimi*

**Daha fazla bilgi edinin:**

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/automation/automation-change-tracking) değişiklik izleme.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/automation/automation-intro) Azure Otomasyon özellikleri.




## <a name="next-steps"></a>Sonraki adımlar 

Diğer en iyi uygulamaları gözden geçirin:


- [En iyi uygulamalar](migrate-best-practices-networking.md) geçişten sonra ağ.
- [En iyi uygulamalar](migrate-best-practices-costs.md) geçişten sonra maliyet yönetimi için.








