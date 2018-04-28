---
title: Azure Active Directory kimlik koruması | Microsoft Docs
description: Azure AD kimlik koruması nasıl yeteneğini bir saldırgan güvenliği aşılmış kimlik veya aygıt yararlanmaya ve güvenli bir kimlik veya önceden şüpheli veya tehlikeye bilinen bir cihaz için sınırlamak sağladığını öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi yönetme
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: b8ae865e06e085ebe1bb71b35d812024190e2b21
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması

Azure Active Directory kimlik koruması sağlayan Azure AD Premium P2 edition özelliğidir:

- Kuruluşunuzdaki kimlikleri etkileyen olası güvenlik açıklarını algılama

- Kuruluşunuzdaki kimlikleri için ilgili algılanan kuşkulu eylemleri otomatik yanıtlar yapılandırın  

- Şüpheli olayları araştırmanıza ve bunları gidermek için uygun eylemi gerçekleştirin   


## <a name="getting-started"></a>Başlarken

Microsoft bulut tabanlı kimlikleri birden fazla bir süredir güvenli. Azure Active Directory kimlik koruması ile ortamınızda Microsoft kimlikleri güvenli hale getirmek için kullandığı aynı koruma sistemleri kullanabilirsiniz.

Güvenlik ihlallerini çoğunluğu saldırganlar bir ortamda bir kullanıcının kimliğini çalarak erişmek zaman yer ayırın. Yıllar içinde saldırganlar üçüncü taraf ihlallerini yararlanan ve Gelişmiş kimlik avı saldırıları kullanarak giderek etkin hale getirildi. Bir saldırgan düşük ayrıcalıklı kullanıcı hesaplarına bile erişim elde hemen bunları yanal hareket önemli şirket kaynaklarına erişim kazanmak daha kolay olduğundan.

Bu gruplarındaki sonucu olarak, şunları yapmanız gerekir:

- Ayrıcalık düzeylerini bağımsız olarak tüm kimlikleri koru

- Proaktif olarak kötüye gelen güvenliği aşılmış kimlik engelle

Güvenliği aşılmış kimlikleri keşfetme hiçbir kolay bir görevdir. Daha fazla bilgi ve potansiyel olarak belirtmek şüpheli olayları algılamak için buluşsal yöntemler kimlikleri riske ve Azure Active Directory Uyarlamalı machine learning algoritmaları kullanır. Bu verileri kullanarak, kimlik koruması, raporları ve algılanan sorunlarını değerlendirin ve uygun azaltma veya düzeltme eylemlerini gerçekleştirin sağlayan uyarıları oluşturur.

Azure Active Directory kimlik koruması izleme ve Raporlama Aracı büyük. Kuruluşunuzdaki kimlikleri korumak için belirtilen risk düzeyi erişildiğinde otomatik olarak algılanan sorunları yanıt risk tabanlı ilkeleri yapılandırabilirsiniz. Azure Active Directory ve EMS tarafından sağlanan diğer koşullu erişim denetimlerini yanı sıra bu ilkeleri otomatik olarak engellemek ya da dahil olmak üzere parola sıfırlamaları Uyarlamalı düzeltme eylemleri ve çok faktörlü kimlik doğrulaması zorlama başlatın.


#### <a name="identity-protection-capabilities"></a>Kimlik koruma özellikleri

**Algılama Güvenlik Açıkları ve riskli hesapları:**  

* Genel güvenlik duruşunu güvenlik açıkları vurgulayarak güvenliğini geliştirmek için özel öneriler sağlama
* Oturum açma risk düzeyleri hesaplama
* Kullanıcı risk düzeyleri hesaplama


**Risk olaylarını araştırma:**

* Risk olayları için bildirimleri gönderme
* Risk olaylarını ilgili ve bağlamsal bilgileri kullanarak araştırma
* Araştırmalar izlemek için temel iş akışı sağlama
* Parola sıfırlama gibi düzeltme eylemleri kolay erişim sağlama

**Risk bağlı olarak koşullu erişim ilkeleri:**

* Oturum açma işlemleri engelleme veya çok faktörlü kimlik doğrulaması zorluklarını gerektirerek riskli oturum açma işlemlerini azaltmak için ilke
* Blok veya güvenli riskli kullanıcı hesapları için ilke
* Çok faktörlü kimlik doğrulaması için kullanıcıların İlkesi



## <a name="identity-protection-roles"></a>Kimlik koruması rolleri

Yük Dengelemesi için kimlik koruması uygulamanız geçici yönetimi etkinlikleri, çeşitli roller atayabilirsiniz. Azure AD Identity Protection 3 dizin rollerini destekler:

| Rol                         | Yapabilirsiniz                          | Yapamaz
| :--                          | ---                                |  ---   |
| Genel yönetici         | Kimlik koruması, yerleşik kimlik koruması için tam erişim| |
| Güvenlik yöneticisi       | Kimlik koruması için tam erişim | Yerleşik kimlik koruması, bir kullanıcı parolalarını sıfırlama |
| Güvenlik okuyucusu              | Salt okunur erişim için kimlik koruması | Yerleşik kimlik koruması, remidiate kullanıcılar, ilkelerini yapılandırmak, parolaları sıfırlama |




Daha fazla ayrıntı için bkz: [Azure Active Directory'de yönetici rolleri atama](active-directory-assign-admin-roles-azure-portal.md)



## <a name="detection"></a>Algılama

### <a name="vulnerabilities"></a>Güvenlik Açıkları

Azure Active Directory kimlik koruması yapılandırmanızı analizleri yaparken ve kullanıcı kimlikleri üzerinde bir etkisi olabilir güvenlik açıkları algılar. Daha fazla ayrıntı için bkz: [Azure Active Directory kimlik koruması tarafından algılanan Güvenlik Açığı](active-directory-identityprotection-vulnerabilities.md).

### <a name="risk-events"></a>Risk olayları

Azure Active Directory kullanıcı kimlikleri ilgili kuşkulu eylemleri algılamak için Uyarlamalı machine learning algoritmaları ve buluşsal yöntemler kullanır. Sistem algılanan her şüpheli eylemi için bir kayıt oluşturur. Bu kayıtları olarak da bilinen risk olaylardır.  
Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md).


## <a name="investigation"></a>Araştırma
Yolculuğunuzun kimlik koruması aracılığıyla genellikle kimlik koruması panosu ile başlar.

![Düzeltme](./media/active-directory-identityprotection/1000.png "düzeltme")

Pano, erişmenizi sağlar:

* Gibi raporları **bayrak eklenen kullanıcılar için risk**, **Risk olayları** ve **güvenlik açıkları**
* Yapılandırması gibi ayarları, **güvenlik ilkeleri**, **bildirimleri** ve **çok faktörlü kimlik doğrulaması kayıt**

Genellikle, etkinlikleri, günlükler ve düzeltme veya azaltma adımları gerekli olup olmadığını karar vermek için bir risk olayı ile ilgili diğer ilgili bilgileri gözden geçirme işlemi olduğundan, araştırma ve kimliğini nasıl değiştirildiği için başlangıç noktası. gizliliği ve güvenliği aşılmış kimlik nasıl kullanıldığını anladığınızdan emin olun.

Araştırma etkinliklerinizi bağlayabilirsiniz [bildirimleri](active-directory-identityprotection-notifications.md) Azure Active Directory koruma e-posta gönderir.

Aşağıdaki bölümlerde daha fazla ayrıntı ve bir araştırma ilgili adımları sağlar.  


## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

Azure Active Directory algılar [risk olayı türleri](active-directory-reporting-risk-events.md#risk-event-types) gerçek zamanlı ve çevrimdışı. Bir oturum açma için kullanıcı algılanan her risk olayı riskli oturum açma adı verilen mantıksal bir kavramken katkıda bulunur. Bir riskli oturum açma bir meşru bir kullanıcı hesabı sahibi tarafından gerçekleştirilmiş olabilecek olmayan bir oturum açma girişimi için göstergesidir.


### <a name="sign-in-risk-level"></a>Oturum açma risk düzeyi

Oturum açma risk düzeyi bir oturum açma girişimi meşru bir kullanıcı hesabı sahibi tarafından gerçekleştirilmedi bir (yüksek, Orta veya düşük) olasılığını göstergesidir.

### <a name="mitigating-sign-in-risk-events"></a>Oturum açma riski olaylar azaltılması

Bir azaltma, saldırgan güvenliği aşılmış kimlik veya cihaz kimliği veya cihaza güvenli bir duruma geri yüklemeden yararlanma yeteneği sınırlamak için bir eylemdir. Bir azaltma kimlik veya aygıtla ilişkili önceki oturum açma risk olaylarını çözümlenmiyor.

Riskli oturum açma işlemleri otomatik olarak azaltmak için oturum açma riski güvenlik ilkelerini yapılandırabilirsiniz. Bu ilkeleri kullanarak, kullanıcı veya oturum açma riskli oturum açma işlemleri engelleme veya kullanıcının gerektiren çok faktörlü kimlik doğrulaması gerçekleştirmek için risk düzeyini göz önünde bulundurun. Bu Eylemler, bir saldırgan zarar görmesine neden bir çalınan kimlik bilgisinden faydalanmakta engelleyebilir ve kimliğini güvenli hale getirmek için biraz zaman verebilir.

### <a name="sign-in-risk-security-policy"></a>Oturum açma riski güvenlik ilkesi
Bir özel oturum açma riski değerlendirir ve önceden tanımlanmış koşullara ve kurallarına göre Azaltıcı Etkenler geçerli bir koşullu erişim ilkesi oturum açma riski ilkesidir.

![Oturum açma risk ilkesine](./media/active-directory-identityprotection/1014.png "risk ilkesine oturum açma")

Azure AD kimlik koruması olanak tanıyarak riskli oturum açma işlemleri azaltma yönetmenize yardımcı olur:

* Kullanıcılar ve gruplar ilkenin uygulandığı ayarlayın:

    ![Oturum açma risk ilkesine](./media/active-directory-identityprotection/1015.png "risk ilkesine oturum açma")
* İlke tetikleyen oturum açma risk düzeyi eşiği (düşük, Orta veya yüksek) ayarlayın:

    ![Oturum açma risk ilkesine](./media/active-directory-identityprotection/1016.png "risk ilkesine oturum açma")
* İlke harekete geçirdiğinde zorlanacak denetimleri ayarlayın:  

    ![Oturum açma risk ilkesine](./media/active-directory-identityprotection/1017.png "risk ilkesine oturum açma")
* İlkeniz durumunu anahtarı:

    ![MFA kayıt](./media/active-directory-identityprotection/403.png "MFA kayıt")
* Gözden geçirin ve onu etkinleştirmek önce değişikliğin etkisini değerlendirin:

    ![Oturum açma risk ilkesine](./media/active-directory-identityprotection/1018.png "risk ilkesine oturum açma")

#### <a name="what-you-need-to-know"></a>Bilmeniz gerekenler
Çok faktörlü kimlik doğrulaması istemek için bir oturum açma riski güvenlik ilkesini yapılandırabilirsiniz:

![Oturum açma risk ilkesine](./media/active-directory-identityprotection/1017.png "risk ilkesine oturum açma")

Ancak, güvenlik nedeniyle, bu ayar yalnızca çok faktörlü kimlik doğrulaması için önceden kaydedilmiş olan kullanıcılar için çalışır. Çok faktörlü kimlik doğrulaması için henüz kaydedilmemiş bir kullanıcı için çok faktörlü kimlik doğrulama gerektirecek şekilde Koşul gerçekleştiyse, kullanıcı engellendi.

Riskli oturum açmalar için çok faktörlü kimlik doğrulamasını zorunlu kılmak istiyorsanız en iyi uygulama, aşağıdakileri yapmalısınız:

1. Etkinleştirme [çok faktörlü kimlik doğrulaması kayıt ilkesi](#multi-factor-authentication-registration-policy) etkilenen kullanıcılar için.
2. Etkilenen kullanıcılar oturum açmak için MFA kayıt gerçekleştirmek için riskli olmayan bir oturumda gerektirir

Bu adımları tamamladıktan, çok faktörlü kimlik doğrulamasının bir riskli oturum açma için gerekli sağlar.

#### <a name="best-practices"></a>En iyi uygulamalar
Seçerek bir **yüksek** eşik bir ilke tetiklenir ve kullanıcılara etkisini en aza indirger zamanların sayısını azaltır.  

Ancak, dışlar **düşük** ve **orta** gerçekleştirilen oturum açma bayrağı risk ilkesinden, saldırgan güvenliği aşılmış kimlik bilgisinden faydalanmakta gelen engelleyebilir değil.

İlke ayarlarken

* Sağlamadığı / çok faktörlü kimlik doğrulamasına sahip olmadığınız kullanıcılar Dışla
* Yerel ayarlar ilkesini etkinleştirme olduğu pratik kullanıcılar hariç tutmak (örneğin hiçbir erişim Yardım Masası)
* Çok fazla yanlış pozitifler (geliştiriciler, güvenlik analistleri) oluşturmak büyük olasılıkla kullanıcıları dışlama
* Kullanım bir **yüksek** eşiği ilk ilke dağıtımı, sırasında veya son kullanıcılar tarafından görülen sorunları en aza gerekir.
* Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşik ek kullanıcı oturum açma zorluklar, ancak daha yüksek düzeyde güvenlik sunar.

İçin bir kural yapılandırmak için çoğu kuruluş için önerilen varsayılan olan bir **orta** kullanılabilirlik ve güvenlik arasında bir denge eşiği.

Oturum açma risk ilkesine şöyledir:

* Tüm tarayıcı trafiğini ve modern kimlik doğrulaması kullanarak oturum açma işlemleri için uygulanır.
* WS-Trust uç noktada ADFS gibi Federasyon IDP devre dışı bırakarak eski güvenlik protokollerini kullanarak uygulamalar için geçerli değil.

**Risk olaylarını** kimlik koruması konsolundaki sayfasında tüm olayları listeler:

* Bu ilkenin uygulandığı
* Gözden geçirme etkinliği ve eylem uygun olup olmadığını belirleme

İlgili kullanıcı deneyimi genel bakış için bkz:

* [Oturum açma riskli kurtarma](active-directory-identityprotection-flows.md#risky-sign-in-recovery)
* [Riskli oturum engellenen açma](active-directory-identityprotection-flows.md#risky-sign-in-blocked)  
* [Oturum açma deneyimlerini Azure AD kimlik koruması](active-directory-identityprotection-flows.md)  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD Identity Protection** dikey penceresinde, **yapılandırma** 'yi tıklatın **oturum açma risk ilkesine**.

    ![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1014.png "kullanıcı ridk İlkesi")



## <a name="users-flagged-for-risk"></a>Riskli oldukları belirlenen kullanıcılar

Tüm etkin [risk olayları](active-directory-identity-protection-risk-events.md) , algılandı Azure Active Directory tarafından katkıda bulunan bir kullanıcı için kullanıcı risk adlı mantıksal bir kavramken. Risk bayrak eklenen kullanıcı geçirildiğini bir kullanıcı hesabı için bir göstergesidir.

![Riskli oldukları belirlenen kullanıcılar](./media/active-directory-identityprotection/1200.png)


### <a name="user-risk-level"></a>Kullanıcı risk düzeyi

Bir kullanıcı risk düzeyi (yüksek, Orta veya düşük), kullanıcının kimliğini aşılmış olasılığını göstergesidir. Bir kullanıcı kimliğiyle ilişkili kullanıcı risk olaylarına göre hesaplanır.

Bir risk olayı ya da durumudur **etkin** veya **kapalı**. Yalnızca olayları risk **etkin** kullanıcı risk düzeyi hesaplamaya katkıda.

Kullanıcı risk düzeyi, aşağıdaki girişleri kullanılarak hesaplanır:

* Kullanıcı etkileyen etkin risk olayları
* Bu olayların risk düzeyi
* Gerçekleştirilen tüm düzeltme eylemlerini olup önlemlerin

![Kullanıcı riskleri](./media/active-directory-identityprotection/1031.png "kullanıcı riskleri")

Oturum açma riskli kullanıcıların engellemek koşullu erişim ilkeleri oluşturmak için kullanıcı risk düzeyleri kullanın veya güvenli bir şekilde parolalarını değiştirmek için bunları zorlayın.

### <a name="closing-risk-events-manually"></a>Risk olaylarını el ile kapatma

Çoğu durumda, risk olayları otomatik olarak kapatmak için sıfırlama güvenli bir parola gibi düzeltme eylemleri gerçekleştirecek. Ancak, bu her zaman mümkün olmayabilir.  
Bu, örneğin, bir durumdur, ne zaman:

* Etkin risk olaylarına sahip bir kullanıcı silindi
* Bildirilen risk olayı sahip olan gerçekleştireceğini yasal kullanıcı tarafından bir araştırma ortaya çıkarır

Risk olaylarını olduğundan **etkin** kullanıcı risk hesaplama katkıda, el ile risk olaylarını el ile kapatarak risk düzeyini düşürmek olabilir.  
Araştırma sürecinde herhangi bir risk olayı durumunu değiştirmek için aşağıdaki eylemlerden birini gerçekleştirin seçebilirsiniz:

![Eylemler](./media/active-directory-identityprotection/34.png "Eylemler")

* **Gidermek** - bir risk olayı araştırdıktan sonra uygun düzeltme eylemi kimlik koruması dışında sürdü ve risk olayı kapalı, düşünülmesi gereken düşünüyorsanız olayı çözümlenmiş olarak işaretler. Çözümlenmiş olaylar risk olayın durumu kapalı olarak ayarlanır ve risk olay artık kullanıcı risk katkıda bulunur.
* **Hatalı pozitif olarak işaretlemek** -bazı durumlarda, bir risk olayı araştırmak ve olabilirsiniz, yanlış bir riskli işaretlenen olduğunu bulmak. Risk olayı hatalı pozitif olarak işaretleyerek bu tür yinelenme sayısını azaltmaya yardımcı olabilir. Bu, makine öğrenimi algoritmaları sınıflandırma benzer olayların gelecekte geliştirmeye yardımcı olur. Hatalı pozitif olayları için durumudur **kapalı** ve kullanıcı risk artık katkıda bulunur.
* **Yoksay** - herhangi bir düzeltme eylemi olmadı ancak etkin listeden kaldırılacak risk olayı istiyorsanız bir risk olayı yoksay işaretleyebilirsiniz ve olay durumu kapatılacak. Yoksayılan olayları için kullanıcı riski katkıda bulunmamaktadır. Bu seçenek yalnızca olağan dışı durumlarda kullanılmalıdır.
* **Yeniden etkinleştirme** -Risk el ile kapatılan olaylar (seçerek **gidermek**, **yanlış pozitif**, veya **Yoksay**) olayı ayarlanırken etkinleştirilebilir Durum geri **etkin**. Yeniden etkinleştirilen risk olaylarını kullanıcı risk düzeyi hesaplama katkıda. Risk olaylarına (güvenli bir parola sıfırlama gibi) düzeltme aracılığıyla kapalı etkinleştirilemez.

**İlgili yapılandırma iletişim kutusunu açmak için**:

1. Üzerinde **Azure AD Identity Protection** dikey altında **Araştır**, tıklatın **Risk olayları**.

    ![El ile parola sıfırlama](./media/active-directory-identityprotection/1002.png "el ile parola sıfırlama")
2. İçinde **Risk olayları** listesinde, bir risk tıklayın.

    ![El ile parola sıfırlama](./media/active-directory-identityprotection/1003.png "el ile parola sıfırlama")
3. Risk dikey penceresinde, bir kullanıcı sağ tıklayın.

    ![El ile parola sıfırlama](./media/active-directory-identityprotection/1004.png "el ile parola sıfırlama")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Bir kullanıcı için tüm risk olaylarını el ile kapatma
El ile bir kullanıcı için risk olayları ayrı ayrı kapatmak yerine, Azure Active Directory kimlik koruması Ayrıca, tek bir tıklatmayla bir kullanıcı için tüm olayları kapatmak için bir yöntem sağlar.

![Eylemler](./media/active-directory-identityprotection/2222.png "Eylemler")

Tıkladığınızda **tüm olayları kapatmak**, tüm olayları kapatılır ve etkilenen kullanıcı artık risk altında.

### <a name="remediating-user-risk-events"></a>Düzelterek kullanıcı risk olayı

Bir düzeltme güvenli bir kimlik veya önceden şüpheli veya tehlikeye bilinen bir cihaz için bir eylemdir. Bir düzeltme eylemi kimliği veya cihaza güvenli bir duruma geri yükler ve kimlik veya aygıtla ilişkili önceki risk olaylarını giderir.

Kullanıcı risk olaylarını düzeltmek için şunları yapabilirsiniz:

* Güvenli parola kullanıcı risk olaylarına elle düzeltmek için sıfırlaması gerçekleştirme
* Bir kullanıcı risk Güvenlik İlkesi'azaltılmasına veya kullanıcı risk olayları otomatik olarak düzeltmek için yapılandırma
* Etkilenen aygıt yeniden görüntü  

#### <a name="manual-secure-password-reset"></a>El ile güvenli parola sıfırlama
Güvenli parola sıfırlama birçok risk olayı için geçerli bir düzeltme ve gerçekleştirildiğinde, otomatik olarak bu risk olaylarını kapatır ve kullanıcı risk düzeyi yeniden hesaplar. Parola için riskli bir kullanıcı sıfırlama başlatmak için kimlik koruması panoyu kullanabilirsiniz.

İlgili iletişim kutusu, bir parola sıfırlama için iki farklı yöntem sunar:

**Parola sıfırlama** - seçin **parolasını sıfırlamak kullanıcının gerektiren** kullanıcı çok faktörlü kimlik doğrulaması için kaydedildiyse otomatik olarak kurtarmaya izin vermek için. Kullanıcının bir sonraki oturum açma sırasında kullanıcı çok faktörlü kimlik doğrulaması sınaması başarılı bir şekilde çözmek için gereken ve daha sonra parolayı değiştirmek zorunda. Kullanıcı hesabı kayıtlı çok faktörlü kimlik doğrulama değilse, bu seçenek kullanılamaz.

**Geçici parolayı** - seçin **geçici bir parola oluşturmak** hemen mevcut parolayı geçersiz kılmak ve kullanıcı için yeni bir geçici parola oluşturun. Yeni bir geçici parola kullanıcı için bir alternatif e-posta adresi veya Kullanıcı Yöneticisi göndermek. Parola geçici olduğundan, kullanıcı oturum açma sırasında parola değiştirme istenir.

![İlke](./media/active-directory-identityprotection/1005.png "İlkesi")

**İlgili yapılandırma iletişim kutusunu açmak için**:

1. Üzerinde **Azure AD Identity Protection** dikey penceresinde tıklatın **bayrak eklenen kullanıcılar için risk**.

    ![El ile parola sıfırlama](./media/active-directory-identityprotection/1006.png "el ile parola sıfırlama")
2. Kullanıcılar listesinden en az bir risk olaylarına sahip bir kullanıcı seçin.

    ![El ile parola sıfırlama](./media/active-directory-identityprotection/1007.png "el ile parola sıfırlama")
3. Kullanıcı dikey penceresinde **parola sıfırlama**.

    ![El ile parola sıfırlama](./media/active-directory-identityprotection/1008.png "el ile parola sıfırlama")

### <a name="user-risk-security-policy"></a>Kullanıcı risk güvenlik ilkesi
Kullanıcı risk güvenlik ilkesi belirli bir kullanıcıya risk düzeyi değerlendirir ve önceden tanımlanmış koşullara ve kurallarına göre düzeltme ve azaltma Eylemler uyguladığında bir koşullu erişim ilkesi var.

![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1009.png "kullanıcı ridk İlkesi")

Azure AD kimlik koruması azaltma ve risk olanak tanıyarak bayrak eklenen kullanıcılar düzeltmesi yönetmenize yardımcı olur:

* Kullanıcılar ve gruplar ilkenin uygulandığı ayarlayın:

    ![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1010.png "kullanıcı ridk İlkesi")
* İlke tetikleyen kullanıcı risk düzeyi eşiği (düşük, Orta veya yüksek) ayarlayın:

    ![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1011.png "kullanıcı ridk İlkesi")
* İlke harekete geçirdiğinde zorlanacak denetimleri ayarlayın:

    ![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1012.png "kullanıcı ridk İlkesi")
* İlkeniz durumunu anahtarı:

    ![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/403.png "MFA kayıt")
* Gözden geçirin ve onu etkinleştirmek önce değişikliğin etkisini değerlendirin:

    ![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1013.png "kullanıcı ridk İlkesi")

Seçerek bir **yüksek** eşik bir ilke tetiklenir ve kullanıcılara etkisini en aza indirger zamanların sayısını azaltır.
Ancak, dışlar **düşük** ve **orta** kimlikleri veya cihazları, güvenli değil ilkesinden risk için işaretlenmiş kullanıcılar önceden şüpheli veya tehlikeye bilinmektedir.

İlke ayarlarken

* Çok fazla yanlış pozitifler (geliştiriciler, güvenlik analistleri) oluşturmak büyük olasılıkla kullanıcıları dışlama
* Yerel ayarlar ilkesini etkinleştirme olduğu pratik kullanıcılar hariç tutmak (örneğin hiçbir erişim Yardım Masası)
* Kullanım bir **yüksek** eşiği ilk ilke dağıtımı, sırasında veya son kullanıcılar tarafından görülen sorunları en aza gerekir.
* Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşik ek kullanıcı oturum açma zorluklar, ancak daha yüksek düzeyde güvenlik sunar.

İçin bir kural yapılandırmak için çoğu kuruluş için önerilen varsayılan olan bir **orta** kullanılabilirlik ve güvenlik arasında bir denge eşiği.

İlgili kullanıcı deneyimi genel bakış için bkz:

* [Hesap kurtarma akışı tehlikeye](active-directory-identityprotection-flows.md#compromised-account-recovery).  
* [Hesap engellendi akış tehlikeye](active-directory-identityprotection-flows.md#compromised-account-blocked).  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD Identity Protection** dikey penceresinde, **yapılandırma** 'yi tıklatın **kullanıcı risk ilkesine**.

    ![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1009.png "kullanıcı ridk İlkesi")

### <a name="mitigating-user-risk-events"></a>Kullanıcı risk olaylar azaltılması
Yöneticiler kullanıcı risk güvenlik ilkesi oturum açma risk düzeyine bağlı olarak bağlı kullanıcıları engelle ayarlayabilir.

Bir oturum açma engelleme:

* Etkilenen kullanıcı için yeni kullanıcı risk olaylarına oluşturulmasını engeller
* Yöneticilerin el ile kullanıcı kimliğini etkileyen risk olaylarına düzeltin ve güvenli bir duruma geri olanak tanır



## <a name="multi-factor-authentication-registration-policy"></a>Çok faktörlü kimlik doğrulaması kayıt ilkesi
Azure çok faktörlü kimlik doğrulaması daha fazlasını bir kullanıcı adı ve parola kullanılmasını gerektiren kim olduğunuzu doğrulama bir yöntemdir. İkinci bir kullanıcı oturum açmaları ve işlemleri için güvenlik katmanı sağlar.  
Çünkü kullanıcı oturum açma işlemleri için Azure çok faktörlü kimlik doğrulaması ihtiyaç duyduğunuz öneririz:

* Kolay doğrulama seçeneklerini aralıklı güçlü kimlik doğrulaması sunar
* Kuruluşunuz korumak ve hesap ödün kurtarmak için hazırlama önemli bir rol oynar

![Kullanıcı ridk İlkesi](./media/active-directory-identityprotection/1019.png "kullanıcı ridk İlkesi")

Daha fazla ayrıntı için bkz: [Azure multi-Factor Authentication nedir?](authentication/multi-factor-authentication.md)

Azure AD kimlik koruması sağlayan bir ilkesi yapılandırarak üretimini çok faktörlü kimlik doğrulaması kayıt yönetmenize yardımcı olur:

* Kullanıcılar ve gruplar ilkenin uygulandığı ayarlayın:

    ![MFA ilkesini](./media/active-directory-identityprotection/1020.png "MFA İlkesi")
* İlke harekete geçirdiğinde yürütülebilmesi için denetimleri ayarlama::  

    ![MFA ilkesini](./media/active-directory-identityprotection/1021.png "MFA İlkesi")
* İlkeniz durumunu anahtarı:

    ![MFA ilkesini](./media/active-directory-identityprotection/403.png "MFA İlkesi")
* Geçerli kayıt durumunu görüntüleyin:

    ![MFA ilkesini](./media/active-directory-identityprotection/1022.png "MFA İlkesi")

İlgili kullanıcı deneyimi genel bakış için bkz:

* [Çok faktörlü kimlik doğrulaması kayıt akışı](active-directory-identityprotection-flows.md#multi-factor-authentication-registration).  
* [Azure AD kimlik koruması ile karşılaştığında oturum açma](active-directory-identityprotection-flows.md).  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD Identity Protection** dikey penceresinde, **yapılandırma** 'yi tıklatın **çok faktörlü kimlik doğrulaması kayıt**.

    ![MFA ilkesini](./media/active-directory-identityprotection/1019.png "MFA İlkesi")

## <a name="next-steps"></a>Sonraki adımlar
* [Kanal 9: Azure AD ve kimlik göster: kimlik koruması Önizleme](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Azure Active Directory kimlik koruması etkinleştirme](active-directory-identityprotection-enable.md)

* [Azure Active Directory kimlik koruması tarafından algılanan güvenlik açığı](active-directory-identityprotection-vulnerabilities.md)

* [Azure Active Directory risk olayları](active-directory-identity-protection-risk-events.md)

* [Azure Active Directory kimlik koruması bildirimleri](active-directory-identityprotection-notifications.md)

* [Azure Active Directory kimlik koruması Kılavuzu](active-directory-identityprotection-playbook.md)

* [Azure Active Directory kimlik koruması Kılavuzu sözlüğü](active-directory-identityprotection-glossary.md)

* [Oturum açma deneyimlerini Azure AD kimlik koruması](active-directory-identityprotection-flows.md)

* [Azure Active Directory kimlik koruması - kullanıcıların engelini kaldırma](active-directory-identityprotection-unblock-howto.md)

* [Azure Active Directory kimlik koruması ve Microsoft Graph kullanmaya başlama](active-directory-identityprotection-graph-getting-started.md)
