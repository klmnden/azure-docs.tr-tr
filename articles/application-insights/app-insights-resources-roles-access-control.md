---
title: "Kaynaklar, rolleri ve erişim denetim Azure Application Insights'ta | Microsoft Docs"
description: "Sahipleri, Katkıda Bulunanlar ve okuyucular, kuruluşunuzun Öngörüler."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: mbullwin
ms.openlocfilehash: 6e811c9b427469fa781cf1f5b7c7deff3a8e6eb3
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Kaynaklar, rolleri ve erişim denetimi Application Insights
Kim okuma ve güncelleştirme erişimine Azure verilerinize denetleyebilirsiniz [Application Insights][start], kullanarak [Microsoft Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md).

> [!IMPORTANT]
> Kullanıcılara erişim atamak **kaynak grubuna veya aboneliğe** uygulama kaynağınızı - kaynak kendisini değil ait olduğu. Ata **Application Insights bileşen katkıda bulunan** rol. Bu web testleri ve uygulama kaynağınızı birlikte uyarıları erişim Tekdüzen denetim sağlar. [Daha fazla bilgi edinin](#access).
> 
> 

## <a name="resources-groups-and-subscriptions"></a>Kaynaklar, gruplar ve abonelikleri
İlk olarak, bazı tanımları:

* **Kaynak** - Microsoft Azure hizmet örneğini bir. Application Insights kaynağınıza toplar, çözümler ve uygulamanızdan gönderilen telemetri verilerini görüntüler.  Web uygulamaları, veritabanları ve VM'ler Azure kaynaklarını diğer türleri içerir.
  
    Kaynaklarınızın görmek için açın [Azure Portal][portal], oturum açın ve tüm kaynaklar'ı tıklatın. Bir kaynağı bulmak için filtre alanında adının bir kısmını yazın.
  
    ![Azure kaynak listesi](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Kaynak grubu** ] [ group] -her kaynak grubuna ait. Bir grup, erişim denetimi için özellikle ilgili kaynakları yönetmek için uygun bir yoludur. Örneğin, bir kaynak grubuna bir Web uygulaması, uygulamayı izlemek için Application Insights kaynağı ve depolama kaynağı dışarı aktarılan verileri tutmak için koyabilirsiniz.

    ![Gözat, kaynak grupları belirtin, sonra bir grup seçin](./media/app-insights-resources-roles-access-control/11-group.png)

* [**Abonelik** ](https://portal.azure.com) - bir Azure aboneliğine oturum Application Insights veya diğer Azure kaynaklarını kullanmak için. Her kaynak grubu, fiyat paketi seçin ve burada, bir kuruluşun abonelik ise üyeleri ve bunların erişim izinlerini seçin Azure aboneliğiniz için aittir.
* [**Microsoft hesabı** ] [ account] -kullanıcı adı ve Microsoft Azure Abonelikleri, XBox Live, Outlook.com ve diğer Microsoft Hizmetleri için oturum açmak için kullandığınız parola.

## <a name="access"></a>Kaynak grubundaki erişimi denetleme
Uygulamanız için oluşturduğunuz kaynak yanı sıra olduğunu da uyarıları ve web testleri için ayrı gizli kaynaklar anlamak önemlidir. Aynı eklenen [kaynak grubu](#resource-group) uygulamanız. Ayrıca diğer Azure hizmetleriyle gibi Web sitesi ya da depolama burada konumuna.

![Application Insights kaynakları](./media/app-insights-resources-roles-access-control/00-resources.png)

Bu kaynaklara erişimi denetlemek için bu nedenle önerilir:

* Erişim denetim **kaynak grubuna veya aboneliğe** düzeyi.
* Ata **uygulama Öngörüler bileşeni katkıda bulunan** kullanıcılara rol. Bu gruptaki diğer hizmetlere erişimi sağlamadan web testleri, uyarılar ve Application Insights kaynakları düzenlemek sağlar.

## <a name="to-provide-access-to-another-user"></a>Başka bir kullanıcıya erişim sağlamak için
Abonelik veya kaynak grubu için sahiplik hakları olması gerekir.

Kullanıcının olmalıdır bir [Microsoft Account][account], ya da erişim kendi [kuruluş Microsoft Account](../active-directory/sign-up-organization.md). Bireylere ve Azure Active Directory'de tanımlanan kullanıcı gruplarına erişim sağlayabilir.

#### <a name="navigate-to-the-resource-group"></a>Kaynak grubuna gidin
Kullanıcı var. ekleyin.

![Uygulamanızın kaynak dikey penceresinde, Essentials'ı açın, kaynak grubu açın ve var. ayarları/kullanıcılar'ı seçin. Ekle'ye tıklayın.](./media/app-insights-resources-roles-access-control/01-add-user.png)

Veya başka düzeyi gidin ve aboneliğe kullanıcı eklemek.

#### <a name="select-a-role"></a>Rol seçin
![Yeni kullanıcı için bir rol seçin](./media/app-insights-resources-roles-access-control/03-role.png)

| Rol | Kaynak grubunda |
| --- | --- |
| Sahip |Kullanıcı erişim dahil her şeyi değiştirebilir |
| Katılımcı |Tüm kaynaklar da dahil olmak üzere her şeyi düzenleyebilirsiniz |
| Uygulama Öngörüler bileşen katkıda bulunan |Application Insights kaynaklara, web testleri ve Uyarıları düzenleyebilirsiniz |
| Okuyucu |Görüntüleyebilir, ancak değişikliği değil |

'Düzenleme' oluşturma, silme ve güncelleştirme içerir:

* Kaynaklar
* Web testleri
* Uyarılar
* Sürekli dışarı aktarma

#### <a name="select-the-user"></a>Kullanıcıyı seçin

İstediğiniz kullanıcıyı dizinde değilse, bir Microsoft hesabı olan herkes davet edebilirsiniz.
(Bunlar Outlook.com, OneDrive, Windows Phone veya XBox LIVE gibi hizmetleri kullanıyorsanız, bunlar bir Microsoft hesabınız vardır.)

## <a name="related-content"></a>İlgili içerik

* [Rol tabanlı erişim denetimi Azure](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
