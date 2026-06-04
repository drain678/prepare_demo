super (). save (*args, **kwargs)
if self photo: 
    try:
        img = Image.open(self.photo.path)
        img = img.resize((300, 200), Image.Resampling.LANCZOS)
        img.save(self.photo.path)
except Exception:
    pass
