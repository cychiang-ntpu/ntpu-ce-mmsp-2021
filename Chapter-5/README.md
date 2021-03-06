# 5. Digital Image Representation

###### tags: `Multimedia Signal Processing (U3329, Fall, 2020)`
###### 通訊系 江振宇 副教授


課程目標：
1. 了解 bitmap 的定義、來源、大小、解析度、顏色。
2. 影像的 spatial domain 以及 frequency domain
3. Two-dimensional discrete cosine transform (2D-DCT)
4. Color


---

## 5.1 數位影像的來源

### 5.1.1. Bitmapping
bitmap images 是由定義每一個像素 (pixel, picture element) 的顏色產生的數位資料，又稱為 pixmaps 或 raster graphics。可由以下方法產生：
1. digital cameras: 手機、專業數位相機、Webcam
線上其他資源:
> https://blog.xuite.net/g5223086/twblog1/168080390-%E6%95%B8%E4%BD%8D%E7%9B%B8%E6%A9%9F%E7%9A%84%E6%94%9D%E5%BD%B1%E5%8E%9F%E7%90%86
https://kknews.cc/photography/e45j9my.html
https://kknews.cc/zh-tw/digital/39nxley.html
https://en.wikipedia.org/wiki/Digital_camera
https://kknews.cc/photography/e4rq6oy.html
    * Analog Camera Photography
        * Film that is covered with silver-laden (鹵化銀?) is exposed to light
        * There are three layers on photographic film, one sensitive to red, one to green and one to blue light (assuming the use of RGB color)
        * At each point across a continuous plane, all three color components are sensed simultaneously
        * The degree to which the silver atoms gather together measures the amount of light to which the film is exposed
    * Analog vs. Digital Photography
        * Analogy photography measures the incident light continuously across the focal plane while digital photography samples it only at discrete points.
        * Another difference is that it is more difficult for digital camera to sense all three color components i.e., red, green and blue at each sample point.
        * The constraints on sampling color and the use of interpolation to “fill the blanks” in digital sampling can lead to color aliasing.
    * [Digital single-lens reflex camera](https://en.wikipedia.org/wiki/Digital_single-lens_reflex_camera)
        * As a counterpart of [Twin-lens reflex camera](https://en.wikipedia.org/wiki/Twin-lens_reflex_camera)
        ![](https://i.imgur.com/BRGfAKx.png)
    * Charge-Coupled Device (CCD)
        * Many current digital cameras use charge-coupled device (CCD) technology to sense light and thereby color
    * CMOS-Complementary Metal-Oxide Semiconductor is an alternative technology for digital photography
    * CCD
        * Consists of two-dimensional array of photosites
        * Each photosite correspond to one sample (one pixel in the digital image)
        * The number of photosites determines the limits of camera’s resolution
    * To sense red, green or blue at a discrete point, the sensor at that photosite is covered with red, green or blue color filter
    * But the question is:
        * Should all three color components be sensed simultaneously at each photosite?
        * Should they be sensed at different moments when the picture is taken?
        * Or should only one color component per photosite be sensed?
    * There are various CCD designs in current technology each with its own advantages and disadvantages
    * Possible solutions:
        * The incident light can be divided into three beams
            * Three sensors are used at each photosite, each covered with a filter that allows only red, green or blue to be sensed.
            * Is an expensive solution and creates a bulkier camera
        * The sensor is can be rotated when the picture is taken so that it takes in information about red, green and blue light in succession
            * The three colors are not sensed at precisely the same moment, so the subject being photographed needs to be still
        * A more recently developed technology ([Foveon X3](https://zh.wikipedia.org/wiki/Foveon_X3%E6%84%9F%E5%85%89%E5%85%83%E4%BB%B6)) uses silicon for the sensors in a method called vertical stacking.
            * Different depths of silicon absorb different wavelengths of light, all three color components can be detected at one photosite.
        * A less expensive method of color detection uses a color array to detect only one color component at each photosite
            * Interpolation is then used to derive the other two color components based on information from neighbouring sites
            * It is the interpolation that can lead to color aliasing
            * [Bayer Color Filter Array](https://zh.wikipedia.org/wiki/%E6%8B%9C%E7%88%BE%E6%BF%BE%E8%89%B2%E9%8F%A1)
                * In the 4 x 4 array shown in the figure, the letter in each block indicates which color is to be detected at each site.
                * The pattern shown here is called a Bayer filter array, or simply a Bayer filter. (It is also possible to use cyan-magenta-yellow combination)
                * There are twice as many green sensors as blue or red, human eye is more sensitive to green
                * The array shown here is just a small portion of what would be on a CCD

        
---

#### Stop to Think! Why RGB?
* Color Perception and Representation
    * Physically, color is composed of electromagnetic waves
    * For humans, the wavelength of visible colors fall between approximately 370 and 780 nanometers (nm), i.e., 1nm = $`10^{-9}`$ meters
    * These waves fall upon the color receptors of the eyes, and in a way not completely understood, the human brain translates the interaction between the waves and the eyes as color perception
    * It is possible to create pure color composed of a single wavelength, e.g., by means of a laser
    * But most colors we see around us are almost always produced by a combination of wavelengths
    * Green cover of a book may look pure green to you, but a spectrograph will break it up into its components wavelengths, producing a spectral density function $`P(\lambda)`$
    * A spectral density function shows the contribution of the wavelengths λ to a given perceived color as λ varies across the visible spectrum
    * Spectral density functions are one mathematical way to represent colors, but not very convenient way for computers
    * ***One problem is that, more than one spectral density curve can represent two colors that look the same***
    * It is possible to represent  a color by means of a simpler spectral density graph (Hue Saturation Value –HSV and Hue Lightness Saturation - HLS color modes uses this color representation)
    * Each color in the spectrum can be characterised by a unique graph that has a simple shape
    * The graph for each color gives the color’s dominant wavelength, equivalent to the hue; its saturation (i.e., color purity) and its luminance.
        * The dominant wavelength is the wavelength at the spike in the graph
        * The area beneath the curve indicates the luminance L
        * Saturation S is the ratio of the area of the spike to the total area.
        ![](https://i.imgur.com/RMiUdru.png)
    * The dimensions of hue, saturation and brightness do not correspond very well to the computer monitors
    * ***Cathode Ray Tube (CRT) monitors – display colored light through a combination of red, green and blue phosphors, which light up at varying intensities when excited by electron bean***
    * Liquid Crystal Display (LCD) panels – display color with neighboring pixels of red, green and blue that are either lit up or masked by the liquid crystals 
    * So, what is the best way to model color for the computer?
    * There is no simple answer since different models have advantages in different situations.
* RGB Color Mode
    * Varying combinations of three primary colors can create a wide range of colors
    * Three colors are primary with respect to one another if no one of them can be created as a combination of the other two
    * Red, Gree and Blue are good choices as primary colors because the cones of the eyes (color receptors) are especially sensitive to these hues
    * C = rR + gG + bB
        * where r, g and b indicate the relative amounts of red, green and blue energy respectively. Also referred to as the values of the RGB color components  or (color channels in application programs)
        * R, G and B are constant values base on the wavelengths chosen for the red, green and blue components
    * The color space for the RGB color model is easy to depict graphically
    * Let R, G and B correspond to three axes in 3D space
    * The relative amount of red, green and blue are normalized to vary between 0 and 1
    * The origin (0,0,0) of the RGB cube correspond to black and white is the value (1,1,1). 
    * The remaining corners correspond to red, green and blue and their complementary colors i.e., cyan (青藍色), magenta (品紅色) and yellow respectively
    ![](https://i.imgur.com/egGzuqk.png)
    * For example (1, 0.65, 0.15) is light orange etc
    * Shades of gray have equal proportions of red, green and blue and lie along the line between (0,0,0) and (1,1,1).
        * All grayscale values have equal amount of R, G and B
        * Standard equation is used to convert RGB to grayscale
        * Let an RGB color pixel be given by (R, G, B), where R, G and B are red, green and blue color components respectively. The corresponding gray value is given by (L, L, L), where L = 0.30R + 0.59G + 0.11B
        * Since all three color components are equal in gray pixel, only one of the three values needs to be stored. 
        * Thus a 24-bit RGB pixel can be stored as an 8-bit grayscale pixel
* CMY Color Model ...
* HSV and HLS Color Models ... 
* Luminance and Chrominance Color Models ...YIQ,YCbCr,YUV...

---

2. scanners:
將實體物建照光，然後使用如 digital camera 的方法取樣及量化 pixel value。 
    ![](https://i.imgur.com/wbdshk2.jpg)
3. paint programs: 小畫家、CorelDRAW、Adobe Fresco、Adobe Photoshop、平板觸控筆(如Apple pencil)+筆記程式、[GNU Paint](https://www.gnu.org/software/gpaint/)、[GIMP](https://www.gimp.org/)

### 5.1.2. Vector graphics
使用數學式來描述圖形的參數，比如以圓心位置以及半徑來描述圓、以多項式描述曲線、以開始點以及結束點表示一條直線等等。可以使用以下方法產生：
1. CorelDRAW
2. 小畫家
![](https://i.imgur.com/jPFemgy.png)
3. MS Office: Word, Power point...
![](https://i.imgur.com/4ZtzhNz.png)
4. Adobe Illustrator

### 5.1.3. Procedural modeling
A number of techniques in computer graphics to create 3D models and textures from sets of rules.工具軟體有：Houdini、Blender、Grome等等。

https://youtu.be/Ahd70h9ZN2o
https://www.youtube.com/watch?v=gmYvA05orbs

---

## 5.2. Bitmaps
### 5.2.1 Definition
* A bitmap is two-dimensional array of pixels describing a digital image
* Each pixel $f(r,c)$, short for picture element, is a number representing the color at position (r,c) in the bitmap, where “r” is the row and “c” is the column

### 5.2.2 Bitmap Digitization 
*針對數位檔案的表示和儲存而言*

* Digital camera uses the same digitization process discussed in Chapter 2 (sampling and quantization)

* Sampling rate for digital camera (**Pixel Dimension**)
    * How many points of color are sampled and recorded in each dimension of the image
    * For example: 1600 x 1200, 1280 x 960, 1024 x 768 and 640 x 480. Some cameras do offer no choice
![](https://i.imgur.com/6D0Ksyr.png)

* Quantization in digital camera (**Bit Depth**)
    * Is a matter of the color model used and the corresponding bit depth (color models will be dealt in more details later in this chapter)
    * For now the digital cameras generally use RGB color, which saves each pixel in three bytes one for each of the color channels: red(R), green(G) and blue(B)
    * A higher bit depth is possible in RGB, but three bytes per pixel is common. 3 bytes = 24bits, therefore, $2^{24}$ = 16,777,216 colors to be represented

* Both sampling and qantization can introduce error
    * proper sampling rate (usaully related to **dimension**)
    ![](https://i.imgur.com/pbz8BLh.png)
source: https://www.cloudynights.com/uploads/monthly_02_2019/post-234657-0-50706300-1549928065.jpg
    * undersampled
    ![](https://i.imgur.com/sENjuML.png)
source: https://www.cloudynights.com/uploads/monthly_02_2019/post-234657-0-67539100-1549928245.jpg

    * Quantization (usually related to **bit depth**)
    ![](https://i.imgur.com/oz7yznN.png)
    ![](https://i.imgur.com/CY3Kz9M.png)
source: https://www.cambridge.org/core/books/handbook-of-medical-image-perception-and-techniques/image-quality-and-its-clinical-relevance/FA12ADE4513608F65A5F12CA816A9247
![](https://i.imgur.com/bRbRzgq.jpg)
source: https://kornelix.net/fotoxx_topics/color_depth.html

### 5.2.3 Pixel Dimension, Resolution and Image Size 
*針對顯示介面，如螢幕 (monitor/computer display) 以及印刷品 (print) 而言*
* Pixel Dimension
    * a pixel: a physical object a point of light on the screen
    * logical pixel vs. physical pixel: 在撰寫有關圖像處理或是 GUI 相關的程式會遇到，當你把 bitmap 檔案在一個螢幕上顯示，就要弄清楚。
    * The logical pixel i.e., the number representing a color and stored for a given position in the image file is mapped to a physical pixel on the computer screen
    * pixel dimension: a number of pixels horizontally (i.e., width, “w”) and vertically (i.e., height, “h”) denoted $w \times h$. The standards are HD, full HD, 4K, 8K...
* Resolution: the number of pixels in an image file per unit of spatial measure, i.e., pixel per inch, ppi
    * The same number of pixels are used in the horizontal and vertical directions
    * Resolution of a printer is a matter of how many dots of color it can print over an area, i.e., dots per inch (DPI)
    * The printer and its software map the pixels in an images file to the dots of color printed
    * There maybe more or fewer pixels per inch than dots printed
* Image Size
    * On Print
        * Is a physical dimensions of an image when it is printed our or displayed on a computer, e.g., in inches or centimeters
        * By this definition image size is a function of the pixel dimension and resolution, as follows:
            * For an image with resolution “r” and pixel dimensions “w x h” where “w” is the width and “h” is the height, the printed image size “a x b” is given by:
            $$a=w/r$$
            $$b=h/r$$
            * Example: If you have an image that is 1600 x 1200, and you choose to print it out at 200ppi, it will be 8” x 6”
    * On Computer Screen
        * For an image with pixel dimensions “w x h” and resolution “r”,  the displayed image size is, as before: $(w/r) \times (h/r)$
            * In this case “r” is the display screen’s resolution. Example: If your computer display screen has pixel dimensions of 1400 x 1050 and it is 12” x 9”, then the display has a resolution of about 117ppi
            * Therefore, an image with pixel dimension “w x h” where “w” is the width and “h” is the height displayed on a computer display with resolution “r” at 100% magnification, the displayed image size “a x b” is given by:
            $$a=w/r$$
            $$b=h/r$$
    * Terms Related to Image Size
        * Image Cropping is simply cutting off part of the picture, discarding the unwanted pixels
        * Resampling is changing the number of pixels in an image
            * Upsampling: increasing the pixel dimensions
            * Downsampling: decreasing the pixel dimensions
            * Resampling always involves some kind of interpolation, averaging or estimation and thus it cannot improve the quality of an image



---

## 5.3 讀寫 Bitmap Image File (\*.bmp)

以下是一個非常不正是但是可以執行的程式，讀了一張 input.bmp 檔，然後把圖像的左下角 300 x 400 的 pixel 強制變成黑色，然後再輸出成 output.bmp

[rwbmp.c](rwbmp.c)

```
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
float PI = 3.14159265359;

/*construct a structure of BMP header*/
typedef struct Bmpheader {
	char identifier[2]; // 0x0000
	unsigned int filesize; // 0x0002
	unsigned short reserved; // 0x0006
	unsigned short reserved2;
	unsigned int bitmap_dataoffset; // 0x000A
	unsigned int bitmap_headersize; // 0x000E
	unsigned int width; // 0x0012
	unsigned int height; // 0x0016
	unsigned short planes; // 0x001A
	unsigned short bits_perpixel; // 0x001C
	unsigned int compression; // 0x001E
	unsigned int bitmap_datasize; // 0x0022
	unsigned int hresolution; // 0x0026
	unsigned int vresolution; // 0x002A
	unsigned int usedcolors; // 0x002E
	unsigned int importantcolors; // 0x0032
	unsigned int palette; // 0x0036
} Bitmap;

/*construct a structure of RGB*/
typedef struct RGB{
	unsigned char R;
	unsigned char G;
	unsigned char B;
} ImgRGB;


void readheader(FILE* fp, Bitmap *x);
ImgRGB** malloc_2D(int row, int col);
void InputData(FILE* fp, ImgRGB **array, int H, int W, int skip);

void output_bmp(ImgRGB **RGB, FILE* outfile, Bitmap bmpheader, int skip);


int main(int argc, char **argv)
{
	int i, j, x, y;
	FILE *fp_in;
	FILE *fp_out;
	
	

	char fn_in[] = "input.bmp";

	char fn_out[] = "output.bmp";

	fp_in = fopen(fn_in, "rb");
	if (fp_in) printf("Open %s as input file!\n", fn_in);
	else printf("Fail to open %s as input file!\n", fn_in);
	fp_out = fopen(fn_out, "wb");
	if (fp_out) printf("Open %s as output file!\n", fn_out);
	else printf("Fail to open %s as output file!\n", fn_out);

	//read header
	Bitmap bmpheader;
	readheader(fp_in, &bmpheader);
	//struct Bmpheader bmpheader

	// W:3024 x H:4032 for input.bmp
	int H = bmpheader.height;
	int W = bmpheader.width;

	// skip paddings at the end of each image line
	int skip = (4 - (bmpheader.width * 3) % 4);
	if (skip == 4) skip = 0;
	char skip_buf[3] = { 0, 0, 0 };

	// 8x8, if not multiples of 8, then bitmap padded, i.e. one more block
	int block_H = H / 8;
	if (H % 8 != 0) block_H++;
	int block_W = W / 8;
	if (W % 8 != 0) block_W++;

	printf("Image size: Width: %d x Height=%d pixels, total %d bytes\n", W, H, W*H * 3);
	printf("Image line skip=%d bytes\n", skip);
	printf("# of 8x8 blocks: W=%d x H=%d blocks, total %d blocks\n", block_W, block_H, block_W*block_H);

	printf("\nLoading BMP data: RRGGBB RRGGBB ... (BGR format in BMP file)\n");
	ImgRGB **Data_RGB = malloc_2D(bmpheader.height, bmpheader.width);
	InputData(fp_in, Data_RGB, bmpheader.height, bmpheader.width, skip);
	fclose(fp_in);

	printf("Data size loaded: %d bytes\n", H*W * 3);

	
	output_bmp(Data_RGB, fp_out, bmpheader, skip);
	printf("\n"); // DEBUG

	printf("Output decompressed image to: %s\n\n", fn_out);

	printf("Free memory and close files!\n");
	free(Data_RGB[0]);
	free(Data_RGB);

	
	fclose(fp_out);

	printf("Job done!\n");
	return 0;
}

/*read header*/
void readheader(FILE* fp, Bitmap *x) {
	fread(&x->identifier, sizeof(x->identifier), 1, fp);
	fread(&x->filesize, sizeof(x->filesize), 1, fp);
	fread(&x->reserved, sizeof(x->reserved), 1, fp);
	fread(&x->reserved2, sizeof(x->reserved2), 1, fp);
	fread(&x->bitmap_dataoffset, sizeof(x->bitmap_dataoffset), 1, fp);
	fread(&x->bitmap_headersize, sizeof(x->bitmap_headersize), 1, fp);
	fread(&x->width, sizeof(x->width), 1, fp);
	fread(&x->height, sizeof(x->height), 1, fp);
	fread(&x->planes, sizeof(x->planes), 1, fp);
	fread(&x->bits_perpixel, sizeof(x->bits_perpixel), 1, fp);
	fread(&x->compression, sizeof(x->compression), 1, fp);
	fread(&x->bitmap_datasize, sizeof(x->bitmap_datasize), 1, fp);
	fread(&x->hresolution, sizeof(x->hresolution), 1, fp);
	fread(&x->vresolution, sizeof(x->vresolution), 1, fp);
	fread(&x->usedcolors, sizeof(x->usedcolors), 1, fp);
	fread(&x->importantcolors, sizeof(x->importantcolors), 1, fp);

}

/* A function of making two dimensions memory locate array*/
ImgRGB** malloc_2D(int row, int col){
	ImgRGB **Array, *Data;
	int i;
	Array = (ImgRGB**)malloc(row*sizeof(ImgRGB *));
	Data = (ImgRGB*)malloc(row*col*sizeof(ImgRGB));
	for (i = 0; i<row; i++, Data += col){
		Array[i] = Data;
	}
	return Array;
}

/*input data without header into RGB structure*/
void InputData(FILE* fp, ImgRGB **array, int H, int W, int skip){
	int temp;
	char skip_buf[3];
	int i, j;
	for (i = 0; i<H; i++){
		for (j = 0; j<W; j++){
			temp = fgetc(fp);
			array[i][j].B = temp;
			temp = fgetc(fp);
			array[i][j].G = temp;
			temp = fgetc(fp);
			array[i][j].R = temp;
			if (i<9 && j<8) printf("%02x%02x%02x ", array[i][j].R, array[i][j].G, array[i][j].B); // DEBUG
		}
		if (skip != 0) fread(skip_buf, skip, 1, fp);
		if (i<9) printf("\n"); // DEBUG
	}
	printf("\n"); // DEBUG
}


/*output header and data*/
void output_bmp(ImgRGB **RGB, FILE* outfile, Bitmap bmpheader, int skip){
	char skip_buf[3] = { 0, 0, 0 };
	int x, y;
	fwrite(&bmpheader.identifier, sizeof(short), 1, outfile);
	fwrite(&bmpheader.filesize, sizeof(int), 1, outfile);
	fwrite(&bmpheader.reserved, sizeof(short), 1, outfile);
	fwrite(&bmpheader.reserved2, sizeof(short), 1, outfile);
	fwrite(&bmpheader.bitmap_dataoffset, sizeof(int), 1, outfile);
	fwrite(&bmpheader.bitmap_headersize, sizeof(int), 1, outfile);
	fwrite(&bmpheader.width, sizeof(int), 1, outfile);
	fwrite(&bmpheader.height, sizeof(int), 1, outfile);
	fwrite(&bmpheader.planes, sizeof(short), 1, outfile);
	fwrite(&bmpheader.bits_perpixel, sizeof(short), 1, outfile);
	fwrite(&bmpheader.compression, sizeof(int), 1, outfile);
	fwrite(&bmpheader.bitmap_datasize, sizeof(int), 1, outfile);
	fwrite(&bmpheader.hresolution, sizeof(int), 1, outfile);
	fwrite(&bmpheader.vresolution, sizeof(int), 1, outfile);
	fwrite(&bmpheader.usedcolors, sizeof(int), 1, outfile);
	fwrite(&bmpheader.importantcolors, sizeof(int), 1, outfile);

	for (x = 0; x<bmpheader.height; x++){
		for (y = 0; y<bmpheader.width; y++){
			if (y < 100 && x<100) { // let pixels at x<100 and y<100 be zeros
				RGB[x][y].B = 0;
				RGB[x][y].G = 0;
				RGB[x][y].R = 0;
			}
			fwrite(&RGB[x][y].B, sizeof(char), 1, outfile);
			fwrite(&RGB[x][y].G, sizeof(char), 1, outfile);
			fwrite(&RGB[x][y].R, sizeof(char), 1, outfile);
			if (x<9 && y<8) printf("%02x%02x%02x ", RGB[x][y].R, RGB[x][y].G, RGB[x][y].B); // DEBUG
		}
		if (skip != 0) { fwrite(skip_buf, skip, 1, outfile); }
		if (x<9) printf("\n"); // DEBUG
	}
}

```

[input](input.bmp)




[output](output.bmp)


---

## 5.4 Frequency in Digital Images
請看 Discrete Cosine Transform
https://hackmd.io/@cychiang-ntpu/ryTaeHjYv

---

## 5.5 Two-Dimensional Discrete Cosine Transform (2D-DCT)

---

## 5.6 Color Model in Deep