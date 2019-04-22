---
title: Değiştirilen varsayılan kuralları - Azure AD Connect'i nasıl | Microsoft Docs
description: Azure AD Connect ile gelen değiştirilmiş varsayılan kuralları düzeltmeyi öğrenin.
services: active-directory
author: billmath
manager: daveba
editor: curtand
ms.reviewer: darora10
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 761f3e6e72319a2e63d6b66f2893130ec5a82ebf
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59698173"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Azure AD Connect'e bağlanan değiştirilmiş varsayılan kuralları Düzelt

Azure AD Connect, eşitleme için varsayılan kurallar ile birlikte gelir.  Ne yazık ki bu kurallar Evrensel tüm kuruluşlar için geçerli değildir ve zamanlar, gereksinimlerinize göre bunları değiştirmeniz gerektiğinde bağlı olabilir.

 Varsayılan kuralları değiştirdiniz veya bunları değiştirmek planlama, lütfen bu belgeyi okumak için bir dakikanızı ayırın.

Bu belge, kullanıcılar tarafından gerçekleştirilen yaygın özelleştirmeleri 2 örneklerini gösterir ve bu özelleştirmeler elde etmek için doğru şekilde açıklanmaktadır.

>[!NOTE] 
> Gerekli bir özelleştirme elde etmek için var olan varsayılan kuralları değiştirme - bu en son sürüme gelecek sürümlerde bu kuralları güncelleştiriliyor engelleyecek şekilde desteklenmez. Bu, gerekli hata düzeltmeleri ve yeni özellikleri alma engeller.  Bu belge, var olan varsayılan kuralları değiştirmeden aynı sonucu elde etmek nasıl anlatılmıştır. 

## <a name="how-to-identify-modified-default-rules"></a>Değiştirilen varsayılan kuralları tanımlamak nasıl?
' In 1.3.7.0 sürümünden başlayarak **Azure AD Connect**, değiştirilen varsayılan kuralını tanımlamak artık kolaydır. Masaüstünde uygulamalar'bölümüne gidin ve tıklayarak **eşitleme kuralları Düzenleyicisi**.

![Düzenleyici](media/how-to-connect-fix-default-rules/default1.png)

Düzenleyici'de herhangi bir değişiklik varsayılan kuralın adını önünde bir simge ile aşağıda gösterildiği gibi gösterilir:

![Simgesi](media/how-to-connect-fix-default-rules/default2.png)

 Ayrıca, standart varsayılan kuralı yanında aynı ada sahip bir devre dışı bırakılmış kural görürsünüz:

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Ortak özelleştirmeleri
Ortak özelleştirmeleri için varsayılan kurallar şunlardır:

- [Öznitelik akışı değiştirme](#changing-attribute-flow)
- [Kapsam belirleme filtresi değiştirme](#changing-scoping-filter)
- [Birleştirme koşulunun değiştirme](#changing-join-condition)

## <a name="before-changing-any-rules"></a>Herhangi bir kural değiştirmeden önce
- Eşitleme Zamanlayıcısı'nı devre dışı bırakın.  Zamanlayıcı, varsayılan olarak 30 dakikada bir çalışır. Değişiklikler yapmak ve yeni kurallarınızı sorun giderme sırasında başlatıyor değil emin olun. Zamanlayıcı geçici olarak devre dışı bırakmak, PowerShell'i başlatın ve çalıştırın `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 ![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default3.png)

- Kapsam belirleme filtresi değişiklik hedef dizinde nesnelerin neden olabilir. Lütfen kapsamı belirleme nesnelerin herhangi bir değişiklik yapmadan önce dikkatli olun. Etkin sunucu üzerinde değişiklik yapmadan önce bir hazırlık sunucusu için değişiklik yapmak için önerilir.
- Lütfen bir önizleme de belirtildiği gibi tek bir nesne üzerinde çalıştırın [doğrulama eşitleme kuralı](#validate-sync-rule) herhangi yeni bir kural ekledikten sonra bölümünde.
- Lütfen yeni bir kural eklemek veya herhangi bir özel eşitleme kuralı değiştirdikten sonra tam eşitleme çalıştırın. Bu eşitleme yeni kurallar tüm nesnelere uygulanır.

## <a name="changing-attribute-flow"></a>Öznitelik akışı değiştirme
Öznitelik akışı için 3 farklı senaryo vardır, standart varsayılan kuralları değiştirmeden elde etmek nasıl göz atalım.
- Yeni öznitelik ekleme
- Var olan özniteliğin değerini geçersiz kılma
- Varolan bir özniteliği eşitleme

### <a name="adding-new-attribute"></a>Yeni öznitelik ekleme:
Bir öznitelik hedef dizine, kaynak dizinden akışı sonra kullanabileceğiniz bulursanız [Azure AD Connect eşitleme: Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md) yeni öznitelik akışı.

İlk tercihiniz kullanılacak olması gerektiğini lütfen unutmayın [Azure AD Connect eşitleme: Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md), bir Azure AD Connect tarafından sağlanan kutusu özelliği dışında. Sizin için işe yaramadı sonra mevcut standart eşitleme Kuralı değiştirmeden bir öznitelik akışı için aşağıdaki adımları gidin, Bununla birlikte, iki yeni eşitleme kuralları ekleyerek bunu yapabilirsiniz.


#### <a name="add-an-inbound-sync-rule"></a>Gelen eşitleme kuralı ekleyin:
Gelen eşitleme kuralı kaynak özniteliği için bir bağlayıcı alanı ve hedef meta veri deposu anlamına gelir. Örneğin, şirket içi Active Directory'den Azure Active Directory için yeni bir öznitelik akışı için yeni bir gelen eşitleme kuralı başlatarak oluşturun **Synchronization Rule Editor**, yönde seçip **gelen** tıklatıp **Yeni Kural Ekle**. 

 ![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default3a.png)

Kural adı için kendi adlandırma kuralını izler, burada kullandığımız **özel içinde ad - kullanıcı**, bu kuralın özel bir kural ve AD Bağlayıcısı alanından meta veri deposu için bir gelen kuralı olduğunu anlamına gelir.   

 ![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default3b.png)

Kuralın gelecek bakım kuralın amacı nedir ve neden gerekir gibi kolay, böylece kendi Kural açıklamasını verir.
Bir bağlı sistem (orman) - öznitelik kaynak seçin. Ardından, bağlı sistem nesne türü ve meta veri deposu nesne türü seçin.

0 – arasındaki öncelik değeri belirtin (sayı, daha yüksek öncelik alt) 99. Diğer alanlar 'Tag' gibi 'Parola eşitlemeyi Etkinleştir' ve 'Disabled' varsayılan olarak tutun.

Scoping sakla 'filtresi' boş, bu kural AD bağlı sistem meta veri deposu arasında birleştirilmiş tüm nesnelere uygulanır anlamına gelir.

Bu kural anlamına gelir 'kuralları JOIN' boş için standart kuralda tanımlanan birleştirme koşulunun Paketle devam eder. Devre dışı bırak/standart varsayılan kural hiçbir birleştirme koşulunun Paketle ise ardından öznitelik akan çünkü silme değil başka bir nedeni budur. 

Uygun bir dönüşüm ekleyin, öznitelik için bir sabit değer, hedef özniteliğe akışı sağlanacak sabiti atayabilir, veya doğrudan kaynak veya hedef öznitelik veya özniteliği için bir ifade arasında eşleme. İşte çeşitli [ifade işlevlerine](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) kullanabilirsiniz.

#### <a name="add-an-outbound-sync-rule"></a>Giden eşitleme kuralı ekleyin:
Öznitelik hedef dizine henüz ilişkilendirilmediği için şu ana kadar yalnızca bir gelen eşitleme kuralı ekleyerek yarım iş uyguladığımız. Öznitelik hedef director bağlamak için kaynak meta veri deposu ve hedef bağlı sistem anlamına gelir. bir giden kuralı oluşturmanız gerekir. Bir giden kuralı oluşturmak için başlatma **Synchronization Rule Editor**, değiştirme **yönü** için **giden** tıklatıp **Yeni Kural Ekle**. 

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default3c.png)

Gelen kural olduğu gibi kendi adlandırma kuralını kullanabilirsiniz **adı** kural. Seçin **bağlı sistem** bağlı sistem nesnesi öznitelik değeri ayarlamak istediğiniz Azure AD kiracısı seçin. Önceliği arasında 0 - 99. 

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default3d.png)

Tutun **Scoping filtre** boş, canlı **birleştirme kuralları** boş, dönüştürme sabiti, doğrudan ya da ifadesi olarak doldurun. 

Bu örnekte biz kullanıcı nesnesi Active Directory'den Azure Active Directory için yeni öznitelik akışı nasıl göstermiştir. Kaynak ve hedef için herhangi bir nesneden herhangi bir öznitelik eşlemek için aşağıdaki adımları kullanabilirsiniz.  Daha fazla bilgi için [özel eşitleme kuralları oluşturma](how-to-connect-create-custom-sync-rule.md) ve [hazırlama için kullanıcı sağlama](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="overriding-value-of-existing-attribute"></a>Var olan özniteliğin değerini geçersiz kılma
Zaten eşlenmiş özniteliğinin değeri geçersiz kılmak istediğiniz, örneğin her zaman Null değeri Azure AD'de bir öznitelik için ayarlamak istediğiniz, önceki adımda ve flow'da belirtildiği gibi bir gelen kuralı yalnızca oluşturarak bunu yapabilirsiniz mümkündür  **AuthoritativeNull** target özniteliği için sabit değer. Lütfen AuthoritativeNulll Null yerine bu durumda kullandık olduğunu unutmayın. Bu durum, daha düşük öncelikli (kuralında daha yüksek sayı değeri) sahip olsa bile null olmayan değer Null değerini değiştirir çünkü. Ancak, AuthoritativeNull Null olarak kabul edilir ve null olmayan değer ile diğer kurallar tarafından değiştirilmeyecek. 

### <a name="dont-sync-existing-attribute"></a>Varolan bir özniteliği eşitleme
Ardından bir öznitelik eşitlenmesini dışlamak istiyorsanız, Azure AD Connect sağlanan özellik filtreleme özniteliği kullanabilirsiniz. Başlatma **Azure AD Connect** Masaüstü simgesine tıklayın ve ardından gelen **eşitleme seçeneklerini özelleştirme**.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default4.png)

 Emin **Azure AD uygulaması ve öznitelik filtreleme** tıklatın ve seçili **sonraki**.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default5.png)

Eşitlemenin çıkarmak istediğiniz öznitelikleri işaretini kaldırın.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="changing-scoping-filter"></a>Kapsam belirleme filtresi değiştirme
Azure AD eşitleme nesneleri çoğunu üstlenir, nesnelerin kapsamını azaltın ve standart varsayılan eşitleme kurallarını değiştirmeden desteklenen bir biçimde verilecek daha az nesne azaltın. Daha sonra nesnelerinin kapsamını artırmak istemeniz durumunda **Düzenle** mevcut bir kuralı kopyalayın ve özgün kuralı devre dışı bırakın. Microsoft Azure AD Connect tarafından yapılandırılmış kapsam artırmanız değil önerir. Nesnelerinin kapsamını artış özelleştirmeleri anlamak ve ürün desteği destek ekibi için sabit yapar.

İşte Azure AD'ye eşitlenmiş nesnelerin kapsamını nasıl azaltabilir. Lütfen kapsamını daraltırsanız unutmayın **kullanıcılar** parola karması eşitlemeyi de filtrelenmiş genişletme kullanıcılar için durur eşitlendiğinden. Nesneleri zaten, ardından kapsam azaltma sonra eşitliyorsanız filtrelenmiş genişletme nesneleri hedef dizinden silinir, lütfen çok dikkatli bir şekilde kapsam üzerinde çalışın.
Eşitleme yapan nesnelerinin kapsamını azaltmak için desteklenen yollar şunlardır.

- [cloudFiltered özniteliği](#cloudfiltered-attribute)
- [OU filtreleme](#ou-filtering)

### <a name="cloudfiltered-attribute"></a>cloudFiltered özniteliği
Bu Active Directory'de ayarlanan bir öznitelik olmadığını unutmayın. Belirtildiği gibi yeni bir gelen kuralı ekleyerek bu özniteliğin değeri ayarlamanız gerekir **mevcut öznitelik değerini geçersiz kılma** bölümü. Ardından **dönüştürme** ve **ifade** bu öznitelik meta veri deposunda ayarlamak için. Kullanıcının departmanı adı büyük/küçük harf duyarsız'ile başlayan tüm eşitlemek istemediğiniz bir örnek aşağıdadır **HRD**:

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

Biz öncelikle departman (Active Directory) kaynağından küçük harflere dönüştürdük. Sol işlevini kullanarak daha sonra yalnızca ilk 3 karakteri sürdü ve hrd ile karşılaştırılır. Eşleştiği değeri True tersi durumda NULL olarak ayarlayın. (Daha yüksek sayı değeri) daha düşük önceliğe sahip başka bir kural için farklı bir koşulla yazabilmesi amacıyla değerin NULL, ayarladığınız unutmayın. Lütfen Önizleme belirtildiği gibi eşitleme kuralını doğrulamak için bir nesne üzerinde [doğrulama eşitleme kuralı](#validate-sync-rule) bölümü.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default7a.png)



### <a name="ou-filtering"></a>OU filtreleme
Bir veya daha fazla OU'lar oluştur ve bu OU'lar eşitlemek istemediğiniz nesneleri taşıma. Ardından Azure AD Connect'e bağlanan başlatarak filtreleme OU yapılandırın **Azure AD Connect** Masaüstü simgesi ve aşağıda gösterildiği gibi seçeneklerini belirleyin. Azure AD Connect yükleme sırasında filtreleme OU de yapılandırabilirsiniz. 

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default8.png)

Sihirbazı izleyin ve ardından eşitlemek istemediğiniz OU'ları seçimini kaldırın.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default9.png)

## <a name="changing-join-condition"></a>Birleştirme koşulunun değiştirme
Microsoft, varsayılan değer kullandığınız Azure AD Connect tarafından yapılandırılmış koşullar katıldığınız önerir. Varsayılan birleştirme koşulları değiştirme özelleştirmeleri anlamak ve ürün desteği destek ekibi için sabit yapar.

## <a name="validate-sync-rule"></a>Eşitleme kuralı doğrula
Yeni eklenen eşitleme kuralı tam eşitleme döngüsünü başlatmadan önizleme özelliğini kullanarak doğrulayabilirsiniz. Başlatma **eşitleme hizmeti** kullanıcı Arabirimi.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default10.png)

Tıklayarak **meta veri deposu arama**seçin kapsam nesnesi olarak **kişi**, **yan tümce Ekle** ve arama ölçütlerinizi bahsedebilirsiniz. Tıklayarak **arama** düğmesi ve nesneye çift **arama sonuçları** bu adımı gerçekleştirmeden önce içeri aktarma ve eşitleme ormana çalıştırdığınızda bu verileri Azure AD CONNECT'te emin olmak için lütfen unutmayın Bu nesne için güncel durumda.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default11.png)



Seçin **Bağlayıcılar**, nesne içinde karşılık gelen connector(forest) seçin, **özellikleri...** .

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default12.png)

Tıklayarak **Önizleme...**

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default13a.png)

Tıklayarak **oluşturma Önizleme** ve **içeri aktarma öznitelik akışı** sol bölmesinde.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default14.png)
 
Burada yeni eklenen kural nesne üzerinde çalışır ve cloudFiltered özniteliği True olarak ayarlanmış fark edeceksiniz.

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default15a.png)
 
Varsayılan kural kuralıyla değiştiren karşılaştırma nasıl?
Metin dosyaları olarak ayrı ayrı hem kuralları verebilirsiniz. Bu kurallar, powershell komut dosyası olarak dışarı aktarılır. Ne tür değişiklikler yapılır görmek için herhangi bir dosya karşılaştırma aracını kullanarak bunları karşılaştırabilirsiniz. Burada Bu örnekte miyim windiff iki dosyayı karşılaştırmak için kullanılır.
 
Kullanıcı kuralı değiştirildi dikkat edin, msExchMailboxGUID özniteliği değiştiğinde **ifade** türü yerine **doğrudan** değerine sahip **NULL** ve  **ExecuteOnce** seçeneği. Identified ve öncelik farklılıkları yoksayabilirsiniz. 

![Varsayılan kurallar](media/how-to-connect-fix-default-rules/default17.png)
 
Değiştirilen varsayılan kural nasıl?
Kurallarınızı için varsayılan ayarları düzeltmek için değiştirilen kuralını silmek ve varsayılan kural aşağıda gösterildiği gibi etkinleştirebilir ve ardından çalıştırın bir **tam eşitleme**. Lütfen özelleştirme kaybetmeyin. böylece, yukarıda belirtildiği gibi düzeltme girişimlerinde bulunun yapmak önce elde etmek çalıştığınız ## sonraki adımlar

## <a name="next-steps"></a>Sonraki Adımlar
- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)

