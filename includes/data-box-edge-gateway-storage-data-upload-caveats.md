---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/21/2019
ms.author: alkohli
ms.openlocfilehash: 8c87e14071b3bb40421ab655c172df739570e295
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188826"
---
Azure'a taşınırken aşağıdaki uyarılar için veri uygulayın.

- Birden fazla cihaz olanla aynı kapsayıcıya yazamaz öneririz.
- Kopyalanan nesne olarak aynı ada sahip bir bulutta (örneğin, bir bloba veya bir dosya) var olan bir Azure nesne varsa, cihaz bulut dosyanın üzerine yazar.
- Paylaşım klasör altında oluşturulan bir boş dizin hiyerarşisi (olmadan tüm dosyaları) için blob kapsayıcılar karşıya yüklenmemiş.
- Sürükleme'ni kullanarak veri kopyalama ve dosya Gezgini ile veya komut satırı aracılığıyla bırakın. Kopyalanan dosyaların toplam boyutu 10 GB'den daha büyük ise, toplu kopyalama programı Robocopy veya rsync gibi kullanmanızı öneririz. Toplu kopyalama araçları aralıklı hatalar için kopyalama işlemi yeniden deneyin ve ek dayanıklılık sunar.
- Paylaşım için oluşturma sırasında tanımlanan BLOB türü ile eşleşmiyor BLOB'ları ile Azure depolama kapsayıcısı ilişkili paylaşımı yükler, blobları gibi güncelleştirilmez. Örneğin, cihazda bir blok blobu paylaşımı oluşturun. Paylaşım, sayfa BLOB'ları olan bir var olan bulut kapsayıcısı ile ilişkilendirin. Bu paylaşım dosyaları indirmek için yenileyin. Bazı bulut sayfa blobları olarak depolanır yenilenmiş dosyaları değiştirin. Göreceğiniz karşıya yükleme hataları.
