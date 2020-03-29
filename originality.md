
# Assignment 1 
## Task 1 (Affine Transformation)
### Ari Purnama Aji_1313617008_Sistem Pakar 112

#### Berikut merupakan kodingan yang saya ambil dari luar:
Pada bagian kodingan tentang transformasi affine 'projectives' sebagian besar saya mempeloreh dan mempelajari algoritma perhitungannya melalui referensi link berikut ini : https://math.stackexchange.com/questions/494238/how-to-compute-homography-matrix-h-from-corresponding-points-2d-2d-planar-homog

### 4. Projectives

```python
# Menentukan setiap koordinat(x,y) untuk titik awal dan titik tujuan
xA1, yA1, xA2, yA2, xA3, yA3, xA4, yA4 = 300, 300, 700, 300, 300, 700, 700, 700     # titik awal
xB1, yB1, xB2, yB2, xB3, yB3, xB4, yB4 = 200, 400, 850, 250, 370, 840, 860, 500     # titik tujuan

# Membuat matriks yang dibentuk dari setiap pasang koordinat titik awal dan akhir
P = np.array([
    [-xA1, -yA1, -1, 0 ,0 ,0, xA1*xB1, yA1*xB1, xB1],
    [0, 0, 0, -xA1, -yA1, -1, xA1*yB1, yA1*yB1, yB1],
    [-xA2, -yA2, -1, 0, 0, 0, xA2*xB2, yA2*xB2, xB2],
    [0, 0, 0, -xA2, -yA2, -1, xA2*yB2, yA2*yB2, yB2],
    [-xA3, -yA3, -1, 0, 0, 0, xA3*xB3, yA3*xB3, xB3],
    [0, 0, 0, -xA3, -yA3, -1, xA3*yB3, yA3*yB3, yB3],
    [-xA4, -yA4, -1, 0, 0, 0, xA4*xB4, yA4*xB4, xB4],
    [0, 0, 0, -xA4, -yA4, -1, xA4*yB4, yA4*yB4, yB4],
    [0, 0, 0, 0, 0, 0, 0, 0, 1]
])

# Membuat vektor nol untuk persamaan
H = np.array([0,0,0,0,0,0,0,0,1])

# Mendapatkan nilai untuk vektor projectives
pr = np.linalg.inv(P) @ H

# Membuat matriks untuk transformasi projectives
PR = np.reshape(pr, [3,3])

# Membuat gambar canvasProjectives sebagai wadah khusus untuk melakukan operasi proyeksi
canvasProjectives = canvasKosong.copy()
```

==========================================================================================================================================

#### Berikut kodingan yang saya buat sendiri:

```python
import numpy as np
from PIL import Image
import matplotlib.pyplot as plt
```


```python
# Melakukan input gambar 
img = Image.open('picture.png')

# Mendapatkan bentuk matriks dari gambar
G = np.array(img)
plt.imshow(img)
plt.show()
```



```python
G.shape
```

    (400, 400, 3)


```python
# Membuat matriks 3D dengan ukuran 1000x1000 yang setiap element matriksnya bernilai 0
canvasKosong = np.zeros((1000, 1000, 3), dtype='uint8')
plt.imshow(canvasKosong)
plt.show()
```


```python
# Membuat gambar canvasAwal yang terlihat juga bentuk gambar sampelnya
canvasAwal = canvasKosong.copy()
canvasAwal[300:700, 300:700] += np.flip(G, 0) 
plt.imshow(canvasAwal)
plt.show()
```


### 1. Scaling

```python
# Membuat gambar canvasScaling sebagai wadah khusus untuk melakukan operasi scaling 
canvasScaling = canvasKosong.copy()

# Menentukan besaran skala
Sx = 1.4   # skala untuk koordinat x
Sy = 0.7   # skala untuk koordinat y

# Membuat matriks umtuk transformasi scaling
S = np.array([
    [Sx, 0, 0],
    [0, Sy, 0],
    [0, 0, 1]
])

# Pengulangan untuk melakukan operasi transformasi dengan mengalikan setiap vektor di titik P dengan matriks scaling
for y in range(300, 700):
    for x in range(300, 700):
        P = np.array([x, y, 1])
        Q = S @ P
        canvasScaling[int(Q[1]), int(Q[0])] = canvasAwal[y,x] # memindahkan pixel di canvas lama pada koordinat (x,y) 
                                                              # ke canvas baru pada koordinat (x',y')

plt.imshow(np.flip(canvasScaling, 0))
plt.show()
```


### 2. Translation

```python
# Membuat gambar canvasTranslation sebagai wadah khusus untuk melakukan operasi translasi 
canvasTranslation = canvasKosong.copy()

# Menentukan besaran pergeseran
tx = -200
ty = 130

# Membuat matriks untuk transformasi translasi
T = np.array([
    [1, 0, tx],
    [0, 1, ty],
    [0, 0, 1]
])

# Pengulangan untuk melakukan operasi transformasi dengan mengalikan setiap vektor di titik P dengan matriks translasi
for y in range(300, 700):
    for x in range(300, 700):
        P = np.array([x, y, 1])
        Q = T @ P
        canvasTranslation[int(Q[1]), int(Q[0])] = canvasAwal[y,x]  # memindahkan pixel di canvas lama pada koordinat (x,y) 
                                                                   # ke canvas baru pada koordinat (x',y')

plt.imshow(np.flip(canvasTranslation, 0))
plt.show()
```


### 3. Rotation

```python
# Membuat gambar canvasRotation sebagai wadah khusus untuk melakukan operasi rotasi
canvasRotation = canvasKosong.copy()

# Menentukan besaran derajat rotasi
teta = np.pi*18/180   #~18 derajat

# Membuat matriks untuk transformasi rotasi
R = np.array([
    [np.cos(teta), -np.sin(teta), 0],
    [np.sin(teta), np.cos(teta), 0],
    [0, 0, 1]
])

# Pengulangan untuk melakukan operasi transformasi dengan mengalikan setiap vektor di titik P dengan matriks rotasi
for y in range(300, 700):
    for x in range(300, 700):
        P = np.array([x, y, 1])
        Q = R @ P
        canvasRotation[int(Q[1]), int(Q[0])] = canvasAwal[y,x]   # memindahkan pixel di canvas lama pada koordinat (x,y) 
                                                                 # ke canvas baru pada koordinat (x',y')

plt.imshow(np.flip(canvasRotation, 0))
plt.show()
```