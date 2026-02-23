# Task-7
Image Resizer tool
import os
from PIL import Image

# Folder names
INPUT_FOLDER = "input_images"
OUTPUT_FOLDER = "output_images"

# Resize size
MAX_WIDTH = 800
MAX_HEIGHT = 600


def resize_images():

    # Create input folder if not exists
    if not os.path.exists(INPUT_FOLDER):
        os.makedirs(INPUT_FOLDER)
        print("input_images folder created.")
        print("Please add images inside input_images folder and run again.")
        return

    # Create output folder if not exists
    if not os.path.exists(OUTPUT_FOLDER):
        os.makedirs(OUTPUT_FOLDER)

    # Check if input folder is empty
    if len(os.listdir(INPUT_FOLDER)) == 0:
        print("No images found in input_images folder.")
        print("Please add some images and run again.")
        return

    # Process images
    for filename in os.listdir(INPUT_FOLDER):

        if filename.lower().endswith((".jpg", ".jpeg", ".png")):
            try:
                input_path = os.path.join(INPUT_FOLDER, filename)

                with Image.open(input_path) as img:

                    # Convert transparent images
                    if img.mode in ("RGBA", "P"):
                        img = img.convert("RGB")

                    # Resize (keep aspect ratio)
                    img.thumbnail((MAX_WIDTH, MAX_HEIGHT))

                    # Save as JPG
                    new_name = os.path.splitext(filename)[0] + ".jpg"
                    output_path = os.path.join(OUTPUT_FOLDER, new_name)

                    img.save(output_path, "JPEG", quality=90)

                    print("Processed:", filename)

            except Exception as e:
                print("Error processing", filename, ":", e)

    print("\nAll images processed successfully!")


if __name__ == "__main__":
    resize_images()
