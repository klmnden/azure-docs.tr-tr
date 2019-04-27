---
title: Kullanıcı profili ve Azure not defterleri ile kullanmak için kimliği
description: Kullanıcı profili ve Azure not defterleri ile kullanıcı Kimliğini oluşturmak ve yönetmek nasıl.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 7d069d86-660f-4c94-b6e3-0c0f38c52d0e
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/25/2019
ms.author: kraigb
ms.openlocfilehash: b8c21b908ca9162a7e44c7af1e222babc6ee1eb7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631978"
---
# <a name="your-profile-and-user-id-for-azure-notebooks"></a>Azure not defterleri için profil ve kullanıcı kimliği

Azure not defterleri, güçlü, işbirliğine dayalı alanı içinde kullanıcı profilinizin genel görüntünüzü başkalarına sunar:

[![Bir Azure not defterleri profil sayfası](media/accounts/profile-page.png)](media/accounts/profile-page.png#lightbox)

Kullanıcı Kimliğinizi, projeleri ve Not Defterleri paylaşmak için kullandığınız URL'leri bir parçasıdır. Aşağıdaki listede, farklı bir URL desenleri açıklar:

- `https://notebooks.azure.com/<user_id>`: Profil sayfanızı.
- `https://notebooks.azure.com/<user_id>/projects`: Projelerinizi. Tüm projeler görürsünüz; diğer kullanıcılar yalnızca ortak projelerinizi bakın.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>`: Proje dosyaları.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/clones`: Belirli bir proje kopyalar.
- `https://notebooks.azure.com/<user_id>/projects/<project_id>/html/<notebook>.ipynb`: Belirli bir not defteri veya dosya HTML önizlemesi.

## <a name="your-user-id"></a>Kullanıcı Kimliğiniz

Azure not defterlerine ilk kez oturum açtığınızda, hesabınızı otomatik olarak "anon-idr3ca" gibi bir geçici kullanıcı kimliği atanır. İle başlayan bir kullanıcı kimliği olduğu sürece "anon-", Azure not defterleri sizden her oturum değiştirmek için:

![Azure not defterleri açarken bir kullanıcı kimliği oluşturmak için istemleri](media/accounts/create-user-id.png)

A **yapılandırma kullanıcı kimliği** komutu, geçici kullanıcı adının yanında da görünür:

![Geçici bir kimliği kullanılırken görüntülenen bir kullanıcı kimliği komutu yapılandırın](media/accounts/configure-user-id-command.png)

Ayrıca, kullanıcı Kimliğinizi herhangi bir zamanda profil sayfanızdan değiştirebilirsiniz.

Bir kullanıcı kimliği dört ve on altı harf, rakam ve kısa çizgi arasında oluşan gerekir. Başka bir karaktere izin verilir ve kullanıcı kimliği başlamak, kısa çizgi ile bitmelidir veya bir satır birden çok kısa çizgi kullanın. Tüm Azure not defterleri hesaplarda kullanıcı kimliklerinin benzersiz olduğu için iletiyi görebilirsiniz, "Kullanıcı kimliği zaten kullanılıyor." (Microsoft ticari marka bir kullanıcı kimliği kullanmayı denerseniz iletisi de görünür) Bu durumlarda, farklı bir kullanıcı kimliği seçin

> [!Important]
> Kimliğinizi değiştirerek önceki ID'nizi kullanarak paylaşılan herhangi bir URL geçersiz kılar Bağlantıları düzeltin için geri önceki Kimliğinize Kimliğinizi değiştirebilirsiniz. Ancak, başka bir kullanıcı için kullanılmayan bir talep olası sırada kimliği.

## <a name="your-profile"></a>Profilinizi

Profilinizi, URL'de herkes tarafından izlenen bilgi oluşan `https://notebooks.azure.com/<user_id>`. Profil sayfanızı Ayrıca, son kullanılan projeler ve tüm yıldızlı projelerin gösterir.

Profilinizi düzenlemek için **profil bilgilerini Düzenle** Profil sayfanızdaki komutu. Profilinizi bölümleri aşağıdaki gibidir:

| Section | İçindekiler |
| --- | --- |
| Profil fotoğrafı | Profil sayfanızdan gösterildiği bir görüntüsü. |
| Hesap Bilgisi | Görünen ad, kullanıcı kimliği ve ortak bir e-posta hesabı. E-posta hesabı buraya farklı olabilir ve diğer kullanıcıların sizinle iletişim kurmak için bir ortalama sağlar [hesabı](azure-notebooks-user-account.md) Azure not defterlerine kendisini imzalamak için kullanın. |
| Profil Bilgileri | Konumunuz, şirket, iş unvanı, web sitesi ve kendiniz kısa bir açıklaması. |
| Sosyal profilleri | GItHub, Twitter ve Facebook kimlikleri, bunları paylaşmak istiyorsanız. |
| Gizlilik Ayarları | İki komutları sağlar:<ul><li>**Profilim dışarı**: oluşturur ve indirir bir *.zip* Azure not defterleri kaydeder, fotoğraf, profil bilgileri ve güvenlik günlükleri gibi profilinizdeki tüm bilgileri içeren dosya.</li><li>**Hesabımı**: Azure not defterlerinde depolanan tüm kişisel bilgilerinizi kalıcı olarak siler.</li></ul> |
| Site özelliklerini etkinleştirme | Azure not defterleri davranışı yönleri denetlemenizi sağlar:<ul><li>**Birleşik not defterleri için ön uç**: daha hızlı not defteri başlatma ve daha iyi kalıcılığını etkinleştirir.</li><li>**JupyterLab içinde varsayılan olarak çalıştığı**: Varsayılan olarak, Azure not defterleri, çoğu kullanıcı için uygun olan basit bir kullanıcı arabirimi sağlar. JupyterLab deneyimli kullanıcılar için daha zengin ancak daha karmaşık bir arabirim sağlar.</li><li>**VNext Web sitesi**: Bu belgede gösterilen Modernleştirilmiş web düzeni sağlar.</li></ul> |

## <a name="next-steps"></a>Sonraki adımlar  

> [!div class="nextstepaction"]
> [Öğretici: bir çalışma doğrusal regresyon yapmak için Jupyter not defteri oluşturma](tutorial-create-run-jupyter-notebook.md)
