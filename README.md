# make-CelebA-HQ
Python script to download and create the celebA-HQ dataset.

To get the celebA-HQ dataset, you need to 

 a) download the celebA dataset. [Large-scale CelebFaces Attributes (CelebA) Dataset](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)
 
 b) download celebA-HQ dataset. [Google Drive](https://drive.google.com/drive/folders/0B4qLcYyJmiz0TXY1NG02bzZVRGs)
 
 c) do some processing to get the HQ images `make_HQ_images.py`.


# Usage
1.Clone the repository
```
git clone https://github.com/mazzzystar/make-CelebA-HQ.git
cd make-CelebA-HQ
```

2.Install necessary packages
 * Install miniconda https://conda.io/miniconda.html
 * Create a new environement
 ```
 conda create -n celebaHQ python=3
 source activate celebaHQ
 ```
 * Install the packages
 ```
 conda install jpeg tqdm requests pillow urllib3 numpy cryptography scipy
 pip install opencv-python==3.4.0.12 cryptography==2.1.4
 ```
 * Install 7zip (On Ubuntu)
 ```
 sudo apt-get install p7zip-full
 ```

3.Download and Unzip CelebA & CelebA-HQ

Downloading CelebA: [Large-scale CelebFaces Attributes (CelebA) Dataset](http://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)

Downloading CelebA-HQ. Google Drive: https://drive.google.com/drive/folders/0B4qLcYyJmiz0TXY1NG02bzZVRGs
```bash
# there are 14 file inside. (img_celeba.7z.001, ... img_celeba.7z.014)
cd CelebA/Img/img_celeba.7z  
# merge .7z files into a single file.
cat img_celeba.7z.0** > img_celeba.7z
# unzip the .7z file
7z x img_celeba.7z
# move the unzipped folder 'img_celeba' under 'Img/' path
mv img_celeba ../


cd CelebA-HQ/
# unzip the zips of deltas00000.zip, deltas00001.zip, .. into one file
unzip '*.zip' -d combined
# move the "image_list.txt" to current github repo folder.
```

4.Make sure your datasets/landmark/scripts under current Github repo folder.
```
make-CelebA-HQ---CelebA
               |    |___Anno
               |    |     |___list_landmarks_celeba.txt
               |    |     |___...
               |    |     
               |    |___Img
               |         |___ img_celeba/
               |
               |_CelebA-HQ
               |    |
               |    |_Combined/
               |  
               |-make_HQ_images.py
               |-image_list.txt  
               |_README.md 
  
```

5.Run the scripts
```
python make_HQ_images.py ./

```
where `./` is the directory where you wish the data to be saved. this script will take a few hours to run depending on your internet connection and your CPU power. The final HQ images will be saved as `.npy` files in the `./celebA-HQ/combined/` folder.

6.Sample of loading image
```python
import numpy as np
from PIL import Image

img = np.load('imgHQ15858.npy')
img = img.reshape(3, 1024, 1024)
print(img.shape)
img = np.transpose(img, (1, 2, 0))
print(img.shape)

im = Image.fromarray(img)
im.show()
```
![](png/0.png)
# Sources
This code is inspired by these files
* https://github.com/nperraud/download-celebA-HQ

# Note
The code above use `jpeg=8d` version of `md5` for checking hash value, however I could get the expected value for `jpeg=9b` version, so I simply ignore the md5 checking. Tell me if you think somewhere wrong.
