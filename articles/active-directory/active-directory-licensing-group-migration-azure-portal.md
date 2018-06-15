---
title: Azure Active Directory'de Grup tabanlı lisans için kullanıcı lisansları kullanıcıları geçirme | Microsoft Docs
description: Grup tabanlı Azure Active Directory'yi kullanarak lisans için tek tek kullanıcı lisanslarını geçiş yapma
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.component: users-groups-roles
ms.date: 01/14/2018
ms.author: curtand
ms.custom: seohack1
ms.openlocfilehash: ed95ba1329ba3052fab3d81327d23475e9902d8a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33762229"
---
# <a name="how-to-add-licensed-users-to-a-group-for-licensing-in-azure-active-directory"></a>Lisanslı kullanıcılar Azure Active Directory'de lisans için bir gruba ekleme

"Doğrudan atama"; aracılığıyla kuruluşlardaki kullanıcılara dağıtılan mevcut lisanslara sahip diğer bir deyişle, tek tek kullanıcı lisansı atamak için PowerShell komut dosyaları veya diğer araçları kullanarak. Kuruluşunuzdaki lisanslarını yönetmek için Grup tabanlı Lisans'ı kullanmaya başlamak istiyorsanız, var olan çözümler grup tabanlı lisanslama ile sorunsuz bir şekilde değiştirmek için geçiş planı gerekir.

Burada, Grup tabanlı lisansa geçirme geçici olarak, şu anda atanmış lisansları kaybetme kullanıcılar neden olacak bir durum kaçınmalısınız göz önünde bulundurmanız gereken en önemli şey olur. Lisansları kaldırılmasına neden herhangi bir işlem, kullanıcıların hizmetler ve verilerine erişimi kaybetme riskini kaldırmak için kaçınılmalıdır.

## <a name="recommended-migration-process"></a>Önerilen geçiş işlemi

1. Lisans atama ve kullanıcılar için temizleme yönetme var olan Otomasyon (örneğin, PowerShell) sahip. Çalışan olduğu gibi bırakın.

2. Yeni bir lisans grubu oluşturun (veya gruplar kullanmak için hangi varolan karar) ve gerekli tüm kullanıcıların üye olarak ekleneceği emin olun.

3. Gerekli lisansları bu gruba atanır; Amacınız, var olan Otomasyon (örneğin, PowerShell) bu kullanıcılara uygulama aynı lisans durumunu yansıtacak biçimde olmalıdır.

4. Bu gruplardaki tüm kullanıcılara lisanslar uygulanıp uygulanmadığını doğrulayın. Bu, her grup'in işleme durumunu denetleyerek ve denetim günlüklerini denetleyerek yapılabilir.

  - Nokta onay bireysel kullanıcılar kendi lisans ayrıntıları bakarak yapabilirsiniz. Bunlar aynı "doğrudan" atanan lisansları ve "devralınan" gruplarından olduğunu görürsünüz.

  - Bir PowerShell betiğini çalıştırabilirsiniz [lisanslarını kullanıcılara nasıl atanacağını doğrulayın](active-directory-licensing-group-advanced.md#use-powershell-to-see-who-has-inherited-and-direct-licenses).

  - Aynı ürün lisansı kullanıcıya hem doğrudan ve bir grubu aracılığıyla atandığında, kullanıcı tarafından yalnızca bir lisans kullanılır. Bu nedenle hiçbir ek lisanslar, geçiş işlemini gerçekleştirmek için gereklidir.

5. Hiçbir lisans anlaşmalarını hata durumuna kullanıcılar için her grubu denetleyerek başarısız olduğunu doğrulayın. Daha fazla bilgi için bkz: [belirleme ve bir grubu için lisans sorunlarını çözmek](active-directory-licensing-group-problem-resolution-azure-portal.md).

6. Özgün doğrudan atamalarını kaldırmayı düşünün; "bir kullanıcı alt kümesini sonucuna ilk izlemek için Dalgalar içinde", yavaş yavaş yapmak isteyebilirsiniz.

  Kullanıcıların özgün doğrudan atamalarını bırakabilir, ancak kullanıcılar ayrıldığında büyük olasılıkla özgün lisans hala korur, lisanslı gruplarına istediğiniz istemeyebilirsiniz.

## <a name="an-example"></a>Bir örnek

1.000 kullanıcının bulunduğu bir kuruluşta sahibiz. Tüm kullanıcılar, Enterprise Mobility + güvenlik (EMS) lisansı gerektirir. 200 kullanıcılar Finans departmanında ve Office 365 Kurumsal E3 lisanslar gerektirir. Ekleme ve lisans dönün ve Git gibi kullanıcılardan kaldırma şirket içinde çalışan bir PowerShell komut dosyası sunuyoruz. Lisanslar otomatik olarak Azure AD tarafından yönetilen şekilde grup tabanlı lisans komut dosyasını değiştirmek istiyoruz.

İşte geçiş işlemi aşağıdaki gibi görünebilir:

1. Azure portalını kullanarak EMS lisansı atayın **tüm kullanıcılar** Azure AD grubu. E3 lisansı atamak **Finans departmanı** gerekli tüm kullanıcıları içeren grup.

2. Her grup için tüm kullanıcılar için lisans atama tamamlandığını onaylayın. Select her grup için dikey penceresine gidin **lisansları**ve üstündeki işleme durumunu denetleyin **lisansları** dikey.

  - "En son lisans değişikliklerin uygulanıp uygulanmadığını tüm kullanıcılara" arayın işleme tamamlandı onaylamak için.

  - Üstteki kendisi için lisansları olmayan başarıyla atanmış olan kullanıcılar hakkında bir bildirim arayın. Bazı kullanıcılar için lisans dışında karşılaştınız mı? Bazı kullanıcılar, çakışan lisans grubu lisansları Devralmayı Engelle SKU'ları var mı?

3. Nokta uygulanan hem doğrudan hem de Grup lisanslar yüklü olduğunu doğrulamak için bazı kullanıcılar denetleyin. Bir kullanıcı seçin için dikey penceresine gidin **lisansları**ve lisansları durumunu inceleyin.

  - Geçiş sırasında beklenen kullanıcı durumunu şudur:

      ![Beklenen kullanıcı durumu](media/active-directory-licensing-group-migration-azure-portal/expected-user-state.png)

  Bu, kullanıcının hem doğrudan hem de devralınan lisansları sahip olduğunu doğrular. Biz, bkz: her ikisi de **EMS** ve **E3** atanır.

  - Her bir lisansın etkinleştirilmiş hizmetler hakkındaki ayrıntıları görüntülemek için seçin. Bu, doğrudan ve Grup lisansları tam olarak aynı hizmet planları kullanıcı için etkinleştirirseniz denetlemek için kullanılabilir.

      ![hizmet planları denetleyin](media/active-directory-licensing-group-migration-azure-portal/check-service-plans.png)

4. Hem doğrudan hem de Grup lisansları eşdeğer olduğunu doğruladıktan sonra kullanıcıları doğrudan lisanslarını kaldırma başlatabilirsiniz. Bu Portalı'nda bireysel kullanıcılar için kaldırarak test edin ve ardından bunları toplu olarak kaldırılmış şekilde Otomasyon betikleri çalıştırın. Aynı kullanıcı Portalı aracılığıyla kaldırılan doğrudan lisansların bir örneği burada verilmiştir. Lisans durumu değişmeden kalır, ancak biz artık doğrudan atamaları görmek dikkat edin.

  ![doğrudan lisansları kaldırıldı](media/active-directory-licensing-group-migration-azure-portal/direct-licenses-removed.png)


## <a name="next-steps"></a>Sonraki adımlar

Lisans Yönetimi grupları üzerinden diğer senaryolar hakkında daha fazla bilgi için okuma

* [Azure Active Directory'deki bir gruba lisans atama](active-directory-licensing-group-assignment-azure-portal.md)
* [Grup tabanlı Azure Active Directory lisanslaması nedir?](active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'deki bir gruba lisans sorunlarını tanımlama ve](active-directory-licensing-group-problem-resolution-azure-portal.md)
* [Azure Active Directory grup tabanlı ilave senaryolar lisanslama](active-directory-licensing-group-advanced.md)
