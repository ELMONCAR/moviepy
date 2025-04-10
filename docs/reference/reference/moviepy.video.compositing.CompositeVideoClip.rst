from moviepy.editor import *
from PIL import Image
import numpy as np

# --- Cargar imagen base ---
image_path = "A_2D_anime-style_digital_animation_still_frame_fea.png"
image = Image.open(image_path)
image_np = np.array(image)
clip = ImageClip(image_np).set_duration(30)

# --- Zoom progresivo ---
def zoom_effect(clip, zoom_factor=1.05, duration=30):
    return clip.resize(lambda t: 1 + (zoom_factor - 1) * (t / duration))

base_clip = zoom_effect(clip, zoom_factor=1.05, duration=30).set_position("center").resize(height=2160)

# --- Partículas flotantes (una simple, puedes duplicar con random) ---
particle = ColorClip(size=(10, 10), color=(255, 255, 255), duration=30).set_opacity(0.3)
particle = particle.set_position(lambda t: (np.sin(t) * 100 + 960, np.cos(t) * 100 + 540))

# --- Glow en los ojos (ajusta la posición según tu imagen) ---
glow = ColorClip(size=(100, 30), color=(128, 0, 255), duration=30).set_opacity(0.6)
glow = glow.set_position((960, 500))  # Ajusta si hace falta

# --- Niebla animada ---
fog = ColorClip(size=(3840, 2160), color=(200, 200, 255), duration=30).set_opacity(0.05)
fog = fog.set_position(lambda t: (int(t * 30) % 3840 - 3840, 0))

# --- Combinar todo ---
final_clip = CompositeVideoClip([base_clip, fog, particle, glow])
final_clip.write_videofile("solo_leveling_wallpaper_FULL.mp4", fps=30)
