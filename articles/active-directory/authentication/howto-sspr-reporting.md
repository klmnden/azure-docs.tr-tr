---
title: Raporlar - Azure Active Directory Self Servis parola sıfırlama
description: Raporlama Azure AD Self Servis parola sıfırlama olayları
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry
ms.openlocfilehash: 91ed8a073dd95ddf37e4a71bfd7c3ab1dcb94f09
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161370"
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Azure AD parola yönetimi için raporlama seçenekleri

Dağıtımdan sonra birçok kuruluşun öğrenmek istediğiniz nasıl ya da (SSPR) Self Servis parola sıfırlama, gerçekten kullanılıyor. Azure Active Directory (Azure AD) sağladığı raporlama özelliği önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur. Uygun şekilde lisansınız varsa, özel sorgular oluşturabilirsiniz.

![Raporlama][Reporting]

[Azure portalı] (https://portal.azure.com/):

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

Power BI kullanıcısıysanız, yok bir içerik paketi SSPR için kullanımı kolay raporlama içeren Azure AD için. İçerik paketini dağıtmak ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [Azure Active Directory Power BI içerik Paketi'ni kullanma](../active-directory-reporting-power-bi-content-pack-how-to.md). İçerik Paketi ile kendi panolarınızı oluşturun ve bunları başkalarıyla kuruluşunuzda paylaşın.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Azure portalında parola yönetim raporlarını görüntüleme

Azure portalı deneyiminde, parola sıfırlama görüntüleyebilir ve parola sıfırlama kayıt etkinlik şekilde geliştirildi. Sıfırlama adımları parolayı bulmak için aşağıdaki bilgileri kullanın ve parola sıfırlama kayıt olaylarını:

1. Gözat [Azure portalında](https://portal.azure.com).
2. Seçin **tüm hizmetleri** sol bölmesinde.
3. Arama **Azure Active Directory** Hizmetler listesinde ve bu seçeneği belirleyin.
4. **Kullanıcı ve gruplar**'ı seçin.
5. Seçin **denetim günlüklerini** gelen **kullanıcılar ve gruplar** menüsü. Bu, dizininizdeki tüm kullanıcılara karşı gerçekleşen denetim olayların tümünü gösterir. Bu görünüm tüm parola ile ilgili olayları görmek için filtre uygulayabilirsiniz.
6. Bu görünüm yalnızca parola sıfırlama ilgili olayları görmek için filtre uygulamak için seçin **filtre** bölmenin üstünde düğme.
7. Gelen **filtre** menüsünde **kategori** aşağı açılan liste ve şekilde değiştirin **Self Servis parola yönetimi** kategori türü.
8. İsteğe bağlı olarak, daha fazla listenin belirli seçerek filtrelemek **etkinlik** ilgilendiğiniz.

## <a name="description-of-the-report-columns-in-the-azure-portal"></a>Azure Portalı'nda rapor sütunlarında açıklaması

Aşağıdaki listede, her rapor sütunlarında ayrıntılı Azure portalında açıklanmaktadır:

* **Kullanıcı**: bir parola deneyen kullanıcıya kayıt işlemi sıfırlayın.
* **Rol**: dizindeki kullanıcı rolü.
* **Tarih ve saat**: tarih ve saat girişimi.
* **Veri kayıtlı**: kullanıcı tarafından sağlanan sırasında parola sıfırlama kaydı kimlik doğrulama verileri.

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Azure Portalı'nda rapor değerlerin açıklaması

Aşağıdaki tabloda, Azure portalında her sütun için ayarlayabileceğiniz farklı değerler açıklanmaktadır:

| Sütun | İzin verilen değerler ve bunların anlamları |
| --- | --- |
| Kayıtlı veri |**Alternatif e-posta**: kullanıcının kimliğini doğrulamak için bir alternatif e-posta veya kimlik doğrulama e-posta kullanılır.<p><p>**Ofis telefonu**: kullanıcının kimliğini doğrulamak için ofis telefonu kullanılan.<p>**Cep telefonu**: kullanıcının kimliğini doğrulamak için cep telefonu veya kimlik doğrulama telefonu kullanılan.<p>**Güvenlik sorularını**: kullanıcı kimlik doğrulaması için güvenlik sorularını kullanılır.<p>**Önceki yöntemlerden herhangi bir birleşimini gibi alternatif e-posta + cep telefonu**: iki ağ geçidi İlkesi belirtilir ve kullanılan kullanıcı hangi iki yöntemi gösterilir gerçekleşir kimlik doğrulaması parolasını sıfırlama isteği. |

## <a name="self-service-password-management-activity-types"></a>Self Servis parola yönetimi etkinlik türleri

Aşağıdaki etkinlik türlerini görünür **Self Servis parola yönetimi** denetim olay kategorisi:

* [Self Servis parola sıfırlaması engellendi](#activity-type-blocked-from-self-service-password-reset): bir kullanıcı parola sıfırlama, belirli bir ağ geçidi kullanın veya bir telefon numarası doğrulama 24 saat içindeki toplam beşten fazla kez denediğini gösterir.
* [(Self Servis) parolası](#activity-type-change-password-self-service): kullanıcı bir gönüllü gerçekleştirilen veya (süre sonu nedeniyle) zorunlu gösterir parola değiştirme.
* [Parolayı Sıfırla (yönetici tarafından)](#activity-type-reset-password-by-admin): yönetici parola sıfırlama Azure portalından bir kullanıcı adına gerçekleştirilen gösterir.
* [Parola sıfırlama (Self Servis)](#activity-type-reset-password-self-service): Kullanıcı başarıyla parolalarını sıfırlama gösterir [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com).
* [Self Servis parola sıfırlama akış etkinliği ilerleme durumu](#activity-type-self-serve-password-reset-flow-activity-progress): sıfırlama işleminin parola parçası olarak kimlik doğrulama kapısı geçirerek belirli bir parola sıfırlama gibi bir kullanıcı devam eder, her bir adımı gösterir.
* [(Self Servis) kullanıcı hesabının kilidi](#activity-type-unlock-user-account-self-service): bir kullanıcı başarıyla Active Directory hesabı parolalarını sıfırlamadan kilidi olduğunu gösteren [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com) kullanarak etkin Dizin özelliğini hesabının sıfırlamadan kilidi.
* [Kullanıcı Self Servis parola sıfırlama için kayıtlı](#activity-type-user-registered-for-self-service-password-reset): bir kullanıcı şu anda belirtilen kiracıyı parola sıfırlama ilkesine uygun olarak kullanıcının parolasını sıfırlamak için gerekli tüm bilgileri kaydettiğini gösterir.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Etkinlik türü: Self Servis parola sıfırlaması engellendi

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: bir kullanıcı parola sıfırlama, belirli bir ağ geçidi kullanın veya bir telefon numarası doğrulama 24 saat içindeki toplam beşten fazla kez denediğini gösterir.
* **Etkinlik aktör**: ek gerçekleştirmesini kısıtladı kullanıcı sıfırlama işlemlerinin. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: ek gerçekleştirmesini kısıtladı kullanıcı sıfırlama işlemlerinin. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumu**:
  * _Başarı_: bir kullanıcı herhangi bir ek sıfırlar gerçekleştirme, herhangi bir ek kimlik doğrulama yöntemleri çalışırken ya da sonraki 24 saat için herhangi bir ek telefon numaraları doğrulanıyor kısıtladı gösterir.
* **Etkinlik durumu hata nedeni**: uygulanamaz.

### <a name="activity-type-change-password-self-service"></a>Etkinlik türü: parola değiştirme (Self Servis)

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: kullanıcı bir gönüllü gerçekleştirilen veya (süre sonu nedeniyle) zorunlu gösterir parola değiştirme.
* **Etkinlik aktör**: parolalarını değiştiren kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: parolalarını değiştiren kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: kullanıcı parolasını başarıyla değiştirildiğini gösterir.
  * _Hata_: kullanıcı parolasını değiştirmek başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu hata nedeni**: 
  * _FuzzyPolicyViolationInvalidPassword_: seçilen kullanıcı olmadığından Microsoft yasaklanmış parola algılama özellikleri çok ortak veya özellikle zayıf olması için otomatik olarak yasaklanmış bir parola.

### <a name="activity-type-reset-password-by-admin"></a>Etkinlik türü: Parolayı Sıfırla (yönetici tarafından)

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: yönetici parola sıfırlama Azure portalından bir kullanıcı adına gerçekleştirilen gösterir.
* **Etkinlik aktör**: başka bir son kullanıcı veya yönetici adına sıfırlama parola gerçekleştirilen yönetici. Ya da bir genel yönetici, parola Yöneticisi, kullanıcı yöneticinize veya Yardım Masası Yöneticisi olması gerekir.
* **Etkinlik hedef**: kullanıcı, parola sıfırlandı. Kullanıcı, bir son kullanıcı veya farklı bir yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: bir yönetici bir kullanıcının parola başarıyla sıfırlandı gösterir.
  * _Hata_: bir yönetici bir kullanıcının parolasını değiştirmek başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.

### <a name="activity-type-reset-password-self-service"></a>Etkinlik türü: parola sıfırlama (Self Servis)

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: Kullanıcı başarıyla parolalarını sıfırlama gösterir [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com).
* **Etkinlik aktör**: kullanıcı parolasını sıfırlama. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: kullanıcı parolasını sıfırlama. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: Kullanıcı başarıyla kendi parolalarını sıfırlama gösterir.
  * _Hata_: bir kullanıcı kendi parolasını sıfırlamak başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu hata nedeni**: 
  * _FuzzyPolicyViolationInvalidPassword_: yönetici olmadığından Microsoft yasaklanmış parola algılama özellikleri çok ortak veya özellikle zayıf olması için otomatik olarak yasaklanmış bir parola seçili.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Etkinlik türü: Self Servis parola sıfırlama akış etkinliği ilerleme durumu

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: parçası parola sıfırlama işleminin gibi belirli bir kullanıcı geçer aracılığıyla (belirli bir parola geçirme kimlik doğrulama ağ geçidi sıfırlama gibi) her bir adım gösterir.
* **Etkinlik aktör**: parola parçası gerçekleştiren kullanıcının akış sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: parola parçası gerçekleştiren kullanıcının akış sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: bir kullanıcı parola sıfırlama işlem akışında belirli bir adım başarıyla tamamlandığını gösterir.
  * _Hata_: parola belirli bir adım başarısız akış sıfırlama gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu nedeniyle**: aşağıdaki tabloya bakın [tüm izin verilen sıfırlama etkinlik durumu nedeniyle](#allowed-values-for-details-column).

### <a name="activity-type-unlock-a-user-account-self-service"></a>Etkinlik türü: (Self Servis) bir kullanıcı hesabının kilidini açma

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: bir kullanıcı başarıyla Active Directory hesabı parolalarını sıfırlamadan kilidi olduğunu gösteren [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com) kullanarak Active Directory özelliğidir hesabının sıfırlamadan kilidi.
* **Etkinlik aktör**: kendi hesap parolalarını sıfırlamadan kilidi kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: kendi hesap parolalarını sıfırlamadan kilidi kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları izin**:
  * _Başarı_: bir kullanıcı, kendi hesabı başarıyla kilidi gösterir.
  * _Hata_: bir kullanıcı hesaplarının kilidini başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Etkinlik türü: kullanıcı Self Servis parola sıfırlama için kayıtlı

Aşağıdaki listede, bu etkinliğin ayrıntılı açıklanmıştır:

* **Etkinlik açıklaması**: bir kullanıcı şu anda belirtilen kiracıyı parola sıfırlama ilkesine uygun olarak kullanıcının parolasını sıfırlamak için gerekli tüm bilgileri kaydettiğini gösterir. 
* **Etkinlik aktör**: parola sıfırlama için kaydolan kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: parola sıfırlama için kaydolan kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları izin**:
  * _Başarı_: bir kullanıcı başarıyla parola sıfırlamaya uygun olarak geçerli ilke kayıtlı olduğunu belirtir. 
  * _Hata_: bir kullanıcı parola sıfırlama için kaydolmasını başarısız olduğunu gösterir. Satır görmek için seçebileceğiniz **etkinlik durum nedeni** hatanın neden oluştuğunun hakkında daha fazla bilgi için kategori. 

     >[!NOTE]
     >Hata, bir kullanıcı kendi parolanızı sıfırlayamıyoruz anlamına gelmez. Bu, kayıt işlemini tamamlamadı anlamına gelir. Bu telefon numarası doğruladıkları değil olsa da, doğrulanmış değil bir telefon numarası gibi doğru olduğundan, hesapta doğrulanmamış verileri varsa, bunlar bu kullanıcının parolasını sıfırlamak için kullanmaya devam edebilirsiniz. Daha fazla bilgi için [bir kullanıcı kayıt olurkenki ne olur?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers).
     >

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](howto-sspr-deployment.md)
* [Parolanızı sıfırlama veya değiştirme](../user-help/active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](../user-help/active-directory-passwords-reset-register.md).
* [Lisansla ilgili bir sorunuz mu var?](concept-sspr-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](howto-sspr-authenticationdata.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](concept-sspr-howitworks.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](concept-sspr-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](howto-sspr-writeback.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](concept-sspr-howitworks.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Reporting]: ./media/howto-sspr-reporting/sspr-reporting.png "Azure AD'de örneği SSPR etkinlik denetim günlükleri"
