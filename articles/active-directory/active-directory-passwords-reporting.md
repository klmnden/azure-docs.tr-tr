---
title: 'Raporlama: Azure AD SSPR''yi | Microsoft Docs'
description: "Azure AD Self Servis parola üzerinde raporlama olayları Sıfırla"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: sahenry
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 8599b843bb1d5692c15f9344d0c46940b7cd5a81
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="reporting-options-for-azure-ad-password-management"></a>Azure AD parola yönetimi için raporlama seçenekleri

Dağıtımdan sonra birçok kuruluş bilmek ister nasıl ya da (SSPR) Self Servis parola sıfırlama, gerçekten kullanılıyor. Azure Active Directory (Azure AD) sağlar raporlama özelliğini önceden oluşturulmuş raporları kullanarak soruları yanıtlamanıza yardımcı olur. Uygun şekilde lisansına sahip, ayrıca özel sorgular oluşturabilirsiniz.

![Raporlama][Reporting]

[Azure portalı] mevcut raporlar aşağıdaki soruları yanıtlanabilir (https://portal.azure.com/):

> [!NOTE]
> Olmalıdır [genel yönetici](active-directory-assign-admin-roles-azure-portal.md), ve, kuruluşunuz adına toplanması bu veri katılımı gerekir. Kabul için ziyaret gerekir **raporlama** sekme veya denetim günlüklerini en az bir kez. O zamana kadar kuruluşunuz için verileri toplanmaz.
>

* Kaç kişinin parola sıfırlama için kayıtlı?
* Parola sıfırlama için kayıtlı olan kim?
* Verileri kaydetme kişiler nelerdir?
* Kaç kişinin son yedi gün içinde parolalarını sıfırlama?
* Kullanıcıların parolalarını sıfırlamak için kullanıcıları veya yöneticileri kullanmak en yaygın yöntemleri nelerdir?
* Sık karşılaşılan sorunları kullanıcıları veya yöneticileri yüz parola sıfırlama kullanmaya çalışırken nelerdir?
* Hangi yöneticileri kendi parolalarını sık sıfırlama?
* Parola sıfırlama ile geçmeden tüm şüpheli etkinlik mi?

## <a name="power-bi-content-pack"></a>Power BI İçerik Paketi

Power BI kullanıcıysanız var. bir içerik paketi SSPR için kullanımı kolay raporlama içeren Azure AD için İçerik Paketi dağıtmak ve nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [Azure Active Directory Power BI içerik paketi kullanmayı](active-directory-reporting-power-bi-content-pack-how-to.md). İçerik paketiyle kendi panolar oluşturun ve kuruluşunuzda başkalarıyla paylaşabilirsiniz.

## <a name="how-to-view-password-management-reports-in-the-azure-portal"></a>Azure portalında parola yönetimi raporları görüntüleme

Azure portal deneyimi biz parola sıfırlama görüntüleyebilir ve parola sıfırlama kaydı etkinliği şekilde geliştirilmiştir. Parola bulmak için aşağıdaki adımları sıfırlama aşağıdaki bilgileri kullanın ve parola sıfırlama kayıt olayları:

1. Gözat [Azure portal](https://portal.azure.com).
2. Seçin **tüm hizmetleri** sol bölmede.
3. Arama **Azure Active Directory** Hizmetler listesinde ve seçin.
4. Seçin **kullanıcılar ve gruplar**.
5. Seçin **denetim günlüklerini** gelen **kullanıcılar ve gruplar** menüsü. Bu, tüm dizininizdeki tüm kullanıcılara karşı gerçekleşen denetim olayları gösterir. Bu görünüm tüm parola ile ilgili olayları görmek için filtre uygulayabilirsiniz.
6. Bu görünüm yalnızca parola sıfırlama ilgili olayları görmek için filtre uygulamak için seçim **filtre** bölmenin üstündeki düğmesi.
7. Gelen **filtre** menüsünde, select **kategori** aşağı açılan listesinde ve şekilde değiştirin **Self Servis parola yönetimi** kategori türü.
8. İsteğe bağlı olarak, daha fazla listesi belirli seçerek filtrelemek **etkinlik** ilgilendiğiniz.

## <a name="how-to-retrieve-password-management-events-from-the-azure-ad-reports-and-events-api"></a>Parola yönetimi olayları Azure AD raporları ve olayları API alma

Parola sıfırlama kaydı raporları ve Azure AD raporları ve olayları API parola sıfırlama dahil tüm bilgileri alma destekler. Bu API kullanarak, tek tek parola sıfırlama ve parola sıfırlama kayıt olayları indirin ve tercih ettiğiniz raporlama teknolojisi ile tümleştirme.

### <a name="how-to-get-started-with-the-reporting-api"></a>Nasıl raporlama API'si ile çalışmaya başlama

Bu verilere erişmek için bir küçük uygulama veya betik bizim sunuculardan verileri almak için yazmanız gerekir. Daha fazla bilgi için bkz: [Azure AD raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).

Bir çalışma betiği çalıştırdıktan sonra senaryolarınızı karşılayacak şekilde alabilirsiniz parola sıfırlama ve kayıt olayları inceleyin isteyeceksiniz:

* [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent): olayları parola sıfırlama için kullanılabilen sütunları listeler.
* [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent): kayıt olayları parola sıfırlama için kullanılabilen sütunları listeler.

### <a name="reporting-api-data-retrieval-limitations"></a>Raporlama API veri alma sınırlamaları

Şu anda Azure AD raporları ve olayları API alır kadar *75,000 olayları tek tek* , [SsprActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent) ve [SsprRegistrationActivityEvent](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprRegistrationActivityEvent) türleri. API yayılma *son 30 gün*.

Almak veya bu pencereyi aşan veri depolamak gerekiyorsa, sonuç farkları sorgulamak için API'yi kullanarak dış bir veritabanında kalıcı öneririz. Kuruluşunuzda SSPR kullanmaya başladığınızda, bu verileri almak üzere başlamanızı öneriyoruz. Harici olarak kalır ve ardından o noktadan itibaren farkları izlemek devam edin.

## <a name="description-of-the-report-columns-in-the-azure-portal"></a>Azure portalında rapor sütunlarını açıklaması

Aşağıdaki listede ayrıntılı Azure portalında rapor sütunların her biri açıklanmaktadır:

* **Kullanıcı**: parola deneyen kullanıcıya kayıt işlemi sıfırlayın.
* **Rol**: dizindeki kullanıcı rolü.
* **Tarih ve saat**: tarih ve saat girişimi.
* **Veri kayıtlı**: sıfırlama kaydı sırasında parola sağlanan kullanıcı kimlik doğrulama verileri.

## <a name="description-of-the-report-values-in-the-azure-portal"></a>Azure portalında rapor değerlerinin açıklaması

Aşağıdaki tabloda, Azure portalında her sütun için ayarlayabileceğiniz farklı değerleri açıklanmaktadır:

| Sütun | İzin verilen değerler ve anlamları |
| --- | --- |
| Kayıtlı veri |**Alternatif e-posta**: kullanıcının kimliğini doğrulamak için bir alternatif e-posta veya kimlik doğrulama e-posta kullanılır.<p><p>**Ofis telefonu**: kullanıcının kimliğini doğrulamak için bir ofis telefonu kullanılan.<p>**Cep telefonu**: kullanıcının kimliğini doğrulamak için cep telefonu veya kimlik doğrulama telefon kullanılan.<p>**Güvenlik soruları**: kullanıcının kimliğini doğrulamak için güvenlik soruları kullanılan.<p>**Herhangi bir bileşimini önceki yöntemler, örneğin, alternatif e-posta + cep telefonu**: iki ağ geçidi ilke belirtilir ve kullanılan kullanıcı hangi iki yöntemi gösterilir oluşur kimlik doğrulama parolasını sıfırlama isteği. |

## <a name="self-service-password-management-activity-types"></a>Self Servis parola yönetimi etkinlik türleri

Aşağıdaki etkinlik türlerini görünür **Self Servis parola yönetimi** denetim olay kategorisi:

* [Self Servis parola sıfırlama engellenen](#activity-type-blocked-from-self-service-password-reset): bir kullanıcı bir parola sıfırlama, belirli bir ağ geçidi kullanın veya bir telefon numarası 24 saat içindeki toplam beşten fazla kez doğrulanacak çalıştığını gösterir.
* [Parola (Self Servis) değiştirme](#activity-type-change-password-self-service): bir kullanıcı bir gönüllü gerçekleştirilen veya (Bitiş nedeniyle) zorunlu gösterir parola değiştirme.
* [Parola sıfırlama (yönetici tarafından)](#activity-type-reset-password-by-admin): yönetici parola Azure portalından bir kullanıcı adına sıfırlama gerçekleştirilen gösterir.
* [Parola sıfırlama (Self Servis)](#activity-type-reset-password-self-service): Kullanıcı başarıyla parolalarını sıfırlama gösterir [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com).
* [Self Servis parola sıfırlama akış etkinliği ilerleme](#activity-type-self-serve-password-reset-flow-activity-progress): sıfırlama işlemi parola parçası olarak kimlik doğrulama geçidi olan belirli bir parola geçirme sıfırlama gibi bir kullanıcı işlemi devam eder, her belirli adım gösterir.
* [Kullanıcı hesabının (Self Servis) kilidini](#activity-type-unlock-user-account-self-service): Kullanıcı başarıyla Active Directory hesabı parolalarını sıfırlamadan kilidi olduğunu gösteren [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com) etkin kullanarak Dizini özelliği hesabın kilidini Sıfırla.
* [Kullanıcı Self Servis parola sıfırlama için kayıtlı](#activity-type-user-registered-for-self-service-password-reset): bir kullanıcı parolasını şu anda belirtilen Kiracı parolası sıfırlama ilkesini uygun olarak sıfırlamayı kullanabilmek için gerekli tüm bilgileri kaydettiğini gösterir.

### <a name="activity-type-blocked-from-self-service-password-reset"></a>Etkinlik türü: Self Servis parola sıfırlama engellendi

Aşağıdaki listede bu etkinliği ayrıntılı açıklanmıştır:

* **Etkinlik açıklama**: bir kullanıcı bir parola sıfırlama, belirli bir ağ geçidi kullanın veya bir telefon numarası 24 saat içindeki toplam beşten fazla kez doğrulanacak çalıştığını gösterir.
* **Etkinlik aktör**: ek gerçekleştirmesini kısıtlanan kullanıcı sıfırlama işlemlerinin. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: ek gerçekleştirmesini kısıtlanan kullanıcı sıfırlama işlemlerinin. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumu**:
  * _Başarı_: bir kullanıcı herhangi bir ek sıfırlar gerçekleştirme, ek kimlik doğrulama yöntemleri çalışırken ya da herhangi bir ek telefon numaraları için sonraki 24 saat doğrulama kısıtlanan gösterir.
* **Etkinlik durumu hata nedeni**: geçerli değil.

### <a name="activity-type-change-password-self-service"></a>Etkinlik türü: parola değiştirme (Self Servis)

Aşağıdaki listede bu etkinliği ayrıntılı açıklanmıştır:

* **Etkinlik açıklama**: bir kullanıcı bir gönüllü gerçekleştirilen veya (Bitiş nedeniyle) zorunlu gösterir parola değiştirme.
* **Etkinlik aktör**: parolalarını değiştiren kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: parolalarını değiştiren kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: bir kullanıcı parolasını başarıyla değiştirildiğini gösterir.
  * _Hata_: bir kullanıcı parolasını değiştirmek başarısız olduğunu gösterir. Görmek için satır seçebilirsiniz **etkinlik durum nedeni** neden oluştuğuna hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu hata nedeni**: 
  * _FuzzyPolicyViolationInvalidPassword_: Microsoft yasaklanan parola algılama özellikleri çok sık kullanılan veya özellikle zayıf olmasını bulduğundan otomatik olarak yasaklanan bir parola seçilen kullanıcı.

### <a name="activity-type-reset-password-by-admin"></a>Etkinlik türü: (yönetici tarafından) parola sıfırlama

Aşağıdaki listede bu etkinliği ayrıntılı açıklanmıştır:

* **Etkinlik açıklama**: yönetici parola Azure portalından bir kullanıcı adına sıfırlama gerçekleştirilen gösterir.
* **Etkinlik aktör**: parola başka bir son kullanıcı veya yönetici adına sıfırlama gerçekleştiren yönetici. Ya da bir genel yönetici, parola yönetici, kullanıcı veya Yardım Masası yönetici olması gerekir.
* **Etkinlik hedef**: kullanıcının parolasını sıfırlandı. Kullanıcı, bir son kullanıcı veya farklı bir yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: bir yönetici kullanıcının parolası başarıyla sıfırlandı gösterir.
  * _Hata_: bir yönetici bir kullanıcının parolasını değiştirmek başarısız olduğunu gösterir. Görmek için satır seçebilirsiniz **etkinlik durum nedeni** neden oluştuğuna hakkında daha fazla bilgi için kategori.

### <a name="activity-type-reset-password-self-service"></a>Etkinlik türü: parola sıfırlama (Self Servis)

Aşağıdaki listede bu etkinliği ayrıntılı açıklanmıştır:

* **Etkinlik açıklama**: Kullanıcı başarıyla parolalarını sıfırlama gösterir [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com).
* **Etkinlik aktör**: parolalarını sıfırlama kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: parolalarını sıfırlama kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: bir kullanıcı, kendi parola başarıyla sıfırlandı gösterir.
  * _Hata_: bir kullanıcı kendi parolasını sıfırlamak başarısız olduğunu gösterir. Görmek için satır seçebilirsiniz **etkinlik durum nedeni** neden oluştuğuna hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu hata nedeni**: 
  * _FuzzyPolicyViolationInvalidPassword_: Microsoft yasaklanan parola algılama özellikleri çok sık kullanılan veya özellikle zayıf olmasını bulduğundan otomatik olarak yasaklanan bir parola yönetici seçili.

### <a name="activity-type-self-serve-password-reset-flow-activity-progress"></a>Etkinlik türü: kendi kendine parola sıfırlama akış etkinliği ilerleme hizmet

Aşağıdaki listede bu etkinliği ayrıntılı açıklanmıştır:

* **Etkinlik açıklama**: sıfırlama işlemi parola parçası olarak bir kullanıcı geçer aracılığıyla (belirli bir parola geçirme kimlik doğrulama geçidi sıfırlama gibi) her belirli adım gösterir.
* **Etkinlik aktör**: parola parçası gerçekleştiren kullanıcı Akış sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: parola parçası gerçekleştiren kullanıcı Akış sıfırlayın. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları**:
  * _Başarı_: bir kullanıcı parola sıfırlama akışının belirli bir adıma başarıyla tamamlandığını gösterir.
  * _Hata_: belirli bir adıma parola başarısız akış sıfırlama gösterir. Görmek için satır seçebilirsiniz **etkinlik durum nedeni** neden oluştuğuna hakkında daha fazla bilgi için kategori.
* **Etkinlik durumu nedeniyle**: için aşağıdaki tabloya bakın [tüm izin verilen sıfırlama etkinlik durumu nedeniyle](#allowed-values-for-details-column).

### <a name="activity-type-unlock-a-user-account-self-service"></a>Etkinlik türü: bir kullanıcı hesabı (Self Servis) kilidini aç

Aşağıdaki listede bu etkinliği ayrıntılı açıklanmıştır:

* **Etkinlik açıklama**: Kullanıcı başarıyla Active Directory hesabı parolalarını sıfırlamadan kilidi olduğunu gösteren [Azure AD parola sıfırlama portalı](https://passwordreset.microsoftonline.com) ' ın Active Directory özelliğini kullanarak hesabının kilidini Sıfırla.
* **Etkinlik aktör**: kendi hesap parolalarını sıfırlamadan kilidi kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: kendi hesap parolalarını sıfırlamadan kilidi kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları izin**:
  * _Başarı_: bir kullanıcı, kendi hesabı başarıyla kilidi gösterir.
  * _Hata_: kullanıcı hesaplarının kilidini başarısız olduğunu gösterir. Görmek için satır seçebilirsiniz **etkinlik durum nedeni** neden oluştuğuna hakkında daha fazla bilgi için kategori.

### <a name="activity-type-user-registered-for-self-service-password-reset"></a>Etkinlik türü: kullanıcı Self Servis parola sıfırlama için kayıtlı

Aşağıdaki listede bu etkinliği ayrıntılı açıklanmıştır:

* **Etkinlik açıklama**: bir kullanıcı parolasını şu anda belirtilen Kiracı parolası sıfırlama ilkesini uygun olarak sıfırlamayı kullanabilmek için gerekli tüm bilgileri kaydettiğini gösterir. 
* **Etkinlik aktör**: parola sıfırlama için kayıtlı olan kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik hedef**: parola sıfırlama için kayıtlı olan kullanıcı. Kullanıcı, bir son kullanıcı veya yönetici olabilir.
* **Etkinlik durumları izin**:
  * _Başarı_: Kullanıcı başarıyla parola sıfırlama geçerli ilkesiyle uygun şekilde kayıtlı olduğunu belirtir. 
  * _Hata_: bir kullanıcının parola sıfırlama için kaydetme başarısız olduğunu gösterir. Görmek için satır seçebilirsiniz **etkinlik durum nedeni** neden oluştuğuna hakkında daha fazla bilgi için kategori. 

     >[!NOTE]
     >Hata, bir kullanıcı kendi parolasını sıfırlamaya anlamına gelmez. Bu, kayıt işlemini bitmedi anlamına gelir. Bunlar bu telefon numarası doğrulanmadı olsa da, doğru doğrulanmaz, bir telefon numarası gibi kendi hesaplarına doğrulanmamış verileri varsa, bunlar, parolasını sıfırlamak için kullanmaya devam edebilirsiniz. Daha fazla bilgi için bkz: [kullanıcı kayıtları ne olur?](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-learn-more#what-happens-when-a-user-registers).
     >

## <a name="next-steps"></a>Sonraki adımlar

* [SSPR’yi başarılı bir şekilde nasıl piyasaya çıkarabilirim?](active-directory-passwords-best-practices.md)
* [Parolanızı sıfırlama veya değiştirme](active-directory-passwords-update-your-own-password.md).
* [Self servis parola sıfırlama için kaydolma](active-directory-passwords-reset-register.md).
* [Lisansla ilgili bir sorunuz mu var?](active-directory-passwords-licensing.md)
* [SSPR hangi verileri kullanır ve kullanıcılarınız için hangi verileri doldurmanız gerekir?](active-directory-passwords-data.md)
* [Kullanıcılar hangi kimlik doğrulama yöntemlerini kullanabilir?](active-directory-passwords-how-it-works.md#authentication-methods)
* [SSPR ile kullanılabilen ilke seçenekleri nelerdir?](active-directory-passwords-policy.md)
* [Parola geri yazma nedir ve neden önemlidir?](active-directory-passwords-writeback.md)
* [SSPR’deki tüm seçenekler nelerdir ve ne anlama gelir?](active-directory-passwords-how-it-works.md)
* [Bir arıza olduğunu düşünüyorum. SSPR’de nasıl sorun giderebilirim?](active-directory-passwords-troubleshoot.md)
* [Başka bir yerde ele alınmayan bir sorum var](active-directory-passwords-faq.md)

[Reporting]: ./media/active-directory-passwords-reporting/sspr-reporting.png "Azure AD'de SSPR etkinlik denetim örneği günlükleri"
