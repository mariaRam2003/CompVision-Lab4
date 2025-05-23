# 🧠 Laboratorio #4 – U-Net + Filtro Anisotrópico de Perona-Malik  
### Visión por Computadora  
**Universidad del Valle de Guatemala**  
📅 31 de mayo 2025  

---

## 👥 Integrantes

- María Marta Ramírez Gil – 21342  
- Gustavo Andrés González Pineda – 21438  
- José Pablo Orellana Orellana – 21970  
- Diego Alberto Leiva – 21752  

---

## 🧩 Descripción del Proyecto

Este laboratorio implementa una red neuronal tipo **U-Net** combinada con el **filtro anisotrópico de Perona-Malik**, aplicada a imágenes en escala de grises. El objetivo es filtrar ruido preservando bordes importantes, utilizando la base de datos **BSDS500**.

Se incluye preprocesamiento, entrenamiento del modelo y evaluación visual.  

---

## 🧠 Filtro Anisotrópico de Perona-Malik

El filtro de Perona-Malik es una técnica de **difusión adaptativa** que suaviza regiones homogéneas en una imagen mientras **preserva bordes definidos**. A diferencia de filtros como el Gaussian Blur, no difumina bordes sino que los respeta gracias a su formulación no lineal.

### Beneficios
- Preservación de bordes
- Reducción eficiente de ruido
- Ideal para imágenes médicas, satelitales y visualización científica

### Referencias clave
- Perona, P., & Malik, J. (1990). _Scale-space and edge detection using anisotropic diffusion_. [IEEE](https://scispace.com/papers/scale-space-and-edge-detection-using-anisotropic-diffusion-2my7clurjr)  
- Guidotti, P. (2025). _Anisotropic Diffusions of Image Processing From Perona-Malik_. [PDF](https://www.math.uci.edu/~gpatrick/source/papers/G131.pdf)  
- BSDS500 Dataset – [GitHub](https://github.com/BIDS/BSDS500)  
- Kaggle Notebooks – [Kaggle](https://www.kaggle.com/code/kmader/anisotropic-diffusion-example)

---

## ⚙️ Requerimientos

Incluidos en el archivo `requirements.txt`:

- numpy  
- tensorflow  
- opencv-python  
- matplotlib  
- scikit-learn  
- tqdm  

**Se recomienda fuertemente usar una GPU** para acelerar el entrenamiento.  

---

## 🚀 Instrucciones de uso

### 1. Crear un ambiente de conda
```bash
conda create -n lab4_unet python=3.10
conda activate lab4_unet
```

### 2. Instalar los requerimientos
```bash
pip install -r requirements.txt
```

### 3. Clonar el repositorio
```bash
git clone <url-del-repo>
cd lab4_unet
```

### 4. Ejecutar el código principal
```bash
python main.py
```

---

## 🧪 Resultados Esperados

- Visualización de máscaras predichas vs ground truth  
- Comparación de imágenes antes y después del filtrado  
- Gráficas de desempeño del modelo  

---

## 📌 Notas Finales

- Proyecto 100% reproducible  
- Código limpio y modular  
- Compatible con Google Colab

# 🛠️ Recomendaciones para Mejorar la Calidad de Imágenes Filtradas

Durante las pruebas del modelo **U-Net** entrenado con ventanas deslizantes, se observaron salidas pixeladas o con artefactos tipo mosaico. A continuación se listan recomendaciones clave para mejorar la calidad del resultado final:

## ✅ Recomendaciones

| Paso | Recomendación |
|------|---------------|
| ✅ 1 | **Aumentar el tamaño de ventana (`window_size`)**: Se sugiere usar `64` o `128` para que el modelo capture estructuras más amplias. |
| ✅ 2 | **Reducir el `stride` en la inferencia**: Usar `stride=8` o `4` suaviza la reconstrucción al solapar más las ventanas. |
| ✅ 3 | **Ampliar la capacidad del modelo U-Net**: Agregar más filtros por capa (por ejemplo: `64 → 128 → 256 → 512`) mejora la capacidad de aprendizaje. |
| ✅ 4 | **Revisar la activación de salida**: Sustituir `sigmoid` por `linear` o `tanh` y normalizar las imágenes en `[0, 1]` o `[-1, 1]` según corresponda. |
| ✅ 5 | **Utilizar funciones de pérdida perceptuales**: Probar `SSIM`, `MAE` o una pérdida mixta como: |

```python
def mixed_loss(y_true, y_pred):
    return 0.8 * tf.reduce_mean(tf.square(y_true - y_pred)) + \
           0.2 * tf.image.ssim(y_true, y_pred, max_val=1.0)
```
| ✅ 6 | **Reducir el nivel de suavizado del filtro anisotrópico**: Usar valores más suaves como `niter=20`, `gamma=0.1` evita que se pierdan detalles esenciales en las imágenes objetivo. |