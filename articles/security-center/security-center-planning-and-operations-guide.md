---
title: Güvenlik Merkezi Planlama ve İşlemler Kılavuzu | Microsoft Belgeleri
description: Bu belge, Azure Güvenlik Merkezi'ni benimsemeden önce planlama ve günlük işlemlere ilişkin dikkat edilmesi gerekenler konusunda yardımcı olur.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: f984e4a2-ac97-40bf-b281-2f7f473494c4
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/11/2019
ms.author: v-mohabe
ms.openlocfilehash: 04cfe489e9eea53bf58dd64e0eac3e5a95033bcc
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65966849"
---
# <a name="azure-security-center-planning-and-operations-guide"></a>Azure Güvenlik Merkezi planlama ve işlemler kılavuzu
Bu kılavuz, kurumları Azure Güvenlik Merkezi'ni kullanmayı planlayan bilgi teknolojisi (BT) uzmanları, BT mimarları, bilgi güvenlik çözümleyicileri ve bulut yöneticilerine yöneliktir.


## <a name="planning-guide"></a>Planlama Kılavuzu
Bu kılavuzda, kuruluşunuzun güvenlik gereksinimlerine ve bulut yönetimi modeline dayanarak Güvenlik Merkezi kullanımınızı iyileştirmek için izleyeceğiniz adımlar ve görevlerin kümesi sağlanmıştır. Güvenlik Merkezi'nin tüm avantajlarından yararlanabilmek için kurumunuzdaki farklı kişilerin veya ekiplerin güvenli geliştirmenin yanı sıra işlem, izleme, yönetim ve olay yanıtı gereksinimlerini karşılamak amacıyla hizmeti nasıl kullandığının anlaşılması oldukça önemlidir. Güvenlik Merkezi'ni kullanmayı planlarken dikkate alınması gereken temel alanlar şunlardır:

* Güvenlik Rolleri ve Erişim Denetimleri
* Güvenlik İlkeleri ve Öneriler
* Veri Koleksiyonu ve Storage
* Devam eden Azure dışı kaynaklar
* Devam Eden Güvenlik İzleme
* Olay Yanıtı

Sonraki bölümde, gereksinimlerinize bağlı olarak bu alanların her birini nasıl planlayacağınızı ve bu önerileri nasıl uygulayacağınızı öğreneceksiniz.


> [!NOTE]
> Tasarlama ve planlama aşamasında da faydalı olabilecek sık sorulan soruların bir listesi için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).
>

## <a name="security-roles-and-access-controls"></a>Güvenlik rolleri ve erişim denetimleri
Kuruluşunuzun büyüklüğüne ve yapısına bağlı olarak birçok kişi ve ekip, güvenlikle ilgili farklı görevleri gerçekleştirmek için Güvenlik Merkezi'ni kullanabilir. Aşağıdaki diyagramda, hayali kişiler ile bunların rollerinin ve güvenlik sorumluluklarının bir örneğini bulabilirsiniz:

![Roller](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig01-new.png)

Güvenlik Merkezi, bu çok çeşitli sorumlulukları karşılamak için kişileri etkinleştirir. Örneğin:

**Cem (İş Yükü Sahibi)**

* Bir bulut iş yükünü ve onunla ilgili kaynakları yönetme
* Korumaları şirket güvenlik ilkesiyle uyumlu bir şekilde uygulamak ve sürdürmekle sorumlu

**Emel (CISO/CIO)**

* Şirket güvenliğinin tüm boyutlarından sorumlu
* Bulut iş yüklerinin tamamında şirketin güvenlik duruşunu anlamak istiyor
* Önemli saldırı ve risklerden haberdar olması gerekiyor

**Ali (BT Güvenliği)**

* Uygun korumaların uygulanmakta olduğundan emin olmak için şirketin güvenlik ilkelerini ayarlar
* İlkelerle uyumluluğu izler
* Yöneticiler veya denetçiler için raporlar oluşturur

**Zehra (Güvenlik İşlemleri)**

* Güvenlik uyarılarını 7/24 izler ve bunlara yanıt verir
* Bulut İş Yükü Sahibi veya BT Güvenlik Analistine İletir

**Salih (Güvenlik Analisti)**

* Atakları araştırır
* Bulut İş Yükü Sahibi ile birlikte çalışarak düzeltme uygulama

Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen [yerleşik roller](../role-based-access-control/built-in-roles.md) sağlayan [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../role-based-access-control/role-assignments-portal.md) kullanır. Bir kullanıcı Güvenlik Merkezi’ni açtığında, yalnızca erişimi olan kaynaklarla ilişkili bilgileri görüntüleyebilir. Bu da bir kaynağın ait olduğu abonelik veya kaynak grubu için kullanıcıya Sahip, Katkıda Bulunan veya Okuyucu rolünün atandığı anlamına gelir. Bu rollere ek olarak iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucusu**: Bu role ait kullanıcı; öneriler, uyarılar, ilke ve sistem durumunu içeren Güvenlik Merkezi yapılandırmalarını yalnızca görüntüleyebilir, herhangi bir değişiklik yapamaz.
- **Güvenlik yöneticisi**: Güvenlik okuyucusu ile aynıdır, ancak aynı zamanda güvenlik ilkesini güncelleştirebilir ve öneriler ile uyarıları kapatabilir.

Yukarıda açıklanan Güvenlik Merkezi rolleri, Azure’un Depolama, Web ve Mobil veya Nesnelerin İnterneti gibi diğer hizmet alanlarına erişemez.  

Önceki diyagramda açıklanan kişiler kullanıldığında aşağıdaki RBAC gerekli olur:

**Cem (İş Yükü Sahibi)**

* Kaynak Grubu Sahibi/Ortak Çalışanı

**Ali (BT Güvenliği)**

* Abonelik Sahibi/Ortak Çalışan veya Güvenlik Yöneticisi

**Zehra (Güvenlik İşlemleri)**

* Uyarıları görüntülemek için Abonelik Okuyucusu veya Güvenlik Okuyucusu
* Uyarıları kapatmak için Abonelik Sahibi/Ortak Çalışanı veya Güvenlik Okuyucusu gerekir

**Salih (Güvenlik Analisti)**

* Uyarıları görüntülemek için Abonelik Okuyucusu
* Uyarıları kapatmak Abonelik Sahibi/Ortak Çalışanı gerekir
* Çalışma alanına erişim gerekli olabilir

Dikkate alınması gereken bazı diğer önemli bilgiler:

* Güvenlik ilkesini yalnızca abonelik Sahipleri/Katkıda Bulunanları ve Güvenlik Yöneticileri düzenleyebilir.
* Yalnızca abonelik ve kaynak grubu Sahipleri ve Katkıda bulunanları bir kaynak için güvenlik önerilerini uygulayabilir.

Güvenlik Merkezi için RBAC kullanarak erişim denetimini planlarken Güvenlik Merkezi'ni kuruluşunuzdaki hangi kişilerin kullanacağını anladığınızdan emin olun. Ayrıca, bunların hangi türde görevler gerçekleştireceğini anlayın ve daha sonra RBAC’yi buna göre yapılandırın.

> [!NOTE]
> Kullanıcılara, görevlerini tamamlamak için gereken rolleri en alt seviyede esneklik sunacak şekilde atamanızı öneririz. Örneğin, yalnızca kaynakların güvenlik durumu hakkındaki bilgileri görüntülemesi gereken ancak eyleme geçmeyecek olan kullanıcıların (örneğin, önerileri uygulamak veya ilkeleri düzeltmek), Okuyucu rolüne atanmaları gerekir.
>
>

## <a name="security-policies-and-recommendations"></a>Güvenlik ilkeleri ve öneriler
Güvenlik ilkesi iş yüklerinizin istenen yapılandırmasını tanımlar ve şirketin veya yasal düzenlemelerin gerektirdiği güvenlik gereksinimlerine uyum sağlanmasına yardımcı olur. Güvenlik Merkezi'nde Azure aboneliğinize yönelik ilkeleri tanımlayabilir, bunları iş yükü türüne veya veri gizlilik düzeyine göre ayarlayabilirsiniz.

Güvenlik Merkezi ilkeleri aşağıdaki bileşenleri içerir:
- [Veri toplama](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection): Aracı sağlama ve veri toplama ayarları.
- [Güvenlik İlkesi](https://docs.microsoft.com/azure/security-center/security-center-policies): bir [Azure İlkesi](../governance/policy/overview.md) hangi denetimlerin izlenir ve Güvenlik Merkezi veya Azure İlkesi kullanımı, yeni tanımları oluşturmak için önerilen ek ilke tanımlama ve ilkeleri atamak belirler Yönetim grupları arasında.
- [E-posta bildirimleri](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details): Güvenlik ilgili kişileri ve bildirim ayarları.
- [Fiyatlandırma katmanı](https://docs.microsoft.com/azure/security-center/security-center-pricing): Kapsam dahilindeki kaynaklar (abonelikler, kaynak grupları ve çalışma alanları için belirtilebilir) için kullanılabilecek olan Güvenlik Merkezi özelliklerini belirleyen ücretsiz veya standart fiyatlandırma katmanı.

> [!NOTE]
> Güvenlik ilgili kişisi belirtmeniz bir güvenlik olayı ortaya çıktığında Azure'un kuruluşunuzdaki doğru kişiyle iletişime geçmesini sağlayacaktır. Bu öneriyi etkinleştirme hakkında daha fazla bilgi için [Azure Güvenlik Merkezi’nde güvenlik kişi ayrıntılarını sağlama](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details) konusunu okuyun.

### <a name="security-policies-definitions-and-recommendations"></a>Güvenlik ilkelerinin tanımları ve öneriler
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. İlkeyi Güvenlik Merkezi'nde düzenleyebilir veya Azure ilke yönetimini kullanarak yeni tanım oluşturabilir, ek ilke tanımlayabilir, ilkeleri farklı Yönetim Gruplarına (kuruluşun tamamı, tek bir iş birimi vs. olabilir) atayabilir ve bu kapsamlarda bu ilkelere uyum sağlanıp sağlanmadığını izleyebilirsiniz.

Güvenlik ilkelerini yapılandırmadan önce her bir [güvenlik önerisini](https://docs.microsoft.com/azure/security-center/security-center-recommendations) gözden geçirip bu ilkelerin sahip olduğunuz çeşitli abonelikler ve kaynak grupları için uygun olup olmadığını belirleyin. Güvenlik Önerilerini ele almak için hangi eylemlerde bulunulacağını ve kuruluşunuzda yeni önerileri izlemekten ve gerekli adımların atılmasından kimin sorumlu olacağını anlamak da önemlidir.

## <a name="data-collection-and-storage"></a>Veri koleksiyonu ve depolama
Azure Güvenlik Merkezi, Microsoft Monitoring Agent kullanır: sanal makinelerinizden güvenlik verilerini toplamak için Azure İzleyici hizmeti tarafından kullanılan aracının aynısı budur. Bu aracıdan [toplanan veriler](https://docs.microsoft.com/azure/security-center/security-center-enable-data-collection), Log Analytics çalışma alanlarınızda depolanır.

### <a name="agent"></a>Aracı

Güvenlik ilkesinde otomatik sağlama etkinleştirildikten sonra, desteklenen tüm Azure VM'lere ve oluşturulan tüm yeni VM'lere Microsoft Monitoring Agent ([Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) veya [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) için) yüklenir. VM'de veya bilgisayarda zaten Microsoft Monitoring Agent yüklüyse, Azure Güvenlik Merkezi yüklü olan aracıyı kullanır. Aracının işlemi müdahaleci olmayacak şekilde tasarlanmıştır ve VM performansı üzerinde çok az etki yapar.

Windows için Microsoft Monitoring Agent, TCP bağlantı noktası 443’ün kullanılmasını gerektirir. Daha fazla bilgi için [Sorun giderme makalesine](security-center-troubleshooting-guide.md) bakın.

Belirli bir noktada Veri Koleksiyonu'nu devre dışı bırakmak isterseniz koleksiyonu güvenlik ilkesinden kapatabilirsiniz. Ancak, Microsoft Monitoring Agent diğer Azure yönetim ve izleme hizmetleri tarafından kullanılıyor olabileceğinden, Güvenlik Merkezi’nde veri toplamayı kapattığınızda aracı otomatik olarak kaldırılmayabilir. Gerekirse aracıyı el ile kaldırabilirsiniz.

> [!NOTE]
> Desteklenen VM'lerin listesini bulmak için bkz. [Azure Güvenlik Merkezi ile ilgili sık sorulan sorular (SSS)](security-center-faq.md).
>

### <a name="workspace"></a>Çalışma alanı

Çalışma alanı, veri kapsayıcısı olarak görev yapan bir Azure kaynağıdır. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Microsoft Monitoring Agent’tan Azure Güvenlik Merkezi adına toplanan veriler, sanal makinenin Coğrafi konumu göz önünde bulundurularak Azure aboneliğinizle ilişkili Log Analytics çalışma alanlarında veya yeni çalışma alanlarında depolanır.

Azure portalında, Azure Güvenlik Merkezi tarafından oluşturulanlar dahil olmak üzere Log Analytics çalışma alanlarınızın listesine göz atabilirsiniz. Yeni çalışma alanları için bir ilgili kaynak grubu oluşturulur. İkisi de şu adlandırma kuralını izler:

* Çalışma alanı: *DefaultWorkspace-[abonelik-kimliği]-[Bölge]*
* Kaynak Grubu: *DefaultResourceGroup-[Bölge]*

Azure Güvenlik Merkezi tarafından oluşturulan çalışma alanları için veriler 30 gün boyunca tutulur. Mevcut çalışma alanları için elde tutma süresi, çalışma alanının fiyatlandırma katmanını temel alır. İsterseniz var olan bir çalışma alanını kullanabilirsiniz.

> [!NOTE]
> Microsoft, bu verilerin gizlilik ve güvenliğini korumak için önemli taahhütlerde bulunur. Microsoft kodlamadan hizmet çalıştırma konularına kadar her alanda uyumluluk ve güvenlik yönergelerine kesin olarak bağlı kalmaktadır. Veri işleme ve gizlilik hakkında daha fazla bilgi için [Azure Güvenlik Merkezi Veri Güvenliği](security-center-data-security.md) makalesini okuyun.
>

## <a name="onboarding-non-azure-resources"></a>Azure dışı kaynakları ekleme

Güvenlik Merkezi, Azure dışı bilgisayarların güvenlik durumunu izleyebilir ancak öncelikle bu kaynakları eklemeniz gerekir. Azure dışı kaynakları nasıl ekleyeceğiniz hakkında daha fazla bilgi almak için [Gelişmiş güvenlik için Azure Güvenlik Merkezi Standart'a kaynak ekleme](https://docs.microsoft.com/azure/security-center/security-center-onboarding#onboard-non-azure-computers) konusuna bakın.

## <a name="ongoing-security-monitoring"></a>Devam eden güvenlik izleme
Güvenlik Merkezi önerilerinin ilk yapılandırması ve uygulamasından sonraki adım, Güvenlik Merkezi işlem süreçlerini dikkate almaktır.

Güvenlik Merkezine Genel Bakış, tüm Azure kaynaklarınızın ve bağladığınız Azure dışı kaynakların birleştirilmiş görünümünü sunar. Aşağıdaki örnekte ilgilenilmesi gereken birçok soruna sahip bir ortam gösterilmektedir:

![pano](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig11.png)

> [!NOTE]
> Güvenlik Merkezi normal işlem yordamlarınıza müdahale etmez, dağıtımlarınızı pasif olarak izler ve etkinleştirdiğiniz güvenlik ilkelerine bağlı olarak öneriler sağlar.

Geçerli Azure ortamınız için Güvenlik Merkezi’ni kullanmayı ilk kez seçtiğinizde, **Öneriler** kutucuğunda yer alan veya kaynak başına (**İşlem**, **Ağ**, **Depolama ve Veriler**, **Uygulama**) sunulan tüm önerileri gözden geçirdiğinizden emin olun.

Tüm önerilere değindikten sonra değinilen tüm kaynaklar için **Önleme** bölümünün yeşil olması gerekir. Yalnızca kaynak güvenlik durumu ve öneriler kutucuklarındaki değişikliklere göre eyleme geçeceğiniz için devam eden izleme bu noktada daha kolay olur.

**Algılama** bölümü daha reaktiftir; bunlar, ya şu anda var olan veya geçmişte oluşan ya da Güvenlik Merkezi denetimleri ile 3. taraf sistemleri tarafından algılanan sorunlarla ilgili uyarılardır. Güvenlik Uyarıları kutucuğu, her gün bulunan tehlike algılama uyarıları sayısını ve bunların farklı önem derecesi kategorilerine (düşük, orta, yüksek) dağılımlarını temsil eden çubuk grafiklerini gösterir. Güvenlik Uyarıları hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve uyarılara yanıt verme](security-center-managing-and-responding-alerts.md)

[Tehdit zekası](https://docs.microsoft.com/azure/security-center/security-center-threat-intel) seçeneğini ziyaret etmeyi günlük işlemlerinizin bir parçası haline getirin. Burada belirli bir bilgisayarın bir botnetin parçası olup olmadığını belirleme gibi ortamdaki güvenlik tehditlerini belirleyebilirsiniz.

### <a name="monitoring-for-new-or-changed-resources"></a>Yeni veya değiştirilmiş kaynakları izleme
Çoğu Azure ortamı, düzenli olarak çalışmaya başlatılan ve yavaşlatılan yeni kaynaklarla dinamiktir (örneğin, yapılandırmalar veya değişiklikler vb.) Güvenlik Merkezi, bu yeni kaynakların güvenlik durumuyla ilgili görünürlüğe sahip olduğunuzdan emin olmanıza yardımcı olur.

Azure ortamınıza yeni kaynaklar (VM'ler, SQL DB'leri) eklediğinizde Güvenlik Merkezi otomatik olarak bu kaynakları keşfeder ve güvenliklerini izlemeye başlar. Buna PaaS web rolleri ve çalışan rolleri de dahildir. Veri Koleksiyonu [Güvenlik İlkesi](tutorial-security-policy.md)'nde etkinleştirilirse sanal makineleriniz için ek izleme işlevleri otomatik olarak etkinleştirilir.

![Temel alanlar](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig3-newUI.png)

1. Sanal makineler için **Önleme** bölümü altındaki **İşlem**’e tıklayın. Verileri etkinleştirme veya ilgili öneriler hakkında tüm sorunlar **Genel Bakış** sekmesinde ve **İzleme Önerileri** bölümünde gösterilir.
2. Yeni kaynak için (eğer varsa) hangi güvenlik risklerinin tanımlandığını görmek için **Öneriler**'i görüntüleyin.
3. Ortamınıza yeni VM'ler eklendiğinde, başlangıçta yalnızca işletim sisteminin yüklenmesi durumu çok yaygındır. Bu VM'ler tarafından kullanılacak diğer uygulamaları dağıtmak için kaynak sahibine bir miktar süre gerekebilir.  İdeal olarak, bu iş yükünün son amacını bilmeniz gerekir. Bu iş yükü bir Uygulama Sunucusu mu olacak? Bu yeni iş yükünün ne olacağına bağlı olarak, bu iş akışındaki üçüncü adım olan uygun **Güvenlik İlkesi**'ni etkinleştirebilirsiniz.
4. Yeni kaynaklar Azure ortamınıza eklendikçe **Güvenlik Uyarıları** kutucuğunda yeni uyarıların görünmesi mümkündür. Bu kutucukta yeni uyarılar olup olmadığını her zaman doğrulayın ve Güvenlik Merkezi önerilerine göre eyleme geçin.

Ayrıca güvenlik riskleri, önerilen temel yapılandırmalardan sapma durumları ve güvenlik uyarıları oluşturan yapılandırma değişikliklerini tanımlamak için var olan kaynakların durumunu düzenli aralıklarla izlemek isteyeceksiniz. Güvenlik Merkezi Panosu'nda başlayın. Tutarlı bir şekilde gözden geçirmek için burada üç temel alanınız vardır.

![İşlemler](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig4-newUI.png)

1. **Önleme** bölüm paneli, temel kaynaklarınıza hızlı erişim sağlar. İşlem, Ağ, Depolama ve veriler ile Uygulamalar’ı izlemek için bu seçeneği kullanın.
2. **Öneriler** paneli Güvenlik Merkezi önerilerini gözden geçirmenizi sağlar. İzleme devam ederken, günlük olarak önerileri almadığınızı fark edebilirsiniz; ilk Güvenlik Merkezi kurulumunda tüm önerileri ele aldığınız için bu durum normaldir. Bundan dolayı bu bölümde her gün yeni bilgilere sahip olmayabilirsiniz ve bilgilere sadece gerekli olduğunda erişmeniz gerekebilir.
3. **Algılama** bölümü, çok sık veya çok nadir değişebilir. Güvenlik uyarılarınızı her zaman gözden geçirin ve Güvenlik Merkezi önerilerine göre eyleme geçin.

### <a name="hardening-access-and-applications"></a>Erişimi ve uygulamaları sağlamlaştırma

Güvenlik sürecinizin bir parçası olarak VM erişimini kısıtlamak ve VM'ler üzerinde çalışan uygulamaları denetlemek için önlemler de almanız gerekir. Azure VM'lerinize gelen trafiği engelleyerek saldırı riskini azaltabilir ve aynı zamanda gerekli olduğunda VM'lere kolayca bağlanabilirsiniz. VM'lerinize erişimi sağlamlaştırmak için [Anlık VM Erişimi](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) özelliğini kullanın.

[Uyarlamalı Uygulama Denetimleri](https://docs.microsoft.com/azure/security-center/security-center-adaptive-application), Azure'da yer alan VM'lerinizde çalışabilecek uygulamaları denetlemenize ve bu sayede VM'lerinizi kötü amaçlı yazılımlara karşı korumanıza yardımcı olabilir. Güvenlik Merkezi, makine öğrenimi özelliklerini kullanarak VM'de çalışan işlemleri analiz eder ve bu bilgileri kullanarak beyaz listeye ekleme kuralları uygulamanıza yardımcı olur.


## <a name="incident-response"></a>Olay yanıtı
Güvenlik Merkezi, tehditler oluşunca algılar ve sizi tehditlere karşı uyarır. Kuruluşlar, yeni güvenlik uyarılarını izlemeli ve gerekirse daha fazla araştırmak veya saldırıyı düzeltmek için eyleme geçmelidir. Güvenlik Merkezi tehdit algılama özelliğinin nasıl çalıştığı hakkında daha fazla bilgi için [Azure Güvenlik Merkezi algılama özellikleri](security-center-detection-capabilities.md) konusunu okuyun.

Bu makale kendi Olay Yanıtı planınızı oluşturmanıza yardımcı olmaya yönelik değildir, ancak olay yanıtı aşamalarının temeli olarak Bulut yaşam döngüsünde Microsoft Azure Güvenlik Yanıtı kullanılacaktır. Aşamalar aşağıdaki diyagramda gösterilmiştir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-1.png)

> [!NOTE]
> Kendi planınızı oluşturmanıza yardımcı olması için National Institute of Standards and Technology'nin (NIST) [Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) belgesini kullanabilirsiniz.
>

Güvenlik Merkezi Uyarılarını aşağıdaki aşamalar sırasında kullanabilirsiniz:

* **Algılama**: bir veya daha fazla kaynakta şüpheli bir etkinliği tanımlayın.
* **Değerlendirme**: şüpheli etkinlik hakkında daha fazla bilgi edinmek için ilk değerlendirmeyi gerçekleştirin.
* **Tanılama**: sorunu çözmeye yönelik teknik yordamı yürütmek için düzeltme adımlarını kullanın.

Her Güvenlik Uyarısı, saldırının yapısını daha iyi anlamanız ve olası risk azaltmalarını önermek için kullanılabilecek bilgiler sağlar. Ayrıca bazı uyarılar, daha fazla bilgi veya Azure'daki diğer bilgi kaynakları için bağlantılar sağlar. Verilen bilgileri daha fazla araştırma ya da risk azaltma için kullanabilir ve ayrıca çalışma alanınızda depolanmış güvenlikle ilgili verileri arayabilirsiniz.

Aşağıdaki örnek, gerçekleşmekte olan şüpheli bir RDP etkinliğini gösterir:

![Şüpheli etkinlik](./media/security-center-planning-and-operations-guide/security-center-planning-and-operations-guide-fig5-ga.png)

Bu sayfa, saldırının gerçekleştiği zaman, kaynak ana bilgisayar adı, hedef VM ile ilgili ayrıntıları gösterir ve ayrıca öneri adımları sunar. Bazı durumlarda, saldırının kaynak bilgileri boş olabilir. Bu türden bir davranış hakkında daha fazla bilgi için bkz. [Azure Güvenlik Merkezi Uyarıları'nda Eksik Kaynak Bilgileri](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/25/missing-source-information-in-azure-security-center-alerts/).

Bu sayfadan bir [araştırma](https://docs.microsoft.com/azure/security-center/security-center-investigation) başlatarak saldırının zaman çizelgesi, gerçekleşme şekli, gizliliği bozulmuş olabilecek sistemler ve kullanılan kimlik bilgileri hakkında bilgi edinebilir, saldırı zincirinin tamamını grafiklerle görebilirsiniz.

Gizliliği bozulmuş sistemi tanımladıktan sonra önceden oluşturulmuş olan güvenlik [playbook'larını](https://docs.microsoft.com/azure/security-center/security-center-playbooks) çalıştırabilirsiniz. Güvenlik playbook'u belirli bir playbook seçilen uyarı tarafından tetiklendiğinde Güvenlik Merkezi'nden çalıştırılabilecek yordam koleksiyonudur.

[Bir Olay Yanıtı için Azure Güvenlik Merkezi ve Microsoft Operations Management Suite’ten Yararlanma](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703) videosunda, bu aşamaların her birinde Güvenlik Merkezi’nin nasıl kullanılabileceğini anlamanıza yardımcı olabilecek bazı tanıtımlar görebilirsiniz.

> [!NOTE]
> Olay Yanıtı işleminiz sırasında size yardımcı olması için Güvenlik Merkezi özelliklerini kullanma hakkında daha fazla bilgi için [Olay Yanıtı için Azure Güvenlik Merkezi’nden Yararlanma](security-center-incident-response.md) makalesini okuyun.
>
>

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Güvenlik Merkezi benimsemeyi nasıl planlayacağınızı öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) - Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) - Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulabilirsiniz.
