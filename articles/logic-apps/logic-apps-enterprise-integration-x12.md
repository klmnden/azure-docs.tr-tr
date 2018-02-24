---
title: "X12 iletileri B2B Kurumsal tümleştirme - Azure Logic Apps | Microsoft Docs"
description: "Azure Logic Apps ile B2B Kurumsal tümleştirme EDI biçimindeki Exchange X12 iletiler"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 7a274ad33b7181d238203290cf63937df5f13bbc
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="exchange-x12-messages-for-enterprise-integration-with-logic-apps"></a>Logic apps ile Kurumsal tümleştirme için Exchange X12 iletileri

X12 exchange önce iletileri Azure mantıksal uygulamaları için bir X12 oluşturmalısınız sözleşmesi ve bu anlaşma tümleştirme hesabınızı depolamak. Bir X12 oluşturmak için adımlar şunlardır anlaşma.

> [!NOTE]
> Bu sayfa kapsar X12 özellikleri Azure mantıksal uygulamaları için. Daha fazla bilgi için bkz: [EDIFACT](logic-apps-enterprise-integration-edifact.md).

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-accounts.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili
* En az iki [ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) , tümleştirme hesabınızda tanımlanır ve X12 ile yapılandırılmış altında tanımlayıcı **iş kimlikleri**    
* Gerekli [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) için karşıya yükleme için [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-accounts.md)

Çalıştırdıktan sonra [tümleştirme hesap oluşturmak](../logic-apps/logic-apps-enterprise-integration-accounts.md), [ortakları eklemek](logic-apps-enterprise-integration-partners.md)ve bir [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) kullanmak istiyorsanız, bir X12 oluşturabileceğiniz aşağıdaki adımları izleyerek anlaşma.

## <a name="create-an-x12-agreement"></a>Bir X12 oluşturma Sözleşmesi

1.  [Azure portalı](http://portal.azure.com "Azure portalı") oturumunu açın. Sol menüden seçin **tüm hizmetleri**. 

    > [!TIP]
    > Görmüyorsanız, **tüm hizmetleri**, menü ilk genişletmeniz gerekebilir. Daraltılmış menüsünün üstünde seçin **Göster menüsü**.

    ![Sol menüsünde "Tüm hizmetleri" öğesini seçin](./media/logic-apps-enterprise-integration-x12/account-1.png)

2.  Arama kutusuna "tümleştirme", filtre olarak yazın. Sonuçlar listesinde **tümleştirme hesapları**.  

    ![Filtre "tümleştirme üzerinde", "Tümleştirme hesapları" seçin](./media/logic-apps-enterprise-integration-x12/account-2.png)

3. İçinde **tümleştirme hesapları** açar, dikey penceresinde, anlaşmayı eklemek istediğiniz tümleştirme hesabını seçin.
Herhangi bir tümleştirme hesabı görmüyorsanız, [oluşturmak ilk](../logic-apps/logic-apps-enterprise-integration-accounts.md "tümleştirme hesapları hakkında").

    ![Tümleştirme hesabı anlaşmayı oluşturulacağı konumu seçin](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. Seçin **genel bakış**seçeneğini belirleyip **anlaşmaları** döşeme. Anlaşmaları döşeme yoksa, ilk kutucuğu ekleyin. 

    !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-as2/agreement-1.png)

5. Açılır anlaşmaları dikey penceresinde, seçin **Ekle**.

    !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-as2/agreement-2.png)     

6. Altında **Ekle**, girin bir **adı** sözleşmenizi için. Anlaşma türü için **X12**. Seçin **ana iş ortağı**, **konak kimliği**, **Konuk iş ortağı**, ve **Konuk kimlik** sözleşmenizi için. Daha fazla özellik Ayrıntılar için bu adımı bölümündeki tabloya bakın.

    ![Anlaşma ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Özellik | Açıklama |
    | --- | --- |
    | Ad |Anlaşma adı |
    | Anlaşma türü | X12 olmalıdır |
    | Konak İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Ana bilgisayar iş ortağı sözleşmesi yapılandırır kuruluşu temsil eder. |
    | Konak Kimliği |Ana makine ortak bir tanımlayıcı |
    | Konuk İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konuk iş ortağı ana iş ortağı iş yapmakta kuruluşu temsil eder. |
    | Konuk Kimliği |Konuk iş ortağı için bir tanımlayıcı |
    | Ayarları Al |Bu özellikleri tüm anlaşmanın tarafından alınan iletileri için geçerlidir. |
    | Gönderme Ayarları |Bu özellikleri anlaşmanın tarafından gönderilen tüm iletiler için geçerlidir. |  

  > [!NOTE]
  > Gönderen Niteleyici Tanımlayıcısı ve alıcı niteleyicisi ve iş ortakları ve gelen ileti içinde tanımlanan bir tanımlayıcı eşleşen anlaşma bağlıdır X12 çözünürlüğü. Bu değerler için ortağınız değiştirirseniz, anlaşmayı çok güncelleştirin.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Anlaşma tanıtıcıları iletileri nasıl alınan yapılandırma

Anlaşma özelliklerini ayarladıysanız, bu anlaşmanın nasıl tanımlar ve bu anlaşma ile ortaktan alınan gelen iletileri işleme yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **alma ayarı**.
İletiler birlikte alış verişleri iş ortağı ile sözleşmenizde göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümdeki tablolarda bakın.

    **Alma ayarlarını** şu bölümlere düzenlenmiştir: tanımlayıcılar, bildirim, şemaları, zarflar, denetim numaraları, doğrulama ve iç ayarlar.

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık sözleşmenizi seçili ayarlarınızı uygun gelen iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

![Tanımlayıcı özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Özellik | Açıklama |
| --- | --- |
| ISA1 (Yetkilendirme Niteleyicisi) |Yetkilendirme niteleyicisi değerini aşağı açılan listeden seçin. |
| ISA2 |İsteğe bağlı. Yetkilendirme bilgileri değeri girin. ISA1 için girilen değer dışında 00 ise, en az bir alfasayısal karakter ve en fazla 10 girin. |
| ISA3 (Güvenlik Niteleyicisi) |Güvenlik niteleyicisi değerini aşağı açılan listeden seçin. |
| ISA4 |İsteğe bağlı. Güvenlik bilgileri değeri girin. ISA3 için girilen değer dışında 00 ise, en az bir alfasayısal karakter ve en fazla 10 girin. |

### <a name="acknowledgment"></a>Bildirim

![Bildirim özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Özellik | Açıklama |
| --- | --- |
| Beklenen TA1 |Teknik bir bildirim değişim gönderene döndürür |
| Beklenen FA |İşlev bildirim değişim gönderene döndürür. Ardından, şema sürümlerine göre 997 veya 999 bildirimler isteyip istemediğinizi seçin |
| AK2/IK2 döngü içerir |Kabul edilen işlem kümeleri için işlevsel bildirimler AK2 döngüler nesil sağlar |

### <a name="schemas"></a>Şemalar

Her işlem türü (ST1) ve gönderen uygulama (GS2) için bir şema seçin. Alma ardışık düzen ST1 ve GS2 için burada ayarladığınız gelen iletide gelen ileti şema şema ve burada ayarladığınız değerler ile eşleşen değerleri tarafından gelen ileti ayrıştırır.

![Şema seçin](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Özellik | Açıklama |
| --- | --- |
| Sürüm |Sürümünü X12 seçin |
| İşlem Türü (ST01) |İşlem türü seçin |
| Gönderen Uygulama (GS02) |Gönderen uygulama seçin |
| Şema |Kullanmak istediğiniz şema dosyası seçin. Şemalar tümleştirme hesabınıza eklenir. |

> [!NOTE]
> Gerekli yapılandırma [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) için karşıya, [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Zarflar

![Bir işlem kümesinde ayırıcı belirtin: Standart tanımlayıcı veya yineleme ayırıcı seçin](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Özellik | Açıklama |
| --- | --- |
| ISA11 Kullanımı |Bir işlem kümesinde kullanılacak ayırıcı belirtir: <p>Seçin **standart tanımlayıcı** ondalık gösterimini bir nokta (.) kullanmak için EDI gelen belgede ondalık gösterimindeki yerine ardışık düzen alırsınız. <p>Seçin **yineleme ayırıcı** basit veri öğesi veya yinelenen veri yapısı yinelenen oluşumları ayırıcı belirtmek için. Örneğin, genellikle (^) ayar yineleme ayırıcı olarak kullanılır. HIPAA şemaları için ayar yalnızca kullanabilirsiniz. |

### <a name="control-numbers"></a>Denetim Numaraları

![Denetim sayı çoğaltmaları nasıl ele alınacağını seçin](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Özellik | Açıklama |
| --- | --- |
| Değişim kontrol numarası çoğaltmaları izin verme |Yinelenen etkileşimler engelleyin. Değişim Denetimi numarasını (ISA13) alınan Değişim Denetimi numarası denetler. Bir eşleşme algılanırsa, alma ardışık düzen değişim işlemiyor. Onay için bir değer vererek gerçekleştirmek için gün sayısını belirtebilirsiniz *denetlemek için yinelenen ISA13 her (gün)*. |
| Yinelenen Grup denetim numaralarına izin verme |Blok yinelenen grup denetimi numaralarıyla interchanges. |
| Yinelenen İşlem kümesi denetim numaralarına izin verme |Blok yinelenen işlem kümesi denetim numaralarıyla interchanges. |

### <a name="validations"></a>Doğrulamalar

![Alınan iletilerin doğrulama özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kuralın belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| Mesaj Türü |EDI ileti türünü seçin. |
| EDI Doğrulaması |Şema EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılar tarafından tanımlanan veri türleri üzerinde EDI doğrulaması gerçekleştirir. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/max) izin verilir. |
| Başta/Sonda Sıfırlara İzin Ver |Başında veya sonunda sıfır ek korumak ve karakterleri boşluk. Bu karakterleri kaldırmayın. |
| Baştaki/Sondaki Sıfırları Kırp |Baştaki veya sondaki sıfır ve boşluk karakterlerini Kaldır. |
| Sonda Ayırıcı İlkesi |Sondaki ayırıcılar oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve ayırıcı olarak alınan değişim önlemek için. Değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, değişim geçerli bildirilmedi. <p>Seçin **isteğe bağlı** etkileşimler ile veya olmadan sonundaki sınırlayıcılar ve ayırıcılar kabul etmek için. <p>Seçin **zorunlu** zaman değişim olmalıdır sonundaki sınırlayıcılar ve ayırıcılar. |

### <a name="internal-settings"></a>İç Ayarlar

![İç ayarlarını seçin](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Özellik | Açıklama |
| --- | --- |
| Kapsanan ondalık biçiminde "Nn" temel 10 sayısal değerine dönüştürür |10 tabanında sayısal bir değer biçimine "Nn" ile belirtilen bir EDI sayıyı dönüştürür |
| Sondaki ayırıcılara izin veriliyorsa boş XML etiketleri oluştur |Sondaki ayırıcılar boş XML etiketleri dahil değişim gönderen sağlamak için bu onay kutusunu seçin. |
| Değişimi işlem kümeleri olarak böl; hata durumunda işlem kümelerini askıya al|İşlem kümesine uygun Zarf uygulayarak ayrı bir XML belge içine bir değişim kümesindeki her bir işlem ayrıştırır. Yalnızca hareketleri burada doğrulama başarısız askıya alır. |
| Değişimi işlem kümeleri olarak böl; hata durumunda değişimi askıya al|Uygun Zarf uygulayarak ayrı bir XML belge içine bir değişim kümesindeki her bir işlem ayrıştırır. Değişim bir veya daha fazla işlem kümelerinde doğrulama başarısız olduğunda tüm değişim askıya alır. | 
| Değişim korumak - işlem kümeleri askıya alma hatası |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Diğer tüm işlem kümeleri işlemeye devam ederken doğrulama, başarısız olan işlem kümeleri askıya alır. |
| Değişimi koru; hata durumunda değişimi askıya al |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Değişim bir veya daha fazla işlem kümelerinde doğrulama başarısız olduğunda tüm değişim askıya alır. |

## <a name="configure-how-your-agreement-sends-messages"></a>Nasıl sözleşmenizi iletileri gönderir yapılandırın

Bu anlaşma nasıl tanımlar ve bu anlaşma ile iş ortağınız göndermek giden iletilerini işleme yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **gönderme ayarları**.
İletileri ile alış verişleri ortağınızla birlikte sözleşmenize göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümdeki tablolarda bakın.

    **Gönderme ayarları** şu bölümlere düzenlenmiştir: tanımlayıcılar, bildirim, şemalar, zarflar, karakter kümeleri ve ayırıcılar, denetim numaraları ve doğrulama.

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık sözleşmenizi seçili ayarlarınızı uygun giden iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

![Tanımlayıcı özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Özellik | Açıklama |
| --- | --- |
| Yetkilendirme niteleyicisi (ISA1) |Yetkilendirme niteleyicisi değerini aşağı açılan listeden seçin. |
| ISA2 |Yetkilendirme bilgileri değeri girin. Bu değer dışında 00 ise, en az bir alfasayısal karakter ve en fazla 10 girin. |
| Güvenlik niteleyicisi (ISA3) |Güvenlik niteleyicisi değerini aşağı açılan listeden seçin. |
| ISA4 |Güvenlik bilgileri değeri girin. Bu değer, değer (ISA4) metin kutusu için 00 dışında ise bir alfasayısal değer en az ve en fazla 10 girin. |

### <a name="acknowledgment"></a>Bildirim

![Bildirim özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Özellik | Açıklama |
| --- | --- |
| Beklenen TA1 |Teknik bir bildirim (TA1) değişim göndericiye. Bu ayar, iletiyi göndermeyi konak ortağı Sözleşmesi'nin Konuk ortaktan bir bildirim istekleri belirtir. Bu bildirimler Sözleşmesi'nin alma ayarlarınızı temel alan ana bilgisayar ortağı tarafından beklenir. |
| Beklenen FA |İşlev bildirim (FA) değişim göndericiye. Birlikte çalıştığınız şema sürümlerine göre 997 veya 999 onayları isteyip istemediğinizi seçin. Bu bildirimler Sözleşmesi'nin alma ayarlarınızı temel alan ana bilgisayar ortağı tarafından beklenir. |
| FA Sürümü |FA sürümü seçin |

### <a name="schemas"></a>Şemalar

![Kullanılacak şema seçin](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Özellik | Açıklama |
| --- | --- |
| Sürüm |Sürümünü X12 seçin |
| İşlem Türü (ST01) |İşlem türü seçin |
| SCHEMA |Kullanılacak şema seçin. Şemalar tümleştirme hesabınızda bulunur. Şema ilk seçerseniz, otomatik olarak sürüm ve işlem yapılandırır türü  |

> [!NOTE]
> Gerekli yapılandırma [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) için karşıya, [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Zarflar

![Bir işlem kümesinde ayırıcı belirtin: Standart tanımlayıcı veya yineleme ayırıcı seçin](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Özellik | Açıklama |
| --- | --- |
| ISA11 Kullanımı |Bir işlem kümesinde kullanılacak ayırıcı belirtir: <p>Seçin **standart tanımlayıcı** ondalık gösterimini bir nokta (.) kullanmak için EDI gelen belgede ondalık gösterimindeki yerine ardışık düzen alırsınız. <p>Seçin **yineleme ayırıcı** basit veri öğesi veya yinelenen veri yapısı yinelenen oluşumları ayırıcı belirtmek için. Örneğin, genellikle (^) ayar yineleme ayırıcı olarak kullanılır. HIPAA şemaları için ayar yalnızca kullanabilirsiniz. |

### <a name="control-numbers"></a>Denetim Numaraları

![Denetim numarası özelliklerini belirtin](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Özellik | Açıklama |
| --- | --- |
| Denetim sürüm numarası (ISA12) |Standart X12 sürümünü seçin |
| Kullanım göstergesi (ISA15) |Bir değişim bağlamında seçin.  Değerler bilgi, üretim verileri veya test verileri |
| Şema |Gönderme ardışık düzene gönderir X12 kodlu bir değişim GS ve ST segment oluşturur |
| GS1 |İsteğe bağlı, aşağı açılan listeden işlevsel kod için bir değer seçin |
| GS2 |İsteğe bağlı, uygulama gönderen |
| GS3 |İsteğe bağlı, uygulama alıcı |
| GS4 |İsteğe bağlı, select CCYYMMDD ya da YYAAGG |
| GS5 |İsteğe bağlı, select SSDD, SSDDSS veya HHMMSSdd |
| GS7 |İsteğe bağlı, aşağı açılan listeden sorumlu aracısı için bir değer seçin |
| GS8 |İsteğe bağlı, belgenin sürümü |
| Değiş tokuş denetim numarası (ISA13) |Gerekli değerleri aralığı Değişim Denetimi numarası girin. En az 1 ve en fazla 999999999 olan sayısal bir değer girin |
| Group Control Number (GS06) |Gerekli bir dizi numarası grubu denetim numarası girin. En az 1 ve en fazla 999999999 olan sayısal bir değer girin |
| İşlem kümesi denetim numarasını (ST02) |Gerekli bir dizi numarası işlem kümesi denetim numarası girin. Sayısal değerleri en az 1 ve en fazla 999999999 olan bir aralığı girin |
| Alan kodu |İsteğe bağlı, bildirim içinde kullanılan işlem kümesi denetim numaraları aralığını için belirlenmiş. (İsteniyorsa) için önek ve sonek alanlara Orta iki alanlar için sayısal bir değer ve alfasayısal bir değer girin. Orta alanları gereklidir ve denetim sayısı için minimum ve maksimum değerleri içeren |
| Sonek |İsteğe bağlı, bir bildirim kullanılan işlem kümesi denetim numaraları aralığını için belirlenmiş. (İsteniyorsa) için önek ve sonek alanlara Orta iki alanlar için sayısal bir değer ve alfasayısal bir değer girin. Orta alanları gereklidir ve denetim sayısı için minimum ve maksimum değerleri içeren |

### <a name="character-sets-and-separators"></a>Karakter Kümeleri ve Ayırıcılar

Karakter kümesi dışında her ileti türü için farklı bir sınırlayıcı kümesini girebilirsiniz. Karakter kümesi için belirli bir ileti şeması belirtilmezse, varsayılan karakter kümesini kullanılır.

![İleti türleri için sınırlayıcılar belirtin](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Özellik | Açıklama |
| --- | --- |
| Kullanılacak karakter |Özellikleri Seç X12 karakter kümesi doğrulanacak. , Temel, genişletilmiş ve UTF8 seçenekleridir. |
| Şema |Bir şema aşağı açılan listeden seçin. Her satır tamamladıktan sonra yeni bir satır otomatik olarak eklenir. Seçili şeması için aşağıdaki ayırıcı açıklamalarına göre kullanmak istediğiniz ayırıcıları kümesi seçin. |
| Giriş Türü |Bir input type aşağı açılan listeden seçin. |
| Bileşen Ayırıcısı |Bileşik veri öğeleri ayırmak için tek bir karakter girin. |
| Veri Öğesi Ayırıcısı |Bileşik veri öğeleri içinde basit veri öğeleri ayırmak için tek bir karakter girin. |
| Değiştirme karakteri |Giden X12 oluştururken yük verilerinin tüm ayırıcı karakter değiştirmek için kullanılan bir değiştirme karakteri girin ileti. |
| Segment Sonlandırıcı |EDI segmentinin sonunu belirtmek için tek bir karakter girin. |
| Sonek |Segment tanımlayıcısı ile kullanılan karakteri seçin. Sonek belirlerseniz, segment Sonlandırıcı veri öğesi boş olamaz. Segment sonlandırıcı boş bırakılırsa, sonek atamanız gerekir. |

> [!TIP]
> Özel karakter değerlerini sağlamak için JSON olarak anlaşma düzenleyin ve özel karakter için bu ASCII değeri belirtin.

### <a name="validation"></a>Doğrulama

![İleti göndermek için doğrulama özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kuralın belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| Mesaj Türü |EDI ileti türünü seçin. |
| EDI Doğrulaması |Şema EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılar tarafından tanımlanan veri türleri üzerinde EDI doğrulaması gerçekleştirir. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/max) izin verilir. |
| Başta/Sonda Sıfırlara İzin Ver |Başında veya sonunda sıfır ek korumak ve karakterleri boşluk. Bu karakterleri kaldırmayın. |
| Baştaki/Sondaki Sıfırları Kırp |Baştaki veya sondaki sıfır karakterleri kaldırın. |
| Sonda Ayırıcı İlkesi |Sondaki ayırıcılar oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve ayırıcı olarak gönderilen değişim önlemek için. Değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, değişim geçerli bildirilmedi. <p>Seçin **isteğe bağlı** etkileşimler ile veya olmadan sonundaki sınırlayıcılar ve ayırıcılar göndermek için. <p>Seçin **zorunlu** gönderilen değişim sonundaki sınırlayıcılar ve ayırıcıları kullanmanız gerekiyorsa. |

## <a name="find-your-created-agreement"></a>Oluşturulan sözleşmenizi Bul

1.  Üzerinde anlaşma özelliklerinizi ayarlama işlemini tamamladıktan sonra **Ekle** dikey penceresinde, seçin **Tamam** sözleşmenizi oluşturmayı tamamlamak ve tümleştirme hesabı dikey penceresine geri dönün.

    Yeni eklenen sözleşmenizi artık görünür, **anlaşmaları** listesi.

2.  Ayrıca, tümleştirme hesabına genel bakış sözleşmelerinizi görüntüleyebilirsiniz. Tümleştirme hesabı dikey penceresinde, seçin **genel bakış**seçeneğini belirleyip **anlaşmaları** döşeme.

    ![Tüm anlaşmalar görüntülemek için "Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/x12/). 

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  

