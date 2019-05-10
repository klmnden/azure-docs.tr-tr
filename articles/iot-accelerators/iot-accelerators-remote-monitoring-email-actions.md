---
title: E-posta eylemi içinde Uzaktan izleme - Azure | Microsoft Docs
description: Bu nasıl yapılır Kılavuzu, yeni veya mevcut bir kuralı için bir e-posta eylemi ekleme işlemini göstermektedir.
author: asdonald
manager: hegate
ms.author: asdonald
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/12/2018
ms.topic: conceptual
ms.openlocfilehash: fbb5f92258ff31dd7077bb1ade7fa7e5644c8bac
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466916"
---
# <a name="add-an-email-action"></a>Bir e-posta eylemi ekleme

E-posta eylemleri, uyarılar hiçbir kaçırmayın emin olmasına yardımcı olur. Mevcut bir kuralı veya yeni bir kural oluştururken bir e-posta eylem ekleyebilirsiniz.

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için Uzaktan izleme çözüm Hızlandırıcısını dağıtılan bir örneğini Azure aboneliğinizde gerekir.

Oluşturun veya bir kuralı değiştirmek için olmalıdır bir [ **yönetici**, ya da doğru izinlere sahip](iot-accelerators-remote-monitoring-rbac.md).

## <a name="edit-an-existing-rule"></a>Var olan kuralı düzenleme

Mevcut bir kuralı için bir e-posta eylem eklemek için aşağıdaki adımları izleyin:

1. Uzaktan izleme çözümünüzü gidin.

1. Gelen **Pano**, gitmek **kuralları** sayfası:

    ![Kurallar sayfası](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. Mevcut bir kuralı değiştirin ve ardından yanındaki onay kutusuna tıklayın **Düzenle** en üstünde. Düzenlenebilir bir **kural** paneli görüntülenir.

1. İçinde **eylem** bölümünde, geçiş **e-posta etkin** için **üzerinde**.

1. İlk kez etkinleştirdiğinizde çözüm Hızlandırıcı, bir e-posta eylemi gerekir [Outlook'a oturum](#outlook).

1. Tuşuna basın ve alıcı kutusu bir e-posta adresi girin **Enter** anahtar her e-posta adresi eklemek için:

    ![Adres girişi](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. Bir konu için e-posta girin.

1. Herhangi bir ek notlar için e-posta alıcılarını düz metin olarak girin. HTML, biçimlendirme kullanabilirsiniz, [e-posta şablonu Düzen](#htmledit).

1. Emin olun **kuralı durumu** ayarlanır **etkin**.

1. **Uygula**'ya tıklayın.

## <a name="create-a-new-rule"></a>Yeni kural oluşturma

Yeni bir kural oluştururken bir e-posta eylem eklemek için aşağıdaki adımları izleyin:

1. Uzaktan izleme çözümünüzü gidin.

1. Gelen **Pano**, gitmek **kuralları** sayfası:

    ![Kurallar sayfası](./media/iot-accelerators-remote-monitoring-email-actions/rules-email.png)

1. Bağlantısındaki [kuralı bir bölüm oluşturun](iot-accelerators-remote-monitoring-automate.md#create-a-rule). İzleme adımları [Gelişmiş bir kural oluşturmak](iot-accelerators-remote-monitoring-automate.md#create-an-advanced-rule) bölümünde ayarladığınız noktaya kadar bir **önem düzeyi**. Tıklamayın **Uygula** henüz.

1. İçinde **eylem** bölümünde, geçiş **e-posta etkin** için **üzerinde**.

1. İlk kez etkinleştirdiğinizde çözüm Hızlandırıcı, bir e-posta eylemi gerekir [Outlook'a oturum](#outlook).

1. Tuşuna basın ve alıcı kutusu bir e-posta adresi girin **Enter** anahtar her e-posta adresi eklemek için:

    ![Adres girişi](./media/iot-accelerators-remote-monitoring-email-actions/address-email.png)

1. Bir konu için e-posta girin.

1. Herhangi bir ek notlar için e-posta alıcılarını düz metin olarak girin. HTML, biçimlendirme kullanabilirsiniz, [e-posta şablonu Düzen](#htmledit).

1. Emin olun **kuralı durumu** ayarlanır **etkin**.

1. **Uygula**'ya tıklayın.

Kuralınıza bir e-posta eylemle şimdi etkinleştirildi. Eylem, her tetiklenişinde alıcılara yeni bir e-posta gönderilir.

## Outlook için oturum açın <a name="outlook"></a>

Çözüm hızlandırıcınız e-posta eylem etkinleştir ilk kez Outlook'a oturum açmanız gerekir. Bu eylem, e-posta bildirimleri gönderen e-posta hesabı ayarlar.

> [!NOTE]
> Çözüm Hızlandırıcı bildirimleri için yalnızca belirli bir Outlook hesabı oluşturmak ve ilk e-posta eyleminizi etkinleştirdiğinizde, o hesabı kullanmanız gerekir.

### <a name="contributor-role-outlook-setup"></a>Katkıda bulunan rolü Outlook Kurulumu

Birisi, **katkıda bulunan** abonelikte rolünün dağıtılan Çözüm Hızlandırıcısı, uygulama ayarlayın ve e-posta eylemleri ile Web kullanıcı arabirimini doğrulamak için yeterli izinlere sahip değil.

Başlamadan önce çözüm hızlandırıcınız e-posta bildirimleri göndermek için kullanmak üzere bir Outlook hesabı oluşturun.

Aşağıdaki adımlarda ayarlama ve e-posta eylemleri el ile doğrulama gösterilmektedir:

1. [Azure portalına](https://portal.azure.com) gidin.

1. Çözüm hızlandırıcınız için kaynak grubuna gidin.

1. Tıklayın **office365 Bağlayıcısı**:

    ![API Bağlantısı](./media/iot-accelerators-remote-monitoring-email-actions/apiconnector1.png)

1. Yetkilendirme işlemini başlatmak için başlığa tıklayın:

    ![Yetkilendirme](./media/iot-accelerators-remote-monitoring-email-actions/connector1.png)

1. Tıklayın **yetkilendirmek**. Oturum açmanız istenir. Oturum açmak için kullandığınız hesap, e-posta bildirimleri göndermek için e-posta adresi uygulamanın kullandığı olmalıdır:

    ![Düğme Yetkilendir](./media/iot-accelerators-remote-monitoring-email-actions/authorize.png)

1. Tıklayın **Kaydet** altındaki. Yetki başlığı yoksa başarılı olur.

1. İçinden bildirimleri gönderildiği gelen e-posta adresini değiştirmek için tıklayın **Düzenle API bağlantısı**.

    ![e-postasını Değiştir](./media/iot-accelerators-remote-monitoring-email-actions/editemail1.png)

### <a name="owner-role-outlook-setup"></a>Sahip rolü Outlook Kurulumu

Birisi, **sahibi** abonelikte rolünün dağıtılan Çözüm Hızlandırıcısı, uygulama, e-posta eylemleri doğru şekilde ayarlanmışsa Web kullanıcı Arabirimi aracılığıyla doğrulayabilirsiniz.

Başlamadan önce çözüm hızlandırıcınız e-posta bildirimleri göndermek için kullanmak üzere bir Outlook hesabı oluşturun.

Aşağıdaki adımlar oturum açmak ve e-posta eylemleri ayarlamak için yardımcı olur:

1. Outlook ile oturum açmak için tıklayın. Azure portalına yönlendirilirsiniz:

   ![Outlook için oturum açın](./media/iot-accelerators-remote-monitoring-email-actions/owneroutlook-email.png)

1. Tıklayın **yetkilendirmek**. Oturum açmanız istenir. Oturum açmak için kullandığınız hesap, e-posta bildirimleri göndermek için e-posta adresi uygulamanın kullandığı olmalıdır:

1. **Kaydet**’e tıklayın. Çözüm hızlandırıcınız dönün ve sayfayı yenileyin.

1. E-posta bildirimi başarıyla yapılandırdıysanız, bu iletiyi görürsünüz:

   ![Outlook oturum açma başarılı](./media/iot-accelerators-remote-monitoring-email-actions/success-email.png)

## HTML e-postayı özelleştirin <a name="htmledit"></a>

Çıkış,-hazır, Uzaktan izleme çözüm Hızlandırıcısını eylem e-postalar için bir temel HTML şablonu sağlar. E-posta şablonunun e-posta eylemi ayarları değerlerini kullanır. Örnek e-posta aşağıda verilmiştir:

![e-posta örneği](./media/iot-accelerators-remote-monitoring-email-actions/emailtemplate1.png)

Aşağıdaki adımlarda HTML e-posta şablonu Düzen gösterilmektedir. Örneğin, daha fazla bilgi dahil edebilir veya özel görüntüleri ekleyin:

1. Java veya .NET Uzaktan izleme GitHub deposunu kopyalayın:

1. E-posta şablonu konuma gidin:
  
    `Dotnet: device-telemetry\ActionsAgent\data\EmailTemplate.html`
  
    `Java device-telemetry/app/resources/data/EmailTemplate.html`

1. Ekleyebilir veya iletisini özelleştirmek için bu şablonda parametreleri kaldırın. Ayrıca, eklemek, kaldırmak veya çağrıları gerektiği gibi değiştirin:

    .NET kodu gibi:  `emailTemplate = emailTemplate.Replace("${subject}", emailAction.GetSubject());`

    Java kod gibi:  `this.emailTemplate.replace("${subject}", emailAction.GetSubject());`

1. Şablon parametreleri şeklinde ele `${...}`. Bir parametre silmek için gerekli satırı silin. Parametre eklemek için bir satır eklemek için değeriyle ekleyin.

1. Görüntüleri veya özel metin eklemek için doğrudan EmailTemplate.HTML dosyasını güncelleştirin.

## <a name="throttling"></a>Azaltma

Uzaktan izleme çözüm Hızlandırıcısını Outlook e-posta bildirimleri göndermek için kullanır. Outlook için gönderilen e-posta sayısını sınırlar [1 dakika başına 30 e-postaları](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#receiving-and-sending-limits). E-posta istemcileri e-posta alırken dakika başına alınan e-posta sayısı da kısıtlama. Belirli bir e-posta istemcinizde sınırlamaları ile denetleyin. Bir kural için e-posta bildirimi ayarladığınızda, kural, en az bir dakikalık bir dönemdeki ortalama değerleri hesaplama ve anlık değerler kullanma:

![Ortalama hesaplama](./media/iot-accelerators-remote-monitoring-email-actions/calculation-email.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, yeni veya mevcut bir kuralı bir uzaktan izleme çözümünde bir e-posta eylemi ekleme gösterdi. Kılavuz, ayrıca ve ileti biçimini tanımlayan HTML düzenleme gösterdi.

Önerilen sonraki adıma öğrenmektir [uyarılar ve cihaz sorunlarını çözmesine nasıl kullanılacağını](iot-accelerators-remote-monitoring-maintain.md).
