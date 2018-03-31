---
title: Unity Top bir küre öğretici
description: Klasik Unity Roll tüm Mobile Engagement Unity öğreticileri için önkoşul olan bir TOP oyuna oluşturma adımları
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 0afd0eca-f74a-43aa-bba8-436a0265c312
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 52d5962645b1408fdba61ec1bf4e4f682b80905b
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a id="unity-roll-a-ball"></a>Unity Roll Top oyuna oluşturma
> [!IMPORTANT]
> Azure Mobile Engagement 31/3/2018 üzerinde denemeler. Bu sayfa, kısa süre sonra silinir.
> 

Bu öğreticide biraz değiştirilmiş için ana adım adım anlatılmaktadır [Unity Roll bir küre öğretici](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial). Bu örnek oyun uygulaması kullanıcı tarafından denetlenen bir küresel 'player' nesnesi oluşur ve oyun amacı ' collectible nesneleri için player nesnesine collectible bu nesnelerle yazılımlarla çakışma ' toplamaktır. Bu Unity Düzenleyicisi ortamıyla temel benzer olduğu varsayılır. Ardından herhangi bir sorunla çalıştırırsanız tam öğretici başvurmalıdır. 

### <a name="setting-up-the-game"></a>Oyun ayarlama
Aşağıdaki adımlar arasındadır [Unity Öğreticisi](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Açık **Unity Düzenleyicisi** tıklatıp **yeni**. 
   
    ![][51] 
2. Sağlayan bir **proje adı** & **konumu**seçin **3B** tıklatıp **proje oluştur**.
   
    ![][52]
3. Yeni bir adla gibi yeni projenin bir parçası olarak oluşturduğunuz varsayılan Sahne Kaydet **MiniGame** içinde yeni bir  **\_planda** klasörü altında **varlıklar** klasörü:
   
    ![][53]
4. Oluşturma bir **3B nesne düzlemi ->** çalma olarak alan ve bu düzlemi nesnesi olarak yeniden adlandırın **başından başlayarak**
   
    ![][1]
5. Bu dönüştürme bileşeni sıfırlama **başından başlayarak** başlangıcı sırasında olmasını nesne. 
   
    ![][3]
6. İşaretini **Göster kılavuz** gelen **en menü** için **başından başlayarak** nesnesi.
   
    ![][4]
7. Güncelleştirme **ölçek** için bileşen **başından başlayarak** olmasını nesnesi [X = 2, Y = 1, Z = 2]. 
   
    ![][5]
8. Yeni bir ekleme **3B nesne küre ->** proje ve bu küre nesne olarak yeniden adlandır **Player**. 
   
    ![][6]
9. Seçin **Player** nesnesi ve'ı tıklatın **dönüştürmeyi Sıfırla** düzlemi nesnesine benzer. 
10. Güncelleştirme **dönüştürme -> konumu Y koordinatını ->** 0,5 olarak Player y bileşen.  
    
    ![][7]
11. Adlı yeni bir klasör oluşturun **malzemeleri** burada biz oluşturacak player renk malzeme projesinde. 
12. Yeni bir **malzeme** adlı **arka plan** bu klasörde. 
    
    ![][8]
13. Malzeme rengi güncelleştirmek **Albedo** bunu özelliği. [0,32,64] RGB değerleri seçebilirsiniz. 
    
    ![][9]
14. Bu yazıda Sahne rengi uygulamak görünüme sürükleyin **başından başlayarak** nesnesi. 
    
    ![][10]
15. Son güncelleştirme **dönüştürme -> döndürme Y ->** 60 daha anlaşılır olması için tek yönlü ışığı nesne üzerinde. 
    
    ![][12]

### <a name="moving-the-player"></a>Player taşıma
Aşağıdaki adımlar arasındadır [Unity Öğreticisi](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. Ekleme bir **RigidBody** bileşeni **Player** nesnesi. 
   
    ![][13]
2. Adlı yeni bir klasör oluşturun **betikleri** projesinde. 
3. Tıklatın **Bileşen Ekle yeni betik -> C# betik ->**. Bu ad **PlayerController**, tıklatıp **oluştur ve Ekle**. Oluşturun ve Player nesnesine bir komut dosyası ekleyin.  
   
    ![][14]
4. Bu komut dosyasını altında taşımak **betikleri** proje klasöründe. 
5. Sık kullanılan komut dosyası Düzenleyicisi'nde düzenlemek için komut dosyasını açın, komut dosyası kodu aşağıdaki kodla güncelleştirin ve kaydedin. 
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
6. Yukarıdaki komut dosyası kullanan Not bir **hızı** özelliği. Unity Düzenleyicisi'nde, 10 hızı özelliğini güncelleştirin.  
   
    ![][15]
7. İsabet **Yürüt** Unity Düzenleyicisi'nde. Şimdi klavyeyi kullanarak top denetim sahibi olmalı ve döndürme ve hareket ettirin. 

### <a name="moving-the-camera"></a>Kamera taşıma
Aşağıdaki adımlar arasındadır [Unity öğretici](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) ve tie **ana kamera** için **Player** nesnesi. 

1. Güncelleştirme **Transform.Position** X olmasını = 0, Y 10.5, Z =-10 =.  
2. Güncelleştirme **Transform.Rotation** X olmasını = 45, Y = 0, Z = 0.  
   
    ![][16]
3. Adlı yeni bir komut dosyası eklemek **CameraController** için **MainCamera** ve altında betikler klasörüne taşıyın. 
   
    ![][17]
4. Düzenlemek için komut dosyasını açın ve aşağıdaki kodu ekleyin:
   
        using UnityEngine;
        using System.Collections;
   
        public class CameraController : MonoBehaviour {
   
            public GameObject player;
   
            private Vector3 offset;
   
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
   
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
5. Böylece iki başka bir işlemle ilişkili Unity ortamında - Player değişkeni ana kamera nesnesi için Player yuvasına sürükleyin. 
   
    ![][18]
6. Şimdi Unity Düzenleyicisi'nde Çal düğmesine basman yeterli ve Player Top nesnesini döndürme hareketini izleyen kamera görürsünüz.  

### <a name="setting-up-the-play-area"></a>Oynatma alanı ayarlama
Aşağıdaki adımlar arasındadır [Unity Öğreticisi](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141). Böylece Player Top nesne hareketini oynatma alanında devre dışı bırakma değil zemin çevreleyen duvarları oluşturacağız. 

1. Tıklatın **Oluştur -> boş oluştur oyun nesnesi ->** ve adlandırın **duvarları**
   
    ![][19]
2. Bu duvarları nesne altında - yeni bir oluşturma **3B nesne küp ->** ve "Batı duvar" olarak adlandırın. 
   
    ![][20]
3. Güncelleştirme **dönüştürme konumu ->** ve **dönüştürme -> Ölçek** bu Batı duvar nesne. 
   
    ![][21]
4. Batı duvar oluşturmak için yinelenen bir **Doğu duvar** konumu ve ölçek güncelleştirilmiş ile Dönüştür. 
   
    ![][22]
5. Doğu duvar oluşturmak için yinelenen bir **Kuzey duvar** konumu & ölçek güncelleştirilmiş ile Dönüştür. 
   
    ![][23]
6. Kuzey duvar yinelenen ve oluşturma bir **Güney duvar** konumu & ölçek güncelleştirilmiş ile Dönüştür. 
   
    ![][24]

### <a name="creating-collectible-objects"></a>Collectible nesneleri oluşturma
Aşağıdaki adımlar arasındadır [Unity Öğreticisi](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141). ' Bunlarla yazılımlarla çakışma toplanacak ' Player Top nesne gereken collectible nesne kümesini oluşturan bazı çekici görünümlü nesneleri oluşturacağız. 

1. Yeni bir **3B küp nesnesi** ve toplama adlandırın. 
2. Ayarlama **dönüştürme -> döndürme** & **dönüştürme -> Ölçek** toplama nesnesinin. 
   
    ![][25]
3. Oluşturulacağı ve bir **yeni C# betik** adlı **değiştirici** toplama nesnesine. Komut dosyası komut dosyaları klasörünün altına yerleştirin emin olun. 
   
    ![][26]
4. Düzenlemek için bu komut dosyasını açın ve aşağıdaki şekilde güncelleştirin: 
   
        using UnityEngine;
        using System.Collections;
   
        public class Rotator : MonoBehaviour {
   
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }
5. Şimdi isabet Unity Düzenleyicisi'ni ve toplama nesne gösterisi oynatma modunda kendi ekseninde döndürme.
6. Adlı yeni bir klasör oluşturun **Prefabs** 
   
    ![][27]
7. Sürükleme **toplama** nesne ve Prefabs klasöre yerleştirin.
   
    ![][28]
8. Yeni bir **boş oyun nesne** adlı **Pickups**. İçin kaynak konumunu sıfırlamak ve oyun bu nesne altındaki toplama nesne sürükleyin.  
   
    ![][29]
9. Yinelenen **toplama** nesne ve üzerinde yayılan **başından başlayarak** geçici nesne **Player** güncelleştirerek nesne **Transform.Position'in X & Z** uygun şekilde değerleri. 
   
    ![][30]
10. Oluşturma bir **yeni Materyaller** adlı **toplama** ve güncelleştirerek rengi kırmızı olmasını güncelleştirme **Albedo özelliği** ne başından başlayarak nesne güncelleştirmek için yaptığımız benzer. 
    
    ![][31]
11. Malzeme tüm 4 toplama nesnelere uygulanır.
    
    ![][32]

### <a name="collecting-the-pickup-objects"></a>Toplama nesneleri toplama
Aşağıdaki adımlar arasındadır [Unity Öğreticisi](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141). Böylece 'toplama nesneler bunlarla yazılımlarla çakışma toplamak için ' Player güncelleştireceğiz. 

1. Açık **PlayerController** betiğini düzenlemek için Player nesnesine ekli ve şu şekilde güncelleştirin:  
   
        using UnityEngine;
        using System.Collections;
   
        public class PlayerController : MonoBehaviour {
   
            public float speed;
   
            private Rigidbody rb;
   
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
   
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
   
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
   
                rb.AddForce (movement * speed);
            }
   
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }
2. Yeni bir **etiketi** adlı **çekme yukarı** (Bu komut dosyasındaki nedir eşleşmelidir)  
   
    ![][33]
   
    ![][34]
3. Bu, geçerli **etiketi** Prefab toplama nesnesine. 
   
    ![][35]
4. Etkinleştirme **IsTrigger** Prefab nesne için onay kutusu.
   
    ![][36]
5. Katı gövde toplama Prefab nesnesine ekleyin. Performansı iyileştirmek için dinamik collider için kullanılan statik collider güncelleştireceğiz. 
   
    ![][37]
6. Son denetleme **IsKinematic** prefab nesnesi için özelliği. 
   
    ![][38]
7. İsabet **Yürüt** Unity Düzenleyicisi ve bu yürütmek kuramaz **Top alma** yönü girdi için klavye tuşlarını kullanarak Player nesnesine taşıyarak oyun. 

### <a name="updating-the-game-for-mobile-play"></a>Oyun mobil play için güncelleştirme
Yukarıdaki bölümlerde temel öğretici Unity gelen sonuçları. Şimdi Biz bu mobil aygıt kolay hale getirmek için oyun değiştirecektir. Klavye girişini test etmek için o ana kadarki oyun için kullandık olduğunu unutmayın. Biz player telefon hareket yani kullanarak denetleyebilmeniz için değişiklik artık Accelerometer giriş olarak kullanma. 

Açık **PlayerController** komut dosyası düzenleme için ve güncelleştirme **FixedUpdate** accelerometer gelen hareket için Player nesnesine taşımak için kullanılacak yöntemi. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Unity ile temel bir oyun oluşturma Bu öğreticiyi sonlandırır ve bu oyun oynamaya tercih ettiğiniz bir aygıta dağıtabilirsiniz. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png    
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png    
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png













