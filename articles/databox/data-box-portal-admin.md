---
title: Azure Data Box portalı yönetici kılavuzu | Microsoft Docs
description: Azure portalı kullanarak Azure Data Box'ınızı nasıl yönetebileceğiniz açıklanmaktadır.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 4a76d59349c37a3dcc120e64f692881b461f58fb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46993482"
---
# <a name="use-the-azure-portal-to-administer-your-data-box"></a>Azure portalı kullanarak Data Box’ınızı yönetme

Bu makalede Data Box ile gerçekleştirilebilen bazı karmaşık iş akışları ve yönetim görevleri anlatılmaktadır. Data Box’ı Azure portal veya yerel web kullanıcı arabirimiyle yönetebilirsiniz. 

Bu makale, Azure portalı kullanarak gerçekleştirebileceğiniz görevlere odaklanmaktadır. Azure portalı kullanarak siparişleri yönetebilir, Data Box’ı yönetebilir ve siparişin durumunu tamamlanana kadar takip edebilirsiniz.


## <a name="cancel-an-order"></a>Siparişi iptal etme

Siparişinizi verdikten sonra çeşitli nedenlerle iptal etmeniz gerekebilir. Siparişi yalnızca sipariş işleme alınmadan önce iptal edebilirsiniz. Sipariş işleme alınıp Data Box hazırlandığında siparişi iptal etmeniz mümkün değildir. 

Bir siparişi iptal etmek için aşağıdaki adımları gerçekleştirin.

1.  **Genel bakış > İptal**'e gidin. 

    ![Siparişi iptal etme 1](media/data-box-portal-admin/cancel-order1.png)

2.  Sipariş iptal etme nedenini belirtin.  

    ![Siparişi iptal etme 2](media/data-box-portal-admin/cancel-order2.png)

3.  Sipariş iptal edildikten sonra portaldaki durumu **İptal edildi** olarak görüntülenir. 

## <a name="clone-an-order"></a>Siparişi kopyalama

Kopyalama belirli durumlarda kullanışlıdır. Örneğin kullanıcı, veri aktarımı için daha önceden Data Box kullanmıştır. Yeni veriler üretildikçe Azure'a aktarmak için diğer bir Data Box’a daha ihtiyaç duyulur. Bu durumda aynı sipariş kopyalanabilir.

Siparişi kopyalamak için aşağıdaki adımları gerçekleştirin.

1.  **Genel bakış > Kopyala**'ya gidin. 

    ![Siparişi kopyalama 1](media/data-box-portal-admin/clone-order1.png)

2.  Siparişin tüm ayrıntıları aynı şekilde korunur. Siparişin adı, özgün siparişin adına *-Kopya* eklenerek oluşturulur. Gizlilik bilgilerini gözden geçirdiğinizi onaylamak için onay kutusunu seçin. **Oluştur**’a tıklayın.    

Kopya sipariş birkaç dakikada oluşturulur ve portal yeni siparişi gösterecek şekilde güncelleştirilir.


## <a name="delete-order"></a>Siparişi silme

Tamamlanan siparişleri silmek isteyebilirsiniz. Siparişte adınız, adresiniz ve iletişim bilgileriniz gibi kişisel bilgileriniz yer alır. Sipariş silindiğinde bu kişisel bilgiler de silinir.

Yalnızca tamamlanan veya iptal edilen siparişleri silebilirsiniz. Siparişi silmek için aşağıdaki adımları gerçekleştirin.

1. **Tüm kaynaklar**'a gidin. Siparişinizi arayın.

2. Silmek istediğiniz siparişe tıklayın ve **Genel bakış**'a gidin. Komut çubuğundan **Sil**'e tıklayın.

    ![Data Box siparişini silme 1](media/data-box-portal-admin/delete-order1.png)

3. Siparişi silme işlemini onaylamanız istendiğinde siparişin adını girin. **Sil**'e tıklayın.

## <a name="download-shipping-label"></a>Sevkiyat etiketini indirme

Data Box’unuzun E-ink ekranı çalışmıyorsa ve iade gönderimi etiketini göstermiyorsa gönderim etiketini indirmeniz gerekebilir. 

Sevkiyat etiketini indirmek için aşağıdaki adımları gerçekleştirin.
1.  **Genel bakış > Sevkiyat etiketi indir** bölümüne gidin. Bu seçenek yalnızca cihaz gönderildikten sonra kullanılabilir. 

    ![Sevkiyat etiketini indirme](media/data-box-portal-admin/download-shipping-label.png)

2.  Aşağıda gösterilen iade sevkiyat etiketi indirilir. Etiketi kaydedin ve yazdırın. Etiketi katlayıp cihazdaki şeffaf kılıfa koyun. Etiketin görünür olduğundan emin olun. Cihazdaki önceki gönderimden kalan tüm çıkartmaları sökün.

    ![Örnek sevkiyat etiketi](media/data-box-portal-admin/example-shipping-label.png)

## <a name="edit-shipping-address"></a>Teslimat adresini düzenleme

Siparişi verdikten sonra teslimat adresini düzenlemeniz gerekebilir. Bu işlem yalnızca cihaz yola çıkana kadar kullanılabilir. Cihaz yola çıktıktan sonra bu seçenek kullanılamaz.

Siparişi düzenlemek için aşağıdaki adımları gerçekleştirin.

1. **Sipariş ayrıntıları > Teslimat adresini düzenle**'ye gidin.

    ![Teslimat adresini düzenleme 1](media/data-box-portal-admin/edit-shipping-address1.png)

2. Gönderim adresini düzenleyin ve doğrulayın ve sonra değişiklikleri kaydedin.

    ![Teslimat adresini düzenleme 2](media/data-box-portal-admin/edit-shipping-address2.png)

## <a name="edit-notification-details"></a>Bildirim ayrıntılarını düzenleme

Sipariş durumu e-postalarının gönderilmesini istediğiniz kullanıcıları değiştirmek isteyebilirsiniz. Örneğin cihaz teslim edildiğinde veya alındığında bir kullanıcının bilgilendirilmesi gerekebilir. Veriler kaynaktan silinmeden önce Azure depolama hesabındaki verileri doğrulanması amacıyla veri kopyalama işlemi tamamlandığında başka bir kullanıcıya bildirim gönderilmesini isteyebilirsiniz. Bu gibi durumlarda bildirim ayrıntılarını düzenleyebilirsiniz.

Bildirim ayrıntılarını düzenlemek için aşağıdaki adımları gerçekleştirin.

1. **Sipariş ayrıntıları > Bildirim ayrıntılarını düzenle**'ye gidin.

    ![Bildirim ayrıntılarını düzenleme 1](media/data-box-portal-admin/edit-notification-details1.png)

2. Bu sayfada bildirim ayrıntılarını düzenleyebilir ve yaptığınız değişiklikleri kaydedebilirsiniz.
 
    ![Bildirim ayrıntılarını düzenleme 2](media/data-box-portal-admin/edit-notification-details2.png)


## <a name="view-order-status"></a>Sipariş durumunu görüntüleme

Cihaz durumu portalda değiştiğinde bu, size e-posta ile bildirilir.

|Sipariş durumu |Açıklama |
|---------|---------|
|Sipariş edildi     | Sipariş başarıyla oluşturuldu. <br>Cihaz kullanılabilir durumdaysa Microsoft tarafından gönderilecek cihaz belirlenir ve cihaz hazırlanır. <br> Cihaz o anda mevcut değilse sipariş cihaz mevcut olduğunda işleme alınır. Siparişin işleme alınması birkaç gün ile birkaç ay sürebilir. Sipariş 90 gün içinde gerçekleştirilemiyorsa sipariş iptal edilir ve bu size bildirilir.         |
|İşlendi     | Siparişin işlenmesi tamamlandı. Siparişiniz uyarınca cihaz gönderilmek üzere veri merkezinde hazırlanır.         |
|Yola çıktı     | Sipariş sevk edildi. Gönderiyi takip etmek için portalda, siparişinizde görüntülenen takip kimliğini kullanın.        |
|Teslim Edildi     | Gönderim, belirtilen adrese teslim edildi.        |
|Teslim alındı     |İade gönderiniz teslim alındı ve kurye tarafından tarandı.         |
|Alındı     | Cihazınız alındı ve Azure veri merkezinde tarandı. <br> Gönderi incelendikten sonra cihaz karşıya yüklemesi başlar.      |
|Veri kopyalama     | Veri kopyalama işlemi devam ediyor. Azure portal’da siparişinizin kopyalama ilerleme durumunu takip edin. <br> Veri kopyalama işlemi tamamlanana kadar bekleyin. |
|Tamamlandı       |Sipariş başarıyla tamamlandı.<br> Şirket içi verilerini sunuculardan silmeden önce verilerinizin Azure’a kopyalandığından emin olun.         |
|Hatalarla tamamlandı| Veri kopyalama tamamlandı ancak kopyalama sırasında hatalar oluştu. <br> Azure portalda belirtilen yolu kullanarak kopyalama günlüklerini gözden geçirin.   |
|İptal edildi            |Sipariş iptal edildi. <br> Siparişi iptal ettiniz veya bir hatayla karşılaşıldı ve sipariş, hizmet tarafından iptal edildi. Sipariş 90 gün içinde gerçekleştirilemiyorsa sipariş iptal edilir ve bu size bildirilir.     |
|Temizleme | Cihaz sürücülerindeki veriler silinir. Cihaz temizleme, sipariş günlüğü raporu Azure portalda mevcut hale geldiğinde tamamlanmış sayılır.|



## <a name="next-steps"></a>Sonraki adımlar

- [Data Box sorunlarını giderme](data-box-faq.md) hakkında bilgi edinin.
