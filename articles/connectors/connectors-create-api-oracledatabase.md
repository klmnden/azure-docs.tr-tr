---
title: Oracle veritabanına - Azure mantıksal uygulamaları | Microsoft Docs
description: INSERT ve Oracle veritabanı REST API'leri ve Azure Logic Apps ile kayıtlarını yönetme
author: ecfan
manager: cfowler
ms.author: estfan
ms.date: 03/29/2017
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: be099cb8c2a36fa8bc09a5a30fca1785dd3933db
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34609565"
---
# <a name="get-started-with-the-oracle-database-connector"></a>Oracle Veritabanı Bağlayıcısı ile çalışmaya başlama

Oracle Veritabanı Bağlayıcısı'nı kullanarak, varolan bir veritabanında veri kullanan kurumsal iş akışları oluşturun. Bu bağlayıcı, şirket içi Oracle veritabanı veya bir Azure sanal makinesi için Oracle veritabanı ile bağlanabilir. Bu bağlayıcı ile şunları yapabilirsiniz:

* Müşteriler veritabanına yeni bir müşteri ekleyerek veya bir sırada siparişler veritabanını güncelleştirmek, iş akışı oluşturma.
* Bir satır veri almak, yeni bir satır ekleyin ve hatta silmek için Eylemler kullanın. Örneğin, bir kayıt Dynamics CRM Online içinde (tetikleyici) oluşturulduğunda, bir satır bir Oracle veritabanına (bir eylem) ekleyin. 

Bu makalede bir mantıksal uygulama Oracle Veritabanı Bağlayıcısı'nı kullanmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar

* Desteklenen Oracle sürümleri: 
    * Oracle 9 ve sonraki sürümler
    * Oracle istemci yazılımı 8.1.7 ve sonraki sürümler

* Şirket içi veri ağ geçidini yükleyin. [Şirket içi veri mantığı uygulamalardan bağlanmak](../logic-apps/logic-apps-gateway-connection.md) adımlar listelenmektedir. Ağ geçidi şirket içi Oracle veritabanına bağlanmak için gereken veya bir Azure VM Oracle DB ile yüklü. 

    > [!NOTE]
    > Şirket içi veri ağ geçidi köprü olarak davranır ve şirket içi veri (buluta değil veri) ve logic apps arasında güvenli veri aktarımını sağlar. Aynı ağ geçidi, birden çok hizmet ve birden çok veri kaynağı ile kullanılabilir. Bu nedenle, yalnızca bir kez ağ geçidi yüklemeniz gerekebilir.

* Oracle istemcisi şirket içi veri ağ geçidinin yüklendiği makineye yükleyin. Oracle .NET için 64-bit Oracle veri sağlayıcısı yüklediğinizden emin olun:  

  [Windows x64 için 64-bit ODAC 12c sürüm 4 (12.1.0.2.4)](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Oracle istemcisi yüklü değilse, oluşturmak veya bağlantı kullanmak çalıştığınızda hata oluşur. Bu makalede yaygın hatalar da görebilirsiniz.


## <a name="add-the-connector"></a>Bağlayıcısını ekleyin

> [!IMPORTANT]
> Bu bağlayıcı hiçbir tetikleyici yok. Yalnızca eylem yok. Mantıksal uygulamanızı oluşturduğunuzda, bu nedenle mantıksal uygulamanızı gibi başlatmak için başka bir tetikleyici ekleyin **çizelgesi - yinelenme**, veya **istek / yanıt - yanıt**. 

1. İçinde [Azure portal](https://portal.azure.com), boş mantıksal uygulama oluşturma.

2. Mantıksal uygulamanızı başlangıcında seçin **istek / yanıt - istek** tetikleyici: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. **Kaydet**’i seçin. Kaydettiğinizde, bir istek URL'sini otomatik olarak oluşturulur. 

4. Seçin **yeni adım**seçip **Eylem Ekle**. Yazın `oracle` kullanılabilir eylemleri görmek için: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Bu ayrıca tetikleyiciler ve Eylemler herhangi bir bağlayıcı için kullanılabilir görmek için en hızlı yoludur. Bağlayıcı adı bölümünde gibi yazın `oracle`. Tasarımcı hiçbir tetikleyici ve eylemleri listeler. 

5. Eylemlerden birini gibi seçin **Oracle veritabanı - Get satır**. Seçin **Connect şirket içi veri ağ geçidi üzerinden**. Oracle Sunucusu adı, kimlik doğrulama yöntemi, kullanıcı adı, parola girin ve ağ geçidi seçin:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. Bağlantı kurulduktan sonra listeden bir tablo seçin ve tablonuza satır kimliği girin. Tabloya tanımlayıcı bilmeniz gerekir. Bilmiyorsanız, Oracle DB yöneticinize başvurun ve çıktısını almak `select * from yourTableName`. Bu, devam etmek için gereken kişisel bilgileri sağlar.

    Aşağıdaki örnekte, İnsan Kaynakları veritabanından iş veri döndürülmüyor: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. Bu sonraki adımda herhangi diğer bağlayıcıları, iş akışını oluşturmak için kullanabilirsiniz. Oracle alma verilerden test etmek isterseniz, daha sonra kendiniz bir e-posta gönderme e-posta bağlayıcıları, böyle bir Office 365 veya Gmail birini kullanarak Oracle verilerle gönderin. Oracle tablosundan dinamik belirteçleri oluşturmak için kullanmak `Subject` ve `Body` e-postanızın:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. **Kaydet** mantıksal uygulama ve ardından **çalıştırmak**. Tasarımcı kapatın ve durum çalıştırır geçmişi bakın. Başarısız olursa, başarısız ileti satırını seçin. Tasarımcı açar ve hangi adım başarısız oldu ve hata bilgilerini de gösterir gösterir. Başarılı olursa, eklenen bilgiler içeren bir e-posta almanız gerekir.


### <a name="workflow-ideas"></a>İş akışı fikirleri

* #Oracle diyez izlemek ve bunlar sorgulanan ve diğer uygulamalar içinde kullanılan şekilde tweet'leri bir veritabanında yerleştirmek istediğiniz. Bir mantıksal uygulama Ekle `Twitter - When a new tweet is posted` tetikleyebilir ve girin **#oracle** diyez. Ardından, ekleyin `Oracle Database - Insert row` eylemi ve tablonuzu seçin:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* Service Bus kuyruğuna gönderilen iletileri. Bu iletileri almak ve onları bir veritabanında koymak istiyor. Bir mantıksal uygulama Ekle `Service Bus - when a message is received in a queue` tetikleyebilir ve sırayı seçin. Ardından, ekleyin `Oracle Database - Insert row` eylemi ve tablonuzu seçin:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Sık karşılaşılan hataları

#### <a name="error-cannot-reach-the-gateway"></a>**Hata**: ağ geçidi ulaşamıyor

**Neden**: şirket içi veri ağ geçidi buluta bağlanabiliyor değil. 

**Azaltma**: ağ geçidiniz burada yükler ve internet'e bağlanabilen şirket içi makinede çalıştığından emin olun.  Ağ geçidi, bir bilgisayar kapalı olabilir veya uyku yüklenmiyor öneririz. Ayrıca, şirket içi veri Ağ Geçidi Hizmeti (PBIEgwService) yeniden başlatabilirsiniz.

#### <a name="error-the-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-see-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-to-install-the-official-provider"></a>**Hata**: kullanılmakta olan sağlayıcı kullanım dışıdır: ' System.Data.OracleClient gerektirir Oracle istemci yazılımı 8.1.7 veya daha büyük.'. Bkz: [ https://go.microsoft.com/fwlink/p/?LinkID=272376 ](https://go.microsoft.com/fwlink/p/?LinkID=272376) resmi sağlayıcıyı yüklemek için.

**Neden**: Oracle istemci SDK şirket içi veri ağ geçidi çalıştığı makinede yüklü değil.  

**Çözümleme**: indirin ve şirket içi veri ağ geçidi ile aynı bilgisayarda Oracle istemci SDK'sını yükleyin.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Hata**: '[Tablename]' tablosu tüm anahtar sütunlarını tanımlamak değil

**Neden**: tablosunda hiçbir birincil anahtar yok.  

**Çözümleme**: Oracle veritabanı connector, birincil anahtar sütunu içeren bir tablo kullanılmasını gerektirir.

#### <a name="currently-not-supported"></a>Şu anda desteklenmiyor

* Görünümleri ve saklı yordamlar 
* Bileşik anahtarlar ile herhangi bir tabloyu
* İç içe nesne türlerinin tablolardaki
 
## <a name="connector-specific-details"></a>Bağlayıcı özgü ayrıntıları

Tüm tetikleyiciler ve Eylemler swagger tanımlanan görüntüleyebilir ve ayrıca herhangi bir sınır bkz [Bağlayıcısı ayrıntıları](/connectors/oracle/). 

## <a name="get-some-help"></a>Bazı Yardım alın

[Azure Logic Apps Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) sorular sormak, soruları yanıtlayın ve diğer Logic Apps kullanıcıların ne yaptıklarını görmek için harika bir yerdir. 

Logic Apps ve bağlayıcıları oylama ve fikirlerinizi adresindeki gönderme tarafından artırmaya yardımcı olabilir [ http://aka.ms/logicapps-wish ](http://aka.ms/logicapps-wish). 


## <a name="next-steps"></a>Sonraki adımlar
[Mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)ve Logic Apps içinde kullanılabilir bağlayıcılar keşfetme [API'leri listesi](apis-list.md).
