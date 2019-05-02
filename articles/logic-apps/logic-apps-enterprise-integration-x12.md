---
title: X12 iletileri B2B Kurumsal tümleştirme - Azure Logic Apps | Microsoft Docs
description: Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme için EDI biçiminde Exchange X12 iletileri
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.date: 01/31/2017
ms.openlocfilehash: f06e213dbae31c9d7c4e212d605cc962aba71d2d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64728750"
---
# <a name="exchange-x12-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme için Exchange X12 iletileri

X12 değiştirebilir önce iletileri Azure Logic Apps için x X12 oluşturmalısınız sözleşmesi ve bu anlaşma tümleştirme hesabınızda depolayın. X X12 oluşturmak için adımlar şunlardır sözleşme.

> [!NOTE]
> Azure Logic Apps bu sayfa kapsar X12 özellikleri. Daha fazla bilgi için [EDIFACT](logic-apps-enterprise-integration-edifact.md).

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili
* En az iki [iş ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) , tümleştirme hesabınızdaki tanımlanır ve X12 ile yapılandırılmış altında tanımlayıcı **iş kimlikleri**    
* Gerekli bir [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) , tümleştirme hesabınıza yükleyebilirsiniz.

Çalıştırdıktan sonra [tümleştirme hesabı oluşturma](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), [iş ortakları ekleme](logic-apps-enterprise-integration-partners.md)ve bir [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) kullanmak istiyorsanız, x X12 oluşturabilmeniz için aşağıdaki adımları izleyerek sözleşme.

## <a name="create-an-x12-agreement"></a>X X12 oluşturma Sözleşmesi

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. 

2. Azure ana menüsünden seçin **tüm hizmetleri**. 
   Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.  

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-x12/account-1.png)

   > [!TIP]
   > Varsa **tüm hizmetleri** görünmüyorsa, menü öğesine sahip olabilir. Daraltılmış menüsünün üstünde, seçin **Göster menüsü**.

3. Altında **tümleştirme hesapları**, anlaşmayı eklemek istediğiniz tümleştirme hesabı'nı seçin.

   ![Tümleştirme hesabı sözleşmesi oluşturulacağı konumu seçin](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. Seçin **genel bakış**, ardından **sözleşmeleri** Döşe. 
   Anlaşmaları kutucuk yoksa kutucuk önce ekleyin. 

   !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-x12/agreement-1.png)

5. Altında **sözleşmeleri**, seçin **Ekle**.

   !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-x12/agreement-2.png)     

6. Altında **Ekle**, girin bir **adı** sözleşmenize için. 
   Sözleşme türü için **X12**. 
   Seçin **konak iş ortağı**, **konak kimliği**, **Konuk iş ortağı**, ve **Konuk kimlik** sözleşmenize için. 
   Daha fazla özellik Ayrıntılar için bu adımda tabloya bakın.

    ![Anlaşma ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Özellik | Açıklama |
    | --- | --- |
    | Ad |Anlaşma adı |
    | Sözleşme Türü | X12 olmalıdır |
    | Konak İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konak iş ortağı sözleşmesi yapılandıran kuruluşu temsil eder. |
    | Konak Kimliği |Konak iş ortağı için bir tanımlayıcı |
    | Konuk İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konuk iş ortağı, konak iş ortağı iş yapmakta kuruluşu temsil eder. |
    | Konuk Kimliği |Konuk iş ortağı için bir tanımlayıcı |
    | Ayarları Al |Bu özellikler, bir anlaşma tarafından alınan tüm iletileri için geçerlidir. |
    | Gönderme Ayarları |Bu özellikler, bir anlaşma tarafından gönderilen tüm iletiler için geçerlidir. |  

   > [!NOTE]
   > Gönderen niteleyicisi tanımlayıcısı ve alıcı niteleyicisi ve iş ortağı ve gelen ileti tanımlanmış tanımlayıcısı eşleşen anlaşma bağlıdır X12 çözünürlüğü. Bu değerler, iş ortağı değiştirirseniz, anlaşma güncelleştirirsiniz.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Nasıl iletileri, anlaşma tanıtıcıları alınan yapılandırma

Sözleşme özelliklerini ayarladıysanız, işbu sözleşme nasıl tanımlar ve işbu sözleşme aracılığıyla iş ortağınız alınan gelen iletileri işleyen yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **alma ayarı**.
Sizinle iletiler birbiriyle değiştirir iş ortaklarıyla sözleşmenize göre bu özelliklerini yapılandırın. Bu bölümdeki tablolarda, özellik açıklamalarına bakın.

    **Alma ayarlarını** bu bölümler halinde düzenlenmiştir: Tanımlayıcılar, bildirim, şemalar, zarflar, denetim numaraları, doğrulama ve iç ayarlar.

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık, anlaşmanız seçili ayarlarınıza uygun gelen iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

![Tanımlayıcı özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Özellik | Açıklama |
| --- | --- |
| ISA1 (Yetkilendirme Niteleyicisi) |Yetkilendirme niteleyicisi değeri, açılır listeden seçin. |
| ISA2 |İsteğe bağlı. Yetkilendirme bilgileri değeri girin. Isa1 için girdiğiniz değer 00 dışında ise, en az bir alfasayısal karakter ve en fazla 10 girin. |
| ISA3 (Güvenlik Niteleyicisi) |Güvenlik niteleyicisi değeri, açılır listeden seçin. |
| ISA4 |İsteğe bağlı. Güvenlik bilgileri değeri girin. Isa3 için girdiğiniz değer 00 dışında ise, en az bir alfasayısal karakter ve en fazla 10 girin. |

### <a name="acknowledgment"></a>Bildirim

![Bildirim özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Özellik | Açıklama |
| --- | --- |
| Beklenen TA1 |Teknik bir bildirim değişim gönderene döndürür |
| Beklenen FA |İşlev bildirim değişim gönderene döndürür. Ardından şema sürümüne dayanan 997 veya 999 bildirimler isteyip istemediğinizi seçin |
| AK2/ık2 döngüsünü dahil |Kabul edilen işlem kümeleri için işlev bildirimleri AK2 döngüler oluşturulmasını sağlar |

### <a name="schemas"></a>Şemalar

Her bir işlem türüne (ST1) ve gönderen uygulamaya (GS2) için bir şema seçin. Alma işlem hattı, burada ayarladığınız içeren gelen ileti Burada ayarlanan değerleri ve şema gelen ileti şemasıyla değerlerini ST1 ve GS2 eşleştirerek gelen ileti ayrıştırır.

![Şema seçin](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Özellik | Açıklama |
| --- | --- |
| Version |Sürümünü X12 seçin |
| İşlem Türü (ST01) |İşlem türü seçin |
| Gönderen Uygulama (GS02) |Gönderen uygulamayı seçin |
| Şema |Kullanmak istediğiniz şema dosyası seçin. Şemaları tümleştirme hesabınıza eklenir. |

> [!NOTE]
> Gerekli yapılandırma [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) için karşıya, [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Zarflar

![Bir işlem kümesinde ayırıcı belirtin: Standart tanımlayıcı veya yineleme ayırıcısı seçin](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Özellik | Açıklama |
| --- | --- |
| ISA11 Kullanımı |İşlem kümesi içinde kullanılacak ayırıcı belirtir: <p>Seçin **standart tanımlayıcı** için ondalık gösterim bir nokta (.) kullanmak için EDI gelen belgenin ondalık gösterim yerine işlem hattını alırsınız. <p>Seçin **yineleme ayırıcısı** basit veri öğesi veya bir yinelenen veri yapısı yinelenen örnekleri için ayırıcı belirtmek için. Örneğin, genellikle simgeyi seçtiğinizde (^) yineleme ayırıcısı olarak kullanılır. HIPAA şemaları için simgeyi seçtiğinizde yalnızca kullanabilirsiniz. |

### <a name="control-numbers"></a>Denetim Numaraları

![Denetim numaralarına ne yapılacağını seçin](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Özellik | Açıklama |
| --- | --- |
| Değişim denetim numarası yinelenmesine izin verme |Yinelenen değişim engelleyin. Değişim denetim numarası (ısa13) alınan değişim denetim numarası için denetler. Alma işlem hattı, bir eşleşme algılanırsa, değişim işlemiyor. Onay için bir değer vererek gerçekleştirmek için gün sayısı belirtebilirsiniz *yinelenen ısa13 denetleme sıklığı (gün)*. |
| Yinelenen Grup denetim numaralarına izin verme |Yinelenen grup denetim numaraları ile blok değişimlerinin. |
| Yinelenen İşlem kümesi denetim numaralarına izin verme |Yinelenen işlem kümesi denetim numaraları ile blok değişimlerinin. |

### <a name="validations"></a>Doğrulamalar

![Alınan iletiler için doğrulama özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| Mesaj Türü |EDI ileti türü seçin. |
| EDI Doğrulaması |EDI doğrulaması şema EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılara tarafından tanımlanan veri türleri üzerinde gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/maks) izin verilir. |
| Başta/Sonda Sıfırlara İzin Ver |Başta veya sonda sıfır ek korumak ve boşluk karakterleri. Bu karakterleri kaldırın. |
| Baştaki/Sondaki Sıfırları Kırp |Baştaki veya sondaki ve boşluk karakterleri kaldırın. |
| Sonda Ayırıcı İlkesi |Sondaki ayırıcılara oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve alınan değişim ayırıcılar önlemek için. Değişim, değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, geçerli bildirilmedi. <p>Seçin **isteğe bağlı** değişim ile veya sonundaki sınırlayıcılar ve ayırıcılar olmadan kabul etmek için. <p>Seçin **zorunlu** zaman değişim olmalıdır sonundaki sınırlayıcılar ve ayırıcı. |

### <a name="internal-settings"></a>İç Ayarlar

![İç ayarlarını seçin](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Özellik | Açıklama |
| --- | --- |
| Varsayılan ondalık biçimdeki "Nn" taban 10 sayısal bir değere Dönüştür |10 tabanında sayısal bir değer biçimine "Nn" ile belirtilen bir EDI sayı dönüştürür |
| Sondaki ayırıcılara izin veriliyorsa boş XML etiketleri oluştur |Sondaki ayırıcılara boş XML etiketleri dahil değişim gönderen sağlamak için bu onay kutusunu seçin. |
| Değişimi işlem kümeleri olarak böl; hata durumunda işlem kümelerini askıya al|Her işlem işlem kümesine uygun Zarf uygulayarak bir değişimin ayrı bir XML belgesine kümesi ayrıştırır. Yalnızca, burada doğrulama başarısız askıya alır. |
| Değişimi işlem kümeleri olarak böl; hata durumunda değişimi askıya al|Uygun Zarf uygulayarak bir değişim ayrı bir XML belgesine kümesindeki her bir işlem ayrıştırır. Bir veya daha fazla işlem kümeleri değişim doğrulama başarısız olduğunda tüm değişimi askıya alır. | 
| Değişimi Koru - hata işlem kümelerini Askıya Al |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Diğer tüm işlem kümeleri işlenmeye devam ederken doğrulama, başarısız olan işlem kümelerini askıya alır. |
| Değişimi koru; hata durumunda değişimi askıya al |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Bir veya daha fazla işlem kümeleri değişim doğrulama başarısız olduğunda tüm değişim askıya alır. |

## <a name="configure-how-your-agreement-sends-messages"></a>Sözleşmenize iletileri nasıl göndereceğini yapılandırın

İşbu sözleşme nasıl tanımlar ve iş ortağınız aracılığıyla bu anlaşma Gönder giden iletileri işleyen yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **gönderme ayarları**.
Sizinle iletiler değiştirir ortağınızla birlikte sözleşmenize göre bu özelliklerini yapılandırın. Bu bölümdeki tablolarda, özellik açıklamalarına bakın.

    **Gönderme ayarları** bu bölümler halinde düzenlenmiştir: Tanımlayıcılar, bildirim, şemalar, zarflar, karakter kümeleri ve ayırıcılar, denetim numaraları ve doğrulama.

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık, anlaşmanız seçili ayarlarınıza uygun giden iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

![Tanımlayıcı özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Özellik | Açıklama |
| --- | --- |
| Yetkilendirme niteleyicisi (ısa1) |Yetkilendirme niteleyicisi değeri, açılır listeden seçin. |
| ISA2 |Yetkilendirme bilgileri değeri girin. Bu değer, 00 dışında ise, en az bir alfasayısal karakter ve en fazla 10 girin. |
| Güvenlik niteleyicisi (ısa3) |Güvenlik niteleyicisi değeri, açılır listeden seçin. |
| ISA4 |Güvenlik bilgileri değeri girin. Bu değer, değer (ISA4) metin kutusuna, 00 dışında ise alfasayısal bir değer en az ve en fazla 10 girin. |

### <a name="acknowledgment"></a>Bildirim

![Bildirim özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Özellik | Açıklama |
| --- | --- |
| Beklenen TA1 |Teknik bir bildirim (TA1) değişim göndericiye. Bu ayar, kimin ileti gönderilirken konak iş ortağı Konuk iş ortağı Sözleşmesi'nden bir bildirim istekleri belirtir. Bu bildirimler, sözleşmenin alma ayarlara göre konak iş ortağı tarafından beklenir. |
| Beklenen FA |Bir işlev bildirimi (FA) değişim göndericiye. Çalıştığınız şema sürümlerine göre 997 veya 999 onayları isteyip istemediğinizi seçin. Bu bildirimler, sözleşmenin alma ayarlara göre konak iş ortağı tarafından beklenir. |
| FA Sürümü |FA sürümü seçin |

### <a name="schemas"></a>Şemalar

![Kullanılacak şeması seçin](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Özellik | Açıklama |
| --- | --- |
| Version |Sürümünü X12 seçin |
| İşlem Türü (ST01) |İşlem türü seçin |
| ŞEMA |Kullanılacak şemayı seçin. Şemaları tümleştirme hesabınızdaki yer alır. Şema ilk seçerseniz, otomatik olarak sürüm ve işlem yapılandırır türü  |

> [!NOTE]
> Gerekli yapılandırma [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md) için karşıya, [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Zarflar

![Bir işlem kümesinde ayırıcı belirtin: Standart tanımlayıcı veya yineleme ayırıcısı seçin](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Özellik | Açıklama |
| --- | --- |
| ISA11 Kullanımı |İşlem kümesi içinde kullanılacak ayırıcı belirtir: <p>Seçin **standart tanımlayıcı** için ondalık gösterim bir nokta (.) kullanmak için EDI gelen belgenin ondalık gösterim yerine işlem hattını alırsınız. <p>Seçin **yineleme ayırıcısı** basit veri öğesi veya bir yinelenen veri yapısı yinelenen örnekleri için ayırıcı belirtmek için. Örneğin, genellikle simgeyi seçtiğinizde (^) yineleme ayırıcısı olarak kullanılır. HIPAA şemaları için simgeyi seçtiğinizde yalnızca kullanabilirsiniz. |

### <a name="control-numbers"></a>Denetim Numaraları

![Denetim numarası özelliklerini belirtin](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Özellik | Açıklama |
| --- | --- |
| Denetim Sürüm Numarası (ISA12) |Standart X12 sürümünü seçin |
| Kullanım göstergesi (ısa15) |Bir değişim bağlamını seçin.  Değerleri bilgileri, üretim verisi veya test verileri |
| Şema |Gönderme işlem hattına gönderir X12 kodlu bir değişim için GS ve ST kesimleri oluşturur |
| GS1 |İsteğe bağlı, aşağı açılan listeden işlev kodu için bir değer seçin |
| GS2 |İsteğe bağlı, uygulama gönderen |
| GS3 |İsteğe bağlı, uygulama alıcı |
| GS4 |İsteğe bağlı, select CCYYMMDD veya YYAAGG |
| GS5 |İsteğe bağlı, select HHMM, SSDDSS veya HHMMSSdd |
| GS7 |İsteğe bağlı, aşağı açılan listeden sorumlu aracısı için bir değer seçin |
| GS8 |İsteğe bağlı, belgenin sürümü |
| Değişim denetim numarası (ısa13) |Gerekli, değişim denetim numarası için değer aralığını girin. En az 1 ve en fazla 999999999 sayısal bir değer girin |
| Grup denetim numarası (GS06) |Gerekli bir dizi numarası için Grup denetim numarası girin. En az 1 ve en fazla 999999999 sayısal bir değer girin |
| İşlem kümesi denetim numarası (ST02) |Gerekli işlem kümesi denetim numarası için bir dizi numarası girin. En az 1 ve en fazla 999999999 sayısal değer aralığını girin |
| Alan kodu |İsteğe bağlı, kullanılan bildirim içinde işlem kümesi denetim numaraları aralığını atanmış. Alfasayısal bir değer ve bir sayısal değer iki Orta alanlar için önek ve sonek alanlar için (istenirse) girin. Orta alanlar gereklidir ve denetim numarası için minimum ve maksimum değerler içeriyor |
| Sonek |İsteğe bağlı, atanmış bir bildirim kullanılan işlem kümesi denetim numaraları aralığını. Alfasayısal bir değer ve bir sayısal değer iki Orta alanlar için önek ve sonek alanlar için (istenirse) girin. Orta alanlar gereklidir ve denetim numarası için minimum ve maksimum değerler içeriyor |

### <a name="character-sets-and-separators"></a>Karakter Kümeleri ve Ayırıcılar

Karakter kümesi dışında her ileti türü için farklı bir sınırlayıcı kümesi girebilirsiniz. Ardından, bir karakter kümesi için belirli bir ileti şema belirtilmezse, varsayılan karakter kümesi kullanılır.

![Sınırlayıcılar için ileti türlerini belirtin](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Özellik | Açıklama |
| --- | --- |
| Kullanılacak karakter |Özellikleri Seç X12 karakter kümesini doğrulamak için. Temel ve genişletilmiş UTF8 seçeneklerdir. |
| Şema |Bir şema, aşağı açılan listeden seçin. Her satır tamamladıktan sonra yeni bir satır otomatik olarak eklenir. Seçili şemayı için aşağıdaki ayırıcı açıklamalarına göre kullanmak istediğiniz ayırıcılar kümesi seçin. |
| Giriş Türü |Bir giriş türü açılan listesinden seçin. |
| Bileşen Ayırıcısı |Bileşik veri öğeleri ayırmak için tek bir karakter girin. |
| Veri Öğesi Ayırıcısı |Bileşik veri öğeleri içinde basit veri öğeleri ayırmak için tek bir karakter girin. |
| Değiştirme karakteri |Giden X12 oluştururken tüm ayırıcı karakterlerler yükünün verilerindeki değiştirerek için kullanılan bir değiştirme karakteri girin ileti. |
| Segment Sonlandırıcı |EDI segment sonunu belirtmek için tek bir karakter girin. |
| Sonek |Segment tanımlayıcısıyla kullanılan karakter seçin. Bir sonek belirlerseniz, segment Sonlandırıcı veri öğesi boş olabilir. Segment sonlandırıcı boş bırakılırsa, sonek atamanız gerekir. |

> [!TIP]
> Özel karakter değerlerini sağlamak için anlaşmayı JSON olarak Düzenle ve özel karakter için ASCII değeri sağlayın.

### <a name="validation"></a>Doğrulama

![İleti göndermek için doğrulama özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| Mesaj Türü |EDI ileti türü seçin. |
| EDI Doğrulaması |EDI doğrulaması şema EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılara tarafından tanımlanan veri türleri üzerinde gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/maks) izin verilir. |
| Başta/Sonda Sıfırlara İzin Ver |Başta veya sonda sıfır ek korumak ve boşluk karakterleri. Bu karakterleri kaldırın. |
| Baştaki/Sondaki Sıfırları Kırp |Başta veya sonda sıfır karakter kaldırın. |
| Sonda Ayırıcı İlkesi |Sondaki ayırıcılara oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve gönderilen değişim ayırıcılar önlemek için. Değişim, değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, geçerli bildirilmedi. <p>Seçin **isteğe bağlı** değişim ile veya sonundaki sınırlayıcılar ve ayırıcılar olmadan göndermek için. <p>Seçin **zorunlu** gönderilen değişim sonundaki sınırlayıcılar ve ayırıcılar olması gerekiyorsa. |

## <a name="find-your-created-agreement"></a>Oluşturulan sözleşmenize Bul

1.  Üzerinde anlaşma özelliklerinizi ayarlama işlemini tamamladıktan sonra **Ekle** sayfasında **Tamam** sözleşmenize oluşturma işlemini tamamladıktan ve tümleştirme hesabınıza döndürmek için.

    Yeni eklenen sözleşmenize artık görünür, **sözleşmeleri** listesi.

2.  Tümleştirme hesabı genel bakış sözleşmelerinizi da görüntüleyebilirsiniz. Tümleştirme hesabı menüsünde **genel bakış**, ardından **sözleşmeleri** Döşe.

    !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="view-the-swagger"></a>Swagger görüntüleyin
Bkz: [ayrıntıları swagger](/connectors/x12/). 

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Enterprise Integration Pack hakkında daha fazla bilgi](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  

