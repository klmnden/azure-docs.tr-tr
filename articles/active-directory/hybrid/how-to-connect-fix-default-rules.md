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
ms.openlocfilehash: d2f0956b44d6df64fb73e5eee7844574237d8755
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067628"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Azure AD Connect'e bağlanan değiştirilmiş varsayılan kuralları Düzelt

Azure Active Directory (Azure AD) Connect kullanıyor için varsayılan kurallar eşitleme.  Ne yazık ki, bu kurallar Evrensel tüm kuruluşlar için geçerli değildir. Gereksinimlerinize göre bunları değiştirmeniz gerekebilir. Bu makalede, en yaygın özelleştirmeler iki örnek anlatılmaktadır ve bu özelleştirmeler elde etmek için doğru şekilde açıklar.

>[!NOTE] 
> Gerekli bir özelleştirme elde etmek için var olan varsayılan kuralları değiştirme desteklenmez. Bu kurallar gelecekte en son sürüme güncelleştirme engeller, bunu yaparsanız serbest bırakır. Gereksinim duyduğunuz hata düzeltmeleri veya yeni özellikler elde etmezsiniz. Bu belgede, var olan varsayılan kuralları değiştirmeden aynı sonucu elde etmek açıklanmaktadır. 

## <a name="how-to-identify-modified-default-rules"></a>Değiştirilen varsayılan kuralları tanımlama
Azure AD Connect 1.3.7.0 sürümünden itibaren değiştirilen varsayılan kuralını tanımlamak kolaydır. Git **Masaüstü uygulamaları**seçip **eşitleme kuralları Düzenleyicisi**.

![Azure AD Connect, eşitleme kuralları Düzenleyicisi vurgulanmış ile](media/how-to-connect-fix-default-rules/default1.png)

Düzenleyici'de herhangi bir değişiklik varsayılan kuralın adın önüne bir uyarı simgesi ile gösterilir.

![uyarı simgesi](media/how-to-connect-fix-default-rules/default2.png)

 Devre dışı aynı ada sahip yanında da görünür kural (Bu, standart varsayılan kural).

![Eşitleme kuralları Düzenleyicisi, standart varsayılan kuralı ve değiştirilen varsayılan kuralı gösteriliyor](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Ortak özelleştirmeleri
Ortak özelleştirmeleri için varsayılan kurallar şunlardır:

- Öznitelik akışı değiştirme
- Kapsam belirleme filtresi değiştirme
- Birleştirme koşulunun Değiştir

Herhangi bir kural değiştirmeden önce:

- Eşitleme Zamanlayıcısı'nı devre dışı bırakın. Zamanlayıcı, varsayılan olarak 30 dakikada bir çalışır. Değişikliği yapmadan ve yeni kurallarınızı sorun giderme sırasında başlatıyor değil emin olun. Zamanlayıcı geçici olarak devre dışı bırakmak PowerShell'i başlatın ve çalıştırın `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 ![Eşitleme Zamanlayıcısı'nı devre dışı bırakmak için PowerShell komutları](media/how-to-connect-fix-default-rules/default3.png)

- Kapsam belirleme filtresi değişiklik hedef dizinde nesnelerin neden olabilir. Kapsam belirleme nesnelerin herhangi bir değişiklik yapmadan önce dikkatli olun. Etkin sunucu üzerinde değişiklik yapmadan önce bir hazırlık sunucusu için değişiklikleri yapmanızı öneririz.
- Belirtildiği gibi tek bir nesne üzerinde bir preview çalıştırması [doğrulama eşitleme kuralı](#validate-sync-rule) herhangi bir yeni kural ekledikten sonra bölümünde.
- Yeni bir kural eklemek veya herhangi bir özel eşitleme kuralı değiştirdikten sonra tam eşitleme çalıştırın. Bu eşitleme, yeni kurallar tüm nesnelere uygulanır.

## <a name="change-attribute-flow"></a>Öznitelik akışı değiştirme
Öznitelik akışı değiştirmek için üç farklı senaryo vardır:
- Yeni bir öznitelik ekleme.
- Varolan bir özniteliği değeri geçersiz kılma.
- Varolan bir özniteliği değil eşitlenecek seçme.

Bu standart varsayılan kuralları değiştirmeden bunu yapabilirsiniz.

### <a name="add-a-new-attribute"></a>Yeni bir öznitelik Ekle
Bir öznitelik hedef dizine, kaynak dizinden akışı olduğunu fark ederseniz, kullanın [Azure AD Connect eşitleme: Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md) bu sorunu gidermek için.

Uzantıları sizin için işe yaramazsa, aşağıdaki bölümlerde açıklanan, iki yeni eşitleme kuralları eklemeyi deneyin.


#### <a name="add-an-inbound-sync-rule"></a>Gelen eşitleme Kuralı Ekle
Bir gelen eşitleme kuralı kaynak özniteliği için bir bağlayıcı alanı ve meta veri hedeflediği anlamına gelir. Örneğin, gelen akış yeni bir öznitelik için Azure Active Directory için Active Directory şirket içinde yeni bir gelen eşitleme kuralı oluşturun. Başlatma **eşitleme kuralları Düzenleyicisi**seçin **gelen** yönü ve select **Yeni Kural Ekle**. 

 ! Eşitleme kuralları Editor](media/how-to-connect-fix-default-rules/default3a.png)

Kural adı için kendi adlandırma kuralı uygulayın. Burada kullandığımız **özel içinde ad - kullanıcı**. Bu kural bir özel kural ve Active Directory Bağlayıcısı alanına öğesinden meta veri deposu için bir gelen kuralı olduğu anlamına gelir.   

 ![Gelen eşitleme kuralı oluşturma](media/how-to-connect-fix-default-rules/default3b.png)

Kuralın gelecek bakım kadar kolay olduğunu kendi Kural açıklamasını sağlayın. Örneğin, açıklama, kuralın amacı nedir ve neden gerekir dayanabilir.

Seçimlerinizi yapın **bağlı sistem**, **bağlı sistem nesnesi türü**, ve **meta veri deposu nesne türü** alanları.

0 ile 99 arasında bir öncelik değeri belirtin (düşük sayı, daha yüksek öncelik). İçin **etiketi**, **parola eşitlemeyi etkinleştirme**, ve **devre dışı bırakılmış** alanları, varsayılan seçimleri kullanın.

Tutun **Scoping filtre** boş. Tüm nesneler için kuralın geçerli olacağı anlamına gelir, Active Directory sistemi bağlı meta veri deposu arasında katıldı.

Tutun **birleştirme kuralları** boş. Başka bir deyişle, bu kural standart kuralda tanımlanan birleştirme koşulunun kullanır. Devre dışı bırakmayın veya standart varsayılan kural silmek için başka bir nedeni budur. Hiçbir birleştirme koşulunun varsa, öznitelik akışı olmaz. 

Uygun dönüştürmeleri için öznitelik ekleyin. Bir sabit hale getirmek için bir sabit atayabilirsiniz değeri, hedef özniteliğe akışı. Kaynak veya hedef öznitelik arasında doğrudan eşleme kullanabilirsiniz. Veya özniteliği için bir ifade kullanabilirsiniz. İşte çeşitli [ifade işlevlerine](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) kullanabilirsiniz.

#### <a name="add-an-outbound-sync-rule"></a>Giden eşitleme Kuralı Ekle
Öznitelik hedef dizine bağlanmak için bir giden kuralı oluşturmanız gerekir. Bu meta veri kaynağıdır ve bağlı sistem hedeflediği anlamına gelir. Bir giden kuralı oluşturmak için başlatma **eşitleme kuralları Düzenleyicisi**, değiştirme **yönü** için **giden**seçip **Yeni Kural Ekle**. 

![Eşitleme kuralları Düzenleyicisi](media/how-to-connect-fix-default-rules/default3c.png)

Olarak gelen kuralı ile kendi adlandırma kuralı adlandırmak için kullanabilirsiniz. Seçin **bağlı sistem** Azure AD kiracısı ve select bağlı sistem olarak için istediğiniz nesne öznitelik değeri ayarlayın. 0 ile 99 arasında önceliği ayarlayın. 

![Giden eşitleme kuralı oluşturma](media/how-to-connect-fix-default-rules/default3d.png)

Tutun **Scoping filtre** ve **birleştirme kuralları** boş. Dönüştürme sabiti, doğrudan ya da ifadesi olarak doldurun. 

Active Directory'den Azure Active Directory'ye yeni kullanıcı nesnesi akışı öznitelik nasıl biliyorsunuz. Kaynak ve hedef için herhangi bir nesneden herhangi bir öznitelik eşlemek için aşağıdaki adımları kullanabilirsiniz. Daha fazla bilgi için [özel eşitleme kuralları oluşturma](how-to-connect-create-custom-sync-rule.md) ve [hazırlama için kullanıcı sağlama](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="override-the-value-of-an-existing-attribute"></a>Varolan bir özniteliği değeri geçersiz kıl
Zaten eşleştirilmiş bir öznitelik değeri geçersiz kılmak isteyebilirsiniz. Örneğin, her zaman null değeri Azure AD'de öznitelik ayarlamak istiyorsanız, yalnızca yalnızca bir gelen kuralı oluşturun. Sabit değeri `AuthoritativeNull`, hedef özniteliğe akışı. 

>[!NOTE] 
> Kullanım `AuthoritativeNull` yerine `Null` bu durumda. Null olmayan değer null değeri değiştirdiğinden daha düşük önceliğe (bir daha yüksek sayı değeri kuralında) sahip bile bu geçerlidir. `AuthoritativeNull`, diğer taraftan, null olmayan bir değer ile diğer kurallar tarafından değiştirilen değil. 

### <a name="dont-sync-existing-attribute"></a>Varolan bir özniteliği eşitleme
Bir öznitelik eşitlenmesini dışlamak istiyorsanız, Azure AD Connect sağlanan özellik filtreleme özniteliği kullanın. Başlatma **Azure AD Connect** Masaüstü simgesine tıklayın ve ardından gelen **eşitleme seçeneklerini özelleştirme**.

![Azure AD Connect ek görevler seçenekleri](media/how-to-connect-fix-default-rules/default4.png)

 Emin **Azure AD uygulaması ve öznitelik filtreleme** seçili ve select **sonraki**.

![Azure AD Connect'in isteğe bağlı özellikler](media/how-to-connect-fix-default-rules/default5.png)

Eşitlemenin çıkarmak istediğiniz öznitelikleri temizleyin.

![Azure AD Connect öznitelikleri](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="change-scoping-filter"></a>Kapsam belirleme filtresi değiştirme
Azure AD eşitleme nesneleri çoğunu üstlenir. Nesnelerin kapsamını azaltın ve standart varsayılan eşitleme kurallarını değiştirmeden aktarılacak nesneleri sayısını azaltın. 

Eşitleme nesnelerinin kapsamını azaltmak için aşağıdaki yöntemlerden birini kullanın:

- cloudFiltered özniteliği
- Kuruluş birimi filtreleme

Eşitlendiğinden kullanıcıların kapsamını daraltırsanız, parola karması eşitlemeyi filtrelenmiş genişletme kullanıcılar için de durdurulur. Kapsamını azaltın sonra nesneleri zaten, eşitliyorsanız, filtrelenmiş genişletme nesneleri hedef dizinden silinir. Bu nedenle, çok dikkatli bir şekilde kapsam emin olun.

>[!IMPORTANT] 
> Azure AD Connect tarafından yapılandırılan nesnelerinin kapsamını artırmak önerilmez. Bunun yapılması özelleştirmeleri anlamak Microsoft destek ekibi zorlaştırır. Nesnelerinin kapsamını artırmanız gerekir, mevcut kuralı düzenleyin, kopyalayın ve özgün kuralı devre dışı bırakın. 

### <a name="cloudfiltered-attribute"></a>cloudFiltered özniteliği
Active Directory'de bu öznitelik ayarlanamaz. Bu özniteliğin değeri, yeni bir gelen kuralı ekleyerek ayarlayın. Ardından **dönüştürme** ve **ifade** meta veri deposunda bu özniteliği ayarlanamadı. Aşağıdaki örnek, departman adı ile başlayan tüm kullanıcıları eşitlemek istemediğiniz gösterir **HRD** (büyük-küçük harf duyarsız):

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

Biz öncelikle departman (Active Directory) kaynağından küçük harfe dönüştürülür. Ardından, kullanarak `Left` işlevi, biz yalnızca ilk üç karakterine sürdü ve ile karşılaştırıldığında `hrd`. Eşleştiği ise değer kümesine `True`, aksi takdirde `NULL`. Değeri null olarak ayarlayarak (bir daha yüksek sayı değeri) daha düşük önceliğe sahip başka bir kural için farklı bir koşulla yazabilirsiniz. Eşitleme kuralı belirtildiği gibi doğrulamak için bir nesne üzerinde çalıştırma Önizleme [doğrulama eşitleme kuralı](#validate-sync-rule) bölümü.

![Gelen eşitleme kuralı seçeneklerini oluşturma](media/how-to-connect-fix-default-rules/default7a.png)

### <a name="organizational-unit-filtering"></a>Kuruluş birimi filtreleme
Bir veya daha fazla kuruluş birimine (OU) oluşturun ve bu OU'lar eşitlemek istemediğiniz nesneleri taşıyabilirsiniz. Ardından, Azure AD Connect'e bağlanan filtreleme OU yapılandırın. Başlatma **Azure AD Connect** Masaüstü simgesi ve aşağıdaki seçeneklerini belirleyin. Azure AD Connect yükleme sırasında filtreleme OU de yapılandırabilirsiniz. 

![Azure AD Connect ek görevler](media/how-to-connect-fix-default-rules/default8.png)

Sihirbazı izleyin ve eşitlemek istemediğiniz OU'ları kaldırın.

![Azure AD Connect etki alanı ve OU filtreleme seçenekleri](media/how-to-connect-fix-default-rules/default9.png)

## <a name="change-join-condition"></a>Birleştirme koşulunun Değiştir
Azure AD Connect tarafından yapılandırılan varsayılan birleştirme koşulları kullanın. Varsayılan birleştirme koşulları değiştirme özelleştirmeleri anlamak ve ürün desteği için Microsoft destek zorlaştırır.

## <a name="validate-sync-rule"></a>Eşitleme kuralı doğrula
Yeni eklenen eşitleme kuralı tam eşitleme döngüsünü başlatmadan önizleme özelliğini kullanarak doğrulayabilirsiniz. Azure AD Connect'i seçin **eşitleme hizmeti**.

![Azure AD Connect, eşitleme hizmeti vurgulanmış ile](media/how-to-connect-fix-default-rules/default10.png)

Seçin **meta veri deposu arama**. Olarak kapsam nesnesi seçin **kişi**seçin **yan tümce Ekle**ve arama ölçütlerinizi bahsedebilirsiniz. Ardından, **arama**, nesne arama sonuçlarında'ne çift tıklayın. Bu adımı gerçekleştirmeden önce içeri aktarma ve eşitleme ormana çalıştırarak verilerinizi Azure AD CONNECT'te bu nesne için güncel olduğundan emin olun.

![Eşitleme Hizmeti Yöneticisi](media/how-to-connect-fix-default-rules/default11.png)

Üzerinde **meta veri deposu nesne özellikleri**seçin **Bağlayıcılar**, nesne ilgili bağlayıcıda (orman) seçip **özellikleri...** .

![Meta veri deposu nesne özellikleri](media/how-to-connect-fix-default-rules/default12.png)

Seçin **Önizleme...**

![Bağlayıcı alanı nesne özellikleri](media/how-to-connect-fix-default-rules/default13a.png)

Önizleme penceresini seçin **oluşturma Önizleme** ve **içeri aktarma öznitelik akışı** sol bölmesinde.

![Önizleme](media/how-to-connect-fix-default-rules/default14.png)
 
Burada, yeni eklenen kural nesne üzerinde çalışır ve ayarlanmış fark `cloudFiltered` özniteliğini true.

![Önizleme](media/how-to-connect-fix-default-rules/default15a.png)
 
Varsayılan kural değiştirilmiş olan kural karşılaştırmak için kuralların her ikisi de ayrı olarak, metin dosyaları olarak dışarı aktarın. Bu kurallar, bir PowerShell komut dosyası dışarı aktarılır. Bunları, değişiklikleri görmek için herhangi bir dosya karşılaştırma aracını (örneğin, windiff) kullanarak karşılaştırabilirsiniz. 
 
Değiştirilen kuralda dikkat `msExchMailboxGuid` özniteliği değiştiğinde **ifade** türü yerine **doğrudan**. Ayrıca, değer değiştirilecek **NULL** ve **ExecuteOnce** seçeneği. Identified ve öncelik farklılıkları yoksayabilirsiniz. 

![Windiff araç çıktısı](media/how-to-connect-fix-default-rules/default17.png)
 
Varsayılan ayarlarına geri değiştirmeye kurallarınızı düzeltmek için değiştirilmiş kural silin ve varsayılan kural'ı etkinleştirin. Elde etmeye çalıştığınız özelleştirme kaybetmediğinizden emin olun. Hazır olduğunuzda, çalıştırma **tam eşitleme**.

## <a name="next-steps"></a>Sonraki adımlar
- [Donanım ve önkoşullar](how-to-connect-install-prerequisites.md) 
- [Hızlı ayarlar](how-to-connect-install-express.md)
- [Özelleştirilmiş ayarlar](how-to-connect-install-custom.md)



