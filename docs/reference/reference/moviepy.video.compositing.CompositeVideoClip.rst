from moviepy.video.VideoClip import ColorClip

# Crear una capa de partículas (ejemplo simple: puntos blancos que se mueven)
particle = ColorClip(size=(10, 10), color=(255, 255, 255), duration=30).set_opacity(0.3)
particle = particle.set_position(lambda t: (np.sin(t) * 100 + 960, np.cos(t) * 100 + 540))

particles = CompositeVideoClip([animated_clip, particle])
particles.write_videofile("solo_leveling_particles.mp4", fps=30)

# Crear un destello de luz en una zona específica (donde están los ojos)
glow = ColorClip(size=(100, 30), color=(128, 0, 255), duration=30).set_opacity(0.6)
glow = glow.set_position((960, 500))  # ajusta coordenadas según imagen

final = CompositeVideoClip([animated_clip, glow])
final.write_videofile("solo_leveling_glow.mp4", fps=30)

fog = ColorClip(size=(3840, 2160), color=(200, 200, 255), duration=30).set_opacity(0.05)
fog = fog.set_position(lambda t: (int(t * 30) % 3840 - 3840, 0))  # movimiento lateral

with_fog = CompositeVideoClip([animated_clip, fog])
with_fog.write_videofile("solo_leveling_fog.mp4", fps=30)
