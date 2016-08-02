<properties
   pageTitle="Azure Active Directory Raporlama: Başlarken | Microsoft Azure"
   description="Azure Active Directory raporlamada kullanılabilen çeşitli raporları listeler"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="stevenpo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# Azure Active Directory Raporlama ile çalışmaya başlama

## Nedir?

Azure Active Directory (Azure AD), dizininize yönelik güvenlik, etkinlik ve denetim raporlarını içerir. Kapsama dahil olan raporların listesi şu şekildedir:

### Güvenlik raporları

- Bilinmeyen kaynaklardan gerçekleştirilen oturum açma işlemleri
- Birden çok hatadan sonra gerçekleştirilen oturum açma işlemleri
- Birden çok coğrafyadan gerçekleştirilen oturum açma işlemleri
- Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri
- Düzensiz oturum açma etkinliği
- Muhtemelen virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri
- Anormal oturum açma etkinliği gösteren kullanıcılar

### Etkinlik raporları

- Uygulama kullanımı: özet
- Uygulama kullanımı: ayrıntılı
- Uygulama panosu
- Hesap hazırlama hataları
- Bireysel kullanıcı cihazları
- Bireysel kullanıcı Etkinliği
- Grup etkinlik raporu
- Parola Sıfırlama Kayıt Etkinlik Raporu
- Parola sıfırlama etkinliği

### Denetim raporları

- Dizin denetimi raporu

> [AZURE.TIP] Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.



## Nasıl çalışır?


### Raporlama işlem hattı

Raporlama işlem hattı üç ana adımdan oluşur. Bir kullanıcı her oturum açtığında veya kimlik doğrulama işlemi gerçekleştirildiğinde aşağıdakiler gerçekleşir:

- İlk olarak, kullanıcının kimliği doğrulanır (başarıyla veya başarısız şekilde) ve sonuç Azure Active Directory hizmeti veritabanlarında depolanır.
- Düzenli aralıklarla, en yeni oturum açma işlemlerinin tümü işlenir. Bu noktada, güvenlik ve anormal etkinlik algoritmalarımız en yeni oturum açma işlemlerinin tümünde şüpheli etkinlik araması gerçekleştirmektedir.
- İşleme aşamasının ardından raporlar yazılır, önbelleğe alınır ve klasik Azure portalında sunulur.

### Rapor oluşturma süreleri

Azure AD platformu tarafından işlenen kimlik doğrulama ve oturum açma işlemlerinin çok fazla olması nedeniyle, işlenen en yeni oturum açma işlemleri ortalama olarak bir saat öncesine aittir. Nadir durumlarda, en yeni oturum açma işlemlerinin işlenmesi 8 saate kadar sürebilir.

En son işlenen oturum açma işlemini, her bir raporun üst kısmındaki yardım metnini inceleyerek bulabilirsiniz.

![Her bir raporun üst kısmındaki yardım metni](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.



## Başlarken


### Klasik Azure portalında oturum açma

Öncelikle [klasik Azure portalında](https://manage.windowsazure.com) genel yönetici veya uyumluluk yöneticisi olarak oturum açmanız gerekir. Ayrıca bir Azure aboneliği hizmet yöneticisi veya ortak yöneticisi olmanız veya "Azure AD Erişimi" Azure aboneliğini kullanıyor olmanız gerekir.

### Raporlara gitme

Raporları görüntülemek için dizininizin üst kısmındaki Raporlar sekmesine gidin.

Raporları ilk kez görüntülüyorsanız raporları görüntüleyebilmek için ilk olarak bir iletişim kutusunu kabul etmeniz gerekir. Bu işlem, bazı ülkelerde özel bilgi olarak kabul edilebilecek olan bu verilerin kuruluşunuzdaki yöneticiler tarafından görüntülenmesinin kabul edilebilir olduğundan emin olmak amacıyla gerçekleştirilir.

![İletişim kutusu](./media/active-directory-reporting-getting-started/dialogBox.png)

### Raporların her birini araştırma

Toplanmakta olan verileri ve işlenen oturum açma işlemlerini görmek için her rapora gidin. [Tüm raporların listesini burada](active-directory-reporting-guide.md) bulabilirsiniz.

![Tüm raporlar](./media/active-directory-reporting-getting-started/reportsMain.png)

### Raporları CSV olarak indirme

Raporların her biri CSV (virgülle ayrılmış değer) dosyası olarak indirilebilir. Verilerinizi daha ayrıntılı olarak çözümlemek için bu dosyaları Excel, PowerBI veya üçüncü taraf analiz programlarında kullanabilirsiniz.

Herhangi bir raporu CSV olarak indirmek için rapora gidin ve alt taraftaki "İndir" düğmesine tıklayın.

![İndir düğmesi](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.





## Sonraki adımlar

### Anormal oturum açma etkinliği için uyarıları özelleştirme

Dizininizin "Yapılandır" sekmesine gidin.

Sayfayı kaydırarak "Bildirimler" bölümüne gidin.

"Anormal oturum açma işlemlerine yönelik e-posta bildirimleri" bölümünü etkinleştirin veya devre dışı bırakın.

![Bildirimler bölümü](./media/active-directory-reporting-getting-started/notificationsSection.png)

### Azure AD Raporlama API'si ile tümleştirme

Bkz. [Raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).

### Multi-Factor Authentication'ı kullanıcılar üzerinde uygulama

Rapordaki bir kullanıcıyı seçin.

Ekranın alt kısmındaki "MFA'yı etkinleştir" düğmesine tıklayın.

![Ekranın alt kısmındaki Multi-Factor Authentication düğmesi](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.




## Daha fazla bilgi edinin


### Denetim olayları

[Azure Active Directory Raporlama Denetim Olayları](active-directory-reporting-audit-events.md) içinde hangi olayların dizinde denetlendiği konusunda bilgi edinin.

### API Tümleştirme

Bkz. [Raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) ve [API başvuru belgeleri](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### İletişim

Geri bildirim, yardım veya sormak istediğiniz sorular için [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) adresine e-posta gönderin.

> [AZURE.TIP] Azure AD Raporlama ile ilgili daha fazla belge için [Erişim ve kullanım raporlarınızı görüntüleme](active-directory-view-access-usage-reports.md) bölümüne bakın.



<!---HONumber=Jun16_HO2-->


