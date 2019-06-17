---
title: B2B Kurumsal tümleştirme - Azure Logic Apps için EDIFACT iletileri | Microsoft Docs
description: Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme için EDI biçiminde EDIFACT iletilerini paylaşma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.date: 07/26/2016
ms.openlocfilehash: bbcdad7c5496cd08994a613b07e1bc7c611e4572
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60684514"
---
# <a name="exchange-edifact-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic apps'teki B2B Kurumsal tümleştirme için EDIFACT iletilerini paylaşma

Azure Logic Apps için EDIFACT iletileri gönderip alabilir önce EDIFACT sözleşmesi oluşturun ve bu sözleşme, tümleştirme hesabında depolayın. EDIFACT sözleşmesi oluşturun ilişkin adımlar aşağıda verilmiştir.

> [!NOTE]
> Bu sayfa, Azure Logic Apps için EDIFACT özellikleri kapsar. Daha fazla bilgi için [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Başlamadan önce

Gereksinim duyduğunuz öğeleri şu şekildedir:

* Bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) zaten tanımlanmış ve Azure aboneliğinizle ilişkili  
* En az iki [iş ortakları](logic-apps-enterprise-integration-partners.md) , tümleştirme hesabında zaten tanımlanmış

> [!NOTE]
> Bir sözleşme oluşturduğunuzda, aldığınız veya iş ortağı gelen ve giden gönderdiğiniz iletileri içeriği sözleşme türüyle eşleşmelidir.

Çalıştırdıktan sonra [tümleştirme hesabı oluşturma](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve [iş ortakları ekleme](logic-apps-enterprise-integration-partners.md), aşağıdaki adımları izleyerek, bir EDIFACT sözleşmesi oluşturabilirsiniz.

## <a name="create-an-edifact-agreement"></a>EDIFACT sözleşmesi oluşturun 

1. [Azure portalı](https://portal.azure.com "Azure portalı") oturumunu açın. 

2. Ana Azure menüsünde **tüm hizmetleri**. Arama kutusuna "tümleştirme" girin ve ardından **tümleştirme hesapları**.

   ![Tümleştirme hesabı bulunamadı](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

   > [!TIP]
   > Varsa **tüm hizmetleri** görünmüyorsa, menü öğesine sahip olabilir. Daraltılmış menüsünün üstünde, seçin **metin etiketlerini göster**.

3. Altında **tümleştirme hesapları**, tümleştirme hesabı sözleşmesi oluşturmak istediğiniz yeri seçin.

   ![Tümleştirme hesabı sözleşmesi oluşturulacağı konumu seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Seçin **sözleşmeleri**. Anlaşmaları kutucuk yoksa kutucuk önce ekleyin.   

   !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Anlaşmaları sayfasında **Ekle**.

   !["Ekle" öğesini seçin](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. Altında **Ekle**, girin bir **adı** sözleşmenize için. İçin **sözleşme türü**seçin **EDIFACT**. Seçin **konak iş ortağı**, **konak kimliği**, **Konuk iş ortağı**, ve **Konuk kimlik** sözleşmenize için.

   ![Anlaşma ayrıntılarını sağlayın](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

   | Özellik | Açıklama |
   | --- | --- |
   | Ad |Anlaşma adı |
   | Sözleşme türü | EDIFACT olması gerekir |
   | Konak iş ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konak iş ortağı sözleşmesi yapılandıran kuruluşu temsil eder. |
   | Konak kimliği |Konak iş ortağı için bir tanımlayıcı |
   | Konuk iş ortağı |Bir anlaşma hem konak hem de Konuk iş ortağı gerekir. Konuk iş ortağı, konak iş ortağı iş yapmakta kuruluşu temsil eder. |
   | Konuk kimliği |Konuk iş ortağı için bir tanımlayıcı |
   | Ayarları Al |Bu özellikler, bir anlaşma tarafından alınan tüm iletileri için geçerlidir. |
   | Gönderme ayarları |Bu özellikler, bir anlaşma tarafından gönderilen tüm iletiler için geçerlidir. |
   ||| 

## <a name="configure-how-your-agreement-handles-received-messages"></a>Nasıl iletileri, anlaşma tanıtıcıları alınan yapılandırma

Sözleşme özelliklerini ayarladıysanız, işbu sözleşme nasıl tanımlar ve işbu sözleşme aracılığıyla iş ortağınız alınan gelen iletileri işleyen yapılandırabilirsiniz.

1. Altında **Ekle**seçin **alma ayarı**.
Sizinle iletiler birbiriyle değiştirir iş ortaklarıyla sözleşmenize göre bu özelliklerini yapılandırın. Bu bölümdeki tablolarda, özellik açıklamalarına bakın.

   **Alma ayarlarını** bu bölümler halinde düzenlenmiştir: Tanımlayıcılar, bildirim, şemalar, denetim numaraları, doğrulama ve iç ayarlar.

   !["Alma ayarlarını" yapılandırma](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık, anlaşmanız seçili ayarlarınıza uygun gelen iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcıları

| Özellik | Açıklama |
| --- | --- |
| UNB6.1 (alıcı başvuru parolası) |1 ile 14 arasında karakter arasında alfasayısal bir değer girin. |
| UNB6.2 (alıcı başvuru niteleyicisi) |En az bir karakter ve en fazla iki karakter alfasayısal bir değer girin. |

### <a name="acknowledgments"></a>İlgili kaynaklar

| Özellik | Açıklama |
| --- | --- |
| İleti (okundu bilgisi CONTRL) |Bir teknik (okundu bilgisi CONTRL) bildirim değişim göndericiye için bu onay kutusunu seçin. Anlaşma Gönder ayarlara göre değişim gönderen bildirimi gönderilir. |
| Onay (kontrol) |Anlaşma Gönder ayarlara göre değişim gönderen gönderilen bir işlev (kontrol) bildirim değişim bildirim göndericiye için bu onay kutusunu seçin. |

### <a name="schemas"></a>Şemalar

| Özellik | Açıklama |
| --- | --- |
| UNH2.1 (TÜR) |İşlem kümesi türü seçin. |
| UNH2.2 (SÜRÜM) |İleti sürüm numarası girin. (En düşük, bir karakter; en büyük, üç karakter). |
| UNH2.3 (SÜRÜM) |İleti sürüm numarası girin. (En düşük, bir karakter; en büyük, üç karakter). |
| UNH2.5 (İLİŞKİLİ ATANAN KOD) |Atanan kodu girin. (En fazla, altı karakter. Alfasayısal olmalıdır). |
| UNG2.1 (UYGULAMA GÖNDEREN KİMLİĞİ) |En az bir karakter ve en fazla 35 karakter alfasayısal bir değer girin. |
| UNG2.2 (UYGULAMA GÖNDEREN KODU NİTELEYİCİSİ) |En fazla dört karakter alfasayısal bir değer girin. |
| ŞEMA |Daha önce yüklenen şemayı, ilişkili tümleştirme hesabından kullanmak istediğiniz seçin. |

### <a name="control-numbers"></a>Denetim numaraları
| Özellik | Açıklama |
| --- | --- |
| Değişim denetim numarası yinelenmesine izin verme |Yinelenen değişim engellemek için bu özelliği seçin. Seçili olduğunda, değişim denetim numarası (UNB5) alınan değişim için daha önce işlenen değişim denetim numarası eşleşmiyor EDIFACT kod çözme eylemi denetler. Eşleşme algılandığında, değişim işlenmez. |
| Onay için yinelenen UNB5 sıklığı (gün) |Yinelenen değişim denetim numaraları izin vermeyecek şekilde seçerseniz, gün sayısını belirtebilirsiniz. Bu ayar için uygun değeri verilerek denetimi gerçekleştirmek ne zaman. |
| Grup denetim numaralarına izin verme |Yinelenen grup denetim numarası (UNG5) olan değişim engellemek için bu özelliği seçin. |
| İşlem kümesi denetim numaralarına izin verme |Yinelenen işlem kümesi denetim numarası (UNH1) olan değişim engellemek için bu özelliği seçin. |
| EDIFACT onay kontrol numarası |İşlem kümesi başvuru sayıları kullanmak için bildirim belirlemek için önek, bir dizi başvuru numarası ve soneki için bir değer girin. |

### <a name="validations"></a>Doğrulamaları

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti türü |EDI ileti türü seçin. |
| EDI doğrulaması |EDI doğrulaması şema EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılara tarafından tanımlanan veri türleri üzerinde gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/maks) izin verilir. |
| Başta/sonda sıfırlara izin ver |Başta veya sonda sıfır ek korumak ve boşluk karakterleri. Bu karakterleri kaldırın. |
| Baştaki/Sondaki sıfırları Kırp |Baştaki veya sondaki ve boşluk karakterleri kaldırın. |
| Sonda ayırıcı İlkesi |Sondaki ayırıcılara oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve alınan değişim ayırıcılar önlemek için. Değişim, değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, geçerli bildirilmedi. <p>Seçin **isteğe bağlı** değişim ile veya sonundaki sınırlayıcılar ve ayırıcılar olmadan kabul etmek için. <p>Seçin **zorunlu** olduğunda alınan değişim olmalıdır sonundaki sınırlayıcılar ve ayırıcı. |

### <a name="internal-settings"></a>İç ayarlar

| Özellik | Açıklama |
| --- | --- |
| Sondaki ayırıcılara izin veriliyorsa boş XML etiketleri oluştur |Sondaki ayırıcılara boş XML etiketleri dahil değişim gönderen sağlamak için bu onay kutusunu seçin. |
| Bölünmüş değişimi işlem kümeleri - olarak işlem kümelerini Askıya Al hatası|Her işlem işlem kümesine uygun Zarf uygulayarak bir değişimin ayrı bir XML belgesine kümesi ayrıştırır. Doğrulama başarısız olan işlem kümelerini askıya alın. |
| Bölünmüş değişimi işlem kümeleri - olarak hata durumunda değişimi Askıya Al|Uygun Zarf uygulayarak bir değişim ayrı bir XML belgesine kümesindeki her bir işlem ayrıştırır. Bir veya daha fazla işlem kümeleri değişim doğrulama başarısız olduğunda tüm oluştuğunda değişimi Askıya Al. | 
| Değişimi Koru - hata işlem kümelerini Askıya Al |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Diğer tüm işlem kümeleri işlenmeye devam ederken doğrulama, başarısız olan işlem kümelerini askıya alın. |
| Değişimi Koru - hata oluştuğunda değişimi Askıya Al |Değişim dokunmaz, tüm toplu değişim için bir XML belgesi oluşturur. Bir veya daha fazla işlem kümeleri değişim doğrulama başarısız olduğunda tüm oluştuğunda değişimi Askıya Al. |

## <a name="configure-how-your-agreement-sends-messages"></a>Sözleşmenize iletileri nasıl göndereceğini yapılandırın

İşbu sözleşme nasıl tanımlar ve işbu sözleşme aracılığıyla iş ortaklarınızla göndermek giden iletileri işleyen yapılandırabilirsiniz.

1.  Altında **Ekle**seçin **gönderme ayarları**.
Sizinle iletiler değiştirir ortağınızla birlikte sözleşmenize göre bu özelliklerini yapılandırın. Bu bölümdeki tablolarda, özellik açıklamalarına bakın.

    **Gönderme ayarları** bu bölümler halinde düzenlenmiştir: Tanımlayıcılar, bildirim, şemalar, zarflar, karakter kümeleri ve ayırıcılar, denetim numaraları ve doğrulamaları.

    !["Ayarları Gönder" yapılandırma](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Bitirdikten sonra seçerek ayarlarınızı kaydettiğinizden emin olun **Tamam**.

Artık, anlaşmanız seçili ayarlarınıza uygun giden iletileri işlemek hazırdır.

### <a name="identifiers"></a>Tanımlayıcıları

| Özellik | Açıklama |
| --- | --- |
| UNB1.2 (söz dizimi sürümü) |Arasında bir değer seçin **1** ve **4**. |
| UNB2.3 (gönderici ters yönlendirme adresi) |En az bir karakter ve en fazla 14 karakter alfasayısal bir değer girin. |
| UNB3.3 (alıcı ters yönlendirme adresi) |En az bir karakter ve en fazla 14 karakter alfasayısal bir değer girin. |
| UNB6.1 (alıcı başvuru parolası) |En az bir ve en fazla 14 karakter alfasayısal bir değer girin. |
| UNB6.2 (alıcı başvuru niteleyicisi) |En az bir karakter ve en fazla iki karakter alfasayısal bir değer girin. |
| UNB7 (uygulama başvuru kimliği) |En az bir karakter ve en fazla 14 karakter alfasayısal bir değer girin |

### <a name="acknowledgment"></a>Bildirim
| Özellik | Açıklama |
| --- | --- |
| İleti (okundu bilgisi CONTRL) |Bir teknik (okundu bilgisi CONTRL) bildirim almak barındırılan iş ortağı görüyorsa, bu onay kutusunu seçin. Bu ayar, kimin ileti gönderme, barındırılan iş ortağı, Konuk iş ortağı bir onay istekleri belirtir. |
| Onay (kontrol) |Bir işlev (kontrol) bildirim almak barındırılan iş ortağı görüyorsa, bu onay kutusunu seçin. Bu ayar, kimin ileti gönderme, barındırılan iş ortağı, Konuk iş ortağı bir onay istekleri belirtir. |
| Kabul edilen işlem kümeleri için SG1/SG4 döngüsü oluştur |İşlevsel bir onay isteği seçerseniz, işlevsel kontrol bildirimler kabul edilen işlem kümeleri için SG1/SG4 döngüsü nesil etmeye zorlamak için bu onay kutusunu seçin. |

### <a name="schemas"></a>Şemalar
| Özellik | Açıklama |
| --- | --- |
| UNH2.1 (TÜR) |İşlem kümesi türü seçin. |
| UNH2.2 (SÜRÜM) |İleti sürüm numarası girin. |
| UNH2.3 (SÜRÜM) |İleti sürüm numarası girin. |
| ŞEMA |Kullanılacak şemayı seçin. Şemaları tümleştirme hesabınızdaki yer alır. Kendi şemaları erişmek için ilk mantıksal uygulamanızı tümleştirme hesabınızı bağlayın. |

### <a name="envelopes"></a>Zarflar
| Özellik | Açıklama |
| --- | --- |
| UNB8 (işleme önceliği kodu) |Birden fazla karakter uzunluğunda değil alfabetik bir değer girin. |
| UNB10 (iletişim anlaşması) |En az bir karakter ve en fazla 40 karakter alfasayısal bir değer girin. |
| UNB11 (Test göstergesi) |Oluşturulan değişim test verilerini olduğunu belirtmek için bu onay kutusunu seçin |
| (Hizmet dize önerisi) UNA segmentini Uygula |UNA segmenti gönderilecek değişim için oluşturmak için bu onay kutusunu seçin. |
| (İşlev grubu üstbilgisi) UNG segmentlerini Uygula |Konuk iş ortağı gönderilen iletileri işlevsel Grup üstbilgisinde gruplandırma okunur kesimler oluşturmak için bu onay kutusunu seçin. UNG segmentlerini oluşturmak için aşağıdaki değerler kullanılır: <p>İçin **UNG1**, en az bir karakter ve en fazla altı karakter alfasayısal bir değer girin. <p>İçin **UNG2.1**, en az bir karakter ve en fazla 35 karakter alfasayısal bir değer girin. <p>İçin **UNG2.2**, en fazla dört karakter alfasayısal bir değer girin. <p>İçin **UNG3.1**, en az bir karakter ve en fazla 35 karakter alfasayısal bir değer girin. <p>İçin **UNG3.2**, en fazla dört karakter alfasayısal bir değer girin. <p>İçin **UNG6**, en az bir ve en çok üç karakter alfasayısal bir değer girin. <p>İçin **UNG7.1**, en az bir karakter ve en çok üç karakter alfasayısal bir değer girin. <p>İçin **UNG7.2**, en az bir karakter ve en çok üç karakter alfasayısal bir değer girin. <p>İçin **UNG7.3**, en az 1 karakter ve en fazla 6 karakterden oluşan alfasayısal bir değer girin. <p>İçin **UNG8**, en az bir karakter ve en fazla 14 karakter alfasayısal bir değer girin. |

### <a name="character-sets-and-separators"></a>Karakter kümeleri ve ayırıcılar

Karakter kümesi dışında her ileti türü için kullanılacak sınırlayıcı farklı bir dizi girebilirsiniz. Ardından bir karakter kümesi için belirli bir ileti şema belirtilmezse, varsayılan karakter kümesi kullanılır.

| Özellik | Açıklama |
| --- | --- |
| UNB1.1 (sistem tanımlayıcısı) |EDIFACT karakter üzerinde giden değişim uygulanacak kümesini seçin. |
| Şema |Bir şema, aşağı açılan listeden seçin. Her satır tamamladıktan sonra yeni bir satır otomatik olarak eklenir. Seçili şemayı için aşağıdaki ayırıcı açıklamalarına göre kullanmak istediğiniz ayırıcılar kümesi seçin. |
| Giriş türü |Bir giriş türü açılan listesinden seçin. |
| Bileşen ayırıcısı |Bileşik veri öğeleri ayırmak için tek bir karakter girin. |
| Veri öğesi ayırıcısı |Bileşik veri öğeleri içinde basit veri öğeleri ayırmak için tek bir karakter girin. |
| Segment Sonlandırıcı |EDI segment sonunu belirtmek için tek bir karakter girin. |
| Son eki |Segment tanımlayıcısıyla kullanılan karakter seçin. Bir sonek belirlerseniz, segment Sonlandırıcı veri öğesi boş olabilir. Segment sonlandırıcı boş bırakılırsa, sonek atamanız gerekir. |

### <a name="control-numbers"></a>Denetim numaraları
| Özellik | Açıklama |
| --- | --- |
| UNB5 (değişim kontrol numarası) |Bir önek değişim denetim numarası ve soneki için değer aralığını girin. Bu değerler, giden bir değişim oluşturmak için kullanılır. Denetim numarası gereklidir ancak önek ve sonek isteğe bağlıdır. Her yeni ileti için Denetim numarası artırılır; önek ve sonek aynı kalır. |
| UNG5 (Grup kontrol numarası) |Bir önek değişim denetim numarası ve soneki için değer aralığını girin. Bu değerler, Grup denetim numarası oluşturmak için kullanılır. Denetim numarası gereklidir ancak önek ve sonek isteğe bağlıdır. En yüksek değer ulaşılana kadar her yeni ileti için Denetim numarası artırılır; önek ve sonek aynı kalır. |
| UNH1 (ileti üst bilgisi başvuru numarası) |Bir önek değişim denetim numarası ve soneki için değer aralığını girin. Bu değerler, ileti üst bilgisi başvuru numarası oluşturmak için kullanılır. Önek ve sonek başvuru numarası gereklidir ancak isteğe bağlıdır. Her yeni ileti için başvuru numarası artırılır; önek ve sonek aynı kalır. |

### <a name="validations"></a>Doğrulamaları

Her doğrulama satır tamamladığınızda, başka bir otomatik olarak eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satır kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti türü |EDI ileti türü seçin. |
| EDI doğrulaması |EDI doğrulaması şema, uzunluk kısıtlamaları, boş veri öğeleri ve sondaki ayırıcılara EDI özellikleri tarafından tanımlanan veri türleri üzerinde gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü değilse EDI, doğrulama veri öğesi gereksinimdir ve yineleme, numaralandırmalar ve veri öğesi uzunluğu doğrulama (min/maks) izin verilir. |
| Başta/sonda sıfırlara izin ver |Başta veya sonda sıfır ek korumak ve boşluk karakterleri. Bu karakterleri kaldırın. |
| Baştaki/Sondaki sıfırları Kırp |Başta veya sonda sıfır karakter kaldırın. |
| Sonda ayırıcı İlkesi |Sondaki ayırıcılara oluşturur. <p>Seçin **izin** sonundaki sınırlayıcılar ve gönderilen değişim ayırıcılar önlemek için. Değişim, değişim sonundaki sınırlayıcılar ve ayırıcılar varsa, geçerli bildirilmedi. <p>Seçin **isteğe bağlı** değişim ile veya sonundaki sınırlayıcılar ve ayırıcılar olmadan göndermek için. <p>Seçin **zorunlu** gönderilen değişim sonundaki sınırlayıcılar ve ayırıcılar olması gerekiyorsa. |

## <a name="find-your-created-agreement"></a>Oluşturulan sözleşmenize Bul

1.  Üzerinde anlaşma özelliklerinizi ayarlama işlemini tamamladıktan sonra **Ekle** sayfasında **Tamam** sözleşmenize oluşturma işlemini tamamladıktan ve tümleştirme hesabınıza döndürmek için.

    Yeni eklenen sözleşmenize artık görünür, **sözleşmeleri** listesi.

2.  Tümleştirme hesabı genel bakış sözleşmelerinizi da görüntüleyebilirsiniz. Tümleştirme hesabı menüsünde **genel bakış**, ardından **sözleşmeleri** Döşe. 

    !["Anlaşmaları" kutucuğunu seçin](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>Swagger dosyasını görüntüle
EDIFACT bağlayıcının Swagger ayrıntılarını görüntülemek için bkz: [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Enterprise Integration Pack hakkında daha fazla bilgi](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  

