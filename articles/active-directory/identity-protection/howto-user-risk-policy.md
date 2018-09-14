---
title: Azure Active Directory kimlik koruması kullanıcı riski ilkesini yapılandırma | Microsoft Docs
description: Azure AD kimlik koruması kullanıcı riski ilkesini yapılandırmayı öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: bcfab9ab95e41b723cb8a8e49d7390a2894d5219
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581373"
---
# <a name="how-to-configure-the-user-risk-policy"></a>Nasıl yapılır: kullanıcı riski ilkesi yapılandırma

Tüm etkin [risk olayları](../reports-monitoring/concept-risk-events.md) , algılandı Azure Active Directory tarafından katkıda bulunan bir kullanıcı için mantıksal bir kavramken kullanıcı riski çağrılır. Riskli olduğu belirlenen bir kullanıcı gizliliği bozulmuş olabilecek bir kullanıcı hesabının göstergesidir.

![Riskli oldukları belirlenen kullanıcılar](./media/howto-user-risk-policy/1200.png)


## <a name="user-risk-level"></a>Kullanıcı risk düzeyi

Bir kullanıcı risk düzeyi (yüksek, Orta veya düşük) kullanıcının kimliğini tehlikede olasılığını göstergesidir. Bir kullanıcı kimliğiyle ilişkili kullanıcı risk olaylarına göre hesaplanır.

Risk olayı durumu bozulmuş **etkin** veya **kapalı**. Olayları yalnızca risk **etkin** kullanıcı risk düzeyi hesaplamaya katkıda bulunun.

Kullanıcı risk düzeyi, aşağıdaki girişleri kullanılarak hesaplanır:

* Kullanıcıyı etkileyen active risk olayları
* Bu olayların risk düzeyi
* Tüm düzeltme eylemlerini alınmış olup olmadığı

![Kullanıcı risk](./media/howto-user-risk-policy/1031.png "kullanıcı risk")

Riskli kullanıcıların oturum açarken engelleyen koşullu erişim ilkeleri oluşturmak için kullanıcı risk düzeyleri kullanın veya bunları parolalarını güvenli bir şekilde değiştirmek için zorla.

## <a name="closing-risk-events-manually"></a>Risk olaylarını elle kapatma

Çoğu durumda, düzeltme eylemleri risk olayları otomatik olarak kapatmak için bir güvenli parola sıfırlama gibi gerçekleştirir. Ancak, bu her zaman mümkün olmayabilir.  
Bu, örneğin, bir durumdur, ne zaman:

* Etkin risk olayları sahip bir kullanıcı silindi
* Bir araştırma bildirilen risk olayı sahip olan gerçekleştireceğini meşru bir kullanıcı tarafından ortaya çıkarır.

Risk olayları olduğundan **etkin** katkıda bulunmak için kullanıcı risk hesaplama, risk olaylarını elle kapatma tarafından el ile bir risk düzeyi daha düşük olabilir.  
Araştırma Kurs sırasında herhangi bir risk olayını durumunu değiştirmek için aşağıdaki eylemlerden birini tercih edebilirsiniz:

![Eylemler](./media/howto-user-risk-policy/34.png "eylemleri")

* **Çözmek** - bir risk olayını araştırdıktan sonra uygun düzeltme eylemi dışında kimlik koruması sürdü ve risk olayı kapatıldı, değerlendirilmesi gerektiğini düşünüyorsanız olayı Çözüldü olarak işaretleyin. Olayları risk olayın durumu kapalı olarak ayarlanır ve risk olayının artık kullanıcı riski katkıda bulunan çözüldü.
* **Hatalı pozitif olarak işaretleme** -bazı durumlarda, bir risk olayını araştırmak ve olabilirsiniz, yanlış riskli işaretlenmiş olduğunu keşfedin. Risk olayı hatalı pozitif sonuç olarak işaretleyerek böyle oluşum sayısını azaltmaya yardımcı olabilir. Bu, makine öğrenimi algoritmaları sınıflandırma benzer olayların gelecekte artırmak için yardımcı olur. Yanlış pozitif sonuç veren olayların durumunu sağlamaktır **kapalı** ve kullanıcı riski artık katkıda bulunur.
* **Yoksay** - herhangi bir düzeltme eylemi olmamıştır, ancak etkin listesinden kaldırılması için risk olayı istiyorsanız, bir risk olayını yoksay işaretleyebilirsiniz ve olay durumu kapatılır. Kullanıcı riski yok sayılan olayları katkıda bulunmuyor. Bu seçenek yalnızca olağan dışı durumlarda kullanılmalıdır.
* **Yeniden** -Risk el ile kapatılmış olayları (seçerek **çözmek**, **hatalı pozitif sonuç**, veya **Yoksay**) olay ayarlama etkinleştirilebilir Durum geri **etkin**. Yeniden etkinleştirilen risk olayları için kullanıcı risk düzeyi hesaplama katkıda bulunur. (Güvenli parola sıfırlama gibi) düzeltme aracılığıyla kapatılan risk olayları yeniden etkinleştirilemez.

**İlgili yapılandırma iletişim kutusunu açmak için**:

1. Üzerinde **Azure AD kimlik koruması** dikey altında **Araştır**, tıklayın **Risk olayları**.

    ![El ile parola sıfırlama](./media/howto-user-risk-policy/1002.png "el ile parola sıfırlama")
2. İçinde **Risk olayları** listesinde, bir risk tıklayın.

    ![El ile parola sıfırlama](./media/howto-user-risk-policy/1003.png "el ile parola sıfırlama")
3. Risk dikey penceresinde, bir kullanıcıya sağ tıklayın.

    ![El ile parola sıfırlama](./media/howto-user-risk-policy/1004.png "el ile parola sıfırlama")

## <a name="closing-all-risk-events-for-a-user-manually"></a>Bir kullanıcı için tüm risk olaylarını elle kapatma
El ile bir kullanıcının risk olayları ayrı ayrı kapatmak yerine, Azure Active Directory kimlik koruması da, tek bir tıklamayla bir kullanıcı için tüm olayları kapatmak için bir yöntem sağlar.

![Eylemler](./media/howto-user-risk-policy/2222.png "eylemleri")

Tıkladığınızda **tüm olayları kapatılamadı**, tüm olayları kapatılır ve etkilenen kullanıcı artık risk altındadır.

## <a name="remediating-user-risk-events"></a>Düzeltme kullanıcı risk olayları

Bir düzeltme bir kimlik veya daha önce olduğundan şüphelenilen veya tehlikeye bilinen bir cihazı güvenli hale getirmek için bir eylemdir. Bir düzeltme eylemi kimliği veya cihaz güvenli bir duruma geri yükler ve kimlik ya da cihaz ile ilişkili önceki risk olayları giderir.

Kullanıcı risk olaylarını düzeltmek için şunları yapabilirsiniz:

* Güvenli parola sıfırlama kullanıcı risk olaylarını elle düzeltmek için gerçekleştirin
* Kullanıcı risk olaylarını otomatik olarak düzeltmek veya gidermek için bir kullanıcı riski ilkesi yapılandırma
* Etkilenen cihaz yeniden görüntü  

### <a name="manual-secure-password-reset"></a>El ile güvenli parola sıfırlama
Güvenli parola sıfırlama birçok risk olayları için geçerli bir düzeltme ve ne zaman gerçekleştirileceğini otomatik olarak bu risk olaylarını kapatır ve kullanıcı risk düzeyi yeniden hesaplar. Kimlik koruması Panosu, parola sıfırlama için riskli kullanıcı başlatmak için kullanabilirsiniz.

İlgili iletişim kutusu, bir parola sıfırlama için iki farklı yöntem sunar:

**Parolayı Sıfırla** - seçin **kullanıcının parolasını sıfırlamasını gerektirmek** kullanıcı çok faktörlü kimlik doğrulaması için kayıtlı olmadığını Self kurtarmaya izin vermek için. Kullanıcının sonraki oturum açma sırasında kullanıcı çok faktörlü kimlik doğrulaması sınaması başarılı bir şekilde çözmek için gerekli ve daha sonra parolayı değiştirmek zorunda. Kullanıcı hesabı zaten kayıtlı çok faktörlü kimlik doğrulaması değilse, bu seçenek kullanılamaz.

**Geçici parola** - seçin **geçici bir parola oluştur** hemen mevcut parolayı geçersiz kılmak ve kullanıcı için yeni bir geçici parola oluşturun. Yeni bir geçici parola kullanıcı için bir alternatif e-posta adresi veya Kullanıcı Yöneticisi gönderin. Geçici bir parola olduğundan, kullanıcı oturum açma sırasında parola değiştirme istenir.

![İlke](./media/howto-user-risk-policy/1005.png "İlkesi")

**İlgili yapılandırma iletişim kutusunu açmak için**:

1. Üzerinde **Azure AD kimlik koruması** dikey penceresinde tıklayın **risk için işaretlenen kullanıcılar**.

    ![El ile parola sıfırlama](./media/howto-user-risk-policy/1006.png "el ile parola sıfırlama")
2. En az bir risk olayı bir kullanıcıyla kullanıcılar listesinden seçin.

    ![El ile parola sıfırlama](./media/howto-user-risk-policy/1007.png "el ile parola sıfırlama")
3. Kullanıcı dikey penceresinde tıklayın **parolayı Sıfırla**.

    ![El ile parola sıfırlama](./media/howto-user-risk-policy/1008.png "el ile parola sıfırlama")

## <a name="user-risk-security-policy"></a>Kullanıcı riski İlkesi
Bir kullanıcı riski İlkesi belirli bir kullanıcı risk düzeyi değerlendirir ve önceden tanımlanmış koşullar ve kurallara dayanan düzeltme ve risk azaltma eylemleri geçerli bir koşullu erişim ilkesi var.

![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/1009.png "kullanıcı riski İlkesi")

Azure AD kimlik koruması risk azaltma ve olanak tanıyarak, riskli olduğu belirlenen kullanıcıları düzeltme yönetmenize yardımcı olur:

* Kullanıcıları ve grupları ilkenin uygulanacağı ayarlayın:

    ![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/1010.png "kullanıcı riski İlkesi")
* İlke tetikleyen kullanıcı risk düzeyi eşiği (düşük, Orta veya yüksek) ayarlayın:

    ![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/1011.png "kullanıcı riski İlkesi")
* İlke tetiklendiğinde zorlanacak denetimleri ayarlayın:

    ![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/1012.png "kullanıcı riski İlkesi")
* İlke durumu anahtarı:

    ![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/403.png "MFA kaydı")
* Gözden geçirin ve bunu etkinleştirdikten önce bir değişikliğin etkisini değerlendirin:

    ![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/1013.png "kullanıcı riski İlkesi")

Seçim bir **yüksek** eşiği ilke tetiklenir ve kullanıcılara etkisini en aza indirir sayısını azaltır.
Ancak, dışlar **düşük** ve **orta** kimlikleri cihazları, güvenli veya değil ilkeyi, risk için işaretlenmiş kullanıcılar daha önce olduğundan şüphelenilen veya tehlikeye bilinen.

İlke ayarlanırken

* Çok sayıda yanlış pozitifleri (geliştiriciler, güvenlik analisti) üreteceği kullanıcılar hariç
* Kullanıcılar yerel ilkesini etkinleştirme olduğu pratik dışında (örneğin Yardım Masası için hiçbir erişim)
* Kullanım bir **yüksek** eşiği ilk ilke üretim, sırasında veya son kullanıcılar tarafından görülen sorunları en aza indirmeniz gerekir.
* Kullanım bir **düşük** kuruluşunuz daha yüksek güvenlik gerektiriyorsa eşiği. Seçerek bir **düşük** eşiği ek kullanıcı oturum açma sorunları, ancak daha fazla güvenlik sunar.

Önerilen çoğu kuruluş için bir kural yapılandırmak için varsayılandır bir **orta** kullanılabilirlik ve güvenlik arasında bir denge için eşiği.

İlgili kullanıcı deneyimini genel bakış için bkz:

* [Hesap kurtarma akışı tehlikeye](flows.md#compromised-account-recovery).  
* [Engellenen hesap akış tehlikeye](flows.md#compromised-account-blocked).  

**İlgili yapılandırma iletişim kutusunu açmak için**:

- Üzerinde **Azure AD kimlik koruması** dikey penceresindeki **yapılandırma** bölümünde **kullanıcı riski İlkesi**.

    ![Kullanıcı riski İlkesi](./media/howto-user-risk-policy/1009.png "kullanıcı riski İlkesi")

## <a name="mitigating-user-risk-events"></a>Kullanıcı risk olaylarını azaltma
Yöneticiler, risk düzeyine bağlı olarak oturum açma sonrası kullanıcıları engellemek için bir kullanıcı riski ilkesi ayarlayabilirsiniz.

Bir oturum açma engelleme:

* Etkilenen kullanıcı için yeni kullanıcı risk olayları oluşturulmasını engeller
* Yöneticilerin el ile kullanıcı kimliğini etkileyen risk olaylarını düzeltmek ve güvenli bir duruma geri sağlar


## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
