---
title: Azure Otomasyon çalışma kitabı ölçüm uyarılarla izleme
description: Bu makalede, Azure Otomasyon çalışma kitabı dışına ölçümleri izleme yoluyla açıklanmaktadır.
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: a8a4b24e6b2503f64cc3fd7f4fd8c7400c547d4d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655364"
---
# <a name="monitoring-runbooks-with-metric-alerts"></a>Runbook'ları ölçüm uyarılarla izleme

Bu makalede, runbook'ları tamamlanma durumunu dayalı uyarılar oluşturma hakkında bilgi edinin.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure’da oturum açın

## <a name="create-alert"></a>Uyarı oluştur

Uyarılar, izlemek için bir koşul ve bu koşul karşılandığında gerçekleştirilecek bir eylem tanımlamanıza olanak sağlar.

Azure portalında gidin **tüm hizmetleri** seçip **İzleyici**. İzleyici sayfasında seçin **uyarıları** tıklatıp **+ yeni uyarı kuralı**.

### <a name="define-the-alert-condition"></a>Uyarı koşulu tanımla

1. **1. Uyarı koşulunu tanımlama** bölümünden **+  Hedef seçin**’i seçin. Aboneliğinizi seçin ve altında **kaynak türüne göre filtre**seçin **Automation hesapları**. Otomasyon hesabınızı seçin ve tıklatın **Bitti**.

   ![Uyarı için bir kaynak seçin](./media/automation-alert-activity-log/select-resource.png)

### <a name="configure-alert-criteria"></a>Uyarı ölçütleri yapılandırın

1. Tıklatın **+ ölçüt eklemek**. Seçin **ölçümleri** için **sinyal türü**ve seçin **toplam iş** tablosundan.

1. **Yapılandırma sinyal mantığı** sayfa burada uyarıyı tetikleyecek mantığı tanımlarsınız. İki boyutlarda sunulur geçmiş grafiği altında **Runbook adı** ve **durum**. Sonuçları filtrelemek için kullanılan bir ölçüm için farklı özellikleri boyutlardır. İçin **Runbook adı**, uyar veya uyarı tüm runbook'larda boş bırakın istediğiniz runbook'u seçin. İçin **durum**, bir durum için izlemek istediğiniz açılan seçin. Geçen hafta içinde çalışan sahip işleri açılır listede görünür runbook adı ve durum değerleri içindir.

   Bir durum veya açılır listede görünmeyen runbook uyarmasını istiyorsanız, **\+** boyut yanındaki. Bu boyut için son yayılan olmayan bir özel değer içinde girmenizi sağlayan bir iletişim kutusu açılır. Bir özellik için mevcut olmayan bir değer girerseniz, uyarıyı tetikleyen değil.

1. Altında **uyarı mantığı**, koşul ve, uyarı için eşiğin tanımlayın. Tanımlanan koşulunuz önizlemesini altında gösterilir.

1. Altında **göre Evaluated** sorgu için timespan seçin ve bu sorguyu hangi sıklıkta güncelleştirileceğini çalıştırılmıştır. Örneğin, seçtiğiniz **son 5 dakikadan** için **süresi** ve **her 1 dakika** için **sıklığı**, uyarıyı numarayı arar runbook, son 5 dakika boyunca, ölçütleri karşılar. Bu sorgu dakikada çalıştırılmış ve tanımladığınız Uyarı ölçütleri artık olduğunda 5 dakikalık penceresindeki yer olan uyarı kendisini çözümler. Tamamladığınızda **Bitti**’ye tıklayın.

   ![Uyarı için bir kaynak seçin](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Uyarı ayrıntılarını tanımlayın

1. **2. Uyarı ayrıntılarını tanımlayın**, uyarıya kolay bir ad verip bir açıklama ekleyin. Ayarlama **önem** uyarı koşulunuz eşleşecek şekilde. 0 ile 5 arasında değişen beş önem derecelerine vardır. Uyarıları davranılır aynı bağımsız önem derecesi iş mantığınızı eşleştirmek için önem derecesi eşleştirebilirsiniz.

1. Bölümünün altında tamamlanmasından sonra kuralını etkinleştirmek izin veren bir düğme vardır. Varsayılan kurallar oluşturma sırasında etkinleştirilir. Hayır'ı seçin, uyarı oluşturabilir ve içinde oluşturulan bir **devre dışı** durumu. Gelen **kuralları** sayfa Azure İzleyicisi'nde, onu seçin ve tıklatın **etkinleştirmek** hazır olduğunuzda uyarı etkinleştirmek için.

### <a name="define-the-action-to-take"></a>Gerçekleştirilecek eylemi tanımlayın

1. **3. Eylem grubunu tanımlama** bölümünden **+ Yeni eylem grubu**’na tıklayın. Eylem grubu, birden çok uyarıda kullanabileceğiniz eylemlerden oluşan bir gruptur. Eylem gruplarına e-posta bildirimleri, runbook'lar, web kancaları ve diğer birçok şey dahildir. Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../monitoring-and-diagnostics/monitoring-action-groups.md)

1. **Eylem grubu adı** kutusuna bir kolay ad bir de kısa ad yazın. Bu eylem grubu kullanılarak bildirim gönderildiğinde tam grup adı yerine kısa ad kullanılır.

1. İçinde **Eylemler** altında bölümünde **eylem türü**seçin **e-posta/SMS/itme/sesli**.

1. **E-posta/SMS/Anında İletme/Ses** sayfasında buna bir ad verin. **E-posta** onay kutusunu işaretleyin ve kullanılması için geçerli bir e-posta adresi girin.

   ![E-posta eylem grubunu yapılandırma](./media/automation-alert-activity-log/add-action-group.png)

1. **E-posta/SMS/Anında İletme/Ses** sayfasında **Tamam**’a tıklayarak sayfayı kapatın ve **Eylem grubu ekle** sayfasında **Tamam**’a tıklayarak bu sayfayı da kapatın. Bu sayfada belirtilen adı olarak kaydedilmiş **eylem adı**.

1. İşlem tamamlandığında **Kaydet**’e tıklayın. Bu, belirli bir durumla tamamlandı runbook olduğunda uyarı kuralı oluşturur.

> [!NOTE]
> Bir e-posta adresi bir eylem grubuna eklerken, bildirim e-posta adresi bir eylem grubuna eklenen belirten gönderilir.

## <a name="notification"></a>Bildirim

Uyarı ölçütler karşılandığında, Eylem grubunu tanımladığı eylem çalışır. Bu makalenin örnekte, bir e-posta gönderilir. Aşağıdaki resimde, uyarıyı tetikleyen sonra aldığınız e-posta örneğidir:

![E-posta uyarısı](./media/automation-alert-activity-log/alert-email.png)

Ölçüm artık tanımlanan eşik dışında olduğunda, uyarıyı devre dışı bırakılır ve tanımladığı Eylem Eylem grubunu çalıştırır. Bir e-posta eylemi türü seçtiyseniz, çözümlendiğini doğrulamaktadır belirten bir çözüm e-posta gönderilir.

## <a name="next-steps"></a>Sonraki adımlar

Otomasyon hesabınızda alertings tümleştirebilirsiniz diğer yolları hakkında bilgi edinmek için aşağıdaki makaleyi devam edin.

> [!div class="nextstepaction"]
> [Bir Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın](automation-create-alert-triggered-runbook.md)