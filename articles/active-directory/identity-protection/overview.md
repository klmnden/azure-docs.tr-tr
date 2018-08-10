---
title: Azure Active Directory kimlik koruması | Microsoft Docs
description: Azure AD kimlik koruması, güvenliği aşılmış kimlik veya cihaz yararlanma ve güvenli bir kimlik veya daha önce olduğundan şüphelenilen veya tehlikeye bilinen bir cihaz yeteneği bir saldırganın sınırlamak nasıl olanak tanıdığını öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 06e3a596b60bf96319071fff68b0bf1655869559
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40003806"
---
# <a name="azure-active-directory-identity-protection"></a>Azure Active Directory Kimlik Koruması

Azure Active Directory kimlik koruması sayesinde Azure AD Premium P2 sürümünü özelliğidir:

- Kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılama

- Otomatik yanıtları, kuruluşunuzun kimliklerini ilgili algılanan kuşkulu eylemleri için yapılandırma  

- Şüpheli olayları araştırmanıza ve bunları çözmek için gerekli adımları uygulayın   


## <a name="getting-started"></a>Başlarken

Microsoft bulut tabanlı kimliklerin aşkın için güvenli. Azure Active Directory kimlik koruması ile ortamınızda kimlikleri güvenliğini sağlamak için Microsoft'un kullandığı aynı koruma sistemlerini kullanabilirsiniz.

Güvenlik ihlallerini büyük çoğunluğu göz önüne bir yerde saldırganların bir kullanıcının kimliğini çalarak bir ortama erişimi elde edin. Yıllar içinde saldırganlar kullanarak gelişmiş kimlik avı saldırıları ve üçüncü taraf ihlallerini yararlanarak, giderek daha etkili hale gelmiştir. Bir saldırganın da düşük ayrıcalıklı kullanıcı hesapları için erişim kazanır hemen sonra bunları yanal hareket önemli şirket kaynaklarına erişim kazanmak oldukça kolay bir işlemdir.

Bu söz konusu kümelerdeki şunları yapmanız gerekir:

- Kendi ayrıcalık düzeyi ne olursa olsun tüm kimlikleri koruyun

- Proaktif olarak güvenliği aşılmış kimlik kötüye gelen engelle

Tehlikeye atılmış kimlik keşfetme hiçbir kolay bir görevdir. Azure Active Directory Uyarlamalı makine öğrenimi algoritmaları kullanır ve anormallikleri ve olası gösterir şüpheli olayları algılamak için buluşsal yöntemler kimlikleri riske. Kimlik koruması, bu verileri kullanarak raporlar ve algılanan sorunlarını değerlendirin ve uygun bir risk azaltma veya düzeltme eylemleri olanak tanıyan uyarılar oluşturur.

Azure Active Directory kimlik koruması bir izleme ve raporlama aracıyla daha büyük. Kuruluşunuzun kimliklerini korumak için belirtilen risk düzeyi ulaşıldığında otomatik olarak algılanan sorunlara yanıt risk tabanlı ilkeler yapılandırabilirsiniz. Azure Active Directory ve EMS tarafından sağlanan diğer koşullu erişim denetimleri ek olarak bu ilkeleri otomatik olarak engellemek veya uyarlamalı düzeltme eylemleri de dahil olmak üzere parolasını sıfırlar ve çok faktörlü kimlik doğrulaması zorlama başlatın.


#### <a name="identity-protection-capabilities"></a>Kimlik koruma özellikleri

**Güvenlik Açıkları ve riskli hesapları algılama:**  

* Güvenlik açıklarını vurgulayarak genel güvenlik duruşunu geliştirmek için özel öneriler sağlama
* Oturum açma risk düzeyleri hesaplanıyor
* Kullanıcı risk düzeyleri hesaplanıyor


**Risk olayları araştırma:**

* Risk olayları için bildirimleri gönderme
* Risk olayları ve ilgili bağlamsal bilgileri kullanarak araştırma
* Araştırmalar izlemek için temel iş akışı sağlama
* Düzeltme eylemleri parola sıfırlama gibi kolay erişim sağlama

**Risk tabanlı koşullu erişim ilkeleri:**

* Riskli oturum açma, oturum açma engelleme veya çok faktörlü kimlik doğrulaması zorluklarını gerektirerek azaltmak için ilke
* Blok veya güvenli riskli kullanıcı hesapları için ilke
* Kullanıcı çok faktörlü kimlik doğrulamasına kaydolacak şekilde zorunlu tutmak için ilke



## <a name="identity-protection-roles"></a>Kimlik koruması rolleri

Yük Dengelemesi için yönetimi etkinlikleri, kimlik koruması uygulamanızın etrafında çeşitli roller atayabilirsiniz. Azure AD kimlik koruması 3 dizin rolünü destekler:

| Rol                         | Yapabilirsiniz                          | Bunu yapamazsınız
| :--                          | ---                                |  ---   |
| Genel yönetici         | Kimlik koruması, yerleşik kimlik koruması tam erişim| |
| Güvenlik yöneticisi       | Kimlik koruması tam erişim | Yerleşik kimlik koruması, kullanıcı parolalarını sıfırlama |
| Güvenlik okuyucusu              | Kimlik koruması salt okunur erişim | Yerleşik kimlik koruması, remidiate kullanıcılar ilkelerini yapılandırma, parolaları sıfırlama |




Daha fazla ayrıntı için [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)



## <a name="detection"></a>Algılama

### <a name="vulnerabilities"></a>Güvenlik Açıkları

Azure Active Directory kimlik koruması yapılandırmanızı analiz eder ve kullanıcılarınızın kimliklerini üzerinde bir etkisi olan güvenlik açıklarını algılar. Daha fazla ayrıntı için [Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını](vulnerabilities.md).

### <a name="risk-events"></a>Risk olayları

Azure Active Directory, kullanıcılarınızın kimliklerini ilgili kuşkulu eylemleri algılamak için Uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Sistem algılanan her bir şüpheli işlem için bir kayıt oluşturur. Bu kayıtlar risk olayları da verilir.  
Daha ayrıntılı bilgi için bkz. [Azure Active Directory risk olayları](../active-directory-identity-protection-risk-events.md).


## <a name="investigation"></a>Araştırma
Kimlik koruması aracılığıyla yolculuğunuza genellikle kimlik koruması panosu ile başlar.

![Düzeltme](./media/overview/1000.png "düzeltme")

Pano, aşağıdakilere erişmenizi sağlar:

* Raporları gibi **risk için işaretlenen kullanıcılar**, **Risk olayları** ve **güvenlik açıkları**
* Yapılandırması gibi ayarları, **güvenlik ilkeleri**, **bildirimleri** ve **çok faktörlü kimlik doğrulaması kaydı**

Genellikle, başlangıç noktası olan etkinlikleri, günlükleri ve düzeltme ya da risk azaltma adımlarını gerekli olup olmadığını karar vermek için bir risk olayını ilgili diğer ilgili bilgileri gözden geçirme işlemi, araştırma ve kimlik nasıldı. gizliliği ve güvenliği aşılmış kimlik nasıl kullanıldığını anlayın.

Araştırma etkinliklerinizi bağlayabilirsiniz [bildirimleri](notifications.md) Azure Active Directory koruması e-posta gönderir.

Aşağıdaki bölümlerde, daha fazla ayrıntı ve araştırma için ilgili adımları sağlar.  


## <a name="risky-sign-ins"></a>Riskli oturum açma işlemleri

Azure Active Directory algılar [risk olayı türleri](../reports-monitoring/concept-risk-events.md#risk-event-types) gerçek zamanlı ve çevrimdışı. Her risk olayı bir oturum açma için kullanıcı algılanan riskli oturum açma adı verilen mantıksal bir kavramken katkıda bulunur. Bir riskli oturum açma kullanıcı hesabının meşru sahibi tarafından gerçekleştirilmiş olabilecek olmayan bir oturum açma girişiminin göstergesidir.


### <a name="sign-in-risk-level"></a>Oturum açma riski düzeyi

Bir oturum açma riski düzeyi bir oturum açma girişimi bir kullanıcı hesabının meşru sahibi tarafından gerçekleştirilmedi (yüksek, Orta veya düşük) olasılığını göstergesidir.

### <a name="mitigating-sign-in-risk-events"></a>Oturum açma risk olayları azaltma

Bir risk azaltma, saldırgan güvenliği aşılmış kimlik veya cihaz kimliği veya cihaz güvenli bir duruma geri yüklemeden yararlanma olanağı sınırlamak için bir eylemdir. Bir risk azaltma kimlik veya cihaz ile ilişkili önceki oturum açma risk olayları çözmez.

Riskli oturum açma işlemleri otomatik olarak gidermek için oturum açma riski güvenlik ilkeleri yapılandırabilirsiniz. Bu ilkeleri kullanarak, kullanıcı veya oturum açma riskli oturum açma işlemleri engellemek ya da gerektirmek için çok faktörlü kimlik doğrulaması gerçekleştirmek için risk düzeyini göz önünde bulundurun. Bu Eylemler, bir saldırganın, zarar verme bir çalınan kimlik yararlanmasını engel olabilir ve kimlik güvenli hale getirmek için biraz zaman tanımanız.

### <a name="sign-in-risk-security-policy"></a>Oturum açma riski İlkesi
Oturum açma riski İlkesi, bir özel oturum açma riskini değerlendirir ve önceden tanımlanmış koşullar ve kurallara dayanan bir risk azaltma işlemleri geçerli bir koşullu erişim ilkesi var.

![Oturum açma riski İlkesi](./media/overview/1014.png "oturum açma riski İlkesi")

Azure AD kimlik koruması sağlayarak azaltma riskli oturum açma işlemleri yönetmenize yardımcı olur:

* Kullanıcıları ve grupları ilkenin uygulanacağı ayarlayın:

    ![Oturum açma riski İlkesi](./media/overview/1015.png "oturum açma riski İlkesi")
* İlke tetikleyen oturum açma riski düzeyi eşiği (düşük, Orta veya yüksek) ayarlayın:

    ![Oturum açma riski İlkesi](./media/overview/1016.png "oturum açma riski İlkesi")
* İlke tetiklendiğinde zorlanacak denetimleri ayarlayın:  

    ![Oturum açma riski İlkesi](./media/overview/1017.png "oturum açma riski İlkesi")
* İlke durumu anahtarı:

    ![MFA kaydı](./media/overview/403.png "MFA kaydı")
* Gözden geçirin ve bunu etkinleştirdikten önce bir değişikliğin etkisini değerlendirin:

    ![Oturum açma riski İlkesi](./media/overview/1018.png "oturum açma riski İlkesi")

#### <a name="what-you-need-to-know"></a>Bilmeniz gerekenler
Çok faktörlü kimlik doğrulaması gerektirmek için bir oturum açma riski ilkesi yapılandırabilirsiniz:

![Oturum açma riski İlkesi](./media/overview/1017.png "oturum açma riski İlkesi")

Ancak, güvenlik nedeniyle, bu ayar yalnızca çok faktörlü kimlik doğrulaması için zaten kaydedilmiş olan kullanıcılar için çalışır. Kullanıcının, multi-Factor authentication için henüz kaydedilmemiş bir kullanıcı için çok faktörlü kimlik doğrulaması gerektiren bir koşulu karşılaması durumunda engellenir.

Riskli oturum açma işlemleri için multi-Factor authentication gerektirmesine istiyorsanız en iyi uygulama, şunları yapmalısınız:

1. Etkinleştirme [çok faktörlü kimlik doğrulaması kayıt ilkesi](#multi-factor-authentication-registration-policy) etkilenen kullanıcılar için.
2. Etkilenen kullanıcılar oturum açmak için bir MFA kayıt gerçekleştirmek için riskli olmayan bir oturumda gerektirir

Bu adımları bir riskli oturum açma için multi-Factor authentication gerekiyor sağlar.

#### <a name="best-practices"></a>En iyi uygulamalar
Seçim bir **yüksek** eşiği ilke tetiklenir ve kullanıcılara etkisini en aza indirir sayısını azaltır.  

Ancak, dışlar **düşük** ve **orta** oturum açma bayrağı risk güvenliği aşılmış kimlik kötüye saldırganın engellemeyebilir ilkesi için.

İlke ayarlanırken

* Olmayan / çok faktörlü kimlik doğrulamasına sahip olmadığınız kullanıcılar hariç
* Kullanıcılar yerel ilkesini etkinleştirme olduğu pratik dışında (örneğin Yardım Masası için hiçbir erişim)
* Çok sayıda yanlış pozitifleri (geliştiriciler, güvenlik analisti) üreteceği kullanıcılar hariç
* Kullanım bir **yüksek** eşiği ilk ilke üretim, sırasında veya son kullanıcılar tarafından görülen sorunları en aza indirmeniz gerekir.
* Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşiği ek kullanıcı oturum açma sorunları, ancak daha fazla güvenlik sunar.

Önerilen çoğu kuruluş için bir kural yapılandırmak için varsayılandır bir **orta** kullanılabilirlik ve güvenlik arasında bir denge için eşiği.

Oturum açma riski İlkesi şöyledir:

* Tüm tarayıcı trafik ve modern kimlik doğrulaması kullanarak oturum açma işlemleri için uygulanır.
* WS-Trust uç noktada ADFS gibi Federasyon IDP devre dışı bırakarak eski güvenlik protokolleri kullanan uygulamalar için geçerli değil.

**Risk olayları** kimlik koruması konsolundaki sayfasında tüm olayları listeler:

* Bu ilke uygulandı
* Gözden geçirme etkinliği ve eylem uygun olup olmadığını belirleme

İlgili kullanıcı deneyimini genel bakış için bkz:

* [Riskli oturum açma kurtarma](flows.md#risky-sign-in-recovery)
* [Riskli engellenen oturum açma](flows.md#risky-sign-in-blocked)  
* [Azure AD kimlik koruması ile oturum açma deneyimleri](flows.md)  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD kimlik koruması** dikey penceresindeki **yapılandırma** bölümünde **oturum açma riski İlkesi**.

    ![Kullanıcı riski İlkesi](./media/overview/1014.png "kullanıcı riski İlkesi")



## <a name="users-flagged-for-risk"></a>Riskli oldukları belirlenen kullanıcılar

Tüm etkin [risk olayları](../reports-monitoring/concept-risk-events.md) , algılandı Azure Active Directory tarafından katkıda bulunan bir kullanıcı için mantıksal bir kavramken kullanıcı riski çağrılır. Riskli olduğu belirlenen bir kullanıcı gizliliği bozulmuş olabilecek bir kullanıcı hesabının göstergesidir.

![Riskli oldukları belirlenen kullanıcılar](./media/overview/1200.png)


### <a name="user-risk-level"></a>Kullanıcı risk düzeyi

Bir kullanıcı risk düzeyi (yüksek, Orta veya düşük) kullanıcının kimliğini tehlikede olasılığını göstergesidir. Bir kullanıcı kimliğiyle ilişkili kullanıcı risk olaylarına göre hesaplanır.

Risk olayı durumu bozulmuş **etkin** veya **kapalı**. Olayları yalnızca risk **etkin** kullanıcı risk düzeyi hesaplamaya katkıda bulunun.

Kullanıcı risk düzeyi, aşağıdaki girişleri kullanılarak hesaplanır:

* Kullanıcıyı etkileyen active risk olayları
* Bu olayların risk düzeyi
* Tüm düzeltme eylemlerini alınmış olup olmadığı

![Kullanıcı risk](./media/overview/1031.png "kullanıcı risk")

Riskli kullanıcıların oturum açarken engelleyen koşullu erişim ilkeleri oluşturmak için kullanıcı risk düzeyleri kullanın veya bunları parolalarını güvenli bir şekilde değiştirmek için zorla.

### <a name="closing-risk-events-manually"></a>Risk olaylarını elle kapatma

Çoğu durumda, düzeltme eylemleri risk olayları otomatik olarak kapatmak için bir güvenli parola sıfırlama gibi gerçekleştirir. Ancak, bu her zaman mümkün olmayabilir.  
Bu, örneğin, bir durumdur, ne zaman:

* Etkin risk olayları sahip bir kullanıcı silindi
* Bir araştırma bildirilen risk olayı sahip olan gerçekleştireceğini meşru bir kullanıcı tarafından ortaya çıkarır.

Risk olayları olduğundan **etkin** katkıda bulunmak için kullanıcı risk hesaplama, risk olaylarını elle kapatma tarafından el ile bir risk düzeyi daha düşük olabilir.  
Araştırma Kurs sırasında herhangi bir risk olayını durumunu değiştirmek için aşağıdaki eylemlerden birini tercih edebilirsiniz:

![Eylemler](./media/overview/34.png "eylemleri")

* **Çözmek** - bir risk olayını araştırdıktan sonra uygun düzeltme eylemi dışında kimlik koruması sürdü ve risk olayı kapatıldı, değerlendirilmesi gerektiğini düşünüyorsanız olayı Çözüldü olarak işaretleyin. Olayları risk olayın durumu kapalı olarak ayarlanır ve risk olayının artık kullanıcı riski katkıda bulunan çözüldü.
* **Hatalı pozitif olarak işaretleme** -bazı durumlarda, bir risk olayını araştırmak ve olabilirsiniz, yanlış riskli işaretlenmiş olduğunu keşfedin. Risk olayı hatalı pozitif sonuç olarak işaretleyerek böyle oluşum sayısını azaltmaya yardımcı olabilir. Bu, makine öğrenimi algoritmaları sınıflandırma benzer olayların gelecekte artırmak için yardımcı olur. Yanlış pozitif sonuç veren olayların durumunu sağlamaktır **kapalı** ve kullanıcı riski artık katkıda bulunur.
* **Yoksay** - herhangi bir düzeltme eylemi olmamıştır, ancak etkin listesinden kaldırılması için risk olayı istiyorsanız, bir risk olayını yoksay işaretleyebilirsiniz ve olay durumu kapatılır. Kullanıcı riski yok sayılan olayları katkıda bulunmuyor. Bu seçenek yalnızca olağan dışı durumlarda kullanılmalıdır.
* **Yeniden** -Risk el ile kapatılmış olayları (seçerek **çözmek**, **hatalı pozitif sonuç**, veya **Yoksay**) olay ayarlama etkinleştirilebilir Durum geri **etkin**. Yeniden etkinleştirilen risk olayları için kullanıcı risk düzeyi hesaplama katkıda bulunur. (Güvenli parola sıfırlama gibi) düzeltme aracılığıyla kapatılan risk olayları yeniden etkinleştirilemez.

**İlgili yapılandırma iletişim kutusunu açmak için**:

1. Üzerinde **Azure AD kimlik koruması** dikey altında **Araştır**, tıklayın **Risk olayları**.

    ![El ile parola sıfırlama](./media/overview/1002.png "el ile parola sıfırlama")
2. İçinde **Risk olayları** listesinde, bir risk tıklayın.

    ![El ile parola sıfırlama](./media/overview/1003.png "el ile parola sıfırlama")
3. Risk dikey penceresinde, bir kullanıcıya sağ tıklayın.

    ![El ile parola sıfırlama](./media/overview/1004.png "el ile parola sıfırlama")

### <a name="closing-all-risk-events-for-a-user-manually"></a>Bir kullanıcı için tüm risk olaylarını elle kapatma
El ile bir kullanıcının risk olayları ayrı ayrı kapatmak yerine, Azure Active Directory kimlik koruması da, tek bir tıklamayla bir kullanıcı için tüm olayları kapatmak için bir yöntem sağlar.

![Eylemler](./media/overview/2222.png "eylemleri")

Tıkladığınızda **tüm olayları kapatılamadı**, tüm olayları kapatılır ve etkilenen kullanıcı artık risk altındadır.

### <a name="remediating-user-risk-events"></a>Düzeltme kullanıcı risk olayları

Bir düzeltme bir kimlik veya daha önce olduğundan şüphelenilen veya tehlikeye bilinen bir cihazı güvenli hale getirmek için bir eylemdir. Bir düzeltme eylemi kimliği veya cihaz güvenli bir duruma geri yükler ve kimlik ya da cihaz ile ilişkili önceki risk olayları giderir.

Kullanıcı risk olaylarını düzeltmek için şunları yapabilirsiniz:

* Güvenli parola sıfırlama kullanıcı risk olaylarını elle düzeltmek için gerçekleştirin
* Kullanıcı risk olaylarını otomatik olarak düzeltmek veya gidermek için bir kullanıcı riski ilkesi yapılandırma
* Etkilenen cihaz yeniden görüntü  

#### <a name="manual-secure-password-reset"></a>El ile güvenli parola sıfırlama
Güvenli parola sıfırlama birçok risk olayları için geçerli bir düzeltme ve ne zaman gerçekleştirileceğini otomatik olarak bu risk olaylarını kapatır ve kullanıcı risk düzeyi yeniden hesaplar. Kimlik koruması Panosu, parola sıfırlama için riskli kullanıcı başlatmak için kullanabilirsiniz.

İlgili iletişim kutusu, bir parola sıfırlama için iki farklı yöntem sunar:

**Parolayı Sıfırla** - seçin **kullanıcının parolasını sıfırlamasını gerektirmek** kullanıcı çok faktörlü kimlik doğrulaması için kayıtlı olmadığını Self kurtarmaya izin vermek için. Kullanıcının sonraki oturum açma sırasında kullanıcı çok faktörlü kimlik doğrulaması sınaması başarılı bir şekilde çözmek için gerekli ve daha sonra parolayı değiştirmek zorunda. Kullanıcı hesabı zaten kayıtlı çok faktörlü kimlik doğrulaması değilse, bu seçenek kullanılamaz.

**Geçici parola** - seçin **geçici bir parola oluştur** hemen mevcut parolayı geçersiz kılmak ve kullanıcı için yeni bir geçici parola oluşturun. Yeni bir geçici parola kullanıcı için bir alternatif e-posta adresi veya Kullanıcı Yöneticisi gönderin. Geçici bir parola olduğundan, kullanıcı oturum açma sırasında parola değiştirme istenir.

![İlke](./media/overview/1005.png "İlkesi")

**İlgili yapılandırma iletişim kutusunu açmak için**:

1. Üzerinde **Azure AD kimlik koruması** dikey penceresinde tıklayın **risk için işaretlenen kullanıcılar**.

    ![El ile parola sıfırlama](./media/overview/1006.png "el ile parola sıfırlama")
2. En az bir risk olayı bir kullanıcıyla kullanıcılar listesinden seçin.

    ![El ile parola sıfırlama](./media/overview/1007.png "el ile parola sıfırlama")
3. Kullanıcı dikey penceresinde tıklayın **parolayı Sıfırla**.

    ![El ile parola sıfırlama](./media/overview/1008.png "el ile parola sıfırlama")

### <a name="user-risk-security-policy"></a>Kullanıcı riski İlkesi
Bir kullanıcı riski İlkesi belirli bir kullanıcı risk düzeyi değerlendirir ve önceden tanımlanmış koşullar ve kurallara dayanan düzeltme ve risk azaltma eylemleri geçerli bir koşullu erişim ilkesi var.

![Kullanıcı riski İlkesi](./media/overview/1009.png "kullanıcı riski İlkesi")

Azure AD kimlik koruması risk azaltma ve olanak tanıyarak, riskli olduğu belirlenen kullanıcıları düzeltme yönetmenize yardımcı olur:

* Kullanıcıları ve grupları ilkenin uygulanacağı ayarlayın:

    ![Kullanıcı riski İlkesi](./media/overview/1010.png "kullanıcı riski İlkesi")
* İlke tetikleyen kullanıcı risk düzeyi eşiği (düşük, Orta veya yüksek) ayarlayın:

    ![Kullanıcı riski İlkesi](./media/overview/1011.png "kullanıcı riski İlkesi")
* İlke tetiklendiğinde zorlanacak denetimleri ayarlayın:

    ![Kullanıcı riski İlkesi](./media/overview/1012.png "kullanıcı riski İlkesi")
* İlke durumu anahtarı:

    ![Kullanıcı riski İlkesi](./media/overview/403.png "MFA kaydı")
* Gözden geçirin ve bunu etkinleştirdikten önce bir değişikliğin etkisini değerlendirin:

    ![Kullanıcı riski İlkesi](./media/overview/1013.png "kullanıcı riski İlkesi")

Seçim bir **yüksek** eşiği ilke tetiklenir ve kullanıcılara etkisini en aza indirir sayısını azaltır.
Ancak, dışlar **düşük** ve **orta** kimlikleri cihazları, güvenli veya değil ilkeyi, risk için işaretlenmiş kullanıcılar daha önce olduğundan şüphelenilen veya tehlikeye bilinen.

İlke ayarlanırken

* Çok sayıda yanlış pozitifleri (geliştiriciler, güvenlik analisti) üreteceği kullanıcılar hariç
* Kullanıcılar yerel ilkesini etkinleştirme olduğu pratik dışında (örneğin Yardım Masası için hiçbir erişim)
* Kullanım bir **yüksek** eşiği ilk ilke üretim, sırasında veya son kullanıcılar tarafından görülen sorunları en aza indirmeniz gerekir.
* Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşiği ek kullanıcı oturum açma sorunları, ancak daha fazla güvenlik sunar.

Önerilen çoğu kuruluş için bir kural yapılandırmak için varsayılandır bir **orta** kullanılabilirlik ve güvenlik arasında bir denge için eşiği.

İlgili kullanıcı deneyimini genel bakış için bkz:

* [Hesap kurtarma akışı tehlikeye](flows.md#compromised-account-recovery).  
* [Engellenen hesap akış tehlikeye](flows.md#compromised-account-blocked).  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD kimlik koruması** dikey penceresindeki **yapılandırma** bölümünde **kullanıcı riski İlkesi**.

    ![Kullanıcı riski İlkesi](./media/overview/1009.png "kullanıcı riski İlkesi")

### <a name="mitigating-user-risk-events"></a>Kullanıcı risk olaylarını azaltma
Yöneticiler, risk düzeyine bağlı olarak oturum açma sonrası kullanıcıları engellemek için bir kullanıcı riski ilkesi ayarlayabilirsiniz.

Bir oturum açma engelleme:

* Etkilenen kullanıcı için yeni kullanıcı risk olayları oluşturulmasını engeller
* Yöneticilerin el ile kullanıcı kimliğini etkileyen risk olaylarını düzeltmek ve güvenli bir duruma geri sağlar



## <a name="multi-factor-authentication-registration-policy"></a>Çok faktörlü kimlik doğrulaması kayıt ilkesi
Azure çok faktörlü kimlik doğrulaması yalnızca bir kullanıcı adı ve parola kullanılmasını gerektiren kim olduğunuzu doğrulama bir yöntemdir. Bu, ikinci bir kullanıcı oturum açma ve işlemler için güvenlik katmanı sağlar.  
Öneririz çünkü kullanıcı oturum açma işlemleri için Azure multi-Factor authentication gerektirmesine, onu:

* Güçlü kimlik doğrulaması ile çok sayıda kolay doğrulama seçeneği sunar
* Hazırlanması kuruluşunuz korumak ve hesabı ödün kurtarmak için önemli bir rol oynar

![Kullanıcı riski İlkesi](./media/overview/1019.png "kullanıcı riski İlkesi")

Daha fazla ayrıntı için [Azure multi-Factor Authentication nedir?](../authentication/multi-factor-authentication.md)

Azure AD kimlik koruması sağlayan bir ilkesi yapılandırarak çok faktörlü kimlik doğrulaması kaydı üretimi yönetmenize yardımcı olur:

* Kullanıcıları ve grupları ilkenin uygulanacağı ayarlayın:

    ![MFA ilkesini](./media/overview/1020.png "MFA İlkesi")
* İlke tetiklendiğinde zorlanacak denetimleri ayarlayın:  

    ![MFA ilkesini](./media/overview/1021.png "MFA İlkesi")
* İlke durumu anahtarı:

    ![MFA ilkesini](./media/overview/403.png "MFA İlkesi")
* Geçerli kayıt durumunu görüntüleyin:

    ![MFA ilkesini](./media/overview/1022.png "MFA İlkesi")

İlgili kullanıcı deneyimini genel bakış için bkz:

* [Çok faktörlü kimlik doğrulaması kayıt akışını](flows.md#multi-factor-authentication-registration).  
* [Azure AD kimlik koruması ile karşılaştığında oturum açma](flows.md).  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD kimlik koruması** dikey penceresindeki **yapılandırma** bölümünde **çok faktörlü kimlik doğrulaması kaydı**.

    ![MFA ilkesini](./media/overview/1019.png "MFA İlkesi")

## <a name="next-steps"></a>Sonraki adımlar
* [Kanal 9: Azure AD kimlik gösterin: kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

* [Azure Active Directory kimlik Koruması'nı etkinleştirme](enable.md)

* [Azure Active Directory kimlik koruması tarafından algılanan güvenlik açıklarını](vulnerabilities.md)

* [Azure Active Directory risk olayları](../active-directory-identity-protection-risk-events.md)

* [Azure Active Directory kimlik koruması bildirimleri](notifications.md)

* [Azure Active Directory kimlik koruması Kılavuzu](playbook.md)

* [Azure Active Directory kimlik koruması Kılavuzu sözlüğü](glossary.md)

* [Azure AD kimlik koruması ile oturum açma deneyimleri](flows.md)

* [Azure Active Directory kimlik koruması - kullanıcı engellemesini kaldırma](howto-unblock-user.md)

* [Microsoft Graph ve Azure Active Directory kimlik koruması ile çalışmaya başlama](graph-get-started.md)
