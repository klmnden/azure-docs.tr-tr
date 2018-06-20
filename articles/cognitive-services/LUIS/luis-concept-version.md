---
title: Sürüm oluşturma HALUK - Azure anlama | Microsoft Docs
description: Sürümleri değişiklikleri dil anlama (HALUK) yönetmek için nasıl kullanılacağını öğrenin
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/13/2018
ms.author: v-geberr
ms.openlocfilehash: dabe7def2766770b686be3c43d4af4f331dd9577
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266082"
---
# <a name="versions"></a>Sürümler
Aynı uygulamayla başka modellerinin oluşturma [sürümleri](luis-how-to-manage-versions.md). 

## <a name="version-id"></a>Sürüm kimliği
Sürüm kimliği karakterler, rakamlar içeriyor veya '.' ve 10 karakterden uzun olamaz.

## <a name="initial-version"></a>İlk sürüm
İlk sürüm (0,1) varsayılan etkin sürümdür. 

## <a name="active-version"></a>Etkin sürüm
İçin [bir sürümü](luis-how-to-manage-versions.md#set-active-version) etkin anlamına gelir, şu anda düzenlenirken ve test gibi [HALUK] [ LUIS] Web sitesi. Bir sürüm kendi verilerine erişim, test ve yayımlamanız için güncelleştirmeleri de olun etkin olarak ayarlayın.

Şu anda etkin sürüm adını sonra uygulama adı üst, sol panelinde görüntülenir. 

[ ![Etkin sürümünü Değiştir](./media/luis-concept-version/version-in-nav-bar-inline.png) ](./media/luis-concept-version/version-in-nav-bar-expanded.png#lightbox)

## <a name="versions-and-publishing-slots"></a>Sürümler ve yayımlama yuvaları
Her iki aşama ve ürün yuvalarına yayımlayın. Her yuva farklı bir sürümü ya da aynı sürüme sahip olabilir. Bu model sürümleri aracılarını veya diğer HALUK uygulamalar için kullanılabilir uç nokta aracılığıyla arasındaki değişiklikleri doğrulamak için kullanışlıdır. 

## <a name="clone-a-version"></a>Bir sürüm kopyalama
Varolan bir sürümün bir kopyasını oluşturmak ve yeni bir sürüm olarak kaydetmek için bir sürüm kopyalanamıyor. Mevcut sürümü aynı içeriği yeni sürümü için bir başlangıç noktası olarak kullanmak için bir sürüm kopyalanamıyor. Bir sürüm kopyalama sonra yeni sürümü hale gelir **etkin** sürümü. 

## <a name="import-and-export-a-version"></a>İçeri ve dışarı aktarma bir sürüm
Uygulama düzeyinde bir sürümünü içe aktarabilirsiniz. Bu sürüm etkin sürüm olur ve uygulama dosyasını "VersionID" özelliğinde sürüm kimliği kullanılır. Varolan bir uygulamaya sürüm düzeyinde da içeri aktarabilirsiniz. Yeni sürüm etkin sürümü olur. 

Uygulama düzeyinde bir sürüm dışa aktarabilirsiniz veya sürüm düzeyinde bir sürüm dışarı aktarabilirsiniz. Tek fark, uygulama düzeyinde dışarı aktarılan sürüm sırada sürüm düzeyinde şu anda etkin sürümdür, üzerinde dışarı aktarmak için herhangi bir sürümü seçebilirsiniz olan **[ayarları](luis-how-to-manage-versions.md)** sayfası. 

Dışarı aktarılan dosyayı içeri aktarıldıktan sonra uygulama retrained olduğundan makine öğrenilen bilgileri içermiyor. Dışarı aktarılan dosyayı ortak çalışanlar içermiyor--sürüm yeni uygulamaya alındıktan sonra bu geri eklemeniz gerekir.

## <a name="export-each-version-as-app-backup"></a>Her sürümün uygulama yedek olarak dışarı aktarma
HALUK uygulamanızı yedeklemek için her sürüm dışa **[ayarları](luis-how-to-manage-versions.md)** sayfası.

## <a name="delete-a-version"></a>Bir sürümü Sil
Etkin sürüm hariç tüm sürümleri Ayarları sayfasında sürümleri listesinden silebilirsiniz. 

## <a name="version-availability-at-the-endpoint"></a>Uç nokta sürüm kullanılabilirliğine
Eğitilmiş sürümleri uygulamanızı otomatik olarak kullanılabilir olmayan [endpoint](luis-glossary.md#endpoint). Yapmanız gerekenler [yayımlama](PublishApp.md) veya bir sürüm için bu uygulama uç noktada kullanılabilir olmasını yayımladığınızda. İçin yayımlayabilirsiniz **hazırlama** ve **üretim**, en fazla iki sürümlerinde kullanılabilir uygulama uç noktada vermiş. Daha fazla bir uç noktada kullanılabilir uygulama sürümlerini ihtiyacınız varsa, sürüm dışarı aktarma ve yeni bir uygulama için yeniden içeri aktarın. Yeni uygulama farklı uygulama kimliği var.

## <a name="collaborators"></a>Ortak çalışanlar
Sahibi ve tüm [ortak](luis-how-to-collaborate.md) uygulamanın tüm sürümlerini tam erişimi vardır.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl ekleneceğini görmek [sürüm](luis-how-to-manage-versions.md) uygulama ayarları sayfasında. 

Tasarım öğrenin [hedefleri](luis-concept-intent.md) modelini.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions