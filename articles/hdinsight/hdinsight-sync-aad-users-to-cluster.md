---
title: Bir kümeye - Azure Hdınsight Azure Active Directory Kullanıcıları eşitlemeye | Microsoft Docs
description: Kimliği doğrulanmış kullanıcılara bir küme Azure Active Directory'den eşitleyin.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: ashishth
ms.openlocfilehash: f2deaaa31a4d0e8a91d048b538e9251a8eb9e1b7
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="synchronize-azure-active-directory-users-to-an-hdinsight-cluster"></a>Hdınsight kümesi için Azure Active Directory Kullanıcıları Eşitle

[Etki alanına katılmış Hdınsight kümeleri](hdinsight-domain-joined-introduction.md) Azure Active Directory (Azure AD) kullanıcılarla güçlü kimlik doğrulaması kullanma yanı sıra kullanın *rol tabanlı erişim denetimi* (RBAC) ilkeleri. Azure AD ile kullanıcı ve grup ekleme gibi kümenize erişmek isteyen kullanıcılar eşitleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Zaten bunu yapmadıysanız [bir etki alanına katılmış Hdınsight kümesi oluşturma](hdinsight-domain-joined-configure.md).

## <a name="add-new-azure-ad-users"></a>Yeni Azure eklemek AD kullanıcıları

Konaklarınızın görüntülemek için Ambari Web kullanıcı arabirimini açın. Her düğüm katılımsız yeni yükseltme ayarlarını güncelleştirilir.

1. İçinde [Azure portal](https://portal.azure.com), etki alanına katılmış kümenizle ilişkilendirilmiş Azure AD dizinine gidin.

2. Seçin **tüm kullanıcılar** sol taraftaki menüden seçip **yeni kullanıcı**.

    ![Tüm kullanıcılar bölmesi](./media/hdinsight-sync-aad-users-to-cluster/aad-users.png)

3. Yeni kullanıcı formu doldurun. Küme tabanlı izinleri atamak için oluşturulan grupları seçin. Bu örnekte, yeni kullanıcılar atayabilirsiniz "HiveUsers" adlı bir grup oluşturun. [Örnek yönergeleri](hdinsight-domain-joined-configure.md) etki alanına katılmış bir küme oluşturmak için iki grup eklenmesi `HiveUsers` ve `AAD DC Administrators`.

    ![Yeni kullanıcı bölmesi](./media/hdinsight-sync-aad-users-to-cluster/aad-new-user.png)

4. **Oluştur**’u seçin.

## <a name="use-the-ambari-rest-api-to-synchronize-users"></a>Kullanıcıların eşitlemek için Ambari REST API kullanın

Küme oluşturma işlemi sırasında belirtilen kullanıcı grupları, o anda eşitlenir. Kullanıcı eşitleme otomatik olarak saatte bir gerçekleşir. Kullanıcıların hemen eşitleme ya da küme oluşturma sırasında belirttiğiniz grupları dışındaki bir grubu eşitlemek için Ambari REST API kullanın.

Aşağıdaki yöntemi POST ile Ambari REST API kullanır. Daha fazla bilgi için bkz: [Hdınsight kümelerini yönetme Ambari REST API kullanarak](hdinsight-hadoop-manage-ambari-rest-api.md).

1. [SSH kümenizle bağlanmak](hdinsight-hadoop-linux-use-ssh-unix.md). Azure portalında, kümeniz için genel bakış bölmesinden seçin **güvenli Kabuk (SSH)** düğmesi.

    ![Secure Shell (SSH)](./media/hdinsight-sync-aad-users-to-cluster/ssh.png)

2. Görüntülenen kopyalama `ssh` komut ve SSH istemciniz yapıştırın. Girin ssh istendiğinde kullanıcı parolası.

3. Kimlik doğrulandıktan sonra aşağıdaki komutu girin:

    ```bash
    curl -u admin:<YOUR PASSWORD> -sS -H "X-Requested-By: ambari" \
    -X POST -d '{"Event": {"specs": [{"principal_type": "groups", "sync_type": "existing"}]}}' \
    "https://<YOUR CLUSTER NAME>.azurehdinsight.net/api/v1/ldap_sync_events"
    ```
    
    Yanıt aşağıdaki gibi görünmelidir:

    ```json
    {
      "resources" : [
        {
          "href" : "http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1",
          "Event" : {
            "id" : 1
          }
        }
      ]
    }
    ```

4. Eşitleme durumunu görmek için yeni bir yürütme `curl` komutunu `href` önceki komuttan döndürülen değer:

    ```bash
    curl -u admin:<YOUR PASSWORD> http://hn0-hadoop.<YOUR DOMAIN>.com:8080/api/v1/ldap_sync_events/1
    ```
    
    Yanıt aşağıdaki gibi görünmelidir:
    
    ```json
    {
      "href" : "http://hn0-hadoop.YOURDOMAIN.com:8080/api/v1/ldap_sync_events/1",
      "Event" : {
        "id" : 1,
        "specs" : [
          {
            "sync_type" : "existing",
            "principal_type" : "groups"
          }
        ],
        "status" : "COMPLETE",
        "status_detail" : "Completed LDAP sync.",
        "summary" : {
          "groups" : {
            "created" : 0,
            "removed" : 0,
            "updated" : 0
          },
          "memberships" : {
            "created" : 1,
            "removed" : 0
          },
          "users" : {
            "created" : 1,
            "removed" : 0,
            "skipped" : 0,
            "updated" : 0
          }
        },
        "sync_time" : {
          "end" : 1497994072182,
          "start" : 1497994071100
        }
      }
    }
    ```

5. Bu sonuç durumu gösterir **tam**, yeni bir kullanıcı oluşturuldu ve kullanıcı bir üyelik atandı. Bu örnekte, "eşitlenmiş HiveUsers için" Kullanıcının atandığı kullanıcı Azure AD'de aynı bu gruba eklendikten sonra LDAP grubu.

> [!NOTE]
> Önceki yöntemi yalnızca belirtilen Azure AD grupları eşitler **erişim kullanıcı grubu** küme oluşturma sırasında etki alanı ayarları özelliği. Daha fazla bilgi için bkz: [bir Hdınsight kümesi oluşturmayı](domain-joined/apache-domain-joined-configure.md).

## <a name="verify-the-newly-added-azure-ad-user"></a>Yeni eklenen doğrulayın Azure AD kullanıcı

Açık [Ambari Web kullanıcı arabirimini](hdinsight-hadoop-manage-ambari.md) doğrulamak için yeni Azure AD kullanıcı eklendi. Ambari Web kullanıcı arabirimini göz atarak erişim **`https://<YOUR CLUSTER NAME>.azurehdinsight.net`**. Küme Yönetici kullanıcı adı ve parola girin.

1. Ambari panodan seçin **yönetmek Ambari** altında **yönetici** menüsü.

    ![Ambari yönetme](./media/hdinsight-sync-aad-users-to-cluster/manage-ambari.png)

2. Seçin **kullanıcılar** altında **kullanıcı + Grup Yönetimi** sayfanın sol taraftaki menü grubu.

    ![Kullanıcılar menü öğesi](./media/hdinsight-sync-aad-users-to-cluster/users-link.png)

3. Yeni kullanıcı kullanıcılar tablo içinde listelenmelidir. Türü kümesine `LDAP` yerine `Local`.

    ![Kullanıcılar sayfası](./media/hdinsight-sync-aad-users-to-cluster/users.png)

## <a name="log-in-to-ambari-as-the-new-user"></a>Ambari için yeni bir kullanıcı olarak oturum açın

Yeni kullanıcı (veya başka bir etki alanı kullanıcı) için Ambari açtığında tam Azure AD kullanıcı adı ve etki alanı kimlik bilgilerini kullanırlar.  Ambari, Azure AD'de kullanıcı görünen adı olan bir kullanıcı diğer adı görüntüler. Yeni örnek kullanıcının kullanıcı adına sahip `hiveuser3@contoso.com`. Ambari bu yeni kullanıcı olarak görüntülenir `hiveuser3` ancak Ambari kullanıcının oturum açtığı `hiveuser3@contoso.com`.

## <a name="see-also"></a>Ayrıca bkz.

* [Etki alanına katılmış Hdınsight'ta Hive ilkelerini yapılandırma](hdinsight-domain-joined-run-hive.md)
* [Etki alanına katılmış Hdınsight kümelerini yönetme](hdinsight-domain-joined-manage.md)
* [Ambari için Kullanıcıları yetkilendirmek](hdinsight-authorize-users-to-ambari.md)
