---
title: Örnek - ISO 27001 ASE/SQL iş yükü şema - denetim eşleme
description: Azure İlkesi ve RBAC ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema örnek eşleme denetimi.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: fa7dfbf9e535d010675942900bad208d3f15e556
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698394"
---
# <a name="control-mapping-of-the-iso-27001-asesql-workload-blueprint-sample"></a>ISO 27001 ASE/SQL iş yükü şema örnek denetimi eşleme

Aşağıdaki makalede, Azure şemaları ISO 27001 ASE/SQL iş yükü şema örnek ISO 27001 denetimlerinin nasıl eşlendiğini ayrıntıları. Denetimler hakkında daha fazla bilgi için bkz. [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html).

Şu eşlemeler üzeresiniz **ISO 27001: 2013** kontrol eder. Gezinti, doğrudan bir özel denetim eşlemesi atlamak için sağ taraftaki kullanın. Eşlenen denetimleri birçoğu ile uygulanan bir [Azure İlkesi](../../../policy/overview.md) girişim. Tam girişim gözden geçirmek için açık **ilke** Azure portal ve select **tanımları** sayfası. Daha sonra bulmak ve seçmek  **[Önizleme] denetim ISO 27001: 2013 denetler ve denetim gereksinimlerini desteklemek için belirli VM uzantılarını dağıtma** yerleşik ilke girişimi.

## <a name="a612-segregation-of-duties"></a>Görev ayrımı prensibini A.6.1.2

Tek bir Azure aboneliği sahibi olan Yönetim yedeklilik için izin vermez. Buna karşılık, çok fazla Azure aboneliğine sahip olan, güvenliği aşılmış sahip hesabı aracılığıyla bir ihlal olasılığını artırabilir. Bu şema iki atayarak Azure abonelik sahipleri uygun sayıda tutmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) denetim sahipleri Azure aboneliklerinin sayısını tanımlar. Abonelik sahibi izinleri yönetme, uygun görev ayrımı uygulamak yardımcı olabilir.

- [Önizleme]: Audit minimum number of owners for subscription
- [Önizleme]: Audit maximum number of owners for a subscription

## <a name="a821-classification-of-information"></a>A.8.2.1 sınıflandırması bilgileri

Azure'nın [SQL güvenlik açığı değerlendirme hizmeti](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) veritabanınızda depolanan hassas verileri keşfetmenize yardımcı olabilir ve bu verileri sınıflandırmak için öneriler içerir. Bu şema atar bir [Azure İlkesi](../../../policy/overview.md) SQL güvenlik açığı değerlendirmesi taraması sırasında güvenlik açıkları düzeltildiğinden denetleme tanımı.

- [Önizleme]: Monitor SQL vulnerability assessment results in Azure Security Center

## <a name="a912-access-to-networks-and-network-services"></a>Ağ ve ağ hizmetlerini A.9.1.2 erişimi

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) Azure kaynaklarına kimlerin erişebildiğini yönetmek için (RBAC). Bu şema yardımcı olan yedi atayarak Azure kaynaklarına erişimi denetlemek [Azure İlkesi](../../../policy/overview.md) tanımlar. Bu ilkeler kaynak türlerinin kullanımını denetleyin ve daha esnek olarak izin verebileceği yapılandırmaları kaynaklarına erişim.
Yetkili kullanıcıların Azure kaynaklarına erişim sağlamak için düzeltici eylemleri Yardım kısıtlanmış, bu ilkeleri can ihlal anlama kaynaklar.

- [Önizleme]: Deploy VM extension to audit Linux VM accounts with no passwords
- [Önizleme]: Deploy VM extension to audit Linux VM allowing remote connections from accounts with no
  Parolalars
- [Önizleme]: Audit Linux VM accounts with no passwords
- [Önizleme]: Audit Linux VM allowing remote connections from accounts with no passwords
- Klasik depolama hesaplarının kullanımını denetleyin
- Klasik sanal makinelerin kullanımını denetleyin
- Yönetilen diskler kullanmayan VM'leri denetle

## <a name="a923-management-of-privileged-access-rights"></a>A.9.2.3 Yönetimi ayrıcalıklı erişim hakları

Bu şema kısıtlamak ve ayrıcalıklı erişim hakları dört atayarak denetlemenize yardımcı olur. [Azure İlkesi](../../../policy/overview.md) sahip dış hesapların denetim ve/veya izinleri ve hesapları sahip yazma ve/veya yazma izinleri tanımları Bu, çok faktörlü kimlik doğrulaması etkin yok. Azure rol tabanlı erişim denetimi (RBAC) kimlerin erişebildiğini yönetmek için Azure kaynaklarına uygular. Bu şema SQL sunucuları ve Service Fabric için Azure Active Directory kimlik doğrulaması kullanımını denetlemek için üç Azure İlkesi tanım ayrıca atar. Azure Active Directory'yi kullanarak kimlik doğrulaması, Basitleştirilmiş izin yönetimi ve veritabanı kullanıcıları ve diğer Microsoft hizmetlerinde merkezi kimlik yönetimi sağlar. Bu plan, ayrıca özel bir RBAC kurallar kullanımını denetlemek için bir Azure İlkesi tanım atar. Özel bir RBAC kurallar hata yapmaya açık olarak özel bir RBAC kuralları uygulamak nerede anlama, gereksinim ve uygun uygulama doğrulamanıza yardımcı olabilir.

- [Önizleme]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Önizleme]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Önizleme]: Audit external accounts with owner permissions on a subscription
- [Önizleme]: Audit external accounts with write permissions on a subscription
- SQL sunucusuna Azure Active Directory Yöneticisi sağlanmasını denetleyin.
- Service Fabric'te istemci kimlik doğrulaması için Azure Active Directory kullanımını denetleyin
- Özel RBAC kurallarının kullanımını denetleyin

## <a name="a924-management-of-secret-authentication-information-of-users"></a>Kullanıcıların gizli kimlik bilgilerinin A.9.2.4 Yönetimi

Bu plan üç atar [Azure İlkesi](../../../policy/overview.md) çok faktörlü kimlik doğrulaması etkin olmayan hesaplar denetlemek için tanımları. Çok faktörlü kimlik doğrulaması, kimlik doğrulama bilgilerini bir parça tehlikede olsa bile hesapları korunmasına yardımcı olur. Çok faktörlü kimlik doğrulama etkin olmadan hesapları izleyerek, büyük olasılıkla tehlikeye hesapları tanımlayabilirsiniz. Bu şema, ayrıca Linux VM denetim iki Azure ilke tanımları yanlış ayarladıysanız, sizi uyarmak için parola dosya izinleri atar. Bu ayar, kimlik doğrulayıcılar tehlikede olmayan emin olmak için düzeltici eylemlerde sağlar.

- [Önizleme]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Önizleme]: Audit accounts with read permissions who are not MFA enabled on a subscription
- [Önizleme]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Önizleme]: Deploy VM extension to audit Linux VM passwd file permissions
- [Önizleme]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="a925-review-of-user-access-rights"></a>Kullanıcı erişim haklarını A.9.2.5 gözden geçirme

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) (RBAC) Azure'da kimlerin erişebileceğini kaynakları yönetmek için yardımcı olur. Azure portalını kullanarak, kimlerin erişebileceğini Azure kaynaklarını ve izinlerini gözden geçirebilirsiniz. Bu şema dört atar [Azure İlkesi](../../../policy/overview.md) tanımlarını gözden geçirme için amorti edilmiş ve yükseltilmiş izinleri olan dış hesapları dahil olmak üzere, öncelik verilmesi gerektiği hesapları denetleme.

- [Önizleme]: Audit deprecated accounts on a subscription
- [Önizleme]: Audit deprecated accounts with owner permissions on a subscription
- [Önizleme]: Audit external accounts with owner permissions on a subscription
- [Önizleme]: Audit external accounts with write permissions on a subscription

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A.9.2.6 kaldırma veya erişim haklarını ayarlama

Azure uygular [rol tabanlı erişim denetimi](../../../../role-based-access-control/overview.md) (RBAC) Azure'da kimlerin erişebileceğini kaynakları yönetmek için yardımcı olur. Kullanarak [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) ve RBAC, kullanıcı rolleri, kuruluş değişiklikleri yansıtacak şekilde güncelleştirebilirsiniz. Gerektiğinde, hesapları oturum açma engellendi (veya kaldırıldı) hangi Azure kaynakları için erişim haklarını hemen kaldırır. Bu şema iki atar [Azure İlkesi](../../../policy/overview.md) kaldırılmak üzere davranılacak amorti edilmiş hesabı denetlemek için tanımları.

- [Önizleme]: Audit deprecated accounts on a subscription
- [Önizleme]: Audit deprecated accounts with owner permissions on a subscription

## <a name="a942-secure-log-on-procedures"></a>Oturum açma A.9.4.2 güvenli yordamları

Bu plan, çok faktörlü kimlik doğrulaması etkin olmayan hesaplar denetlemek için üç Azure İlkesi tanım atar. Azure multi-Factor Authentication ikinci bir form kimlik doğrulaması gerektirerek ek güvenlik sağlar ve güçlü kimlik doğrulaması sunar. Çok faktörlü kimlik doğrulama etkin olmadan hesapları izleyerek, büyük olasılıkla tehlikeye hesapları tanımlayabilirsiniz.

- [Önizleme]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Önizleme]: Audit accounts with read permissions who are not MFA enabled on a subscription
- [Önizleme]: Audit accounts with write permissions who are not MFA enabled on a subscription

## <a name="a943-password-management-system"></a>A.9.4.3 parola yönetimi sistemi

Bu şema 10 atayarak güçlü parolalar zorunlu yardımcı olan [Azure İlkesi](../../../policy/overview.md) en az gücü ve diğer parola gereksinimlerini zorlamaz; Windows Vm'leri denetle tanımlar. Parola gücü ilkeyi ihlal VM'lerin tanıma tüm VM kullanıcı hesapları için parola ilkesiyle uyumlu olduğundan emin olmak için düzeltici eylemleri yardımcı olur.

- [Önizleme]: Deploy VM extension to audit Windows VM enforces password complexity requirements
- [Önizleme]: Deploy VM extension to audit Windows VM maximum password age 70 days
- [Önizleme]: Deploy VM extension to audit Windows VM minimum password age 1 day
- [Önizleme]: Deploy VM extension to audit Windows VM passwords must be at least 14 characters
- [Önizleme]: Deploy VM extension to audit Windows VM should not allow previous 24 passwords
- [Önizleme]: Audit Windows VM enforces password complexity requirements
- [Önizleme]: Audit Windows VM maximum password age 70 days
- [Önizleme]: Audit Windows VM minimum password age 1 day
- [Önizleme]: Audit Windows VM passwords must be at least 14 characters
- [Önizleme]: Audit Windows VM should not allow previous 24 passwords

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>A.10.1.1 İlkesi kullanımı için şifreleme denetimleri

Bu şema 13 atayarak, politikasını cryptograph denetimlerin kullanımına yardımcı olan [Azure İlkesi](../../../policy/overview.md) belirli cryptograph denetimlerini ve denetim zorunlu tanımları zayıf şifreleme ayarları kullanın.
Burada Azure kaynaklarınıza uygun olmayan şifreleme yapılandırmaları olabilir kaynakların, bilgi Güvenlik ilkenize uygun şekilde yapılandırıldığından emin olmak için düzeltici eylemleri yardımcı olabileceğini anlama. Özellikle, bu şema tarafından atanan ilkeler, blob depolama hesapları ve data lake depolama hesapları için şifreleme gerektiren; SQL veritabanlarında saydam veri şifrelemesi; Depolama hesapları, SQL veritabanları, sanal makine disklerini ve Otomasyon hesabı değişkenleri eksik şifrelemesini denetle; Depolama hesapları, işlev uygulamaları, Web uygulaması, API Apps ve Redis Cache için güvenli bağlantıları denetim; sanal makine zayıf parola şifrelemesini denetler; ve Service Fabric şifrelenmemiş iletişimin denetleyebilirsiniz.

- [Önizleme]: Audit HTTPS only access for a Function App
- [Önizleme]: Audit HTTPS only access for a Web Application
- [Önizleme]: Audit HTTPS only access for an API App
- [Önizleme]: Audit missing blob encryption for storage accounts
- [Önizleme]: Deploy VM extension to audit Windows VM should not store passwords using reversible
  şifrelemen
- [Önizleme]: Audit Windows VM should not store passwords using reversible encryption
- [Önizleme]: Monitor unencrypted SQL database in Azure Security Center
- [Önizleme]: Monitor unencrypted VM Disks in Azure Security Center
- Otomasyon hesabı değişkenlerinin şifrelemesinin etkinleştirilmesini denetleyin
- Redis Cache önbelleğinizde yalnızca güvenli bağlantıların etkinleştirilmesini denetleyin
- Depolama hesaplarına güvenli aktarımı denetleyin
- Service Fabric'te ClusterProtectionLevel özelliğinin EncryptAndSign olarak ayarlanmasını denetleyin
- Saydam veri şifreleme durumunu denetle

## <a name="a1241-event-logging"></a>A.12.4.1 olay günlüğü

Bu şema sistem olaylarını yedi atayarak kaydedilir olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) Azure kaynaklarını denetim günlüğü ayarlarını tanımlar.
Tanılama günlükleri, Azure kaynaklarını içinde gerçekleştirilen işlemler hakkında bilgi sağlar.

- [Önizleme]: Denetim bağımlılık Aracısı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Bağımlılık Aracısı VMSS - VM görüntüsü (OS) listelenmemiş dağıtımda denetleme
- [Önizleme]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Monitor unaudited SQL database in Azure Security Center
- Tanılama ayarını denetle
- SQL sunucu düzeyi Denetim ayarlarını denetle

## <a name="a1243-administrator-and-operator-logs"></a>Günlükleri A.12.4.3 yönetici and işleci

Bu şema sistem olaylarını, Azure kaynaklarını denetim günlüğü ayarlarını yedi Azure ilke tanımları atayarak oturum açmış olmanıza yardımcı olur. Tanılama günlükleri, Azure kaynaklarını içinde gerçekleştirilen işlemler hakkında bilgi sağlar.

- [Önizleme]: Denetim bağımlılık Aracısı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Bağımlılık Aracısı VMSS - VM görüntüsü (OS) listelenmemiş dağıtımda denetleme
- [Önizleme]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Monitor unaudited SQL database in Azure Security Center
- Tanılama ayarını denetle
- SQL sunucu düzeyi Denetim ayarlarını denetle

## <a name="a1244-clock-synchronization"></a>A.12.4.4 saati eşitleme

Bu şema sistem olaylarını denetleme, günlük ayarlarını Azure kaynaklarında yedi Azure İlkesi tanımları atayarak oturum açtığınızdan emin olun yardımcı olur. Azure günlüklerini eşitlenmiş iç saat kaynakları genelinde olayların süresi bağıntılı bir kayıt oluşturmak için kullanır.

- [Önizleme]: Denetim bağımlılık Aracısı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Bağımlılık Aracısı VMSS - VM görüntüsü (OS) listelenmemiş dağıtımda denetleme
- [Önizleme]: Denetim günlüğü analiz aracı dağıtımı - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Denetim günlüğü analiz aracı dağıtım VMSS - VM görüntüsü (OS) listeden kaldırıldı
- [Önizleme]: Monitor unaudited SQL database in Azure Security Center
- Tanılama ayarını denetle
- SQL sunucu düzeyi Denetim ayarlarını denetle

## <a name="a1251-installation-of-software-on-operational-systems"></a>İşletim sistemlerine yazılım A.12.5.1 yüklemesi

Uyarlamalı uygulama denetimlerini, Azure Güvenlik Merkezi'nden, Azure'da yer alan sanal makineleriniz uygulamaları denetlemenize yardımcı çalıştırabilirsiniz çözümüdür. Bu plan için izin verilen uygulamalar yapılan değişiklikleri izleyen bir Azure İlkesi tanım atar. Bu özellik, yardımcı olur. Denetim, yazılım yüklemesini ve Azure vm'lerdeki uygulamalar.

- [Önizleme]: Monitor possible app Whitelisting in Azure Security Center

## <a name="a1261-management-of-technical-vulnerabilities"></a>Teknik güvenlik açıklarının A.12.6.1 Yönetimi

Bu şema beş atayarak bilgi sistemi güvenlik açıkları yönetmenize yardımcı olan [Azure İlkesi](../../../policy/overview.md) eksik sistem güncelleştirmeleri, işletim sistemi güvenlik açıkları, SQL güvenlik açıkları ve sanal makine izleyen tanımları Azure Güvenlik Merkezi'nde güvenlik açıklarını. Azure Güvenlik Merkezi, dağıtılan Azure kaynaklarınızın güvenlik durumunu gerçek zamanlı öngörülere sahip olmanıza olanak sağlayan bir raporlama yetenekleri sağlar.

- [Önizleme]: Monitor missing Endpoint Protection in Azure Security Center
- [Önizleme]: Monitor missing system updates in Azure Security Center
- [Önizleme]: Monitor OS vulnerabilities in Azure Security Center
- [Önizleme]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Önizleme]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="a1262-restrictions-on-software-installation"></a>Yazılım yükleme A.12.6.2 kısıtlamaları

Uyarlamalı uygulama denetimlerini, Azure Güvenlik Merkezi'nden, Azure'da yer alan sanal makineleriniz uygulamaları denetlemenize yardımcı çalıştırabilirsiniz çözümüdür. Bu plan için izin verilen uygulamalar yapılan değişiklikleri izleyen bir Azure İlkesi tanım atar. Yazılım yükleme kısıtlamaları yazılım güvenlik açıklarından giriş olasılığını azaltmaya yardımcı olabilir.

- [Önizleme]: Monitor possible app Whitelisting in Azure Security Center

## <a name="a1311-network-controls"></a>A.13.1.1 ağ denetimleri

Bu şema yönetmenize ve denetlemenize ağları atayarak yardımcı olan bir [Azure İlkesi](../../../policy/overview.md) , esnek kurallara sahip ağ güvenlik grupları izleyen tanımı. Çok esnek olan kuralları istenmeyen ağ erişimini izin verebilir ve gözden geçirilmesi gerekir. Bu şema korumasız uç noktalar, uygulamaları ve depolama hesaplarına izleyen üç Azure İlkesi tanım ayrıca atar. Uç noktaları ve güvenlik duvarı ve depolama hesapları ile sınırsız erişim tarafından korunmayan uygulamalar bilgi sistemi içinde yer alan bilgileri istenmeyen erişime izin verebilir.

- [Önizleme]: Monitor permissive network access in Azure Security Center
- [Önizleme]: Monitor unprotected network endpoints in Azure Security Center
- [Önizleme]: Monitor unprotected web application in Azure Security Center
- Depolama hesaplarına kısıtlanmamış ağ erişimini denetleyin

## <a name="a1321-information-transfer-policies-and-procedures"></a>A.13.2.1 bilgi aktarımı politikaları ve yordamları

Blueprint bilgi aktarımı Azure Hizmetleri ile güvenli iki atayarak olmanıza yardımcı olur. [Azure İlkesi](../../../policy/overview.md) depolama hesapları ve Redis önbelleği için güvenli bağlantıları denetlemek için tanımları.

- Redis Cache önbelleğinizde yalnızca güvenli bağlantıların etkinleştirilmesini denetleyin
- Depolama hesaplarına güvenli aktarımı denetleyin

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema örnek denetimi eşleme gözden geçirdikten sonra mimarisi ve bu örneğin nasıl dağıtılacağını öğrenmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 App Service ortamı/SQL veritabanı iş yükü şema - genel bakış](./index.md)
> [ISO 27001 App Service ortamı/SQL veritabanı iş yükü blueprint - adımları dağıtma](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.