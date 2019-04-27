---
title: Market teklifleri - Azure Market oluştur | Microsoft Docs
description: Azure ve bulut iş ortağı portalını kullanarak AppSource Marketlerden Teklifler oluşturma
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pbutlerm
ms.openlocfilehash: af9b34d90098409135020fa8a45ecd0253f25b22
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60825712"
---
# <a name="create-azure-marketplace-and-appsource-offers"></a>Azure Market ve AppSource teklifler oluşturun

Yayımcılar oluşturmak (ve ardından yayımlamak) etkinleştirmek için bir bulut iş ortağı portalı temel amacı Microsoft Azure ve AppSource Marketlerden sunar.  Bu işlemi her zaman istenen Teklif türü seçme ile başlayan [yeni teklif menü](../portal-tour/cpp-new-offer-menu.md).  Yanıt, uygun **yeni teklif** sayfası, bu Teklif türü için görüntülenir.  Örneğin, aşağıdaki görüntüde varsayılan gösterilmektedir **yeni teklif** Azure uygulama türü için sayfa.

![Yeni Teklif varsayılan sayfası](./media/new-offer-page.png)

Bu sayfanın üst görüntülenen yatay menü bulunan iki sekme seçimleri vardır: 
- **Düzenleyici** sekmesi - giriş bilgilerinin ve yeni teklif örneğinin varlığını karşıya yükleme sağlar.  Bu sekme, varsayılan olarak görüntülenir.
- **Durum** sekmesi - Yayımlama durumunu sağlar ve doğrulama ve gözden geçirme sorunları listeler. 

Bir teklif oluşturduğunuzda kullandığınız **Düzenleyicisi** Bu teklif hakkında bilgi girmek için sekmesinde. 

## <a name="editing-operations"></a>Düzenleme işlemleri

Yatay araç veri giriş alanı bulunan aşağıdaki düğmelerden görüntüler:

|   Düğme    |   Amaç                                                          |
|   ------    |  --------                                                          |
| **Kaydet**    | Son veri girişi değişiklikleri kaydeder.  El ile bu sayfadan ayrılmak gidin veya değişiklikleriniz kaybolur önce değişiklikleri kaydetmeniz gerekir. | 
| **Atma** | (Son kaydetmeden itibaren) son veri girişi değişiklikleri iptal eder.             |
| **Compare** | Yayımlanan bir teklifi ile geçerli teklif durumu karşılaştırır.  Teklif başarıyla yayımlandıktan sonra yalnızca etkin.  |
| **Yayımlama** | Bu teklif için yayımlama işlemi başlar.                       |
| **Silme**  | Bu teklif, oluşturulduktan sonra ancak bunu yayımlamadan önce siler. |
|   |   |


## <a name="editing-tabs"></a>Sekmeleri düzenleme

Bir teklifi oluştururken, sol taraftaki dikey sütununda bulunan her sekmede gerekli ve isteğe bağlı veri sağlamak **yeni teklif** sayfası.  --Metin kutuları, açılan menüler ve onay kutularını--gibi standart kullanıcı arabirimi denetimleri için veri toplama görüntülenir.  Aşağıdaki tabloda, belirli düzenleme tabs koleksiyonunu teklif türüne bağlı olsa da, bazı ortak sekme listeler.

|      Sekme adı       |   Amaç                                                            |
|      --------       |   -------                                                            |
| **Teklif ayarları**  | Teklif ve yayımcı kimliği bilgi toplar.                    |
| **SKU'lar**            | Her teklife stok tutma birimini (STB) sürümü için teknik ve işletmeye özelliklerini tanımlar |
| **Test Sürüşü**      | İsteğe bağlı bu özelliği destekleyen türleri için bir gösterimi için teklifinizi tanımlar.  Daha fazla bilgi için [Test Sürüşü nedir?](../test-drive/what-is-test-drive.md)  |
| **Market** veya **mağaza** | Metin dizelerinin, belgeler ve teklif Market'te listelemek için kullanılan görüntüler toplar |
| **Destek**         | Müşteri, mühendislik ve çevrimiçi destek iletişim bilgileri toplar  |
|  |  |

Benzer şekilde adlandırılmış sekmelerle içeriği farklı bir teklif türleri arasında değişebilir.  Bu sekme özel Teklif Ayrıntıları "Teklif oluşturma" bölümündeki her bir teklif türü için sağlanır.


## <a name="next-steps"></a>Sonraki adımlar

Oluşturma ve bir teklif kaydedin ve önce veya yayımladıktan sonra sonra [durumunu görüntülemek](./cpp-view-status-offer.md).
