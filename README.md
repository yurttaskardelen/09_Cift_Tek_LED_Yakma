# 09_Cift_Tek_LED_Yakma (Flaşör Efekti)

Bu proje, **STM32F407-Discovery** kartı üzerinde 4 adet LED kullanarak **çift/tek flaşör** (alternatif yanıp sönme) animasyonu gerçekleştirir.

Bu depo, `08` numaralı projedeki **"paralel dizi"** yönteminin geliştirilmiş bir versiyonudur. Bu kez, animasyonun her bir adımı (`çiftler yansın`, `tekler yansın`) için ayrı bir "durum dizisi" (state array) tanımlanmıştır.

* **Pin Dizisi:** `ledler[]` (Hangi pinlerin kullanılacağını belirler)
* **Durum Dizileri:** `led_durum_cift[]` ve `led_durum_tek[]` (Animasyon desenlerini belirler)

Bu yöntem, karmaşık desenleri veya animasyon adımlarını "veri" olarak (dizilerde) saklayarak ana kod bloğunu (`while(1)`) son derece temiz ve kısa tutmayı sağlar.

> **💡 Tekniklerin Kombinasyonu**
>
> Bu proje, önceki projelerde öğrendiğimiz iki güçlü tekniği birleştirir:
>
> 1.  **Paralel Dizi (`08`):** `for` döngüsü ve `led_durum_...[]` dizileri kullanılarak LED'ler *yakılır*.
> 2.  **Bitwise OR (`06`):** `HAL_GPIO_WritePin(..., PIN_1 | PIN_2 | ...)` komutu kullanılarak tüm LED'ler tek seferde, verimli bir şekilde *söndürülür*.

---

### 🎯 Proje Senaryosu

Kod, `while(1)` döngüsü içinde sürekli olarak iki ana adımı tekrar eder:

1.  **Aşama 1 (Çift LED'ler):**
    * `led_durum_cift[]` dizisi (`{0,1,0,1}`) uygulanır.
    * `PA2` ve `PA4` LED'leri yanar.
    * 500 ms beklenir.
    * Tüm LED'ler (Bitwise OR ile) söndürülür.
2.  **Aşama 2 (Tek LED'ler):**
    * `led_durum_tek[]` dizisi (`{1,0,1,0}`) uygulanır.
    * `PA1` ve `PA3` LED'leri yanar.
    * 500 ms beklenir.
    * Tüm LED'ler (Bitwise OR ile) söndürülür.
3.  Döngü başa döner.

**Sonuç:** Çift ve tek LED'ler yarım saniye arayla birbirleriyle yer değiştirerek yanıp söner (flaşör efekti).

---

### 🛠️ Gerekli Donanım

* **1x** STM32F407-Discovery Geliştirme Kartı
* **4x** Tercih edilen renklerde LED
* **4x** 220 ya da 330 Ohm Direnç (LED'ler için ön direnç)
* Breadboard ve Jumper kablolar

---

### 🔌 Devre Şeması

LED'lerin anot (uzun) bacakları STM32 pinlerine, katot (kısa) bacakları ise direnç üzerinden GND hattına bağlanmalıdır.

| LED | Direnç | STM32 Pini |
| :--- | :--- | :--- |
| LED 1 | 220 Ohm | `PA1` |
| LED 2 | 220 Ohm | `PA2` |
| LED 3 | 220 Ohm | `PA3` |
| LED 4 | 220 Ohm | `PA4` |
| (Tümü) | - | `GND` |

<img width="473" height="404" alt="Pin_Baglantilari" src="https://github.com/user-attachments/assets/2faf879d-af80-4f97-9495-9c89e4afac5b" />

### Kod Bloğu

<img width="1107" height="660" alt="image" src="https://github.com/user-attachments/assets/e4a27139-9f55-46e4-95d8-76bc2dcb9070" />

---

### 🚀 Nasıl Kullanılır?

1.  Bu depoyu klonlayın (`git clone ...`).
2.  STM32CubeIDE yazılımını açın.
3.  `File > Open Projects from File System...` seçeneği ile proje klasörünü seçin.
4.  Proje içindeki `.ioc` dosyasını açarak pin yapılandırmasını inceleyebilirsiniz.
5.  Derleyin (Build) ve ST-Link V2 üzerinden kartınıza yükleyin (Run).
