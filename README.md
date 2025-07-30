# Jpeg-to-JPG-converter
As part of a Machine learning project focused on classifying pigeon images ("Piggions"), a custom image renaming and packaging script was developed using Google Colab.

 <img width="706" height="556" alt="image" src="https://github.com/user-attachments/assets/6eecb452-4c8c-433c-9b24-d2484da0d938" />

from google.colab import files
import os
import zipfile
import shutil

uploaded = files.upload()

image_files = [f for f in uploaded.keys() if f.lower().endswith(('.jpeg', '.jpg'))]
image_files.sort()

renamed_files = []
for idx, original_name in enumerate(image_files, start=1):
    new_name = f"Piggion_{idx:03}.jpg"
    if original_name != new_name:
        if original_name.lower().endswith('.jpeg'):
            os.rename(original_name, new_name)
        else:
            if os.path.abspath(original_name) != os.path.abspath(new_name):
                shutil.copy(original_name, new_name)
    renamed_files.append(new_name)

zip_filename = "piggion_images.zip"
with zipfile.ZipFile(zip_filename, 'w') as zipf:
    for f in renamed_files:
        zipf.write(f)

files.download(zip_filename)
