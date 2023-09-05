# ConsoleImageRedactor
Graphic filters (image_processor)
This project is a part of learning to code with C++. This is a console app to edit pictures in 24-bit BMP format.

Attention! This .md page takes a while to load the example pictures from ./results folder .

Format of command line arguments
Description of the format of command line arguments:

{program name} {path to input file} {path to output file} [-{filter name 1} [filter parameter 1] [filter parameter 2] ...] [-{filter name 2} [filter parameter 1] [filter parameter 2] ...] ...

When running without arguments, the program outputs help.

Example
This is our example image:

lenna.bmp

Let us apply these filters:

./image_processor input.tmp /tmp/output.bmp -crystallize_advanced 2000 -blur 3

In this example

The image is loaded from the file input.bmp
Advanced crystallize filter with 2000 random points is applied.
A blur with sigma 3 is applied
The resulting image is saved to the file /tmp/output.bmp
Our result here is following:

output

The filter list may be empty, then the image must be saved unchanged. Filters are applied in the order in which they are listed in the command line arguments.

Filters
In the formulas, we further assume that each color component is represented by a real number from 0 to 1. Pixel colors are represented by triples (R, G, B). Thus, (0, 0, 0) – black, (1, 1, 1) – white.

List of basic filters
Matrix filter (no user-usage due to it is inside component of the project)
If the filter is set by a matrix, it means that the value of each of the colors is determined by the weighted sum of the values of this color in neighboring pixels in accordance with the matrix. In this case, the target pixel corresponds to the central element of the matrix.

For example, for a filter given by a matrix

encoding

The value of each of the colors of the target pixel C[x][y] will be determined by the formula

C[x][y] =
  min(1, max(0,
   1*C[x-1][y-1] + 2*C[x][y-1] + 3*C[x+1][y-1] +
   4*C[x-1][y]   + 5*C[x][y]   + 6*C[x+1][y]   +
   7*C[x-1][y+1] + 8*C[x][y+1] + 9*C[x+1][y+1]
))
When processing pixels close to the edge of the image, part of the matrix may extend beyond the image boundary. In this case, we will use the value of the image pixel closest to it as the value of the pixel that goes beyond the border.

Crop (-crop width height)
Crops the image to the specified width and height. The upper left part of the image is used.

If the requested width or height exceeds the dimensions of the original image, the available part of the image is given.

Command: -crop 400 400 Result:

crop result

Grayscale (-gs)
Converts the image to grayscale using the formula

encoding

Command: -gs Result:

greyscale result

Negative (-neg)
Converts an image to a negative using the formula

encoding

Command: -neg Result:

negative result

Sharpening (-sharp)
Sharpening. It is applied by using a matrix

encoding

Command: -sharp Result:

sharpening result

Edge Detection (-edge threshold)
Border selection. The image is converted to grayscale and a matrix is applied

encoding

Pixels with a value exceeding the threshold are colored white, the rest are black.

Command: -edge 15 Result:

edge detection result

Gaussian Blur (-blur sigma)
Gaussian blur, the parameter is sigma.

The value of each of the pixel colors C[x0][y0] is determined by the formula

encoding

Command: -blur 3 Result:

Gaussianl blur result

Crystallize (-crystallize points_amount)
This filter turns picture into set of crystalls with the same color, the parameter is the amount of crystalls.

It works like this: first, an array of random points of a given size is created, after that the value of each pixel turns into the value of the closest pixel from our array. That is how it works.

Command: -crystallu ize 2000 Result:

crystallize result

Crystallize advanced (-crystallize_advanced points_amount)
Advanced version of the previous one, but here the value of pixel turns into a weighted sum of two closest points with the weights proportional to the default distanses between the points.

Command: -crystallize_advanced 2000 Result:

advanced crystallize result

Some filter combinations
Command: -crystallize_advanced 2000 -gs Result:

extra 1

Command: -sharp -edge 15 Result:

extra 2

Command: -sharp -gs Result:

extra 3
