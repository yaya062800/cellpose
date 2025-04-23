import os
import csv
import process_image_cellpose
import test_image_normalize
from PIL import Image
import numpy as np
import shutil
import cv2
from cellpose import io

def process_images(base_dir):
    data_rows = []
    
    # Walk through the directory structure
    for root, dirs, files in os.walk(base_dir):
        parts = root.split(os.path.sep)
        if len(parts) == 4:
            site = parts[-3]
            mouse = parts[-2]
            file_id = parts[-1]


                    #image = cv2.imread(image_path, cv2.IMREAD_GRAYSCALE)
                    image = io.imread(image_path)

                    #img_intensity_adjusted = test_image_normalize.adjust_intensity(image, 1.5)
                    
                    count, img_out, diams, diams_status = process_image_cellpose.process_cell(img_in=image, img_output=image_path, model_type_input='nuclei', chan=[0,0], gpu=True)

                    # Convert the output array to an image
                    img_out_pil = Image.fromarray(np.uint8(img_out))  # Convert array to PIL Image
                    
                    # Save the output image
                    output_image_path = os.path.join(output_dir, f"{filename.rsplit('.', 1)[0]}_out.png")
                    img_out_pil.save(output_image_path)
                    # Append data to the file_id_rows
                    file_id_rows.append([
                        site, mouse, file_id, filename, region_name, count, diams_status, diams
                    ])

            # Sort the rows for the current file_id by the numeric part of the file number
            file_id_rows.sort(key=lambda x: int(x[3].split('_')[0]))  # sort by the first number before the underscore
            data_rows.extend(file_id_rows)

    # Write the sorted data to a CSV file
    csv_file = os.path.join(base_dir, 'output_Cellpose.csv')
    with open(csv_file, 'w', newline='') as file:
        writer = csv.writer(file)
        writer.writerow(['Site', 'Mouse', 'File_ID', 'File', 'Region', 'Count', 'Diameter_Probe_Status', 'Diameter_Detected'])
        writer.writerows(data_rows)

base_dir = 'C:/Users/lin -workspace/riken/cellpose_counting_projection/Image_data_for_Proc'
process_images(base_dir)
