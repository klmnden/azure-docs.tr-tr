Şimdi bir tetikleyici ekleme.

1. Girin *sftp* logic apps Tasarımcısı'nda arama kutusuna seçip **SFTP - bir dosya eklendiğinde veya değiştirildiğinde** tetikleyicisi   
   ![SFTP tetikleyici görüntü 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. **Ne zaman bir dosya eklendiğinde veya değiştirildiğinde** denetimi açılır  
   ![2 SFTP tetikleyicisi görüntüsü](./media/connectors-create-api-sftp/trigger-2.png)  
3. Seçin **...**  denetimin sağ tarafında bulunur. Bu klasör seçici denetimi açar  
   ![SFTP tetikleyicisi görüntüsü 3](./media/connectors-create-api-sftp/action-1.png)  
4. Seçin **SFTP** için yeni veya değiştirilmiş dosyaları izlemek için klasör olarak kök klasörünü seçin. Kök klasör görüntüleniyor artık bildirim **klasör** denetimi.  
   ![SFTP tetikleyicisi görüntüsü 4](./media/connectors-create-api-sftp/action-2.png)   

Bu noktada, mantıksal uygulamanızın bir dosya değiştirildiğinde veya belirli bir SFTP klasöründe oluşturulan bir tetikleyici ve Eylemler iş akışı çalıştırma başlayacak bir tetikleyici ile yapılandırıldı. 

> [!NOTE]
> Bir mantıksal uygulama işlevsel olması en az bir tetikleyici ve bir eylem içermelidir. Bir eylem eklemek için sonraki bölümdeki adımları izleyin.  
> 
> 

