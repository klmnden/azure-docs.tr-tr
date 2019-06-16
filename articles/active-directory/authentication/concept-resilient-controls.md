---
title: Azure Active Directory bir esnek erişim denetimi yönetim stratejisi - oluşturma
description: Bu belge, beklenmedik kesintileri sırasında kilitleme riskini azaltmak için dayanıklılık sağlamak için bir kuruluş stratejileri hakkında yönergeler benimseyin sağlar.
services: active-directory
author: martincoetzer
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 12/19/2018
ms.author: martincoetzer
ms.collection: M365-identity-device-management
ms.openlocfilehash: ff59b93603af61fd8ea571966a3c43a06929ae04
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113483"
---
# <a name="create-a-resilient-access-control-management-strategy-with-azure-active-directory"></a>Azure Active Directory ile esnek erişim denetimi yönetim stratejisi oluşturma

>[!NOTE]
> Bu belgede yer alan bilgileri, Microsoft Corporation'ın geçerli görünümü tarih itibariyle doğrudur ele alınan sorunlar temsil eder. Microsoft'un değişen piyasa koşullarına yanıt vermesi gerekir çünkü Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft yayın tarihinden sonra sunulan bilgilerin doğruluğunu garanti edemez.

Bu tek erişim denetimini kullanılamaz duruma gelirse, çok faktörlü kimlik doğrulaması (MFA) gibi bir tek erişim denetimi veya BT sistemlerinin güvenliğini sağlamak için ek olarak, tek bir ağ konumuna dayalı kuruluşlar kendi uygulama ve kaynaklara erişim hatalarına açıktır veya yanlış yapılandırılmış. Örneğin, bir doğal felaket büyük parçaları telekomünikasyon altyapı veya kurumsal ağ içinde bir yetersizlik neden olabilir. Böyle bir kesinti son kullanıcılar ve yöneticiler oturum açabilir yüklenmesini engelleyebilir.

Bu belge, bir kuruluş stratejileri hakkında yönergeler aşağıdaki senaryoları ile beklenmedik kesintileri sırasında kilitleme riskini azaltmak için dayanıklılık sağlamak için benimseyin sağlar:

 1. Kuruluşlar, kilitleme riskini azaltmak için kendi dayanıklılığı artırabilir **kesilme önce** risk azaltma stratejisi veya yedek planlar uygulayarak.
 2. Kuruluşların uygulama ve kaynakları seçtikleri erişmeye devam edebilir **bir kesinti sırasında** risk azaltma stratejisi ve yedek planlar yerinde sahip.
 3. Kuruluşlar, günlükleri gibi bilgileri korumak emin olun **kesilme sonra** ve bunlar, uygulanan tüm olasılıkları geri önce.
 4. Önleme stratejileri veya alternatif planları uygulamadığınız kuruluşların uygulayabilirsiniz **Acil seçenekleri** kesintisi ile dağıtılacak.

## <a name="key-guidance"></a>Anahtar Kılavuzu

Bu belgede dört önemli dersler şunlardır:

* Acil Durum erişim hesapları'nı kullanarak yönetici kilitleme kaçının.
* Kullanıcı başına MFA yerine MFA, koşullu erişim (CA) kullanarak uygulayın.
* Kullanıcı kilitlemesi birden çok koşullu erişim (CA) denetimlerini kullanarak azaltın.
* Kullanıcı kilitlemesi birden çok kimlik doğrulama yöntemleri veya her kullanıcının eşdeğerleri sağlayarak azaltın.

## <a name="before-a-disruption"></a>Önce bir kesinti

Gerçek bir bozulma Azaltıcı kaynaklanabilecek erişim denetimi sorunları ile ilgili bir kuruluşun birincil odak noktası olması gerekir. Asıl olay için planlama ve erişim denetimleri emin olmak için stratejileri uygulama Azaltıcı içerir ve işlemleri kesintileri sırasında etkilenmez.

### <a name="why-do-you-need-resilient-access-control"></a>Esnek erişim denetimi neden gerekiyor?

 Kullanıcıların uygulamalara ve kaynaklara erişim denetim düzlemi kimliğidir. Hangi kullanıcıların kimlik sisteminizde denetler ve erişim denetimleri veya kimlik doğrulama gereksinimleri gibi hangi koşullar altında kullanıcılar uygulamalara erişin. Bir veya daha fazla kimlik doğrulama veya erişim denetimi gereksinimleri öngörülemeyen koşullar nedeniyle kimlik doğrulaması kullanıcılar için kullanılamaz, kuruluşların birini veya ikisini de aşağıdaki sorunlarla karşılaşabilirsiniz:

* **Yönetici kilidi:** Yöneticiler Kiracı veya hizmet yönetemez.
* **Kullanıcı kilitlemesi:** Kullanıcılar, uygulamaları veya kaynaklara erişemez.

### <a name="administrator-lockout-contingency"></a>Yönetici kilitleme olasılık

Kiracınız için yönetici erişimi kilidini açmak için Acil Durum erişim hesapları oluşturmanız gerekir. Olarak da bilinen bu Acil Durum erişim hesapları *sonu cam* hesapları, normal ayrıcalıklı hesap erişim yordamları kullanılamaz olduğunda Azure AD'ye yapılandırmasını yönetmek erişim sağlar. En az iki Acil Durum erişim hesaplarının oluşturulması aşağıdaki [Acil Durum erişim hesabı önerileri]( https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access).

### <a name="mitigating-user-lockout"></a>Kullanıcı kilitlemesi azaltma

 Kullanıcı kilitlemesi riskini azaltmak için nasıl, uygulama ve kaynaklara erişecek seçimini kullanıcılara vermek için birden çok denetimlerle koşullu erişim ilkelerini kullanın. Bir kullanıcı arasında seçim vererek, örneğin, MFA ile oturum açma **veya** yönetilen bir CİHAZDAN oturum açarken **veya** kullanıcının sahip olduğu ve şirket ağından erişim denetimleri biri kullanılamıyorsa, oturum açma çalışmaya devam etmek için diğer seçenekleri.

#### <a name="microsoft-recommendations"></a>Microsoft önerileri

Kuruluş için var olan koşullu erişim ilkelerinizi şu erişim denetimleri ekleyebilirsiniz:

1. Farklı iletişim kanalları, örneğin Microsoft Authenticator uygulamasını (internet tabanlı), OATH belirteci (oluşturulan cihazda) ve SMS (telephonic) kullanan birden çok kimlik doğrulama yöntemleri için her bir kullanıcı sağlayın.
2. Windows iş için Hello doğrudan cihaz oturum açma MFA gereksinimlerini karşılamak için Windows 10 cihazlara dağıtın.
3. Güvenilen cihazlar aracılığıyla kullanır [Azure AD karma birleştirme](https://docs.microsoft.com/azure/active-directory/devices/overview) veya [Microsoft Intune yönetilen cihazların](https://docs.microsoft.com/intune/planning-guide). Bir kullanıcı MFA testini olmadan İlkesi güçlü kimlik doğrulama gereksinimlerini karşılayan bir güvenilen cihazı güvenilen cihazları kullanıcı deneyimini geliştirir. Mfa'yı daha sonra yeni bir cihaz kaydederken ve güvenilmeyen cihazlardan uygulamaları ya da kaynaklara erişirken gerekli olacaktır.
4. Kullanıcı veya oturum açma risk sabit MFA ilkeleri yerine olduğunda erişimi engelleyen Azure AD kimlik koruması risk tabanlı ilkeler kullanın.

>[!NOTE]
> Risk tabanlı ilkeler gerektiren [Azure AD Premium P2](https://azure.microsoft.com/pricing/details/active-directory/) lisansları.

Aşağıdaki örnek, kullanıcının kendi uygulamaları ve kaynaklarına erişmek bir esnek erişim denetimi sağlamak için oluşturmalısınız ilkelerini açıklar. Bu örnekte, bir güvenlik grubu gerektirir **AppUsers** adlı bir grubu erişimi vermek istediğiniz hedef kullanıcılarla **CoreAdmins** çekirdek Yöneticiler ve adlandırılmış grup  **EmergencyAccess** Acil Durum erişim hesaplarına sahip.
Bu örnek ilke kümesi, seçili kullanıcıların vereceği **AppUsers**, güvenilen bir CİHAZDAN bağlanıyorsanız seçili uygulamalar için erişim veya güçlü kimlik doğrulaması, örneğin MFA sağlayın. Acil Durum hesapları ve çekirdek yöneticilerinin hariç tutar.

**CA azaltma ilkeleri ayarlayın:**

* İlke 1: Hedef gruplar dışındaki kişilere erişimi engelle
  * Kullanıcılar ve gruplar: Tüm kullanıcıları dahil edin. AppUsers CoreAdmins ve EmergencyAccess Dışla
  * Bulut uygulamaları: Tüm uygulamaları içerir
  * Koşullar: (Hiçbiri)
  * İzin verme denetimi: Engelle
* İlke 2: Mfa'yı veya güvenilen cihazı gerektiren AppUsers erişim izni verin.
  * Kullanıcılar ve gruplar: AppUsers içerir. CoreAdmins ve EmergencyAccess hariç tut
  * Bulut uygulamaları: Tüm uygulamaları içerir
  * Koşullar: (Hiçbiri)
  * İzin verme denetimi: Erişim, çok faktörlü kimlik doğrulaması gerektiren, cihazın uyumlu olmasını gerektir. Birden fazla denetim için: Seçilen denetimlerden birini gerektir.

### <a name="contingencies-for-user-lockout"></a>Kullanıcı kilitlemesi için olasılıkları

Alternatif olarak, kuruluşunuz yedek ilkeleri de oluşturabilirsiniz. Yedek ilkeleri oluşturmak için iş sürekliliği, işletim maliyeti, maliyet ve güvenlik risklerini arasında denge ölçütleri tanımlamanız gerekir. Örneğin, kullanıcılar, uygulamaları, istemcilerin veya bir alt kümesini konumlar bir alt kümesi için bir alt kümesi için bir alt kümesi için yalnızca bir yedek İlkesi etkinleştirebilirsiniz. Yedek ilkeleri yöneticileri ve son kullanıcıların uygulamalara ve kaynaklara, risk azaltma yöntem uygulandığında bir kesinti sırasında sürümlere erişmenizi sağlayacaktır.
Bir kesinti sırasında maruz kalma riskinizi anlama riskini azaltmaya yardımcı olur ve planlama sürecinizi önemli bir parçasıdır. Yedek planınızı oluşturmak için önce kuruluşunuzun aşağıdaki iş gereksinimlerini belirleyin:

1. Görev açısından kritik uygulamalarınızı önceden belirler: Daha düşük bir risk/güvenlik duruşunu olsa da erişimi vermeniz gerekir apps nedir? Bu uygulamaların bir listesini oluşturun ve tüm kaybolduktan tüm erişim denetimi, bu uygulamalar yine de çalışmaya devam etmesi gerektiğini kabul ediyorum, diğer proje katılımcıları (iş, güvenlik, hukuk liderlik) emin olun. Büyük olasılıkla sonuna kategorileri ile oluşturacağınız:
   * **Kategori 1 görev açısından kritik uygulamalarını** , olamaz kullanılabilir birden fazla işlem birkaç dakika, örneğin kuruluşun gelir doğrudan etkileyen uygulamaları.
   * **Kategori 2 önemli uygulamaları** iş birkaç saat içinde erişilebilir olması gerekir.
   * **Düşük öncelikli uygulama kategorisi 3** birkaç gün kesintiye dayanacak.
2. 1\. ve 2 kategoriye giren uygulamalar için önceden planlama izin vermek istediğiniz erişim düzeyini hangi türde Microsoft önerir:
   * Tam erişim veya indirmeleri sınırlama gibi kısıtlı oturum izin vermek istiyor musunuz?
   * Uygulama, ancak tüm uygulama parçası erişmesine izin vermek istiyor musunuz?
   * Bilgi çalışanı erişmesine ve erişim denetimi geri yüklenene kadar yönetici erişimi engellemek istiyor musunuz?
3. Bu uygulamalar için erişim hangi girebilecek alanlar, kasıtlı olarak açılır ve hangilerinin, kapatılacak planladığınız Microsoft de önerir:
   * Çevrimdışı veri kaydedebileceğiniz yalnızca erişim ve blok zengin istemcileri tarayıcı izin vermek istiyor musunuz?
   * Yalnızca kurumsal ağ içinde kullanıcılar için erişime izin ver ve engellenen kullanıcılar dışında tutmak istiyor musunuz?
   * Kesinti sırasında yalnızca belirli ülke veya bölgelerde erişim izin vermek istiyor musunuz?
   * İlkeleri bir alternatif erişim denetimi kullanılabilir değilse, başarılı veya başarısız için özellikle görev açısından kritik uygulamalar için yedek ilkeleri istiyor musunuz?

#### <a name="microsoft-recommendations"></a>Microsoft önerileri

Yedek bir koşullu erişim ilkesi bir **ilke devre dışı** , Azure MFA'yı üçüncü taraf MFA, risk veya cihaz tabanlı denetimler atlar. Ardından, yedek planınızı etkinleştirmek, kuruluşunuzun karar verdiğinde, yöneticiler ilkesini etkinleştirmek ve normal denetim tabanlı ilkeler devre dışı bırakın.

>[!IMPORTANT]
> Yedek planı kullanımdayken kullanıcılarınızın güvenliği zorla ilkelerini devre dışı bırakma, güvenlik duruşunuzu bile geçici olarak azaltır.

* Bir geri dönüş ilke kümesi, bir kesinti durumunda bir kimlik bilgisi türü veya bir erişim denetimi mekanizması etkileri uygulamalarınıza erişim yapılandırın. Bir üçüncü taraf MFA sağlayıcısı gerektiren etkin bir ilke için bir yedek olarak bir denetim olarak etki alanına katılım gerektiren devre dışı durumda bir ilke yapılandırın.
* MFA, yöntemleri izleyerek gerekli değildir, parolalar, tahmin kötü aktörleri riskini azaltmak [parola yönergeleri](https://aka.ms/passwordguidance) teknik incelemesi.
* Dağıtma [Azure AD Self Servis parola sıfırlama (SSPR)](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) ve [Azure AD parola koruması](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-deploy) kullanıcıların yaygın parola ve koşulları yasaklamak için seçtiğiniz kullanma emin olmak için.
* Belirli bir kimlik doğrulama düzeyi yalnızca tam erişim dönülüyor yerine ulaşılan değil, bu uygulamaların içindeki erişimi kısıtlama ilkeleri kullanın. Örneğin:
  * Exchange ve SharePoint için sınırlı oturuma talep gönderen bir yedekleme ilkesi yapılandırın.
  * Kuruluşunuz Microsoft Cloud App Security kullanıyorsa MCAS ve MCAS sağlar, salt okunur erişim ilgilenir. ancak değil yükleyen bir ilke dönülüyor göz önünde bulundurun.
* Bir kesinti sırasında bulmayı kolay olduğundan emin olmak için ilkelerinizi adlandırın. İlkenin adına aşağıdaki öğeleri içerir:
  * A *etiket sayısı* ilkesi için.
  * Göstermek için bu ilke, yalnızca acil durumlar için metin. Örneğin: **ACİL DURUMDA ETKİNLEŞTİR**
  * *Kesintisi* için geçerlidir. Örneğin: **MFA kesintisi sırasında**
  * A *sıra numarası* sırası göstermek için ilkeleri etkinleştirin.
  * *Uygulamaları* için geçerlidir.
  * *Denetimleri* geçerli olacaktır.
  * *Koşullar* gerektirir.
  
Bu adlandırma standardı yedek ilkeleri şu şekilde olacaktır: 

```
EMnnn - ENABLE IN EMERGENCY: [Disruption][i/n] - [Apps] - [Controls] [Conditions]
```

Aşağıdaki örnekte: **Örnek bir - erişimi için Görev açısından kritik İşbirliği uygulamaları geri yüklemek için yedek CA ilkesi**, tipik kurumsal tahminlere olduğu. Bu senaryoda, kuruluş, MFA genellikle tüm Exchange Online ve SharePoint Online'a erişimi gerektirir. ve (Azure MFA, şirket içi olmadığını MFA sağlayıcısı veya üçüncü taraf MFA) müşterinin MFA sağlayıcısı kesinti yaşandığında kesintisi bu durumda olur. Bu ilke, yalnızca, uygulama kendi güvenilen şirket ağından erişirken belirli hedeflenen kullanıcılar bu uygulamaları güvenilir Windows cihazlarından erişimine tarafından bu kesinti azaltır. Bu ayrıca Acil Durum hesapları ve çekirdek Yöneticiler bu kısıtlamaları dışında bırakır. Diğer kullanıcıların kesinti nedeniyle uygulamalara erişim hala yoktur ancak hedeflenen kullanıcılara ardından Exchange Online'a ve SharePoint Online erişim sahibi. Bu örnek bir adlandırılmış bir ağ konumuna gerektirecek **CorpNetwork** ve bir güvenlik grubu **ContingencyAccess** hedef kullanıcılarla adlı bir grubu **CoreAdmins** ile Çekirdek yöneticileri ve adlandırılmış grup **EmergencyAccess** Acil Durum erişim hesaplarına sahip. Yedek plan istenen erişim sağlamak için dört İlkesi gerektirir. 

**A - yedek CA ilkeleri erişimi için Görev açısından kritik İşbirliği uygulamaları geri yüklemek için örnek:**

* İlke 1: Exchange ve SharePoint için etki alanına katılmış cihaz gerektir
  * Ad: EM001 - ACİL DURUMDA ETKİNLEŞTİR: MFA kesintisi [1/4] - Exchange, SharePoint - hibrit Azure AD'ye katılma gerektirir
  * Kullanıcılar ve gruplar: ContingencyAccess içerir. CoreAdmins ve EmergencyAccess hariç tut
  * Bulut uygulamaları: Exchange Online ve SharePoint Online
  * Koşullar: Tüm
  * İzin verme denetimi: Etki alanına katılmış gerektirir
  * Durum: Devre dışı
* İlke 2: Windows dışındaki blok platformları
  * Ad: EM002 - ACİL DURUMDA ETKİNLEŞTİR: MFA kesintisi Windows dışındaki 2/4 - Exchange SharePoint - blok erişimi
  * Kullanıcılar ve gruplar: Tüm kullanıcıları dahil edin. CoreAdmins ve EmergencyAccess hariç tut
  * Bulut uygulamaları: Exchange Online ve SharePoint Online
  * Koşullar: Cihaz platformu dahil tüm platformları, Windows Dışla
  * İzin verme denetimi: Engelle
  * Durum: Devre dışı
* İlke 3: Blok ağları CorpNetwork dışında
  * Ad: EM003 - ACİL DURUMDA ETKİNLEŞTİR: MFA kesintisi şirket ağı dışındaki 3/4 - Exchange SharePoint - blok erişimi
  * Kullanıcılar ve gruplar: Tüm kullanıcıları dahil edin. CoreAdmins ve EmergencyAccess hariç tut
  * Bulut uygulamaları: Exchange Online ve SharePoint Online
  * Koşullar: Konumları CorpNetwork hariç, herhangi bir yere ekleyin
  * İzin verme denetimi: Engelle
  * Durum: Devre dışı
* İlke 4: EAS açıkça engelle
  * Ad: EM004 - ACİL DURUMDA ETKİNLEŞTİR: MFA kesintisi 4/4 - Exchange - blok EAS tüm kullanıcılar için
  * Kullanıcılar ve gruplar: Tüm kullanıcıları
  * Bulut uygulamaları: Exchange Online içerir
  * Koşullar: İstemci uygulamaları: Exchange Active Sync
  * İzin verme denetimi: Engelle
  * Durum: Devre dışı

Etkinleştirme sırası:

1. ContingencyAccess CoreAdmins ve EmergencyAccess mevcut MFA ilkesinden hariç tutun. ContingencyAccess bir kullanıcının SharePoint Online ve Exchange Online erişip doğrulayın.
2. İlke 1 etkinleştir: Dışlama grupları olmayan etki alanına katılmış cihazlarda kullanıcıların Exchange Online ve SharePoint Online'a erişebilir olduğundan emin olun. Dışlama grubundaki kullanıcılar SharePoint Online ve Exchange herhangi bir CİHAZDAN erişebilirsiniz doğrulayın.
3. İlke 2 etkinleştir: Dışlama grup içinde olmayan kullanıcıların mobil cihazlarından SharePoint Online ve Exchange Online için alınamıyor doğrulayın. Herhangi bir CİHAZDAN (Windows/iOS/Android) çıkarma grubundaki kullanıcıların SharePoint ve Exchange erişip doğrulayın.
4. 3 ilkesini etkinleştirin: Dışlama grupları olmayan kullanıcılar SharePoint erişemez ve Exchange şirket ağına, hatta bir etki alanı ile kapalı makine katılmış doğrulayın. Herhangi bir ağdan çıkarma grubundaki kullanıcıların SharePoint ve Exchange erişip doğrulayın.
5. 4 ilkesini etkinleştirin: Tüm kullanıcılar Exchange Online mobil cihazlardaki yerel e-posta uygulamalarından alınamıyor doğrulayın.
6. SharePoint Online ve Exchange Online için mevcut MFA ilkesini devre dışı bırakın.

Bu sonraki örnekte **örnek B - Salesforce mobil erişmesine izin vermek için yedek CA ilkeleri**, bir iş uygulamasının erişimi geri yüklenir. Bu senaryoda, müşteri genellikle satış çalışanlar erişimleri salesforce'a (çoklu oturum için Azure AD ile yapılandırılan) mobil cihazlardan yalnızca uyumlu cihazlardan izin gerektirir. Kesinti bu durumda cihaz uyumluluğunu değerlendirmek bir sorun yoktur ve hassas bir kerede kesinti olup burada satış gereksinimlerini ekibi, anlaşmalar kapatmak için salesforce'a erişim. Bunlar anlaşmalar kapatın ve iş kesintiye devam edebilmesi için bu yedek ilkeler kritik kullanıcıların erişim Salesforce'a bir mobil CİHAZDAN izin vermiş olursunuz. Bu örnekte, **SalesforceContingency** erişimi korumak için gereken tüm Sales employees içerir ve **SalesAdmins** gerekli Salesforce yöneticileri içerir.

**Örnek B - yedek CA ilkeleri:**

* İlke 1: Herkes değil SalesContingency takım engelle
  * Ad: EM001 - ACİL DURUMDA ETKİNLEŞTİR: Cihaz uyumluluk kesintisi [1/2] - Salesforce - SalesforceContingency dışında blok tüm kullanıcılar
  * Kullanıcılar ve gruplar: Tüm kullanıcıları dahil edin. SalesAdmins ve SalesforceContingency hariç tut
  * Bulut uygulamaları: Salesforce.
  * Koşullar: None
  * İzin verme denetimi: Engelle
  * Durum: Devre dışı
* İlke 2: Mobile (saldırı yüzey alanını azaltmak için) dışındaki herhangi bir platform satış ekibinin engelle
  * Ad: EM002 - ACİL DURUMDA ETKİNLEŞTİR: Cihaz uyumluluk kesintisi iOS ve Android dışında 2/2 - Salesforce - bloğu tüm platformlar
  * Kullanıcılar ve gruplar: SalesforceContingency içerir. SalesAdmins Dışla
  * Bulut uygulamaları: Salesforce
  * Koşullar: İOS ve Android cihaz platformu dahil tüm platformlara hariç tut
  * İzin verme denetimi: Engelle
  * Durum: Devre dışı

Etkinleştirme sırası:

1. Mevcut cihaz uyumluluk İlkesi'nden SalesAdmins ve SalesforceContingency için Salesforce hariç tutun. Salesforce SalesforceContingency gruptaki bir kullanıcının erişebileceği doğrulayın.
2. İlke 1 etkinleştir: Salesforce SalesContingency dışındaki kullanıcılar erişemez doğrulayın. Kullanıcıların SalesAdmins doğrulayın ve Salesforce SalesforceContingency erişebilir.
3. İlke 2 etkinleştir: SalesContingency grupta bulunan kullanıcılara Salesforce Windows/Mac dizüstü erişemez ancak hala mobil cihazlarından erişebilirsiniz doğrulayın. SalesAdmin, yine de Salesforce herhangi bir CİHAZDAN erişebilirsiniz doğrulayın.
4. Mevcut cihaz uyumluluk ilkesi için Salesforce devre dışı bırakın.

### <a name="deploy-password-hash-sync-even-if-you-are-federated-or-use-pass-through-authentication"></a>Parola karma eşitlemesi, Federasyon veya geçişli kimlik doğrulaması kullanmak olsa bile Dağıt

Kullanıcı kilitlemesi, aşağıdaki koşullar geçerli olduğunda da meydana gelebilir:

- Kuruluşunuzun karma kimlik çözümü, doğrudan kimlik doğrulama veya Federasyon ile kullanır.
- (Örneğin, Active Directory, AD FS veya bağımlı bileşenin), şirket içi kimlik sistemlerinin kullanılamaz. 
 
Kuruluşunuz daha dayanıklı olacak şekilde gerekir [parola karması eşitlemeyi etkinleştirme](https://docs.microsoft.com/azure/security/azure-ad-choose-authn)sağlar çünkü [geçiş parola karması eşitleme için kullanılacak](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-user-signin) , şirket içi kimlik sistemlerinin çalışmıyorsa.

#### <a name="microsoft-recommendations"></a>Microsoft önerileri
 Azure AD Connect Sihirbazı'nı Federasyon veya geçişli kimlik doğrulaması, kuruluş kullanılıp kullanılmayacağını bakılmaksızın kullanarak parola karma eşitlemesini etkinleştirin.

>[!IMPORTANT]
> Bu kullanıcıların dönüştürmek için parola karması eşitleme kullanılacak yönetilen kimlik doğrulaması Federasyon gerekli değildir.

## <a name="during-a-disruption"></a>Bir kesinti sırasında

Bir risk azaltma planını uygulamak için ettiyseniz, otomatik olarak tek bir erişim denetimi kesilme varlığını sürdürmesi mümkün olacaktır. Beklenmedik durum planı oluşturmak için ettiyseniz, erişim denetimi kesinti sırasında yedek ilkelerinizi etkinleştirmeniz mümkün olacaktır:

1. Hedeflenen kullanıcılar, belirli ağlar belirli uygulamalara erişim izni yedek ilkelerini etkinleştirin.
2. Normal denetim tabanlı ilkelerinizi devre dışı bırakın.

### <a name="microsoft-recommendations"></a>Microsoft önerileri

Hangi risk azaltma işlemleri ya da olasılıkları bir kesinti sırasında kullanıldığına bağlı olarak, erişim yalnızca parolalarla kuruluşunuz veriyor. Hiçbir koruma, dikkatli bir şekilde ağırlıklı gereken önemli bir güvenlik riski oluşturur. Kuruluşların gerekir:

1. Değişiklik denetimi stratejinizin bir parçası, her değişikliği ve erişim denetimleri tümüyle çalışır duruma hemen sonra uygulanan tüm olasılıkları geri yapabilmek için önceki durum belgeleyin.
2. Kötü amaçlı aktörler mfa'yı devre dışı durumdayken parola ilaç veya kimlik avı saldırıları yoluyla parolaların Hasat dener varsayılır. Ayrıca, kötü aktörleri zaten daha önce bu penceresi boyunca çalıştı herhangi bir kaynağa erişimi veremez parolaları olabilir. Diğer yöneticiler gibi kritik kullanıcılar için MFA için bunları devre dışı bırakmadan önce kendi parolalarını sıfırlama kısmen bu riskini azaltabilirsiniz.
3. Mfa'yı devre dışı olduğu süre boyunca kimlerin ne tanımlamak için tüm oturum açma etkinliği arşivleme.
4. [Tüm risk olayları bildirilen önceliklendirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) bu penceresi sırasında.

## <a name="after-a-disruption"></a>Sonra bir kesinti

Kesinti nedeniyle hizmet geri yüklendikten sonra etkin Yedek planı bir parçası olarak yaptığınız değişiklikleri geri alın. 

1. Normal ilkelerini etkinleştir
2. Yedek ilkelerinizi devre dışı bırakın. 
3. Yapılan ve kesinti sırasında belgelenen diğer değişiklikleri geri alın.
4. Kimlik bilgilerini yeniden oluşturun ve yeni kimlik bilgilerinin ayrıntıları, Acil Durum erişim hesabı yordamların bir parçası olarak fiziksel olarak güvenli bir Acil Durum erişim hesabı'nı kullandıysanız unutmayın.
5. Devam [tüm risk olayları bildirilen önceliklendirme](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins) şüpheli etkinlik için kesinti sonra.
6. Verilen tüm yenileme belirteçleri iptal [PowerShell kullanarak](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken?view=azureadps-2.0) bir kullanıcı kümesini hedeflemek için. Tüm yenileme belirteçleri iptal kesinti sırasında kullanılan ayrıcalıklı hesaplar için önemlidir ve işi yeniden kimlik doğrulaması yapın ve geri yüklenen ilkeleri denetimi karşılamak için zorlar.

## <a name="emergency-options"></a>Acil Durum seçenekleri

 Bir Acil Durum ya da kuruluşunuzun durumunda daha önce bir risk azaltma veya Yedek planı uygulamak ve ardından önerileri izleyin [olasılıkları kullanıcı kilitlemesinin için](#contingencies-for-user-lockout) zaten koşullu erişim kullanıyorsanız bölümü MFA zorlamak için ilke'ı tıklatın.
Kuruluşunuzun kullanıcı başına MFA eski ilkeleri kullanıyorsanız, aşağıdaki alternatif göz atabilirsiniz:

1. Şirket ağına giden IP adresi varsa, Kurumsal ağa yalnızca kimlik doğrulamasını etkinleştirmek için güvenilen IP'ler olarak ekleyebilirsiniz.
   1. Envanteri giden IP adresi yoksa veya içindeki ve dışındaki şirket ağına erişimi etkinleştirmek için gereken tüm IPv4 adres alanına 0.0.0.0/1 ve 128.0.0.0/1 belirterek güvenilen IP'ler ekleyebilirsiniz.

>[!IMPORTANT]
 > Erişim engelini kaldırmak için güvenilen IP adresleri genişletmek, IP adresleri (örneğin, mümkün olmayan seyahat veya tanınmayan konumlardan) ile ilişkili risk olayları oluşturulmaz.

>[!NOTE]
 > Yapılandırma [güvenilen IP'ler](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-mfasettings) için Azure MFA yalnızca bulunan [Azure AD Premium lisansları](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-licensing).

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Azure AD Authentication belgeleri](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-iis)
* [Azure AD'de Acil Durum erişimi yönetici hesaplarını yönetme](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-emergency-access)
* [Azure Active Directory'de adlandırılmış konumları yapılandırma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)
  * [Set-MsolDomainFederationSettings](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0)
* [Hibrit Azure Active Directory'ye katılmış cihazları yapılandırma](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan)
* [İş İçin Windows Hello Dağıtım Kılavuzu](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide)
  * [Parola Kılavuzu - Microsoft Research'ün](https://research.microsoft.com/pubs/265143/microsoft_password_guidance.pdf)
* [Azure Active Directory koşullu erişim koşulları nelerdir?](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions)
* [Azure Active Directory koşullu erişim erişim denetimleri nelerdir?](https://docs.microsoft.com/azure/active-directory/conditional-access/controls)
