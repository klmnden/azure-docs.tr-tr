---
title: FTP sunucusu - Azure Logic Apps bağlanma
description: Oluşturabilir, izleyebilir ve Azure Logic Apps ile bir FTP sunucusunda dosyaları yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: divswa, klam, LADocs
ms.topic: article
ms.date: 06/19/2019
tags: connectors
ms.openlocfilehash: 66f1d726dcfa1a077abbff0d9f028036db43cc25
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67293085"
---
# <a name="create-monitor-and-manage-ftp-files-by-using-azure-logic-apps"></a>Oluşturabilir, izleyebilir ve Azure Logic Apps kullanarak FTP dosyalarını yönetme

Azure Logic Apps ve FTP Bağlayıcısı ile otomatik görevler ve iş akışları oluşturma, izleme, gönderme ve örneğin hesabınızda, diğer eylemlerin yanı sıra FTP sunucusu dosyalarıyla alırsınız oluşturabilirsiniz:

* Dosyaları eklenen veya değiştirilen zamana yönelik İzleyici.
* Al, oluştur, kopyalayın, listesinde güncelleştirmek ve dosyaları silin.
* Dosya içeriğini ve meta verileri alın.
* Arşivi klasöre ayıklayın.

FTP sunucunuzdan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir Tetikleyicileri kullanabilirsiniz. FTP sunucunuzdaki dosyaları yönetmek için logic apps çalışma eylemlerini kullanabilirsiniz. Ayrıca, FTP eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, FTP sunucunuzdan düzenli olarak dosyaları alırsanız, Office 365 Outlook Bağlayıcısı veya Outlook.com Bağlayıcısı'nı kullanarak dosyaları ve içeriklerini hakkında e-posta gönderebilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

## <a name="limits"></a>Limits

* FTP Bağlayıcısı, yalnızca açık FTP (FTPS) SSL üzerinden destekler ve örtük FTPS ile uyumlu değil.

* Varsayılan olarak, FTP Eylemler okuma veya yazma dosyaları *50 MB veya daha küçük*. 50 MB'tan büyük dosyaları işlemek için FTP eylemleri desteğini [ileti Öbekleme](../logic-apps/logic-apps-handle-large-messages.md). **Dosya içeriğini Al** eylem örtülü olarak kullanan Öbekleme.

* FTP Tetikleyicileri Öbekleme desteklemez. Dosya içeriği isterken Tetikleyicileri 50 MB üzerinde olan dosyalar seçin veya daha küçük. 50 MB'tan büyük dosyaları almak için bu düzeni izleyin:

  * Dosya özellikleri gibi döndüren bir FTP tetikleyicisini kullanma **dosya eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** .

  * FTP ile tetikleyici izleyin **dosya içeriğini Al** tam dosyasını okur ve örtük olarak kullanıldığı Öbekleme eylem.

## <a name="how-ftp-triggers-work"></a>Nasıl iş FTP tetikler

FTP iş FTP dosya sistemi yoklama ve son yoklamadan bu yana değiştirilmiş her dosya için arayarak tetikler. Bazı araçlar, dosyaları değiştirdiğinizde, zaman damgası koruma sağlar. Bu gibi durumlarda, Tetikleyiciniz çalışabilmesi için bu özelliği devre dışı gerekir. Bazı ortak ayarları şunlardır:

| SFTP istemci | Eylem |
|-------------|--------|
| Winscp | Git **seçenekleri** > **tercihleri** > **aktarım** > **Düzenle**  >  **Korumak zaman damgası** > **devre dışı bırak** |
| FileZilla | Git **aktarım** > **korumak aktarılan dosyaların zaman damgaları** > **devre dışı bırak** |
|||

Tetikleyici yeni bir dosya bulduğunda tetikleyici, yeni dosya eksiksiz ve kısmen yazılmış olup olmadığını denetler. Örneğin, dosya sunucusu tetikleyici iade ederken bir dosya değişiklikleri sürüyor olabilir. Kısmen yazılı bir dosya döndürme önlemek için zaman damgası, son değişiklikler var, ancak bu dosyayı hemen döndürmüyor dosyası için tetikleyici notlar. Tetikleyici, yalnızca sunucu yeniden yoklanırken dosyayı döndürür. Bazı durumlarda, bu davranışı, iki kez tetikleyicinin kadar yoklama aralığı bir gecikmeye neden olabilir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* FTP konak sunucu adresi ve hesap kimlik bilgilerinizi

  FTP Bağlayıcısı FTP sunucunuza çalışması için Internet ve ayarlama erişilebilir olmasını gerektirir *pasif* modu. Kimlik bilgilerinizi, mantıksal uygulamanızın bir bağlantı oluşturun ve FTP hesabınıza erişmesine olanak tanır.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* FTP hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir FTP tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). FTP eylem kullanmak için mantıksal uygulamanızı başka bir tetikleyici ile başlar, **yinelenme** tetikleyici.

## <a name="connect-to-ftp"></a>FTP bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "ftp" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin.

   veya

   Var olan mantıksal uygulamalar, son adım, bir eylem eklemek istediğiniz altında seçin için **yeni adım**ve ardından **Eylem Ekle**. Arama kutusuna "ftp filtreniz olarak" girin. Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. Artı işaretini seçin ( **+** ) görünür ve seçin **Eylem Ekle**.

1. Bağlantınız için gerekli bilgileri sağlayın ve ardından **Oluştur**.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

<a name="file-added-modified"></a>

### <a name="ftp-trigger-when-a-file-is-added-or-modified"></a>FTP tetikleyici: Dosya eklendiğinde veya değiştirildiğinde

Bir dosya eklendiğinde veya bir FTP sunucusuna değiştirilen tetikleyici algıladığında, bu tetikleyiciyi bir mantıksal uygulama iş akışı başlatır. Örneğin, dosyanın içeriğini denetler ve söz konusu içeriği almak etkinleştirilip etkinleştirilmeyeceğini karar bir koşul ekleyebilirsiniz içeriğin belirtilen bir koşulu karşılayıp temel. Son olarak, dosyanın içeriğini alır bir eylem ekleme ve içeriği SFTP sunucusunda bir klasöre yerleştirin.

**Kuruluş örnek**: Bu tetikleyici, müşteri siparişleri açıklayan yeni dosyaları bir FTP klasörü izlemek için kullanabilirsiniz. Ardından bir FTP eylem gibi kullanabilir **dosya içeriğini Al**, böylece daha ayrıntılı işleme için sipariş içeriklerini almak ve o sırada bir sipariş veritabanında depolayın.

Bu tetikleyiciyi gösteren bir örnek aşağıda verilmiştir: **Dosya eklendiğinde veya değiştirildiğinde**

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "ftp" girin. Tetikleyiciler listesinde şu tetikleyiciyi seçin: **Ne zaman bir dosyalanmış eklendiğinde veya değiştirildiğinde - FTP**

   ![Bulma ve FTP tetikleyicisini seçin](./media/connectors-create-api-ftp/select-ftp-trigger.png)  

1. Bağlantınız için gerekli bilgileri sağlayın ve ardından **Oluştur**.

   Varsayılan olarak, bu bağlayıcıyı dosyaları metin biçiminde aktarır. Aktarım için ikili dosyaları, örneğin, nerede biçimlendirmek ve kodlama kullanıldığında seçin **ikili aktarım**.

   ![FTP sunucusu bağlantı oluşturma](./media/connectors-create-api-ftp/create-ftp-connection-trigger.png)  

1. Yanındaki **klasör** kutusunda, bir liste görünecek şekilde klasör simgesini seçin. Yeni ve düzenlenen dosyaları için izlemek istediğiniz klasörü bulmak üzere dik açılı oku seçin ( **>** ), bu klasöre göz atın ve ardından klasörü seçin.

   ![Bulmak ve izlemek için bir klasör seçin](./media/connectors-create-api-ftp/select-folder.png)  

   Seçilen klasörün görünür **klasör** kutusu.

   ![Seçili klasörü](./media/connectors-create-api-ftp/selected-folder.png)  

Mantıksal uygulamanızın tetikleyici vardır, mantıksal uygulamanız yeni veya düzenlenmiş bir dosya bulduğunda çalıştırmak istediğiniz eylemler ekleyin. Bu örnekte, yeni veya güncelleştirilmiş içeriği alır bir FTP eylem ekleyebilirsiniz.

<a name="get-content"></a>

### <a name="ftp-action-get-content"></a>FTP eylem: İçerik alın

Bu dosya eklendiğinde veya bu eylem bir FTP sunucusuna dosya içeriği alır. Örneğin, önceki örnekte ve bu dosyayı eklenmiş veya düzenlenmişse sonra dosyanın içeriğini alır bir eylem tetikleyici ekleyebilirsiniz.

Bu eylem gösteren bir örnek aşağıda verilmiştir: **İçerik alın**

1. Tetikleyici veya diğer tüm eylemler altında seçin **yeni adım**.

1. Arama kutusuna "ftp filtreniz olarak" girin. Eylemler listesinde şu eylemi seçin: **FTP - dosya içeriğini Al**

   ![FTP eylemini seçin](./media/connectors-create-api-ftp/select-ftp-action.png)  

1. FTP sunucunuz ve hesabı için bir bağlantı zaten varsa sonraki adıma gidin. Aksi takdirde, bu bağlantı için gerekli bilgileri sağlayın ve ardından **Oluştur**.

   ![FTP sunucusu bağlantı oluşturma](./media/connectors-create-api-ftp/create-ftp-connection-action.png)

1. Sonra **dosya içeriğini Al** eylemi açılır tıklayın içinde **dosya** dinamik içerik listesinde görünmesi kutusu. Bu gibi durumlarda, çıkış özellikleri artık önceki adımlardan seçebilirsiniz. Dinamik içerik listesinden **dosya içeriği** eklenen veya güncelleştirilen dosyayı ilgili içeriği olan bir özellik.  

   ![Bul ve dosyayı seçin](./media/connectors-create-api-ftp/ftp-action-get-file-content.png)

   **Dosya içeriği** özelliği artık görünür **dosya** kutusu.

   ![Seçili "dosya içerik" özelliği](./media/connectors-create-api-ftp/ftp-action-selected-file-content-property.png)

1. Mantıksal uygulamanızı kaydedin. Akışınızı test etmek için mantıksal uygulamanız artık izleyen FTP klasörüne bir dosya ekleyin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, gözden geçirme [bağlayıcının başvuru sayfası](/connectors/ftpconnector/).

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)
