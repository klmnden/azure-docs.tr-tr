---
title: "Logic Apps bağlayıcılar genel bakış | Microsoft Docs"
description: "Bir mantıksal uygulama kullanılabilir bağlayıcılar genel bakış"
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: ca8dab2e-9b69-4b1e-865d-1facd9f0cdac
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/15/2016
ms.author: jehollan
ms.openlocfilehash: 9cbb258ae9e32549669623e6824dd9b18fa1f68f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="using-connectors-in-a-logic-app"></a>Bir mantıksal uygulama bağlayıcıları kullanma
Bağlayıcılar, hizmetleri, protokolleri ve platformlar olayları, verilere ve eylemlere hızlı erişim sağlar.  Logic Apps destekleyen bağlayıcıların tam listesi için [bulunabilir burada](apis-list.md).  Bağlayıcılar, tetikleyici veya bir mantıksal uygulama içinde eylem olarak kullanılabilir ve yapılandırılan bir gerektirebilir *bağlantı* kullanmak için (örneğin: erişmek veya sizin adınıza göndermek için bir Twitter hesabı yetkilendirme).

## <a name="basics"></a>Temel Bilgiler
Bağlayıcılar olan Dynamics, Azure, Salesforce gibi diğer hizmetler ile tümleştirmek için bir mantıksal uygulama bir parçası olarak erişim barındırılan Hizmetler [ve daha fazla](apis-list.md).  Bunlar dağıtılmış ve ölçek, performans ve güvenlik hallolduğuna ile tümleştirme akışlarınızı oluşturabilmeniz Microsoft tarafından yönetilmelidir.  Bir bağlayıcı için bir mantıksal uygulama arama bağlayıcı eylem seçerek ekleyin veya altında tetikleyicisi **Göster Microsoft yönetilen API'ler**.

![Tetikleyici seçmek için eylem menüsü][1]

Her bağlayıcı eylem veya tetikleyici kendi yapılandırmak için özellikler kümesini sahip olur.  Eylem hakkında daha fazla bilgi için bilgi düğmeleri tıklayın veya belgelerinde başvuru [daha fazla bilgi için](apis-list.md).

Bir hizmet veya değil API henüz bir bağlayıcı ile tümleştirmek istiyorsanız, logic apps aracılığıyla da genişletebilirsiniz bir [özel bağlayıcı](../logic-apps/logic-apps-create-api-app.md) veya yalnızca HTTP gibi bir protokolü üzerinden doğrudan hizmete çağırın.

## <a name="triggers"></a>Tetikleyiciler
Bazı bağlayıcılar, bağlayıcı bir olay bir mantıksal uygulama yangın ve herhangi bir veri tetikleyici bir parçası olarak geçirin anlamına gelir bir tetikleyici vardır.  Bir tetikleyici her zaman bir mantıksal uygulama ilk adımı ' dir.  Popüler tetikleyiciler gibi işlemleri içerir:

* Yineleme - her saat çalıştırın
* Bir HTTP isteği alındığında
* Bir öğe için bir sıra eklendiğinde
* Bir e-posta alındığında

Bazı Tetikleyicileri aracılığıyla bir bildirim mantıksal uygulama için bir olay gerçekleştiğinde anlık ateşlenir ve diğerleri mantıksal uygulama hizmet (en fazla 15 dakikada) bir olay için ne sıklıkta denetleyecektir yapılandırılmış bir yineleme aralığı gerekir.  

Bir olayı alındığında çalıştırmak mantıksal uygulama ateşlenir ve iş akışı Eylemler başlar.  Tetikleyici (örneğin 'üzerinde yeni bir tweet' tetikleyici tweet Çalıştır olarak geçer) iş akışı boyunca herhangi bir veri erişebileceğiniz mümkün olacaktır.

## <a name="actions"></a>Eylemler
Çoğu bağlayıcılar iş akışının bir parçası yürütülen bir veya daha çok eylemler vardır.  Çalıştır tetikleyiciden harekete sonra gerçekleşecek herhangi bir adım eylemlerdir.  Bir eylem eklemek için **yeni adım** düğmesi ve kullanmak istediğiniz bağlayıcıyı arayın.  Bir kez seçili (ve tüm yapılandırdıktan sonra [bağlantıları](#connections) gerekli olabilecek) yapılandırabileceğiniz eylem kartı görürsünüz.  Herhangi bir çıkış belirteçleri tıklayarak veri önceki adımları seçin veya diğer bir yapılandırmada gerektiği şekilde girin.

![Bir bağlayıcı eylemi yapılandırma][2]

## <a name="connections"></a>Bağlantılar
Çoğu bağlayıcılar yapılandırmak gereken bir *bağlantı* Bağlayıcısı'nı kullanmadan önce.  A *bağlantı* bağlayıcı erişmeniz gerekiyorsa oturum açma veya bağlantı yapılandırması.  OAuth kullanın, bir bağlantı oluşturmak bağlayıcılarının hizmete burada erişim belirtecinizi kullanılabilir şifrelenir ve güvenli bir Azure gizli depolama alanına depolanır (örneğin, Office 365, Salesforce veya GitHub) imzalama anlamına gelir.  Diğer bağlayıcıları (örneğin, FTP ve SQL) sunucusu adresi, kullanıcı adı ve parola gibi yapılandırmasını içeren bir bağlantı gerektirir.  Bu bağlantı yapılandırma ayrıntılarını da şifrelenir ve güvenli bir şekilde depolanır.  Bağlantıları hizmet izin verdiği sürece hizmetine erişim kuramaz.  Azure Active Directory OAuth bağlantıları (örneğin, Office 365 ve Dynamics) için size erişim belirteci süresiz olarak yenilemek devam edebilirsiniz.  Diğer hizmetler, ne kadar süreyle biz bir belirteç yenilenen kullanabileceği üzerinde sınırları bırakabilir.  Genel bir parola değiştirme gibi bazı eylemler tüm erişim belirteci geçersiz kılar.  

Bağlantıları görüntülenebilir ve tıklayarak Azure'da yönetilen **Gözat** ve seçerek **API bağlantıları**.  API bağlantıları kaynaktan görüntüleme, düzenleme, güncelleştirme veya oluşturmuş olduğunuz tüm bağlantıları yeniden yetkilendirin.

## <a name="next-steps"></a>Sonraki Adımlar
* [İlk mantıksal uygulamanızı oluşturma](../logic-apps/logic-apps-create-a-logic-app.md)
* [Ortak kullanımlar ve logic apps örnekleri öğrenin](../logic-apps/logic-apps-examples-and-scenarios.md)
* [Enterprise Integration tetikleyiciler ve Eylemler kullanmaya başlama](../logic-apps/logic-apps-enterprise-integration-overview.md)

<!--Image References -->
[1]: ./media/connectors-overview/addAction.png
[2]: ./media/connectors-overview/configureAction.png
