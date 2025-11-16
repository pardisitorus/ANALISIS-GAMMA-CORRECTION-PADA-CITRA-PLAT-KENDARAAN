# Analisis Gamma Correction pada Citra Plat Kendaraan

## Deskripsi Proyek
Proyek ini bertujuan untuk menganalisis efek koreksi gamma pada citra plat kendaraan dengan menggunakan berbagai nilai gamma (0.1, 1.5, dan 3.5). Implementasi dilakukan menggunakan Python dengan library PIL, NumPy, dan Matplotlib untuk memproses dan memvisualisasikan hasil koreksi gamma.

## Langkah-Langkah Implementasi

### 1. Import Library yang Diperlukan
```python
from PIL import Image
import numpy as np
import matplotlib.pyplot as plt
```

### 2. Memuat Citra Asli
```python
img_path = "plate3.jpg"
img_pil = Image.open(img_path).convert("RGB")
img_np = np.array(img_pil)  # uint8, shape (H,W,3)
```

### 3. Implementasi Fungsi Gamma Correction
```python
def gamma_correction(img_uint8, gamma: float):
    x = img_uint8.astype(np.float32) / 255.0
    y = np.power(x, gamma)
    out = np.clip(y * 255.0, 0, 255).astype(np.uint8)
    return out
```

### 4. Penerapan Gamma Correction dengan Nilai Berbeda
```python
gammas = [0.1, 1.5, 3.5]
gamma_results = {}
for g in gammas:
    corrected = gamma_correction(img_np, g)
    gamma_results[g] = corrected
    # Menyimpan citra hasil koreksi
    Image.fromarray(corrected).save(f"plate3_gamma_{str(g).replace('.', '_')}.jpg")
```

### 5. Visualisasi Hasil
```python
plt.figure(figsize=(12, 6))

plt.subplot(1, len(gammas) + 1, 1)
plt.imshow(img_np)
plt.title("Original")
plt.axis('off')

for i, g in enumerate(gammas):
    plt.subplot(1, len(gammas) + 1, i + 2)
    plt.imshow(gamma_results[g])
    plt.title(f"Gamma={g}")
    plt.axis('off')

plt.tight_layout()
plt.show()
```

## Analisis Hasil

### Gamma = 0.1
- **Efek**: Citra menjadi sangat terang (overexposed)
- **Karakteristik**: Nilai piksel dinaikkan secara signifikan
- **Aplikasi**: Berguna untuk meningkatkan visibilitas pada area gelap, namun dapat menyebabkan kehilangan detail pada area terang

### Gamma = 1.5
- **Efek**: Citra menjadi sedikit lebih gelap
- **Karakteristik**: Kontras meningkat dengan penekanan pada area gelap
- **Aplikasi**: Cocok untuk meningkatkan kontras tanpa kehilangan detail yang signifikan

### Gamma = 3.5
- **Efek**: Citra menjadi sangat gelap (underexposed)
- **Karakteristik**: Nilai piksel diturunkan secara drastis
- **Aplikasi**: Berguna untuk menekan noise pada area terang, namun dapat menyembunyikan detail penting

## Kesimpulan

1. **Koreksi gamma** merupakan teknik powerful untuk manipulasi kontras citra dengan mengubah hubungan antara nilai input dan output piksel.

2. **Pemilihan nilai gamma** yang tepat sangat bergantung pada:
   - Karakteristik citra asli
   - Tujuan pengolahan citra
   - Kondisi pencahayaan awal

3. Untuk aplikasi **pengenalan plat kendaraan**:
   - Gamma rendah (0.1) dapat membantu pada kondisi cahaya rendah
   - Gamma sedang (1.0-1.5) biasanya optimal untuk kondisi normal
   - Gamma tinggi (3.5) berguna untuk mengurangi silau atau overexposure

4. Implementasi ini berhasil menunjukkan bagaimana koreksi gamma dapat mempengaruhi visibilitas dan kontras citra plat kendaraan, yang merupakan langkah penting dalam preprocessing untuk sistem OCR (Optical Character Recognition).

Proyek ini dapat dikembangkan lebih lanjut dengan implementasi deteksi otomatis nilai gamma optimal berdasarkan karakteristik histogram citra.
