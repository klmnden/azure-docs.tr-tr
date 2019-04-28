---
title: Azure NetApp dosyaları kaynak sağlayıcısı hatalarıyla ilgili sorunları giderme | Microsoft Docs
description: Nedenler, çözümler ve genel Azure NetApp dosyaları kaynak sağlayıcısı hatalar için geçici çözümler açıklanmıştır.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: b-juche
ms.openlocfilehash: d4e06429aa1efec7c3301c7d0f0e7e17800fd520
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769444"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>Azure NetApp Files Kaynak Sağlayıcısı hatalarını giderme
Bu makalede, ortak Azure NetApp dosyaları kaynak sağlayıcısı hataları, nedenleri, çözümler ve geçici çözümleri açıklar. 

<a name="error_01"></a>***Azure Key Vault yapılandırılmadı.***   
Azure Key Vault, temel alınan API erişimi için gerekli kimlik bilgilerini depolar. Bu hata, temel alınan API'ye erişmek için tam kimlik bilgilerini Azure Key Vault almadı gösterir.

* Nedeni  
Azure Key Vault doğru kimlik bilgilerini alamadı veya kimlik bilgileri eksik.  

* Çözüm   
Azure NetApp dosyaları hizmeti, Azure Key Vault'u kullanır. Azure Key Vault, Azure Active Directory'den bir belirteç kullanarak kimliğini doğrular. Bu nedenle, uygulama sahibi uygulama Azure Active Directory'ye kaydetmeniz gerekir.

* Geçici çözüm   
Yok.  Azure Key Vault Azure NetApp dosyaları kullanmak için doğru şekilde ayarlanmalıdır.  

<a name="error_02"></a>***Belirteç oluşturma değiştirilemez.***   
Birim oluşturulduktan sonra oluşturma belirteç değiştirmeye çalıştığınızda bu hata oluşur.
Birim oluşturulduğunda ve daha sonra değiştirilemez oluşturma belirteci ayarlamanız gerekir.

* Nedeni   
Birim, desteklenmeyen bir işlem değil oluşturulduktan sonra oluşturma belirteç değiştirme çalışıyorsunuz.

* Çözüm   
Birim oluşturulduktan sonra hata iletisini yoksayın istekten parametreyi kaldırmayı göz önünde bulundurun.

* Geçici çözüm   
Oluşturma belirteç değiştirmeniz gerekiyorsa, yeni bir oluşturma belirteci ile yeni bir birim oluşturmak ve sonra verileri yeni birime taşıyın.


<a name="error_03"></a>***Oluşturma belirteç en az 16 karakter uzunluğunda olmalıdır.***   
Oluşturma belirteci uzunluğu gereksinimini karşılamadığında, bu hata oluşur. Oluşturma belirteci uzunluğu en az 16 karakter olmalıdır.

* Nedeni   
Oluşturma belirteç uzunluk sınırı koşuluna uymuyor.  API'yi kullanarak bir birim oluşturduğunuzda, belirteç oluşturma gereklidir. Portalı kullanıyorsanız, belirteç otomatik olarak oluşturulabilir.

* Çözüm   
Oluşturma belirteç uzunluğunu artırın. Örneğin, başka bir sözcüğe başında ya da oluşturma belirteç sonuna ekleyebilirsiniz.

* Geçici çözüm   
Gereken en düşük uzunluk oluşturma belirtecin geçilemez.  Belirteç oluşturma uzunluğunu artırmak için bir önek veya sonek kullanabilirsiniz.


<a name="error_04"></a>***Azure NetApp dosyaları bulunamadı bir birim silinirken bir hata oluştu.***   
İç kayıt defteri kaynakların eşitlenmemiş olduğundan, bu hata oluştu.

* Nedeni   
Birim, silindikten sonra biraz zaman için portalda görüntülenen kalabilir. API'yi kullanarak birimi silerseniz, birime doğru şekilde belirtilmedi mümkündür. Hata süresi geçen tarayıcı önbelleğini tarafından da neden olabilir.

* Çözüm   
Açık tarayıcı önbelleğini portalı kullanıyorsanız. 10 dakikada bir yenilenir bir iç önbellek yoktur.  Yeniden önbelleğini temizlemek deneyebilirsiniz.  10 dakika sonra sorun devam ederse, destek bileti oluşturabilirsiniz.

* Geçici çözüm   
Farklı bir birime sırada kullanın ve var olan bir yok sayın.


<a name="error_05"></a>***Azure NetApp dosya bulundu, yeni bir birim eklenirken hata oluştu.***   
İç kayıt defteri kaynakların eşitlenmemiş olduğundan, bu hata oluşur.

* Nedeni   
Birim, silindikten sonra biraz zaman için portalda görüntülenen kalabilir. API'yi kullanarak birimi silerseniz, birime doğru şekilde belirtilmedi mümkündür.

* Çözüm   
Portalı kullanıyorsanız, birimin zaten oluşturuldu.  Birim otomatik olarak görünmelidir. Sorun devam ederse bir destek bileti oluşturabilirsiniz.

* Geçici çözüm   
Farklı bir ad ve farklı oluşturma belirteci ile bir birim oluşturabilirsiniz.


<a name="error_06"></a>***Dosya yolu adı harf, rakam ve kısa çizgi içerebilir (""-"") yalnızca.***   
Dosya yolu desteklenmeyen karakterler içeriyor, bu hata oluşur. Örneğin, bir süre ("."), virgül (","), alt çizgi ("\_"), veya dolar işareti ("$").

* Nedeni   
Dosya yolu desteklenmeyen karakterler içeriyor. Örneğin, bir nokta ("."), virgül (","), alt çizgi ("\_"), veya dolar işareti ("$").

* Çözüm   
Alfabetik harf, rakam veya kısa çizgi olmayan karakterleri kaldırın ("-"), girdiğiniz dosya yolundan.

* Geçici çözüm   
(Örneğin, "yerine yeni birim" "NewVolume" kullanarak) yeni bir kelimelerin belirtmek için alanları yerine büyük/küçük harf kullanın ya da alt çizgi, kısa çizgi ile değiştirin.


<a name="error_07"></a>***Hacim kimliği değiştirilemez.***   
Toplu kimliğe değiştirmeye çalıştığınızda bu hata oluşur.  Birim kimliği değiştiriliyor desteklenmeyen bir işlem değil.

* Nedeni   
Birim oluşturulduğunda dosya sistemi kimliği ayarlanır. Birim kimliği daha sonra değiştirilemez.

* Çözüm   
Yok.

* Geçici çözüm   
Yok.  Birim oluşturulur ve sonradan değiştirilemez birim kimliği üretilir.


<a name="error_08"></a>***Geçersiz bir değer '{0}' için alınan {1}.***   
Bu ileti, RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 ve Nfsv4 için alanları bir hata gösterir.

* Nedeni   
Giriş doğrulaması isteği aşağıdaki alanları en az biri için başarısız oldu: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 ve Nfsv4.

* Çözüm   
Komut satırına tüm gerekli ve çakışmayan parametreleri ayarladığınızdan emin olun. Örneğin, hem UnixReadOnly hem de UnixReadWrite parametreleri aynı anda ayarlanamaz.

* Geçici çözüm   
Çözüm bölümüne bakın.  


<a name="error_09"></a>***İçin eksik değer '{0}'.***   
Bu hata, gerekli bir özniteliği aşağıdaki parametreleri en az biri için istek eksik olduğunu gösterir: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 ve Nfsv4.

* Nedeni   
Giriş doğrulaması isteği aşağıdaki alanları en az biri için başarısız oldu: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 ve Nfsv4.

* Çözüm   
Komut satırına tüm gerekli ve çakışmayan parametreleri ayarladığınızdan emin olun. Örneğin, aynı anda hem UnixReadOnly hem de UnixReadWrite parametreleri ayarlanamıyor.

* Geçici çözüm   
Çözüm bölümüne bakın.  


<a name="error_10"></a> ***{0} zaten kullanımda.***   
Kaynak adı zaten kullanılmış bu hatayı gösterir.

* Nedeni   
Var olan bir birim olarak aynı olan bir ada sahip bir birim oluşturmak çalışıyorsunuz.

* Çözüm   
Bir birim oluştururken benzersiz bir ad kullanın.

* Geçici çözüm   
Yeni birim hedeflenen adı kullanabilmesi için gerekirse, mevcut birimin adını değiştirebilirsiniz.


<a name="error_11"></a> ***{0} çok kısa.***   
Bu hata, birim adı en az uzunluk gereksinimini karşılamıyor gösterir.

* Nedeni   
Birim adı çok kısadır.

* Çözüm   
Birim adı uzunluğunu artırın.  

* Geçici çözüm   
Birim adı için ortak bir önek veya sonek ekleyebilirsiniz.


<a name="error_12"></a>***Azure NetApp dosya API'sini erişilemiyor.***   
Azure API birimleri yönetmek için Azure NetApp dosya API'sini üzerinde kullanır.  Bu hata, API bağlantısı ile ilgili bir sorun olduğunu gösterir.

* Nedeni   
Temel alınan API, bir iç hata imzalanmayarak yanıt vermiyor. Bu hata geçici olabilir.

* Çözüm   
Bu sorunu geçici olarak olasıdır.  İstek, bir süre sonra başarılı olması gerekir.

* Geçici çözüm   
Yok. Temel alınan API birimleri yönetmek için gereklidir.  


<a name="error_13"></a>***Kimlik bilgileri, abonelik için bulunamadı '{0}'.***   
Bu hata, sağlanan kimlik bilgileri geçersiz veya abonelikte doğru ayarlanmamış gösterir.

* Nedeni   
Geçersiz veya yanlış ayarlanmış bir kimlik bilgileri, birimleri yönetmek için hizmete erişimi engelleyin.

* Çözüm   
Kimlik bilgilerini ayarlamak ve komut satırında doğru girildiğinden emin olun.

* Geçici çözüm   
Yok.  Doğru kimlik bilgilerini ayarlama, Azure NetApp dosyaları kullanmak için gereklidir.  


<a name="error_14"></a>***Hiçbir işlem sonucu kimliği için bulunamadı '{0}'.***   
Bu hata, bir iç hata işleminin tamamlanmasını engelliyor gösterir.

* Nedeni   
Bir iç hata oluştu ve işlemin tamamlanmasını engelledi.

* Çözüm   
Bu hata geçici olabilir.  Birkaç dakika bekleyin ve yeniden deneyin. Sorun devam ederse teknik destek sorunu araştırmak için bir bilet oluşturun.

* Geçici çözüm   
Birkaç dakika bekleyin ve sorunun devam edip etmediğini denetleyin.


<a name="error_15"></a>***İşlem '{0}' desteklenmiyor.***   
Bu hata, komut etkin bir abonelik veya kaynak için kullanılabilir olmadığını gösterir.

* Nedeni   
Abonelik veya kaynak için işlemi kullanılamaz.

* Çözüm   
Kullanmakta olduğunuz kaynak ve abonelik için kullanılabilir ve komut doğru girildiğinden emin olun.

* Geçici çözüm   
Çözüm bölümüne bakın.  


<a name="error_16"></a>***Düzeltme işlemi, bu kaynak türü için desteklenmiyor.***   
Bağlama hedefi veya anlık görüntü değiştirmeye çalıştığınızda bu hata oluşur.

* Nedeni   
Bağlama hedefi tanımlı oluşturulur ve daha sonra değiştirilemez.

* Çözüm   
Yok.  Bağlama hedefi birimin oluşturulduktan sonra değiştirilemez.

* Geçici çözüm   
Yok.


<a name="error_17"></a>***Salt okunur bir özellik için bir değer alındı '{0}'.***   
Değiştirilemez bir özellik için bir değer tanımlarken bu hata oluşur. Örneğin, birim kimliğini değiştiremezsiniz.

* Nedeni   
Değiştirilemez bir parametre (örneğin, birim kimliği) değiştirileceğini çalıştı.

* Çözüm   
Yok. Parametresi için birim kimliği değiştirilemez.

* Geçici çözüm   
Birim kimliği değişiklik yapılması gerekmez.  Bu nedenle, geçici bir çözüm gerekli değildir.

<a name="error_18"></a>***İstenen {0} bulunamadı.***   
Bu hata, örneğin, bir birim veya anlık görüntü var olmayan bir kaynağa başvuran çalıştığınızda oluşur. Kaynak silinmiş olabilir veya olduğunu kaynak adına sahip.

* Nedeni   
Yanlış yazılmış kaynak adı zaten silinmiş veya var olmayan (örneğin, bir birim veya anlık görüntü) kaynağa başvurma çalışıyorsunuz.

* Çözüm   
İstek için doğru başvurulduğundan emin olmak yazım hatalarını denetleyin.

* Geçici çözüm   
Çözüm bölümüne bakın.

<a name="error_19"></a>***Abonelik için kimlik bilgileri alınamıyor '{0}'.***   
Bu hata, sağlanan kimlik bilgileri geçersiz veya abonelik içinde yanlış ayarlanmış gösterir.

* Nedeni   
Geçersiz kimlik bilgileri veya hatalı abonelik kümesine erişimi engelle birimleri yönetmek için hizmet.

* Çözüm   
Kimlik bilgilerini ayarlamak ve komut satırında doğru girildiğinden emin olun.

* Geçici çözüm   
Yok.  Doğru kimlik bilgilerini ayarlama, Azure NetApp dosyaları kullanmak için gereklidir.

<a name="error_20"></a>***Bilinmeyen Azure NetApp dosya hatası.***   
Azure API birimleri yönetmek için Azure NetApp dosya API'sini üzerinde kullanır. Hata iletişim API'sine bir sorun olduğunu gösterir.

* Nedeni   
Temel alınan API bilinmeyen bir hata gönderir.  Bu hata geçici olabilir.

* Çözüm   
Büyük olasılıkla geçici bir sorundur ve bir süre sonra istek başarılı olması gerekir. Sorun devam ederse, sorunu araştırılması için bir destek bileti oluşturun.

* Geçici çözüm   
Yok.  Temel alınan API birimleri yönetmek için gereklidir.

<a name="error_21"></a>***Bilinmeyen bir özellik için alınan değeri '{0}'.***   
Var olmayan özellikler birim, anlık görüntü veya bağlama hedefi gibi bir kaynak için sağlandığında bu hata oluşur.

* Nedeni   
İstek, her kaynakla kullanılabilir özellikler kümesi vardır.  İstekte var olmayan herhangi bir özellik içeremez.

* Çözüm   
Tüm özellik adlarının doğru yazıldığından ve Özellikler abonelik ve kaynak için kullanılabilir olduğundan emin olun.

* Geçici çözüm   
Hataya neden olan özellik ortadan kaldırmak için istekte tanımlanan özellikler sayısını azaltın.


<a name="error_22"></a>***Güncelleştirme işlemi, bu kaynak türü için desteklenmiyor.***   
Yalnızca birimler güncelleştirilebilir. Bir desteklenmeyen güncelleştirme işlemi gerçekleştirmeye çalıştığınızda bu hata, bir anlık görüntü güncelleştirme oluşur.

* Nedeni   
Güncelleştirmeye çalıştığınız kaynak güncelleştirme işlemi desteklemiyor.  Yalnızca birim özelliklerini değiştiren olabilir.

* Çözüm   
Yok.  Güncelleştirmeye çalıştığınız kaynak güncelleştirme işlemi desteklemiyor. Bu nedenle değiştirilemez.

* Geçici çözüm   
Bir birim için yerinde güncelleştirme ile yeni bir kaynak oluşturmak ve verileri geçirin.


<a name="error_23"></a>***Öğe sayısı: {0} nesnesi için: {1} min-maks aralık dışında.***   
Verme İlkesi kuralları, minimum veya maksimum aralığını gereksinimini karşılamayan bu hata oluşur.  Verme İlkesi tanımlarsanız, bir dışarı aktarma İlkesi kuralı en az ve en fazla beş verme ilkesi kuralları olması gerekir.

* Nedeni   
Tanımladığınız verme ilkesi gerekli aralığı karşılamıyor.  

* Çözüm   
Dizin zaten kullanılmadığını emin olun ve 1 ile 5 aralığında olmasıdır.

* Geçici çözüm   
Verme İlkesi birimlere kullanmak için zorunlu değildir. Tamamen verme ilkesi kuralları olması gerekmiyorsa, bu nedenle, dışarı aktarma İlkesi atlayabilirsiniz.


<a name="error_24"></a>***Yinelenen değer hatası nesne için {0}.***   
Verme İlkesi ile benzersiz bir dizin tanımlanmamış olduğunda bu hata oluşur.  Verme ilkeleri tanımlar, tüm verme ilkesi kuralları benzersiz bir dizin 1 ile 5 arasında olması gerekir.

* Nedeni   
Tanımlanmış bir dışarı aktarma ilkesi için verme ilkesi kuralları gereksinimini karşılamıyor. Bir dışarı aktarma İlkesi kuralı en az ve en fazla beş verme ilkesi kuralları olması gerekir.  

* Çözüm   
Dizin zaten kullanılmadığını ve 5-1 aralığında olduğundan emin olun.

* Geçici çözüm   
Ayarlamaya çalıştığınız kural için farklı bir dizin kullanın.


