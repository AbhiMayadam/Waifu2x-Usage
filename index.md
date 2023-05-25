<head>
	<style>
	p, li{
			font-size: 20px;
		 }
	</style>
</head>

# How do I get and use Waifu2x?

There are 2 different versions of Waifu2x
1. [Waifu2x-Ncnn-Vulkan](https://github.com/AbhiMayadam/Waifu2x-Usage/blob/main/waifu2x-ncnn-vulkan-GUI_2.1.0.1.zip) [Note: f11894 took down their repository so this is the latest copy I have. I will update this link if something changes]
2. [Waifu2x-Caffe](https://github.com/lltcggie/waifu2x-caffe/releases)
3. [Waifu2x-Colab](https://colab.research.google.com/drive/1RjyCk30cc24ez1-a1Qa3CP3g_yk9AJwq)

Waifu2x-Ncnn-Vulkan will run on basically any hardware from 2014 and later, Waifu2x-Caffe will run only on Nvidia GPUs (and CPU, but that's slow as balls, so don't even think about it) and Waifu2x-Colab is an implementation of Waifu2x-Ncnn Vulkan that runs on Google's servers and can read files directly from your Google Drive. This only needs a web browser to run.

## How to check what GPU do you have?
1. Right click on the taskbar and launch Task Manager, Click on more details if your Task Manager looks like this.
![TM](https://i.imgur.com/uqQgAoP.png)
2. Go to the performance tab and scroll the left pane all the way down and you will see GPU entries.
![TM](https://i.imgur.com/3ejEpOM.png)
3. Select GPU 0 or GPU 1 if your system has 2 GPUs to get the full names of both.
![TM](https://i.imgur.com/hrrGuSN.png)
4. If the GPU brand is AMD or Intel, you need to use Waifu2x-ncnn-vulkan. Older intel iGPUs don't support Vulkan. If the model code is 4 digits, such as HD Graphics 5500, it will not support Vulkan on Windows. 3 Digit model codes such as HD 620 and later will support Vulkan. Xe graphics supports Vulkan.  Intel Skylake iGPUs such as HD 520 technically support Vulkan, but do not in practice.
4a. If you have a Nvidia GPU, look at the Dedicated GPU Memory Entry. If it says 2 GB or less like here, use Waifu2x-Ncnn-Vulkan, if it is greater than 2 GB, use Waifu2x-caffe.

![TM](https://i.imgur.com/Wbdm50B.png)


## Using Waifu2x
1. Unzip Waifu2x-ncnn-vulkan-gui or Waifu2x-Caffe into the folder,
2. Click on waifu2x-ncnn-vulkan-gui.exe or waifu2x-caffe.exe depending on the version you downloaded.

	<figure>
	<img align="middle" src="https://i.imgur.com/LkZIusX.png" alt="Waifu2x NCNN Vulkan Explorer Screenshot" style="width:100%" />
	<figcaption align = "center"><b>Waifu2x NCNN-Vulkan GUI</b></figcaption>
	</figure>

	<figure>
	<img align="middle" src="https://i.imgur.com/DAKSskp.png" alt="Waifu2x NCNN Caffe Explorer Screenshot" style="width:90%" 	align="middle" />
	<figcaption align = "center"><b>Waifu2x Caffe</b></figcaption>
	</figure>

3. Go to Preferences and enable Execute high precision processing (fp32 mode). This will increase processing time, but will decrease artifacts that arise from
![w2x](https://media.discordapp.net/attachments/722854123434803286/943467728273047592/unknown.png)

4. For Waifu2x-Caffe, click on the App Setting button and make sure it is set to CUDA.
![w2x](https://i.imgur.com/vvJQADl.png)

5. From here on, the instructions will be basically the same for either Waifu2x version. Click on Input Directory/Source for Caffe and Vulkan respectively, and navigate to the images you want. Caffe supports folders (but not folders within folders) and Vulkan will support folders if you drag it into the top window.

6. You can keep the output directory as blank to make folders for each individual chapter if you are running multiple folders at once. It will make a folder in the same directory as the original raws. If you make a custom output directory, it will not keep the parent folder.

7. Make sure output is set to PNG and not JPG.

8. Choosing the correct denoise level is dependent on the JPEG quality of the image. You can use an jpg quality checker website such as <https://www.imgonline.com.ua/eng/determine-jpeg-quality.php>. Upload an unstitched raw image (must be jpg originally, do not convert) and get the value.

9. Set it to Denoise and set your denoise level based on this number. Level 0 is 93+, Level 1 is 75-90, Level 2 is 75 and below, and Level 3 is simply strong denoise and works on all images At the point where you would use Level 2, you might as well make the jump to Level 3 denoise. Do not over denoise as an over denoised image will look worse and can lead to loss of detail.

10. Alternatively, you can determine what level is needed if you zoom all the way in on a border of a line. If the colors bordering the line are solid, use Level 0, if there is some artifacts (small splotches that are lighter or darker than the base color), use level 1. Use Level 3 if there is a lot of them (and I mean a lot)

11. Make sure the box for TTA is not checked. It increases processing time by 8x and only yields a very small quality increase.

12. Click Run and wait for it to process.


### Advanced Settings
**What is block/split size and what is the ideal number for block/split size?**

Block size is the size of the chunks that the Waifu2x processes at a time. Bigger Chunks = better results and faster results, but this comes at a cost. Your VRAM is the limiting factor in determining block size as that is where your GPU stores all of the working information. If the chunks are too big, your GPU literally bites off more than it can chew and you will run out of VRAM.

For determining the best block size for your setup, run an image with images that you would normally denoise with various block sizes starting from 100 (increase by 100 each time) and looking at VRAM utilization. Once you get around to 40-60% of your VRAM utilization, I would stop. This gives you some headroom if you get much larger raws. (For example, I got 1440px * 12000px raws when I normally do 690px * 7000px raws. I ran out of VRAM as my block size was set to 300)

A rule of thumb is a block size of 100 for each GB of VRAM you have as a low estimate. Again, it is dependent on your raws' average dimensions, how many threads you have and what model you run (9/10 it will be cunet), but it will get you close. I am not sure on how it scales for Caffe so if you have some numbers, please let me know.

**What is TTA?**

TTA stands for test-time-augmentation and what it does is process the same image in 8 different orientations and averages the result of said images. You get a higher quality image, but at the cost of processing time as it takes 8 times as long to process the same image, so it is not worth it in most cases. Probably useful for art.

**What is FP32?**

FP32 is shorthand for Single Precision Floating Point Format. All computers do math, and we have already established that more information is better than less information. The same is true for math. FP32 stores more digits of a number in memory than FP16, or Half Precision Floating Point format, and this leads to more accurate colors as everything is magnitudes more precise. This comes at a cost at performance as it takes twice as long to process the image, but unlike TTA, there is a very visible improvement.

**What is Color Cast?**

Color cast is a slight tone that shows up in low quality raws being processed by Waifu2x or not enabling FP32 on Waifu2x-ncnn-vulkan (I believe Waifu2x-Caffe defaults to FP32, but don't quote me on that). it comes from the lack of precision in FP16 mode, as mentioned in the above paragraph.



## Troubleshooting

**My Task Manager looks like this and has no GPU entries?**

![fuckywucky](https://i.imgur.com/BwrHd7x.png)

You will need to install your GPU drivers or you have a very old Graphics Card. If it is the latter, use Device Manager, then select Display Adapters to determine what GPU you have. Odds are if it doesn't show up in Task Manager, there is a non zero chance that it doesn't support Vulkan and either waifu2x-caffe (If you have Nvidia) or waifu2x-colab must be used.

**I am running Waifu2x and I do not see any GPU activity?**

For some reason, Windows does not report GPU Compute at all. Go to your GPU entry in Task Manager, click on any of the titles of the graphs (Don't choose 3D processing or Copy) and select Compute 0. Waifu2x activity will show up. You will also see spikes under the Copy Graph.
![fuckywucky](https://i.imgur.com/ZNHQYMf.png)

**I got an error that looked something like this and Waifu2x-ncnn-vulkan stopped processing?**

![fuckywucky](https://i.imgur.com/qTWsZfJ.png)

What happened was that you ran out of VRAM. Keep lowering and trying it again until it runs properly. A good rule of thumb is 100 * VRAM in GB. 800 is the max you should do.

**I got an error like this and Waifu2x-ncnn-vulkan didn't even launch?**

![fuckywucky](https://i.imgur.com/RrjPBjK.png)

Your GPU doesn't support Vulkan. If you have another GPU, switch to that (even if it is an iGPU) and make sure your GPU drivers are up to date. If you have done all you can, you will need to use Waifu2x-Colab for denoising.

## Sources
<https://shorturl.at/iuJRY>

<https://en.wikipedia.org/wiki/Single-precision_floating-point_format>

<https://en.wikipedia.org/wiki/Half-precision_floating-point_format>

<https://docs.google.com/document/d/1L3mt9zZAs8BVpK6BtlVca-gmsbaA02-e0yviBqz3O2c/edit>

## Thanks
Thank you tangerine01 for looking over my site and giving a lot of pointers and corrections.

<footer>
  <p> If you have any questions, comments, recommendations, etc., please make a post in the Discussion tab on the Github repository or you can message me on Discord at notthebees#3150.</p>
</footer>
