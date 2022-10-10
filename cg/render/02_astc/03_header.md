# ASTC：头部格式

## 1. 大小

* 头部：16字节
* a,b 最小取值 4*4，最大取值8*8
* 数据：w*h个像素，rgba，用a*b像素压缩成一个块，最后压缩成：
	+ 每一块：128位 = 16字节
	+ 宽度块数：w_block_num = floor( (w + a - 1) / a )
	+ 高度块数：h_block_num = floor( (h + b - 1) / b )
	+ 总的字节数 = 16 * w_block_num * h_block_num

例子：一张 268 * 179 像素的32位png，以8*8像素压缩一个块，那么：

+ 宽度块数：w_block_num = floor( (268 + 8 - 1) / 8 ) = 34
+ 高度块数：h_block_num = floor( (179 + 8 - 1) / 8 ) = 23
+ 数据长度：16 * 34 * 23 = 12512 字节
+ astc文件长度 = 16 + 12512 = 12528 字节

## 2. 头部：16字节

``` c

struct astc_header
{
	uint8_t magic[4];			// 固定内容：19 171 161 92
	uint8_t block_x;			// 每块多少像素
	uint8_t block_y;			// 每块多少像素
	uint8_t block_z;			// 每块多少像素
	uint8_t dim_x[3];			// 原图像的分辨率，dims = dim[0] + (dim[1] << 8) + (dim[2] << 16)
	uint8_t dim_y[3];			// 原图像的分辨率
	uint8_t dim_z[3];			// 原图像的分辨率
};

unsigned int xblocks = (dim_x + block_x - 1) / block_x;
unsigned int yblocks = (dim_y + block_y - 1) / block_y;
unsigned int zblocks = (dim_z + block_z - 1) / block_z;

size_t data_size = xblocks * yblocks * zblocks * 16;
uint8_t *buffer = new uint8_t[data_size];

```