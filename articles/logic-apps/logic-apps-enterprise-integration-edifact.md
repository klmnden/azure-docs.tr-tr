---
title: "B2B Kurumsal tümleştirme - Azure Logic Apps için EDIFACT iletileri | Microsoft Docs"
description: "Azure Logic Apps ile B2B Kurumsal tümleştirme EDI biçimindeki Exchange EDIFACT iletiler"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/26/2016
ms.author: LADocs; jonfan
ms.openlocfilehash: 68009b74a410f7e854de675a1d8d0c32e310d2c9
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="exchange-edifact-messages-for-enterprise-integration-with-logic-apps"></a>Logic apps ile Kurumsal tümleştirme için Exchange EDIFACT iletileri

Azure mantıksal uygulamaları için EDIFACT iletileri değiştirebilir önce EDIFACT sözleşmesi oluşturun ve bu anlaşma tümleştirme hesabınızı depolamak. EDIFACT sözleşmesi oluşturmak için adımlar şunlardır.

> [!NOTE]
> Bu sayfa, Azure mantıksal uygulamaları için EDIFACT özellikleri kapsar. Daha fazla bilgi için bkz: [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şöyledir:

* Bir [tümleştirme hesabını](../logic-apps/logic-apps-enterprise-integration-accounts.md) , daha önce tanımlanan ve Azure aboneliğinizle ilişkili  
* En az iki [ortakları](logic-apps-enterprise-integration-partners.md) tümleştirme hesabınızda zaten tanımlanmış

> [!NOTE]
> Bir sözleşme oluşturduğunuzda, alan veya ve ortaktan Gönder iletileri içeriği anlaşma türünü eşleşmesi gerekir.

Çalıştırdıktan sonra [tümleştirme hesap oluşturmak](../logic-apps/logic-apps-enterprise-integration-accounts.md) ve [ortakları eklemek](logic-apps-enterprise-integration-partners.md), aşağıdaki adımları izleyerek bir EDIFACT sözleşmesi oluşturabilirsiniz.

## <a name="create-an-edifact-agreement"></a>EDIFACT sözleşmesi oluşturun 

1.  [Azure portalı](http://portal.azure.com "Azure portalı") oturumunu açın. Sol menüden seçin **tüm hizmetleri**.

    > [!TIP]
    > Görmüyorsanız, **tüm hizmetleri**, menü ilk genişletmeniz gerekebilir. Daraltılmış menüsünün üstünde seçin **Göster menüsü**.

    ![Sol menüsünde "Tüm hizmetleri" öğesini seçin](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

2. Arama kutusuna filtreniz için "tümleştirme" yazın. Sonuçlar listesinde **tümleştirme hesapları**.

    ![Filtre "tümleştirme üzerinde", "Tümleştirme hesapları" seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-3.png)

3. İçinde **tümleştirme hesapları** açar, dikey penceresinde, anlaşmayı oluşturmak istediğiniz tümleştirme hesabını seçin.
Herhangi bir tümleştirme hesabı görmüyorsanız, [oluşturmak ilk](../logic-apps/logic-apps-enterprise-integration-accounts.md "tümleştirme hesapları hakkında").  

    ![Tümleştirme hesabı anlaşmayı oluşturulacağı konumu seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Seçin **anlaşmaları** döşeme. Anlaşmaları döşeme yoksa, ilk kutucuğu ekleyin.   

    !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Açılır anlaşmaları dikey penceresinde, seçin **Ekle**.

    !["Ekle" yi seçin](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. Altında **Ekle**, girin bir **adı** sözleşmenizi için. İçin **anlaşma türünü**seçin **EDIFACT**. Seçin **ana iş ortağı**, **konak kimliği**, **Konuk iş ortağı**, ve **Konuk kimlik** sözleşmenizi için.

    ![Anlaşma ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

    | Özellik | Açıklama |
    | --- | --- |
    | Ad |Anlaşma adı |
    | Anlaşma türü | EDIFACT olmalıdır |
    | Konak İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Ana bilgisayar iş ortağı sözleşmesi yapılandırır kuruluşu temsil eder. |
    | Konak Kimliği |Ana makine ortak bir tanımlayıcı |
    | Konuk İş Ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konuk iş ortağı ana iş ortağı iş yapmakta kuruluşu temsil eder. |
    | Konuk Kimliği |Konuk iş ortağı için bir tanımlayıcı |
    | Ayarları Al |Bu özellikleri tüm anlaşmanın tarafından alınan iletileri için geçerlidir. |
    | Gönderme Ayarları |Bu özellikleri anlaşmanın tarafından gönderilen tüm iletiler için geçerlidir. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Anlaşma tanıtıcıları iletileri nasıl alınan yapılandırma

Anlaşma özelliklerini ayarladıysanız, bu anlaşmanın nasıl tanımlar ve bu anlaşma ile ortaktan alınan gelen iletileri işleme yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **alma ayarı**.
İletiler birlikte alış verişleri iş ortağı ile sözleşmenizde göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümdeki tablolarda bakın.

    **Alma ayarlarını** şu bölümlere düzenlenmiştir: tanımlayıcılar, bildirim, şemalar, denetim numaraları, doğrulama ve iç ayarlar.

    !["Alma ayarlarını" yapılandırma](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık sözleşmenizi seçili ayarlarınızı uygun gelen iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

| Özellik | Açıklama |
| --- | --- |
| UNB6.1 (Alıcı Başvuru Parolası) |1 ile 14 arasında karakter arasında alfasayısal bir değer girin. |
| UNB6.2 (Alıcı Başvuru Niteleyicisi) |En az bir karakter ve en çok iki karakter alfasayısal bir değer girin. |

### <a name="acknowledgments"></a>İlgili kaynaklar

| Özellik | Açıklama |
| --- | --- |
| İleti Okundu Bilgisi (CONTRL) |Değişim göndericiye teknik (CONTRL) bildirim için bu onay kutusunu seçin. Bildirim gönderme ayarları anlaşma için temel değişim gönderen gönderilir. |
| Onay (KONTROL) |Bir işlev (CONTRL) bildirim değişim bildirim göndericiye için bu onay kutusunu anlaşma için Gönder ayarlara göre değişim gönderen gönderilir seçin. |

### <a name="schemas"></a>Şemalar

| Özellik | Açıklama |
| --- | --- |
| UNH2.1 (TYPE) |Bir işlem kümesi türü seçin. |
| UNH2.2 (SÜRÜM) |İleti sürüm numarası girin. (En az, bir karakter; maksimum, üç karakter). |
| UNH2.3 (SÜRÜM) |İleti sürüm numarası girin. (En az, bir karakter; maksimum, üç karakter). |
| UNH2.5 (İLİŞKİLİ ATANAN KODU) |Atanan kodu girin. (En büyük, altı karakter. Alfasayısal olmalıdır). |
| UNG2.1 (UYGULAMA GÖNDEREN KİMLİĞİ) |En az bir karakter ve en çok 35 karakter alfasayısal bir değer girin. |
| UNG2.2 (UYGULAMA GÖNDEREN KODU NİTELEYİCİSİ) |En fazla dört karakter alfasayısal bir değer girin. |
| SCHEMA |İlişkili tümleştirme hesabınızdan kullanmak istediğiniz daha önce yüklenen şemasını seçin. |

### <a name="control-numbers"></a>Denetim Numaraları
| Özellik | Açıklama |
| --- | --- |
| Değişim kontrol numarası çoğaltmaları izin verme |Yinelenen etkileşimler engellemek için bu özelliği seçin. Seçili kod çözme EDIFACT eylem alınan değişim Değişim Denetimi numarası (UNB5) daha önce işlenen değiş tokuş denetim numarası eşleşmiyor denetler. Bir eşleşme algılanırsa, değişim işlenmez. |
| Her (days) günde bir UNB5 kopyalarını denetle |Yinelenen değişim denetim numaraları izin vermeyecek şekilde seçerseniz, gün sayısını belirtmek için bu ayarı uygun değere vererek denetimi gerçekleştirmek ne zaman. |
| Yinelenen Grup denetim numaralarına izin verme |Yinelenen grup denetim numaraları (UNG5) ile etkileşimler engellemek için bu özelliği seçin. |
| Yinelenen İşlem kümesi denetim numaralarına izin verme |Yinelenen işlem kümesi denetim numaraları (UNH1) ile etkileşimler engellemek için bu özelliği seçin. |
| EDIFACT Onay Kontrol Numarası |İşlem kümesi referans numaraları kullanmak için bir bildirim belirlemek için önek, bir dizi başvuru numarası ve bir sonek için bir değer girin. |

### <a name="validations"></a>Doğrulamalar

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kuralın belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| Mesaj Türü |EDI ileti türünü seçin. |
| EDI Doğrulaması |Şema EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılar tarafından tanımlanan veri türleri üzerinde EDI doğrulaması gerçekleştirir. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/max) izin verilir. |
| Başta/Sonda Sıfırlara İzin Ver |Başında veya sonunda sıfır ek korumak ve karakterleri boşluk. Bu karakterleri kaldırmayın. |
| Baştaki/Sondaki Sıfırları Kırp |Baştaki veya sondaki sıfır ve boşluk karakterlerini Kaldır. |
| Sonda Ayırıcı İlkesi |Sondaki ayırıcılar oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve ayırıcı olarak alınan değişim önlemek için. Değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, değişim geçerli bildirilmedi. <p>Seçin **isteğe bağlı** etkileşimler ile veya olmadan sonundaki sınırlayıcılar ve ayırıcılar kabul etmek için. <p>Seçin **zorunlu** zaman alınan değişim olmalıdır sonundaki sınırlayıcılar ve ayırıcılar. |

### <a name="internal-settings"></a>İç Ayarlar

| Özellik | Açıklama |
| --- | --- |
| Sondaki ayırıcılara izin veriliyorsa boş XML etiketleri oluştur |Sondaki ayırıcılar boş XML etiketleri dahil değişim gönderen sağlamak için bu onay kutusunu seçin. |
| Değişimi işlem kümeleri olarak böl; hata durumunda işlem kümelerini askıya al|İşlem kümesine uygun Zarf uygulayarak ayrı bir XML belge içine bir değişim kümesindeki her bir işlem ayrıştırır. Doğrulama başarısız işlem kümeleri askıya alın. |
| Değişimi işlem kümeleri olarak böl; hata durumunda değişimi askıya al|Uygun Zarf uygulayarak ayrı bir XML belge içine bir değişim kümesindeki her bir işlem ayrıştırır. Değişim bir veya daha fazla işlem kümelerinde doğrulama başarısız olduğunda tüm değişim askıya alın. | 
| Değişim korumak - işlem kümeleri askıya alma hatası |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Diğer tüm işlem kümeleri işlemeye devam ederken doğrulama, başarısız olan işlem kümeleri askıya alın. |
| Değişimi koru; hata durumunda değişimi askıya al |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Değişim bir veya daha fazla işlem kümelerinde doğrulama başarısız olduğunda tüm değişim askıya alın. |

## <a name="configure-how-your-agreement-sends-messages"></a>Nasıl sözleşmenizi iletileri gönderir yapılandırın

Bu anlaşma nasıl tanımlar ve bu anlaşma ile iş ortaklarınıza göndermek giden iletilerini işleme yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **gönderme ayarları**.
İletileri ile alış verişleri ortağınızla birlikte sözleşmenize göre bu özelliklerini yapılandırın. Özellik açıklamaları için bu bölümdeki tablolarda bakın.

    **Gönderme ayarları** şu bölümlere düzenlenmiştir: tanımlayıcılar, bildirim, şemalar, zarflar, karakter kümesi ve ayırıcılar, denetim numaraları ve doğrulama.

    !["Ayarlarını Gönder" yapılandırın](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık sözleşmenizi seçili ayarlarınızı uygun giden iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

| Özellik | Açıklama |
| --- | --- |
| UNB1.2 (sözdizimi sürüm) |Arasında bir değer seçin **1** ve **4**. |
| UNB2.3 (Gönderici Ters Yönlendirme Adresi) |En az bir karakter ve en çok 14 karakter alfasayısal bir değer girin. |
| UNB3.3 (Alıcı Ters Yönlendirme Adresi) |En az bir karakter ve en çok 14 karakter alfasayısal bir değer girin. |
| UNB6.1 (Alıcı Başvuru Parolası) |Bir en az ve en çok 14 karakter alfasayısal bir değer girin. |
| UNB6.2 (Alıcı Başvuru Niteleyicisi) |En az bir karakter ve en çok iki karakter alfasayısal bir değer girin. |
| UNB7 (Uygulama Başvuru Kimliği) |En az bir karakter ve en çok 14 karakter alfasayısal bir değer girin |

### <a name="acknowledgment"></a>Bildirim
| Özellik | Açıklama |
| --- | --- |
| İleti Okundu Bilgisi (CONTRL) |Barındırılan ortak bir teknik (CONTRL) bildirim almak görüyorsa bu onay kutusunu seçin. Bu ayar, kimin ileti gönderme, barındırılan ortak konuk iş ortağı onay istekleri belirtir. |
| Onay (KONTROL) |Barındırılan ortak bir işlev (CONTRL) bildirim almak görüyorsa bu onay kutusunu seçin. Bu ayar, kimin ileti gönderme, barındırılan ortak konuk iş ortağı onay istekleri belirtir. |
| Kabul edilen işlem kümeleri için SG1/SG4 döngüsü oluştur |İşlev bildirim istemek seçerseniz, kabul edilen işlem kümeleri için işlevsel CONTRL ilgili kaynaklar içinde SG1/SG4 döngüler nesil zorlamak için bu onay kutusunu seçin. |

### <a name="schemas"></a>Şemalar
| Özellik | Açıklama |
| --- | --- |
| UNH2.1 (TYPE) |Bir işlem kümesi türü seçin. |
| UNH2.2 (SÜRÜM) |İleti sürüm numarası girin. |
| UNH2.3 (SÜRÜM) |İleti sürüm numarası girin. |
| SCHEMA |Kullanılacak şema seçin. Şemalar tümleştirme hesabınızda bulunur. Şemalar erişmek için önce tümleştirme hesabınızı mantığı uygulamanıza bağlayın. |

### <a name="envelopes"></a>Zarflar
| Özellik | Açıklama |
| --- | --- |
| UNB8 (İşleme Önceliği Kodu) |Birden fazla karakter uzunluğunda değil alfabetik bir değer girin. |
| UNB10 (İletişim Anlaşması) |En az bir karakter ve en çok 40 karakter alfasayısal bir değer girin. |
| UNB11 (Test Göstergesi) |Oluşturulan değişim test verileri olduğunu belirtmek için bu onay kutusunu seçin |
| UNA Segmentini Uygula (Hizmet Dize Önerisi) |Gönderilecek değişimi için UNA kesimi oluşturmak için bu onay kutusunu seçin. |
| UNG Segmentlerini Uygula (İşlev Grubu Üstbilgisi) |Konuk iş ortağı gönderilen iletileri işlevsel Grup üstbilgisinde gruplandırma segmentleri oluşturmak için bu onay kutusunu seçin. Aşağıdaki değerleri UNG segmentler oluşturmak için kullanılır: <p>İçin **UNG1**, en az bir karakter ve en fazla altı karakter alfasayısal bir değer girin. <p>İçin **UNG2.1**, en az bir karakter ve en çok 35 karakter alfasayısal bir değer girin. <p>İçin **UNG2.2**, en fazla dört karakter alfasayısal bir değer girin. <p>İçin **UNG3.1**, en az bir karakter ve en çok 35 karakter alfasayısal bir değer girin. <p>İçin **UNG3.2**, en fazla dört karakter alfasayısal bir değer girin. <p>İçin **UNG6**, aşağıdakilerden en az ve en çok üç karakter alfasayısal bir değer girin. <p>İçin **UNG7.1**, en az bir karakter ve en çok üç karakter alfasayısal bir değer girin. <p>İçin **UNG7.2**, en az bir karakter ve en çok üç karakter alfasayısal bir değer girin. <p>İçin **UNG7.3**, en az 1 karakter ve en çok 6 karakter alfasayısal bir değer girin. <p>İçin **UNG8**, en az bir karakter ve en çok 14 karakter alfasayısal bir değer girin. |

### <a name="character-sets-and-separators"></a>Karakter Kümeleri ve Ayırıcılar

Karakter kümesi dışında her ileti türü için kullanılacak sınırlayıcı farklı bir dizi girebilirsiniz. Karakter kümesi için belirli bir ileti şeması belirtilmezse, varsayılan karakter kümesini kullanılır.

| Özellik | Açıklama |
| --- | --- |
| UNB1.1 (Sistem Tanımlayıcısı) |EDIFACT karakter üzerinde giden değişim uygulanacak kümesini seçin. |
| Şema |Bir şema aşağı açılan listeden seçin. Her satır tamamladıktan sonra yeni bir satır otomatik olarak eklenir. Seçili şeması için aşağıdaki ayırıcı açıklamalarına göre kullanmak istediğiniz ayırıcıları kümesi seçin. |
| Giriş Türü |Bir input type aşağı açılan listeden seçin. |
| Bileşen Ayırıcısı |Bileşik veri öğeleri ayırmak için tek bir karakter girin. |
| Veri Öğesi Ayırıcısı |Bileşik veri öğeleri içinde basit veri öğeleri ayırmak için tek bir karakter girin. |
| Segment Sonlandırıcı |EDI segmentinin sonunu belirtmek için tek bir karakter girin. |
| Sonek |Segment tanımlayıcısı ile kullanılan karakteri seçin. Sonek belirlerseniz, segment Sonlandırıcı veri öğesi boş olamaz. Segment sonlandırıcı boş bırakılırsa, sonek atamanız gerekir. |

### <a name="control-numbers"></a>Denetim Numaraları
| Özellik | Açıklama |
| --- | --- |
| UNB5 (Değişim Kontrol Numarası) |Bir önek, değişim denetimi numarası ve bir sonek için değer aralığını girin. Bu değerler, giden bir değişim oluşturmak için kullanılır. Denetim numarası ederken önek ve sonek isteğe bağlıdır. Denetim sayısı, her yeni ileti artırılır; önek ve sonek aynı kalır. |
| UNG5 (Grup Kontrol Numarası) |Bir önek, değişim denetimi numarası ve bir sonek için değer aralığını girin. Bu değerler, Grup kontrol numarası oluşturmak için kullanılır. Denetim numarası ederken önek ve sonek isteğe bağlıdır. En büyük değerine ulaşılana kadar denetim sayısı için her yeni ileti artırılır; önek ve sonek aynı kalır. |
| UNH1 (İleti Üst Bilgisi Başvuru Numarası) |Bir önek, değişim denetimi numarası ve bir sonek için değer aralığını girin. Bu değerler, ileti başlığı başvuru numarası oluşturmak için kullanılır. Başvuru numarası ederken önek ve sonek isteğe bağlıdır. Başvuru numarası yeni her ileti için artırılır; önek ve sonek aynı kalır. |

### <a name="validations"></a>Doğrulamalar

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kuralın belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| Mesaj Türü |EDI ileti türünü seçin. |
| EDI Doğrulaması |Şema, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılar EDI özellikleri tarafından tanımlanan veri türleri üzerinde EDI doğrulaması gerçekleştirir. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/max) izin verilir. |
| Başta/Sonda Sıfırlara İzin Ver |Başında veya sonunda sıfır ek korumak ve karakterleri boşluk. Bu karakterleri kaldırmayın. |
| Baştaki/Sondaki Sıfırları Kırp |Baştaki veya sondaki sıfır karakterleri kaldırın. |
| Sonda Ayırıcı İlkesi |Sondaki ayırıcılar oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve ayırıcı olarak gönderilen değişim önlemek için. Değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, değişim geçerli bildirilmedi. <p>Seçin **isteğe bağlı** etkileşimler ile veya olmadan sonundaki sınırlayıcılar ve ayırıcılar göndermek için. <p>Seçin **zorunlu** gönderilen değişim sonundaki sınırlayıcılar ve ayırıcıları kullanmanız gerekiyorsa. |

## <a name="find-your-created-agreement"></a>Oluşturulan sözleşmenizi Bul

1.  Üzerinde anlaşma özelliklerinizi ayarlama işlemini tamamladıktan sonra **Ekle** dikey penceresinde, seçin **Tamam** sözleşmenizi oluşturmayı tamamlamak ve tümleştirme hesabı dikey penceresine geri dönün.

    Yeni eklenen sözleşmenizi artık görünür, **anlaşmaları** listesi.

2.  Ayrıca, tümleştirme hesabına genel bakış sözleşmelerinizi görüntüleyebilirsiniz. Tümleştirme hesabı dikey penceresinde, seçin **genel bakış**seçeneğini belirleyip **anlaşmaları** döşeme. 

    ![Tüm anlaşmalar görüntülemek için "Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>Görünüm Swagger dosyası
EDIFACT bağlayıcı Swagger ayrıntılarını görüntülemek için bkz: [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  

