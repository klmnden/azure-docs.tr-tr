Bir tetikleyici ekleyelim.

1. Girin *sftp* arama kutusuna logic apps tasarımcısında seçip **bir dosya eklendiğinde veya SFTP -** tetikleyici   
   ![SFTP tetikleyici görüntü 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. **Ne zaman bir dosya eklenen veya değiştirilen** denetim açılır  
   ![SFTP tetikleyici görüntü 2](./media/connectors-create-api-sftp/trigger-2.png)  
3. Seçin **...**  denetimi sağ tarafta bulunur. Bu Klasör Seçici denetim açar  
   ![SFTP tetikleyici görüntü 3](./media/connectors-create-api-sftp/action-1.png)  
4. Seçin **SFTP** için yeni veya değiştirilmiş dosyaları izlemek için klasör olarak kök klasörü seçin. Kök klasör gösterilen artık bildirim **klasörü** denetim.  
   ![SFTP tetikleyici görüntü 4](./media/connectors-create-api-sftp/action-2.png)   

Bu noktada, mantıksal uygulamanızı bir dosya değiştiren veya belirli SFTP klasörde oluşturulan diğer tetikleyiciler ve Eylemler iş akışı bir farklı çalıştır başlayacak bir tetikleyici ile yapılandırıldı. 

> [!NOTE]
> Bir mantıksal uygulama işlevsel olması en az bir tetikleyici ve bir eylem içermelidir. Bir eylem eklemek için sonraki bölümdeki adımları izleyin.  
> 
> 

