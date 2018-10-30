---
title: FTP - Azure Logic Apps sunucusuna | Microsoft Docs
description: Oluşturabilir, izleyebilir ve Azure Logic Apps ile bir FTP sunucusunda dosyaları yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 10/15/2018
tags: connectors
ms.openlocfilehash: a14f045193c01b8c26019314ddde4c2116d8bad6
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232826"
---
# <a name="create-monitor-and-manage-ftp-files-by-using-azure-logic-apps"></a>Oluşturabilir, izleyebilir ve Azure Logic Apps kullanarak FTP dosyalarını yönetme

Azure Logic Apps ve FTP Bağlayıcısı ile otomatik görevler ve iş akışları oluşturma, izleme, gönderme ve örneğin hesabınızda, diğer eylemlerin yanı sıra FTP sunucusu dosyalarıyla alırsınız oluşturabilirsiniz:

* Dosyaları eklenen veya değiştirilen zamana yönelik İzleyici.
* Al, oluştur, kopyalayın, listesinde güncelleştirmek ve dosyaları silin.
* Dosya içeriğini ve meta verileri alın.
* Arşivi klasöre ayıklayın.

FTP sunucunuzdan yanıtlar almak ve çıkış diğer eylemler için kullanılabilir Tetikleyicileri kullanabilirsiniz. FTP sunucunuzdaki dosyalarla görevleri gerçekleştirmek için logic apps eylemleri kullanabilirsiniz. Ayrıca, FTP eylemleri çıktısını kullanan diğer eylemler olabilir. Örneğin, FTP sunucunuzdan dosyalarını düzenli olarak almak, Office 365 Outlook Bağlayıcısı veya Outlook.com Bağlayıcısı'nı kullanarak bu dosyaları ve bunların içeriğini hakkında e-posta gönderebilirsiniz. Logic apps kullanmaya yeni başladıysanız gözden [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> FTP Bağlayıcısı 50 MB üzerinde olan dosyaları destekler veya kullanılmadıkça daha küçük [büyük iletileri işlemek için Öbekleme](../logic-apps/logic-apps-handle-large-messages.md). 
>
> Ayrıca, FTP Bağlayıcısı yalnızca açık FTP (FTPS) SSL üzerinden destekler ve örtük FTPS ile uyumlu değil. 

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>. 

* FTP konak sunucu adresi ve hesap kimlik bilgilerinizi

  FTP Bağlayıcısı FTP sunucunuza çalışması için Internet ve ayarlama erişilebilir olmasını gerektirir *pasif* modu. Mantıksal uygulamanızı bir bağlantı oluşturup FTP hesabınıza erişmek için kimlik bilgilerinizi yetkilendirin.

* Hakkında temel bilgilere [mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* FTP hesabınıza erişmek için istediğiniz mantıksal uygulaması. Bir FTP tetikleyicisi ile başlatmak için [boş mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md). FTP eylem kullanmak için mantıksal uygulamanızı başka bir tetikleyici ile başlar, **yinelenme** tetikleyici.

## <a name="connect-to-ftp"></a>FTP bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "ftp" girin. Tetikleyiciler listesinde istediğiniz tetikleyicisini seçin. 

   -veya-

   Var olan mantıksal uygulamalar, son adım, bir eylem eklemek istediğiniz altında seçin için **yeni adım**ve ardından **Eylem Ekle**. 
   Arama kutusuna "ftp filtreniz olarak" girin. 
   Eylemler listesinde, istediğiniz eylemi seçin.

   Adımlar arasında bir eylem eklemek için işaretçinizi adımlar arasındaki okun üzerine getirin. 
   Artı işaretini seçin (**+**), görünür ve ardından **Eylem Ekle**.

1. Bağlantınız için gerekli bilgileri sağlayın ve ardından **Oluştur**.

1. Seçili tetikleyici veya eylem için gerekli bilgileri sağlayın ve mantıksal uygulamanızın iş akışı oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

### <a name="ftp-trigger-when-a-file-is-added-or-modified"></a>FTP tetikleyici: ne zaman bir dosya eklendiğinde veya değiştirildiğinde

Bir dosya eklendiğinde veya bir FTP sunucusuna değiştirilen tetikleyici algıladığında, bu tetikleyiciyi bir mantıksal uygulama iş akışı başlatır. Örneğin, dosyanın içeriğini denetler ve söz konusu içeriği almak etkinleştirilip etkinleştirilmeyeceğini karar bir koşul ekleyebilirsiniz içeriğin belirtilen bir koşulu karşılayıp temel. Son olarak, dosyanın içeriğini alır bir eylem ekleme ve içeriği SFTP sunucusunda bir klasöre yerleştirin. 

**Kuruluş örnek**: Bu tetikleyici, müşteri siparişleri temsil eden yeni dosyaları bir FTP klasörü izlemek için kullanabilirsiniz. Ardından bir FTP eylem gibi kullanabilir **dosya içeriğini Al**, böylece daha ayrıntılı işleme için sipariş içeriklerini almak ve o sırada bir sipariş veritabanında depolayın.

Geçerli ve işlevsel bir mantıksal uygulama bir tetikleyici ve en az bir eylem gerektirir. Bu nedenle, bir tetikleyici ekledikten sonra Eylem Ekle emin olun.

Bu tetikleyiciyi gösteren bir örnek aşağıdadır: **ne zaman bir dosya eklendiğinde veya değiştirildiğinde**

1. Oturum [Azure portalında](https://portal.azure.com)ve Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açın, açık değilse.

1. Boş mantıksal uygulama için arama kutusuna filtreniz olarak "ftp" girin. Tetikleyiciler listesinde şu tetikleyiciyi seçin: **ne zaman bir dosyalanmış eklendiğinde veya değiştirildiğinde - FTP**

   ![Bulma ve FTP tetikleyicisini seçin](./media/connectors-create-api-ftp/select-ftp-trigger.png)  

1. Bağlantınız için gerekli bilgileri sağlayın ve ardından **Oluştur**.

   ![FTP sunucusu bağlantı oluşturma](./media/connectors-create-api-ftp/create-ftp-connection-trigger.png)  

1. Yanındaki **klasör** kutusunda, bir liste görünecek şekilde klasör simgesini seçin. Yeni ve düzenlenen dosyaları için izlemek istediğiniz klasörü bulmak üzere dik açılı oku seçin (**>**), bu klasöre göz atın ve ardından klasörü seçin.

   ![Bulmak ve izlemek için bir klasör seçin](./media/connectors-create-api-ftp/select-folder.png)  

   Seçilen klasörün görünür **klasör** kutusu.

   ![Seçili klasörü](./media/connectors-create-api-ftp/selected-folder.png)  

Mantıksal uygulamanızın tetikleyici vardır, mantıksal uygulamanız yeni veya düzenlenmiş bir dosya bulduğunda çalıştırmak istediğiniz eylemler ekleyin. Bu örnekte, yeni veya güncelleştirilmiş içeriği alır bir FTP eylem ekleyebilirsiniz.

### <a name="ftp-action-get-content"></a>FTP eylem: içerik alma

Bu dosya eklendiğinde veya bu eylem bir FTP sunucusuna dosya içeriği alır. Örneğin, önceki örnekte ve bu dosyayı eklenmiş veya düzenlenmişse sonra dosyanın içeriğini alır bir eylem tetikleyici ekleyebilirsiniz. 

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

Tetikleyiciler ve Eylemler sınırları hakkında teknik ayrıntılar için bağlayıcının Openapı'nin açıklanmıştır (önceki adıyla Swagger) açıklama, bağlayıcının gözden [başvuru sayfası](/connectors/ftpconnector/).

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer hakkında bilgi edinin [Logic Apps bağlayıcıları](../connectors/apis-list.md)