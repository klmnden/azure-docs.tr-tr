---
title: FTP - Azure Logic Apps sunucusuna | Microsoft Docs
description: Oluşturabilir, izleyebilir ve Azure Logic Apps ile bir FTP sunucusunda dosyaları yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 07/22/2016
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 4355a767d2ecd500662cdf4522e8a7e12de86b80
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866160"
---
# <a name="get-started-with-the-ftp-connector"></a>FTP Bağlayıcısı ile çalışmaya başlama
FTP Bağlayıcısı, izleme, yönetme ve dosyaları bir FTP sunucusunda oluşturmak için kullanın. 

Kullanılacak [bağlayıcıyı](apis-list.md), ilk mantıksal uygulamanızı oluşturmak gerekir. Tarafından oluşturabileceğinize dair [artık bir mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-ftp"></a>FTP bağlanma
Mantıksal uygulamanızı herhangi bir hizmete erişebilmeniz için önce ilk oluşturmak için ihtiyacınız bir *bağlantı* hizmeti. A [bağlantı](connectors-overview.md) bir mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar.  

### <a name="create-a-connection-to-ftp"></a>FTP bağlantı oluşturun
> [!INCLUDE [Steps to create a connection to FTP](../../includes/connectors-create-api-ftp.md)]
> 
> 

## <a name="use-a-ftp-trigger"></a>FTP tetikleyicisini kullanma
Bir tetikleyici bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılan bir olaydır. [Tetikleyiciler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!IMPORTANT]
> FTP Bağlayıcısı Internet'ten erişilebilir ve Pasif modu ile çalışmak için yapılandırılmış bir FTP sunucusu gerektirir. Ayrıca, FTP Bağlayıcısı, **örtük FTPS (SSL üzerinden FTP) ile uyumlu değil**. FTP Bağlayıcısı, yalnızca açık FTPS (SSL üzerinden FTP) destekler.  
> 
> 

Bu örnekte, ı size nasıl kullanılacağını gösterir **FTP - bir dosya eklendiğinde veya değiştirildiğinde** bir dosya eklendiğinde veya değiştirildiğinde, bir FTP sunucusuna bir mantıksal uygulama iş akışı başlatmak için tetikleyin. Kurumsal bir örnekte, bu tetikleyiciyi müşterilerden siparişleri temsil eden yeni dosyaları bir FTP klasörü izlemek için kullanabilirsiniz.  Ardından bir FTP Bağlayıcısı eylem gibi kullanabilir **dosya içeriğini Al** siparişler veritabanınızda daha fazla işleme ve depolama sırasını içeriğini almak için.

1. Girin *ftp* logic apps Tasarımcısı'nda arama kutusuna seçip **bir dosya eklendiğinde veya FTP -** tetikleyicisi   
   ![FTP tetikleyici görüntü 1](./media/connectors-create-api-ftp/ftp-trigger-1.png)  
   **Ne zaman bir dosya eklendiğinde veya değiştirildiğinde** denetimi açılır  
   ![2 FTP tetikleyicisi görüntüsü](./media/connectors-create-api-ftp/ftp-trigger-2.png)  
2. Seçin **...**  denetimin sağ tarafında bulunur. Bu klasör seçici denetimi açar  
   ![FTP tetikleyicisi görüntüsü 3](./media/connectors-create-api-ftp/ftp-trigger-3.png)  
3. Seçin **>** (sağ ok) için yeni veya değiştirilmiş dosyaları izlemek istediğiniz klasörü bulmak üzere göz atın. Klasör gösterilen artık dikkat edin ve klasör seçin **klasör** denetimi.  
   ![FTP tetikleyicisi görüntüsü 4](./media/connectors-create-api-ftp/ftp-trigger-4.png)   

Bu noktada, mantıksal uygulamanızın bir dosya değiştirildiğinde veya belirli bir FTP klasöründe oluşturulan bir tetikleyici ve Eylemler iş akışı çalıştırma başlayacak bir tetikleyici ile yapılandırıldı. 

> [!NOTE]
> Bir mantıksal uygulama işlevsel olması en az bir tetikleyici ve bir eylem içermelidir. Bir eylem eklemek için sonraki bölümdeki adımları izleyin.  
> 
> 

## <a name="use-a-ftp-action"></a>FTP eylemini kullanma
Bir eylem, bir mantıksal uygulamada tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

Bir tetikleyici ekledikten sonra Tetikleyici tarafından bulunan yeni veya değiştirilen dosyanın içeriğini alır bir eylem eklemek için bu adımları izleyin.    

1. Seçin **+ yeni adım** FTP sunucusundaki dosyanın içeriklerini almak için bir eylem eklemek için  
2. Seçin **Eylem Ekle** bağlantı.  
   ![FTP eylem görüntü 1](./media/connectors-create-api-ftp/ftp-action-1.png)  
3. Girin *FTP* FTP ilgili tüm eylemleri aramak için.
4. Seçin **FTP - dosya içeriğini Al** ne zaman gerçekleştirilecek eylemi yeni veya değiştirilmiş bir dosyanın FTP klasöründe bulunur.      
   ![FTP eyleminin resmi 2](./media/connectors-create-api-ftp/ftp-action-2.png)  
   **Dosya içeriğini Al** denetim açar. **Not**: mantıksal uygulamanızı, daha önce yapmadıysanız, FTP sunucusu hesabınıza erişmesine yetki istenecektir.  
   ![FTP eylem görüntüsü 3](./media/connectors-create-api-ftp/ftp-action-3.png)   
5. Seçin **dosya** denetimi (boşluk bulunan aşağıdaki ** dosya ***). Burada, FTP sunucusunda bulunan yeni veya değiştirilmiş dosyadan çeşitli özelliklerden herhangi birini kullanabilirsiniz.  
6. Seçin **dosya içeriği** seçeneği.  
   ![FTP eyleminin resmi 4](./media/connectors-create-api-ftp/ftp-action-4.png)   
7. Gösteren denetimi güncelleştirilir **FTP - dosya içeriğini Al** eylem alacak *dosya içeriği* FTP sunucusunda yeni veya değiştirilen dosyanın.      
   ![FTP eyleminin resmi 5](./media/connectors-create-api-ftp/ftp-action-5.png)     
8. Çalışmanızı kaydedin, sonra iş akışınızı test etmek için FTP klasörüne bir dosya ekleyin.    

Bu noktada, mantıksal uygulama, bir FTP sunucusundaki bir klasöre izlemek ve yeni bir dosya ya da değiştirilmiş bir dosya FTP sunucusunda bulduğunda iş akışını başlatmak için bir tetikleyici ile yapılandırıldı. 

Mantıksal uygulama, ayrıca yeni veya değiştirilen dosyanın içeriklerini almak için bir eylem ile yapılandırıldı.

Artık başka bir eylem gibi ekleyebilirsiniz [SQL Server - Satır Ekle](connectors-create-api-sqlazure.md) yeni veya değiştirilmiş dosyasının içeriğini bir SQL veritabanı tablosuna eklemek için eylem.  

## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/ftpconnector/). 

## <a name="next-steps"></a>Sonraki Adımlar
[Mantıksal uygulama oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

