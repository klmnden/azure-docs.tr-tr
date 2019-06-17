---
title: Azure DevTest labs'deki autoshutdown ilkelerini yönetme | Microsoft Docs
description: Böylece bunlar kullanımda olmadığında sanal makineleri otomatik olarak kapatılır Laboratuvar için autoshutdown İlkesi ayarlama konusunda bilgi edinin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2019
ms.author: spelluru
ms.openlocfilehash: 9adf8dd4a5a3c469ed130b29308a0d828aee40bf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873998"
---
# <a name="manage-autoshutdown-policies-for-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara yönelik autoshutdown ilkeleri yönetme
Azure DevTest Labs, maliyet denetlemek ve her bir laboratuvar ilkelerini (ayarlar) yöneterek, Labs boşa harcamayı sağlar. Bu makalede bir laboratuvar hesabı autoshutdown ilkesi yapılandırın ve Laboratuvar hesabında autoshutdown ayarları bir laboratuvara yönelik yapılandırma gösterilmektedir. Her laboratuar ilkesini ayarlama görüntülemek için bkz [Azure DevTest Labs'de Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md).  

## <a name="set-auto-shutdown-policy-for-a-lab"></a>Bir laboratuvara yönelik otomatik kapatma ilkesi ayarlama
Laboratuvar sahibi olarak, tüm VM'ler için laboratuvarda bir kapanma zamanlaması yapılandırabilirsiniz. Bunu yaptığınızda, kullanılmayan makineler çalışıyor (boşta öğesinden) maliyetlerinden tasarruf edebilirler. Laboratuvarınız sanal makineleri merkezi olarak aynı zamanda Kaydet Laboratuvar kullanıcılarınızın çaba, tek tek makineler için bir zamanlama ayarından tüm bir kapatma ilke uygulayabilir. Bu özellik, Laboratuvar kullanıcılarınıza tam denetim için bir denetim yok sunmasını başlangıç Laboratuvar zamanlamanızı ilkesi ayarlamanıza olanak sağlar. Laboratuvar sahibi olarak, aşağıdaki adımları izleyerek bu ilke yapılandırabilirsiniz:

1. Laboratuvarınız için giriş sayfasında, seçin **yapılandırması ve ilkelerini**.
2. Seçin **otomatik kapatma ilke** içinde **zamanlamaları** soldaki menüden bölümü.
3. Seçeneklerden birini seçin. Aşağıdaki bölümlerde, bu seçenekler hakkında daha fazla ayrıntı sağlar: İlkesi ayarlama, yalnızca laboratuvar'da oluşturulan yeni vm'lere ve zaten var olan VM'ler için geçerlidir. 

    ![Otomatik kapatma ilkesi seçenekleri](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

## <a name="configure-auto-shutdown-settings"></a>Otomatik kapanma ayarlarını yapılandırma
Bu Laboratuvar Vm'leri kapatmak süreyi belirtmenize olanak tanıyarak Laboratuvar israfı en aza indirmek için autoshutdown ilke yardımcı olur.

(Görüntüleyip değiştirmek için) bir laboratuvara yönelik ilkeleri, aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin.   
4. Seçin **yapılandırması ve ilkelerini**.

    ![İlke ayarları bölmesi](./media/devtest-lab-set-lab-policy/policies-menu.png)
5. Laboratuvar'ın **yapılandırması ve ilkelerini** bölmesinde **otomatik kapatma** altında **zamanlamaları**.
   
    ![Autoshutdown](./media/devtest-lab-set-lab-policy/auto-shutdown.png)
6. Seçin **üzerinde** Bu ilkeyi etkinleştirmek için ve **kapalı** devre dışı.
7. Bu ilkeyi etkinleştirmek, geçerli bir laboratuvar içindeki tüm sanal makineleri kapatmaya saat (ve saat dilimi) belirtin.
8. Belirtin **Evet** veya **Hayır** seçeneği belirtilen autoshutdown saatten önce 15 dakika bildirim göndermek için. Seçerseniz **Evet**, bir Web kancası URL uç noktasını girin veya e-posta adresi, gönderilen veya gönderilmesi bildirim istediğiniz belirtme. Kullanıcı bildirimi alır ve kapatma erteleme seçeneği verilir. Daha fazla bilgi için [bildirimleri](#notifications) bölümü. 
9. **Kaydet**’i seçin.

    Varsayılan olarak, geçerli Laboratuvardaki tüm sanal makineler için bu ilke etkinleştirildikten sonra uygulanır. Bu ayar, belirli bir sanal makineden kaldırmak için sanal makinenin yönetim bölmesini açın ve değiştirmek, **Autoshutdown** ayarı.

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve iptal edebilir
Bu ilke için Laboratuvarınızı ayarlarsanız, Laboratuvar kullanıcıları geçersiz kılabilir veya Laboratuvar zamanlama dışında iyileştirilmiş. Bu seçenek Laboratuvar kullanıcıları kendi sanal makinelerinin otomatik kapatma zamanlamasını üzerinde tam denetim verir. Laboratuvar kullanıcıları herhangi bir değişiklik, VM otomatik kapanma zamanlaması sayfasında bakın.

![İlke seçeneği - 1 otomatik Kapat](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Kullanıcı bir zamanlama ayarlar ve çıkma olamaz
Bu ilke için Laboratuvarınızı ayarlarsanız, Laboratuvar kullanıcıları Labs zamanlaması geçersiz kılabilirsiniz. Ancak, bunlar otomatik kapatma ilke dışı bırakamazlar. Bu seçenek laboratuvarınızda her makineye bir otomatik kapatma zamanlamasını altında olduğundan emin olur. Laboratuvar kullanıcıları kendi sanal makinelerinin otomatik kapatma zamanlamasını güncelleştirme ve kapatma bildirimlerini ayarlama.

![İlke seçeneği - 2 otomatik Kapat](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Kullanıcı Laboratuvar Yöneticisi tarafından ayarladığı zamanlamayı üzerinde denetimi yoktur
Bu ilke için Laboratuvarınızı ayarlarsanız, Laboratuvar kullanıcıları geçersiz kılabilir veya Laboratuvar zamanlama dışında iyileştirilmiş. Bu seçenek, Laboratuvar Yönetim zamanlamaya Laboratuvardaki her makine için tam denetim sunar. Laboratuvar kullanıcıları yalnızca kendi sanal makineleri için otomatik kapatma bildirimler ayarlayabilirsiniz.

![İlke seçeneği - 3 otomatik Kapat](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="notifications"></a>Bildirimler
Laboratuvar sahibi tarafından ayarlanan autoshutdown, bildirimleri Laboratuvar kullanıcılar Vm'lerini varsa tetiklenen autoshutdown önce 15 dakika gönderdikten sonra etkilenecek. Bu seçenek, Laboratuvar kullanıcıları kapatmadan önce çalışmalarını kaydetmeleri için bir fırsat sağlar. Bildirim ayrıca her VM için aşağıdaki eylemleri için bağlantılar sağlar:

- Bu süre autoshutdown atla
- Böylece VM üzerinde çalışıyorum autoshutdown bir saat veya 2 saat için erteleme süresi.

Yapılandırılmış web kancası uç noktası aracılığıyla bildirim gönderilir ya da autoshutdown ayarlarında Laboratuvar sahibi tarafından bir e-posta adresi belirtildi. Web kancaları, yapı veya belirli olaylara abone tümleştirmeler ayarlamak olanak sağlar. Bu olaylardan biri tetiklendiğinde DevTest Labs bir HTTP POST yükü Web kancası'nın yapılandırılmış URL'sine gönderin. Web kancaları hakkında daha fazla bilgi için bkz: [bir Web kancası veya API Azure işlevi oluşturma](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

Çünkü bunlar, çeşitli uygulamalar tarafından (örneğin, Slack, Azure Logic Apps ve benzeri) yaygın olarak desteklenen ve bildirim göndermek için kendi şekilde olanak tanır, web kancası kullanmanızı öneririz. Örneğin, bu makalede, Azure Logic Apps kullanarak autoshutdown bildirim e-postaları almak konusunda yol göstermektedir. İlk olarak, şimdi hızlı bir şekilde laboratuvarınızda autoshutdown bildirimini etkinleştirmek için temel adımları inceleyin.   

## <a name="create-a-logic-app-that-receives-email-notifications"></a>E-posta bildirimleri alan bir mantıksal uygulama oluşturma
[Azure Logic Apps](../logic-apps/logic-apps-overview.md) kolaylaştırır çok sayıda kullanıma hazır bağlayıcılar, Office 365 ve twitter gibi diğer istemcilerle bir hizmet tümleştirmek sağlar. Yüksek düzeyde bir mantıksal uygulama için e-posta bildirimi ayarlamak için adımları dört aşamaya ayrılabilir: 

- Mantıksal uygulama oluşturun. 
- Yerleşik şablonu yapılandırın.
- E-posta istemcinizi ile tümleştirin
- Web kancası URL'sini alın.

### <a name="create-a-logic-app"></a>Mantıksal uygulama oluşturma
Başlamak için aşağıdaki adımları kullanarak Azure aboneliğinizde bir mantıksal uygulama oluşturun:

1. Seçin **+ kaynak Oluştur** sol menüsünde **tümleştirme**seçip **mantıksal uygulama**. 

    ![Yeni mantıksal uygulama menüsünde](./media/devtest-lab-auto-shutdown/new-logic-app.png)
2. Üzerinde **- mantıksal uygulama oluşturma** sayfasında, aşağıdaki adımları izleyin: 
    1. Girin bir **adı** mantıksal uygulama için.
    2. Azure **aboneliğinizi** seçin.
    3. Yeni bir **kaynak grubu** veya mevcut bir kaynak grubunu seçin. 
    4. Seçin bir **konumu** mantıksal uygulama için. 

        ![Yeni mantıksal uygulama - ayarlar](./media/devtest-lab-auto-shutdown/new-logic-app-page.png)
3. İçinde **bildirimleri**seçin **kaynağa Git** bildirim. 

    ![Kaynağa git](./media/devtest-lab-auto-shutdown/go-to-resource.png)
4. Seçin **mantıksal Uygulama Tasarımcısı** altında **dağıtım araçları** kategorisi.

    ![HTTP istek/yanıt seçin](./media/devtest-lab-auto-shutdown/select-http-request-response-option.png)
5. Üzerinde **HTTP istek-yanıt** sayfasında **bu şablonu kullan**. 

    ![Bu şablon seçeneği seçin](./media/devtest-lab-auto-shutdown/select-use-this-template.png)
6. İçine aşağıdaki JSON kopyalama **istek gövdesi JSON şeması** bölümü: 

    ```json
    {
        "$schema": "http://json-schema.org/draft-04/schema#",
        "properties": {
            "delayUrl120": {
                "type": "string"
            },
            "delayUrl60": {
                "type": "string"
            },
            "eventType": {
                "type": "string"
            },
            "guid": {
                "type": "string"
            },
            "labName": {
                "type": "string"
            },
            "owner": {
                "type": "string"
            },
            "resourceGroupName": {
                "type": "string"
            },
            "skipUrl": {
                "type": "string"
            },
            "subscriptionId": {
                "type": "string"
            },
            "text": {
                "type": "string"
            },
            "vmName": {
                "type": "string"
            }
        },
        "required": [
            "skipUrl",
            "delayUrl60",
            "delayUrl120",
            "vmName",
            "guid",
            "owner",
            "eventType",
            "text",
            "subscriptionId",
            "resourceGroupName",
            "labName"
        ],
        "type": "object"
    }
    ```
    
    ![İstek gövdesi JSON şeması](./media/devtest-lab-auto-shutdown/request-json.png)
7. Seçin **+ yeni adım** Tasarımcısı'nda aşağıdaki adımları izleyin:
    1. Arama **Office 365 Outlook - e-posta Gönder**. 
    2. Seçin **bir e-posta** gelen **eylemleri**. 
    
        ![E-posta seçeneği Gönder](./media/devtest-lab-auto-shutdown/select-send-email.png)
    3. Seçin **oturum** e-posta hesabınızda oturum açmak için. 
    4. Seçin **Kime** alan ve sahibi seçin.
    5. Seçin **konu**ve e-posta bildiriminin konu girin. Örneğin: "Makine vmName Laboratuvar için kapatma: labName."
    6. Seçin **GÖVDESİ**ve e-posta bildirimi postasının gövde içeriği tanımlayın. Örneğin: "15 dakika içinde kapatılacak şekilde vmName zamanlandı. Bu kapatma tıklayarak atla: URL. Bir saat için Gecikmeli kapatma: delayUrl60. 2 saat Gecikmeli kapatma: delayUrl120. "

        ![İstek gövdesi JSON şeması](./media/devtest-lab-auto-shutdown/email-options.png)
1. Araç çubuğunda **Kaydet**’i seçin. Şimdi, kopyalayabilirsiniz **HTTP POST URL'si**. URL'yi panoya kopyalamak için Kopyala düğmesini seçin. 

    ![Web kancası URL'si](./media/devtest-lab-auto-shutdown/webhook-url.png)

## <a name="next-steps"></a>Sonraki adımlar
Tüm ilkeleri hakkında bilgi edinmek için bkz: [Azure DevTest Labs'de Laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md).
