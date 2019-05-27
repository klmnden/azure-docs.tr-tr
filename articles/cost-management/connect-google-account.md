---
title: Google Cloud Platform hesabı Azure Cloudyn bağlanın | Microsoft Docs
description: Cloudyn raporlarında maliyet ve kullanım verilerini görüntülemek için bir Google Cloud Platform hesap bağlanın.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: 247d959abadc92d70bdd60555a090986743e9322
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002066"
---
# <a name="connect-a-google-cloud-platform-account"></a>Google Cloud Platform hesaba Bağlan

Var olan Google Cloud Platform hesabınızı Cloudyn'e bağlanabilirsiniz. Hesabınızı Cloudyn'e bağladıktan sonra maliyet ve kullanım verileri, Cloudyn raporlarında kullanılabilir. Bu makalede, yapılandırmak ve Google hesabınızı Cloudyn'e bağlanmak için yardımcı olur.


## <a name="collect-project-information"></a>Proje bilgileri toplayın

Projeniz hakkında bilgi toplamak yoluyla başlattığınız.

1. Google Cloud Platform konsola oturum [ https://console.cloud.google.com ](https://console.cloud.google.com).
2. Cloudyn ve Not eklemek istediğiniz proje bilgileri gözden geçirin **proje adı** ve **proje kimliği**. Sonraki adımlar için kullanışlı bilgiler tutun.  
    ![Proje adı ve Google Cloud Platform konsolunda gösterilen proje kimliği](./media/connect-google-account/gcp-console01.png)
3. Faturalandırma etkin değil ve projenize bağlı, bir faturalama hesabı oluşturun. Daha fazla bilgi için [yeni bir faturalama hesabı oluşturma](https://cloud.google.com/billing/docs/how-to/manage-billing-account#create/_a/_new/_billing/_account).

## <a name="enable-storage-bucket-billing-export"></a>Depolama demetine fatura vermeyi etkinleştir

Cloudyn, bir depolama alanı Google fatura verilerinizi alır. Tutun **demet adı** ve **öneki bildirin** Cloudyn kayıt sırasında daha sonra kullanmak üzere kullanışlı bilgiler.

Kullanım raporları depolamak için Google bulut depolama kullanarak en az ücretler doğurur. Daha fazla bilgi için [bulut depolama fiyatlandırması](https://cloud.google.com/storage/pricing).

1. Fatura dışa aktarma dosyasına etkinleştirmediyseniz konumundaki yönergeleri [fatura dışa aktarma dosyasına etkinleştirme](https://cloud.google.com/billing/docs/how-to/export-data-file#how_to_enable_billing_export_to_a_file). JSON veya CSV fatura dışarı aktarma biçimini kullanabilirsiniz.
2. Google Cloud Platform konsolunda, aksi takdirde gidin **faturalama** > **faturalandırma dışarı aktarma**. Unutmayın, faturalandırma **demet adı** ve **öneki bildirin**.  
    ![Faturalandırma dışarı aktarma sayfasında gösterilen faturalama dışarı aktarma bilgileri](./media/connect-google-account/billing-export.png)

## <a name="enable-google-cloud-platform-apis"></a>Google Cloud Platform API'leri etkinleştirin

Kullanım ve varlık bilgilerini toplamak için aşağıdaki Google Cloud Platform etkin API'ler Cloudyn gerekir:

- BigQuery API
- Google Cloud SQL
- Google Cloud Datastore API
- Google bulut depolama
- Google bulut depolama JSON API
- Google Compute Engine API

### <a name="enable-or-verify-apis"></a>Etkinleştirmek veya API'leri doğrulayın

1. Google Cloud Platform konsolunda Cloudyn'e kaydetmek istediğiniz projeyi seçin.
2. Gidin **API'leri ve Hizmetleri** > **Kitaplığı**.
3. Daha önce listelenen her API bulmak için arama özelliğini kullanın.
4. Her bir API için doğrulayın **etkin API** gösterilir. ' A tıklayıp **etkinleştirme**.

## <a name="add-a-google-cloud-account-to-cloudyn"></a>Google Cloud hesabı Cloudyn'e Ekle

1. Cloudyn portalını Azure portalından açın veya gidin [ https://azure.cloudyn.com ](https://azure.cloudyn.com/) ve oturum açın.
2. Tıklayın **ayarları** (dişli simgesi) seçip **bulut hesapları**.
3. İçinde **hesap yönetimi**seçin **Google hesapları** sekmesine ve ardından **yeni Ekle +**.
4. İçinde **Google hesabı adı**, fatura hesap için e-posta adresi girin, ardından tıklayın **sonraki**.
5. Google kimlik doğrulaması iletişim kutusunu seçin veya Google hesabı girin ve ardından **izin** cloudyn.com hesabınıza erişim sağlayın.
6. İstek proje bilgileri eklemek önceki olduğunu not. İçerirler **proje kimliği**, **proje** adı **fatura** demet adı ve **fatura dosya** rapor ön eki'e tıklayın  **Kaydet**.  
    ![Cloudyn hesabınıza Google proje ekleme](./media/connect-google-account/add-project.png)

Google hesabınız hesapları listesinde görünür ve yazması gerekir **doğrulanan**. Bunun altında Google proje adı ve kimliği görünür ve yeşil onay işareti simgesi vardır. Hesap durumu söyleyin **tamamlandı**.

Birkaç saat içinde Google maliyet ve kullanım bilgilerini Cloudyn raporlarında gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

- Cloudyn hakkında daha fazla bilgi için devam [kullanımı ve maliyetleri gözden geçirme](./tutorial-review-usage.md) Cloudyn Öğreticisi.
