# python_csv_combine
Python script to combine csv files in to one csv file

This script will combine csv files in several folders into one csv file in Final (created folder if not exists)

Folders structure for combining...

Main Folder
  | --------- Data Folder
                 | --------- sub folder
                              | ---------- csv files
                              
Put python file into data folder and run.
This will conbine csv files in sub folders and create one csv file inside "Final" folder and rename conbined csv files with processed extention

------------------------------------------------------------------------------------------------------------------------------------------------------
# csv combine file

from operator import truediv
import pandas as pd
import pathlib
import os

data_dir = '.'
out_dir = 'Final'   # output directory name is Final. 

original_file_name_starts_with ='Name' # original file name starts with this

if os.path.exists(pathlib.Path(out_dir)):
    print("output directory exist.")
else:
    os.mkdir('./' + out_dir)

if os.path.exists(pathlib.Path(out_dir) / 'combined_csv.csv'):
    os.remove(pathlib.Path(out_dir) / 'combined_csv.csv')
    print("The existing file has been deleted successfully. Latest file will be created later.")
else:
    print("The file does not exist! New file will be created later.")

try:

    list_files = []
    for filename in pathlib.Path(data_dir).glob('**/*.csv'):
        list_files.append(filename)
    
    df = pd.concat(map(pd.read_csv, list_files), ignore_index=True)
    df.drop_duplicates()
    out_dir2 = './' + out_dir
    df.to_csv(pathlib.Path(out_dir2) / 'combined_csv.csv', index=False)
    print("combined csv file created sucessfully inside " + out_dir + ".")
    del df

    # rename processed files
    for filename in pathlib.Path(data_dir).glob('**/'+ original_file_name_starts_with +'*.csv'):
        print(str(filename) + " will be renamed.")
        os.rename(str(filename), str(filename)+ '.processed')

except:
    print("No csv file to combine. No file will be created.")

