---
title: Windows 10 veya Windows 7 - Azure Windows sanal masaüstü Önizleme bağlanın
description: Windows 10 veya Windows 7 Windows sanal masaüstü önizlemesine bağlanmak nasıl.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/24/2019
ms.author: helohr
ms.openlocfilehash: b7d7b25d0355f2379b90313f17e2b595234df827
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65145983"
---
# <a name="connect-from-windows-10-or-windows-7"></a>Windows 10 veya Windows 7 bağlanın

> Şunlara uygulanır Windows 7 ve Windows 10.

İndirilebilir bir istemci kullanılabilir sağlayan erişim için Windows sanal masaüstü Önizleme kaynakları Windows 7 ve Windows 10 çalıştıran cihazlardan.

> [!IMPORTANT]
> Kullanmayın **RemoteApp ve Masaüstü bağlantıları (RADC)** veya **Uzak Masaüstü Bağlantısı (MSTSC)** Windows sanal masaüstü ya da istemci desteklemediğinden, Windows sanal masaüstü kaynaklara erişmek için.

## <a name="install-the-client"></a>İstemciyi yükleme

[İndirme](https://go.microsoft.com/fwlink/?linkid=2068602) ve istemciyi yerel bilgisayarınıza yükleyin. Yönetici hakları gerektirir.

## <a name="subscribe-to-a-feed"></a>Bir akışa abone ol

Yönetilen kaynaklar listesi için yöneticiniz tarafından sağlanan akışına abone olarak alın. Abone kaynakları yerel Bilgisayarınızda kullanılabilmesini sağlar.

Bir akışa abone olmak için:

1. Tüm uygulamalar listesini, istemciden arama için başlangıç **Uzak Masaüstü**.
1. Seçin **abone ol** hizmete bağlanmak ve kaynaklarınızı almak için ana sayfasında.
1. **Oturum** istendiğinde kullanıcı hesabınıza.

Başarıyla kimlik doğrulandıktan sonra artık kaynakların kullanabileceğiniz listesini görmeniz gerekir.

Kaynakları iki yöntemden birini kullanarak başlatabilirsiniz.

- İstemcinin ana sayfadan başlatmak için bir kaynak çift tıklayın.
- Başlat Menüsü'nden diğer uygulamalar normalde yaptığınız gibi bir kaynak başlatın.
  - Arama çubuğunda uygulamalar için arama yapabilirsiniz.

Bir akışa abone sonra akış içeriği düzenli aralıklarla otomatik olarak güncelleştirilir. Kaynakları eklenmesine, değiştirilmesine veya yöneticiniz tarafından yapılan değişiklikler temel alınarak kaldırıldı.

## <a name="view-the-details-of-a-feed"></a>Bir akışı ayrıntılarını görüntüleyin

Abone sonra Ayrıntılar bölmesini erişerek akış hakkında daha fazla bilgi görüntüleyebilirsiniz.

1. İstemcinin ana sayfasında üç noktayı seçin (**...** ) akış adının sağındaki.
1. Açılan menüden **ayrıntıları**.
1. Ayrıntılar bölmesini istemci sağ tarafında görünür.

Ayrıntılar bölmesini akış hakkında yararlı bilgiler içerir:

- Abone olmak için kullanılan kullanıcı adı ve URL
- Uygulamalar ve Masaüstü sayısı
- Son güncelleştirme tarih/saat
- Son güncelleştirme durumu

Gerekli olursa, el ile güncelleştirme seçerek başlayabilirsiniz **Şimdi Güncelleştir**.

## <a name="unsubscribe-from-a-feed"></a>Bir akıştan aboneliği iptal et

Bu bölümde bir akıştan aboneliği öğretir. Farklı bir hesapla yeniden abone ya da sistemden kaynaklarınızı kaldırmak için abonelikten çıkabilirsiniz.

1. İstemcinin ana sayfasında üç noktayı seçin (**...** ) akış adının sağındaki.
1. Açılan menüden **Unsubscribe**.
1. Gözden geçirin ve seçin **devam** iletişim.

## <a name="update-the-client"></a>İstemci güncelleştirmesi

İstemcinin yeni bir sürümü kullanılabilir olduğunda, istemci ve Windows İşlem Merkezi bildirilir. Güncelleştirme işlemini başlatmak için bildirimi seçin.
