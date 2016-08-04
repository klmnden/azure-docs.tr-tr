<properties
   pageTitle="Azure Active Directory B2B işbirliği önizlemesini kullanmaya yönelik ayrıntılı kılavuz | Microsoft Azure"
   description="Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler."
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# Azure AD B2B işbirliği önizlemesi: Ayrıntılı kılavuz

Bu kılavuz, Azure AD B2B işbirliğini kullanmaya yönelik bilgileri ana hatlarıyla sunar. Contoso'nun BT yöneticisi olarak uygulamaları, üç iş ortağı şirketin çalışanlarıyla paylaşmak istiyoruz. İş ortağı şirketlerinden hiçbirinin Azure AD'ye sahip olması gerekmez.

- Simple Partner Org'dan Alice
- Medium Partner Org'dan Bob'un bir dizi uygulamaya erişmesi gerekiyor
- Complex Partner Org'dan Carol'ın bir dizi uygulamaya ve Contoso'da grup üyeliğine erişmesi gerekiyor

İş ortağı kullanıcılara davet gönderildikten sonra Azure portalı üzerinden uygulamalara ve grup üyeliğine erişim elde etmek için bunları Azure AD'de yapılandırabiliriz. Alice'i ekleyerek başlayalım.

## Alice'i Contoso dizinine ekleme
1. Yalnızca Alice'in **Email**, **DisplayName** ve **InviteContactUsUrl** bilgilerini doldurarak gösterilen üst bilgilerle bir .csv dosyası oluşturun. **DisplayName**, davette ve aynı zamanda Contoso Azure AD dizininde görüntülenen addır. **InviteContactUsUrl**, Alice'in Contoso ile iletişim kurma yöntemidir. Aşağıdaki örnekte InviteContactUsUrl, Contoso'nun LinkedIn profilini belirtir. .csv dosyasının ilk satırındaki etiketleri, tam olarak [CSV dosya biçimi başvurusu](active-directory-b2b-references-csv-file-format.md) kapsamında belirtilen şekilde yazmak son derece önemlidir.  
![Alice için örnek CSV dosyası](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Azure portalında Contoso dizinine (Active Directory > Contoso > Kullanıcılar > Kullanıcı Ekle) bir kullanıcı ekleyin. "Kullanıcı Türü" açılan listesinde "İş ortağı şirketlerdeki kullanıcılar" seçeneğini belirleyin. .csv dosyasını karşıya yükleyin. Karşıya yüklemeden önce .csv dosyasının kapalı olduğundan emin olun.  
![Alice için CSV dosyasını karşıya yükleme](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice, artık Contoso Azure AD dizininde bir Dış Kullanıcı olarak temsil edilir.  
![Alice, Azure AD içinde listelenir](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alice, aşağıdaki e-postayı alır.  
![Alice için davet e-postası](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice bağlantıya tıklar; daha sonra kendisinden daveti kabul etmesi ve iş kimlik bilgileri ile oturum açması istenir. Alice Azure AD dizininde değilse kendisinden kaydolması istenir.  
![Alice için davetten sonra kaydolma](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alice, kendisine uygulamalara erişim hakkı verilene kadar boş olan Uygulama Erişim Paneli'ne yeniden yönlendirilir.  
![Alice için Erişim Paneli](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Bu yordam, B2B işbirliğinin en basit biçimini sunar. Contoso Azure AD dizininde bir kullanıcı olarak Alice'e Azure portalı üzerinden uygulamalara ve gruplara erişim hakkı verilebilir. Şimdi Moodle ve Salesforce uygulamalarına erişmesi gereken Bob'ı ekleyelim.

## Bob'ı Contoso dizinine ekleme ve Bob'a uygulamalara erişim hakkı verme
1. Moodle ve Salesforce uygulama kimliklerini bulmak için Azure AD Modülü'nün yüklü olduğu Windows PowerShell'i kullanın. Kimlikler cmdlet kullanılarak alınabilir: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` Bu, Contoso ve AppPrincialIds kapsamındaki tüm kullanılabilir uygulamalar listesini getirir.  
![Bob için kimlikleri alma](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Bob'ın Email, DisplayName, **InviteAppID**, **InviteAppResources** ve InviteContactUsUrl bilgilerini içeren bir .csv dosyası oluşturun. **InviteAppResources** alanını, bir boşluk bırakarak PowerShell'den bulunan Moodle ve Salesforce uygulamalarına ait AppPrincipalId bilgileri ile doldurun. **InviteAppId** alanını, e-posta ve oturum açma sayfalarında Moodle logosunu belirtmek için Moodle'a ait aynı AppPrincipalId bilgisi ile doldurun.  
![Bob için örnek CSV dosyası](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Alice'te olduğu gibi .csv dosyasını Azure Portal aracılığıyla karşıya yükleyin. Bob, artık Contoso Azure AD dizininde bir dış kullanıcıdır.

4. Bob, aşağıdaki e-postayı alır.  
![Bob için davet e-postası](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Bob bağlantıya tıklar ve kendisinden daveti kabul etmesi istenir. Oturum açtıktan sonra Erişim Paneli'ne yönlendirilir ve artık hem Moodle hem de Salesforce uygulamasını kullanabilir.  
![Bob için Erişim Paneli](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Sırada hem uygulamalara hem de Contoso dizinindeki grup üyeliğine erişmesi gereken Carol var.

## Carol'ı Contoso dizinine ekleme, Carol'a uygulamalara erişim hakkı ve grup üyeliği verme

1. Contoso'da uygulama kimlikleri ve grup kimliklerini bulmak için Azure AD Modülü'nün yüklü olduğu Windows PowerShell'i kullanın.
 - Bob için olduğu gibi cmdlet `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` kullanarak AppPrincipalId alma
 - cmdlet `Get-MsolGroup | fl DisplayName, ObjectId` kullanarak gruplar için ObjectId alın. Bu işlem, Contoso'daki tüm grupların ve bu gruplara ilişkin ObjectId bilgilerinin yer aldığı bir liste getirir. Grup Kimlikleri de Azure portalındaki grubun Özellikler sekmesinde Nesne Kimliği olarak alınabilir.  
![Carol için kimlikleri ve grupları alma](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Carol'ın Email, DisplayName, InviteAppID, InviteAppResources, **InviteGroupResources** ve InviteContactUsUrl bilgilerini doldurarak .csv dosyası oluşturun. **InviteGroupResources** alanı, bir boşluk bırakılarak MyGroup1 ve Externals gruplarının ObjectId bilgileri ile doldurulur.  
![Carol için örnek CSV dosyası](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Azure portalı aracılığıyla .csv dosyasını karşıya yükleyin.

4. Carol, Contoso dizininde bir kullanıcı ve aynı zamanda Azure portalında göründüğü gibi MyGroup1 ve Externals gruplarının bir üyesidir.  
![Carol, Azure AD'de bir grupta listelenir](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Carol, daveti kabul etmeye yönelik bir bağlantı içeren e-posta alır. Oturum açtıktan sonra Moodle ve Salesforce'a erişebilmek için Uygulama Erişim Paneli'ne yeniden yönlendirilir.  

Azure AD B2B işbirliği kapsamında iş ortağı kuruluşlardan kullanıcı ekleme ile ilgili tüm bilgiler verilmiştir. Bu kılavuzda, üç ayrı .csv dosyası kullanarak Alice, Bob ve Carol adında üç kullanıcının Contoso dizinine nasıl ekleneceği gösterilmiştir. Bu işlem, ayrı .csv dosyalarını tek bir dosya halinde sıkıştırarak daha kolay hale getirilebilir.  
![Alice, Bob ve Carol için örnek CSV dosyası](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## İlgili makaleler
Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

- [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Nasıl çalışır?](active-directory-b2b-how-it-works.md)
- [CSV dosya biçimi başvurusu](active-directory-b2b-references-csv-file-format.md)
- [Dış kullanıcı belirteci biçimi](active-directory-b2b-references-external-user-token-format.md)
- [Dış kullanıcı nesnesi öznitelik değişiklikleri](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Geçerli önizleme sınırlamaları](active-directory-b2b-current-preview-limitations.md)
- [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)



<!----HONumber=Jun16_HO2-->


