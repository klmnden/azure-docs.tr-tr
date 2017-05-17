---
title: "Azure Active Directory Raporlama: Başlarken | Microsoft Belgeleri"
description: "Azure Active Directory raporlamada kullanılabilen çeşitli raporları listeler"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: eec9b73cbaccfa50eec6f237e4d1d810c6efa1d9
ms.openlocfilehash: e5b8ac91914203156bd395d7f462385e9f6dbcb4
ms.contentlocale: tr-tr
ms.lasthandoff: 02/24/2017


---
# <a name="getting-started-with-azure-active-directory-reporting"></a>Azure Active Directory Raporlama ile çalışmaya başlama
## <a name="what-it-is"></a>Nedir?
Azure Active Directory (Azure AD), dizininize yönelik güvenlik, etkinlik ve denetim raporlarını içerir. Kapsama dahil olan raporların listesi şu şekildedir:

### <a name="security-reports"></a>Güvenlik raporları
* Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri
* Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri
* Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri
* Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri
* Düzensiz oturum açma etkinliği
* Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri
* Anormal oturum açma etkinliği gösteren kullanıcılar

### <a name="activity-reports"></a>Etkinlik raporları
* Uygulama kullanımı: özet
* Uygulama kullanımı: ayrıntılı
* Uygulama panosu
* Hesap hazırlama hataları
* Bireysel kullanıcı cihazları
* Bireysel kullanıcı Etkinliği
* Grup etkinlik raporu
* Parola Sıfırlama Kayıt Etkinlik Raporu
* Parola sıfırlama etkinliği

### <a name="audit-reports"></a>Denetim raporları
* Dizin denetimi raporu

> [!TIP]
> Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.
> 
> 

## <a name="how-it-works"></a>Nasıl çalışır?
### <a name="reporting-pipeline"></a>Raporlama işlem hattı
Raporlama işlem hattı üç ana adımdan oluşur. Bir kullanıcı her oturum açtığında veya kimlik doğrulama işlemi gerçekleştirildiğinde aşağıdakiler gerçekleşir:

* İlk olarak, kullanıcının kimliği doğrulanır (başarıyla veya başarısız şekilde) ve sonuç Azure Active Directory hizmeti veritabanlarında depolanır.
* Düzenli aralıklarla, en yeni oturum açma işlemlerinin tümü işlenir. Bu noktada, güvenlik ve anormal etkinlik algoritmalarımız en yeni oturum açma işlemlerinin tümünde şüpheli etkinlik araması gerçekleştirmektedir.
* İşleme aşamasının ardından raporlar yazılır, önbelleğe alınır ve klasik Azure portalında sunulur.

### <a name="report-generation-times"></a>Rapor oluşturma süreleri
Azure AD platformu tarafından işlenen kimlik doğrulama ve oturum açma işlemlerinin çok fazla olması nedeniyle, işlenen en yeni oturum açma işlemleri ortalama olarak bir saat öncesine aittir. Nadir durumlarda, en yeni oturum açma işlemlerinin işlenmesi 8 saate kadar sürebilir.

En son işlenen oturum açma işlemini, her bir raporun üst kısmındaki yardım metnini inceleyerek bulabilirsiniz.

![Her bir raporun üst kısmındaki yardım metni](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.
> 
> 

## <a name="getting-started"></a>Başlarken
### <a name="sign-into-the-azure-classic-portal"></a>Klasik Azure portalında oturum açma
Öncelikle [klasik Azure portalında](https://manage.windowsazure.com) genel yönetici veya uyumluluk yöneticisi olarak oturum açmanız gerekir. Ayrıca bir Azure aboneliği hizmet yöneticisi veya ortak yöneticisi olmanız veya "Azure AD Erişimi" Azure aboneliğini kullanıyor olmanız gerekir.

### <a name="navigate-to-reports"></a>Raporlara gitme
Raporları görüntülemek için dizininizin üst kısmındaki Raporlar sekmesine gidin.

Raporları ilk kez görüntülüyorsanız raporları görüntüleyebilmek için ilk olarak bir iletişim kutusunu kabul etmeniz gerekir. Bu işlem, bazı ülkelerde özel bilgi olarak kabul edilebilecek olan bu verilerin kuruluşunuzdaki yöneticiler tarafından görüntülenmesinin kabul edilebilir olduğundan emin olmak amacıyla gerçekleştirilir.

![İletişim kutusu](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Raporların her birini araştırma
Toplanmakta olan verileri ve işlenen oturum açma işlemlerini görmek için her rapora gidin. [Tüm raporların listesini burada](active-directory-reporting-guide.md) bulabilirsiniz.

![Tüm raporlar](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>Raporları CSV olarak indirme
Raporların her biri CSV (virgülle ayrılmış değer) dosyası olarak indirilebilir. Verilerinizi daha ayrıntılı olarak çözümlemek için bu dosyaları Excel, PowerBI veya üçüncü taraf analiz programlarında kullanabilirsiniz.

Herhangi bir raporu CSV olarak indirmek için rapora gidin ve alt taraftaki "İndir" düğmesine tıklayın.

![İndir düğmesi](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Anormal oturum açma etkinliği için uyarıları özelleştirme
Dizininizin "Yapılandır" sekmesine gidin.

Sayfayı kaydırarak "Bildirimler" bölümüne gidin.

"Anormal oturum açma işlemlerine yönelik e-posta bildirimleri" bölümünü etkinleştirin veya devre dışı bırakın.

![Bildirimler bölümü](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>Azure AD Raporlama API'si ile tümleştirme
Bkz. [Raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Multi-Factor Authentication'ı kullanıcılar üzerinde uygulama
Rapordaki bir kullanıcıyı seçin.

Ekranın alt kısmındaki "MFA'yı etkinleştir" düğmesine tıklayın.

![Ekranın alt kısmındaki Multi-Factor Authentication düğmesi](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.
> 
> 

## <a name="learn-more"></a>Daha fazla bilgi edinin
### <a name="audit-events"></a>Denetim olayları
[Azure Active Directory Raporlama Denetim Olayları](active-directory-reporting-audit-events.md) içinde hangi olayların dizinde denetlendiği konusunda bilgi edinin.

### <a name="api-integration"></a>API Tümleştirme
Bkz. [Raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) ve [API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>İletişim
Geri bildirim, yardım veya her türlü sorularınız için [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) adresine e-posta gönderin.

> [!TIP]
> Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.
> 
> 


