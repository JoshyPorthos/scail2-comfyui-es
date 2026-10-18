# SCAIL-2 en ComfyUI — cambia a una persona de un video por tu personaje (flujos en español)

> 🦖 **Este flujo es gratis.** Si te sirvió, suscríbete a **El Dino Porthos** en YouTube para más tutoriales: **https://www.youtube.com/@eldinoporthos**

Flujos del tutorial de **SCAIL-2** de **El Dino Porthos** (YouTube: @eldinoporthos). 📺 El video: *(se añade al publicarlo)*.
Cambia a una persona, o a un animal, de un video por cualquier personaje: misma cámara, misma luz y mismo sonido, gratis
en tu propio computador.

| Archivo | Qué es |
|---|---|
| **`scail2_reemplazo_receta_porthos_es.json`** | El recomendado: con los tres trucos (4 ensayos, atención INT8 y la foto entera), en español |
| `scail2_reemplazo_plantilla_es.json` | La plantilla oficial de ComfyUI («Character Replacement», SCAIL-2 int8), solo traducida (para comparar) |
| `ejemplo/` | El ejemplo listo para probar: la toma (un gato atigrado) y la foto del casting (un gato de esmoquin) |

## Cómo usarlo
1. Haz clic en **`scail2_reemplazo_receta_porthos_es.json`** (arriba) y pulsa **«Download raw file»** (la flechita ⬇ arriba a la derecha).
2. Descarga también los dos archivos de la carpeta **`ejemplo/`** y ponlos en `ComfyUI/input/`.
3. Abre **ComfyUI** y **arrastra el flujo** desde tu carpeta de Descargas encima del lienzo. Al soltarlo, el flujo aparece.
4. Descarga estos siete modelos y ponlos en `ComfyUI/models/` (no hace falta cuenta):

| Carpeta | Archivo | Peso | Dónde |
|---|---|---|---|
| `diffusion_models` | wan2.1_14B_SCAIL_2_int8_convrot.safetensors | 16,7 GB | huggingface.co/Comfy-Org/SCAIL-2 |
| `loras` | wan2.1_SCAIL_2_DPO_lora_bf16.safetensors | 1,23 GB | huggingface.co/Comfy-Org/SCAIL-2 |
| `loras` | lightx2v_I2V_14B_480p_cfg_step_distill_rank64_bf16.safetensors | 0,74 GB | huggingface.co/Kijai/WanVideo_comfy |
| `text_encoders` | umt5_xxl_fp8_e4m3fn_scaled.safetensors | 6,74 GB | huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged |
| `clip_vision` | clip_vision_h.safetensors | 1,26 GB | huggingface.co/Comfy-Org/Wan_2.1_ComfyUI_repackaged |
| `vae` | Wan2_1_VAE_bf16.safetensors | 0,25 GB | huggingface.co/Kijai/WanVideo_comfy |
| `checkpoints` | sam3.1_multiplex_fp16.safetensors | 1,75 GB | huggingface.co/Comfy-Org/sam3.1 |

5. Dale a **Ejecutar**. Viene listo con el ejemplo del gato (896 × 512, semilla 7): unos 4 minutos la primera vez después
   de abrir ComfyUI (carga los modelos); las siguientes, unos 3 y medio.

## Con tu propio video
- **Cargar la toma**: tu video, con una persona o un animal bien visible.
- **Foto del casting**: tu personaje, de cuerpo entero; si la toma es un primer plano, una foto de cabeza y pecho.
- **Libreto** (en inglés): describe la escena **ya filmada**, con el personaje nuevo, sin dar órdenes: el lugar, la luz
  y la cámara; el personaje y dónde está; qué hace; qué tiene en las manos; la ropa de pies a cabeza. Di el plano de la
  toma («extreme close-up of…»), nunca el de la foto. No escribas «cambia a la mujer por…»: escribe «un dinosaurio
  baila en una calle…».
- **A quién sigue en la toma** y **Qué hay en la foto**: `person` / `human`; para un animal, su nombre en inglés (`cat`, `dog`).
- **Ancho × alto**: 896 × 512 para probar; 1280 × 704 para la buena (más nítida, unas 3 veces más lenta).
- **Semilla**: el «número de la suerte». Cámbialo para otra versión; déjalo igual para repetir la misma.

## Qué cambia frente a la plantilla oficial
1. **4 ensayos en vez de 6** en las dos escenas: el atajo (LoRA lightx2v) está entrenado para 4 pasos. Misma imagen,
   menos tiempo.
2. **Redondear las cuentas**: nodo «Backend de atención del modelo» → `comfy kitchen attention` (atención INT8; existe en
   tarjetas NVIDIA y AMD). Unas 1,8 veces más rápido.
3. **La foto entera**: el director recorta la foto del casting por el centro para darle la forma del video; con una foto
   vertical y un video horizontal se pierden la cabeza y los pies. La cajita «Foto entera, con la forma del video» la
   encaja completa sobre blanco.
4. El libreto, a quién sigue, qué hay en la foto y el tamaño se escriben **una sola vez** para las dos escenas.

## Consejos
- **Borde blanco alrededor del personaje**: pasa con fotos sobre fondo blanco. Pon la foto sobre un fondo parecido al de
  la toma y desaparece.
- **Escenas**: cada una son 81 cuadros (con un video de 24 cuadros por segundo, unos 3,4 s). El relevo continúa desde los
  5 últimos cuadros de la anterior y le iguala el color, así la unión no se nota. Hasta 157 cuadros (unos 6,5 s) caben
  en las dos escenas que trae.
- **¿Video más largo?** Copia el relevo y pégalo **con sus conexiones** (Ctrl+Mayús+V); ponle número de escena 3, conecta
  «cuadros de la escena anterior» a la salida del relevo 2 y su salida al hueco libre de «Unir las escenas».
- **Dos personas en la toma**: cámbialas de una en una.

Medido en: AMD Radeon RX 9070 XT 16 GB · Ryzen 9 9950X3D · 48 GB de RAM · Windows 11 (ComfyUI con ROCm). Con NVIDIA los
pasos son los mismos; estas cifras son de la AMD.
Cada 5 segundos de video (24 cuadros por segundo), en 896 × 512: **unos 8 min como viene → unos 3 min y medio con la
receta** (solo con los 4 ensayos, unos 6 min). En 1280 × 704, unas 3 veces más.

El ejemplo (los dos gatos) también está hecho con inteligencia artificial, como todo el video.
Modelos: SCAIL-2 (sobre Wan 2.1), SAM 3.1 de Meta y la LoRA lightx2v; revisa la licencia de cada uno en su página.

Más tutoriales con flujos en español: **[LTX-2.5 en AMD](https://github.com/JoshyPorthos/ltx25-comfyui-amd-es)** ·
**[Qwen Image Edit 2511](https://github.com/JoshyPorthos/qwen-edit-2511-comfyui-es)** · **https://www.porthosia.com**

¿Te sirvió? Suscríbete a El Dino Porthos en YouTube: https://www.youtube.com/@eldinoporthos
