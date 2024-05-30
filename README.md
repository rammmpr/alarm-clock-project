Proyek ini adalah jam alarm digital yang menampilkan waktu pada layar 7 segmen 4 digit. Berikut adalah fitur-fiturnya:

- Dua tombol untuk mengatur waktu saat ini (Jam / Menit)
- Alarm yang dapat diprogram dengan fungsi tunda
- Suara alarm dapat dengan mudah disesuaikan dalam kode, bahkan memainkan melodi
- Simbol titik dua berkedip untuk menunjukkan detik
- RTC opsional untuk membuat jam lebih akurat
- Pemulihan kehilangan daya: waktu saat ini dan pengaturan alarm disimpan di RTC

### Cara Menggunakan Jam

Untuk mengatur waktu, tekan tombol Menit/Jam.

Menekan tombol Alarm mengaktifkan/menonaktifkan alarm. Layar akan menampilkan status alarm dengan menunjukkan kata "on" atau "off".

Setelah mengaktifkan alarm, waktu alarm saat ini akan ditampilkan selama beberapa detik. Anda dapat menggunakan tombol Menit/Jam untuk menyesuaikan waktu alarm. Untuk selesai, tekan tombol Alarm lagi, atau tunggu beberapa detik.

Ketika alarm berbunyi, tekan tombol Alarm sebentar untuk menunda selama [9 minutes]. Layar akan menampilkan empat lingkaran untuk memberi tahu Anda bahwa alarm ditunda.

Untuk menghentikan alarm, tahan tombol Alarm selama satu detik atau lebih.

### Struktur Proyek

Kode dibagi menjadi beberapa modul:

1. [config.h](#source-config_h) - Opsi konfigurasi untuk jam: penggunaan chip RTC, waktu Tunda, dan lainnya.                          
2. [alarm-clock.ino](#source-alarm_clock_ino) - Kode program utama yang mengelola antarmuka pengguna: tampilan 7 segmen dan tombol.                
3. [Clock](#source-clock_h) - Kelas `Clock` yang mengelola waktu saat ini dan mesin keadaan alarm. Menggunakan RTClib library untuk berkomunikasi dengan chip RTC. 
4. [AlarmTone](#source-clock_h) - Kelas `AlarmTone` yang memainkan suara alarm. Anda dapat menyesuaikan nada dan melodi alarm dengan mengubah nilai array `TONES`, `TONE_TIME`, dan `TONE_SPACING`. 

### Hardware

| Nama Perangkat         | Kuantitas | Keterangan                                                             |
| ---------------------- | --------- | ---------------------------------------------------------------------- |
| Arduino Uno R3         | 1         |                                                                      |
| 4-Digit 7-Segment      | 4         | Common Anode, 14 pin                                                  |
| Resistor 220Ω          | 8         | Hubungkan ke pin segmen 7 segmen                                      |
| Transistor PNP         | 4         | Opsional, direkomendasikan                                            |
| Resistor 4.7kΩ         | 4         | Diperlukan jika menggunakan transistor PNP                           |
| Tombol Tekan 12mm      | 3         |                                                                      |
| Buzzer Piezo           | 1         | Digunakan untuk alarm                                                  |
| DS1307 RTC             | 1         | Opsional      

- Anda juga dapat menggunakan tampilan 7-segmen Common Cathode, cukup sesuaikan konstanta DISPLAY_TYPE di config.h, dan beralih ke transistor NPN.

ntuk menjaga perangkat keras jam agar seminimal mungkin, Arduino mengontrol tampilan 7-Segment secara langsung, menggunakan perpustakaan SevSeg.
Pendekatan ini memiliki kelemahan, yaitu menggunakan 12 pin GPIO!

Jika Anda ingin menghemat pin Arduino, Anda dapat menggunakan register geser 74HC595 untuk mengurangi penggunaan pin menjadi 6, atau bahkan tampilan 7-Segment dengan chip pengendali terintegrasi, seperti
TM1637, HT16K33, atau MAX7219. Dalam hal ini, Anda perlu mengubah kode untuk menggunakan perpustakaan tampilan yang berbeda (SevSeg tidak mendukung kasus ini), tetapi ini di luar cakupan proyek ini.

### Koneksi Pin

| Pin Arduino Uno | Nama Perangkat  | Pin Perangkat |
| --------------- | --------------- | ------------- |
| 2               | 7-Segment       | 14 (Dig 1)    |
| 3               | 7-Segment       | 11 (Dig 2)    |
| 4               | 7-Segment       | 10 (Dig 3)    |
| 5               | 7-Segment       | 6 (Dig 4)     |
| 6               | 7-Segment       | 13 (A)        |
| 7               | 7-Segment       | 9 (B)         |
| 8               | 7-Segment       | 4 (C)         |
| 9               | 7-Segment       | 2 (D)         |
| 10              | 7-Segment       | 1 (E)         |
| 11              | 7-Segment       | 12 (F)        |
| 12              | 7-Segment       | 5 (G)         |
| 13              | 7-Segment       | 8 (Colon)     |
| A0              | Tombol Jam      | -             |
| A1              | Tombol Menit    | -             |
| A2              | Tombol Alarm    | -             |
| A3              | Buzzer / Speaker| -             |
| A4              | DS1307 RTC      | SDA           |
| A5              | DS1307 RTC      | SCL           |

- Nomor pin untuk tampilan 7-segmen Anda mungkin berbeda. Silakan lihat lembar data yang relevan dengan perangkat Anda untuk mengetahui nomor pin yang relevan.
