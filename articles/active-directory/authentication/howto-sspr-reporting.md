---
title: Raporlar - Azure Active Directory Self Servis parola sıfırlama
description: Raporlama Azure AD Self Servis parola sıfırlama olayları
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 49b247338bbb1f20082fdef2a2bc291fb6183b10
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59493068"
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Azure AD parola yönetimi için raporlama seçenekleri

Dağıtımdan sonra birçok kuruluşun öğrenmek istediğiniz nasıl ya da (SSPR) Self Servis parola sıfırlama, gerçekten kullanılıyor. Azure Active Directory (Azure AD) sağladığı raporlama özelliği önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur. Uygun şekilde lisansınız varsa, özel sorgular oluşturabilirsiniz.

![SSPR üzerinde raporlama denetim kullanarak Azure AD'de oturum][Reporting]

Mevcut raporları aşağıdaki sorular yanıtlanabilir [Azure portalında](https://portal.azure.com/):

> [!NOTE]
> Siz [genel yönetici](../users-groups-roles/directory-assign-admin-roles.md), ve, bu veriler kuruluşunuz adına toplanması için katılım gerekir. Katılım için ziyaret gerekir **raporlama** sekme veya denetim günlükleri en az bir kez. O zamana kadar kuruluşunuz için veri toplanmaz.
>

* Kaç kişinin parola sıfırlama için kaydolan?
* Kimin parola sıfırlama için kaydolan?
* Verileri kaydetme kişiler nelerdir?
* Kaç kişinin parolalarını son yedi gün içinde sıfırlansın mı?
* Kullanıcılar veya yöneticilerin parolalarını sıfırlayabilir kullanan en yaygın yöntemleri nelerdir?
* Ortak sorunları kullanıcılar veya yöneticilerin yüz parola sıfırlamayı kullanabilmek çalışırken nelerdir?
* Hangi yöneticileri sık kendi parolalarını sıfırlama?
* Parola sıfırlama ile geçmeden herhangi bir şüpheli etkinlik var mı?

## <a name="power-bi-content-pack"></a>Power BI İçerik Paketi

Power BI kullanıcısıysanız, yok bir içerik paketi SSPR için kullanımı kolay raporlama içeren Azure AD için. İçerik paketini dağıtmak ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory Power BI içerik Paketi'ni kullanma](../reports-monitoring/howto-power-bi-content-pack.md). İçerik Paketi ile kendi panolarınızı oluşturun ve bunları başkalarıyla kuruluşunuzda paylaşın.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Azure portalında parola yönetim raporlarını görüntüleme

Azure portalı deneyiminde, parola sıfırlama görüntüleyebilir ve parola sıfırlama kayıt etkinlik şekilde geliştirildi. Sıfırlama adımları parolayı bulmak için aşağıdaki bilgileri kullanın ve parola sıfırlama kayıt olaylarını:

1. [Azure portala](https://portal.azure.com) gidin.
2. Seçin **tüm hizmetleri** sol bölmesinde.
3. Arama **Azure Active Directory** Hizmetler listesinde ve bu seçeneği belirleyin.
4. **Kullanıcı ve gruplar**'ı seçin.
5. Seçin **denetim günlüklerini** gelen **kullanıcılar ve gruplar** menüsü. Bu, dizininizdeki tüm kullanıcılara karşı gerçekleşen denetim olayların tümünü gösterir. Bu görünüm tüm parola ile ilgili olayları görmek için filtre uygulayabilirsiniz.
6. Bu görünüm yalnızca parola sıfırlama ilgili olayları görmek için filtre uygulamak için seçin **filtre** bölmenin üstünde düğme.
7. Gelen **filtre** menüsünde **kategori** aşağı açılan liste ve şekilde değiştirin **Self Servis parola yönetimi** kategori türü.
8. İsteğe bağlı olarak, daha fazla listenin belirli seçerek filtrelemek **etkinlik** ilgilendiğiniz.

### <a name="converged-registration-preview"></a>Yakınsanmış kayıt (Önizleme)

Yakınsanmış kayıt genel önizlemede katılan, Denetim günlüklerinde kullanıcı etkinliği ile ilgili bilgi kategorisi altında bulunacaktır **kimlik doğrulama yöntemleri**.

## <a name="description-of-the-report-columns-in-the-azure-portal"></a>Azure Portalı'nda rapor sütunlarında açıklaması

Aşağıdaki listede, her rapor sütunlarında ayrıntılı Azure portalında açıklanmaktadır:

* **Kullanıcı**: Bir parola deneyen kullanıcıya kayıt işlemi sıfırlayın.
* **Rol**: Dizindeki kullanıcı rolü.
* **Tarih ve saat**: Tarih ve saat girişimi.
* **Kayıtlı veri**: Kullanıcı tarafından sağlanan sırasında parola sıfırlama kaydı, kimlik doğrulama verileri.

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Azure Portalı'nda rapor değerlerin açıklaması

Aşağıdaki tabloda, Azure portalında her sütun için ayarlayabileceğiniz farklı değerler açıklanmaktadır:

| Sütun | İzin verilen değerler ve bunların anlamları |
| --- | --- |
| Kayıtlı veri |**Alternatif e-posta**: Kullanıcı kimlik doğrulaması için bir alternatif e-posta veya kimlik doğrulama e-posta kullanılır.<p><p>**Ofis telefonu**: Kullanıcı bir ofis telefonu kimliğini doğrulamak için kullanılır.<p>**Cep telefonu**: Kullanıcı, bir cep telefonu veya kimlik doğrulama telefon numarasını kimlik doğrulaması için kullanılır.<p>**Güvenlik sorularını**: Kullanıcının güvenlik sorularını kimliğini doğrulamak için kullanılır.<p>**Önceki yöntemlerden herhangi bir birleşimini gibi alternatif e-posta + cep telefonu**: İki ağ geçidi İlkesi belirtilir ve kullanılan kullanıcı hangi iki yöntemi gösterilir gerçekleşir kimlik doğrulaması parolasını sıfırlama isteği. |

## <a name="self-service-password-management-activity-types"></a>Self Servis parola yönetimi etkinlik türleri

Aşağıdaki etkinlik türlerini görünür **Self Servis parola yönetimi** denetim olay kategorisi:

* [Self Servis parola sıfırlaması engellendi](#activity-type-blocked-from-self-service-password-reset): Kullanıcı parola sıfırlama, belirli bir ağ geçidi kullanın veya bir telefon numarası doğrulama 24 saat içindeki toplam beşten fazla kez denediğini gösterir.
* [Parolayı değiştirme (Self Servis)](#activity-type-change-password-self-service): Bir kullanıcı bir gönüllü gerçekleştirilen veya (süre sonu nedeniyle) zorunlu gösterir parola değiştirme.
* [Parolayı Sıfırla (yönetici tarafından)](#activity-type-reset-password-by-admin): Yönetici parola sıfırlama Azure portalından bir kullanıcı adına gerçekleştirilen gösterir.
* [Parola sıfırlama (Self Servis)](#activity-type-reset-password-self-service): Kullanıcı başarıyla parolalarını sıfırlama gösterir [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com).
* [Self Servis parola sıfırlama akış etkinliği ilerleme durumu](#activity-type-self-serve-password-reset-flow-activity-progress): Sıfırlama işleminin parola parçası olarak kimlik doğrulama kapısı geçirerek belirli bir parola sıfırlama gibi bir kullanıcı geçer her belirli bir adıma gösterir.
* [(Self Servis) kullanıcı hesabının kilidi](#activity-type-unlock-a-user-account-self-service)): Kullanıcı başarıyla Active Directory hesabı parolalarını sıfırlamadan kilidi olduğunu gösteren [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com) sıfırlamadan kilidini açma hesabının Active Directory özelliğini kullanarak.
* [Kullanıcı Self Servis parola sıfırlama için kayıtlı](#activity-type-user-registered-for-self-service-password-reset): Kullanıcı şu anda belirtilen kiracıyı parola sıfırlama ilkesine uygun olarak kullanıcının parolasını sıfırlamak için gerekli tüm bilgileri kaydettiğini gösterir.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Etkinlik türü: Self servis parola sıfırlaması engellendi

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Kullanıcı parola sıfırlama, belirli bir ağ geçidi kullanın veya bir telefon numarası doğrulama 24 saat içindeki toplam beşten fazla kez denediğini gösterir.
* **Etkinlik aktör**: Ek gerçekleştirmesini kısıtladı kullanıcı işlemleri sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: Ek gerçekleştirmesini kısıtladı kullanıcı işlemleri sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumu**:
  * _Success_: Bir kullanıcı herhangi bir ek sıfırlar gerçekleştirme, herhangi bir ek kimlik doğrulama yöntemleri çalışırken ya da sonraki 24 saat için herhangi bir ek telefon numaraları doğrulanıyor kısıtladı gösterir.
* **Etkinlik durumu hata nedeni**: Geçerli değildir.

### <a name="activity-type-change-password-self-service"></a>Etkinlik türü: Parolayı değiştirme (self servis)

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Bir kullanıcı bir gönüllü gerçekleştirilen veya (süre sonu nedeniyle) zorunlu gösterir parola değiştirme.
* **Etkinlik aktör**: Kullanıcıların parolalarını değiştiren kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: Kullanıcıların parolalarını değiştiren kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Success_: Bir kullanıcının parolasını başarıyla değiştirildi gösterir.
  * _Hata_: Bir kullanıcının parolasını değiştirmek başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu hata nedeni**:
  * _FuzzyPolicyViolationInvalidPassword_: Seçilen kullanıcı olmadığından Microsoft yasaklanmış parola algılama özellikleri çok ortak veya özellikle zayıf olması için otomatik olarak yasaklanmış bir parola.

### <a name="activity-type-reset-password-by-admin"></a>Etkinlik türü: Parola sıfırlama (yönetici tarafından)

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Yönetici parola sıfırlama Azure portalından bir kullanıcı adına gerçekleştirilen gösterir.
* **Etkinlik aktör**: Parola başka bir son kullanıcı veya yönetici adına sıfırlama gerçekleştirilen yönetici. Parola Yöneticisi, kullanıcı yöneticinize veya Yardım Masası Yöneticisi olması gerekir.
* **Etkinlik hedef**: Kullanıcı, parola sıfırlandı. Kullanıcı, bir son kullanıcı veya farklı bir yönetici olabilir.
* **Etkinlik durumları**:
  * _Success_: Bir yönetici bir kullanıcının parola başarıyla sıfırlandı gösterir.
  * _Hata_: Bir yönetici bir kullanıcının parolasını değiştirmek başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.

### <a name="activity-type-reset-password-self-service"></a>Etkinlik türü: Parola sıfırlama (self servis)

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Kullanıcı başarıyla parolalarını sıfırlama gösterir [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com).
* **Etkinlik aktör**: Kullanıcının parolasını sıfırlamasını kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: Kullanıcının parolasını sıfırlamasını kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Success_: Kullanıcı başarıyla kendi parolalarını sıfırlama gösterir.
  * _Hata_: Bir kullanıcı kendi parolasını sıfırlamak başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu hata nedeni**:
  * _FuzzyPolicyViolationInvalidPassword_: Yönetici olmadığından Microsoft yasaklanmış parola algılama özellikleri çok ortak veya özellikle zayıf olması için otomatik olarak yasaklanmış bir parola seçilir.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Etkinlik türü: Kendi kendine hizmet parola sıfırlama akış etkinliği ilerleme durumu

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Bölümü parola sıfırlama işleminin (belirli bir parola geçirme kimlik doğrulama ağ geçidi sıfırlama gibi), bir kullanıcı üzerinden geçer her belirli bir adıma belirtir.
* **Etkinlik aktör**: Parola parçası gerçekleştiren kullanıcının akış sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: Parola parçası gerçekleştiren kullanıcının akış sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Success_: Kullanıcı parola sıfırlama işlem akışında belirli bir adım başarıyla tamamlandığını gösterir.
  * _Hata_: Parola belirli bir adım başarısız akış sıfırlama gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu nedeniyle**:   İçin aşağıdaki tabloya bakın [tüm izin verilen sıfırlama etkinlik durumu nedeniyle](#description-of-the-report-columns-in-the-azure-portal).

### <a name="activity-type-unlock-a-user-account-self-service"></a>Etkinlik türü: (Self Servis) bir kullanıcı hesabının kilidini açma

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Kullanıcı başarıyla Active Directory hesabı parolalarını sıfırlamadan kilidi olduğunu gösteren [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com) sıfırlamadan kilidini açma hesabının Active Directory özelliğini kullanarak.
* **Etkinlik aktör**: Kendi hesap parolalarını sıfırlamadan kilidi kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: Kendi hesap parolalarını sıfırlamadan kilidi kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları izin**:
  * _Success_: Bir kullanıcı, kendi hesabı başarıyla kilidi gösterir.
  * _Hata_: Bir kullanıcı hesaplarının kilidini başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Etkinlik türü: Self servis parola sıfırlama için kaydolan kullanıcı

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Kullanıcı şu anda belirtilen kiracıyı parola sıfırlama ilkesine uygun olarak kullanıcının parolasını sıfırlamak için gerekli tüm bilgileri kaydettiğini gösterir. 
* **Etkinlik aktör**: Parola sıfırlama için kaydolan kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: Parola sıfırlama için kaydolan kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları izin**:
  * _Success_: Kullanıcı başarıyla parola sıfırlamaya uygun olarak geçerli ilke kayıtlı olduğunu belirtir. 
  * _Hata_: Kullanıcı parola sıfırlama için kaydolmasını başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.

     >[!NOTE]
     >Hata, bir kullanıcı kendi parolanızı sıfırlayamıyoruz anlamına gelmez. Bu, kayıt işlemini tamamlamadı anlamına gelir. Bu telefon numarası doğruladıkları değil olsa da, doğrulanmış değil bir telefon numarası gibi doğru olduğundan, hesapta doğrulanmamış verileri varsa, bunlar bu kullanıcının parolasını sıfırlamak için kullanmaya devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Sspr'yi başarılı bir sunum nasıl tamamlamak?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md).
* [Lisans bir sorunuz var mı?](concept-sspr-licensing.md)
* [SSPR tarafından kullanılan verileri ve hangi verilerin, kullanıcılarınız için doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kimlik doğrulama yöntemleri, kullanıcılara kullanılabilir mi?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [Tüm SSPR seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR nasıl giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele değil bir sorum var](active-directory-passwords-faq.md)

[Reporting]: ./media/howto-sspr-reporting/sspr-reporting.png "Azure AD'de örneği SSPR etkinlik denetim günlükleri"
