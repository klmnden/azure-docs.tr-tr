---
title: "FedRAMP Azure şeması Otomasyonu - yapılandırma yönetimi"
description: "FedRAMP - yapılandırma yönetimi için Web uygulamaları"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 5b953c0d-236f-4b61-b2c5-df2199490c73
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: e93aa430b7150f07210f5d1f37e2027d95334a59
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="configuration-management-cm"></a>Yapılandırma Yönetimi (CM)

> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.

## <a name="nist-800-53-control-cm-1"></a>NIST 800 53 denetim CM-1

#### <a name="configuration-management-policy-and-procedures"></a>Yapılandırma yönetimi ilke ve yordamlar

**CM-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim taahhüt bir yapılandırma yönetim ilkesi Kurumsal varlıklar ve uyumluluk arasında koordinasyon; ve ilişkili yapılandırma yönetimi denetimleri ve yapılandırma yönetimi ilkesi uygulaması kolaylaştırmak için yordamlar; gözden geçirir ve geçerli yapılandırma yönetim ilkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve yapılandırma yönetimi yordamları [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde yapılandırma yönetimi ilke ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-2"></a>NIST 800 53 denetim CM-2

#### <a name="baseline-configuration"></a>Temel yapılandırma

**CM-2** kuruluş geliştirir, belgeler ve bilgi sisteminin geçerli bir taban çizgisi yapılandırmasını yapılandırma denetimi altında tutar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması oluşturan eşlik eden kaynakları ve Azure Resource Manager şablonları "yapılandırma kod olarak" temel dağıtılmış mimarisi için temsil eder. Çözüm, ancak yapılandırma denetimi için kullanılabilen GitHub sağlanır. Çözüm dağıtılan her sanal makine için istenen durum Yapılandırması'nı (DSC) temel içerir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-2-1a"></a>NIST 800 53 denetim CM-2 (1) bir

#### <a name="baseline-configuration--reviews-and-updates"></a>Taban çizgisi yapılandırmasını | Gözden geçirme ve güncelleştirmeler

**CM-2 (1) bir** kuruluş gözden geçirir ve bilgi sistem taban çizgisi yapılandırmasını güncelleştirir [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, gözden geçirme ve müşteri tarafından dağıtılan kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için) temel yapılandırmasını güncelleştirme sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-2-1b"></a>NIST 800 53 denetim CM-2 (1) .b

#### <a name="baseline-configuration--reviews-and-updates"></a>Taban çizgisi yapılandırmasını | Gözden geçirme ve güncelleştirmeler

**CM-2 (1) .b** kuruluş gözden geçirir ve [atama kuruluşunuz tarafından tanımlanan koşullar nedeniyle] gerektiğinde bilgileri sistem taban çizgisi yapılandırmasını güncelleştirir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, gözden geçirme ve gerektiğinde müşteri tarafından dağıtılan kaynakların temel yapılandırmasını güncelleştirme sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-2-1c"></a>NIST 800 53 denetim CM-2 (1) .c

#### <a name="baseline-configuration--reviews-and-updates"></a>Taban çizgisi yapılandırmasını | Gözden geçirme ve güncelleştirmeler

**CM-2 (1) .c** kuruluş gözden geçirir ve temel yapılandırma bilgileri sisteminin bilgileri sistem bileşeni yüklemeleri ve yükseltmeleri ayrılmaz bir parçası güncelleştirir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, gözden geçirme ve gerektiğinde müşteri tarafından dağıtılan kaynakların temel yapılandırmasını güncelleştirme sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-2-2"></a>NIST 800 53 denetim CM-2 (2)

#### <a name="baseline-configuration--automation-support-for-accuracy--currency"></a>Taban çizgisi yapılandırmasını | Otomasyon desteği doğruluğunu / para birimi

**CM-2 (2)** kuruluş güncel, tam, doğru ve kullanıma hazır temel yapılandırma bilgileri sisteminin korumak için otomatik mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması oluşturan eşlik eden kaynakları ve Azure Resource Manager şablonları "yapılandırma kod olarak" temel dağıtılmış mimarisi için temsil eder. Çözüm, ancak yapılandırma denetimi için kullanılabilen GitHub sağlanır. Azure portalında bir Otomasyon betiğini dağıtılan tüm kaynaklar için kullanılabilir ve bu kaynakları her zaman güncel bir gösterimini sağlar.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-2-3"></a>NIST 800 53 denetim CM-2 (3)

#### <a name="baseline-configuration--retention-of-previous-configurations"></a>Taban çizgisi yapılandırmasını | Önceki yapılandırmaların bekletme

**CM-2 (3)** kuruluş korur [atama: Temel yapılandırmalar bilgi sisteminin önceki sürümlerinde kuruluşunuz tarafından tanımlanan] geri alma desteklemek için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için temel yapılandırmaları önceki sürümlerini koruma için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-2-7a"></a>NIST 800 53 CM-2 (7) bir denetim

#### <a name="baseline-configuration--configure-systems-components-or-devices-for-high-risk-areas"></a>Taban çizgisi yapılandırmasını | Sistemler, bileşenleri ya da aygıtları için yüksek riskli alanlar yapılandırın

**CM-2 (7) bir** kuruluş sorunları [atama: kuruluş tarafından tanımlanan bilgileri sistemleri, sistem bileşenleri veya aygıt] ile [atama: kuruluş tanımlanan yapılandırmaları] konuma seyahat bireylere, kuruluş önemli riskini olarak kabul eder.

**Sorumlulukları:**`Not Applicable`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri tarafından denetlenen fiziksel aygıt yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure müşteri içerik hiçbir zaman fiziksel olarak ABD'de continental içinde bulunduğu Microsoft Azure dışında depolanır. Microsoft Azure personel, Microsoft Azure stok içinde yer alan aygıtlarla seyahat değil. Bu nedenle, bu denetim Microsoft Azure için geçerli değildir. |


 ### <a name="nist-800-53-control-cm-2-7b"></a>NIST 800 53 CM-2 (7) .b denetleme

#### <a name="baseline-configuration--configure-systems-components-or-devices-for-high-risk-areas"></a>Taban çizgisi yapılandırmasını | Sistemler, bileşenleri ya da aygıtları için yüksek riskli alanlar yapılandırın

**CM-2 (7) .b** kuruluşun uyguladığı [atama: kuruluş tanımlanan güvenlik önlemlerinin] kişiler döndüğünüzde cihazlara.

**Sorumlulukları:**`Not Applicable`

|||
|---|---|
| **Müşteri** | Azure üzerinde dağıtılan sistemler kapsamında müşteri tarafından denetlenen fiziksel aygıt yok. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure müşteri içerik hiçbir zaman Microsoft Azure dışında depolanır ve Microsoft Azure personel Microsoft Azure stok içinde yer alan aygıtlarla seyahat etmeseler, böylece bu denetim geçerli değil. |


 ## <a name="nist-800-53-control-cm-3a"></a>NIST 800 53 denetim CM-3.a

#### <a name="configuration-change-control"></a>Yapılandırma değişikliği denetimi

**CM 3.a** Kuruluş yapılandırması denetimli değişiklik bilgileri sistem türlerini belirler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, ne tür değişikliklerin kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için) müşteri tarafından dağıtılan yapılandırma denetimli belirlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-3b"></a>NIST 800 53 denetim CM-3.b

#### <a name="configuration-change-control"></a>Yapılandırma değişikliği denetimi

**CM 3.b** kuruluş önerilen yapılandırma denetimli sisteme yapılan değişiklikleri bilgileri gözden geçirir ve onaylar veya açık göz önünde bulundurarak güvenlik etkisi çözümlemesi için bu değişikliklerden disapproves.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için önerilen yapılandırma denetimli değişiklikleri gözden geçirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-3c"></a>NIST 800 53 denetim CM-3.c

#### <a name="configuration-change-control"></a>Yapılandırma değişikliği denetimi

**CM 3.c** kuruluş belgeleri yapılandırmasını değiştirme kararları bilgi sistemiyle ilişkilendirilmiş.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri ilişkili kaynakları (CM 03.b bakın) müşteri tarafından dağıtılan yapılandırma denetimli değişiklikleri belgelemesinden sorumlu değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-3d"></a>NIST 800 53 denetim CM-3.d

#### <a name="configuration-change-control"></a>Yapılandırma değişikliği denetimi

**CM 3.d** kuruluş bilgileri sistem onaylanan yapılandırma denetimli değişiklikleri uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri CM-03.b onaylanmış yapılandırma denetimli değişiklikleri uygulamak için sorumlu değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-3e"></a>NIST 800 53 denetim CM-3.e

#### <a name="configuration-change-control"></a>Yapılandırma değişikliği denetimi

**CM 3.e** Kuruluş için bilgi sistemde yapılan değişiklikleri yapılandırma denetimli kayıtlarının korur [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yapılandırma denetimli değişiklikler müşteri tarafından dağıtılan kaynaklar için bir kayıt tutma için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-3f"></a>NIST 800 53 denetim CM-3.f

#### <a name="configuration-change-control"></a>Yapılandırma değişikliği denetimi

**CM 3.f** kuruluş denetler ve bilgi sisteme yapılandırma denetimli değişikliklerle ilişkili etkinlikleri gözden geçirir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Denetim ve yapılandırma değişikliklerini gözden geçirme sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-3g"></a>NIST 800 53 denetim CM-3.g

#### <a name="configuration-change-control"></a>Yapılandırma değişikliği denetimi

**CM 3.g** kuruluş düzenler ve yapılandırma değişikliği denetim etkinlikleri gözetim sağlayan [atama: kuruluş tanımlı yapılandırma değişikliği denetim öğesi (örneğin, komitesi, Pano)] [seçimi (convenes bir veya daha fazla): [atama: kuruluş tarafından tanımlanan sıklığı]; [Atama: kuruluş tanımlı yapılandırma değişikliği koşullar]].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Eşgüdümleme ve gözetim yapılandırma değişikliği denetim etkinlikleri için sağlama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-1a"></a>NIST 800 53 denetim CM-3 (1) bir

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Yapılandırma değişikliği denetimi | Belge otomatik / bildirim / Yasak değişiklikleri

**CM-3 (1) bir** kuruluş bilgi sistemi için önerilen değişiklikleri belgelemek için otomatik mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri (CM 03.b bakın) önerilen değişiklikleri belgelemek için otomatik mekanizmaları kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-1b"></a>NIST 800 53 denetim CM-3 (1) .b

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Yapılandırma değişikliği denetimi | Belge otomatik / bildirim / Yasak değişiklikleri

**CM-3 (1) .b** kuruluş bildirmek için otomatik mekanizmaları kullanan [atama: düzenlenmiş tanımlı onay yetkilileri] bilgileri önerilen değişikliklerin onay sistem ve istek değiştirin.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, önerilen değişiklikler müşteri tarafından dağıtılan kaynaklar için yol ve isteği onayı için otomatikleştirilmiş bir mekanizma kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-1c"></a>NIST 800 53 denetim CM-3 (1) .c

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Yapılandırma değişikliği denetimi | Belge otomatik / bildirim / Yasak değişiklikleri

**CM-3 (1) .c** organizasyon onaylı edilmemiş veya tarafından onaysız bilgi sistemi önerilen değişikliklerin vurgulamak için otomatik mekanizmaları kullanır [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, gözden geçirilmeyen değişiklik teklifleri vurgulamak için otomatikleştirilmiş bir mekanizma kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-1d"></a>NIST 800 53 denetim CM-3 (1) .d

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Yapılandırma değişikliği denetimi | Belge otomatik / bildirim / Yasak değişiklikleri

**CM-3 (1) .d** kuruluş belirlenen onayları alınana kadar bilgi sistemde yapılan değişiklikleri önlemek için otomatik mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklara onaylanmamış değişikliklerinin uygulanmasını önlemek için otomatikleştirilmiş bir mekanizma kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-1e"></a>NIST 800 53 denetim CM-3 (1) .e

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Yapılandırma değişikliği denetimi | Belge otomatik / bildirim / Yasak değişiklikleri

**CM-3 (1) .e** Kuruluş bilgileri sistemde yapılan tüm değişiklikleri belgelemek için otomatik mekanizmaları kullanır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklara uygulanan tüm değişiklikleri belgelemek için otomatikleştirilmiş bir mekanizma kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-1f"></a>NIST 800 53 denetim CM-3 (1) .f

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Yapılandırma değişikliği denetimi | Belge otomatik / bildirim / Yasak değişiklikleri

**CM-3 (1) .f** kuruluş bildirmek için otomatik mekanizmaları kullanan [atama: kuruluş tarafından tanımlanan personel] bilgi sistemi onaylı değişiklikler olduğunda tamamlanır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklara onaylı değişiklikler tamamlandığında bildirimleri sağlamak için otomatikleştirilmiş bir mekanizma kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-2"></a>NIST 800 53 denetim CM-3 (2)

#### <a name="configuration-change-control--test--validate--document-changes"></a>Yapılandırma değişikliği denetimi | Test / doğrula / belge değiştirir

**CM-3 (2)** kuruluş testleri, doğrular ve bilgi sistemde yapılan değişiklikleri işletim sistemini değişiklikleri uygulamadan önce belgeleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, test, doğrulama ve belgelendirme değişiklikleri için müşteri tarafından dağıtılan kaynakları uygulamadan önce sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-4"></a>NIST 800 53 denetim CM-3 (4)

#### <a name="configuration-change-control--security-representative"></a>Yapılandırma değişikliği denetimi | Güvenlik temsilcisi

**CM-3 (4)** kuruluşun bir üyesi olması için bir bilgi güvenlik temsilcinize gerektirir [atama: kuruluş tanımlı yapılandırma değişikliği denetim öğesi].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, bilgi güvenlik temsilcisinden CM-03.g tanımlanan değişiklik denetim öğesi bir üyesi olmanız atanmasından sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-3-6"></a>NIST 800 53 denetim CM-3 (6)

#### <a name="configuration-change-control--cryptography-management"></a>Yapılandırma değişikliği denetimi | Şifreleme Yönetimi

**CM-3 (6)** kuruluş sağlamak için kullanılan şifreleme mekanizmaları sağlar [atama: kuruluş tanımlanan güvenlik önlemlerinin] yapılandırma yönetimi altında.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, şifreleme mekanizmasıyla yapılandırma yönetimi altında olan sağlamak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-4"></a>NIST 800 53 denetim CM-4

#### <a name="security-impact-analysis"></a>Güvenlik etki analizi

**CM-4** kuruluş değişikliği uygulamadan önce olası güvenlik etkileri belirlemek için bilgi sistemde yapılan değişiklikleri analiz eder.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar için önerilen değişiklikleri çözümlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-4-1"></a>NIST 800 53 denetim CM-4 (1)

#### <a name="security-impact-analysis--separate-test-environments"></a>Güvenlik etki analizi | Ayrı bir Test ortamları

**CM-4 (1)** kuruluş için güvenlik etkileri açıkları, zayıf, uyumsuzluk nedeniyle ya da bilerek arayan bir işletimsel ortamında uygulamadan önce ayrı bir test ortamında bilgi sistemde yapılan değişiklikleri analiz eder malice.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklara işletimsel ortamında uygulamadan önce test ortamında önerilen değişiklikleri çözümlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-5"></a>NIST 800 53 denetim CM-5

#### <a name="access-restrictions-for-change"></a>Değişiklik için erişim kısıtlamaları

**CM 5** kuruluş tanımlar, belgeler, onaylar ve bilgi sistemi yapılan değişikliklerle ilişkili fiziksel ve mantıksal erişim kısıtlamaları zorunlu kılar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure Active Directory ayrıcalıkları katı sağlama rollere kullanıcıları atayarak rol tabanlı erişim denetimini kullanarak uygulanan hesap denetim hangi kullanıcıların görüntüleyebileceği ve denetim kaynakları dağıtıldı. Active Directory hesap ayrıcalığı kullanıcıları güvenlik gruplarına atayarak rol tabanlı erişim denetimini kullanarak uygulanır. Bu güvenlik grupları kullanıcıların işletim sistemi yapılandırmasına göre gerçekleştirebileceğiniz eylemler denetler. Bu rol tabanlı düzenleri iş gereksinimlerini karşılamak üzere müşteri tarafından genişletilebilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-5-1"></a>NIST 800 53 denetim CM-5 (1)

#### <a name="access-restrictions-for-change--automated-access-enforcement--auditing"></a>Erişim değişiklik için kısıtlamaları | Erişim zorlama otomatik / denetleme

**CM-5 (1)** bilgi sistemi erişim kısıtlamaları zorunlu kılan ve zorlama eylemleri denetimi destekler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure Active Directory ayrıcalıkları katı sağlama rollere kullanıcıları atayarak rol tabanlı erişim denetimini kullanarak uygulanan hesap denetim hangi kullanıcıların görüntüleyebileceği ve denetim kaynakları dağıtıldı. Active Directory hesap ayrıcalığı kullanıcıları güvenlik gruplarına atayarak rol tabanlı erişim denetimini kullanarak uygulanır. Bu güvenlik grupları kullanıcıların işletim sistemi yapılandırmasına göre gerçekleştirebileceğiniz eylemler denetler. Tüm erişir ve erişim denemesi denetlenir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-5-2"></a>NIST 800 53 denetim CM-5 (2)

#### <a name="access-restrictions-for-change--review-system-changes"></a>Erişim değişiklik için kısıtlamaları | Sistem değişiklikleri gözden geçirin

**CM-5 (2)** kuruluş bilgileri sistem değişiklikleri incelemeleri [atama: kuruluş tarafından tanımlanan sıklığı] ve [atama: kuruluş tarafından tanımlanan koşullar] yetkisiz değişiklikler oluşmuş olup olmadığını belirlemek için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yetkisiz değişiklikler oluşmuş olup olmadığını belirlemek için müşteri tarafından dağıtılan kaynaklara değişiklikleri gözden geçirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-5-3"></a>NIST 800 53 denetim CM-5 (3)

#### <a name="access-restrictions-for-change--signed-components"></a>Erişim değişiklik için kısıtlamaları | İmzalanmış bileşenleri

**CM-5 (3)** bilgi sistemi yüklenmesini engeller [atama: kuruluş tarafından tanımlanan yazılımını ve bellenimini bileşenleri] doğrulaması olmadan tanınır bir sertifikayla imzalanmış bileşen imzalanıp ve kuruluş tarafından onaylanmış.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan sanal makinelerin hangi kullanıcıların ve/veya yükleme belirli uygulamaları çalıştıran belirtmek için Windows AppLocker uygulayın. Daha fazla, tüm Windows işletim sistemi güncelleştirmeleri dijital olarak singed. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-5-5a"></a>NIST 800 53 CM 5 (5) bir denetim

#### <a name="access-restrictions-for-change--limit-production--operational-privileges"></a>Erişim değişiklik için kısıtlamaları | Üretim sınırlamak / işletimsel ayrıcalıkları

**CM-5 (5) bir** kuruluş bilgileri sistem bileşenleri ve bir üretim veya işletimsel ortamı içindeki sistem ilgili bilgileri değiştirmek için ayrıcalıkları sınırlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan üretim veya işletimsel ortamlar içinde değişiklik yapmak için ayrıcalıkları sınırlamak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-5-5b"></a>NIST 800 53 CM 5 (5) .b denetleme

#### <a name="access-restrictions-for-change--limit-production--operational-privileges"></a>Erişim değişiklik için kısıtlamaları | Üretim sınırlamak / işletimsel ayrıcalıkları

**CM-5 (5) .b** kuruluş gözden geçirir ve ayrıcalıkları reevaluates [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, gözden geçirme ve CM-05 (05) ayarlarıyla tanımlanmış ayrıcalıklar reevaluating sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-6a"></a>NIST 800 53 denetim CM-6.a

#### <a name="configuration-settings"></a>Yapılandırma ayarları

**CM 6.a** kuruluş kurar ve bilgi teknolojisi ürünleri bilgileri kullanarak içinde değişiklik için yapılandırma ayarlarını belgeleri [atama: kuruluş tanımlanan güvenlik yapılandırma denetim listeleri], yansıtır işletimsel gereksinimleri ile tutarlı en kısıtlayıcı modu.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması dağıtılan her sanal makine için istenen durum Yapılandırması'nı (DSC) temel içerir. Bu bildirim temelli PowerShell komut dosyalarını tanımlayabilir ve uygulanmış kaynaklarını yapılandırın. Bu çözümü tarafından dağıtılmış kaynaklar için dahil DSC temel iş gereksinimlerini karşılamak üzere müşteri tarafından genişletilebilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-6b"></a>NIST 800 53 denetim CM-6.b

#### <a name="configuration-settings"></a>Yapılandırma ayarları

**CM 6.b** kuruluş yapılandırma ayarlarını uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması dağıtılan her sanal makine için istenen durum Yapılandırması'nı (DSC) temel içerir. Taban çizgileri otomatik olarak uygulanan sanal makineler için özel bir komut dosyası sanal makine uzantısını kullanarak dağıtım sırasında. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-6c"></a>NIST 800 53 denetim CM-6.c

#### <a name="configuration-settings"></a>Yapılandırma ayarları

**CM 6.c** kuruluş tanımlar, belgeler ve yerleşik yapılandırma ayarlarını sapmaları onaylar [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri] göre [atama: kuruluş tarafından tanımlanan işletimsel gereksinimlerini].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, belgeleme tanımlama ve müşteri tarafından dağıtılan kaynaklar için kurulmuş yapılandırma ayarları sapmaları onaylama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-6d"></a>NIST 800 53 denetim CM-6.d

#### <a name="configuration-settings"></a>Yapılandırma ayarları

**CM 6.d** kuruluş izleyiciler ve denetimleri kuruluş ilkeleri ve yordamları uygun olarak yapılandırma ayarlarına yapılan değişiklikleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması Automation DSC dağıtır. Automation DSC makine yapılandırmaları belirli bir kuruluş tarafından tanımlanan yapılandırma ile hizalar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-6-1"></a>NIST 800 53 denetim CM-6 (1)

#### <a name="configuration-settings--automated-central-management--application--verification"></a>Yapılandırma ayarları | Merkezi Yönetim otomatik / uygulama / doğrulama

**CM-6 (1)** kuruluşun merkezi olarak yönetmek, uygulamak ve yapılandırma ayarlarını doğrulamak için otomatik mekanizmaları kullanan [atama: kuruluş tarafından tanımlanan bilgileri sistem bileşenleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması Azure Otomasyonu DSC dağıtır. Automation DSC makine yapılandırmaları belirli bir kuruluş tarafından tanımlanan yapılandırma ile hizalanan ve değişiklikleri sürekli olarak izler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-6-2"></a>NIST 800 53 denetim CM-6 (2)

#### <a name="configuration-settings--respond-to-unauthorized-changes"></a>Yapılandırma ayarları | Yetkisiz değişiklikler için yanıt

**CM-6 (2)** kuruluş kullanır [atama: kuruluş tanımlanan güvenlik önlemlerinin] yetkisiz değişiklikler için yanıtlamaları [atama: kuruluş tanımlı yapılandırma ayarları].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması Azure Otomasyonu DSC dağıtır. Bölümü Azure'nın Operations Management Suite (OMS), otomasyonu DSC bir uyarı oluşturmak için veya algılandığında yapılandırma hataları düzeltmek için yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-7a"></a>NIST 800 53 denetim CM-7.a

#### <a name="least-functionality"></a>En az işlevi

**CM 7.a** kuruluş yalnızca temel özellikleri sağlamak üzere bilgi sistemi yapılandırır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynakları amaçlarının için en az işlevselliği sağlayacak şekilde yapılandırılır. İstenen durum Yapılandırması'nı (DSC) temel dağıtılan her sanal makine için dahil edilmiştir. Bu bildirim temelli PowerShell komut dosyalarını tanımlayabilir ve uygulanmış kaynaklarını yapılandırın. Bu çözümü tarafından dağıtılmış kaynaklar için dahil DSC temel daha fazla iş gereksinimlerini karşılayacak şekilde işlevselliği sınırlamak için müşteri tarafından genişletilebilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-7b"></a>NIST 800 53 denetim CM-7.b

#### <a name="least-functionality"></a>En az işlevi

**CM 7.b** kuruluş yasaklar ya da aşağıdaki işlevleri, bağlantı noktaları, protokoller ve/veya Hizmetleri kullanımını kısıtlar: [atama: kuruluş tanımlı yasaklanmış veya kısıtlanmış İşlevler, bağlantı noktaları, protokoller ve/veya hizmetler].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması yalnızca için gerekli bağlantı noktalarını ve protokolleri kullanımını kısıtlamak için Azure uygulama ağ geçidi ve ağ güvenlik grupları dağıtır. Daha fazla uygulama ağ geçidi, ağ güvenlik gruplarını ve sanal makineler için DSC temelleri İşlevler, bağlantı noktaları, protokoller ve yalnızca hedeflenen işlevleri sağlamak için hizmetlerin kullanımını kısıtlamak için müşteri tarafından yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-7-1a"></a>NIST 800 53 denetim CM-7 (1) bir

#### <a name="least-functionality--periodic-review"></a>En az işlevselliği | Periyodik olarak İnceleme

**CM-7 (1) bir** kuruluş bilgileri sistem incelemeleri [atama: kuruluş tarafından tanımlanan sıklığı] gereksiz ve/veya güvenli olmayan işlevler, bağlantı noktaları, protokolleri ve hizmetleri tanımlamak için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklarına (uygulamalar, işletim sistemleri, veritabanları ve yazılım dahil etmek için) gereksiz ve/veya güvenli olmayan işlevler, bağlantı noktaları, protokolleri ve hizmetleri tanımlamak için gözden geçirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-7-1b"></a>NIST 800 53 denetim CM-7 (1) .b

#### <a name="least-functionality--periodic-review"></a>En az işlevselliği | Periyodik olarak İnceleme

**CM-7 (1) .b** kuruluş devre dışı bırakır [atama: kuruluş tanımlı işlevler, bağlantı noktaları, protokoller ve gereksiz ve/veya güvenli olarak kabul bilgi sistemi içindeki Hizmetler].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, İşlevler, bağlantı noktaları, protokoller ve gereksiz veya Güvensiz olarak kabul hizmetleri devre dışı bırakmak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-7-2"></a>NIST 800 53 denetim CM-7 (2)

#### <a name="least-functionality--prevent-program-execution"></a>En az işlevselliği | Program yürütme engelle

**CM-7 (2)** program yürütme ile uyumlu olarak bilgi sistemi engeller [seçimi (bir veya daha fazla): [atama: yazılım kuruluşunuz tarafından tanımlanan ilkelerine programı kullanım ve kısıtlamaları]; koşulları yetkilendirme kuralları ve koşullar yazılım programı kullanımının].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, program yürütme müşteri tanımlı yazılım programı kullanım ilkeleri uygun olarak önlemek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-7-5a"></a>NIST 800 53 CM-7 (5) bir denetim

#### <a name="least-functionality--authorized-software--whitelisting"></a>En az işlevselliği | Yazılım yetkili / uygulamaları güvenilir listeye almayı

**CM-7 (5) bir** kuruluş tanımlayan [atama: kuruluş tarafından tanımlanan yazılım programları yetkili bilgi sistemde yürütmesine].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yetkili yazılım programları tanımlamaktan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-7-5b"></a>NIST 800 53 CM-7 (5) .b denetleme

#### <a name="least-functionality--authorized-software--whitelisting"></a>En az işlevselliği | Yazılım yetkili / uygulamaları güvenilir listeye almayı

**CM-7 (5) .b** kuruluş yetkili yazılım programları yürütülmesi bilgi sistemde izin vermek için bir reddetme all, izin tarafından özel durum ilkesi kullanır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yetkili yazılım programları yürütülmesi müşteri tarafından dağıtılan kaynaklardaki izin vermek için bir reddetme all, izin tarafından özel durum İlkesi kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-7-5c"></a>NIST 800 53 CM-7 (5) .c denetleme

#### <a name="least-functionality--authorized-software--whitelisting"></a>En az işlevselliği | Yazılım yetkili / uygulamaları güvenilir listeye almayı

**CM-7 (5) .c** kuruluş gözden geçirir ve yetkili yazılım programlarının listesini güncelleştirir [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, gözden geçirme ve yetkili yazılım programlarının listesini güncelleştirmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-8a"></a>NIST 800 53 denetim CM-8.a

#### <a name="information-system-component-inventory"></a>Bilgi sistem bileşeni envanteri

**CM 8.a** kuruluş geliştirir ve doğru şekilde geçerli bilgi sistemi yansıtır; Information system yetkilendirme sınırları içinde tüm bileşenleri içerir; altındadır bilgi sistem bileşenleri envanterini belgeleri Ayrıntı düzeyi izleme ve raporlama için gerekli olarak kabul; ve içerir [atama: kuruluş tarafından tanımlanan bilgileri olarak kabul etkili bilgi sistem bileşeni sorumluluk elde etmek gerekli].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tüm kaynakların bir Azure Resource Manager kaynak grubuna dağıtır. Azure Resource Manager dağıtılan kaynakların her zaman güncel bir listesini sağlar ve envanter yönetimi için etiket ve Grup kaynaklar için özelleştirilebilir. Bu çözümü tarafından dağıtılmış kaynaklar sistem sınır ile ilişkilendirilebilir bir belirli kaynak etiketi verilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-8b"></a>NIST 800 53 denetim CM-8.b

#### <a name="information-system-component-inventory"></a>Bilgi sistem bileşeni envanteri

**CM 8.b** kuruluş gözden geçirir ve bilgi sistem bileşeni envanteri güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tüm kaynakların bir Azure Resource Manager kaynak grubuna dağıtır. Azure Resource Manager dağıtılan kaynaklar gözden geçirme için Azure portalında her zaman güncel bir listesini sunar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-8-1"></a>NIST 800 53 denetim CM-8 (1)

#### <a name="information-system-component-inventory--updates-during-installations--removals"></a>Bilgi sistem bileşeni stok | Güncelleştirmeleri yüklemeleri sırasında / çıkarma

**CM-8 (1)** kuruluş bilgileri sistem bileşenleri Envanter bileşeni yükleme, kaldırma işlemleri ve bilgi sistem güncelleştirmeleri ayrılmaz bir parçası güncelleştirir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tüm kaynakların bir Azure Resource Manager kaynak grubuna dağıtır. Azure portalındaki kaynakları dikey kaynakları dağıtılan kaldırılır ve gibi her zaman güncel envanterini sağlayarak tüm dağıtılan kaynakları listeler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-8-2"></a>NIST 800 53 denetim CM-8 (2)

#### <a name="information-system-component-inventory--automated-maintenance"></a>Bilgi sistem bileşeni stok | Otomatik bakım

**CM-8 (2)** kuruluş bilgileri sistem bileşenleri güncel, tam, doğru ve kullanıma hazır envanterini sağlanmasına yardımcı olmak için otomatik mekanizmaları uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tüm kaynakların bir Azure Resource Manager kaynak grubuna dağıtır. Azure portalındaki kaynakları dikey kaynakları dağıtılan kaldırılır ve gibi her zaman güncel envanterini sağlayarak tüm dağıtılan kaynakları listeler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-8-3a"></a>NIST 800 53 CM-8 (3) bir denetim

#### <a name="information-system-component-inventory--automated-unauthorized-component-detection"></a>Bilgi sistem bileşeni stok | Yetkisiz bileşen algılama otomatik

**CM-8 (3) bir** kuruluş otomatik mekanizmaları kullanan [atama: kuruluş tarafından tanımlanan sıklığı] bileşenlerinin yetkisiz donanım, yazılım ve bellenim bilgi sistemi içinde varolup olmadığını algılamak için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde izinsiz yazılımların varolup olmadığını algılamak için otomatik mekanizmaları kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-8-3b"></a>NIST 800 53 CM-8 (3) .b denetleme

#### <a name="information-system-component-inventory--automated-unauthorized-component-detection"></a>Bilgi sistem bileşeni stok | Yetkisiz bileşen algılama otomatik

**CM-8 (3) .b** yetkisiz bileşenleri algılandığında kuruluş aşağıdaki eylemleri gerçekleştirir: [seçimi (bir veya daha fazla): ağ erişimi gibi bileşenleri tarafından devre dışı bırakır; bileşenleri yalıtır; bildirir [atama: kuruluş tarafından tanımlanan "personel veya rolleri]].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yetkisiz yazılım algılandığında eylemin gerçekleştirilmesi sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-8-4"></a>NIST 800 53 denetim CM-8 (4)

#### <a name="information-system-component-inventory--accountability-information"></a>Bilgi sistem bileşeni stok | Sorumluluk bilgileri

**CM-8 (4)** kuruluş bilgileri sistem bileşeni Envanter bilgileri, tarafından tanımlamak için bir yol içerir [seçimi (bir veya daha fazla): ad, konum; rol], kişiler sorumlu/bu bileşenleri yönetmek için sorumlu .

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tüm kaynakların bir Azure Resource Manager kaynak grubuna dağıtır. Azure kaynak etiketleri anahtar / sorumluluk ve/veya yönetim amaçları için kaynakları kategorilere ayırmak için işe çiftleri değeri. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-8-5"></a>NIST 800 53 denetim CM-8 (5)

#### <a name="information-system-component-inventory--no-duplicate-accounting-of-components"></a>Bilgi sistem bileşeni stok | Yinelenen bir hesaplama bileşenleri yok

**CM-8 (5)** bilgi sisteminin yetkilendirme sınırları içinde tüm bileşenleri diğer bilgileri sistem bileşeni envanterleri içinde çoğaltılmaz kuruluş doğrular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tüm kaynakların bir Azure Resource Manager kaynak grubuna dağıtır. Azure Resource Manager dağıtılan kaynakların her zaman güncel bir listesini sunar. Bu çözümü tarafından dağıtılmış kaynaklar sistem sınır ile ilişkilendirilebilir bir belirli kaynak etiketi verilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-9a"></a>NIST 800 53 denetim CM-9.a

#### <a name="configuration-management-plan"></a>Yapılandırma yönetim planı

**CM 9.a** kuruluş geliştirir, belgeler ve rolleri, sorumlulukları ve yapılandırma yönetimi işlemleri ve yordamları adresleri bilgileri sistem için bir yapılandırma yönetim planı uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri geliştirilmesi, belgeleme ve müşteri tarafından dağıtılan kaynaklar için bir yapılandırma yönetim planı uygulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-9b"></a>NIST 800 53 denetim CM-9.b

#### <a name="configuration-management-plan"></a>Yapılandırma yönetim planı

**CM 9.b** kuruluş geliştirir, belgeler ve sistem geliştirme kullanım ömrü boyunca tanımlayıcı yapılandırma öğeleri geçiş için ve için bir işlem kurar bilgileri sistem için bir yapılandırma yönetim planı uygular yapılandırma öğeleri yapılandırmasını yönetme.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri geliştirilmesi, belgeleme ve müşteri tarafından dağıtılan kaynaklar için bir yapılandırma yönetim planı uygulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-9c"></a>NIST 800 53 denetim CM-9.c

#### <a name="configuration-management-plan"></a>Yapılandırma yönetim planı

**CM 9.c** kuruluş geliştirir, belgeler ve yapılandırmasını tanımlayan bilgileri sistem öğeleri için bilgileri sistem ve yapılandırma öğeleri yapılandırma bölümünde yerleştirir için bir yapılandırma yönetim planı uygular yönetimi.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri geliştirilmesi, belgeleme ve müşteri tarafından dağıtılan kaynaklar için bir yapılandırma yönetim planı uygulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-9d"></a>NIST 800 53 denetim CM-9.d

#### <a name="configuration-management-plan"></a>Yapılandırma yönetim planı

**CM 9.d** kuruluş geliştirir, belgeler ve yapılandırma yönetimi planı yetkisiz olarak ifşa ve değişiklik korur bilgileri sistem için bir yapılandırma yönetim planı uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri geliştirilmesi, belgeleme ve müşteri tarafından dağıtılan kaynaklar için bir yapılandırma yönetim planı uygulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-10a"></a>NIST 800 53 denetim CM-10.a

#### <a name="software-usage-restrictions"></a>Yazılım kullanım kısıtlamaları

**CM 10.a** kuruluş yazılımı ve ilişkili belgeleri sözleşme anlaşmalar ve telif hakkı yasaları uygun olarak kullanır.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Windows ve SQL Server lisansları bu Azure şeması tarafından dağıtılan kaynaklar için dahil edilmiştir. Bu, Azure, yerleşik bir özelliğidir. Mevcut yazılım lisansı anlaşmaları kuruluşlarla alternatif lisans modelleri dağıtma düşünebilirsiniz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-10b"></a>NIST 800 53 denetim CM-10.b

#### <a name="software-usage-restrictions"></a>Yazılım kullanım kısıtlamaları

**CM 10.b** kuruluş yazılım ve kopyalama ve dağıtım denetlemek için miktar lisansları tarafından korunan ilişkili belge kullanımını izler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Windows ve SQL Server lisansları bu Azure şeması tarafından dağıtılan kaynaklar için dahil edilmiştir. Kullanıcı lisansları ayrı ayrı izlemek için gerekli değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-10c"></a>NIST 800 53 denetim CM-10.c

#### <a name="software-usage-restrictions"></a>Yazılım kullanım kısıtlamaları

**CM 10.c** kuruluş denetler ve eşler arası dosya paylaşım Bu yetenek yetkisiz dağıtım, görüntü, performans veya telif haklı iş çoğaltılması için kullanılmaz emin olmak için teknolojisi kullanımını belgeleri.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan yetenek paylaşımı eşler arası dosya yok. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-10-1"></a>NIST 800 53 denetim CM-10 (1)

#### <a name="software-usage-restrictions--open-source-software"></a>Yazılım kullanım kısıtlamaları | Açık kaynak yazılım

**CM-10 (1)** kuruluş kullanma açık kaynak yazılımının aşağıdaki kısıtlamaları kurar: [atama: kuruluş tanımlı kısıtlamaları].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde yapılandırma yönetim ilkesi kullanma kısıtlamaları açık kaynak yazılımının adresi. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-11a"></a>NIST 800 53 denetim CM-11.a

#### <a name="user-installed-software"></a>Kullanıcı tarafından yüklenen yazılımlar

**CM 11.a** kuruluş kurar [atama: kuruluş tanımlanan ilkeleri] yöneten kullanıcılar tarafından yazılım yüklemesi.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar kullanıcılar tarafından yazılım yüklemesi yöneten bir ilke oluşturmak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-11b"></a>NIST 800 53 denetim CM-11.b

#### <a name="user-installed-software"></a>Kullanıcı tarafından yüklenen yazılımlar

**CM 11.b** kuruluş yazılım yükleme ilkeleri aracılığıyla zorlar [atama: kuruluş tarafından tanımlanan yöntemler].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yazılım yükleme ilkeleri zorlama için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-cm-11c"></a>NIST 800 53 denetim CM-11.c

#### <a name="user-installed-software"></a>Kullanıcı tarafından yüklenen yazılımlar

**CM 11.c** kuruluş ilkesi uyumluluk adresindeki izler [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, CM-11.a tanımlanan ilkeleriyle müşteri dağıtılan kaynak uyumluluğunu izlemekten sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-cm-11-1"></a>NIST 800 53 denetim CM-11 (1)

#### <a name="user-installed-software--alerts-for-unauthorized-installations"></a>Kullanıcı tarafından yüklenen yazılım | Yetkisiz yüklemeler için uyarıları

**CM-11 (1)** bilgileri sistem uyarıları [atama: kuruluş tarafından tanımlanan personel ya da roller] ne zaman yazılım yetkisiz yüklemesi algılandı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yetkisiz yazılım yüklemesi algıladığında uyarılar sağlamaktan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |

