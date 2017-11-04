---
title: "Dosya ve klasör düzeyleri - Azure Hdınsight kullanıcı izinlerini Yönet | Microsoft Docs"
description: "Etki alanına katılmış Hdınsight kümeleri için dosya ve klasör izinlerini yönetmek üzere nasıl."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: maxluk
ms.openlocfilehash: 9ca91721e691eca239478c4ac8b85e2652babdfd
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="manage-user-permissions-at-the-file-and-folder-levels"></a>Dosya ve klasör düzeyinde kullanıcı izinlerini Yönet

[Etki alanına katılmış Hdınsight kümeleri](./domain-joined/apache-domain-joined-introduction.md) Azure Active Directory (Azure AD) kullanıcılar, güçlü kimlik doğrulaması kullanmak ve aynı zamanda *rol tabanlı erişim denetimi* YARN ve Hive gibi çeşitli hizmetler için (RBAC) ilkeleri. Varsayılan veri deponuza kümeniz için Azure Storage veya Windows Azure depolama BLOB'lar (WASB) ise, dosya ve klasör düzeyinde izinler zorunlu kılabilir. Apache bırakabilmenizi, eşitlenen kümenin dosyalara erişimi denetlemek için kullanabileceğiniz Azure AD kullanıcıları ve grupları.
<!-- [synchronized Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md). -->

Etki alanına katılmış Hdınsight kümeleri için Apache bırakabilmenizi örneği bırakabilmenizi WASB hizmeti ile önceden yapılandırılmış olarak gelir. Bırakabilmenizi-HDFS için benzer bir ilke yönetimi altyapısı bırakabilmenizi WASB hizmetidir ancak farklı bir uygulama bırakabilmenizi'nın erişim ilkeleri. Gelen bir kaynak isteği eşleşen bırakabilmenizi İlkesi yoksa bırakabilmenizi WASB hizmet REDDETME varsayılan yanıt vermiyor. Bırakabilmenizi hizmeti için WASB denetimi iznini geçmez.

## <a name="permission-and-policy-model"></a>İzin ve ilke modeli

Kaynak erişim istekleri aşağıdaki akış kullanarak değerlendirilir:

![Apache bırakabilmenizi ilke değerlendirme akışı](./media/hdinsight-add-acls-at-file-folder-levels/ranger-policy-evaluation-flow.png)

İzin verme kuralları izin verme kuralları tarafından izlenen önce değerlendirilir. İlke eşleşirse, eşleşen sonunda, REDDETME döndürülür.

### <a name="user-variable"></a>Kullanıcı değişkeni

Kullanabileceğiniz `{USER}` erişmek kullanıcı için ilke atarken değişkeni bir `/{username}` alt, örneğin:

```
resource: path=/app-logs/{USER}, user: {USER}, recursive=true, permissions: all, delegateAdmin=true
```

Yukarıdaki ilke kullanıcılara altında kendi alt klasörüne erişimi verir. `/app-logs/` dizin. Bu ilkeyi nasıl bırakabilmenizi kullanıcı arabiriminde göründüğünü aşağıda verilmiştir:

![Kullanımı kullanıcı değişkeni örneği](./media/hdinsight-add-acls-at-file-folder-levels/user-variable.png)

### <a name="policy-model-examples"></a>İlke model örnekleri

Aşağıdaki tabloda, ilke modeli nasıl çalıştığını birkaç örnek listelenmektedir:

| Bırakabilmenizi İlkesi | Var olan dosya sistemi | Kullanıcı isteği | Sonuç |
| -- | -- | -- | -- |
| Regression/Finans /, bob, yazma | Regression | Kemal, dosya /data/finance/mydatafile.txt oluştur | İzin ver - Ara klasörü 'Finans', üst onay nedeniyle oluşturulur |
| Regression/Finans /, bob, yazma | Regression | alice, Create dosya /data/finance/mydatafile.txt | REDDETME - eşleşen ilke yok |
| / data/Finans *, bob, yazma | Regression | Kemal, dosya /data/finance/mydatafile.txt oluştur | İzin ver - Bu isteğe bağlı yinelemeli ilke durumda (`*`) şu sunmak; bkz [joker karakterler](#wildcards) |
| /Data/Finance/mydatafile.txt, bob, yazma | Regression | Kemal, dosya /data/finance/mydatafile.txt oluştur | REDDETME - üst denetimi ' / veri ' hiçbir ilke olduğundan başarısız |
| /Data/Finance/mydatafile.txt, bob, yazma | / data/Finans | Kemal, dosya /data/finance/mydatafile.txt oluştur | REDDETME - '/ data/Finans' üst denetimi için ilke yok |

İşlem türüne göre dosya düzeyinde veya klasör düzeyinde izinler gereklidir. Örneğin, "oluşturma" çağrısı üst klasör düzeyinde izinler gerektirse "okuma/Aç" çağrısı dosya düzeyinde okuma erişimi gerektirir.

### <a name="wildcards-"></a>Joker karakter (*)

Joker karakter olduğunda (`*`) mevcut bir ilke yolu joker yol ve tüm alt ağacını uygular. Bu özyineleme kullanarak aynı olan bir `recurse-flag`. Bırakabilmenizi-WASB, joker karakter özyineleme ve kısmi adıyla eşleşen gösterir.

## <a name="manage-file-and-folder-level-permissions-with-apache-ranger"></a>Dosya ve klasör düzeyinde izinler Apache bırakabilmenizi ile yönetme

Zaten yapmadıysanız, izleyin [bu yönergeleri](./domain-joined/apache-domain-joined-configure.md) yeni bir etki alanına katılmış kümesi sağlamak için.

Açık bırakabilmenizi WASB göz atarak `https://<YOUR CLUSTER NAME>.azurehdinsight.net/ranger/`. Küme Yönetici kullanıcı adı ve kümenizi oluşturulurken tanımlanan parolayı girin.

Oturum açtıktan sonra bırakabilmenizi Pano bakın:

![Bırakabilmenizi Panosu](./media/hdinsight-add-acls-at-file-folder-levels/ranger-dashboard.png)

Kümenizi ilişkili Azure depolama hesabı için geçerli dosya ve klasör izinlerini görüntülemek için  ***CLUSTERNAME*_wasb** WASB denetimi kutusundaki bağlantı.

![Bırakabilmenizi Panosu](./media/hdinsight-add-acls-at-file-folder-levels/wasb-dashboard-link.png)

Geçerli ilke listesi görüntülenir. Çeşitli tipik ilkeler bir başlangıç noktası olarak dahil edilen - örnek kullanımları görmek için her ilke ayrıntılarını kontrol edin.

Her ilke için ilke etkinleştirilip etkinleştirilmediği, Denetim günlüğü olup yapılandırılmış ve tüm atanan gruplar ve kullanıcılar görebilirsiniz. Her ilke için iki eylem düğmesi vardır: düzenleyebilir ve silebilirsiniz.

![İlke listesi](./media/hdinsight-add-acls-at-file-folder-levels/policy-list.png)

### <a name="adding-a-new-policy"></a>Yeni bir ilke ekleme

1. Üstte WASB ilkeleri sayfasının sağ seçin **yeni ilke Ekle**.

    ![Yeni bir ilke ekleme](./media/hdinsight-add-acls-at-file-folder-levels/add-new.png)

2. Bir tanımlayıcı girin **ilke adı**. Azure belirtin **depolama hesabı** kümeniz için (*ACCOUNT_NAME*. blob.core.windows.net) ve **depolama hesabı kapsayıcısının** oluşturduğunuzda belirtilen, Küme. Girin **göreli yol** (küme göre) erişilen klasör veya dosya için.

    ![Yeni ilke formu](./media/hdinsight-add-acls-at-file-folder-levels/new-policy.png)

3. Formun belirtin, **izin koşulları** bu yeni kaynak için. Geçerli gruplar ve kullanıcılar seçin ve onların izinlerini ayarlayın. Aşağıdaki örnekte, biz tüm kullanıcılara izin verme `sales` okuma/yazma erişimi için Grup.

    ![Satış izin ver](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales.png)

4. **Kaydet**’i seçin.

### <a name="example-policy-conditions"></a>Örnek ilke koşulları

Apache bırakabilmenizi [ilke değerlendirme akış](#permission-and-policy-model) herhangi bir bileşimini belirtmenizi sağlar izin verme ve ihtiyaçlarınızı karşılamak için koşullar reddetme. İşte birkaç örnek:

1. Tüm satış kullanıcılar, ancak hiçbir Stajyer izin ver:

    ![Satış, izin Stajyer Reddet](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns.png)

2. Kimin okuma erişimi olması "hiveuser3" adlı bir stajyer hariç tüm Stajyer Reddet ve satış tüm kullanıcılara izin ver:

    ![Satış, izin hiveuser3 dışında Stajyer Reddet](./media/hdinsight-add-acls-at-file-folder-levels/allow-sales-deny-interns-except-hiveuser3.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Etki alanına katılmış Hdınsight'ta Hive ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)
* [Etki alanına katılmış Hdınsight kümelerini yönetme](./domain-joined/apache-domain-joined-manage.md)
* [Ambari yönetme - Ambari için Kullanıcıları yetkilendirmek](hdinsight-authorize-users-to-ambari.md)

<!-- * [Synchronize Azure AD users and groups](hdinsight-sync-aad-users-to-cluster.md) -->

