---
title: Oracle veritabanı - Azure Logic Apps | Microsoft Docs
description: Ekle ve Oracle veritabanı REST API'ler ve Azure Logic Apps ile kayıtlarını yönetme
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 03/29/2017
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 06f65aef203b4f0d765f21b9d17b90081de85c94
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60453645"
---
# <a name="get-started-with-the-oracle-database-connector"></a>Oracle Veritabanı Bağlayıcısı ile çalışmaya başlama

Oracle Veritabanı Bağlayıcısı'nı kullanarak, varolan bir veritabanında veri kullanan kurumsal iş akışları oluşturun. Bu bağlayıcı, şirket içi Oracle veritabanı veya bir Azure sanal makinesinde Oracle veritabanı ile bağlanabilirsiniz. Bu bağlayıcıyı kullanarak, şunları yapabilirsiniz:

* Bir müşteri veritabanına yeni bir müşteri eklemek veya bir sipariş veritabanındaki sipariş güncelleştiriliyor, iş akışınızı oluşturun.
* Bir satır veri almak, yeni bir satır ekleyin ve hatta silebilir eylemlerini kullanın. Örneğin, Dynamics CRM Online'da (tetikleyici) bir kayıt oluşturulduğunda bir satır bir Oracle veritabanına (bir eylem) ekleyin. 

Bu makalede, Oracle veritabanı bağlayıcının bir mantıksal uygulamada kullanma işlemini göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

* Desteklenen Oracle sürümleri: 
    * Oracle 9 ve üstü
    * Oracle istemci yazılımı 8.1.7 ve sonraki sürümler

* Şirket içi veri ağ geçidi yükleyin. [Mantıksal uygulamalardan şirket içi verilere bağlanma](../logic-apps/logic-apps-gateway-connection.md) adımları listelenir. Ağ geçidi, şirket içi Oracle veritabanına bağlanmak için gereklidir ve Oracle DB ile bir Azure VM'yi yüklü. 

    > [!NOTE]
    > Şirket içi veri ağ geçidi bir köprü görevi görür ve şirket içi veriler (bulutta olmayan veriler) ve logic apps arasında güvenli veri aktarımı sağlar. Aynı ağ geçidi, birden çok hizmet ve birden çok veri kaynağı ile kullanılabilir. Bu nedenle, yalnızca bir kez ağ geçidi yüklemeniz gerekebilir.

* Oracle istemcisini şirket içi veri ağ geçidinin yüklü olduğu bir makineye yükleyin. .NET Oracle için 64-bit Oracle veri sağlayıcısı'nı yüklediğinizden emin olun:  

  [Windows x64 için 64-bit ODAC 12c sürüm 4 (12.1.0.2.4)](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Oracle istemcisi yüklü değilse, oluşturmak veya bağlantısı çalıştığınızda hata oluşur. Bu makalede sık karşılaşılan bakın.


## <a name="add-the-connector"></a>Bağlayıcı Ekle

> [!IMPORTANT]
> Bu bağlayıcı, herhangi bir tetikleyici yok. Bu, yalnızca eylemleri vardır. Mantıksal uygulamanızı oluşturduğunuzda, bu nedenle gibi mantıksal uygulamanızı başlatmak için başka bir tetikleyici ekleme **zamanlama - yinelenme**, veya **istek / yanıt - yanıt**. 

1. İçinde [Azure portalında](https://portal.azure.com), boş bir mantıksal uygulama oluşturun.

2. Mantıksal uygulamanızı başlangıcında seçin **istek / yanıt - istek** tetikleyici: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. **Kaydet**’i seçin. İstek URL'si, kaydettiğinizde, otomatik olarak oluşturulur. 

4. Seçin **yeni adım**seçip **Eylem Ekle**. Yazın `oracle` kullanılabilir eylemleri görmek için: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Bu da herhangi bir bağlayıcı için kullanılabilir eylemleri ve Tetikleyicileri görmek için en hızlı yoludur. Bağlayıcı adı bir parçası yazın `oracle`. Tasarımcı, hiçbir tetikleyici ve eylemleri listeler. 

5. Eylemler, biri gibi seçin **Oracle Database - Get satır**. Seçin **şirket içi veri ağ geçidi üzerinden Bağlan**. Oracle Sunucusu adı, kimlik doğrulama yöntemi, kullanıcı adı, parola girin ve ağ geçidini seçin:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. Bağlandıktan sonra listeden bir tablo seçin ve tablonuza satır kimliği girin. Tablo tanımlayıcısı bilmeniz gerekir. Bilmiyorsanız, Oracle DB yöneticinize başvurun ve çıktısını almak `select * from yourTableName`. Bu, devam etmeniz için gereken bilgiler sağlar.

    Aşağıdaki örnekte, iş verilerini, İnsan Kaynakları veritabanından döndürülüyor: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. Bu sonraki adımda diğer bağlayıcılardan herhangi biri, iş akışınızı oluşturmak için kullanabilirsiniz. Veri Oracle almayı test etmek isterseniz, sonra kendinize bir e-posta Gönder e-posta bağlayıcıların böyle bir Office 365 veya Gmail birini kullanarak Oracle verileri gönderin. Oracle tablosu gelen dinamik belirteçleri oluşturmak için kullanın `Subject` ve `Body` e-postanızın:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. **Kaydet** mantıksal uygulama ve ardından **çalıştırma**. Tasarımcı kapatın ve durumu için çalıştırma geçmişi bakın. Başarısız olursa, başarısız iletisi satırı seçin. Tasarımcı açılır ve hangi adım başarısız oldu ve hata bilgilerini de gösterir gösterir. Başarılı olursa, eklenen bilgiler içeren bir e-posta almanız gerekir.


### <a name="workflow-ideas"></a>İş akışı fikir

* #Oracle diyez etiketi izleyin ve bunlar sorgulanabilen ve diğer uygulamaları içinde kullanılan şekilde tweetleri bir veritabanında koymak istiyorsanız. Bir mantıksal uygulama, ekleme `Twitter - When a new tweet is posted` tetikleyin ve girin **#oracle** diyez etiketi. Ardından, ekleme `Oracle Database - Insert row` eylem ve tablonuzu seçme:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* Bir Service Bus kuyruğuna iletiler gönderilir. Bu iletiler alın ve onları bir veritabanında koymak istiyorsanız. Bir mantıksal uygulama, ekleme `Service Bus - when a message is received in a queue` tetikleyin ve kuyruk seçin. Ardından, ekleme `Oracle Database - Insert row` eylem ve tablonuzu seçme:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Sık karşılaşılan hatalar

#### <a name="error-cannot-reach-the-gateway"></a>**Hata**: Ağ geçidine erişilemiyor

**Neden**: Şirket içi veri ağ geçidi buluta bağlayın mümkün değil. 

**Risk azaltma**: Burada, yüklediğiniz ve internet'e bağlanabilen şirket içi makinede ağ geçidinizin çalıştığından emin olun.  Ağ geçidi, bir bilgisayar kapalı olabilir veya uyku yüklenmiyor öneririz. Ayrıca, şirket içi veri Ağ Geçidi Hizmeti (PBIEgwService) yeniden başlatabilirsiniz.

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-see-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a>**Hata**: Kullanılan sağlayıcı kullanım dışıdır: ' System.Data.OracleClient, Oracle istemci version 8.1.7 gerektirir veya büyük.'. Bkz: [ https://go.microsoft.com/fwlink/p/?LinkID=272376 ](https://go.microsoft.com/fwlink/p/?LinkID=272376) resmi sağlayıcıyı yükleyin.

**Neden**: Şirket içi veri ağ geçidinin çalıştırıldığı makinede Oracle istemci SDK'sı yüklü değil.  

**Çözüm**: İndirin ve şirket içi veri ağ geçidi ile aynı bilgisayarda Oracle istemci SDK'sını yükleyin.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Hata**: '[Tablename]' tablosu hiçbir anahtar sütununu tanımlamıyor

**Neden**: Tabloda bir birincil anahtar yok.  

**Çözüm**: Oracle Veritabanı Bağlayıcısı, bir birincil anahtar sütunu içeren bir tablo kullanılmasını gerektirir.

#### <a name="currently-not-supported"></a>Şu anda desteklenmiyor

* Görünümler 
* Bileşik anahtarlar içeren herhangi bir tablo
* Tablolardaki iç içe nesne türleri
 
## <a name="connector-specific-details"></a>Özel bağlayıcı ayrıntıları

Tetikleyiciler ve eylemlerin swagger'da tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınırlama Bkz [bağlayıcı ayrıntıları](/connectors/oracle/). 

## <a name="get-some-help"></a>Bazı Yardım alın

[Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) sorular sormak, soruları yanıtlamak ve diğer Logic Apps kullanıcılarının neler yaptığını görmek için harika bir yerdir. 

Oylama ve fikirlerinizi, gönderme Logic Apps ve bağlayıcıları geliştirmeye yardımcı [ https://aka.ms/logicapps-wish ](https://aka.ms/logicapps-wish). 


## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md), Logic Apps sürümünde kullanılabilen bağlayıcıları keşfedin [API listesi](apis-list.md).
