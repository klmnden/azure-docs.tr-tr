---
title: Azure maliyeti yönetimi için bir Google Cloud Platform'un hesabı bağlama | Microsoft Docs
description: Maliyet görüntülemek için bir Google Cloud Platform'un hesap bağlanın ve kullanım verilerini maliyet Yönetimi'nde repots.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/07/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: d4b906bd966da66659d23b935f7dbbd44b33899a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35296450"
---
# <a name="connect-a-google-cloud-platform-account"></a>Bir Google Cloud Platform'un hesabına bağlanma

Azure maliyeti Management'a varolan Google Cloud Platform'un hesabınızı bağlanabilir. Maliyet yönetim hesabınıza bağlandıktan sonra maliyet ve kullanım verileri Yönetimi maliyeti raporlarında kullanılabilir. Bu makalede, yapılandırmak ve Google hesabınız maliyet yönetimi ile bağlanmak için yardımcı olur.

## <a name="collect-project-information"></a>Proje bilgilerini toplama

Projenizi hakkında bilgi toplanıyor tarafından başlatın.

1. Google Cloud Platform'un konsoluna oturum açın [ https://console.cloud.google.com ](https://console.cloud.google.com).
2. Onboarding Yönetimi maliyeti ve not için istediğiniz proje bilgileri gözden **proje adı** ve **proje kimliği**. Sonraki adımlar için kullanışlı bilgilerin tutun.  
    ![Google Cloud Platform'un konsol](./media/connect-google-account/gcp-console01.png)
3. Faturalama etkin değil ve projenize bağlı varsa bir faturalama hesabı oluşturun. Daha fazla bilgi için bkz: [yeni bir faturalama hesabı oluşturma](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create\_a\_new\_billing\_account).

## <a name="enable-storage-bucket-billing-export"></a>Depolama demet fatura vermeyi etkinleştir

Maliyet yönetim depolama kova verilerden faturalama, Google alır. Tutmak **demet adı** ve **rapor önek** Yönetimi maliyeti kayıt sırasında daha sonra kullanmak üzere kullanışlı bilgileri.

Kullanım raporları depolamak için Google bulut depolama kullanarak en az ücretleri doğurur. Daha fazla bilgi için bkz: [bulut depolama fiyatlandırma](https://cloud.google.com/storage/pricing).

1. Bir dosya için fatura verme etkin değil, yönergeleri izleyin. [fatura dışa aktarma dosyasına etkinleştirme](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file). JSON veya CSV fatura dışarı aktarma biçimi kullanabilirsiniz.
2. Google Cloud Platform'un konsolunda, aksi takdirde gidin **faturalama** > **faturalama verme**. Fatura Not **demet adı** ve **rapor önek**.  
    ![Faturalama dışarı aktarma](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Google Cloud Platform API'leri etkinleştirin

Kullanım ve varlık bilgileri toplamak için aşağıdaki Google bulut platformu etkin API'ler maliyet yönetim gerekir:

- BigQuery API
- Google bulut SQL
- Google bulut veri deposu API
- Google bulut depolama
- Google bulut depolama JSON API
- API Google bilgi işlem altyapısı

### <a name="enable-or-verify-apis"></a>Etkinleştirmek veya API'ler doğrulayın

1. Google Cloud Platform'un konsolunda maliyet yönetimiyle kaydetmek istediğiniz projesini seçin.
2. Gidin **API'leri & Hizmetleri** > **Kitaplığı**.
3. Daha önce listelenen her API bulmak için aramayı kullanın.
4. Her API için doğrulayın **etkin API** gösterilir. Aksi takdirde tıklatın **etkinleştirmek**.

## <a name="add-a-google-cloud-account-to-cost-management"></a>Google Cloud hesap yönetimi maliyeti Ekle

1. Azure portalından Cloudyn portalını açın veya gitmek [ https://azure.cloudyn.com ](https://azure.cloudyn.com/) ve oturum açın.
2. Tıklatın **ayarları** (dişli simgesi) ve ardından **bulut hesapları**.
3. İçinde **hesap yönetimi**seçin **Google hesabı** sekmesini ve sonra **yeni Ekle +**.
4. İçinde **Google hesabı adı**, fatura hesap için e-posta adresi girin'ı tıklatın **sonraki**.
5. Google kimlik doğrulama iletişim seçin veya bir Google hesabı girin ve ardından **izin** cloudyn.com hesabınıza erişim için.
6. İstek proje bilgileri eklemek önceki vardı not ettiğiniz. İçerirler **proje kimliği**, **proje** adı **fatura** demet adı ve **fatura dosya** rapor öneki'a tıklayın  **Kaydet**.  
    ![Google proje ekleyin](./media/connect-google-account/add-project.png)

Google hesabınız hesaplar listesinde görüntülenir ve yazması gerekir **kimlik doğrulaması yapılmış**. Bunun altında Google proje adı ve kimliği görünür ve yeşil onay işareti simgesi sahiptir. Hesap durumu söyleyin **tamamlandı**.

Birkaç saat içinde Yönetimi maliyeti raporlar Google maliyet ve kullanım bilgilerini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- Azure maliyeti yönetimi hakkında daha fazla bilgi edinmek için devam [gözden kullanım ve maliyetleri](./tutorial-review-usage.md) maliyet Yönetimi Öğreticisi.
