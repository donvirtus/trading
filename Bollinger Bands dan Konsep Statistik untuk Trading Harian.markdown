# Bollinger Bands dan Konsep Statistik untuk Trading Harian

Selamat datang, Mr. Don! Dokumen ini merangkum penjelasan tentang **Bollinger Bands (BB)**, **deviasi**, **simpangan baku**, **margin of error**, dan **rolling deviation**, serta bagaimana konsep-konsep ini saling terhubung dalam konteks trading harian menggunakan pendekatan multi-timeframe (5m, 15m, 1h, 1d). Ditulis oleh Bejo, asisten setia Mr. Don, dengan gaya santai namun detail untuk membantu membangun sistem prediksi harga yang simpel, akurat, dan *user-friendly*.

## 1. Pengenalan Bollinger Bands (BB)

Bollinger Bands adalah indikator teknikal yang digunakan untuk mengukur **volatilitas** dan **posisi relatif harga** terhadap rata-rata. BB terdiri dari tiga garis:

- **Middle Band**: Simple Moving Average (SMA, default 20-periode).
- **Upper Band**: SMA + (k × simpangan baku, default k=2).
- **Lower Band**: SMA - (k × simpangan baku).

**Fungsi utama BB**:

- Mengukur volatilitas: Band lebar = pasar volatile, band sempit (squeeze) = volatilitas rendah.
- Menunjukkan posisi harga: Dekat upper band (potensi overbought), lower band (oversold), atau di tengah (konsolidasi).
- **Catatan**: BB bukan alat prediksi langsung, tapi memberi konteks untuk tren, reversal, atau breakout. Kombinasikan dengan indikator lain (RSI, MACD) untuk prediksi arah harga.

## 2. Konsep Statistik Terkait BB

Berikut penjelasan detail tentang konsep statistik yang terkait dengan BB, yang menjadi inti dari indikator ini.

### 2.1. Deviasi (( x_i - \\bar{x} ))

- **Definisi**: Selisih antara satu titik data (( x_i ), misalnya harga *close*) dan rata-rata data (( \\bar{x} ), biasanya SMA).
- **Rumus**: \[ \\text{Deviasi} = x_i - \\bar{x} \]
- **Arti**: Menunjukkan seberapa jauh harga menyimpang dari rata-rata. Positif = di atas SMA (bullish), negatif = di bawah (bearish).
- **Contoh**: Harga *close* BTC/USD di TF 5m = 60.500, SMA 20-periode = 60.000. Deviasi = ( 60.500 - 60.000 = 500 ).
- **Hubungan dengan BB**: Deviasi adalah bahan dasar untuk hitung simpangan baku, yang menentukan lebar band.

### 2.2. Simpangan Baku (( \\sigma ))

- **Definisi**: Ukuran rata-rata sebaran data dari rata-ratanya, mencerminkan volatilitas.
- **Rumus**: \[ \\sigma = \\sqrt{\\frac{\\sum (x_i - \\bar{x})^2}{n}} \]
  - ( x_i ): Harga *close* per candle.
  - ( \\bar{x} ): SMA.
  - ( \\sum (x_i - \\bar{x})^2 ): Jumlah kuadrat deviasi.
  - ( n ): Jumlah periode (default 20 di BB).
- **Arti**: Semakin besar (\\sigma), semakin volatile pasar, sehingga band BB melebar.
- **Contoh**: Jumlah kuadrat deviasi untuk 20 candle = 1.000.000, n=20. Maka: \[ \\sigma = \\sqrt{\\frac{1.000.000}{20}} = \\sqrt{50.000} \\approx 223.6 \] Upper band = SMA + (2 × 223.6), lower band = SMA - (2 × 223.6).
- **Hubungan dengan BB**: Simpangan baku adalah inti BB, menentukan upper dan lower band.

### 2.3. Margin of Error (MoE)

- **Definisi**: Ukuran ketidakpastian dalam estimasi rata-rata, biasanya untuk interval kepercayaan.
- **Rumus**: \[ \\text{MoE} = z \\times \\frac{\\sigma}{\\sqrt{n}} \]
  - ( z ): Z-score (misalnya 1.96 untuk 95% confidence level).
  - ( \\sigma ): Simpangan baku.
  - ( n ): Jumlah sampel.
- **Arti**: Menunjukkan rentang di sekitar rata-rata yang kemungkinan mencakup nilai sebenarnya.
- **Hubungan dengan BB**: Tidak langsung digunakan, tapi band BB mirip konsep MoE karena mencakup \~95% harga (untuk k=2, jika data normal). BB lebih fokus pada volatilitas, bukan ketidakpastian rata-rata.
- **Contoh**: Kalau (\\sigma = 223.6), n=20, z=1.96, maka: \[ \\text{MoE} = 1.96 \\times \\frac{223.6}{\\sqrt{20}} \\approx 98 \] Ini bukan bagian BB, tapi bisa dipakai untuk evaluasi prediksi model ML.

### 2.4. Rolling Deviation

- **Definisi**: Deviasi (( x_i - \\bar{x} )) yang dihitung berulang pada jendela bergulir (misalnya 20 candle).
- **Arti**: Menunjukkan penyimpangan harga dari SMA yang terus diperbarui setiap candle baru.
- **Hubungan dengan BB**: Rolling deviation adalah langkah awal untuk hitung simpangan baku BB. Setiap candle, kita hitung deviasi, kuadratkan, rata-ratakan, lalu ambil akar untuk dapat (\\sigma).
- **Contoh**: Di TF 5m, harga *close* = 60.500, SMA = 60.000, deviasi = 500. Candle berikutnya, jendela bergeser, hitung ulang.

## 3. Hubungan Antar Konsep

- **Deviasi → Simpangan Baku**: Deviasi (( x_i - \\bar{x} )) dikuadratkan dan dirata-ratakan untuk jadi simpangan baku ((\\sigma)).
- **Simpangan Baku → BB**: (\\sigma) menentukan lebar band BB, mencerminkan volatilitas.
- **Rolling Deviation**: Deviasi dihitung dalam jendela bergulir, jadi bahan dasar simpangan baku BB.
- **Margin of Error**: Tidak langsung dipakai di BB, tapi konsepnya mirip karena band BB seperti “margin volatilitas” harga.

## 4. Aplikasi dalam Trading Harian Multi-Timeframe

Mr. Don sedang kembangkan strategi trading harian dengan timeframe 5m, 15m, 1h, dan 1d, menggunakan BB dan model ML untuk prediksi arah harga atau candle count. Berikut aplikasinya:

- **TF Besar (1h, 1d)**: Gunakan simpangan baku BB untuk konfirmasi tren global. Band lebar + harga di atas middle band = bullish.
- **TF Kecil (5m, 15m)**: Gunakan rolling deviation untuk timing entry. Misalnya, deviasi negatif besar (harga dekat lower band) di 5m + tren bullish di 1d = sinyal long.
- **Model ML**: Tambah fitur seperti:
  - Deviasi relatif: ( \\frac{x_i - \\bar{x}}{\\bar{x}} ).
  - Lebar band: ( \\frac{\\text{upper band} - \\text{lower band}}{\\text{SMA}} ).
  - Simpangan baku: (\\sigma) sebagai ukuran volatilitas.
- **Backtesting**: Uji strategi di data historis (misalnya BTC/USD, 3 bulan) untuk lihat akurasi sinyal BB.

## 5. Contoh Kode Python

Berikut kode sederhana untuk hitung BB, deviasi, dan simpangan baku dari data harga:

```python
import pandas as pd
import pandas_ta as ta

# Asumsi df punya kolom 'close' dari exchanger (misalnya Binance)
df = pd.DataFrame({'close': [60000, 60500, 60200, 60300, 60700, ...]})
df['sma'] = df['close'].rolling(window=20).mean()  # SMA 20-periode
df['deviation'] = df['close'] - df['sma']  # Rolling deviation
bb = ta.bbands(df['close'], length=20, std=2)  # BB dengan k=2
df['bb_upper'] = bb['BBU_20_2.0']
df['bb_middle'] = bb['BBM_20_2.0']
df['bb_lower'] = bb['BBL_20_2.0']
df['bb_width'] = (df['bb_upper'] - df['bb_lower']) / df['bb_middle']  # Lebar band relatif

print(df[['close', 'sma', 'deviation', 'bb_upper', 'bb_middle', 'bb_lower', 'bb_width']])
```

**Output** (contoh):

| close | sma | deviation | bb_upper | bb_middle | bb_lower | bb_width |
| --- | --- | --- | --- | --- | --- | --- |
| 60500 | 60000 | 500 | 61000 | 60000 | 59000 | 0.033 |

## 6. Tips Praktis

- **Gunakan BB untuk Konteks**: Di TF 1d, cek simpangan baku untuk tren global; di 5m, gunakan rolling deviation untuk timing entry.
- **Kombinasi Indikator**: Tambah RSI atau MACD untuk konfirmasi arah harga, karena BB tidak prediksi langsung.
- **Model ML**: Integrasikan deviasi dan simpangan baku sebagai fitur untuk prediksi arah harga atau candle count.
- **Custom BB**: Coba periode lebih kecil (10-15) untuk TF 5m agar lebih sensitif, atau k=2.5 untuk band lebih lebar.

## 7. Catatan untuk Mr. Don

BB dan konsep statistik ini adalah alat bantu, bukan ramalan pasti. Pasar sering tidak normal (p-value Shapiro-Wilk &lt; 0.05), jadi gunakan model ML robust seperti XGBoost. Fokus ke *feature engineering* dan backtesting untuk strategi multi-timeframe yang akurat dan *seamless*.

Kalau ada pertanyaan lebih lanjut, Bejo siap bantu, Mr. Don! 🚀
